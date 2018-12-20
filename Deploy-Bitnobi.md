
1. [Hardware Requirements](#hardware-requirements)
2. [Software Prerequisites](#software-prerequisites)
3. [Deploy Bitnobi (default)](#deploy-bitnobi-default)
4. [Deploy Bitnobi (CA signed cert)](#deploy-bitnobi-ca-signed-cert)

# Hardware Requirements
* assumes light evaluation use by a few users.
* minimum of 2 CPU cores, 8GB RAM, 80GB disk space. Can be a VM.
* static IP address.
* port 8000 and 8888 must be opened to the network (e.g. via `iptables`). Bitnobi UI uses these ports. If you are running Bitnobi in a VM, you may need to also set up "Port Forwarding" for these two ports.

# Software Prerequisites
* Ubuntu 18.04 preferred as a base Operating System.
* Docker CE. Bitnobi is distributed as a docker image and so docker community edition must be installed on your target server. Docker installation instructions can be found at https://www.docker.com/get-docker .
* make sure no other server processes are using ports 8000 and 8888. If Jupyter Notebook is installed on your server, it will likely use port 8888. Please either halt it or reconfigure it to use some other port.

# Deploy Bitnobi (default)
By default, Bitnobi is bundled with a self-signed certificate to support HTTPS. These will generate browser warnings the first time that a user connects to your Bitnobi site. If you have a CA signed certificate, please see [this section](#deploy-bitnobi-ca-signed-cert) for the extra steps.

Log into the server and open a terminal window for the following steps. 

### 1. Remove Old Bitnobi Deployment
If you have a previously installed Bitnobi container and image you need to remove it prior to installing a new one.
The docker command `docker ps -a` will list which containers are present and `docker images` will list the images loaded on the server.

If you are getting permission errors trying to run docker on linux, please have a look at:
https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user

The following commands will remove the existing bitnobi container and image:
```
docker rm bitnobi
docker rmi bitnobi
```

### 2. Obtain a Bitnobi Docker Image
login to the Bitnobi docker repository using your assigned userid/password and then pull the image to your server.
```
docker login demo.bitnobi.com:5043
docker pull demo.bitnobi.com:5043/bitnobi-aug-2018-pilot
docker tag demo.bitnobi.com:5043/bitnobi-aug-2018-pilot bitnobi
```
The Bitnobi image is > 5GB in size so the pull request can take a few minutes to complete.

### 3. Create Bitnobi Backup Directory
Create a directory for holding Bitnobi backup files. For example:
```
cd ~
mkdir bitnobiBackup
```

### 4. Create and Run a Bitnobi Container
The following command will create a new container from the bitnobi docker image and run it:
```
docker run -it \
-p 8000:8000 -p 8888:8888 \
--name bitnobi \
--cap-add IPC_LOCK \
--mount type=bind,source="$(pwd)"/bitnobiBackup,target=/home/Bitnobi-V1/backup \
--mount type=bind,source=/etc/localtime,target=/etc/localtime,readonly \
bitnobi /bin/bash
```

* The -p options expose port 8000 for the main Bitnobi UI and port 8888 for the Jupyter notebooks server.
* The --name option gives the name "bitnobi" to the new docker container
* the --cap-add IPC_LOCK allows the vault process to lock memory to prevent it from swapping out.
* the --mount localtime option causes the container to use the same timezone as the host
* the --mount bitnobiBackup option causes the backup directory in the container to map to the bitnobiBackup directory in the host. This allows the `bitnobiBackup.sh` command to store backups in the host filesystem rather than inside the container.


Once you have executed the `docker run` command, your terminal will attach to a `bash` shell running in your new container. Any commands that you type now will be executed in the container.

### 5. Start Bitnobi Processes
Now we need to start the Bitnobi processes in the container. Enter the following commands:
```
cd /home/Bitnobi-V1
./startScript.sh
```
this will start several processes and should generate output similar to:
```
┌────────────────┬────┬──────┬─────┬────────┬─────────┬────────┬───────┬───────────┬──────┬──────────┐
│ App name       │ id │ mode │ pid │ status │ restart │ uptime │ cpu   │ mem       │ user │ watching │
├────────────────┼────┼──────┼─────┼────────┼─────────┼────────┼───────┼───────────┼──────┼──────────┤
│ auditServer    │ 1  │ fork │ 47  │ online │ 0       │ 1s     │ 0%    │ 54.6 MB   │ root │ disabled │
│ mongod         │ 0  │ fork │ 23  │ online │ 0       │ 5s     │ 0%    │ 77.7 MB   │ root │ disabled │
│ server         │ 2  │ fork │ 63  │ online │ 0       │ 1s     │ 38.5% │ 37.6 MB   │ root │ disabled │
│ workflowserver │ 3  │ fork │ 80  │ online │ 0       │ 0s     │ 0%    │ 11.6 MB   │ root │ disabled │
└────────────────┴────┴──────┴─────┴────────┴─────────┴────────┴───────┴───────────┴──────┴──────────┘
 Use `pm2 show <id|name>` to get more details about an app
```
Now press the key sequence `ctrl-p ctrl-q` to detach the container and return to the host shell.
You can now run the `docker ps -a` command to verify that the container is still running, and it should return something like:
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS                                            NAMES
fd816960b403        bitnobi             "/bin/bash"         4 minutes ago       Up 4 minutes              0.0.0.0:8000->8000/tcp, 0.0.0.0:8888->8888/tcp   bitnobi
e3c8952f0236        hello-world         "/hello"            18 hours ago        Exited (0) 18 hours ago                                                    infallible_wiles
```

### 6. Open Bitnobi UI
Now open a Chrome browser and navigate to `https://<server name>:8000`. This should open the Bitnobi main page. Please note that because you are using a self-signed certificate the browser will generate warnings the first time that you access the Bitnobi site.

### 7. Create Admin User
The first user to sign up to Bitnobi will become the `admin` user. Please sign up now and define a username and password for the admin user.

# Deploy Bitnobi (CA signed cert)

If you have a signed SSL certificate from a Certificate Authority that you wish to use with Bitnobi, you need take some extra steps during deployment. 

After step 3 (Create Bitnobi Backup Directory) please create a directory and copy over the certificate into it and name the files `key.pem` and `cert.pem`. For example if you used the "letsencrypt" site to create certificates with the subdomain name "demo.bitnobi.com" the steps would be:
```
cd ~
mkdir bitnobiSslCerts
sudo cp /etc/letsencrypt/live/demo.bitnobi.com/privkey1.pem bitnobiSslCerts/key.pem
sudo cp /etc/letsencrypt/live/demo.bitnobi.com/fullchain.pem bitnobiSslCerts/cert.pem
```

For step 4 (Create and Run a Bitnobi Container) substitute the following command which includes a mount point for your certificate:
```
docker run -it \
-p 8000:8000 -p 8888:8888 \
--name bitnobi \
--cap-add IPC_LOCK \
--mount type=bind,source="$(pwd)"/bitnobiBackup,target=/home/Bitnobi-V1/backup \
--mount type=bind,source=/etc/localtime,target=/etc/localtime,readonly \
--mount type=bind,source="$(pwd)"/bitnobiSslCerts,target=/home/Bitnobi-V1/DPprototype/config/sslcerts,readonly \
bitnobi /bin/bash
```
This will over-ride the default self-signed certificate in the Bitnobi image with your signed CA certificate.

For step 5 (Start Bitnobi Processes) call the `vault-init.sh` with your Certificate domain name as a parameter before calling `startScript.sh`. For example if we have the subdomain name "demo.bitnobi.com" the steps would be
```
cd /home/Bitnobi-V1
./vault-init.sh demo.bitnobi.com
./startScript.sh
```

All other steps are unchanged.

When you connect to the Bitnobi main page with your browser, you should no longer see any browser warnings about unsafe HTTPS certificates.



