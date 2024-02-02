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

Now we'll execute some of the most common operations in the lifecycle of an experiment via the Optimizely APIs.   
Each of the operations has its own `.rest` file in the `requests` directory of this repo. At this point, the directory structure of your project should be as follows:

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

### 1. Create a feature flag and flag variables.
Run the API request in the file **1_create_flag.rest**. You should see a response like this: 
```json
{
  "key": "inventory_on_pdp",
  "name": "Inventory on PDP",
  "description": "Flag to either show or hide inventory on the product detail page.",
  "url": "/projects/{{your_project_id}}/flags/inventory_on_pdp",
  "update_url": "/projects/{{your_project_id}}/flags",
  "delete_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp",
  "archive_url": "/projects/{{your_project_id}}/flags/archived",
  "variable_definitions": {
    "show_amounts": {
      "key": "show_amounts",
      "description": "If inventory is shown, this variable controls what is displayed: available/unavailable vs the actual inventory amount.",
      "type": "boolean",
      "default_value": "false",
      "created_time": "2024-01-26T07:06:11.433733Z",
      "updated_time": "2024-01-26T07:06:11.433741Z"
    },
    "text_color": {
      "key": "text_color",
      "description": "The color of the text displaying the inventory.",
      "type": "string",
      "default_value": "green",
      "created_time": "2024-01-26T07:06:11.433741Z",
      "updated_time": "2024-01-26T07:06:11.433742Z"
    },
    "low_stock_threshold": {
      "key": "low_stock_threshold",
      "description": "The threshold quantity remaining in order to show a low stock message.  Setting this to 0 results in not displaying the low stock message.",
      "type": "integer",
      "default_value": "0",
      "created_time": "2024-01-26T07:06:11.433743Z",
      "updated_time": "2024-01-26T07:06:11.433744Z"
    },
    "low_stock_message": {
      "key": "low_stock_message",
      "description": "The Low Stock message to display if low_stock_threshold is set to a number greater than 0.",
      "type": "string",
      "default_value": "Low Stock - Act Quick",
      "created_time": "2024-01-26T07:06:11.433744Z",
      "updated_time": "2024-01-26T07:06:11.433745Z"
    }
  },
  "environments": {
    "production": {
      "key": "production",
      "name": "Production",
      "enabled": false,
      "priority": 1,
      "rules_summary": {},
      "enable_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/production/ruleset/enabled"
    },
    "development": {
      "key": "development",
      "name": "Development",
      "enabled": false,
      "priority": 2,
      "rules_summary": {},
      "enable_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset/enabled"
    }
  },
  "id": 122050,
  "urn": "flags.flags.optimizely.com::122050",
  "archived": false,
  "outlier_filtering_enabled": false,
  "project_id": {{your_project_id}},
  "created_by_user_id": {{your_email}},
  "account_id": {{your_account_id}},
  "created_time": "2024-01-26T07:06:11.398293Z",
  "updated_time": "2024-01-26T07:06:11.398299Z",
  "revision": 1
}
```

### 2. Create variations.
Add the `flag_key` to your environment like this:
```diff
{
  "rest-client.environmentVariables": {
    "$shared": {
      "flagsUrl": "https://api.optimizely.com/flags/v1",
      "baseUrl": "https://api.optimizely.com/v2",
      "token": "Bearer {{yourToken}}",
      "projectId": {{yourProjectId}},
+     "flagKey": {{your_flag_key}}
    }
  }
}
```
Now, run all the API requests in **2_create_variations.rest**. Responses should be like this:
```json
{
  "key": "on_hide_amounts",
  "name": "On Hide Amounts",
  "description": "Inventory is shown as available/unavailable, in the color green.",
  "variables": {
    "low_stock_message": {
      "key": "low_stock_message",
      "type": "string",
      "value": "Low Stock - Act Quick",
      "is_default": true
    },
    "low_stock_threshold": {
      "key": "low_stock_threshold",
      "type": "integer",
      "value": "0",
      "is_default": true
    },
    "show_amounts": {
      "key": "show_amounts",
      "type": "boolean",
      "value": "false",
      "is_default": true
    },
    "text_color": {
      "key": "text_color",
      "type": "string",
      "value": "green",
      "is_default": true
    }
  },
  "id": 382325,
  "urn": "variations.flags.optimizely.com::382325",
  "flag_key": "inventory_on_pdp",
  "environment_usage_count": {},
  "archived": false,
  "enabled": true,
  "in_use": false,
  "created_time": "2024-01-26T11:41:00.800857Z",
  "updated_time": "2024-01-26T11:41:00.800862Z",
  "url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/variations/on_hide_amounts",
  "update_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/variations",
  "delete_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/variations/on_hide_amounts",
  "archive_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/variations/archived",
  "fetch_flag_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp",
  "revision": 1
}

```
### 3. Create events.
Now we will create some events in the project. Go ahead and run all the requests in **3_create_events.rest**. For each of them, you'll see a response like this:
```json
{
  "archived": false,
  "category": "other",
  "created": "2024-01-26T11:47:56.638984Z",
  "description": "Event used to track users that have added the item to the cart.",
  "event_type": "custom",
  "id": 27447690047,
  "is_classic": false,
  "key": "add_to_cart",
  "name": "add_to_cart",
  "project_id": {{your_project_id}}
}
```
Save the id of the events. We will need to use them later.
### 4. Create attributes.
Run the **4_create_attributes.rest**. For each request, you should see a response like this:
```json
{
  "archived": false,
  "condition_type": "custom_attribute",
  "description": "",
  "id": 27509090124,
  "key": "is_logged_in",
  "last_modified": "2024-01-29T08:01:51.953697Z",
  "name": "is_logged_in",
  "project_id": {{your_project_id}}
}
```
### 5. Create audiences.
We need to create some audiences too. To create them, run **5_create_audiences.rest**. For each request, there will be a response like this:
```json
{
  "archived": false,
  "conditions": "[\"and\", [\"or\", [\"or\", {\"match_type\": \"exact\", \"name\": \"is_logged_in\", \"type\": \"custom_attribute\", \"value\": false}]]]",
  "created": "2024-01-29T08:04:58.002727Z",
  "description": "",
  "experiment_count": 0,
  "id": 27430350138,
  "is_classic": false,
  "last_modified": "2024-01-29T08:04:58.002734Z",
  "name": "Anonymous Users",
  "project_id": {{your_project_id}},
  "segmentation": false
}
```
### 6. Create an experiment.
Now we will create an experiment. But, we need to add an event as a metric for the experiment. We've already created some events in step **3**, now we will use one of them. For this case, we're going to use the event `add_to_cart`.   
We hope you already stored the id of the event; make sure you add them to the environment. You also need to add the environment you want to create the experiment on. For this case, we will use `development`.  
```diff
{
  "rest-client.environmentVariables": {
    "$shared": {
      "flagsUrl": "https://api.optimizely.com/flags/v1",
      "baseUrl": "https://api.optimizely.com/v2",
      "token": "Bearer {{yourToken}}",
      "projectId": {{yourProjectId}},
      "flagKey": {{your_flag_key}},
+     "environment": "development",
+     "eventId": 27280660613
    }
  }
}
```
Now we will execute the **6_create_experiment.rest**. You should see a response like this:
````json
{
  "url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset",
  "update_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset",
  "enable_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset/enabled",
  "rules": {
    "stock_message_test": {
      "key": "stock_message_test",
      "name": "Stock Message Test",
      "description": "",
      "variations": {
        "off": {
          "key": "off",
          "name": "Off",
          "percentage_included": 3333
        },
        "on_show_amounts": {
          "key": "on_show_amounts",
          "name": "On Show Amounts",
          "percentage_included": 3333
        },
        "on_show_amounts_red": {
          "key": "on_show_amounts_red",
          "name": "On Show Amounts Red",
          "percentage_included": 3334
        }
      },
      "type": "a/b",
      "distribution_mode": "manual",
      "id": 295492,
      "urn": "rules.flags.optimizely.com::295492",
      "archived": false,
      "enabled": false,
      "created_time": "2024-01-29T08:54:17.963801Z",
      "updated_time": "2024-01-29T08:54:17.963808Z",
      "audience_conditions": [],
      "audience_ids": [],
      "percentage_included": 10000,
      "metrics": [
        {
          "event_id": {{your_event_id}},
          "event_type": "custom",
          "scope": "visitor",
          "aggregator": "unique",
          "winning_direction": "increasing",
          "display_title": "add_to_cart"
        }
      ],
      "allow_list": {},
      "group_rule": {
        "group_id": 0,
        "traffic_allocation": 0
      },
      "group_remaining_traffic_allocation": 100
    }
  },
  "rule_priorities": [
    "stock_message_test"
  ],
  "id": 323190,
  "urn": "rulesets.flags.optimizely.com::323190",
  "archived": false,
  "enabled": false,
  "updated_time": "2024-01-29T08:54:18.279750Z",
  "flag_key": "inventory_on_pdp",
  "environment_key": "development",
  "environment_name": "Development",
  "default_variation_key": "off",
  "default_variation_name": "Off",
  "revision": 2
}

````
Congratulations! You just created your first experiment.

### 7. Launch the experiment.
Now we will launch the experiment we just created. Execute the endpoints in **7_launch_experiment.rest**.  
Sample response:
```json
{
  "url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset",
  "update_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset",
  "disable_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset/disabled",
  "rules": {
    "stock_message_test": {
      "key": "stock_message_test",
      "name": "Stock Message Test",
      "description": "",
      "variations": {
        "off": {
          "key": "off",
          "name": "Off",
          "percentage_included": 3333
        },
        "on_show_amounts": {
          "key": "on_show_amounts",
          "name": "On Show Amounts",
          "percentage_included": 3333
        },
        "on_show_amounts_red": {
          "key": "on_show_amounts_red",
          "name": "On Show Amounts Red",
          "percentage_included": 3334
        }
      },
      "type": "a/b",
      "distribution_mode": "manual",
      "id": 295492,
      "urn": "rules.flags.optimizely.com::295492",
      "archived": false,
      "enabled": true,
      "created_time": "2024-01-29T08:54:17.963801Z",
      "updated_time": "2024-01-29T08:59:54.871320Z",
      "audience_conditions": [],
      "audience_ids": [],
      "percentage_included": 10000,
      "metrics": [
        {
          "event_id": {{your_event_id}},
          "event_type": "custom",
          "scope": "visitor",
          "aggregator": "unique",
          "winning_direction": "increasing",
          "display_title": "add_to_cart"
        }
      ],
      "fetch_results_ui_url": "https://app.optimizely.com/v2/projects/{{your_project_id}}/results/9300000403730/experiments/9300000495990",
      "allow_list": {},
      "group_rule": {
        "group_id": 0,
        "traffic_allocation": 0
      },
      "group_remaining_traffic_allocation": 100
    }
  },
  "rule_priorities": [
    "stock_message_test"
  ],
  "id": 323190,
  "urn": "rulesets.flags.optimizely.com::323190",
  "archived": false,
  "enabled": true,
  "updated_time": "2024-01-29T08:59:55.179266Z",
  "flag_key": "inventory_on_pdp",
  "environment_key": "development",
  "environment_name": "Development",
  "default_variation_key": "off",
  "default_variation_name": "Off",
  "revision": 4
}

```
### 8. Conclude the experiment.
You can also disable an experiment. To do this, run **8_conclude_experiment.rest**.
Sample response:
```json
{
  "url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset",
  "update_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset",
  "disable_url": "/projects/{{your_project_id}}/flags/inventory_on_pdp/environments/{{your_environment}}/ruleset/disabled",
  "rules": {
    "stock_message_test": {
      "key": "stock_message_test",
      "name": "Stock Message Test",
      "description": "",
      "variations": {
        "off": {
          "key": "off",
          "name": "Off",
          "percentage_included": 3333
        },
        "on_show_amounts": {
          "key": "on_show_amounts",
          "name": "On Show Amounts",
          "percentage_included": 3333
        },
        "on_show_amounts_red": {
          "key": "on_show_amounts_red",
          "name": "On Show Amounts Red",
          "percentage_included": 3334
        }
      },
      "type": "a/b",
      "distribution_mode": "manual",
      "id": 295492,
      "urn": "rules.flags.optimizely.com::295492",
      "archived": false,
      "enabled": false,
      "created_time": "2024-01-29T08:54:17.963801Z",
      "updated_time": "2024-01-29T09:12:44.867722Z",
      "audience_conditions": [],
      "audience_ids": [],
      "percentage_included": 10000,
      "metrics": [
        {
          "event_id": {{your_event_id}},
          "event_type": "custom",
          "scope": "visitor",
          "aggregator": "unique",
          "winning_direction": "increasing",
          "display_title": "add_to_cart"
        }
      ],
      "fetch_results_ui_url": "https://app.optimizely.com/v2/projects/{{your_project_id}}/results/9300000403730/experiments/9300000495990",
      "allow_list": {},
      "group_rule": {
        "group_id": 0,
        "traffic_allocation": 0
      },
      "group_remaining_traffic_allocation": 100
    }
  },
  "rule_priorities": [
    "stock_message_test"
  ],
  "id": 323190,
  "urn": "rulesets.flags.optimizely.com::323190",
  "archived": false,
  "enabled": true,
  "updated_time": "2024-01-29T09:12:45.111613Z",
  "flag_key": "inventory_on_pdp",
  "environment_key": "development",
  "environment_name": "Development",
  "default_variation_key": "off",
  "default_variation_name": "Off",
  "revision": 5
}
```
### 9. Analyze results.
Let's analyze results of the experiment. Open the file **9_analyze_results.rest** and execute the requests one by one. Follow the instructions before each request.
