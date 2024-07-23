# My custom Dockerfile

- A custom Dockerfile used to create a Jenkins agent image that can handle a variety of CI/CD tasks including building Docker images, running Docker Compose files, executing kubectl commands, and deploying Helm charts. This Dockerfile uses an Alpine-based Docker image as the base image and installs Docker, Docker Compose, kubectl, Helm, and git.
```sh
 $ cd custom-dockerfile

 $ docker image build -t jenkins-cicd-toolbox .
 # Publish Docker container to Docker Hub
 $ docker tag jenkins-cicd-toolbox ziedab/jenkins-cicd-toolbox
 $ docker push  ziedab/jenkins-cicd-toolbox

 cd ..
``` 

