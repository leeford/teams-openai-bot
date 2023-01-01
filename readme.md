# Teams OpenAI Bot

This is a sample example OpenAI bot for Microsoft Teams. It allows a user in Teams to send a chat message to the Bot, which then uses [OpenAI API](https://beta.openai.com/overview) to get an answer to the chat message.

![Example usage](https://user-images.githubusercontent.com/472320/210185524-12d8699c-69af-4ffb-a738-317fea55b5d6.gif)

> Please note: Currently there is no publicly available API for [ChatGPT](https://openai.com/blog/chatgpt/). This sample uses the OpenAI API, which models don't currently include ChatGPT. The OpenAI API is currently in beta, and is not free. You can sign up for a free account [here](https://beta.openai.com/). For a list of supported models, see [here](https://beta.openai.com/docs/api-reference/models).

## Prerequisites

* You need to have an OpenAI account and API key. You can sign up for a free account [here](https://beta.openai.com/)

## Run locally

1. You need to ensure you have the [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) installed first.
2. Create an Ngrok tunnel to your local machine on port 7071 ([download here](https://ngrok.com/download))
    * `ngrok http 7071`
    * Copy the HTTPS URL (e.g. `https://12345678.ngrok.io`. (Note: this may change each time you restart ngrok and you will need to update the Azure Bot Registration with the new URL)
3. [Create a Single Tenant Azure Bot Registration](https://learn.microsoft.com/en-us/azure/bot-service/bot-service-quickstart-registration?view=azure-bot-service-4.0&tabs=singletenant) in the [Azure Portal](https://portal.azure.com)
4. Under `Configuration`, populate the Messaging endpoint with the HTTPS URL with the path of `/api/messages/` from step 2 (e.g. `https://12345678.ngrok.io/api/messages`) and make a note of the `Microsoft App ID` and `Microsoft App Password/Client secret` (you will need these later)
    ![image](https://user-images.githubusercontent.com/472320/210186162-815859ee-4bd1-4c7a-b1ab-d8637e08b179.png)
5. Under Channels, add the Microsoft Teams channel and enable messaging
6. Create a populate a `local.settings.json` file with the following (with your own values):

    ```json
    {
    "IsEncrypted": false,
    "Values": {
        "FUNCTIONS_WORKER_RUNTIME": "node",
        "AzureWebJobsStorage": "",
        "OPENAI_API_KEY": "<YOUR OPENAI API KEY>",
        "OPENAI_MODEL": "text-davinci-003", // Or whatever model you want to use (see https://beta.openai.com/docs/api-reference/models)
        "MicrosoftAppId": "<YOUR MICROSOFT APP REGISTRATION ID>",
        "MicrosoftAppPassword": "<YOUR MICROSOFT APP REGISTRATION CLIENT SECRET>",
        "MicrosoftAppTenantId": "<YOUR MICROSOFT APP REGISTRATION TENANT ID>",
        "MicrosoftAppType": "SingleTenant"
    }
    }
    ```

7. Then, use the following to install, build and run the code (from the root folder):

    ```bash
    npm install
    npm run build
    func host start
    ```

8. You can at this point choose to create a Teams App Manifest and side-load the app into Teams, or you can use the link on the Azure Portal to test the bot in Teams.
    ![Open in Teams](https://user-images.githubusercontent.com/472320/210186363-61638c2b-fd48-461c-a1bf-898421687b1a.png)
