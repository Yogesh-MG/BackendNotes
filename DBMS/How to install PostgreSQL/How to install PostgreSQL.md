### **1. Install PostgreSQL on Windows**

1. **Download the PostgreSQL Installer:**
    
    - Go to the [official PostgreSQL download page](https://www.postgresql.org/download/windows/).
    - Download the installer for the latest version of PostgreSQL.
2. **Run the Installer:**
    
    - Double-click the downloaded `.exe` file to start the installation process.
    - Follow the installation wizard steps:
        - **Choose Installation Directory:** Select the directory where you want to install PostgreSQL.
        - **Select Components:** Ensure "PostgreSQL Server" and "pgAdmin" (optional GUI tool) are selected.
        - **Set Password:** Provide a password for the `postgres` superuser account.
        - **Choose Port Number:** The default port is 5432. You can keep it as is unless you have a specific requirement.
        - **Select Locale:** Choose the default locale or leave it as default.
    - Finish the installation process.
3. **Verify Installation:**
    
    - Open **pgAdmin** from the Start Menu to access PostgreSQL's graphical interface.
    - Connect to the PostgreSQL server using the `postgres` user and the password you set during installation.

### **1.1. Install PostgreSQL on Linux**

1. **Open the terminal of Linux:**
    
    - Go to the [official PostgreSQL download page](https://www.postgresql.org/download/windows/).(If you feel stucked)
	
```shell
$sudo apt update
$sudo apt install postgresql postgresql-contrib  

#this will install the Postgresql in the Linux

# after installationn 
$sudo systemctl start postgresql

```

### **2. Create a PostgreSQL Database and User**

2. **Open pgAdmin:**
    
    - Launch **pgAdmin** from the Start Menu.
    - Connect to your PostgreSQL server using the credentials you provided during installation.
3. **Create a Database:**
    
    - Right-click on **Databases** in the pgAdmin browser and select **Create > Database**.
    - Enter a name for your database (e.g., `mydatabase`), and click **Save**.
4. **Create a User:**
    
    - Expand the **Login/Group Roles** node under **Server** in pgAdmin.
    - Right-click and select **Create > Login/Group Role**.
    - Enter a name for the user (e.g., `myuser`), and set a password.
    - Under the **Privileges** tab, grant the necessary privileges, such as **Can login**.
    - Click **Save**.
5. **Grant Privileges:**
    
    - Right-click on the database you created and select **Properties**.
    - Go to the **Privileges** tab.
    - Add the new user and grant the required privileges (e.g., all privileges)





<h6>Error note</h6>
<p>So when the user try to open the pgadmin4 from apps they may encounter the error like 
pgadmin cannot be contacted </p>
<p>To solve this just got to</p>
**C:\Users\%User_name%\AppData\Roaming\pgAdmin**  in your file.
<p>in this delete <h7>pgadmin4.db</h7> file which is causing the conflict it helps to solve your problem</p>

Next
