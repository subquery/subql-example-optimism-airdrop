# SubQuery - Optimism Airdrop Example

A SubQuery project defines which data The SubQuery will index from the blockchain, and how it will store it.

This SubQuery Project indexes all claim events from the Optimism airdrop

## Preparation

#### Environment

- [Typescript](https://www.typescriptlang.org/) are required to compile project and define types.

- Both SubQuery CLI and generated Project have dependencies and require [Node](https://nodejs.org/en/).

#### Install the SubQuery CLI

Install SubQuery CLI globally on your terminal by using NPM:

```
npm install -g @subql/cli
```

Run help to see available commands and usage provide by CLI

```
subql help
```

## Initialize the starter package

Inside the directory in which you want to create the SubQuery project, simply replace `project-name` with your project name and run the command:

```
subql init --starter project-name
```

Then you should see a folder with your project name has been created inside the directory, you can use this as the start point of your project. And the files should be identical as in the [Directory Structure](https://doc.subquery.network/directory_structure.html).

Last, under the project directory, run following command to install all the dependency.

```
yarn install
```

## Configure your project

In the starter package, we have provided a simple example of project configuration. You will be mainly working on the following files:

- The Manifest in `project.yaml`
- The GraphQL Schema in `schema.graphql`
- The Mapping functions in `src/mappings/` directory

For more information on how to write the SubQuery,
check out our doc section on [Define the SubQuery](https://doc.subquery.network/define_a_subquery.html)

#### Code generation

In order to index your SubQuery project, it is mandatory to build your project first.
Run this command under the project directory.

```
yarn codegen
```

## Build the project

In order to deploy your SubQuery project to our hosted service, it is mandatory to pack your configuration before upload.
Run pack command from root directory of your project will automatically generate a `your-project-name.tgz` file.

```
yarn build
```

## Indexing and Query

#### Run required systems in docker

Under the project directory run following command:

```
docker-compose pull && docker-compose up
```

#### Query the project

Open your browser and head to `http://localhost:3000`.

Finally, you should see a GraphQL playground is showing in the explorer and the schemas that ready to query.

For the `subql-starter` project, you can try to query with the following code to get a taste of how it works.

```graphql
query {
  claims(first: 5, orderBy: VALUE_DESC) {
    nodes {
      blockHeight
      value
      blockHeight
      blockTimestamp
      account
    }
  }
  dailyClaimSummaries(first: 5) {
    nodes {
      id
      totalClaimed
      claims(first: 5) {
        totalCount
        nodes {
          id
          account
          value
        }
      }
    }
  }
}
```

```json
{
  "data": {
    "claims": {
      "nodes": [
        {
          "blockHeight": "100322581",
          "value": "7477664852469040021504",
          "blockTimestamp": "1684714980",
          "account": "0x85399353400C5B67fD6eE53B1d2cd183bAE7dDdb"
        },
        {
          "blockHeight": "100316590",
          "value": "1746193727981909180416",
          "blockTimestamp": "1684711341",
          "account": "0xfa4d3CD41555d3A0FafD4A97e9ba91882A2f4755"
        }
      ]
    },
    "dailyClaimSummaries": {
      "nodes": [
        {
          "id": "2023-05-21",
          "totalClaimed": "9223858580450949201920",
          "claims": {
            "totalCount": 2,
            "nodes": [
              {
                "id": "247333",
                "account": "0xfa4d3CD41555d3A0FafD4A97e9ba91882A2f4755",
                "value": "1746193727981909180416"
              },
              {
                "id": "129721",
                "account": "0x85399353400C5B67fD6eE53B1d2cd183bAE7dDdb",
                "value": "7477664852469040021504"
              }
            ]
          }
        }
      ]
    }
  }
}
```
