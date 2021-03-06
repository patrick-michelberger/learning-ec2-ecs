# learning-ec2-ecs
This repo is used to learn using the Amazon EC2 Container Service

## Setup AWS

1. Install AWS Command line interface 

`$ pip install --upgrade --user awscli`

2. Configure it with your credentials 

`$ aws configure`

3. Install AWS ECS Command line interface

`$ sudo curl -o /usr/local/bin/ecs-cli https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-darwin-amd64-latest`

4. Apply execute permissions 

`$ sudo chmod +x /usr/local/bin/ecs-cli`

## How to

1. Create your cluster. Use a EC2 Keypair pem file

```ecs-cli up --keypair id_rsa --capability-iam --size 2 --instance-type t2.medium```

2. Create a compose file `docker-compose.yml`

```
version: '2'
services:
  wordpress:
    image: wordpress
    cpu_shares: 100
    mem_limit: 524288000
    ports:
      - "80:80"
    links:
      - mysql
  mysql:
    image: mysql
    cpu_shares: 100
    mem_limit: 524288000
    environment:
      MYSQL_ROOT_PASSWORD: password
```

3. Deploy compose file to cluster

```$ ecs-cli compose --file hello-world.yml up```

4. View the running containers on a cluster

```$ ecs-cli ps```

5. Scale the tasks on the cluster

```ecs-cli compose --file hello-world.yml scale 2```

6. Create an ECS Service from compose file

Stop all containers

```$ ecs-cli compose --file hello-world.yml down```

Create service

```$ ecs-cli compose --file hello-world.yml service up```

7. Clean up

Delete the service 

```$ ecs-cli compose --file hello-world.yml service rm```

Take down cluster

```$ ecs-cli down --force```


##  Amazon EC2 Container Registry

1. Authenticate Docker to an Amazon ECR registry with get-login

`$ aws ecr get-login`

2. Copy and paste the docker login command into a terminal to authenticate your Docker CLI to the registry. 

3. Create a repository 

`$ aws ecr create-repository --repository-name myrepo`

4. Push an image to Amazon ECR

`$ docker images`

Tag the image to push to your repository. Update it with your AWS account id and aws ECR domain.

`$ docker tag myimage AWS_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myimage`

Push the image.

`$ docker push AWS_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myimage`

5. Pull an image from Amazon ECR

`$ docker pull AWS_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myimage`


6. Delete an image 

`$ aws ecr batch-delete-image --repository-name myrepo --image-ids imageTag=myimage`

7. Delete repository 

`$ aws ecr delete-repository --repository-name myrepo --force`