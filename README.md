# graphql-list-fields

This is a fork of: [jakepusateri/graphql-list-field](https://github.com/jakepusateri/graphql-list-fields) which adds named-inline-fragements (see description below). If the [PR](https://github.com/jakepusateri/graphql-list-fields/pull/15) is merged into upstream, we'll get rid of the fork again.

When implementing a GraphQL server, it can be useful to know the list of fields being queried on
a given type. This module takes a GraphQLResolveInfo object and returns a list of fields.

Supported features
- Basic Fields
- Fragments
- Inline Fragments
- `@skip` and `@include` directives
- Nested fields into dot.notation

```
npm install --save graphql-list-fields
```

## Usage
```javascript
import getFieldNames from 'graphql-list-fields';

// in some resolve function
resolve(parent, args, context, info) {
    const fields = getFieldNames(info);
    return fetch('/someservice/?fields=' + fields.join(','));
}
```

### named inline fragments
When supplying `true` as the second argument (`getFieldList(ast, true)`), the type of a spread is included in the path:
```
{
  someType {
    e {
      ... on NestedType {
        x
      }
    }
  }
}
```
results in: `[e.NestedType.x]` instead of `[e.x]`
