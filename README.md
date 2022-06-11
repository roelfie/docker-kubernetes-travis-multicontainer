# docker-kubernetes-travis-multicontainer
Project used in the Docker / Kubernetes Udemy course, to setup a CI/CD pipeline with Travis &amp; AWS for a multi-container application.

## Fibonacci: Multi-container React App
The starting point for this project is the 
[Fibonacci React App](https://github.com/roelfie/docker-kubernetes-course/tree/main/src/docker/8_docker_fibonacci)
from the Docker / Kubernetes course.

The Fibonacci app is an application [composed of 6 containers](./docker-compose.yml).

### Container registries
In [docker-kubernetes-travis-ci-cd](https://github.com/roelfie/docker-kubernetes-travis-ci-cd) we have created a 
Travis pipeline for a single container application. That pipeline cloned the repository into AWS S3 and let 
Beanstalk do the 'docker build'.

In this pipeline we will [let Travis build & upload the images to Docker Hub](TODO) so Beanstalk can 
simply download & deploy the containers from Docker Hub.

A similar approach works for any cloud provider. Instead of using Docker Hub, we could use any container registry 
(like AWS ECR or GCP Container registry).


### More info
* https://docs.docker.com/engine/reference/run/#env-environment-variables
* https://create-react-app.dev/docs/running-tests/#continuous-integration

My Travis builds of this GitHub project can be found [here](TODO).


