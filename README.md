# Engineering Handbook

Approach for overall alignment of engineering in the unchained community

## Libraries

- Date Manipulation: [date-fns](https://date-fns.org)
- Form Handling: [react-hook-form](https://react-hook-form.com) ([discussion](https://github.com/unchainedshop/engineering-handbook/issues/1))
- Internationalization:[react-intl](https://formatjs.io)

## Internationalization / Localization

Always use (upcoming) standards wherever possible:
https://www.ecma-international.org/ecma-402/
http://cldr.unicode.org
http://userguide.icu-project.org/formatparse/messages

The formatjs library is home to various polyfill regarding the topic and also develops the react-intl library:
https://formatjs.io/docs/intl-messageformat

Implementation Examples:

- Do use https://formatjs.io/docs/polyfills/intl-numberformat to format currencies, don't use custom currency formatting functions.

## Configuration

Please don't invent configuration files (.rc, .config.json, or equivalent). All environment specific configuration should be stored in environment variables (https://12factor.net)

And especially for Node.js: Don't read process.env in functions, always load it at root level of a file because reading from process.env is blocking: 
- https://stackoverflow.com/questions/45789919/is-reading-and-writing-process-env-values-synchronous#48828102
- https://github.com/nodejs/node/issues/3104

So instead of:

```
  function() {
     // read
     if (process.env.NODE_ENV...
  }
```

Do:

```

import ...

const { NODE_ENV } = process.env;

  function() {
     // read
     if (NODE_ENV...
  }

```
## Naming

### Avoid ambiguous plurals

If you have to name variables/queries that are in a collection, add -list -detail for clarity.

Example:
```js
// Good

const productDetail = {};
const productList = [];


// Bad
const product = {};
const products = [];
```

## Patterns/Snippets

### Mutation Hooks

- Replace XXX with the PascalCased mutation name
- Replace xxx with the camelCased mutation name

```tsx
// filename: useXXX.tsx

import gql from "graphql-tag";
import { useMutation } from "@apollo/react-hooks";

const XXXMutation = gql`
  mutation XXX($input: String!) {
    xxx(input: $input) {
      result
    }
  }
`;

const useXXX = () => {
  const [xxxMutation] = useMutation(XXXMutation);

  const xxx = async ({ input }) => {
    // Do some preparations

    await xxxMutation({
      variables: { input }
    });
  };

  return {
    xxx
  };
};

export default useXXX;
```

### Query Hooks

- Replace XXX with the PascalCased mutation name
- Replace xxx with the camelCased mutation name

```tsx
// filename: useXXX.tsx

import { useQuery } from 'react-apollo';
import gql from 'graphql-tag';

const XXXQuery = gql`
  query XXX {
    xxx {
      result
    }
  }
`;

const useXXX = () => {
  const { loading, error, data } = useQuery(XXXQuery);

  const xxx = data?.xxx;

  return {
    loading,
    error,
    xxx,
  };
};

export default useXXX;
```

## Tooling

### Eslint / Prettier

Using a certain Eslint / Prettier Configuration is mainly up to the community and can be different for different repositories.

Of course, Prettier sees itself as a tool in it's own right outlined here (https://prettier.io/docs/en/integrating-with-linters.html) but we think it's exactly the opposite and it should be part of linting, that's why we usually run Prettier through Eslint as Plugin. This is our minimal suggested configuration:

1. Eslint as tool of choice configured through .eslintrc and .editorconfig
2. Use eslint-plugin-prettier through Eslint to format code (https://github.com/prettier/eslint-plugin-prettier)
3. Use typescript-eslint Eslint Plugin to lint typescript (https://github.com/typescript-eslint/typescript-eslint#what-about-tslint)
4. We extend the rules with airbnb-base (https://www.npmjs.com/package/eslint-config-airbnb-base), plugin:prettier/recommended (https://github.com/prettier/eslint-plugin-prettier) and plugin:@typescript-eslint/recommended (https://github.com/typescript-eslint/typescript-eslint)
5. Configure plugins through .eslintrc and not through own config files like for example .prettierrc
6. We do not depend on any global installation of Eslint or Prettier, all packages regarding linting have to be part of the devDependencies in package.json

Example: https://github.com/unchainedshop/unchained/blob/master/.eslintrc

Some Editors like Atom and Visual Studio Code let's you install Prettier and Eslint Plugins. Please make sure that your IDE is configured in a way that it respects the linting configuration of the author. This can be done by meeting these two requirements:

1. Only Enable Prettier when there is a .prettierrc File in the project
2. Only Enable Eslint when there is a .eslintrc File in the project
3. Enable Fix on Save for Formatters (Eslint, Prettier)
4. Only enable Prettier or Eslint when the node_modules needed are installed locally, never let it use global installations

**Visual Studio Config**

Instructions to comply with VSCode:

- Uninstall (or disable for workspace) the Prettier Extension
- Install (or enable for workspace) the Eslint Extension
- Then make sure that you have these two configs set:

```
"editor.formatOnSave": false,    
"editor.codeActionsOnSave": {        
  "source.fixAll": true    
},
```

Yes, `editor.formatOnSave` should be set to false.

### Git

Write commit messages in plain english, present imperative tense, starting with an uppercase letter. Generally, follow this guideline: https://chris.beams.io/posts/git-commit/
