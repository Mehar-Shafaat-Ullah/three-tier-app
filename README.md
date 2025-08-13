![image](https://github.com/user-attachments/assets/210e00cc-c766-4b34-b205-895ac41d52b0)

![image](https://github.com/user-attachments/assets/5c7af11a-7569-41ef-a93b-70bdd04b9bd2)
 
![image](https://github.com/user-attachments/assets/a324e467-8ddc-4ada-9b3e-bca3ac43f60a)

![image](https://github.com/user-attachments/assets/8ee33fe2-ca78-4c0c-84b5-b6a8d9aac84e)

![image](https://github.com/user-attachments/assets/8fc554ae-6ad6-46af-a276-eb000026137c)


![image](https://github.com/user-attachments/assets/9c199eca-8887-4df8-8ad4-6f8504254f77)

![image](https://github.com/user-attachments/assets/a510a67e-9b79-4f75-8d2d-88e20c1cf8ce)

**Deploying a sample 3-tier application with Docker Swarm**

**Step-by-Step Instructions:**

1. Provision 2 AWS EC2 Instances, preferably with AMI Ubuntu 24.04 LTS. This setup has 1 Master (Manager) and 1 Worker (Slave) node as part of the Swarm Cluster.

2. On both EC2 Instances, please install Docker , which is also going to install Docker Swarm:

   https://docs.docker.com/engine/install/ubuntu/

3. On the Master node, initiate the swarm cluster:

 _sudo docker swarm init_

4. On the Worker node, execute the "docker swarm join" command generated in step 3:

   example command: _sudo docker swarm join --token SWMTKN-1-65mdl491vt3fxe76g2egngd7lj23nsewprmhy6kvrmr4z386ka-byvige262gb93inckg1xfnrh5 172.31.19.78:2377_

5. On Master node: _sudo docker node ls_

   ![image](https://github.com/user-attachments/assets/9f28a28a-daa3-4334-8128-2977586f3cf1)

6. Install git: _sudo apt install git -y_

7. Deploy a swarm stack for the cloned project:
  
  _git clone https://github.com/bhavukm/swarmapp.git_

  _cd swarmapp_

  _sudo docker stack deploy -c docker-compose.yml swarmapp_

8. Access the application: http://worker-node-ip:80

9. _sudo docker service ls_
   
10. On worker node: _sudo docker ps_

11. _sudo docker service scale swarmapp_frontend=3_

12. _sudo docker service ls_   # check scaled application replicas

13. _sudo docker service ps swarmapp_frontend_

14. On Master node: _watch sudo docker service ls_

15. On Worker node: _sudo docker ps_

16. Delete one container manually: _sudo docker rm -f container-id_  #On the Master node, the replicas will automatically change from 2 to 3
