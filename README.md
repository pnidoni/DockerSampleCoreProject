Lets consider aspnet core mvc project.
Create dockerfile inside your project root folder
Add below snippet into it
```
FROM microsoft/aspnetcore-build as publish
WORKDIR /publish
COPY projectname.csproj .
RUN dotnet restore
COPY . .
RUN dotnrt publish --output ./out

FROM microsoft/aspnetcore 
LABEL author="Pravin Nidoni"
WORKDIR /var/www/app
COPY --from=publish /publish/out .
ENV ASPNETCORE_URLS=http://*:5000
ENTRYPOINT ["dotnet", "projectname.dll"]

CMD ["/bin/bash", "-c", "dotnet restore && dotnet run"]

```

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

`below statement copies filename.cs file to current working directory of the docker(to \publish in the above case)`
```
COPY filename.cs .
```
`below statement copies entire content to current working directory of the docker(to \publish in the above case)`
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
