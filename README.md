# @betty-blocks/utils-graphql

> GraphQL utilities

<!-- START doctoc -->

<!-- END doctoc -->

## Installation

### Using [npm](https://docs.npmjs.com/cli/npm)

    $ npm install --save @betty-blocks/utils-graphql

### Using [yarn](https://yarnpkg.com)

    $ yarn add @betty-blocks/utils-graphql

## Types

```javascript
export type {DocumentNode} from "graphql/language/ast";

type GqlErrorLocation = {|
  line: number,
  column: number
|};

type GqlError = {|
  message: string,
  locations?: Array<GqlErrorLocation>
|};

type GqlRequest<Variables: void | Object = void> = {|
  operation: string,
  variables?: Variables
|};

type GqlRequestCompat<Variables: void | Object = void> = {|
  query: string,
  variables?: Variables
|};

type GqlResponse<Data> = {|
  data?: Data,
  errors?: Array<GqlError>
|};

type GqlOperationType = "mutation" | "query" | "subscription";
```

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### errorsToString

Transforms an array of GqlError into a string.

#### Parameters

-   `gqlErrors` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;GqlError>** 

#### Examples

```javascript
const gqlRespose = {
  errors: [
    {message: "First Error", locations: [{column: 10, line: 2}]},
    {message: "Second Error", locations: [{column: 2, line: 4}]}
  ]
}

const error = errorsToString(gqlRespose.errors);
// string with the following:
// First Error (2:10)
// Second Error (4:2)
```

Returns **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### getOperationType

Returns the type (query, mutation, or subscription) of the given operation

#### Parameters

-   `operation` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** 

#### Examples

```javascript
const operation = `
  subscription userSubscription($userId: ID!) {
    user(userId: $userId) {
      id
      name
    }
  }
`;

const operationType = getOperationType(operation);

console.log(operationType); // "subscription"
```

Returns **GqlOperationType** 

### hasSubscription

Returns true if documentNode has a subscription or false otherwise

#### Parameters

-   `documentNode` **DocumentNode** 

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** 

### requestFromCompat

Creates a GqlRequest using given GqlRequestCompat

#### Parameters

-   `gqlRequestCompat` **GqlRequestCompat&lt;Variables>** 
    -   `gqlRequestCompat.query`  
    -   `gqlRequestCompat.variables`  

#### Examples

```javascript
const query = `
  query userQuery($userId: ID!) {
    user(userId: $userId) {
      id
      email
    }
  }
`;

console.log(requestFromCompat({query, variables: {userId: 10}}));
// {operation: "...", variables: {userId: 10}}
```

Returns **GqlRequest&lt;Variables>** 

### requestToCompat

Creates a GqlRequest using given GqlRequestCompat

#### Parameters

-   `gqlRequest` **GqlRequest&lt;Variables>** 
    -   `gqlRequest.operation`  
    -   `gqlRequest.variables`  

#### Examples

```javascript
const operation = `
  query userQuery($userId: ID!) {
    user(userId: $userId) {
      id
      email
    }
  }
`;

console.log(requestToCompat({operation, variables: {userId: 10}}));
// {query: "...", variables: {userId: 10}}
```

Returns **GqlRequestCompat&lt;Variables>** 

## License

[MIT](LICENSE.txt) :copyright: **Jumpn Limited**
