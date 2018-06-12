## Running a simple ASP.NET 4.7.2 Framework Web application on Docker


1. File -> New -> Project
2. Select ASP.NET Framework
    - Target 4.7.2
    - MVC App with Web API
3. Copy Dockerfile
4. Run `docker build --pull -t <docker image name> .` in the CLI to build a docker image
    - Make sure you have set Docker to run Windows containers 
5. Run `docker run --name <conatiner name> --rm -it -p 8000:80 <docker image name>` in the CLI to run the docker container
6. Follow [this guide](https://github.com/microsoft/dotnet-framework-docker/blob/master/samples/aspnetapp/README.md#view-the-aspnet-app-in-a-running-container-on-windows) to view the ASP.NET app in a running container on Windows.
