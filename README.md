Lets consider aspnet core mvc project.
<br />
Create dockerfile inside your project root folder
<br />
Add below snippet into it
```
FROM microsoft/dotnet:2.2-sdk-nanoserver-1709 as publish
WORKDIR /publish
COPY core.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish --output ./out

FROM microsoft/dotnet:2.2-aspnetcore-runtime-nanoserver-1709 
LABEL author="Pravin Nidoni"
WORKDIR /var/www/app
COPY --from=publish /publish/out .
ENV ASPNETCORE_URLS=http://*:5000
ENTRYPOINT ["dotnet", "core.dll"]

# CMD ["/bin/bash", "-c", "dotnet restore && dotnet run"]

```
Run the above snippet using below command to build the docker image for `windows deamon`.
```
 docker build --rm -f "win.Dockerfile" -t coreapp.win .
```
The above command will generate `coreapp.win` image in your local.
To find out all the available images run below command.
```
docker images
```
To create the container from above image run 
```
docker run -p 6100:5000 coreapp.win
docker run -p <HOST_PORT>:<CONTAINER:PORT> IMAGE_NAME
docker run -it -p 8080:5000 -v %cd%:C:/app -w "/app" microsoft/dotnet:2.2-sdk-nanoserver-1709 &REM creates volume /app and it points to current working directory in windows. Opens interactive terminal
```
The above command will create container and website will start on port `localhost:6100`. Run below command to see all the running containers and some other useful commands related to containers.
```
docker ps		&REM lists all the running containers
docker ps -all	&REM lists all the available containers
docker container ls --all	&REM lists all the available containers
docker stop <container-id>	&REM to stop the running container
docker rm <container-id>	&REM to remove the container
```
<br />
Some of the other useful commands

Create image from base image
```
FROM <base-image-name>
e.g: FROM microsoft/aspnetcore
```
Use Label to set the author or teh version. It's key value pair.
```
LABEL author="" version=""
e.g: LABEL author="Pravin Nidoni"
```
Use WORKDIR command to set the working directory `e.g: setting the docker working directory to \publish`
```
WORKDIR \publish
```
Copy used to copy the files or folders

below statement copies filename.cs file to current working directory of the docker(to \publish in the above case)
```
COPY filename.cs .
```
below statement copies entire content to current working directory of the docker(to \publish in the above case)
```
COPY . .
```
`below statement copies image output to another image`
```
COPY --from=publish /publish/out .
```
Execute commands using RUN
```
RUN dotnet restore
RUN dotnet publish --output ./out
```
Create image from docker file
```
docker build -t <un>/aspnetapp .
docker build -t <un>/aspnetapp -f "dockerfilename" .
```
