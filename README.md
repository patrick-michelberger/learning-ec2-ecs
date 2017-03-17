# learning-ec2-ecs
This repo is used to learn using the Amazon EC2 Container Service

## Setup AWS

1. Install AWS Command line interface `$ pip install --upgrade --user awscli`
2. Configure it with your credentials `$ aws configure`

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