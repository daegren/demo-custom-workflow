# demo-custom-workflow

A demonstration of a custom content publishing workflow using [Sanity](https://www.sanity.io/).

## Development

```sh
git clone git@github.com:sanity-io/demo-custom-workflow.git
cd demo-custom-workflow
npm install

# Start the development server
npm start
```

## Documentation

- [What’s the deal with this project?](#whats-the-deal-with-this-project)
  - [Start a new project using this demo as boilerplate](#start-a-new-project-using-this-demo-as-a-boilerplate)
  - [Create your custom schema definitions](#create-your-custom-schema-definitions)
  - [Configure the custom workflow](#configure-the-custom-workflow)
  - [Starting the development server](#starting-the-development-server)
  - [Deployment](#deployment)
  - [Got questions](#got-questions)
- [Demo code reference](#demo-code-reference)
  - [`app/components`](#appcomponents)
  - [`app/config`](#appconfig)
  - [`app/documentActions`](#appdocumentactions)
  - [`app/documentBadges`](#appdocumentbadges)
  - [`app/lib`](#applib)
  - [`app/schema`](#appschema)
  - [`app/scripts`](#appscripts)
  - [`app/structure`](#appstructure)
  - [`app/tools`](#apptools)

### What’s the deal with this project?

The purpose of this repository is to provide an example of how to go about creating a custom publishing workflow environment using [Sanity Studio](https://www.sanity.io/docs/sanity-studio).

> NOTE: Some of the code in this demo uses internal APIs that may change in the future. If you use this code as a starting point, and experience missing functionality, please review the changelog in this project. We recommend keeping a reference to this repo’s GitHub URL.

#### Start a new project using this demo as a boilerplate

```sh
git clone git@github.com:sanity-io/demo-custom-workflow.git my-sanity-studio
cd my-sanity-studio
rm -rf .git
# customize README.md
# customize package.json
# then
git init
sanity init
```

#### Create your custom schema definitions

Edit the schema files in `app/schema` to create content models you need for your project. Visit the [content modelling documentation](https://www.sanity.io/docs/content-modelling) to learn about custom schemas.

#### Configure the custom workflow

Once you have created the document types needed for your project, you need to add the custom document types to the workflow `types` configuration array in `app/config/workflow.js`.

#### Starting the development server

In this Sanity Studio project is run in the same manner as standard Sanity Studio project:

```sh
npm start
```

#### Deployment

If you want to deploy this Studio on a public URL so that editors can join your project, you may use the default Sanity hosting:

```sh
sanity deploy
```

#### Got questions?

Join our Slack community: https://slack.sanity.io/


### Demo code reference

The Sanity Studio customization code is mainly stored in the `app` directory.

#### `app/components`

The files in the `app/components` directory is a collection of custom React components used to render a KANBAN board, user selection inputs, workflow publishing buttons and so on.

#### `app/config`

The `app/config/workflow.js` file is used to configure which document types to include in the custom workflow and the possible workflow states.

The default states are:

- `Draft`
- `In review`
- `Require changes`
- `Accepted`
- `Published`

If you need other workflow states than the ones provided in this demo, you may rename and customize the contents of the `states` array. Keep in mind that it will require you to also change the logic in the document actions.

#### `app/documentActions`

This demo is made to showcase Sanity’s new _Document actions API_. This API enables the customization of the buttons in the footer drawer of the document editing environment in the Sanity Studio.

In `app/documentActions/index.js` you’ll find the document actions resolver function, and the custom actions used for the workflow implementation are placed in the `app/documentActions/workflow` directory.

If you want to disable the custom document actions, then remove the document action resolver configuration from the `parts` array in `sanity.json`:

```json
  "parts": [
    {
      "implements": "part:@sanity/base/document-actions/resolver",
      "path": "app/documentActions"
    }
  ]
```

See the documentation for more information on how to use the Document actions API.

#### `app/documentBadges`

The _Document badges API_ is related to the document actions, and is a way to customize the document status badges in the editing environment.

```json
  "parts": [
    {
      "implements": "part:@sanity/base/document-badges/resolver",
      "path": "app/documentBadges"
    }
  ]
```

If you want to disable the custom document badges, then remove the document badge resolver configuration from the `parts` array in `sanity.json`:

See the documentation for more information on how to use the Document badges API.

#### `app/lib`

This demo application requires some functionality that is not provided by the Sanity Studio out of the box.

- `app/lib/document`  
  React hooks and observables that are used to subscribe to real-time updates on workflow documents in this demo.
- `app/lib/project`
  React hooks and observables that are used to subscribe to real-time updates on the Sanity project – mainly which users are part of the project.
- `app/lib/router`
  The router provider is used to give easier access to the Sanity router context value, and for creating internal links more easily.
- `app/lib/user`
  React hooks used to stream user info and lists of user IDs that are used in this demo.
- `app/utils`
  Utility functions.
- `app/workflow`
  React hooks that make it easier to work with the workflow metadata documents.

#### `app/schema`

The schema definitions used in this demo.

```json
  "parts": [
    {
      "name": "part:@sanity/base/schema",
      "path": "app/schema"
    }
  ]
```

#### `app/scripts`

Scripts used while developing this demo.

#### `app/structure`

The Structure configuration used in this demo.

```json
  "parts": [
    {
      "name": "part:@sanity/desk-tool/structure",
      "path": "app/structure"
    }
  ]
```

#### `app/tools`

The `app/tools/board.js` is the root configuration for the KANBAN tool in this demo. It is referenced in the `sanity.json` configuration file:

```json
  "parts": [
    {
      "implements": "part:@sanity/base/tool",
      "path": "app/tools/board"
    }
  ]
```
