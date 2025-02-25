---
title: Continuous Integration
---

GraphQL Inspector can be used with any Continuous Integration. It's easy!

## Usage

Let's take for example Schema Diffing and assume your latest `schema.graphql` is under `master` branch:

    graphql-inspector diff 'git:origin/master:./schema.graphql' 'schema.graphql'

You can use that command in any CI, for example, in CircleCI it would look like this:

```yaml
# Inside the .circleci/config.yml file:
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:12.4.0
    steps:
      - checkout
      - run:
          name: Install
          command: yarn install
      - run:
          name: GraphQL Inspector
          command: 'graphql-inspector diff "git:origin/master:./schema.graphql" "schema.graphql"'
```

Now, when you push your changes you would see the following:

![Schema Diff in CircleCI](/img/ci/diff.jpg)

> Remember, you can use any command the CLI offers.

We recommend to automate the workflow and use `husky` to run [`$ graphql-inspector introspect`](../essentials/introspect):

```json
{
  // ...
  "scripts": {
    "graphql:dump": "graphql-inspector introspect schema.js --write schema.graphql"
  },
  "husky": {
    "hooks": {
      "pre-commit": "yarn graphql:dump && git add schema.graphql"
    }
  }
}
```
