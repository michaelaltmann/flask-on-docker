# Dockerizing Flask with Postgres, Gunicorn, and Nginx

## Want to learn how to build this?

Check out the [post](https://testdriven.io/blog/dockerizing-flask-with-postgres-gunicorn-and-nginx).

## Want to use this project?

### Development

Uses the default Flask development server.

1. Rename _.env.dev-sample_ to _.env.dev_.
1. Update the environment variables in the _docker-compose.yml_ and _.env.dev_ files.
1. Build the images and run the containers:

   ```sh
   $ docker-compose up -d --build
   ```

   Test it out at [http://localhost:5000](http://localhost:5000). The "web" folder is mounted into the container and your code changes apply automatically.

### Production

Uses gunicorn + nginx.

1. Rename _.env.prod-sample_ to _.env.prod_ and _.env.prod.db-sample_ to _.env.prod.db_. Update the environment variables.
1. Build the images and run the containers:

```
docker-compose -f docker-compose.prod.yml up -d --build
```

Test it out at [http://localhost:1337](http://localhost:1337). No mounted folders. To apply changes, the image must be re-built.

## AWS Deployment

### First Approach: Build Docker on EC2

# Copy

```
scp -i ~/.ssh/aws-eb.pem \
 -r $(pwd)/{.env.prod.*,docker-compose.prod.yml,services} \
 ubuntu@ec2-3-142-72-253.us-east-2.compute.amazonaws.com:flask-on-docker

```

### Build the docker containers on the EC2

```
ssh -i ~/.ssh/aws-eb.pem ubuntu@ec2-3-142-72-253.us-east-2.compute.amazonaws.com
cd flask-on-docker
docker-compose -f docker-compose.prod.yml build
```

### Start the docker containers on EC2

```
docker-compose -f docker-compose.prod.yml up -d
```

Open http://ec2-3-142-72-253.us-east-2.compute.amazonaws.com/web2

## Alternative Approach: Deploy Docker Images

Build docker image locally

```
 docker-compose -f docker-compose.prod.yml build
```

Authenticate to the ECR

```
aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 216407560218.dkr.ecr.us-east-2.amazonaws.com
```

Push the images

```
docker-compose -f docker-compose.prod.yml push
```

Log into EC2

```
ssh -i ~/.ssh/aws-eb.pem ubuntu@ec2-3-142-72-253.us-east-2.compute.amazonaws.com
cd flask-on-docker
```

Pull the images

```
docker pull 216407560218.dkr.ecr.us-east-2.amazonaws.com/docker-reg:nginx
docker pull 216407560218.dkr.ecr.us-east-2.amazonaws.com/docker-reg:web
docker pull 216407560218.dkr.ecr.us-east-2.amazonaws.com/docker-reg:web2
```

### Start the containers

```
docker-compose -f docker-compose.prod.yml up -d
```
