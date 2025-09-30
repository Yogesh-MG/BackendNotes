# 🚀 How to Auto-Start Any Application on Raspberry Pi (Ubuntu) at Boot

When running Ubuntu on Raspberry Pi, you can ensure that applications start automatically after the system powers on and connects to the network. There are a few methods, but the most reliable is using **systemd services**.

---
## 🔹 1. Why systemd?

- Runs applications at boot.
- Can ensure dependencies (like networking, Docker, databases) are ready.
- Monitored by Ubuntu, so you can check logs, restart, and manage easily.

---

## 🔹 2. Steps to Auto-Start Any App

### Step 1: Decide What You Want to Run

Example applications:
- A Python script → `python3 /home/ubuntu/myapp/app.py`
- A binary → `/home/ubuntu/myapp/myprogram`
- A Docker Compose stack → `docker-compose up -d`

---
### Step 2: Create a Systemd Service File

Create a new service file:
```bash
sudo nano /etc/systemd/system/myapp.service
```

Example template:
```bash
[Unit] 
Description=My Custom Application 
After=network-online.target 
Wants=network-online.target  

[Service] 
WorkingDirectory=/home/ubuntu/myapp 
ExecStart=/usr/bin/python3 /home/ubuntu/myapp/app.py 
Restart=always 
User=ubuntu  

[Install] 
WantedBy=multi-user.target```

---

### Step 3: Enable the Service

```bash
sudo systemctl daemon-reload 
sudo systemctl enable myapp.service 
sudo systemctl start myapp.service
```

---

### Step 4: Verify

Check status:

	
```bash 
sudo systemctl status myapp.service
```
Check logs:

```bash
journalctl -u myapp.service -f
```
---

## 🔹 3. Examples

### Example A: Run a Python Script

Service file (`/etc/systemd/system/myscript.service`):


```bash
[Unit] 
Description=Run Python Script at Boot 
After=network-online.target  

[Service] 
ExecStart=/usr/bin/python3 /home/ubuntu/scripts/myscript.py WorkingDirectory=/home/ubuntu/scripts 
Restart=on-failure 
User=ubuntu  

[Install] 
WantedBy=multi-user.target

```


---

### Example B: Run a Docker Compose App

Service file (`/etc/systemd/system/mydocker.service`):

```bash
[Unit] 
Description=Run Docker Compose App 
Requires=docker.service network-online.target 
After=docker.service network-online.target  

[Service] 
WorkingDirectory=/home/ubuntu/project 
ExecStart=/usr/bin/docker-compose up -d 
ExecStop=/usr/bin/docker-compose down 
Restart=always 
User=ubuntu  

[Install] 
WantedBy=multi-user.target
```

---

## 🔹 4. Tips

- Use **`Restart=always`** for long-running apps (scrapers, servers).
    
- Use **`Restart=on-failure`** for scripts that may crash.
    
- For apps needing internet → always include:
    
    `After=network-online.target Wants=network-online.target`
    

---

## 🔹 5. Useful Commands

- Enable service at boot:
    
    `sudo systemctl enable myapp.service`
    
- Disable service:
    
    `sudo systemctl disable myapp.service`
    
- Start manually:
    
    `sudo systemctl start myapp.service`
    
- Stop:
    
    `sudo systemctl stop myapp.service`
    
- Check logs:
    
    `journalctl -u myapp.service -f`
    

---

✅ With this setup, **any app (Python, binary, or Docker)** will start automatically as soon as your Pi powers on and connects to the network.