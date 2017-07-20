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



## Build your own Dockerfile

1. prepare your `Dockerfile`
2. `sudo docker build -t mycaddy .` to build your own `mycaddy:latest `docker image
3. run your own docker image with `sudo docker run -d -p 80:2015 mycaddy`
4. reload the browser to see the changes



## Create a new IAM User

1. go to IAM console 

2. Create a IAM user "**ecs-builder**" with **Programmatic access**

3. select Attach **existing policies directly**

4. select **AmazonEC2ContainerRegistryFullAccess**

5. **Next Review**

6. **Create User**

7. click **Show** under **Secret access key**

8. go to Lightsail shell, run `aws configure` to configure the **Access key ID** and **Secret access key**

   ```
   [ec2-user@ip-172-26-6-78 ~]$ aws configure
   AWS Access Key ID [None]: XXXXXXXXXXXXXXX
   AWS Secret Access Key [None]: XXXXXXXXXXXXXXXXXXXXX
   Default region name [None]: ap-northeast-1
   Default output format [None]: 
   [ec2-user@ip-172-26-6-78 ~]$ 
   ```

9. run ` aws ecr describe-repositories` to see the existing list of repos on ECR (you may see an empty list)



## prepare the ECR and push the Docker image

1. create a new ECR repository `aws ecr create-repository --repository-name mycaddy`

```
{
    "repository": {
        "registryId": "903779448426", 
        "repositoryName": "mycaddy", 
        "repositoryArn": "arn:aws:ecr:ap-northeast-1:903779448426:repository/mycaddy", 
        "createdAt": 1500591587.0, 
        "repositoryUri": "903779448426.dkr.ecr.ap-northeast-1.amazonaws.com/mycaddy"
    }
}
```

2. list the repo again `aws ecr describe-repositories`  and you should be able to see the new created repo

3. run `$(aws ecr get-login) ` to evaluate the docker login credentials

4.  tag your docker image 

   ```
   $ sudo docker tag mycaddy:latest 903779448426.dkr.ecr.ap-northeast-1.amazonaws.com/mycaddy:lates
   ```

   ​

5. push your docker image to ECR

   ```
   $ sudo docker push 903779448426.dkr.ecr.ap-northeast-1.amazonaws.com/mycaddy:latest
   ```

   ​





# Clean Ups

1. delete the Lightsail instance
2. delete the IAM user **ecs-builder** created in this workshop

