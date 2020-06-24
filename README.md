# Engineering Handbook

Approach for overall alignment of engineering in the unchained community

## Libraries

- Date Manipulation: [date-fns](https://date-fns.org)
- Form Handling: [react-hook-form](https://react-hook-form.com) ([discussion](https://github.com/unchainedshop/engineering-handbook/issues/1))
- Internationalization:[react-intl](https://formatjs.io)

## Internationalization / Localization

Always use (upcoming) standards wherever possible:
https://www.ecma-international.org/ecma-402/
http://cldr.unicode.org
http://userguide.icu-project.org/formatparse/messages

The formatjs library is home to various polyfill regarding the topic and also develops the react-intl library:
https://formatjs.io/docs/intl-messageformat

Implementation Examples:

- Do use https://formatjs.io/docs/polyfills/intl-numberformat to format currencies, don't use custom currency formatting functions.

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
