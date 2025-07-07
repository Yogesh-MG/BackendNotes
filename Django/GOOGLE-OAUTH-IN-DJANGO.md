
## Overview

The Google OAuth integration allows users to authenticate via Google, setting HttpOnly cookies (`access_token`, `refresh_token`) for secure session management. The flow:
1. User clicks "Sign in with Google" on the login page.
2. Google returns an `id_token` via `@react-oauth/google`.
3. Frontend sends `id_token` to `/api/auth/google/`.
4. Backend verifies `id_token`, matches email to a user, sets cookies, and returns success.
5. Frontend fetches user profile (`/api/users/me/`) and redirects to a role-based dashboard.

This note focuses exclusively on Google OAuth setup, omitting email/password login or unrelated features.

---

## Backend Setup (Django)

### 1. Install Dependencies

Install packages for Google OAuth and JWT authentication.

```bash

$pip install django djangorestframework rest_framework_simplejwt 
$pip install django-allauth python-decouple google-auth

````

Update requirements.txt:

```text
django==5.2
djangorestframework==3.15.2
rest_framework_simplejwt==5.3.1
django-allauth==0.63.6
python-decouple==3.8
google-auth==2.35.0
```

2. Configure Django Settings

Update settings.py to enable django-allauth, rest_framework_simplejwt, and CORS for Google OAuth.

python

```python
# backend/settings.py
from pathlib import Path
from decouple import config
from datetime import timedelta

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = config('DJANGO_SECRET_KEY')
DEBUG = config('DEBUG', default=True, cast=bool)
ALLOWED_HOSTS = ['127.0.0.1', 'localhost', '192.168.1.2']

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.google',
    'rest_framework',
    'corsheaders',
    'rest_framework_simplejwt',
]

SITE_ID = 1

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'allauth.account.middleware.AccountMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}

CORS_ALLOWED_ORIGINS = [
    'http://localhost:5173',  # Vite frontend
]

CORS_ALLOW_CREDENTIALS = True
CORS_ALLOW_METHODS = ['GET', 'POST', 'OPTIONS']
CORS_ALLOW_HEADERS = ['content-type', 'x-csrftoken', 'authorization']

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(hours=1),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
    'USER_ID_FIELD': 'id',
}

AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
]

SOCIALACCOUNT_PROVIDERS = {
    'google': {
        'APP': {
            'client_id': config('GOOGLE_CLIENT_ID'),
            'secret': config('GOOGLE_CLIENT_SECRET'),
            'key': ''
        },
        'SCOPE': ['profile', 'email'],
        'AUTH_PARAMS': {'access_type': 'online'},
    }
}

CSRF_TRUSTED_ORIGINS = ['http://localhost:5173']
CSRF_COOKIE_SECURE = False  # True in production
CSRF_COOKIE_HTTPONLY = False
SESSION_COOKIE_SECURE = False  # True in production

SOCIALACCOUNT_LOGIN_ON_GET = True
ACCOUNT_EMAIL_VERIFICATION = 'none'
```

Create .env in the backend root:

```text
DJANGO_SECRET_KEY=your-secret-key
DEBUG=True
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
```

3. Set Up Google OAuth Credentials

4. Go to [Google Cloud Console](https://console.cloud.google.com/).
    
5. Create a project (e.g., BenryAutomation-PMS).
    
6. Enable OAuth 2.0 API (APIs & Services > Library).
    
7. Configure OAuth consent screen:
    
    - Select External user type.
        
    - Add scopes: email, profile.
        
    - Set app name (e.g., BenryAutomation PMS), support email, developer contact.
        
8. Create OAuth credentials:
    
    - Go to APIs & Services > Credentials > Create Credentials > OAuth 2.0 Client IDs.
        
    - Set Application type to Web application.
        
    - Add Authorized JavaScript origins: http://localhost:5173.
        
    - Add Authorized redirect URIs: http://localhost:8000/accounts/google/login/callback/.
        
    - Copy Client ID and Client Secret to .env.
        
9. Save and verify client_id matches settings.py.
    

10. Implement GoogleLoginView

Create GoogleLoginView to verify Google id_token and set JWT cookies.

python

```python
# core/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework_simplejwt.tokens import RefreshToken
from django.contrib.auth import get_user_model
from google.oauth2 import id_token
from google.auth.transport import requests
from decouple import config

User = get_user_model()

class GoogleLoginView(APIView):
    def post(self, request):
        id_token_str = request.data.get('id_token')
        if not id_token_str:
            return Response({'detail': 'No Google id_token provided'}, status=status.HTTP_400_BAD_REQUEST)
        
        try:
            client_id = config('GOOGLE_CLIENT_ID')
            id_info = id_token.verify_oauth2_token(id_token_str, requests.Request(), client_id)
            google_user_id = id_info['sub']
            email = id_info['email']
            name = id_info.get('name', '')
            print(f"Google User ID: {google_user_id}, Email: {email}, Name: {name}")
            try:
                user = User.objects.get(email=email)
                if not user.is_active:
                    return Response(
                        {'detail': 'Account is inactive. Contact admin.'},
                        status=status.HTTP_401_UNAUTHORIZED
                    )
            except User.DoesNotExist:
                return Response(
                    {'detail': 'No account found with this email. Contact admin.'},
                    status=status.HTTP_401_UNAUTHORIZED
                )
            
            refresh = RefreshToken.for_user(user)
            access_token = str(refresh.access_token)
            refresh_token = str(refresh)
            response = Response({'detail': 'Google login successful'})
            response.set_cookie(
                'access_token',
                access_token,
                max_age=3600,
                httponly=True,
                secure=False,  # True in production
                samesite='Strict',
            )
            response.set_cookie(
                'refresh_token',
                refresh_token,
                max_age=7 * 24 * 3600,
                httponly=True,
                secure=False,  # True in production
                samesite='Strict',
            )
            return response
        except ValueError as e:
            return Response({'detail': f'Google login failed: Invalid id_token: {str(e)}'}, status=status.HTTP_400_BAD_REQUEST)
        except Exception as e:
            return Response({'detail': f'Google login failed: {str(e)}'}, status=status.HTTP_400_BAD_REQUEST)
```

5. Update URLs

Add GoogleLoginView to urls.py.

python

```python
# urls.py
from django.urls import path, include
from core.views import GoogleLoginView

urlpatterns = [
    path('accounts/', include('allauth.urls')),
    path('auth/google/', GoogleLoginView.as_view(), name='google_login'),
]
```

---

Frontend Setup (React)

1. Install Dependencies

Install packages for Google OAuth and API calls.

bash

```bash
npm install @react-oauth/google axios
```

Update package.json:

json

```json
{
  "dependencies": {
    "@react-oauth/google": "^0.12.1",
    "axios": "^1.7.2"
  }
}
```

2. Configure Environment Variables

Create .env in the frontend root:

```text
VITE_API_URL=http://192.168.1.2:8000/api/
VITE_GOOGLE_CLIENT_ID=your-google-client-id
```

3. Set Up API Config

Configure api_config.tsx to handle Google OAuth API calls and CSRF.

typescript

```typescript
// src/utils/api_config.tsx
import axios, { AxiosError, AxiosResponse, InternalAxiosRequestConfig } from 'axios';

const BASE_URL = import.meta.env.VITE_API_URL || 'http://192.168.1.2:8000/api/';

const apiClient = axios.create({
  baseURL: BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
  withCredentials: true,
});

const getCsrfToken = (): string | undefined => {
  const name = 'csrftoken';
  const value = `; ${document.cookie}`;
  const parts = value.split(`; ${name}=`);
  if (parts.length === 2) return parts.pop()?.split(';').shift();
  return undefined;
};

apiClient.interceptors.request.use(
  (config: InternalAxiosRequestConfig): InternalAxiosRequestConfig => {
    if (['post', 'put', 'patch', 'delete'].includes(config.method?.toLowerCase() || '')) {
      const csrfToken = getCsrfToken();
      if (csrfToken) {
        config.headers.set('X-CSRFToken', csrfToken);
      }
    }
    const accessToken = getCookie('access_token');
    if (accessToken) {
      config.headers.set('Authorization', `Bearer ${accessToken}`);
    }
    return config;
  },
  (error) => Promise.reject(error)
);

apiClient.interceptors.response.use(
  (response: AxiosResponse) => response,
  async (error: AxiosError) => {
    const originalRequest = error.config as InternalAxiosRequestConfig & { _retry?: boolean };
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      try {
        const refreshToken = getCookie('refresh_token');
        if (!refreshToken) throw new Error('No refresh token');
        const { data } = await axios.post(`${BASE_URL}token/refresh/`, { refresh: refreshToken });
        setCookie('access_token', data.access, 1);
        originalRequest.headers.set('Authorization', `Bearer ${data.access}`);
        return apiClient(originalRequest);
      } catch (refreshError) {
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }
    return Promise.reject(error);
  }
);

export const setCookie = (name: string, value: string, hours: number) => {
  const expires = new Date(Date.now() + hours * 3600 * 1000).toUTCString();
  document.cookie = `${name}=${value}; expires=${expires}; path=/; Secure; SameSite=Strict`;
};

export const getCookie = (name: string): string | null => {
  const value = `; ${document.cookie}`;
  const parts = value.split(`; ${name}=`);
  if (parts.length === 2) return parts.pop()?.split(';').shift() || null;
  return null;
};

export const googleLogin = async (id_token: string) => {
  const response = await apiClient.post('/auth/google/', { id_token });
  return response.data; // { detail: 'Google login successful' }
};

export const getUserProfile = async () => {
  const response = await apiClient.get('/users/me/');
  return response.data; // { id, email, designation, first_name, last_name }
};

export default apiClient;
```

4. Update Login Component

Add Google OAuth to Login.tsx with a "Sign in with Google" button.

typescript

```typescript
// src/components/Login.tsx
import { useState, useEffect } from 'react';
import { useNavigate } from 'react-router-dom';
import { GoogleOAuthProvider, GoogleLogin } from '@react-oauth/google';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import MainLayout from '@/components/layout/MainLayout';
import { googleLogin, getUserProfile } from '@/utils/api_config';
import { User, GoogleLoginResponse } from '@/types';

const Login = () => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const navigate = useNavigate();

  useEffect(() => {
    const fetchCsrfToken = async () => {
      try {
        await fetch(`${import.meta.env.VITE_API_URL || 'http://192.168.1.2:8000/api/'}token/`, {
          method: 'OPTIONS',
          credentials: 'include',
        });
      } catch (err) {
        console.error('Failed to fetch CSRF token:', err);
      }
    };
    fetchCsrfToken();
  }, []);

  const handleGoogleLogin = async (credentialResponse: { credential?: string }) => {
    setIsLoading(true);
    setError(null);
    try {
      if (!credentialResponse.credential) throw new Error('No Google credential');
      console.log('Google id_token:', credentialResponse.credential);
      const data = await googleLogin(credentialResponse.credential);
      if (data.detail === 'Google login successful') {
        const user = await getUserProfile();
        navigate(getDashboardPath(user.designation));
      } else {
        throw new Error('Unexpected Google login response');
      }
    } catch (err) {
      setError('Google login failed. Please try again.');
      console.error('Google login error:', err);
    } finally {
      setIsLoading(false);
    }
  };

  const getDashboardPath = (designation: string) => {
    switch (designation) {
      case 'worker':
        return '/dashboard/tasks';
      case 'supervisor':
      case 'incharge':
        return '/dashboard/team';
      case 'manager':
        return '/dashboard/team';
      case 'md':
        return '/dashboard';
      default:
        return '/dashboard';
    }
  };

  return (
    <GoogleOAuthProvider clientId={import.meta.env.VITE_GOOGLE_CLIENT_ID || ''}>
      <MainLayout userRole="worker" showSidebar={false}>
        <div className="flex items-center justify-center min-h-[calc(100vh-200px)] px-4">
          <div className="w-full max-w-md">
            <Card className="border-gray-200 dark:border-gray-700 shadow-lg">
              <CardHeader className="space-y-1 text-center">
                <CardTitle className="text-2xl font-bold">
                  Sign in to{' '}
                  <span className="bg-gradient-to-r from-benry-indigo to-benry-coral bg-clip-text text-transparent">
                    BenryAutomation
                  </span>
                </CardTitle>
              </CardHeader>
              <CardContent>
                <div className="mt-4">
                  <div className="relative">
                    <div className="absolute inset-0 flex items-center">
                      <span className="w-full border-t border-gray-300 dark:border-gray-600" />
                    </div>
                    <div className="relative flex justify-center text-xs uppercase">
                      <span className="bg-white dark:bg-gray-900 px-2 text-gray-500">sign in with</span>
                    </div>
                  </div>
                  <div className="mt-4">
                    <GoogleLogin
                      onSuccess={handleGoogleLogin}
                      onError={() => setError('Google login failed. Please try again.')}
                      text="signin_with"
                      width="100%"
                    />
                  </div>
                </div>
                {error && <p className="text-sm text-red-500 text-center mt-4">{error}</p>}
              </CardContent>
            </Card>
          </div>
        </div>
      </MainLayout>
    </GoogleOAuthProvider>
  );
};

export default Login;
```

5. Update Types

Define TypeScript interfaces for Google OAuth responses.

typescript

```typescript
// src/types/index.ts
export interface GoogleLoginResponse {
  detail: string;
}

export interface User {
  id: number;
  email: string;
  designation: 'worker' | 'supervisor' | 'incharge' | 'manager' | 'md';
  first_name: string;
  last_name: string;
}
```

---

Testing the Integration

Backend Testing

1. Start backend:
    
    bash
    
    ```bash
    python manage.py migrate
    python manage.py createsuperuser --email md@example.com --username md
    python manage.py runserver 192.168.1.2:8000
    ```
    
2. Test /api/auth/google/:
    
    - Obtain a Google id_token (via frontend or Google OAuth Playground).
        
    - Send:
        
        bash
        
        ```bash
        curl -X POST http://192.168.1.2:8000/api/auth/google/ \
          -H 'Content-Type: application/json' \
          -H 'X-CSRFToken: your-csrf-token' \
          -d '{"id_token": "your-id-token"}'
        ```
        
    - Expect: { "detail": "Google login successful" } and cookies (access_token, refresh_token).
        
3. Verify user:
    
    - Create user in Django admin (md@example.com, designation=md, is_active=True).
        
    - Ensure email matches Google account.
        

Frontend Testing

1. Start frontend:
    
    bash
    
    ```bash
    npm run dev
    ```
    
    - Open http://localhost:5173/login.
        
2. Test Google OAuth:
    
    - Click “Sign in with Google”.
        
    - Log in with a Google account matching a database user (e.g., md@example.com).
        
    - Verify redirect to dashboard (e.g., /dashboard for MD).
        
    - Check cookies (access_token, refresh_token, csrftoken).
        
3. Test errors:
    
    - Google account not in database: See “No account found with this email”.
        
    - Invalid id_token: See “Google login failed: Invalid id_token”.
        

---

Notes

- Security: HttpOnly cookies (secure=False for dev, True for production) protect against XSS. CSRF token required for POST requests.
    
- Production:
    
    - Update VITE_API_URL, ALLOWED_HOSTS, CORS_ALLOWED_ORIGINS for production domain.
        
    - Set secure=True for cookies.
        
    - Update Google redirect URI to production backend.
        
- Mobile: Google button is full-width, touch-friendly (px-4).
    

This completes the Google OAuth integration for BenryAutomation PMS.


---
