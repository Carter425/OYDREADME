# Azure OpenAI On Your Data

The following is a custom copilot that uses the Azure OpenAI Chat Completions API **Azure OpenAI On Your Data** feature to facilitate RAG (retrieval augmented generation).
You can chat with your data in Azure AI Search, Azure Blob Storage, URL/web address, Azure Cosmos DB for MongoDB vCore, uploaded files, and Elasticsearch.

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

-   [Azure OpenAI On Your Data](#azure-openai-on-your-data)
    -   [Summary](#summary)
    -   [Prerequisites](#prerequisites)
    -   [Setting up your custom copilot in Azure OpenAI Studio](#setting-up-your-custom-copilot-in-azure-openai-studio)
    -   [Setting up and testing your custom copilot in Visual Studio Code](#setting-up-and-testing-your-custom-copilot-in-visual-studio-code)
    -   [Provisioning, Deploying, and Publishing your custom copilot](#provisioning-deploying-and-publishing-your-custom-copilot)
        -   [Assigning Cognitive Service OpenAI User role to your deployed App Service resource](#assigning-cognitive-service-openai-user-role-to-your-deployed-app-service-resource)
    -	[Supplementary Details and Tips](#supplementary-details-and-tips)
    	-   [Enable your Custom Copilot for Group Chats and Channels](#enable-your-custom-copilot-for-group-chats-and-channels)
    	-   [Enable Out of Scope Conversations](#enable-out-of-scope-conversations)
    	-   [Assigning Cognitive Service OpenAI user role to your account for local testing](#assigning-cognitive-service-openai-user-role-to-your-account-for-local-testing)

<!-- /code_chunk_output -->

## Summary
You can deploy to a standalone Teams app (preview) directly from Azure OpenAI Studio, enabling you to bring conversational experience to your users in Teams to improve operational efficiency and democratize access of information. This Teams app is configured to users within a single tenant and personal chat (non-group chat) scenarios. See the [Enable your Custom Copilot for Group Chats and Channels](#enable-your-custom-copilot-for-group-chats-and-channels) section to enable group chat scenarios noting that the AI response quality from Azure OpenAI On Your Data has not been fully tested for group chats. 

This guide will show you have the set up your custom copilot for Teams using Azure OpenAI Studio and Teams Toolkit.

### Prerequisites

| Install                                                                                                                       | For using...                                                                                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Visual Studio Code](https://code.visualstudio.com/download)                                                                  | Typescript build environments. Use the latest version.                                                                                                                                                                                                              |
| [Teams Toolkit](https://marketplace.visualstudio.com/items?itemName=TeamsDevApp.ms-teams-vscode-extension) (5.3.x or greater) | Microsoft Visual Studio Code extension that creates a project scaffolding for your app. Use the latest version.                                                                                                                                                     |
| [Node.js](https://nodejs.org/en) (16 or 18)                                                                                   | Back-end JavaScript runtime environment. Recommended to use Node Version 16.x or 18.x, Node version >=19 is not supported. For more information, see [Node.js version compatibility table for project type](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/build-environments#nodejs-version-compatibility-table-for-project-type)                                                                                                                                          |
| [Microsoft Teams](https://www.microsoft.com/microsoft-teams/download-app)                                                     | Access to a Microsoft Teams account with the appropriate permissions to install an app, [enable custom Teams apps, and turn on custom app uploading.](https://learn.microsoft.com/en-us/microsoftteams/platform/concepts/build-and-test/prepare-your-o365-tenant#enable-custom-teams-apps-and-turn-on-custom-app-uploading)                                                                                                                                             |
| [Microsoft&nbsp;Edge](https://www.microsoft.com/edge) (recommended) or [Google Chrome](https://www.google.com/chrome/)        | A browser with developer tools.                                                                                                                                                                                                                                |                                                                                                                                                                              
| [Azure OpenAI](https://oai.azure.com/portal)                                                                                  | Deploy Azure OpenAI large language models and test your custom copilot ideas in the Azure OpenAI Studio Playground. If you want to host your app or access resources in Microsoft Azure, you must create an Azure OpenAI service.                                                                                                                                                                                                                                 |
| [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)                  | The Azure Command-Line Interface (CLI) is a cross-platform command-line tool to connect to Azure and execute administrative commands on Azure resources. For more information on setting up environment variables, see the [Azure SDK documentation](https://github.com/Azure/azure-sdk-for-go/wiki/Set-up-Your-Environment-for-Authentication). |
| [Cognitive Service OpenAI User](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/role-based-access-control#:~:text=to%20a%20role.-,Cognitive%20Services%20OpenAI%20User,-If%20a%20user) Role                                                                              | Your Azure account has been assigned “Cognitive Service OpenAI user” role of the Azure OpenAI resource you are using to allow you to use your account to make Azure OpenAI inference API calls. For more information see  [Assign Azure roles using the Azure portal](https://learn.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal).                                                                                                                                                                                              |

### Setting up your custom copilot in Azure OpenAI Studio

1. Follow the [use your data quickstart instructions](https://learn.microsoft.com/en-us/azure/ai-services/openai/use-your-data-quickstart?tabs=command-line%2Cpython-new&pivots=programming-language-studio#add-your-data-using-azure-openai-studio) to add your data using Azure OpenAI Studio chat playground.

1. After adding your data, click `Deploy to` and then `A new Teams app(preview)`.

1. Enter the name of your Teams app.

1. Click `download` to download your Teams app as a zip file.

1. Open the location of where you downloaded the zip file and extract the zip file.


### Setting up and testing your custom copilot in Visual Studio Code

Note: Testing this sample requires that you are logged into Azure CLI and you have Cognitive Services OpenAI User role assigned to you per the pre-requisites.

1. Go to Visual Studio Code.
   
1. Select `File > Open Folder`.
   
1. Go to the location where you extracted your Teams app folder and select it.

1. If you chose `API key` in data connection, manually copy and paste your Azure AI Search key in `src\prompts\chat\config.json` file. Your **Azure AI Search Key** can be found in Azure OpenAI Studio Playground by clicking the `View code` button and looking under **Azure Search Resource Key**. If you chose `system assigned managed identity`, you can skip this step. Learn more about different data connection options in the [Data connection section](https://review.learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data?tabs=ai-search%2Ccopilot&branch=pr-en-us-277392#data-connection) .
   
1. Open the Visual Studio terminal by selecting `View > Terminal` and log into Azure CLI selecting the Azure account that you assigned Cognitive Service OpenAI User role to. Use the following command to log in:
   ```bash
     az login
   ```
1. From the left pane, select Teams Toolkit extension.
   
1. Under ACCOUNTS, sign in to the following:
   - Microsoft 365 account with permissions to upload custom apps
     
1. To debug your app, press the `F5` key or from the left pane, select `RUN AND DEBUG ▷` (Ctrl+Shift+D) and then select `Debug in (Edge)` from the dropdown list.  Select `Run > Start Debugging` (F5). IMPORTANT: `Debug in Test Tool` does not work with this feature at this time.

1. A browser tab opens a Teams web client requesting to add the bot to your tenant. Select Add to begin a chat with your custom copilot

> If you do not have permission to upload custom apps (sideloading), Teams Toolkit will recommend creating and using a Microsoft 365 Developer Program account - a free program to get your own dev environment sandbox that includes Teams.   

### Provisioning, Deploying, and Publishing your custom Copilot
After you have tested it locally, you can provision, deploy and publish your Teams app using Teams Toolkit. 

**IMPORTANT** As this sample uses managed identity, for your custom copilot to generate responses you must assign Cognitive Service OpenAI User role to your custom copilot’s App Service resource group after deploying your app to Azure.

1. [Provision your app](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/provision)
1. [Deploy to Azure](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/deploy)
1. [Publish to Teams](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/publish)

### Assigning Cognitive Service OpenAI User role to your deployed App Service resource
As this sample uses managed identity, you must enable assign Cognitive Service OpenAI User role to your custom copilot’s App Service resource group after deploying your app to Azure in order for your deployed custom copilot to receive responses from Azure OpenAI.

1. Go to [Azure portal](https://portal.azure.com) and select resource groups
   
1. Select the resource group you deployed your custom copilot to
   
1. Select the `App Service` resource
   
1. Go to `settings > identity` and enable system assigned identity by selecting `On`
   
1. Select `Save` to enable system assigned identity.
   
1. Click `Azure role assignments`
   
1. Click `add role assignments`.

1. Under Scope select `Resource group`
   
1. Under Subscription select the Azure subscription of your Azure OpenAI resource
   
1. Under Resource group select your Azure OpenAI resource
   
1. Under Role select `Cognitive Service OpenAI user`
   
1. Select `save` to finish assigning the role

Now, your published custom copilot Teams app is ready for use.

## Supplementary Details and Tips

### Enable your Custom Copilot for Group Chats and Channels

This custom copilot sample is pre-configured for only personal chats (1 on 1) during preview due to ongoing testing from Azure OpenAI On Your Data to determine the effects of group chats on AI response quality. Group chats can be enabled with the understanding that the AI response quality has not been fully tested for these scenarios. 

A custom copilot can be mentioned ("@customcopilotname") in a channel if it has been added to the team. Note that additional replies to a custom copilot in a channel require @ mentioning the custom copilot. It will not respond to replies where it isn't mentioned. See here for more information. 

To enable group chats:

1. Go to appPackage\manifest.json file
   
1. Add the **team** and **groupchat** parameters to your bots' scopes in additional to the existing **personal** scope.
   ```bash
    "bots": [
	        {
	            "botId": "${{BOT_ID}}",
	            "scopes": [
	                "personal",
	                "team",
	                "groupchat"
	            ],

   ```
   
### Enable Out of Scope Conversations

You can modify the settings in the **Data parameters** section in src\prompts\chat\config.json file. 
The **in_scope** parameter configures the chatbot's approach to handling queries unrelated to the data source or when search documents are insufficient for a complete answer. When this setting is set to `false`, the model supplements its responses with its own knowledge in addition to your documents. 

By default, the **in_scope** parameter is set to `true` resulting in the model attempting to only rely on your documents for responses. Out of scope questions may receive the following response:

“The requested information is not available in the retrieved data. Please try another query or topic.”

For more information please see [Runtime parameters](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/use-your-data?tabs=ai-search#runtime-parameters)

### Assigning Cognitive Service OpenAI user role to your account for local testing

Testing this sample requires that you are logged into Azure CLI and you have Cognitive Services OpenAI User role assigned to you per the prerequisites. Detailed instructions are below on how to assign the “Cognitive Service OpenAI user” role to your account.

To assign yourself the Cognitive Services OpenAI User role in Azure, follow these steps:

1. **Sign in to the [Azure portal](https://portal.azure.com):** Go to the Azure portal and sign into your Azure account.
1. **Navigate to Subscriptions:** Navigate to `Subscriptions`. If you don't see it, you can search for `Subscriptions` in the search bar at the top of the portal.
1. **Select Your Subscription:** Click on the subscription you want to assign the role to.
1. **Access IAM (Identity and Access Management):** In the subscription menu, select `Access control (IAM)`.
1. **Add Role Assignment:** Click the `+ Add` button and then select `Add role assignment`.
1. **Choose Role:** In the `Role` dropdown, search for and select `Cognitive Services OpenAI User` and click `Next`
1. **Assign to User:** In the `Assign access to` dropdown, select `User, group, or service principal`
1. **Select Your Account:** Click `+ Select members` , search for your account name or email and select it.
1. **Review and Assign:** Click `Review + assign` to complete the role assignment.
1. **Wait for Role Assignment to Propagate:** It can take up to 5 minutes for the changes to take effect.

See [Cognitive Service OpenAI User](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/role-based-access-control) and [Assign Azure roles using the Azure portal](https://learn.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal) for additional details.


