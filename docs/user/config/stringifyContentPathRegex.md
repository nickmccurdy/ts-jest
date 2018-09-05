---
title: Stringify content option
---

The `stringifyContentPathRegex` option has been kept for backward compatibility of `__HTML_TRANSFORM__`, and should actually be another simple Jest processor. It's a regular expression pattern used to match the path of file to be transformed. If it matches, the file will be exported as a module exporting its content.

Let's say for example that you have a file `foo.ts` which contains `export default "bar"`, and your `stringifyContentPathRegex` is set to `foo\\.ts$`, the resulting module won't be the result of compiling `foo.ts` source, but instead it'll be a module which exports the string `"export default \"bar\""`.

**CAUTION**: Whatever file(s) you want to match with `stringifyContentPathRegex` pattern, you must ensure the Jest `transform` option pointing to `ts-jest` matches them. You may also have to add the extendson(s) of this/those file(s) to `moduleFileExtensions` Jest option.

### Example:

In the `jest.config.js` version, you could do as in the `package.json` version of the config, but extending from the preset will ensure more compatiblity without any changes when updating.

<div class="row"><div class="col-md-6" markdown="block">

```js
// jest.config.js
const { jestPreset } = require('ts-jest');

module.exports = {
  // [...]
  moduleFileExtensions: [
    ...jestPreset.moduleFileExtensions,
    'html'
  ],
  transform: {
    ...jestPreset.transform,
    '\\.html$': 'ts-jest'
  }
  globals: {
    'ts-jest': {
      stringifyContentPathRegex: /\.html$/
    }
  }
};
```

</div><div class="col-md-6" markdown="block">

```js
// OR package.json
{
  // [...]
  "jest": {
    "moduleFileExtensions": ["js", "ts", "html"]
    "transform": {
      "\\.(html|ts|js)$": "ts-jest"
    }
    "globals": {
      "ts-jest": {
        "stringifyContentPathRegex": "\\.html$"
      }
    }
  }
}
```

</div></div>