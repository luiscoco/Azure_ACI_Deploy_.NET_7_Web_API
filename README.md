# How to create a Docker image from a .NET 8 Web API and how to upload it to Azure Container Registry (ACR)

## 1. Create an Azure Container Registry

We create a Container Registry service

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/16399ba3-d529-4862-99ef-71713d08d594)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/3691d9c5-850c-4305-9de5-38fda83a8372)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/be13a2f2-7f66-43db-b30a-d2c6c9d14a4e)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/9fbbc617-f846-4c2d-b26d-5a813ed85e9a)

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/30b1e0a2-ccea-4ef6-98ec-c93e3a69d0e3)

## 2. Create a .NET 8 Web API 

Open VSCode and in a Terminal window for creating a new Web API without HTTPS type this command:

```
dotnet new webapi --no-https --framework net8.0
```

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/a64a9e9b-a983-423c-b420-52e15859a0c1)

We verify the application with the command

```
dotnet run
```

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/de2f16e5-1de4-44a3-a040-d4d08aa84284)

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

Install the **Docker** extension in VSCode

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/6c9243c9-9b8e-4b28-9ce6-ba5ba3286449)

Press Ctrl+Shift+P and type "**Add Docker Files to Workspace**"

![image](https://github.com/luiscoco/Azure_ACR_Upload_.NET_8_Web_API/assets/32194879/25f1dcfd-631b-497a-bad7-5bfb416a33d4)



## 4. 



## 5. 
