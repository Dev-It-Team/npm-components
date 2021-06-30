# npm-components
npm packages for the organization.

Packages are published to npm under the organization devitteam

See : https://www.npmjs.com/settings/devitteam/packages

You must be a member of the organization in order to publish a package.

# Steps :
0. Connect to your npm account : `npm adduser`
1. Build component : `npm build-library` (see existing samples in `package.json`, update the version if neededd)
2. npm publish

# Use the components
`npm install @devitteam/component-name`
Then in Vue projects :
```js
import ComponentName from '@devitteam/component-name
...
    components: {
        ...,
        ComponentName
    },
...
```
