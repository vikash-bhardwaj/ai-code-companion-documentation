# VSCode GPT README

# Visual Studio Extension: One Stop Solution for Engineers and Architects

This Visual Studio extension is designed to enhance the productivity of engineers and architects by leveraging the power of OpenAI APIs. With this extension, users can access OpenAI APIs without worrying about their data privacy. The idea behind this plugin development is to ensure that user prompts are shared directly with the AI provider and no other third-party integration is used to train other models on your code.

## Features

- Ensure to retain the separate context of your chat history per project workspace (This helps engineers to work with separate projects without mixing the chat)
- Ensure to retain user's history context, when token usage is about to reach maximum token length (defined by extension setting `VSCode GPT: Model Max Tokens Length`) for provided model name it trims the messages by following FIFO logic - First in First Out
- API Access token is stored in encrypted form and it's not as part of extension settings
- You can create your own Encryption key to ensure further enhanced security for your access token
  - To provide your Encryption Key, please create a file at root directory of your workspace with name `vscodegpt.config.js` and provide the Encryption Key like below
    ```javascript
    module.exports = {
      encryptionKey: "vscode2gpt112f9dbd8a37fe98421901"
    };
    ```
- Context Menu Commands(refer below first screenshot) to work with working file and option to write custom message/prompt for selected code in the editor (refer below second screenshot)
  
  <img src="./assets/extension-context-menu.png" alt="Extension predefined commands for selected code via Context Menu" width="400" />   <img src="./assets/extension-selection-command.png" alt="Extension Capability to add custom prompt/message for selected code" width="400" />
- Ensure data privacy by sharing user prompts directly with the AI provider. It access OpenAI APIs directly from Visual Studio to get responses for your prompts without any middleware or third party integrations to train other models on your codebase

## Requirements

This extension require a access token to use the OpenAI provider's APIs hence be aware on the usage and cost of the provided access token.

## Extension Settings

### Set-up your AI Provider Access Key for VSCode GPT to communicate to the APIs

To start conversation with VSCode GPT you need to provide your AI Provider Access Key by using below steps:

- Open command palette by pressing `Cmd/Ctrl+Shift+P`
- Search for `VSCode GPT` in the command palette to find all command available for extension
- Look for `VSCode GPT: Set Access Key` and select the command to set the access key
- You will see input box to enter the access key, paste the access key and hit `Return/Enter`. Please refer below screenshots:
  ![Access Key Input screenshot for providing AI Provider Access key](./assets/access-key-input.png)

### This extension contributes the following settings for it's users:
#### Extension's settings screenshot:
![Extension's settings screenshot](./assets/extension-settings.png)

#### Settings usage:
- `VSCode GPT: Api Key`: Default: `OpenAI`: Allows you to select one of the AI Provider from  the predefined list.
- `VSCode GPT: Max Tokens`: Default: `800`: Allow you to change the max tokens to be used for API response.
- `VSCode GPT: Model Max Tokens Length`: Default: `4096`: Allow users to provide the maximum length of tokens allowed for the model in one request, going to be used for logic to trim chat history. Please look for  the maximum tokens allowed for the AI model you are using
- `VSCode GPT: Model Name`: Default: `gpt-3.5-turbo`: Allows you to change Model name used for your AI provider.
- `VSCode GPT: Temperature`: Default: `0.5`: Allows you to change the value for Temperature.
- `VSCode GPT: Top_P`: Default: `0.6`: Allows you to change the value for Top_P.

## Contributing

Currently working on launching a microsite with a data privacy page. In the meantime, if you would like to enquire about project, please feel free to reach out to me at vscodegpt@gmail.com.

## License

This extension is licensed under the [Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/#:~:text=If%20a%20copy%20of%20the,org%2FMPL%2F2.0%2F.).

## Known Issues

Not knows as of now unless I get from you

## Release Notes

### 1.0.0

Initial release of VSCode GPT
**Enjoy!**
