**Docker Swarm Architecture:**

![image](https://github.com/user-attachments/assets/210e00cc-c766-4b34-b205-895ac41d52b0)

![image](https://github.com/user-attachments/assets/5c7af11a-7569-41ef-a93b-70bdd04b9bd2)

Docker Swarm is a container orchestration tool for managing a cluster of Docker engines (Nodes) as a single virtual system.

 It is not initialized by default when you install Docker, but it gets installed along with Docker.
 
![image](https://github.com/user-attachments/assets/a324e467-8ddc-4ada-9b3e-bca3ac43f60a)


**Deploying a sample 3-tier application with Docker Swarm and orchestrating the containers**

**Step-by-Step Instructions:**

1. Provision 2 AWS EC2 Instances, preferably with AMI Ubuntu 24.04 LTS. This setup has 1 Master and 1 Worker node as part of the Swarm Cluster.

2. On both EC2 Instances, please install Docker, which is also going to install Docker Swarm:

   https://docs.docker.com/engine/install/ubuntu/

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

3. On the Master node, initiate the swarm cluster:

   	sudo docker swarm init

4. On the Worker node, execute the "docker swarm join" command generated in step 3:

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

11. sudo docker service scale swarmapp_frontend=3

12. sudo docker service ls   # check scaled application replicas

13. sudo docker service ps swarmapp_frontend

14. On Master node: watch sudo docker service ls

15. On Worker node: sudo docker ps

16. Delete one container manually: sudo docker rm -f container-id  #On the Master node, the replicas will automatically change from 2 to 3

   


