# ESLint JSON Language Plugin

## Overview

This package contains a plugin that allows you to natively lint JSON and JSONC files using ESLint.

**Important:** This plugin requires ESLint v9.6.0 or higher and you must be using the [new configuration system](https://eslint.org/docs/latest/use/configure/configuration-files).

## Installation

For Node.js and compatible runtimes:

```shell
npm install @eslint/json -D
# or
yarn add @eslint/json -D
# or
pnpm install @eslint/json -D
# or
bun install @eslint/json -D
```

For Deno:

```shell
deno add @eslint/json
```

## Usage

This package exports these languages:

-   `"json/json"` is for regular JSON files
-   `"json/jsonc"` is for JSON files that support comments ([JSONC](https://github.com/microsoft/node-jsonc-parser)) such as those used for Visual Studio Code configuration files
-   `"json/json5"` is for [JSON5](https://json5.org) files

Depending on which types of JSON files you'd like to lint, you can set up your `eslint.config.js` file to include just the files you'd like. Here's an example that lints JSON, JSONC, and JSON5 files:

```js
import json from "@eslint/json";

export default [
	{
		plugins: {
			json,
		},
	},

	// lint JSON files
	{
		files: ["**/*.json"],
		language: "json/json",
		rules: {
			"json/no-duplicate-keys": "error",
		},
	},

	// lint JSONC files
	{
		files: ["**/*.jsonc", ".vscode/*.json"],
		language: "json/jsonc",
		rules: {
			"json/no-duplicate-keys": "error",
		},
	},

	// lint JSON5 files
	{
		files: ["**/*.json5"],
		language: "json/json5",
		rules: {
			"json/no-duplicate-keys": "error",
		},
	},
];
```

In CommonJS format:

```js
const json = require("@eslint/json").default;

module.exports = [
	{
		plugins: {
			json,
		},
	},

	// lint JSON files
	{
		files: ["**/*.json"],
		language: "json/json",
		rules: {
			"json/no-duplicate-keys": "error",
		},
	},

	// lint JSONC files
	{
		files: ["**/*.jsonc", ".vscode/*.json"],
		language: "json/jsonc",
		rules: {
			"json/no-duplicate-keys": "error",
		},
	},

	// lint JSON5 files
	{
		files: ["**/*.json5"],
		language: "json/json5",
		rules: {
			"json/no-duplicate-keys": "error",
		},
	},
];
```

## Recommended Configuration

To use the recommended configuration for this plugin, specify your matching `files` and then use the `json.configs.recommended` object, like this:

```js
import json from "@eslint/json";

export default [
	// lint JSON files
	{
		files: ["**/*.json"],
		ignores: ["package-lock.json"],
		language: "json/json",
		...json.configs.recommended,
	},

	// lint JSONC files
	{
		files: ["**/*.jsonc"],
		language: "json/jsonc",
		...json.configs.recommended,
	},

	// lint JSON5 files
	{
		files: ["**/*.json5"],
		language: "json/json5",
		...json.configs.recommended,
	},
];
```

**Note:** You generally want to ignore `package-lock.json` because it is auto-generated and you typically will not want to manually make changes to it.

## Rules

-   `no-duplicate-keys` - warns when there are two keys in an object with the same text.
-   `no-empty-keys` - warns when there is a key in an object that is an empty string or contains only whitespace (note: `package-lock.json` uses empty keys intentionally)

## Configuration Comments

In JSONC and JSON5 files, you can also use [rule configurations comments](https://eslint.org/docs/latest/use/configure/rules#using-configuration-comments) and [disable directives](https://eslint.org/docs/latest/use/configure/rules#disabling-rules).

```jsonc
/* eslint json/no-empty-keys: "error" */

{
	"foo": {
		"": 1, // eslint-disable-line json/no-empty-keys -- We want an empty key here
	},
	"bar": {
		// eslint-disable-next-line json/no-empty-keys -- We want an empty key here too
		"": 2,
	},
	/* eslint-disable json/no-empty-keys -- Empty keys are allowed in the following code as well */
	"baz": [
		{
			"": 3,
		},
		{
			"": 4,
		},
	],
	/* eslint-enable json/no-empty-keys -- re-enable now */
}
```

Both line and block comments can be used for all kinds of configuration comments.

## Frequently Asked Questions

### How does this relate to `eslint-plugin-json` and `eslint-plugin-jsonc`?

This plugin implements JSON parsing for ESLint using the language plugins API, which is the official way of supporting non-JavaScript languages in ESLint. This differs from the other plugins:

-   `eslint-plugin-json` uses a processor to parse the JSON, meaning it doesn't create an AST and you can't write custom rules for it.
-   `eslint-plugin-jsonc` uses a parser that still goes through the JavaScript linting functionality and requires several rules to disallow valid JavaScript syntax that is invalid in JSON.

As such, this plugin is more robust and faster than the others. You can write your own custom rules when using the languages in this plugin, too.

### What about missing rules that are available in `eslint-plugin-json` and `eslint-plugin-jsonc`?

Most of the rules in `eslint-plugin-json` are actually syntax errors that are caught automatically by the parser used in this plugin.

Similarly, many of the rules in `eslint-plugin-jsonc` specifically disallow valid JavaScript syntax that is invalid in the context of JSON. These are also automatically caught by the parser in this plugin.

Any other rules that catch potential problems in JSON are welcome to be implemented. You can [open an issue](https://github.com/eslint/json/issues/new/choose) to propose a new rule.

## License

Apache 2.0

## Sponsors

<!-- NOTE: This section is autogenerated. Do not manually edit.-->
<!--sponsorsstart-->
<!--sponsorsend-->

<!--techsponsorsstart-->
<!--techsponsorsend-->
