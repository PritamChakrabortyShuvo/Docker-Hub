# ✨Docker File to Docker Container✨

This guide will walk you through the basic steps of creating a Dockerfile, building a Docker image, and running a Docker container on your local machine. It also includes instructions for deploying to a remote server or cloud service.

## Step 1 : Create a DockerFile
```bash
  #Use the slim base Nginx image as base image 
  FROM nginx:stable-alpine

  # Copy the my HTML file to Nginx HTML directory
  COPY index.html /usr/share/nginx/html/

  #Expose Port 80
  EXPOSE 80
```
### Explanation of Dockerfile Instructions
**FROM:** Specifies the base image to use.

**COPY:** Copies files from the host machine to the container.

**EXPOSE:** Specifies the port the container listens on.

## Step 2 : Create a HTML file

```bash
  <!DOCTYPE html>
  <html>
  <head>
      <title>NodeJS</title>
  </head>
  <body>
      <h3>Hi!! User</h3>
      <p>Welcome to Nginx!!</p>
      <p>Congratulations! You successfully accessed the Nginx environment.</p>
  </body>
  </html>
```
## Step 3 : Create a Docker Image 

Once you have created your Dockerfile, you can build the Docker image using the following command:

```bash 
  docker build -t image_name:version .
```
**Breakdown :**

**```docker build -t image_name:version```:** Command to build the Docker image & tags the image with a name & version

**```.```:** Specifies the build context (current directory).

## Step 4 : Push the image on registry

Before pushing your image, you need to log in to Docker Hub. You can do this using the ```docker login``` command. You'll be prompted to enter your Docker Hub username and password.

```bash
  docker login
```
```bash
  docker push dockerhub-username/image_name:version
```
**Breakdown :**

**```docker push```**: This is the command used to push an image or a repository to a Docker registry.

**```your-dockerhub-username```**: This is a placeholder for your actual Docker Hub username.

**```image_name```**: This is the name of your Docker image.

**```:version```**: This is the tag for the specific version of your image. Tags are often used to differentiate between different versions of the same image.

## Step 4 : Run the Container

```bash
  docker run -d -p 4000:80 --name container_name your-dockerhub-username/image_name:version
```
**Breakdown :**

**```docker run -d```**: Command to run the container in detached mode.

**```-p 4000:80```**: Maps port 4000 on the host to port 80 in the container.

**```--name container_name```**: This flag assigns a name to the running container, making it easier to manage.

**```your-dockerhub-username/image_name:version```**: This specifies the Docker image to use for creating the container. It includes the Docker Hub username, the image name, and the specific version tag.

## Step 5 : Verify the Container is Running

You can verify that the container is running by opening a web browser and navigating to ```http://localhost:4000```. You should see the output of your Nginx application.

## Conclusion 

This guide covers the basic steps of **creating a Dockerfile, building a Docker image, and running a Docker container both locally and by deploying it to Docker Hub.** There are many more things you might need to do depending on your application's requirements, such as creating configuration files (like JSON), managing dependencies with ```requirements.txt```, and handling other setup tasks. 

For more advanced usage and best practices, visit [docker.docs](https://docs.docker.com/)