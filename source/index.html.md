---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Enjoy...</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Atumaatu API documentation. You can use this API to access out endpoints, which can get information on your budgets, create your budgets, and get the content of a budget from our database.

We have language bindings in Ruby and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This API documentation page was created with [Slate](https://github.com/slatedocs/slate).

# Authentication

Atumaatu requires a JWT keys to allow access to the API. You get one by either signing up or logging in.

Atumaatu expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer ${token}`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your JWT token.
</aside>

## Register

```javascript
const axios = require('axios');

axios({
  method: 'post',
  url: 'https://atumaatu.herokuapp.com/v1/users',
  data: JSON.stringify({
    user: {
      firstname: "Jane",
      lastname: "Deo",
      email: "email@email.com",
      password: "password",
      password_confirmation: "password",
      currency: "NGN"
    }
  }),
  headers: {
    "Content-Type": "application/json",
  }
})
```

> The above command returns JSON structured like this:

```json
  {
    "payload": {
      "firstName":"Jane",
      "id":9,
      "lastName":"Deo",
      "currency":"NGN",
      "email":"email@email.com",
      "token":"some.bearer.token"
    },
    "links":[]
  }
```

> Incase of an error, it is returned like this:

```json
  {
    "error": "error message e.g. Validation failed: Email has already been taken"
  }
```
This endpoint registers a user.

### HTTP Request

`POST https://atumaatu.herokuapp.com/v1/users`

### Request Body

Parameter | Type | Description
--------- | ----- | -----------
firstname | string | user's firstname
lastname  | string | user's lastname
email     | string | user's email address
password  | string | user's lastname
password_confirmation | string | user's lastname
currency  | string | user's default currency

## Login

```javascript
const axios = require('axios');

axios({
  method: 'post',
  url: 'https://atumaatu.herokuapp.com/v1/sessions',
  data: JSON.stringify({
    email: "email@email.com",
    password: "password",
  }),
  headers: {
    "Content-Type": "application/json",
  }
})
```

> The above command returns JSON structured like this:

```json
  {
    "payload": {
      "firstName":"Jane",
      "id":9,
      "lastName":"Deo",
      "currency":"NGN",
      "email":"email@email.com",
      "token":"some.bearer.token"
    },
    "links":[]
  }
```

> Incase of an error, it is returned like this:

```json
  {
    "error": "error message e.g. User or password is incorrect"
  }
```
This endpoint logs a user.

### HTTP Request

`POST https://atumaatu.herokuapp.com/v1/sessions/`

### Request Body

Parameter | Type | Description
--------- | ----- | -----------
email     | string | user's email address
password  | string | user's lastname

# User

This section simple entails updating the user preffered currency 

## Update User's Currency

```javascript
const axios = require('axios');

axios({
  method: 'put',
  url: 'https://atumaatu.herokuapp.com/v1/sessions/:user_id',
  data: JSON.stringify({
    user: {
      currency: "USD",
    }
  }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this:

```json
  {
    "payload": {
      "firstName":"Jane",
      "id":9,
      "lastName":"Deo",
      "currency":"USD",
      "email":"email@email.com",
      "token":"some.bearer.token"
    },
    "links":[]
  }
```

> Incase of an error, it is returned like this:

```json
  {
    "error": "error message "
  }
```
This endpoint updates the currency of a user.

### HTTP Request

`POST https://atumaatu.herokuapp.com/v1/sessions/:user_id`

### URL Parameters

Parameter | Description
--------- | -----------
user_id | The ID of the user to update

### Request Body

Parameter | Type | Description
--------- | ---- | -----------
currency  | string | user's new currency choice

# Budget

This api endpoints deals with viewing all budgets you have created, creating, editing, copying and deleting a budget.

## Get All Budgets


```javascript
const axios = require('axios');

axios({
  method: 'get',
  url: 'https://atumaatu.herokuapp.com/v1/budgets',
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this when you have budgets:

```json
{
  "payload": {
    "budgets":[{
        "name":"Budget January",
        "budgetedCost":800,
        "actualCost":0,
        "id":27,
        "income":30000,
        "createdAt":"2020-06-29T21:03:53.744Z",
        "itemCount":2
        }]
      },
  "links":[]
}
```
> The above command returns JSON structured like this when you have no budget:

```json
  {
    "payload": {
      "budgets":[]
    },
    "links":[]
  }
```

This endpoint retrieves all budgets.

### HTTP Request

`GET https://atumaatu.herokuapp.com/v1/budgets`

## Create a Budget

```javascript
const axios = require('axios');

axios({
  method: 'post',
  url: 'https://atumaatu.herokuapp.com/v1/budgets',
  data: JSON.stringify({ name: 'budget_name', income: 'expected income' }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this:

```json
{
  "id":29,
  "name":"Budget January",
  "budgeted_cost":0,
  "user_id":8,
  "created_at":"2020-06-30T11:25:40.030Z",
  "updated_at":"2020-06-30T11:25:40.030Z",
  "actual_cost":0,
  "income":20000
}
```

This endpoint creates a budget.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`POST https://atumaatu.herokuapp.com/v1/budgets`

### Request Body

Parameter | Type | Description
--------- | ----- | -----------
name     | string | budget name
income  | number | expect user's income

## Copy a Budget
```javascript
const axios = require('axios');

axios({
  method: 'put',
  url: 'https://atumaatu.herokuapp.com/v1/budgets/copy',
  data: JSON.stringify({ existing_budget_id: 'id' }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this:

```json
{
  "payload": {
    "budgets":[{
        "name":"Budget January",
        "budgetedCost":800,
        "actualCost":0,
        "id":27,
        "income":30000,
        "createdAt":"2020-06-29T21:03:53.744Z",
        "itemCount":2
      }, {
        "name":"Budget January copy",
        "budgetedCost": 800,
        "actualCost": 0,
        "id":28,
        "income": 30000,
        "createdAt":"2020-06-29T22:03:53.744Z",
        "itemCount": 2
        }]
      },
  "links":[]
}
```
This endpoint copies a budget.

### HTTP Request

`PUT https://atumaatu.herokuapp.com/v1/budgets/copy`

### Request Body

Parameter | Type | Description
--------- | ---- | -----------
existing_budget_id  | number | The id of the budget you want to copy


## Edit a Budget

```javascript
const axios = require('axios');

axios({
  method: 'put',
  url: 'https://atumaatu.herokuapp.com/v1/budgets/:budget_id',
  data: JSON.stringify({ name: 'budget_name', income: 'expected income' }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you changed the name of budget_id 27 to 'Budget May':

```json
{
  "payload": {
    "budgets":[{
        "name":"Budget May",
        "budgetedCost": 800,
        "actualCost": 0,
        "id": 27,
        "income": 30000,
        "createdAt": "2020-06-29T21:03:53.744Z",
        "itemCount": 2
      }, {
        "name":"Budget January copy",
        "budgetedCost": 800,
        "actualCost": 0,
        "id": 28,
        "income": 30000,
        "createdAt":"2020-06-29T22:03:53.744Z",
        "itemCount": 2
        }]
      },
  "links":[]
}
```

This endpoint edits a budget. You can either edit the name, income or both.

### HTTP Request

`PUT https://atumaatu.herokuapp.com/v1/budgets/:budget_id`

### URL Parameters

Parameter | Description
--------- | -----------
budget_id | The ID of the budget to edit

## Delete a Budget

```javascript
const axios = require('axios');

axios({
  method: 'delete',
  url: 'https://atumaatu.herokuapp.com/v1/budgets/:budget_id',
  data: {},
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you delete budget_id 27:

```json
{
  "payload": {
    "budgets":[{
        "name":"Budget January copy",
        "budgetedCost": 800,
        "actualCost": 0,
        "id": 28,
        "income": 30000,
        "createdAt":"2020-06-29T22:03:53.744Z",
        "itemCount": 2
        }]
      },
  "links":[]
}
```
This endpoint deletes a budget.

### HTTP Request

`DELETE https://atumaatu.herokuapp.com/v1/budgets/:budget_id`

### URL Parameters

Parameter | Description
--------- | -----------
budget_id | The ID of the budget to delete

# Items

The api endpoints handles creating, editing, deleting an item and viewing all items added.

## Get All items


```javascript
const axios = require('axios');

axios({
  method: 'get',
  url: 'https://atumaatu.herokuapp.com/v1/items',
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this when you have budgets:

```json
{
  "payload": 
    {
      "items": [
        {
          "name":"Toiletuii",
          "id":22,
          "deletable":true
        },
        {
          "name":"Toilet",
          "id":23,
          "deletable":true
        },
        {
          "name":"Milk",
          "id":24,
          "deletable":true
        },
        {
          "name":"Internet",
          "id":25,
          "deletable":false
        }
      ]
    },
  "links":[]
}
```
> The above command returns JSON structured like this when you have no budget:

```json
  {
    "payload": {
      "items":[]
    },
    "links":[]
  }
```

This endpoint retrieves all items you have ever added when creating a budget membership.

### HTTP Request

`GET https://atumaatu.herokuapp.com/v1/items`

## Create an Item

```javascript
const axios = require('axios');

axios({
  method: 'post',
  url: 'https://atumaatu.herokuapp.com/v1/items',
  data: JSON.stringify({ item: { name: 'item_name' } }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if we add an item with name `Milo`:

```json
{
  "payload": 
    {
      "items": [
        {
          "name":"Toiletuii",
          "id":22,
          "deletable":true
        },
        {
          "name":"Toilet",
          "id":23,
          "deletable":true
        },
        {
          "name":"Milk",
          "id":24,
          "deletable":true
        },
        {
          "name":"Internet",
          "id":25,
          "deletable":false
        },
        {
          "name":"Milo",
          "id": 26,
          "deletable":true
        }
      ]
    },
  "links":[]
}
```

This endpoint creates an item.

### HTTP Request

`POST https://atumaatu.herokuapp.com/v1/items`

### Request Body

Parameter | Type | Description
--------- | ----- | -----------
item     | object | item object that wraps the required info necessary to create it
name  | string | item name

## Edit an Item

```javascript
const axios = require('axios');

axios({
  method: 'put',
  url: 'https://atumaatu.herokuapp.com/v1/items/:item_id',
  data: JSON.stringify({ item: { name: 'item_new_name' } }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you edit item_id 25 name to `Internet - ntel`:

```json
{
  "payload": 
    {
      "items": [
        {
          "name":"Toiletuii",
          "id":22,
          "deletable":true
        },
        {
          "name":"Toilet",
          "id":23,
          "deletable":true
        },
        {
          "name":"Milk",
          "id":24,
          "deletable":true
        },
        {
          "name":"Internet - ntel",
          "id":25,
          "deletable":false
        },
        {
          "name":"Milo",
          "id": 26,
          "deletable":true
        }
      ]
    },
  "links":[]
}
```
This endpoint edits a budget.

### HTTP Request

`PUT https://atumaatu.herokuapp.com/v1/items/:item_id`

### URL Parameters

Parameter | Description
--------- | -----------
item_id | The ID of the item to edit

## Delete a Budget

```javascript
const axios = require('axios');

axios({
  method: 'delete',
  url: 'https://atumaatu.herokuapp.com/v1/items/:item_id',
  data: {},
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you delete item_id 23:

```json
{
  "payload": 
    {
      "items": [
        {
          "name":"Toiletuii",
          "id":22,
          "deletable":true
        },
        {
          "name":"Milk",
          "id":24,
          "deletable":true
        },
        {
          "name":"Internet",
          "id":25,
          "deletable":false
        },
        {
          "name":"Milo",
          "id": 26,
          "deletable":true
        }
      ]
    },
  "links":[]
}
```
This endpoint deletes an item.

### HTTP Request

`DELETE https://atumaatu.herokuapp.com/v1/items/:item_id`

### URL Parameters

Parameter | Description
--------- | -----------
item_id | The ID of the item to delete

# Budget Membership

This api endpoint deals with adding items to a budget.

## Get All items
```javascript
const axios = require('axios');

axios({
  method: 'get',
  url: 'https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id',
  data: {},
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you get budget_memberships for budget_id 29 name to:

```json
{
  "payload": {
    "budget": {
      "id":29,
      "name":"Budget January",
      "budgeted_cost":200,
      "user_id":8,
      "created_at":"2020-06-30T11:25:40.030Z",
      "updated_at":"2020-06-30T12:00:01.938Z",
      "actual_cost":0,
      "income":20000
    },
    "items":[
      {
        "name":"Internet",
        "budgetedCost":200,
        "actualCost":0,
        "executed":false,
        "bud_mem_id":51,
        "item_id":25
      }
    ]
  },
  "links":[]
}
```
This endpoint gets all the items in a budget.

### HTTP Request

`GET https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id`

### URL Parameters

Parameter | Description
--------- | -----------
budget_id | The ID of the budget to add items to

## Add an item or items

```javascript
const axios = require('axios');

axios({
  method: 'post',
  url: 'https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id',
  data: JSON.stringify({ items: [{ name: "Milo", budgeted_cost: 1000 }] }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you add an item to budget 29:

```json
{
  "payload": {
    "budget": {
      "id":29,
      "name":"Budget January",
      "budgeted_cost":1200,
      "user_id":8,
      "created_at":"2020-06-30T11:25:40.030Z",
      "updated_at":"2020-06-30T12:00:01.938Z",
      "actual_cost":0,
      "income":20000
    },
    "items":[
      {
        "name":"Internet",
        "budgetedCost":200,
        "actualCost":0,
        "executed":false,
        "bud_mem_id":51,
        "item_id":25
      },
      {
        "name":"Milo",
        "budgetedCost": 1000,
        "actualCost": 0,
        "executed": false,
        "bud_mem_id": 52,
        "item_id": 26
      }
    ]
  },
  "links":[]
}
```
This endpoint adds an item or many items to a budget.

### HTTP Request

`POST https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id`

### URL Parameters

Parameter | Description
--------- | -----------
budget_id | The ID of the budget to add items to

### Request Body

Parameter | Type | Description
--------- | ----- | -----------
items | array | item array that wraps the required info necessary to create it
name  | string | item name
budgeted_cost | number | budgeted_cost of the item

## Edit an item

```javascript
const axios = require('axios');

axios({
  method: 'put',
  url: 'https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id',
  data: JSON.stringify({ actual_cost: 900, budgeted_cost: 1000, executed: true, bud_mem_id: 52 }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you edit an item with bud_mem_id 52 in budget 29:

```json
{
  "payload": {
    "budget": {
      "id":29,
      "name":"Budget January",
      "budgeted_cost":1200,
      "user_id":8,
      "created_at":"2020-06-30T11:25:40.030Z",
      "updated_at":"2020-06-30T12:00:01.938Z",
      "actual_cost":900,
      "income":20000
    },
    "items":[
      {
        "name":"Internet",
        "budgetedCost":200,
        "actualCost":0,
        "executed":false,
        "bud_mem_id":51,
        "item_id":25
      },
      {
        "name":"Milo",
        "budgetedCost": 1000,
        "actualCost": 900,
        "executed": true,
        "bud_mem_id": 52,
        "item_id": 26
      }
    ]
  },
  "links":[]
}
```
This endpoint edits an items in a budget. The response also returns an updated `budgeted_cost` and `actual_cost` for the budget.

### HTTP Request

`PUT https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id`

### URL Parameters

Parameter | Description
--------- | -----------
budget_id | The ID of the budget to add items to

### Request Body

Parameter | Type | Description
--------- | ----- | -----------
actual_cost | number | the actual price you bought spent on the item
executed  | boolean | true if actual price is added, false if actual price isn't added
budgeted_cost | number | budgeted_cost of the item
bud_mem_id | number | budget membership id of item you want to edit

## Delete an item from the budget

```javascript
const axios = require('axios');

axios({
  method: 'delete',
  url: 'https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id',
  data: JSON.stringify({ bud_mem_id: 52 }),
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  }
})
```

> The above command returns JSON structured like this if you delete an item with bud_mem_id 52 from budget 29:

```json
{
  "payload": {
    "budget": {
      "id":29,
      "name":"Budget January",
      "budgeted_cost":200,
      "user_id":8,
      "created_at":"2020-06-30T11:25:40.030Z",
      "updated_at":"2020-06-30T12:00:01.938Z",
      "actual_cost":0,
      "income":20000
    },
    "items":[
      {
        "name":"Internet",
        "budgetedCost":200,
        "actualCost":0,
        "executed":false,
        "bud_mem_id":51,
        "item_id":25
      }
    ]
  },
  "links":[]
}
```
This endpoint delete an item in a budget. The response also returns an updated `budgeted_cost` and `actual_cost` for the budget.

### HTTP Request

`DELETE https://atumaatu.herokuapp.com/v1/budget_memberships/:budget_id`

### URL Parameters

Parameter | Description
--------- | -----------
budget_id | The ID of the budget to add items to