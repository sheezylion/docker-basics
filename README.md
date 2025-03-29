# Docker-basics
Docker is an open-source containerization platform that allows developers to package applications and their dependencies into lightweight, portable containers. These containers ensure that applications run consistently across different environments, whether on a developer’s local machine, a test server, or a production system.

## With Docker, you can:
- Eliminate “it works on my machine” issues by running applications with the same dependencies across various systems.
- Improve efficiency by using less system resources than traditional virtual machines.
- Speed up deployment with pre-built container images and automated workflows.

## Key concepts include:

**Docker Engine** – The core of Docker that runs containers.

**Docker Images** – Pre-configured templates used to create containers.

**Docker Containers** – The running instances of Docker images.

**Docker Hub** – A public registry for sharing and pulling images.

**Dockerfile** – A script that defines how to build a Docker image.

# Docker Basics: Hands-on Practice with AWS Ubuntu Server
In this session, we will install Docker on an AWS Ubuntu server and practice essential commands to manage Docker containers. Below is a step-by-step guide to setting up and testing Docker.

## Setting Up an AWS Ubuntu Server
### Launch an AWS EC2 instance

**Step 1**: Go to AWS Console and search for EC2, click on it and click on launch instance.

**Step 2**: Give it a name, Choose the appropriate instance type, create a key-pair or select if already created, check security settings and allow ssh, review and launch the instance.

<img width="1103" alt="Screenshot 2025-03-29 at 06 29 49" src="https://github.com/user-attachments/assets/b581f019-d366-498b-9d46-2de1a91ce1a7" />

<img width="1661" alt="Screenshot 2025-03-29 at 06 31 54" src="https://github.com/user-attachments/assets/a74e54b9-23d5-47d6-8c7d-b09a622594a7" />

### Open your terninal and ssh into the EC2 instance
**Step 1**:
```
ssh -i <path to key-pair> ubuntu@<ip-address>
```

<img width="772" alt="Screenshot 2025-03-29 at 06 35 02" src="https://github.com/user-attachments/assets/94b20da8-0ded-4949-a088-683eb23c5b8e" />


**Step 2**: 
Update the system dependencies

```
sudo apt update
```
<img width="1300" alt="Screenshot 2025-03-29 at 06 36 37" src="https://github.com/user-attachments/assets/6aa1ab71-993d-4d72-bb8b-779c92b7acff" />

This ensures all packages and dependencies are updated before installing Docker.

### Installing Docker on Ubuntu
**Step 1**: Install Docker
Run the following command to install Docker:

```
sudo apt install docker.io -y
```
<img width="1009" alt="Screenshot 2025-03-29 at 06 39 16" src="https://github.com/user-attachments/assets/23a1e736-447d-43cf-9d65-582a192e7b9e" />

**Step 2**:
Start the Docker Engine

```
sudo systemctl start docker
```

**Step 3**:
Verify Docker is running

```
sudo systemctl status docker
```

<img width="1579" alt="Screenshot 2025-03-29 at 06 40 57" src="https://github.com/user-attachments/assets/678a5956-3cd4-4aed-8897-60cc73d88337" />

### Granting User Permissions
**Step 1**: 
Add the Ubuntu user to the Docker group. This ensures we can run Docker commands without using sudo:

```
sudo usermod -aG docker ubuntu
```
**Step 2**:
Exit and log back into the server

```
exit
ssh -i <path to key-pair> ubuntu@ip-address
```

<img width="889" alt="Screenshot 2025-03-29 at 06 43 41" src="https://github.com/user-attachments/assets/8f4b2a99-bd5e-4989-9cd6-ef5de721f713" />

**Step 3**
Verify Docker access without sudo

```
docker --version
```
<img width="729" alt="Screenshot 2025-03-29 at 06 44 35" src="https://github.com/user-attachments/assets/e4de25a8-0d1c-4e15-b8ab-227b2341b0e5" />


### Running Basic Docker Commands

**Step 1**:
Test Docker with the Hello World image

```
docker run hello-world
```

<img width="725" alt="Screenshot 2025-03-29 at 06 45 44" src="https://github.com/user-attachments/assets/d9e5ce26-180e-44d9-9e6d-04d6637fa9b7" />

This confirms Docker is installed correctly by running a test container.

**Step 2**:
Pull the latest Nginx image from docker hub

```
docker pull nginx:latest
```
<img width="671" alt="Screenshot 2025-03-29 at 06 47 12" src="https://github.com/user-attachments/assets/935dac30-9672-4b4f-9fef-4a40601d9555" />

This downloads the latest version of Nginx from Docker Hub.

**Step 3**:
List all downloaded images

```
docker images
```
<img width="709" alt="Screenshot 2025-03-29 at 06 48 29" src="https://github.com/user-attachments/assets/fef150f0-c449-4644-8869-4d23de39fdac" />

**Step 4**:
Run an Nginx container

```
docker run -d -p 80:80 --name my-nginx nginx:latest
```

- -d: Runs the container in detached mode (background).

- -p 80:80: Maps port 80 on the server to port 80 in the container.

- --name my-nginx: Assigns the container a custom name (my-nginx).

**Step 5**:
Check if the container is running

```
docker ps
```
<img width="1249" alt="Screenshot 2025-03-29 at 06 50 56" src="https://github.com/user-attachments/assets/59cd1568-f414-43d0-8366-f2d0f89f9fcc" />

**Viewing the Nginx Homepage on Your Browser**
After running the Nginx container, you need to allow external access to port 80 so you can view the homepage in your browser. Follow these steps:

1. Go to your AWS EC2 instance and navigate to the Security Groups section.

2. Locate the security group attached to your EC2 instance and click on it.

3. Click on the Inbound rules tab and then Edit inbound rules.

Add a new rule:

- Type: HTTP

- Protocol: TCP

- Port Range: 80

Source: Anywhere (0.0.0.0/0) to allow global access or your specific IP for restricted access.

5. Click Save rules.

**Accessing Nginx in Your Browser**
1. Copy your EC2 instance's public IP address from the AWS dashboard.

2. Open your browser and paste the IP address followed by :80.


```
http://your-ec2-public-ip:80
```
<img width="1554" alt="Screenshot 2025-03-29 at 06 54 09" src="https://github.com/user-attachments/assets/220bc5f8-f771-4b5b-bb11-26b72d29c333" />

**Step 6**:
Access the running container’s terminal. To enter the Nginx container and explore its file system, use:

```
docker exec -it my-nginx /bin/bash
```

<img width="1252" alt="Screenshot 2025-03-29 at 06 59 54" src="https://github.com/user-attachments/assets/bd5cce4e-b391-408b-8a4d-8b56977ae611" />

This command gives you an interactive shell inside the container.

### Stopping and Removing Containers and Images
When you're done testing, it's good practice to clean up unused containers and images.

**Step 1**:
Stop the Running Nginx Container

```
docker stop my-nginx
```

This stops the container but does not delete it.

**Step 2**:
Remove the Stopped Container

```
docker rm my-nginx
```

<img width="480" alt="Screenshot 2025-03-29 at 07 02 06" src="https://github.com/user-attachments/assets/a514d076-b4aa-47d2-a517-588828dacbfc" />

This permanently deletes the container.

**Step 3**:
Remove the Nginx Image. If you no longer need the image, remove it with:

```
docker rmi nginx:latest
```
<img width="721" alt="Screenshot 2025-03-29 at 07 04 05" src="https://github.com/user-attachments/assets/a3e56338-d5ea-49bf-913f-119d76c4d9b4" />


## Conclusion
By following this guide, you have successfully:
- Launched an Ubuntu server on AWS
- Installed and configured Docker
- Pulled and managed images from Docker Hub
- Deployed an Nginx container and interacted with it
- Used essential Docker commands for container management


