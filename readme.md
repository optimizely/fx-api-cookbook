# Feature Experimentation REST API Cookbook

This repo contains API recipes to get started with Optimizely Feature Experimentation (FX). 👩‍🍳

We'll walk through some of the most common operations in the lifecycle of an experiment using the Optimizely APIs. 🧪

## Prerequisites

To get started with this API cookbook, first complete the below setup.

### Create Optimizely Account

If you're not an existing enterprise customer, create a completely free feature flagging account with Optimizely [here](https://www.optimizely.com/free-feature-flagging/) (no credit card required!).

### Install VS Code + Extensions

We'll be using VS Code as a REST client make requests to the Optimizely APIs. Use the below links to install your IDE (integrated development environment) and needed extensions:

- [VS Code](https://code.visualstudio.com/download)
- [Rest Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) (REST Client extension for VS Code)

### Get Optimizely API Personal Access Token

Follow [these steps](https://docs.developers.optimizely.com/feature-experimentation/docs/using-the-rest-api#generate-a-token) to get a personal access token to the Optimizely APIs.

### VS Code REST Client Setup

Next, we'll setup VS Code so we can make requests to the Optimizely APIs.

Let's create some environment variables for the Optimizely API endpoints and your personal access token that we can reuse across API requests.

In VS Code, create a directory named `.vscode` in the root of your project. In this directory, create a file named `settings.json` and copy and paste the below into the file. Make sure to replace `{{token}}` with your personal access token and `{{yourProjectId}}` with your project ID (you can find this in the URL from the app homepage, e.g. `https://app.optimizely.com/v2/projects/{{your ProjectId}}/flags/list`):

```
{
  "rest-client.environmentVariables": {
    "$shared": {
      "flagsUrl": "https://api.optimizely.com/flags/v1",
      "baseUrl": "https://api.optimizely.com/v2",
      "token": "Bearer {{yourToken}}",
      "projectId": {{yourProjectId}}
    }
  }
}
```

You can find more information on adding environment variables [here](https://github.com/Huachao/vscode-restclient).

## Overview

Now we'll execute some of the most common operations in the lifecycle of an experiment via the Optimizely APIs, including:

1. Create a feature flag and flag variables.
2. Create variations.
3. Create events.
4. Create attributes.
5. Create audiences.
6. Create an experiment.
7. Launch the experiment.
8. Conclude the experiment.
9. Analyze results.

Each of the above sections has its own `.rest` file in the `requests` directory of this repo. At this point, the directory structure of your project should be as follows:

```
📦fx-api-recipes
 ┣ 📂.vscode
 ┃ ┗ 📜settings.json
 ┣ 📂requests
 ┃ ┣ 📜1_create_flag.rest
 ┃ ┣ 📜2_create_variations.rest
 ┃ ┣ 📜3_create_events.rest
 ┃ ┣ 📜4_create_attributes.rest
 ┃ ┣ 📜5_create_audiences.rest
 ┃ ┣ 📜6_create_experiment.rest
 ┃ ┣ 📜7_launch_experiment.rest
 ┃ ┣ 📜8_conclude_experiment.rest
 ┃ ┗ 📜9_analyze_results.rest
 ┗ 📜readme.md
```

To execute the requests, open each `.rest` file in VS Code and click the `Send Request` link above the request.

For more information on how to send requests using the REST Client extension, see [here](https://marketplace.visualstudio.com/items?itemName=humao.rest-client#usage).
