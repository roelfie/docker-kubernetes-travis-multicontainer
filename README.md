# docker-kubernetes-travis-multicontainer
Project used in the Docker / Kubernetes Udemy course, to setup a CI/CD pipeline with Travis &amp; AWS for a multi-container application.

### Fibonacci: Multi-container React App
The starting point for this project is the 
[Fibonacci React App](https://github.com/roelfie/docker-kubernetes-course/tree/main/src/docker/8_docker_fibonacci)
from the Docker / Kubernetes course.

The Fibonacci app is an application [composed of 6 containers](./docker-compose.yml).

### Container registries
In [docker-kubernetes-travis-ci-cd](https://github.com/roelfie/docker-kubernetes-travis-ci-cd) we have created a 
Travis pipeline for a single container application. That pipeline cloned the repository into AWS S3 and let 
Beanstalk do the 'docker build' (based on a 2-phase Dockerfile that first performed an 'npm run build' in a Node 
image & then copied the resulting /app/build folder into an Nginx image).

In this pipeline we will [let Travis build & upload the images to Docker Hub](./.travis.yml) so Beanstalk can 
simply download & deploy the containers from Docker Hub (*).

It will however need to be instructed what to do with these images. The previous example had only one image, and 
Beanstalk knew what to do with it (docker build). With multiple containers it is a bit more complex. See below...

(*) A similar approach works for any cloud provider. And instead of using Docker Hub, we could use any container 
registry (like AWS ECR or GCP Container Registry).

### Multiple Nginx

The application contains two instances of nginx: 
* One nginx in front, to route the data to 'client' or to 'api'.
* One nginx to host the 'client' React app (on a non-standard port!)

![nginx instances](./_img/multiple-nginx.png)

### Dockerrun.aws.json vs. docker-compose.yml

#### Dockerrun.aws.json
The course is outdated. AWS used its own build file called `Dockerrun.aws.json` (resembling `docker-compose.yml`) to 
deploy a multi-container application onto a _Multi-container Docker running on 64bit Amazon Linux_.

* [Dockerrun.aws.json](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_v2config.html)
* [AWS ECS Task Definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)

#### docker-compose.yml 
The multi-container platform will be discontinued in July 2022 and from now on we should use the _AWS Linux 2_ 
platform. Deployment to the _Amazon Linux 2_ platform is done using a standard 
[docker-compose.yml](./docker-compose.yml).

See [instructions on AWS Linux 2 platform](./README-LINUX2.md) (copied from section 157 of the course).

* [Docker configuration](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/single-container-docker-configuration.html)
* [Migrating from Multi-container Docker to AWS Linux 2](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/migrate-to-ec2-AL2-platform.html)


### More info
* https://docs.docker.com/engine/reference/run/#env-environment-variables
* https://create-react-app.dev/docs/running-tests/#continuous-integration


