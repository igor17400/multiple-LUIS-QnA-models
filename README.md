# Multiple LUIS and QnA models
This repository consists in a chatbot code for using several LUIS and QnA models from microsoft chatbot framework. 

I will be using 14.nlp-with-dispatch Microsoft's sample (https://github.com/Microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore/14.nlp-with-dispatch). Therefore, the first step is to clone this code so we can use it as our guide through this document.

## Deploying the chatbot

To use our chatbot we need to deploy it to azure. 

Acess this link for futher reading about deploying chatbots to azure (https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-deploy-az-cli?view=azure-bot-service-4.0&tabs=csharp). The following steps are just a synthezised version of this tutorial. 

1) Login to Azure and set subscription

```
az login
az account set --subscription "<azure-subscription>"
```

2) Execute the command below to create a new app registration into your Azure account

```
az ad app create --display-name "displayName" --password "AtLeastSixteenCharacters_0" --available-to-other-tenants
```

Command with parameters filled

```
    az ad app create --display-name "mult-registrations-app" --password "password_default" --available-to-other-tenants
```

3) Execute the command below to Deploy via ARM template (with new Resource Group)

```
az deployment create --template-file "<path-to-template-with-new-rg.json" --location <region-location-name> --parameters appId="<app-id-from-previous-step>" appSecret="<password-from-previous-step>" botId="<id or bot-app-service-name>" botSku=F0 newAppServicePlanName="<new-service-plan-name>" newWebAppName="<bot-app-service-name>" groupName="<new-group-name>" groupLocation="<region-location-name>" newAppServicePlanLocation="<region-location-name>" --name "<bot-app-service-name>"

```

The code below is the same as the above, however I have filled the required parameters. 

Remember! The majority of parameters are different, so pay atention to the values of yours'. This is just a example to how those parameters should be filled.    

```
az deployment create --template-file ".\DeploymentTemplates\template-with-new-rg.json" --location westus --parameters appId="519dfd8c-8d93-43d3-a3e8-ccbbd2d346d9" appSecret="password_default" botId="botMulti" botSku=F0 newAppServicePlanName="botMulti-service-plan" newWebAppName="botMulti-app-service" groupName="groupForMultiBot" groupLocation="westus" newAppServicePlanLocation="westus" --name "botMulti"

```

<img src="images\image1.PNG"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" 
     heigh="100"
     width="700"/>

Afte this command you should see somehting similar to the image abovebelow into your azure resource group you just created.

4) Deploy your code to deployment

You can do this using your visual studio. Click with your right buttom on your project name in visual studio. CLick "publish"-->"select existing"-->"botMulti-app-service"

If you test this chatbot on WebChat nothing will happen because we haven't configure appSettings.json yet.


## Creating Luis and QnA resources

1) Search for the resource group created earlier

2) Click on Add(+)

3) Search for Language Understating --> Create 

    - Name: luis-multi
    - Authoring location: West-US
    - Authoring pricing tier: F0
    - Prediction location: West US
    - Prediction pricing tier: S0
    - Click on "Review + create"
    - Click on "Create"

4) Click on Add(+)

5) Search for QnA Maker --> Create

    - Name: QnAMulti
    - Pricing Tier: F0
    - Resource Group: groupForMultiBot
    - Azure Search Princing Tier: F(3 indexes)
    - Azure Search Location: West Us
    - App Name: QnAMulti
    - Website location: West Us
    - App insights location: West Us
    - Create


## Create Luis App

1) Sing in to the portal --> https://www.luis.ai/
2) 