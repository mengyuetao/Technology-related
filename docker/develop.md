
# best practices

- keep small : multistage builds



# command

```
docker build -t kubia .      # build a image
docker container ls -a       # list all the containers
docker exec -it bukia-container bash  # execute command in container bukia-container
docker image ls                       # get all the service
docker inspect kubia-container        # inspect kubia-container
docker login                          # login
docker ps -a                          # get all the docker process
docker pull mengyuetao/kubia          # get the image whoes name is kubia from repo of mengyuetao  
docker push mengyuetao/kubia          # push the image
docker rm kubia-container                  # remove conatiner
docker run busybox echo "Hello world"      # run a image with parameters
docker run --name kubia-container -p 8080:8080 -d kubia   #run a image  with container name and port maps  
docker run -p 8001:8080 -d mengyuetao/kubia       
docker service ls                         # list all the service
docker stop kubia-container               # stop a service
docker tag kubia mengyuetao/kubia         # tag the image 

```
