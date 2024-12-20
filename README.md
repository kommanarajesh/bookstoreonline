

# CloudLibrary App Architecture Steps
----------------------------------
-------------------------------------


# Please refer the below github link for the updated code
https://github.com/muralikrishna188/bookstoreonline.git

1. Create a Vnet1 and subnet1 with cloudlibrary-rg

Virtual network IP Address range - 10.50.0.0/16 (Select any IP address range)
catalog-subnet - 10.50.1.0/24

2. Create second vnet and subnet with cloudlibrary-rg

vnet ip address range - 10.90.0.0/16
weather-subnet -  10.90.1.0/24

## Download .net sdk SDK 6.0.306
1. download .net core 6.0.306 version using the below link

   ASP.NET Core Runtime 6.0.33
   Hosting Bundle

https://dotnet.microsoft.com/en-us/download/dotnet/6.0

2. Install .net sdk

3. Verify the dotnet version

## Intsall extensions in VS code 
-----------------------
Install below extensions in the Visual Studio Code

C# extension,
Azure Account, 
App service

## Catalog Service Setup
-----------------------------------
## Setup up catalog service - local desktop

1. Copy the catalog code base into local system

2. open the code with VSCode

3. build the project using below command
   #### dotnet build 

4. publish the code using below command
   #### dotnet publish -o publish
   it will create a folder called publish

5.  code location
https://github.com/muralikrishna188/bookstoreonline.git

Please clone the code

(Important note: Please   use **core-update-v1** branch to clone the code & use below command

git checkout core-update-v1


## Setup up catalog service - Windows Server
6. Install IIS

7. Install dotnet core 6
https://dotnet.microsoft.com/en-us/download/dotnet/6.0

8. Copy publish folder to server C drive

9. Create a website and map to local path

## Weather service setup

1. create a ubuntu20.4 server

2. install git on the weather server using below command - 
   #### sudo apt install git
   
3. Download the code using command
   Here swicth to core branch and clone the code 
   #### git clone https://github.com/muralikrishna188/bookstoreonline.git
   
4. Execute below commands
   #### sudo apt update
   #### sudo apt install nodejs
   #### sudo apt install npm
   #### sudo npm start

## Inventory service setup   

### Setup up Inventory service - local desktop

1. Copy the Inventory code base into local system & Azure Cloud
https://github.com/muralikrishna188/bookstoreonline.git

2. Open the code with VSCode

3. Run the below command to build the project
   #### dotnet build 

4. Run the below command to publish the project
   #### dotnet publish -o publish
   it will create a folder called publish

6. Create a webapp with Azure cloud
   
5. Go to publish folder right click -> Deploy to webapp   

## SetUp Cart service using docker, Azure Container registry and Kubernetes
1. Setup Docker server with ubuntu OS using the below documentation
https://docs.docker.com/engine/install/ubuntu/

2. Install git in the docker server

3. Create git PAT token and Clone the Cart code to docker server

Git clone https://github.com/muralikrishna188/bookstoreonline.git
4. Open the folder & Create docker image using docker build command

5. Create Azure Container regisrty using the below document
https://learn.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal?tabs=azure-cli

6. Install azure CLI in the docker server to Push the image from docker server to container registry
https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt

7. Now login to Azure container regstry from docker server using Azure Cli

8. Use az login command to login azure cloud

or we can use below steps to push the Image

9. Login Azure Container Registry (ACR) with below command

   Collect username, password from Azure Container Registry

#### docker login azurecr.io -u username -p COPIEDKEYAZURECONTAINERREGISTRY

10. Add the docker image tag before pushing to Azure Container registry
#### docker tag imagename acrregistryname/imagename

11. Now Push the docker the image from docker server to Azure Container Registry 
#### docker push bookshopacr101.azurecr.io/cart:latest

12. Now observe the pushed image in the Azure Container Regisrty

13. Create kubernetes cluster with Azure Container registry with below document
https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli

14. Add kubernetes plugin in visual studio code 

15. Login to Kubernetes cluster using Azure CLI

#### az aks get-credentials -n cloudlibrarykube101 -g cloudlibrary-rg

16. update Azure container regisrty name in Kubernetes deployment.yml file

17. Depoly application into the kubernetes cluster

18. Verify the app with service IP address

## Setup order service with FunctionApp Local and Azure
1. Copy the Order service code base into local system
https://github.com/muralikrishna188/bookstoreonline.git

2. Install Azure Functions Core Tools in local system
https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=v4%2Cwindows%2Ccsharp%2Cportal%2Cbash

a. Version 4.x
b. windows 64 bit
Now download and  install Functions Core Tool

3. Open the order service code with VSCode

4. here we can observe 

Recieve orders from HTTP request and store into storage blob
Trigger - HTTP
Binding - Blob storage

5. Observer Order.cs and Ordersample.json

6. Add Azure functions extension in the Visual Studio Code

7. Once you add Azure Functions extension in the VSCode observe the Functions in the Local project workspace

a. Click the Azure Account -> observe workspace

8. Before Running Functions, Run below commands

a. dotnet restore
b. dotnet build


## Function app with Azure CLoud

1. Create storage account
##### az storage account create --name functdstg585 --resource-group cloudlibrary-rg --location eastus --sku Standard_LRS

2. Create functionapp with previous storage account

##### az functionapp create --consumption-plan-location eastus --name fnbtgbapddt10125 --os-type Windows --resource-group cloudlibrary-rg --runtime dotnet --storage-account functdstg585567 --functions-version 4

a. verify the functionapp in azure portal

3. Now goto VS code and Deploy the function to the Azure

4. observer the output now

5. Now we can observer the functions in the Azure portal

6. Click ProcessorderCosmos function -> Click Code+Test 

7. Click copy function url

8. Now open the POSTMAN app 
a. paste the fucntion url
b. copy and paste the ordersample.json in the body with raw data

9. Now you should see some logs in under Logs below path
 FunctionApp -> Functions -> click ProcessOrderFunction -> Click Code+Test -> Logs

 The log window is a bit fragile and doesn't always show the logs. However, logs are also written to the log files.

You can access these logs from the Kudu console: https://[your-function-app].scm.azurewebsites.net/

From the menu, select Debug console > CMD

On the list of files, go into LogFiles > Application > Functions > Function > [Name of your function]

There you will see a list of log files.

## Setup Catalog Application Network security group (NSG) rules 
1. Till now we are able to access the catalog app within server only
 let us create NSG rule to access the Catalog server from User desktop & from the Internetet

2. Create one more NSG rule to access SSH the weather server from local desktop

3. Currently Catalog and weather VMs are placed in same vnet & subnet.
 Create one more subnet (weather-new-subnet) in bookshopaaprg-vnet to place the weather server. 

4. Now move the weather server from existing subnet to new subnet (weather-new-subnet)
 go to weather vm -> Networking -> network interface -> ip configuration -> subnet -> change to new subnet
once you change subnet , vm will be restarted, we need start the nodejs application.

5. Create a new network security group (NSG) named as weather-subnet-nsg with rules SSH from local desktop 

6. Now go to New NSG weather-subnet-nsg -> subnets -> assosiate -> select new subnet

## move weather server from one vnet to other vnet to seuere the app

1. Create one more vnet called weather-vnet  172.16.0.0/16 with subnet (weather-subnet) 

2. Now delete the VM in the bookshopaaprg-vnet , while deleting unselect the os disk and delete remaining componenets like nic and public ip address.

3. Now go to Disks in search bar -> select weathervm disk -> create a VM from os disk -> provide the server name and change the new network and subnet name 

change the ssh nsg rule to access weather ssh from local desktop

4. now catalog and weather vms are placed in differnet vnets, there in no communication between these server, we need to enable vnet peering to establish connection between two apps

5. weather app should not access from internet using 8080, 

6. it should access from catalog server only. create nsg rule 

Source - IP Address
IP address - catalog server ip
Port range - 8080
protocol - TCP
Action - Allow

## Secure VM access

a. JIT (Just in time access) - allowing access (ports) for limited time period
b. Jump Server
c. bastion Server
d. VPN
e. service endpoint
d. private link


## APP service restrictions

Go to Appservice -> Networking -> Access restrictions -> craete a rule

## Setup Application gateway

1. Create a Dedicated Vnet and Subnet for Application gateway  ( different IP address range)

2. Appgatewat tier - Standard v2

3. vent -  cloudlibrary-appgw-vnet -172.17.0.0/16 

4. subnet - appgw-subnet - 172.17.1.0/24

5. Create a public Ip as application gateway frontend

6. Add backend pool
   a. Add inventory backend pool with app service
   b. Add catalog backend pool without catalag VM, we can add later. 
   c. Before adding we need vnet peering between bookshopvnet and bookshop-appgw-vnet

7. Create two routing rules Inventory routing rule and Catalog routing rule
   a. Catalog rule
   -------------------
   ------------------

    Catalog listner rule - 
    ---------------
    protocol - http
    port - 8080

    backend target - 
    -------------
    select backend pool

    http settings -> 
    settings name -catalog settings
    backend protocol - http
    port - 8080  
    leave remaining default values

   b. inventory rule
   -------------------
   ------------------

    Inventory  listner rule - 
    ---------------
    protocol - http
    port - 80


    backend target - 
    -------------
    select backend pool


    http settings -> settings name -inventory  settings

    backend protocol - http

    port - 80

    Override with new hostname - Select yes

    leave remaining default values    

8. Access inventory app service from only through App gateway
   
   a. Create a service endpoint
     
      Go to bookshop-appgw-vnet -> 

      -> click service endpoint

      -> Click add 

      -> Select service as Microsoft.web

      -> Select subnet appgw-subnet

   b. Create access restriction with inventory app service

      ->  Click inventory app service

      -> Networking

      -> Access restrictions 

           -> Add rule

           -> Priority 100

           -> Source settings type - Virtualnetwork -  bookshop-appgw-vnet - appgw-subnet

      now we can access appservice from only application gateway

9. Add catalog VM to application gateway

   Cuurently catalog vm is in boookshopapp-vnet and appgatweay is  bookshop-appgw-vnet.

   Before adding catalogserver to backend pool we need to create vnet peering  boookshopapp-vnet &   bookshop-appgw-vnet

   -> create vnet peering  boookshopapp-vnet &  bookshop-appgw-vnet 

   -> now go to application gateway

   -> Click catalog backend pool

   -> add Catalog vm to Catalog backend pool

10. now let us try validate Catalog vm inbound rule settings

    -> Go to catalog VM 

    -> networking

    -> inbound rules

    -> remove 8080 port access from anywhere

    -> only allow RDP to access my desktop ip address.
              
11. now we can access apps using appgateway

     catalog - appgateway ip:8080

     inventory - appgateway ip:80

## Create Azure SQL Database

1. Go to Azure Portal 

    SQL Databases -> Create new ->

     -> Select resource group - cloudlibrary-rg , location EAST US

     -> Enter DB name

     -> Create SQL user name and password (we need this passowrd later)

     -> Elastic pool -no

     -> selct - Development as workload environment

     -> Select backup strategy -> Local redundant

     -> Connectvity method - no access (we can change later)

     -> Connection policy - Default

     Once completed DB setup

     Go to Database

      -> Set firewall rule -> Public access -> firewall rules -> Add your client IP Address

2. Now open VS Code -> enable sql server extension 

    -> Click sql server extension -> Add connection string

     ->Go to  Sql databse in the Azure Portal, Copy the ADO.net connection string

     -> Replace your password in connection string

     -> paste it in vscode sql connection string

     -> Now we can see azure sql db in the Azure portal

## connect catalog app to azure sql database

1. open catalog code with Visual studio Code

2. Go to appsettings.json file -> Replace connection string with your password

3. now go to startup.cs file , now on wards app should not in memory databse, it should run with sql db

   change useinMemory to False

4. To Interact dotnet catalog code with db install below
#### dotnet tool install --global dotnet-ef

5. to create require statements to run in database , run below command

#### dotnet ef migrations add InitialCreate

 it will create new folder called migrations

6. Create tables in Azure sql databse. run below command

#### dotnet ef database update

7. now we can observe tables in VSCode SQL DB

8. To update the latest changes intiate dotnet build 

9. Now login catalog server, browse catalog webpage to book database

10. now publish the dotnet code 
#### dotnet publish -o publish
  
11. now copy local publish folder to catalog server C:/catalog

12. Try to open the catalog application from web browser
we will recive an error because we didnt allowed to acces catalog server to Azure Sql DB

13. now go to azure sql and open firewall settings
    -> Add firewall rule
    -> enter catalog server public ip address

14. Now we can acess books database in the website without any erross  

# Secure Catalog menu database connection

1. Go to Catalog server vnet catalogvnet
2. CLick service endpoints 
   -> select service - Microsoft.sql
   -> Select catalog VM subnet
   -> Click Add

3. Now go to Azure sql db -> Click firewall rules
   -> Add Virtual network rule
   -> Name - rule name
   -> Vnet -> catalog vnet name
   -> Subnet -> Catalog subnet name

Now we can remove database access from Local desktop and Catalog server

## Connecting Inventory menu code with Azure sql database

1. Open inventory code folder with visual stuido code
  -> Go to pages folder -> index.cshtml.cs -> 
     un comment lines placed under List books section

2. now publish the code using dotnet publish
   
   dotnet publish -o publish 
  
3. Next Go to Publish folder in the Inventory code -> Deploy to webapp  

4. next try to access Inventory app service using web browser.

We will receive an error because we didn't mentioned connection string app service to access SQL database

5. Go to appservice configuration and update the books database connection string

  -> Create new connection string
   
   -> Connection String name: BooksDB    (mandatory name as per code in appsettings.json)
   -> Connection string value: which is copied from  SQL database ADO .net 
   -> Type: SQL Server

6. Now let us try to access inventory app service with browser, still we will recive an error due to the database firewall rules
  -> go to azure sql database
  -> Create firewall rule IP address ( which is reieved in the error message)

7. now we can go to invetory app using browser update/change inventory details like book stock to 0 (out of stok) then observe in the catalog portal  


## Azure Cloud Seurity

1. Azure Service Principal : 
An Azure service principal is a security identity used by user-created apps, services, and automation tools to access specific Azure resources. 

Think of it as a 'user identity' (login and password or certificate) with a specific role, and tightly controlled permissions to access your resources. 

It only needs to be able to do specific things, unlike a general user identity. 

It improves security if you only grant it the minimum permissions level needed to perform its management tasks

2. User Assigned managed identity: 
User-assigned identities can be used by multiple resources, and their life cycles are decoupled from the resources’ life cycles with which they’re associated.

When a user-assigned identity is associated with the four virtual machines, only two role assignments are required, compared to eight with system-assigned identities. 

If the virtual machines' identity requires more role assignments, they'll be granted to all the resources associated with this identity

3. System Assigned : As system-assigned identities are created and deleted along with the resource, role assignments can't be created in advance.

# Integrate key vault with Catalog

## Create a key vault
1. Create a key vault with Resource group bookshopapprg

2. Select access policy as Azure Virtual Machines for deployment

3. Select network as "Public", we can use private endpoint, once we configure

## integrate keyvault info with Catalogmenu code
1. open  Catalogmenu code VSCode

2. replace new program.cs file old program.cs file which is placed in git hub

3. To Enable sync between Azure keyvault and Code, Run this command
   dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault

4. Go to keyvault
   -> add diagnostic settings to audit key vault events

5. Create a secret    
   -> configure the secret as per appsettings.json
      -> Name - ConnectionStrings--BooksDB
      -> value - Copy connection string value from vscode

6. Now remove booksdb connection string from Appsettings.json
   after that add below entry
   "Keyvault": {
    "BaseUrl": "https://kvbookshop101.vault.azure.net/"
  }

here BaseUrl will refer program.cs file

7. Now build the code with 
#### dotnet build

8. now publish the code

#### dotnet publish -o publish

9. copy the local publish folder to server publish folder

10. due to keyvault persmissions , we will get an error

## Enable keyvault access policy

1. first enable identity in the catalov vm
   -> Here we need to capture Principle id

2. Go to Keyvault -
    -> Create a access polocy
    -> select Principle id from previous step

3. now restart IIS server

4. Access the catalog app












