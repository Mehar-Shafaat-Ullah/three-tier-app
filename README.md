**Deploying a sample 3-tier application with Docker Swarm and orchestrating the containers**

**Step-by-Step Instructions:**

1. Provision 2 AWS EC2 Instances, preferably with AMI Ubuntu 24.04 LTS. This setup has 1 Master and 1 Worker node as part of the Swarm Cluster.

2. On both EC2 Instances, please install Docker, which is also going to install docker swarm:

   for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

   # Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

3. On Master node, initiate the swarm cluster:

   	sudo docker swarm init

4. On Worker node, execute the "docker swarm join" command generated in step 3:

   example command: sudo docker swarm join --token SWMTKN-1-65mdl491vt3fxe76g2egngd7lj23nsewprmhy6kvrmr4z386ka-byvige262gb93inckg1xfnrh5 172.31.19.78:2377

5. On Master node: sudo docker node ls

   ![image](https://github.com/user-attachments/assets/9f28a28a-daa3-4334-8128-2977586f3cf1)

6. Install git: sudo apt install git -y

7. Deploy a swarm stack for the cloned project:
  
   git clone https://github.com/bhavukm/swarmapp.git

   cd swarmapp

   sudo docker stack deploy -c docker-compose.yml swarmapp

8. Access the application: http://worker-node-ip:80

9. sudo docker service ls

10. On worker node: sudo docker ps


   


