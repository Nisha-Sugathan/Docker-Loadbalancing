# Docker-Loadbalancing
Here the load balancing is enabled for three containers running in a single instance , all containers are listening to different ports..We are adding add all the ports in the registered targets.
### Prerequisites
Need to install docker on your machine
Add your username to docker group

### Docker Installation

```
sudo yum install docker -y &> /dev/null
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo usermod -a -G docker ec2-user
```
![docker_installation](https://github.com/Nisha-Sugathan/Docker-Bind_mounting/assets/134600837/ba7797c4-9a73-4ce6-b593-2befa5850e0d)

### Create a website setup on 3 directories
```
  wget https://www.tooplate.com/zip-templates/2134_gotto_job.zip
  unzip 2134_gotto_job.zip
  cp -r 2134_gotto_job website1
  cp -r 2134_gotto_job website2
  cp -r 2134_gotto_job website3
 ``` 
  
  ### Create 3 docker containers using the httpd:alpine image

```
  
  docker container run -d \
  --name website1 \
   -p 8081:80  \
   --restart always \
   -v /home/ec2-user/website1/:/usr/local/apache2/htdocs/ \
    httpd:alpine
```

```

  docker container run -d \
  --name website2 \
   -p 8082:80  \
   --restart always \
   -v /home/ec2-user/website2/:/usr/local/apache2/htdocs/ \
    httpd:alpine
    
  ```
  
  ```
      docker container run -d \
  --name website3 \
   -p 8083:80  \
   --restart always \
   -v /home/ec2-user/website3/:/usr/local/apache2/htdocs/ \
    httpd:alpine

```
All websites are loading properly

```
http://ec2-3-109-209-162.ap-south-1.compute.amazonaws.com:8081/
http://ec2-3-109-209-162.ap-south-1.compute.amazonaws.com:8082/
http://ec2-3-109-209-162.ap-south-1.compute.amazonaws.com:8083/

```
### Create a target group for three target group

The instances are listening for three ports, so we need to add all the ports in the registered targets

![Target_port](https://github.com/Nisha-Sugathan/Docker-Loadbalancing/assets/134600837/be0b4298-c1cc-4d2a-9732-2647e305299d)

### create a  load balancer with listener port to 443 and also add a listener to redirect to port 80
![AWS_LB](https://github.com/Nisha-Sugathan/Docker-Loadbalancing/assets/134600837/7975b916-ffcd-40d8-af66-e48fefedf250)


### Add a domain in Route53 hosted zone and add A record as alias to ALB name

![Route 53](https://github.com/Nisha-Sugathan/Docker-Loadbalancing/assets/134600837/c43b9172-0cb3-4ca4-9dcc-199438076a2c)

https://dockerlb.devopstest2023.online/
