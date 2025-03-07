### **1. IN LINUX terminal

1. **Switch to the postgres user:**
    
```shell
#To enter the postgres in linux
$ sudo -i -u postgres

-- step-2
#To connect to particular database with specified user
postgres@user:~$ psql -h localhost -p 5432 -U user_name database_name

-- step-2.1
postgres@user:~$psql   ## now database activated

-- step-3 to create a new database
postgres=#CREATE DATABASE data-base-name;

-- step-4 to create a user
postgres=#CREATE USER user_name WITH PASSWORD 'password';

-- step-5 to grant privilige
postgres=#GRANT ALL PRIVILEGES ON DATABASE mydatabase TO newuser;

-- Switch to PostgreSQL superuser
sudo -u postgres psql

-- Grant usage and create privileges on the public schema
postgres=#GRANT USAGE, CREATE ON SCHEMA public TO your_user;

-- Grant all privileges on the database
postgres=#GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_user;

-- Allow the user to own future objects in the schema
postgres=#ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO your_user;
postgres=#ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON SEQUENCES TO your_user;

-- (Optional) If permission issues persist, change the database owner
postgres=#ALTER DATABASE your_database_name OWNER TO your_user;

```
