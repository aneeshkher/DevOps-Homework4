# DevOps CSC 591 007 - Homework 4  
#### Name: Aneesh Kher
#### Unity ID: aakher  

---

## Task 1 - File IO  
For this task, I created a [Dockerfile](https://github.com/aneeshkher/DevOps-Homework4/blob/master/task1/Dockerfile) and built an image using it. Once the image was build, I performed the operations using the following commands.  
First and foremost, I build an image and created a container using the commands.  

```
docker build -t hw_part1_image .
docker run -d --name hw4_part1_cont hw4_part1_image
```  

The port was exposed using the following command in the docker file.  
```
EXPOSE 9001
CMD socat TCP4-LISTEN:9001,fork,reuseaddr SYSTEM:"cat my_created_file.txt" | sh
```  

After building the image, I put a file in the container using the command.

`sudo docker exec -it <id> script /dev/null -c "echo 'Hey there DevOps' > hw1_DevOps.txt"`  

This created a file inside the container.  

Finally, running the following command linked the ambassador container to the original one.  
`docker run -d --link hw4_part1_cont --name hw4_part1_amb -p 40000:9001 svendowideit/ambasador`  

Using `curl -i localhost:40000`, I retrieved the file.  

---
## Task 2 - Ambassador Pattern  
In this task, docker-compose files were used to create and spin up containers. Two machines were used, one of which ran a redis-server and an ambassador to the server, and the other one ran a redis-client and an ambassador to the client. Using `docker-compose up`, the containers were brought up.  
Once brought up, the client would communicate with the server only through the ambassador container, and never directly.  

---

## Task 3 - Docker Deploy  
For this task, the green-blue deployment strategy was used. A `post-commit` hook was configured in the App's repository, which contained the commands to build a docker image and push it to the local repository. For this method to work, a container running `registry` had to be spun up, which would listen on port 5000 and store the images in a local repository.  

The repositories were configured on the same machine using the blue-green strategy in different directoried. To push to them, a custom remote had to be added.  
```
git remote add blue file:///root/deploy/blue.git
git remote add green file:///root/deploy/green.git
```

After adding the remotes, all changes can be pushed to these repositories, which are initialized using `git init --bare`  


On making a change to the `main.js` file, the machine would be build and pushed to the local repository on a commit. On a push, the `post-receive` file would be called, which would  pull the image, remove the old containers, and deploy the new one. This way, all the changes made to the file were immediately visible on the browser after the push succeeded.  The commands used for pushing were,  
```
git push blue master
git push green master
```

---

## Screencasts
1. Link to [task 1 screencast](https://www.youtube.com/watch?v=_4CaYVHMyDE).
2. Link to [task 2 screencast](https://www.youtube.com/watch?v=VzQQEAeolIM).
3. Link to [task 3 screencast](https://www.youtube.com/watch?v=N7Es6hnZ0UE).

---


