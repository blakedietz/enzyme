# 2019-04-10

## Migration guide notes

### enzyme

#### shallow

Version 3 of enzyme has changed the functionality of `shallow`. The short of it is that `shallow` in v2 would not trigger lifecycle methods such as `componentDidMount`. Now in v3 React lifecycle methods are run by default. This caused breakages in tests as new logic was being executed when migrating from v2 to v3. There was an opt out mechanism that was added. The opt out looks like the After snippet in the below example.

```jsx
// Before
import { shallow } from "enzyme";

// ...

const wrapper = shallow(<Component />);

// After
import { shallow } from "enzyme";

// ...

const wrapper = shallow(<Component />, { disableLifecycleMethods: true });
```

Continuing forward all new usages of shallow should **not** include the opt out parameters.

### JSS configurations

### extend

The jss-extend utility was removed in the migration from material-ui and no longer falls `extend:`. Just search for the string `extend:`. If you find references to it, replace the usages with the object spread operator.[<sub>1</sub>][jss-utils]

```javascript
// Before
const styles = theme => ({
  asuh: {
    extend: someOtherStuff,
    background: "red"
  }
});

// After
const styles = theme => ({
  asuh: {
    ...someOtherStuff,
    background: "red"
  }
});
```

### composes

For this specifically look for `padding:` or `margin:`. If you find that there is an array as a set of properties such as the following

```javascript
// before
const styles = theme => ({
  asuh: {
    padding: [0, 1, 2, 3],
    margin: [0, 1, 2, 3],
    background: "red"
  }
});

// after
const styles = theme => ({
  asuh: {
    // Or even better use theme.pacing.unit * x for spacing computations
    padding: "0px 1px 2px 3px",
    margin: "0px 1px 2px 3px",
    background: "red"
  }
});
```

### jssToot to componentFactory

Make sure to update the project's `index.js` to utilize `componentFactory` instead of `jssRoot`. This is important for two reasons.

`componentFactory` has some added benefits. First it includes error handling logic if the dom node that is attempting to be mounted to does not exist. Second it also automatically includes the `MuiThemeProvider` in the tree which is important as this is how all imported `tsheets-ui` components receive their themeing.

### Node version

#### .nvmrc

Due to some changes with polyfills in babel transform runtime, promise polyfills are no longer included, thus it's important to update node to the latest LTS version. So a polyfill is not needed at test runtime. These polyfills are included already in the application.

Update .nvmrc to the following

```
v10.15.3
```

You'll see errors like the following if this is the case

```bash
finally is not a function
```

#### README.md

While you're at it, make sure to update the README.md too.

#### .circleci/config.yml

Make sure to update your docker images too.

## References

[jss-utils]: https://github.com/mui-org/material-ui/pull/9129
[enzyme-shallow]: https://github.com/airbnb/enzyme/blob/master/docs/guides/migration-from-2-to-3.md#lifecycle-methods

1. [Removal of extends, composes and some others][jss-utils]
2. [Lifecycle methods][enzyme-shallow]

<!--@nested-tags:tsheets/2019/04/10-->
