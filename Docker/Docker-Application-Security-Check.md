# Application Security Checklist
*Last Updated: March 9, 2025*  
*Purpose: Ensure a new app deployment is secure across HTTPS, web server, framework, container, network, database, and application layers.*

---

#### 1. HTTPS Security
- [ ] **Verify HTTPS Works**
  - Command: `curl -I https://<domain>`
  - Goal: Returns `HTTP/1.1 200 OK`.
  - Fix: Check SSL config if errors like "wrong version number" appear.
- [ ] **Test SSL Quality**
  - Tool: [SSL Labs](https://www.ssllabs.com/ssltest/)
  - Goal: Grade A or higher (TLS 1.2+, strong ciphers).
  - Fix: Edit SSL config (e.g., `/etc/letsencrypt/options-ssl-apache.conf`) to disable weak protocols.
- [ ] **Check Certificate**
  - Command: `openssl s_client -connect localhost:443 -servername <domain> < /dev/null`
  - Goal: Valid certificate chain, not expired.
  - Fix: Renew with Certbot if invalid (`certbot renew`).

---

#### 2. Web Server Security (Apache/Nginx)
- [ ] **Enabled Modules**
  - Command: `apache2ctl -M` (Apache) or `nginx -V` (Nginx)
  - Goal: Includes `ssl`, `headers`, `rewrite`, `proxy` (if proxying).
  - Fix: `a2enmod <module>` or reconfigure Nginx.
- [ ] **Security Headers**
  - Command: `cat <config-file> | grep -i Header`
  - Goal: Includes `X-Forwarded-Proto`, `X-XSS-Protection`, `Content-Security-Policy`.
  - Fix: Add headers (e.g., `Header set X-XSS-Protection "1; mode=block"`).
- [ ] **Hide Server Version**
  - Command: `curl -I https://<domain>`
  - Goal: `Server` header hides version (e.g., `Server: gunicorn`, not `Apache/2.4.62`).
  - Fix: Set `ServerTokens Prod` and `ServerSignature Off` in Apache config.

---

#### 3. Framework Security (Django Example)
- [ ] **Secure Settings**
  - File: `<project>/settings.py`
  - Goal:
    - `ALLOWED_HOSTS = ['localhost', '127.0.0.1', '<ip>', '<domain>']`
    - `DEBUG = False`
    - `SECURE_SSL_REDIRECT = True`
    - `SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')`
    - `CSRF_COOKIE_SECURE = True`
    - `SESSION_COOKIE_SECURE = True`
    - `SECURE_BROWSER_XSS_FILTER = True`
    - `X_FRAME_OPTIONS = 'DENY'`
    - `SECURE_CONTENT_TYPE_NOSNIFF = True`
  - Fix: Update settings, redeploy.
- [ ] **Deployment Check**
  - Command: `python manage.py check --deploy`
  - Goal: No critical warnings.
  - Fix: Address each warning (e.g., disable `DEBUG`).

---

#### 4. Container Security (Docker)
- [ ] **Running Processes**
  - Command: `docker exec <container_id> ps aux`
  - Goal: Only expected processes (e.g., app server, database, web server).
  - Fix: Investigate and kill unexpected processes.
- [ ] **File Permissions**
  - Command: `docker exec <container_id> ls -ld /app/*`
  - Goal: Sensitive dirs (e.g., static files) are `755`, logs are `755` or `777` if needed.
  - Fix: `chmod 755 <dir>` if too permissive.
- [ ] **Exposed Ports**
  - Command: `docker ps`
  - Goal: Only necessary ports mapped (e.g., `80`, `443`).
  - Fix: Adjust `docker run -p` options.

---

#### 5. Network Security (EC2/Server)
- [ ] **Security Group Rules**
  - Command: `aws ec2 describe-security-groups --region <region>`
  - Goal: 
    - TCP 22 (SSH): Limited to your IP (e.g., `<your-ip>/32`).
    - TCP 80 (HTTP): Open (`0.0.0.0/0`).
    - TCP 443 (HTTPS): Open (`0.0.0.0/0`).
  - Fix: `aws ec2 authorize-security-group-ingress` to tighten rules.
- [ ] **Open Ports**
  - Command: `sudo netstat -tuln | grep -E '22|80|443'`
  - Goal: Only 22, 80, 443 on `0.0.0.0`.
  - Fix: Use `ufw` or security group to close others.
- [ ] **SSH Hardening**
  - File: `/etc/ssh/sshd_config`
  - Goal: `PasswordAuthentication no`
  - Fix: Edit file, restart SSH (`sudo service ssh restart`).

---

#### 6. Database Security (PostgreSQL Example)
- [ ] **Access Control**
  - Command: `docker exec <container_id> psql -U <user> -d <db> -c "SELECT * FROM pg_hba_file_rules;"`
  - Goal: Only `local` or `host` with `md5` for app user.
  - Fix: Edit `pg_hba.conf`, restart PostgreSQL.
- [ ] **Password Strength**
  - Check: Dockerfile or config.
  - Goal: Strong password (e.g., `X7k!p9mQw2`).
  - Fix: Update password, redeploy.

---

#### 7. External Security Scan
- [ ] **Port Scan**
  - Command: `sudo apt install nmap; nmap -p- <server-ip>`
  - Goal: Only 22, 80, 443 open.
  - Fix: Close unexpected ports via security group.
- [ ] **Web Vulnerability Scan**
  - Tool: [OWASP ZAP](https://www.zaproxy.org/)
  - Goal: No high-severity issues.
  - Fix: Address findings (e.g., add `Strict-Transport-Security` header).

---

#### 8. Application-Level Security
- [ ] **CSRF Protection**
  - Test: Submit invalid form data on `<domain>`.
  - Goal: Fails with CSRF error.
  - Fix: Ensure CSRF middleware is enabled.
- [ ] **XSS Protection**
  - Test: Inject `<script>alert('test')</script>` in inputs.
  - Goal: Escaped or blocked.
  - Fix: Enable Django’s escaping or add CSP.
- [ ] **SQL Injection**
  - Test: Input `' OR '1'='1` in fields.
  - Goal: No unauthorized access.
  - Fix: Use ORM/parameters, not raw SQL.

---

#### Deployment Steps to Secure
1. Build Docker image with secure base (`Dockerfile`).
2. Configure web server with SSL and headers (`apache2.conf`).
3. Harden framework settings (`settings.py`).
4. Set DNS (A record for `<subdomain>.<domain>`).
5. Obtain SSL cert (`certbot --apache`).
6. Run container with minimal ports.
7. Verify with tests above.

---

#### Notes
- **HSTS**: Add `Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"` for HTTPS enforcement.
- **Backup**: Save configs after each deployment.
- **Monitoring**: Consider logs or tools like Fail2Ban.