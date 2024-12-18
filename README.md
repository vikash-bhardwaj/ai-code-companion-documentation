# `AI Code Companion` - Visual Studio Code Extension: One Stop Solution for Engineers and Architects

This Visual Studio extension is designed to enhance the productivity of engineers and architects by leveraging the power of OpenAI APIs. With this extension, users can access OpenAI APIs without worrying about their data privacy. The idea behind this plugin development is to ensure that user prompts are shared directly with the AI provider and no other third-party integration is used to train other models on your code.

## Usage

### Installation and Set-up

- Install the extension either by visiting Visual Studio Code [marketplace page](https://marketplace.visualstudio.com/items?itemName=vikash-bhardwaj.aicodecompanion) or search the extension in the "Extensions" activity bar tab with name "AI Code Companion" by vikash-bhardwaj
- Post installation please refer to the below [set-up section](#setup)
- <strong style="color: red">Important!!</strong> - Post setting up the accesskey for your selcted AI provider please ensure you also check the model name in the settings. If you are using `PSChat` as your AI Data Provider then please change the model name because default model name will not work as is with PSChat. While you can check the respective AI Provider docs for all model names provided by your AI Provider, you can use `gpt-3.5-turbo` or `gpt-3.5-turbo-16k` for OpenAI & `gpt35turbo` or `gpt4` For PSChat.
- <strong style="color: red">**Note for local `Ollama` Models integration to work without Internet**</strong> - Please download and install `Ollama` and start it to work with different `Ollama` Models.
  - Download the installer and install `Ollala` from <a href="https://ollama.com/download">Download Ollama Page</a>
  - Follow the docs to download the model of your choice by following the - <a href="https://github.com/ollama/ollama">Documentation page</a>
  - Once you download the model, open extension settings and change the `API Key` dropdown value to `Ollama Chat` and update `Model Name` field with downloaded model like "llama3.2:latest" or "Mistral" or "llava", etc. (Please change the settings in "Workspace" Tab in case that exist as "Users" Tab setting wont work if Workspace Tab exist).
  - In case you have higher configuration machine to run local models then you can also play around changing the `Model Max Token Length` to higher number like `64000` for retaining the longer chat context in history.
- <strong style="color: red">**Note for `Azure Open AI` Set-up**</strong> - Extension has ability to connect to Azure OpenAI API through `API Key Authentication` and `Microsoft Entra Authentication` method. Please refer [Azure OpenAI Service REST API reference](https://learn.microsoft.com/en-us/azure/ai-services/openai/reference) for more information on authenticating for Azure Open AI.

  If you choose to use Azure OpenAI as your service provider, follow these steps to set up the API Endpoint for extension before referring to the below [set-up section](#setup):

  1. Create a configuration file with name `.aicodecompanion.config.js` at the `root` of your project with below configuration. This file allow you to provide the `Azure Open AI` API URL, which is a unique URL for your account. In case of other AI Providers you don't need to provide this object unless you wish you change the API endpoint for other providers too.

  ```javascript
  // Example configuration for providing the API Endpoint:
  module.exports = {
    apiProvider: {
      AzureOpenAI: {
        endPointUrl:
          "https://username-azure-open-ai.openai.azure.com/openai/deployments/azure-open-ai-model/chat/completions?api-version=2023-07-01-preview",
      },
      // Below is reference for OpenAI and PSChat Providers in case you want to change the URL for these Options. It will work only if Request and Response Contracts are same for your endpoints
      OpenAI: {
        endPointUrl: "Open AI URL",
      },
      PSChat: {
        endPointUrl: "PSChat URL",
      },
    },
  };
  ```

  2. Once you have created this configuration file, please reload the project workspace for extension to purge the cache and use the latest config created by you.

- For more details on how to set-up and use `AI Code Companion` and understand few usee cases please watch the Video: <br />
  <a href="https://www.youtube.com/watch?v=wmkSrL484V0" target="_blank"><img src="./assets-readme/youtube-video-thumbnail.jpg" alt="AI Code Companion: Boosting Productivity with OpenAI APIs in VS Code | Usage Guide and Tutoria" width="70%" /></a>

### Usage Screenshots

<img src="./assets-readme/extension-splash-screen.png" alt="Extension splash screen screenshot" width="24%" /> <img src="./assets-readme/extension-loading-state.png" alt="Extension animated loading state for API progress" width="24%" /> <img src="./assets-readme/extension-initial-question.png" alt="Extension usage screenshot" width="24%" /> <img src="./assets-readme/extension-code-question.png" alt="Extension usage screenshot" width="24%" />

---

## Features

- Ensure to retain the separate context of your chat history per project workspace (This helps users to work with separate projects without mixing the chat context)
- Ensure auto trimming of user's history context to have seamless experience, when token usage for the API is about to reach maximum token length (defined by extension setting `AI Code Companion: Model Max Tokens Length`) for provided model name it trims the messages by following FIFO logic - First in First Out
- Provide capability to login with Atlassian JIRA and fetch the JIRA ticket details, update tickets for basic fields updates and also create JIRA tickets from VS Code itself. Enable you to simply refer the story id and ask to write code, for instance "fetch the details for JIRA story ACC-122 and write ReactJS Components". (Doesn't work with Streaming so might feel little slow compared with other responses, team is working on it).
  - Please type double slash anywhere in text input to get the commands for JIRA and select it or type "//jira or //JIRA" as this will be used as identifier for `AI Code Companion` to work with JIRA. See below screenshot as an example:
    <br /><img src="./assets-readme/jira-widget-before-login.jpg" alt="JIRA Widget before Login" width="47%" /> <img src="./assets-readme/jira-widget-post-login.jpg" alt="JIRA Widget after Login" width="47%" />
    <br /><img src="./assets-readme/jira-sample-prompt.jpg" alt="JIRA Sample Prompt with Response" width="47%" />
- Provide capability to have multiple Tabs to work with ease, having separate context for each Tab.
- Generate any type of diagrams from prompt or from the provided code, Simply select the code from files and ask it to create flow diagrams and save them for your Solution Design docs.
  <br /><img src="./assets-readme/diagrams-sample-prompt.jpg" alt="Sample Prompt for Diagram generation" width="47%" /> <img src="./assets-readme/diagrams-sequence.jpg" alt="Sample Sequence Diagram for prompt" width="47%" />
  <br /><img src="./assets-readme/diagrams-flow.jpg" alt="Sample Flow Diagram for prompt" width="47%" />
- Provide capability to upload Images or SVG to write code, just upload the Figma Images and get your Components created with all required dependencies including CSS Styles. You can also use this feature to write code or explain the provided diagrams.
- Auto validation of complete code file to identify potential Run Time errors available in code with details to fix them. You can disable this feature from settings and manually run the command `Validate Code for Potential Runtime Errors` to do the validation for a file.
- Very easy to add Context from multiple files to your prompt, please refer below screenshots to see how you can add files/Folders and Specific methods/code blocks as context to speed up the development and get quality code generation based on your context:
  - Use `@` keyword as trigger sequence in Chat Input Box to attach different type of context like opened files from editor, files from Workspace, Code Blocks, Images.
  - Use `/` keyword as trigger sequence in Chat Input Box to open the files drawer for adding them as context from any of the opened files in editor to your prompt. You can filter the list with typing the file name with slash keyword.
  - Use `#` keyword as trigger sequence in Chat Input Box to open the Workspace drawer for adding files as context to your prompt.
  - Select the Code and say `Add Code as Extra Context to Prompt` with right click context menu, refer the first screenshot "add-code-block-as-context".
  - Right click on file/folder in explorer or editor and say `Add File as Extra Context to Prompt`, refer the second & third screenshots "add-code-file-as-context" and "add-code-file-as-context-from-editor".
  - Simply open a Complete File explorer and attach either code blocks or files as context, refer the fourth screenshot "add-code-block-files-as-context-from-explorer".
    <br /><img src="./assets-readme/add-code-block-as-context.jpg" alt="Add selected code block as Context" width="47%" /> <img src="./assets-readme/add-code-file-as-context.jpg" alt="Add file as context from file explorer" width="47%" />
    <br /><img src="./assets-readme/extension-at-the-rate-drawer.jpg" alt="At the rate Context Drawer" width="47%" /> <img src="./assets-readme/extension-attach-files-with-files-drawer.jpg" alt="Attach file Drawer with slash key" width="47%" />
    <br /><img src="./assets-readme/extension-attach-files-with-workspace-drawer.jpg" alt="Attach files from workspace drawer with hash key" width="47%" /> <img src="./assets-readme/add-code-file-as-context-from-editor.jpg" alt="Add file as context from editor" width="47%" />
    <br /><img src="./assets-readme/add-code-block-files-as-context-from-explorer.jpg" alt="Add file as context from External Companion Explorer" width="47%" />
- `Automated Code Reviews` for your `GIT` changes, with just one click of a button you can now review the changes in your GIT repository. `AI Code Companion` will go through all of your GIT changes(modified and added files) and provide you comments. You can also provide your custom prompt message while running the code review action. You can play around this by running it multiple times to get different perspectives and improve code quality.
  - You can use this feature by two ways:
    - Either with help of Button `Review GIT Changes and create notes for your PR` provided in the Extension Interface or by Running the Command for same from command palatte(refer below screenshots)
      <br /><img src="./assets-readme/extension-code-review.jpg" alt="Automated review for GIT Changes with AI Code Companion" width="47%" /> <img src="./assets-readme/extension-code-review-command.jpg" alt="Command screenshot for Automated review for GIT Changes with AI Code Companion" width="47%" />
    - You can also get code review done at individual file level either by using context Menu option from editor or by opening the context Menu from file name in explorer bar. Please refer below screenshots for your reference:
      <br /><img src="./assets-readme/extension-code-review-context-menu.jpg" alt="Automated review for GIT Changes with AI Code Companion from Context Menu" width="47%" /> <img src="./assets-readme/extension-code-review-context-menu-file.jpg" alt="Command screenshot for Automated review for GIT Changes with AI Code Companion from File Context Menu" width="47%" />
- <a name="component-generator"></a>`UI Component Generator`:
  - Enhance your development workflow with `AI Code Companion`, a VS Code extension designed to elevate the quality of your UI components. Despite the advancements of Gen AI, it still requires detailed prompts to produce code that adheres to Non-Functional Requirements (NFRs) and best practices. Our tool fills this gap by offering a curated library of UI components that span various frameworks and styling techniques, with a strong emphasis on best practices such as accessibility and performance.
  - Our component library is dynamic, with regular updates to refine existing components and introduce new ones, ensuring that you stay at the forefront of development standards. If you're passionate about code quality and have insights on crafting prompts that yield components meeting high standards of accessibility and performance, we welcome your contributions. Join us in our mission to make AI-generated code not just functional, but exemplary. Reach out to us, and let's collaborate to enhance the AI Code Companion library together.
  - How to use this feature:
    - Open the Component Generator either by opening it from menu(Click on three dots next to Settings icon in the Extension Title bar) or by running the command `AI Code Companion: Generate UI Components`. For more information, refer to the screenshot below, and watch the video provided above on how to use the extension:
      <br /><img src="./assets-readme/extension-generate-ui-components.jpg" alt="Automated component generation with Gen AI" width="90%" />
- Provide an easy approach to see the differences in the existing and code generated by Companion.
- Multiple options to interact with AI provider for asking questions and increase productivity:

  - Context Menu Commands for quick access to common tasks like refactor code, find issues, explain and document code etc. Select the code in file and right click to access these commands. (refer below first screenshot).
  - Flexibility to write custom prompts/queries to ask AI Provider and same can be done to add more context for selected code in the editor. No need to switch to other windows as extension provides interactive approach to provide complex requirements for your code in the editor itself, just select the code and ask AI provider to achieve complex tasks for selected code like writing test cases, understanding the code, refactoring & optimizing the code and this call gets further improved with retained context in the chat history. (refer below second screenshot)
  - Flexibility to ask questions in form of inline code comments from editor:

    - You can use single line comments or multi line comments to provide prompts/queries. Please use keyboard shortcut `Ctrl+Alt/Option+Enter/Return` from any line in the comment to executing the Inline prompts with `AI Code Companion`.
    - To keep the easy access to history for responses for inline prompts, extension will add the responses to the chat window if it's in focus, if chat window is not in focus then the responses will be generated in new file.
    - Please note that Inline prompts are not maintained in AI Provider chat history and only maintained in chat window. Each inline prompt will be treated as new prompt to AI provider, this is to allow bigger prompts and leave space for maximum tokens to be used in responses.
    - Intutive approach to check the progress for Inline Prompts execution, you can check the status of API either with help of inline icon (&#8987;) or look for API progress in status bar. (refer third and fourth screenshot for refernce)

    <img src="./assets-readme/extension-context-menu.png" alt="Extension predefined commands for selected code via Context Menu" width="33%" /> <img src="./assets-readme/extension-selection-command.png" alt="Extension Capability to add custom prompt/message for selected code" width="33%" /> <img src="./assets-readme/extension-inline-comments-api-progress.jpg" alt="Extension Capability show progress for Inline Comments API Calls" width="66%" />

- <a name="unit-test"></a>`AI Code Companion` has become smarter to write test cases for selected code using multiple approaches, users can write test cases for code with following options:

  - Users can select the code in editor and write Prompt in the available Input Textbox to write test cases
  - Users can also use Context Menu options to write test cases and extension gives full freedom to users for configuring the libraries to write test cases. See below options available for users when it comes to writing test cases using context menu commands:
    <br /> <img src="./assets-readme/extension-testing-libraries-menu.jpg" alt="AI Code Companion - Testing libraries options pick screenshot" width="48%" /> <br />

    1. Create a file at root of your project with name `.aicodecompanion.config.js` to define testing libraries for the project workspace for every programming language like below. You can also define your own custom prompt in case you dont like the prompt generated by the companion for you, see as an example for unitTests under javascript:

    ```javascript
      module.exports = {
        testingLibraries: {
          javascript: { // this should match with language ID which is provided by VS Code Editor for the code snippet/file
            unitTests: {
              testingLibrary: "jest",
              additionalLibraries: ["react-testing-library"], // Optional
              promptMessage: "Write unit test cases for the code using RTL" // Optional: Only provide this if the prompt provided by extension is not working well for you
            },
            endToEndTests: {
              testingLibrary: "cypress",
              additionalLibraries: [
                "cypress-react-unit-test",
                "cypress-testing-library",
              ]
            },
          },
          python: {
            unitTests: {
              testingLibrary: "pytest",
              additionalLibraries: ["pytest-cov"],
            }
          }
          java: {
            unitTests: {
              testingLibrary: "junit"
            }
          }
        },
      };
    ```

    2. In case you don't provide the above config and still use the context menu then `AI Code Companion` is smart enough to recommend you the library names to pick from popular libraries for the given programming language. You will see below UI to pick the libraries and once you select the libraries it will remember the choices for future. You can reset the option by executing the command `Reset Testing Libraries Options for Unit and E2E Tests` using `cmd/ctr+shift+p` (refer second screenshot).
       <br /> <img src="./assets-readme/extension-testing-libraries-options.jpg" alt="AI Code Companion - Testing libraries options pick screenshot" width="48%" /> <img src="./assets-readme/extension-remove-testing-libraries-options.jpg" alt="AI Code Companion - remove testing libraries options command screenshot" width="48%" /> <br />

- Flexibility to provide different model names available with your AI Provider and other supported parameters by the AI Provider.
- Provides intutive buttons with every codeblock in the AI responses for easily copying the codeblocks, creating new file with codeblocks or insert codeblocks at cursor position/selected code (refer below screenshot).
  <br /><img src="./assets-readme/extension-codeblock-buttons.jpg" alt="Extension screenshot to highlight copy code, create file and insert code buttons" width="40%" />
- Users can Cancel the ongoing API request by multiple ways:
  - With help of `Cancel` button for chat prompts
  - With help of `Cancel Request` button from progress bar in the Statusbar
  - With help of `Abort Prompt API Request` command from `Command Pallete` (this will cancel both - any request created by chat prompts or request created by inline comments) (refer below screenshot)
    <br /><img src="./assets-readme/extension-cancel-api-requests.jpg" alt="Extension screenshot to highlight cancel API request options" width="50%" />
- API Access token is stored in encrypted form and it's not as part of extension settings
- If needed you can create your own Encryption key to ensure further enhanced security for your access token

  - To provide your Encryption Key, please create a file at root directory of your workspace with name `.aicodecompanion.config.js` and provide the Encryption Key like below
    ```javascript
    module.exports = {
      encryptionKey: "vscode2gpt112f9dbd8a37fe98421901",
    };
    ```

- Ensure data privacy by sharing user prompts directly with the AI provider. It access OpenAI APIs directly from Visual Studio to get responses for your prompts without any middleware or third party integrations to train other models on your codebase
- Provides easy approach to clear chat history (please note this will delete messages from chat along with maintained chat history for previous context)
- Allows you to Enable/Disable Extension logs with a simple extension setting for easy API debugging (We ensured that the Access token is not printed as part of logs). Please look for setting `Enable Logs` under Extension Settings.
- Provides easy approach to repeat the prompt from chat history and in case of API failure

## Requirements

This extension require a access token to use the OpenAI provider's APIs hence be aware on the usage and cost of the provided access token.

### <a name="get-your-key"></a>Where to get the access token/key?

- if you are using OpenAI Platform [platform.openai.com](https://platform.openai.com) then after login to the platform with your credentials visit to Manage Account section and look for "User" section and click on "[API Keys](https://platform.openai.com/account/api-keys)". Generate your new Access Token/Key for using with the extension.
- if you are using PSChat Platform then after login to the platform with your credentials visit to "Personal Access Tokens" section under developer section. Generate your new Access Token/Key for using with the extension.

## <a name="setup"></a>Set up your AI Provider Access Key for AI Code Companion to Communicate with AI Provider's API

To start conversation with AI Code Companion you need to provide your AI Provider Access Key by using below steps, please refer to [above section](#get-your-key) to find steps to get Access Key:

- Check if you have right AI provider selected in the extension settings, look for setting `AI Code Companion: Api Key` dropdown.
- If you have multiple AI provider and you want to use separate AI provider for different projects then please change the AI provider in the `Workspace` settings. Workspace settings will overwrite the user's settings.
- Open command palette by pressing `Cmd/Ctrl+Shift+P` from Visual Studio Code
- Search for `AI Code Companion` in the command palette to find all commands available for extension
- Look for `AI Code Companion: Set Access Key` and select the command to set the access key. Please refer below screenshot:
  <img src="./assets-readme/command-set-access-key.png" alt="Extension Set Access Key Command" />
- You will see input box to enter the access key, paste the access key and hit `Return/Enter`. Please refer below screenshots:
  Screenshot before entering access key:
  <img src="./assets-readme/access-key-input.png" alt="Extension Set Access Key Inputbox" />

  Screenshot post entering access key:
  <img src="./assets-readme/command-access-key-input.png" alt="Extension Set Access Key Inputbox with filled value" />

- If you want to remove your Access Key, then you can execute the command `AI Code Companion: Remove Access Key`. Please refer to below screenshot:
  <img src="./assets-readme/command-remove-access-key.png" alt="Extension Remove Access Key" />
- Please note that model names are different between OpenAI and PSChat AI Providers so please validate you have correct model name for selcted AI Provider

## Extension Settings

### This extension contributes the following settings:

#### Extension's settings screenshot:

![Extension's settings screenshot](./assets-readme/extension-settings.jpg)

#### Settings usage:

- `AI Code Companion: Api Key`: Default: `OpenAI`: Allows you to select one of the AI Provider from the predefined list.
- `AI Code Companion: Enable APIStreaming`: Default: `True`: Allows you to get fastest responses with Streaming when it is supported by API Provider(Currently Supported for Open AI).
- `AI Code Companion: Enable Atlassian Jira Confluence Drawer`: Default: `True`: Allows you to login with your Atlassian JIRA Cloud Login and work directly with JIRA from `AI Code Companion`, like to fetch JIRA Story and write code for that.
- `AI Code Companion: Enable Auto File Validation For Run Time Errors`: Default: `False`: Allows users to validate any file for potential Run Time Errors like Null Checks missing and provide report with code references. If this is enabled then any file you open for first time will be automatically validated. You can also run this with VS Code Command `Validate Code for Potential Runtime Errors`.
- `AI Code Companion: Max Tokens`: Default: `800`: Allow you to change the max tokens to be used for API response.
- `AI Code Companion: Model Max Tokens Length`: Default: `4096`: Allow users to provide the maximum length of tokens allowed for the model in one request, going to be used for logic to trim chat history. Please look for the maximum tokens allowed for the AI model you are using
- `AI Code Companion: Model Name`: Default: `gpt-3.5-turbo`: Allows you to change Model name used for your AI provider.
- `AI Code Companion: Temperature`: Default: `0.5`: Allows you to change the value for Temperature.
- `AI Code Companion: Top_P`: Default: `0.6`: Allows you to change the value for Top_P.
- `AI Code Companion: Enable Logs`: Default: `false`: Allows you to enable/disable logs for debugging
- `AI Code Companion: Enable User Prompt For Code Review`: Default: `true`: Allows you to enable/disable Input Prompt to take Prompt Message for additional code review context
- `AI Code Companion: User Name`: Default: `You`: Allows user to have a personal touch by showing your own used name in Chat.

## :sparkles: A Note to Contributors :sparkles:

A heartfelt thanks to these champs who have helped us enhance our project, either through their valuable pull requests or by identifying defects. Your contributions are greatly appreciated!

### Our Valued Contributors

- <img src="https://images.weserv.nl/?url=github.com/manishekhawat.png?v=4&h=300&w=300&fit=cover&mask=circle&maxage=7d" width="50" height="50" alt="manish Shekhawat" align="middle" /> &nbsp;[Manish Shekhawat](https://github.com/manishekhawat)

  - Implemented splash screen for the extension.

- <img src="https://images.weserv.nl/?url=github.com/engamankumar.png?v=4&h=300&w=300&fit=cover&mask=circle&maxage=7d" width="50" height="50" alt="Aman Kumar" align="middle" /> &nbsp;[Aman Kumar](https://github.com/engamankumar)

  - Theming for the Chat Interface, Defect callouts in features.
  - Added capability to show Difference in existing and generated code by Companion.

- <img src="https://images.weserv.nl/?url=github.com/shreysudan.png?v=4&h=300&w=300&fit=cover&mask=circle&maxage=7d" width="50" height="50" alt="Soumya Dandapat" align="middle" /> &nbsp;[Shrey Sudan](https://github.com/shreysudan)

  - Added Multi Tabs Capability

- <img src="https://images.weserv.nl/?url=github.com/surensubhu.png?v=4&h=300&w=300&fit=cover&mask=circle&maxage=7d" width="50" height="50" alt="Surender Natarajan" align="middle" /> &nbsp;[Surender Natarajan](https://github.com/surensubhu)
  - Integrated Azure Open AI endpoint as one of the AI Provider to work with `AI Code Companion`.

## License

This extension is licensed under the [Mozilla Public License 2.0](https://www.mozilla.org/en-US/MPL/2.0/#:~:text=If%20a%20copy%20of%20the,org%2FMPL%2F2.0%2F.).

## Privacy Policy

[Link to Privacy Policy](https://vikash-bhardwaj.github.io/ai-code-companion-documentation/privacy-policy)

This extension collects certain data for the purpose of interacting with APIs provided by AI Provider selected by user, default AI provider is OpenAI. We are committed to protecting your privacy and handling your data securely. Please review our privacy policy for more information on how we collect, use, and protect your data. if you would like to enquire about project, please feel free to reach out to us at aicodecompanion@gmail.com.

## Known Issues

- Currently if an Inline Comment execution is in progress then another inline comment will not work at same time
- If Inline comment execution is in progress then Code Review for GIT changes will not work or vice versa

## <a name="release-notes"></a>Release Notes

### [2.1.0]

#### New Features

- Added Capability to work with local `Ollama` models which allow users to work with AI without internet connection. Please look at the above installation section for more details regarding setting up Ollama and running the same in `AI Code Companion`.
- Added Capability to make the attach file as context much more easy with `@` keyword from Chat Input Box. With this feature users can simply type `@` to open all options to attach different type of context. Users can also use `/` to open the Opened files drawer in editor to attach them as context and similarly use `#` to open Workspace Drawer to add any file as context from workspace.
- Fixed a defect where Attach Code as Context window was not working.
- Redesigned the attach context Button.

### [2.0.0]

#### New Features

This is a major release with complete redesign and many interesting features that makes `Ai Code Companion` more powerful.

- Added capability to Stream the responses with OpenAI that makes it very fast for generating the prompt responses.
- Added capability to login with Atlassian JIRA and fetch the JIRA ticket details, update tickets for basic fields updates and also create JIRA tickets from VS Code itself. Enable you to simply refer the story id and ask to write code, for instance "fetch the details for JIRA story ACC-122 and write ReactJS Components". (Doesn't work with Streaming so might feel little slow compared with other responses, team is working on it). For Screenshots, please refer above Features section.
- Added capability to add multiple Chat Tabs to work on parallel on multiple threads
- Added capability to generate any type of diagrams like sequence, flow, architecture, mind-maps etc with simple prompts and code files. Refer to Features section for few usage snapshots.
- Added capability to write code from Image, simply upload your Design Figma images or other type of diagrams and get your components in seconds. Have been testing this feature and best one to create UI components using few prompts :) - You can also upload diagrams to get the explanation or write code.
- Added capability to add validate your code files for potential Run Time Errors, please refer features sections for more details. You can run the VS Code command `Validate Code for Potential Runtime Errors` to validate code files.
- Added capability to context from multiple files with very easy approaches, please refer the features section for more details.
- Added Capability to Compare the Code generated by AI with what you provided, compare and merge code with ease from existing files.
- Redesigned the Chat UI.

### [1.9.0]

#### New Features

- Added API integration with Azure Open AI, please refer to above set-up section for working with Azure Open AI endpoint.

### [1.8.0]

#### New Features

- Added the capability to generate UI Components with the help of a wizard that ensures adherence to Accessibility and Performance best practices. Please refer to the above Features section for the [UI Component Generator](#component-generator) for more information.

#### Theming and Defect Fixes in Existing Features

- Major theming changes for the Chat Interface.
- Fixed multiple defects to improve existing features:
  - Single File code review was not working with the context menu for Windows OS.
  - Code review was not functioning for staged files.
  - Other code review issues, such as the right-click code review action not working for folders or file names containing spaces, etc.
  - Multiple repeat prompts were appearing in some edge cases.
  - Scroll position was not being maintained in some edge cases.
  - Some other minor defects.

### [1.7.0]

#### New Features

- Added capability to code review the GIT Changes at individual file level with context menu options from editor as well as from file name in file Explorer bar. Refer the features section update for more details.
- Also added capability for users to provide additional notes for the code review so that Model can consider that extra code review guidelines provided by user. This is configurable from the extension settings.

### [1.6.1]

#### Theming Changes

- Few Theming changes

### [1.6.0]

#### New Feature

- Added capability to edit existing prompts from Chat History
- Added a Repeat prompt capability in case of API failure so that user don't need to type/copy paste the prompt

### [1.5.0]

#### New Feature

- Added capability to enable/disable logs for users to help in debugging

#### Experience Improvement:

- By default focus should be set for the Input box when user open the extension manually or using command & context menu

### [1.4.1]

#### Support for VS Code 1.83.0:

- With new version of VS Code 1.83.0 it started breaking the Delete and Settings Icons used so added a fix for same

### [1.4.0]

#### New Features:

- Added new context menu commands for developers to write test cases with just a single button click, ensured developers get full freedom to pick the libraries options their own rather than writing test cases with some generic library. For details refer to the features section for [writing test cases](#unit-test)
- Provided support for Light Theme as the code blocks were not clear for few languages in light theme
- Extended support for inline prompts using comments for other programming languages

#### Experience Improvements:

- Fixed an issue where the multi line comment was not working if it was not having a "\*" at starting of line

### [1.3.1]

#### Updated Readme:

- Updated Readme for adding details about Youtube Video published for how to install and few use cases around how you can use the `AI Code Companion`

### [1.3.0]

#### New Features:

- Added ability to review the code for all GIT Changes, please refer to updated features section for more details

#### Experience Improvements:

- Fixed the timeout issue
- Fixed a defect where inline code comments were not adding loading state if the comment was starting with line zero in the file
- Updated the Splash screen with important inoformation
- Updated the readme with feedback from users
- Fixed a defect where Inline Comments prompts were not working when the file was huge

### [1.2.0]

#### New Features:

- Added capability to use Inline Comments (both single and multi line comments) for asking AI Provider. Use keyboard shortcut `Ctrl+Alt+Enter/Return` from any line of your comment to ask questions. For progress bar/loading state please refer above features section
- Added capability to create new files from codeblocks
- Added capability to insert the code from codeblocks to working file
- Added capability to cancel the API requests, please refer above features section for more details

#### Experience Improvements:

- Ability to maintain the chatbox scroll position, in previous version it always used to scroll at the end of messages
- Ability to add line breaks in the Prompt Inputbox to change existing message, in previous version `Shift+Enter/Return` was always forcing cursor at the end of message but user should be able to edit the message to add line breaks anywhere and cursor will remain in focus too with line breaks.
- Increased the height for Inputbox
- Fixed the Send Button alignment for smaller viewports,send button gets shifted to next lien for smaller viewpport. In previous version button was getting cropped
- Fixed few defects

### [1.1.0]

- Added support for older versions of VS Code, starting with 1.74.0.

### [1.0.0]

- Initial release of the extension

**Enjoy!**
