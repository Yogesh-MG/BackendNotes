
#### 1.For Linux Based System
``First checkout this wesite https://docs.docker.com/desktop/setup/install/linux/

<p>If The requirements matches
<br></p>
1. Set up Docker's apt repository
```shell
$sudo apt-get update
$sudo apt-get install ca-certifivcate curl
$sudo install -m 0755 -d /etc/apt/keyrings
$sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
$sudo chmod a+r /etc/apt/keyrings/docker.asc

$echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
$sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$sudo apt-get update
```

2. Install the latest docker version
```shell
## Now install the latest version of the Docker
$sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
3. Launch the downloaded Docker
```shell
## Now Launch the Docker either using the GUI or in terminal follow
$systemctl --user start docker-desktop
```



#### 2.For Window System
[Click here to download the Docker for windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)
 That's it you downloaded the Desktop version of Docker. 

  