# How to deploy a .NET 7 Web API in Azure Container Instance

## 1. Create an Azure Container Registry

We create a Container Registry service

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/16399ba3-d529-4862-99ef-71713d08d594)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/3691d9c5-850c-4305-9de5-38fda83a8372)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/be13a2f2-7f66-43db-b30a-d2c6c9d14a4e)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/9fbbc617-f846-4c2d-b26d-5a813ed85e9a)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/30b1e0a2-ccea-4ef6-98ec-c93e3a69d0e3)

## 2. Create a .NET 7 Web API in Visual Studio 2022 Community Edition

Open Visual Studio 2022 Community Edition and we create a new .NET 7 API 

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/c0aed5a6-25b0-4c4f-9b8b-d7cf1f53761a)

We build and run the application to verify it

We navigate to the Web API URL contoller endpoint:

http://localhost:5026/weatherforecast

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/da91ee93-78ca-4858-8a9a-9039e93d25d7)

And also we visit the swagger API doc webpage:

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/3b0587a0-02bd-44ca-926c-81a1077cdbee)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/7f2cd3b9-edaf-4435-9345-3895b4d9d9a2)

## 3. Create the docker image and we push in Azure ACR

**IMPORTANT**! We first have to run **Docker Desktop**

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/8dfb1f93-83e3-4595-b95c-92cbe9ddaa9b)

Then we login in docker 

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/ee3d392f-5dbb-4382-a9b7-f462f15928c5)

If we want to reset the docker configuration, go to this path and delete the config.json file and then run the above command **docker login**

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/d263f28f-6cb5-49e7-aff7-8a94919b1a03)

Now we login in Azure CLI

```
az login
```

We go to the Azure Portal and we activate the admin user

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/40c6defd-2b5a-4c43-a036-8130657c92f7)

or we can activate the admin user with this command:

```
az acr update --name mywebapicontainer --resource-group myRG --admin-enabled true
```

Then we login in the Azure ACR and set the username and password in the admin user page

```
az acr login --name mywebapicontainer
```

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/9ce65286-9e73-4e21-879a-68ad6f607bb2)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/6e4b5967-bbd2-4c31-903f-917b1b70025a)

To automatically create the **Dockerfile** we add docker support to out application

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/5a478b78-860c-4ff8-ac5c-4c3502bb8b3c)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/d21664ac-1bba-45fd-a4fd-1bca266f58d1)

This is the dockerfile generated by Visual Studio

```dockerfile
#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebAPIdotNet7.csproj", "."]
RUN dotnet restore "./././WebAPIdotNet7.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./WebAPIdotNet7.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./WebAPIdotNet7.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAPIdotNet7.dll"]
```

## 4. We build the Docker Image

```
docker build -t mywebapicontainer.azurecr.io/mywebapicontainer:v1 .
```

## 5. We push the Docker image to the Azure ACR

```
docker push mywebapicontainer.azurecr.io/mywebapicontainer:v1
```

## 5. We create the Azure Container Instance (ACI) and we deploy it

```
az container create --resource-group myRG --name mycontainerinstance --image mywebapicontainer.azurecr.io/mywebapicontainer:v1 --cpu 1 --memory 1.5 --registry-login-server mywebapicontainer.azurecr.io --registry-username mywebapicontainer --registry-password tk5N+2tBFnNxImB0ByTt58Nt+HLvCwLWMA8bNn1lwY+ACRAeOtn/ --dns-name-label mywebapidns7788 --ports 80 --location westeurope
```

## 6. In Azure Portal we navigate to Azure ACI

We navigate to the ACI continer in Azure Portal

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/30df0435-831f-49ba-9901-ad6e6f04824f)

We copy the DNS and past it in the internet web browser: http://mywebapidns.westeurope.azurecontainer.io/weatherforecast

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/e94d42ca-e41e-44f8-a115-b5ddbae7ba54)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/c15ee849-15f8-47d3-ae5a-e723b7afa770)
