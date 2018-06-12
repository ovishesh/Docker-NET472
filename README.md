## Running a simple ASP.NET 4.7.2 Framework Web application on Docker


1. File -> New -> Project
2. Select ASP.NET Framework
    - Target any .NET Frameworks from this list - `4.0`, `4.5.2`, `4.6.2`, `4.7.2`
    - MVC App with Web API
3. Create a simple Dockerfile similar to the [sample here](./WebApp/Dockerfile)
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
5. Run `docker run --name <conatiner name> --rm -it -p 8000:80 <docker image name>` in the CLI to run the docker container
6. Follow [this guide](https://github.com/microsoft/dotnet-framework-docker/blob/master/samples/aspnetapp/README.md#view-the-aspnet-app-in-a-running-container-on-windows) to view the ASP.NET app in a running container on Windows.