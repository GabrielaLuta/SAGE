# Before you begin
Ensure you have the following:
* Install Docker from [here](https://docs.docker.com/get-started/get-docker/). 
* Create a Docker Hub account [here](https://app.docker.com/signup).
* Download Python from [here](https://www.python.org/downloads/).
* Download Git from [here](https://git-scm.com/downloads), if needed.

Use **ExampleApp** as a sample application. Test it locally and verify that the source code works. <!-- I need Dev/SME input for access to an existing app to provide as sample -->

# Create and deploy a Docker image for an app

1. Create the following directory and subfolders:

_mkdir quickstart_docker_  
_mkdir quickstart_docker/application_  
_mkdir quickstart_docker/docker_  
_mkdir quickstart_docker/docker/application_  

_quickstart_docker/ # Catalog of all project_  
_├──application/ # App code_    
_└──docker/ # Docker miscellaneous_    
_└──application/ # Dockerfile for app_  

2. In _quickstart_docker/application # Catalog of all project_ add a file called **application.py** and run the following script:  
```
import http.server
import socketserver

PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer(("", PORT), Handler)

print("serving at port", PORT)
httpd.serve_forever()
```

> [!NOTE]
> The application requires an environment built with Python. To save time, use something from **Docker Hub**.

3. In _mkdir quickstart_docker/docker/application_, create a file named **Dockerfile** with the following contents and save it:

```
# Use base image from the registry
FROM python:3.5

# Set the working directory to /app
WORKDIR /app

# Copy the 'application' directory contents into the container at /app
COPY ./application /app

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]

# Use base image from the registry
FROM python:3.5

# Set the working directory to /app
WORKDIR /app

# Copy the 'application' directory contents into the container at /app
COPY ./application /app

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Execute 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]
```
To read more about docker images and formatting guidelines, go [here](https://docs.docker.com/engine/reference/builder/).

4. Build the image by running the following command:

`docker build . -f-docker/application/Dockerfile -t exampleapp`

<!-- I need Dev/SME input for arguments, as I'm unsure of format and potential typos: Arguments: . - working catalog, build cotext;  -f docker/application/Dockerfile - docker-file; -t exampleapp - you tag the image and find it easily later.-->

5. View the image you built by running the following command:
```
$ docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
exampleapp latest 83ioe0edc28a 2 seconds ago 154MB
python 3.6 05stv8636w3f 6 weeks ago 154MB
```
6. Add the image to a private repository (or host it for free on Docker Hub).
 
  
