# ecs-getting-started-workshop
Amazon ECS Getting Started Workshop



## Launch your Lightsail

1. Go to [Lightsail console](https://lightsail.aws.amazon.com/ls/webapp/home/resources)
2. Create an instance(OS only)
3. Select **Amazon Linux** and then click **Create**
4. Connect to the Lightsail instance
5. `sudo yum update -y` to update all installed packages
6. `sudo yum install docker -y` to install dockder
7. `sudo service docker start` to start the docker daemon



## Run Caddy Web server with PHP

1. `sudo docker run -d -p 80:2015 --name web abiosoft/caddy:php`
2. `sudo docker ps` to check the container running status
3. open http://<your_lightsail_instance_ip> to see the php welcome page
4. stop the container with `sudo docker rm -f web`
5. create a local src directory at $HOME directory
6. `mkdir src`
7. launch the Caddy again with local volume mount:

`sudo docker run -d -v /home/ec2-user/src:/srv -p 80:2015 --name web abiosoft/caddy:php`







