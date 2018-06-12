## Running a simple ASP.NET 4.7.2 Framework Web application on Docker

This guide walks you through to building a simple Docker Windows Server Core container running the latest 1803 version that comes with a lot of enhancements (such as: half in size :), networking improvements, etc.). The good thing is that the dotnet framework SDK image on [Docker Hub](https://hub.docker.com/r/microsoft/dotnet-framework) with tag `4.7.2-sdk-windowsservercore-1803` already allows room for us to target either `4.0`, `4.5.2`, `4.6.2` or `4.7.2` to build our app, you can read more about it [here](https://github.com/Microsoft/dotnet-framework-docker/blob/master/4.7.2-windowsservercore-1803/sdk/Dockerfile#L48). We can run this on the image `microsoft/aspnet:4.7.2-windowsservercore-1803`. 

# Follow these steps to run

1. File -> New -> Project
1. Select ASP.NET Framework
    * Target any .NET Frameworks from this list - `4.0`, `4.5.2`, `4.6.2`, `4.7.2`
1. Create a simple Dockerfile similar to the [sample here](./WebApp/Dockerfile)
````dockerfile
FROM microsoft/dotnet-framework:4.7.2-sdk-windowsservercore-1803 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY <path-to>/*.csproj ./<path-to>/
COPY <path-to>/*.config ./<path-to>/
RUN nuget restore

# copy everything else and build app
COPY <path-to>/. ./<path-to>/
WORKDIR /app/<path-to>
RUN msbuild /p:Configuration=Release


FROM microsoft/aspnet:4.7.2-windowsservercore-1803 AS runtime
WORKDIR /inetpub/wwwroot
COPY --from=build /app/<path-to>/. ./
````
4. Run `docker build --pull -t <docker image name> .` in the CLI to build a docker image
    - Make sure you have set Docker to run Windows containers 
1. Run `docker run --rm -it -p 8000:80 <docker image name>` in the CLI to run the docker container
1. Follow [this guide](https://github.com/microsoft/dotnet-framework-docker/blob/master/samples/aspnetapp/README.md#view-the-aspnet-app-in-a-running-container-on-windows) to view the ASP.NET app in a running container on Windows or follow the steps below:
    * Open up another command prompt.
    * Check the name of the conatiner by running `docker ps` and look at the CONTAINER ID or NAMES section that maps to your conatiner image 
    * Run `docker exec <container name> ipconfig`.
    * Copy the container IP address and paste into your browser (for example, `172.29.245.43`).


## References
1. [Dockerfile for 4.7.2-windowsservercore-1803 SDK ](https://github.com/Microsoft/dotnet-framework-docker/blob/master/4.7.2-windowsservercore-1803/sdk/Dockerfile#L48)
1. [View the ASP.NET app in a running container on Windows](https://github.com/microsoft/dotnet-framework-docker/blob/master/samples/aspnetapp/README.md#view-the-aspnet-app-in-a-running-container-on-windows)
1. [dotnet framework on docker hub](https://hub.docker.com/r/microsoft/dotnet-framework)
1. [ASP.NET on docker hub](https://hub.docker.com/r/microsoft/aspnet)