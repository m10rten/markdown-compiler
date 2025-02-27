# markdown-includes

[![Bundle size](https://img.shields.io/bundlephobia/min/markdown-includes/latest?style=for-the-badge&color=3178c6)](https://bundlephobia.com/package/markdown-includes@latest)&nbsp;
[![Downloads](https://img.shields.io/npm/dt/markdown-includes?style=for-the-badge)](https://www.npmjs.com/package/markdown-includes)&nbsp;
[![License](https://img.shields.io/npm/l/markdown-includes?style=for-the-badge&color=efb103)](https://github.com/m10rten/markdown-includes/blob/main/LICENSE)&nbsp;
[![Version](https://img.shields.io/npm/v/markdown-includes?style=for-the-badge&color=cb3837&logo=npm)](https://www.npmjs.com/package/markdown-includes)&nbsp;
[![GitHub Repo stars](https://img.shields.io/github/stars/m10rten/markdown-includes?color=E9E9E9&logo=Github&style=for-the-badge)](https://www.github.com/m10rten/markdown-includes)&nbsp;
[![GitHub issues](https://img.shields.io/github/issues-raw/m10rten/markdown-includes?label=issues&style=for-the-badge)](https://www.github.com/m10rten/markdown-includes/issues)

Compile multiple Markdown files into 1, easily create a menu, remove comments and more with easy to use tags.

## Features

- ✅ Easy to use within your new or existing projects.
- ⌚ Compile multiple Markdown files into 1 with the `&|include` tag.
- 🗃️ Create a menu of contents with the `&|menu` tag.
- 🧹 Remove comments from output file with the `&|no_comments` tag.
- 📝 Create a table based on JSON with the `&|table` tag.

## Table of contents

- [Usage](#usage)
  - [Hierarchy](#hierarchy-of-options)
  - [Options](#options)
- [Tag Examples](#tag-examples)
  - [Include file](#include-file)
    - [Options](#options-1)
    - [Input](#input)
    - [Output](#output)
  - [Include menu](#include-menu)
    - [Input](#input-1)
      - [Options](#options-2)
    - [Output](#output-1)
  - [No comments in output file](#no-comments-in-output-file)
    - [Input](#input-2)
    - [Output](#output-2)
  - [Table](#table)
    - [Input](#input-3)
      - [Options](#options-3)
    - [Output](#output-3)
- [API Usage](#api-usage)
  - [Configuration](#configuration)
  - [Methods](#methods)
- [Contributing](#contributing)
- [License](#license)
- [Authors](#authors)

## Usage

1. Create a markdown file with any name, e.g. `index.md`.
2. Install the compiler globally or locally in your project:

   ```bash
   # npm
   npm install -g markdown-includes

   # or yarn
   yarn global add markdown-includes

   # or pnpm
   pnpm install -g markdown-includes
   ```

   Or install it locally in your project:

   ```bash
   # npm
   npm install markdown-includes
   ```

3. Choose your method to run the compiler: CLI (as shown in this chapter) or [API](#api-usage).

   ```bash
   mdi <input file> [options]
   ```

   Or if you installed it locally:

   ```bash
   npx mdi <input file> [options]
   ```

   **Multiple files**
   When you want to compile multiple files, use the `*` as the `<input file>`, like this: `mdi ./*`. <br>
   It will compile all files in the current directory specified: `mdi examples/*` will compile all inside examples.

   > ⚠️ The patterns are from [`glob`](https://npmjs.com/package/glob) on npm.

### Hierarchy of options

The options are set with the following order of priority: <br>
_CLI arguments > Config file > Tag options > System default_

### Options

- `--debug`: See what is logged to the console.
- `--out <file>`: Specify the output file. If not specified, the output will be written to the console.
- `--version` | `-v`: Show the version number.
- `--help` | `-h`: Show the help.
- `--watch` | `-w`: Watch the input file for changes and recompile when it changes.
- `--menu-depth <number>`: Specify the default system depth of the menu. System default is `3`.
- `--no-comments`: Remove comments from the output file. System default is `false`.
- `--root <path>`: Specify the root folder. System default is the current working directory.
- `--config <path>` | `-c`: Specify the config file. System default is `mdi.config.js` in the current working directory.
- `--extensions <list>` Set the extensions to include. (default: `.md,.mdx`)
- `--ignore <list>`: Set the folders to ignore. (default: `node_modules,.git`)

## Tag Examples

### Include file

#### Options

- `&|include include.md`: Include a file.
- `` &|include `chapters/**/*`  ``: Include all files in the `chapters` folder and all subfolders, due to markdown syntax, you need to use backticks to wrap the pattern.

#### Input

```markdown
# Title

&|include include.md
```

#### Output

```markdown
# Title

This is the included file.
```

### Include menu

Easily create a menu of contents with the `&|menu` tag. The menu will be created based on the headings in the document, at the location of the tag.

#### Input

```markdown
# Document

&|menu

## 1. Test

### Sub item
```

##### Options

- `&|menu 3`: Specify the depth of the menu in a specific document. Document default is `3`.

#### Output

```markdown
# Document

- [Document](#document)
  - [1. Test](#1-test)
    - [Sub item](#sub-item)

## Test

### Sub item
```

### No comments in output file

No more removing comments, just use this tag and all comments (inline, single or multi-line) will be stripped from the output.

#### Input

```markdown
&|no_comments

## Markdown test

Lorum ipsum

<!-- this is a comment -->
```

#### Output

```markdown
## Markdown test

Lorum ipsum
```

### Table

Use the `&|table` tag to include a table from a JSON file.

For example, if you have a JSON file with the following data:

```json
[
	{
		"Name": "John",
		"Age": 20
	},
	{
		"Name": "Jane",
		"Age": 21
	}
]
```

#### Options

- `&|table <path> <keys?>`: Specify the keys to include in the table. If no keys are specified, all keys will be included.
  > ⚠️ The keys are case sensitive.

#### Input

```markdown
&|table data.json
```

#### Output

```markdown
| Name | Age |
| ---- | --- |
| John | 20  |
| Jane | 21  |
| ...  | ... |
```

## API Usage

### Configuration

When you want to use the API, you can use the default exported class to create a new compiler instance.

```js
const create = require("markdown-includes");

const config = {
	debug: true, // log actions to the console
	output: "./out", // output directory
	menuDepth: 3, // default menu depth
	noComments: false, // remove comments from output
	root: "./", // root directory
	path: "path-to-file.md", // path to file
	extensions: [".md", ".mdx"], // file-extensions to include
	ignore: ["node_modules", ".git"], // folders to ignore
};

const compiler = await create(config);

// will use the path from the config, or path specified as an argument.
compiler.compile();
```

Or TypeScript:

```ts
import create, { Config } from "markdown-includes";

const config: Config = {
  ...
};

const compiler = await create(config);

compiler.compile();
```

The compiler can also read the config from a file. The config file should export the config object.

```js
import create from "markdown-includes";

const compiler = await create({ config: "./path-to-config.js" });

compiler.compile();
```

### Methods

- `compile(path?: string) => Promise<void>`: Compile the file.
  - `path`: The path to the file. Uses the `path` from the config if not specified.
- `watch(path?: string, debug?: boolean)`: Watch the file for changes and recompile when it changes. Uses [`node-watch`](https://npmjs.com/package/node-watch) on npm.
  - `path`: The path to the file.
  - `debug`: Whether to log the actions to the console.
- `table(content: string, columns?: string[], debug?: boolean) => Promise<string[]>`)
  - `content`: The content of the file.
  - `columns`: The columns to include in the table.
  - `debug`: Whether to log the actions to the console.
- `parse(content: string, dir: string, menuDepth: number, noComments: boolean, debug?: boolean) => Promise<string>`: Parse the content of the file.
  - `content`: The content of the file.
  - `dir`: The directory of the file. Used to resolve recursive includes.
  - `menuDepth`: The depth of the menu.
  - `noComments`: Whether to remove comments from the output.
  - `debug`: Whether to log the actions to the console.

## Contributing

Contributions are welcome! Please open an issue or a pull request if you want to contribute.

**Guidelines**:

- Use `npm` to install dependencies.
- Use `npm run build` to build the project.
<!-- - Use `npm run test` to run the tests. -->
- Use `npm run lint` to lint the project.
- Files should be formatted with `prettier` according to the `.prettierrc.json` file.
- Files should be linted with `eslint` according to the `.eslintrc.json` file.
- Pull requests should be made to the `dev` branch.
- Pull requests should be made with a description of the changes.
- Pull requests have according branches, for example: `feature/feature-name` or `fix/fix-name`.

When ready to push a feature or fix, please make sure to run `npm run build` and `npm run lint` before pushing.

Guidelines are not set in stone, so if you have a better idea, please open an issue or a pull request.

## License

MIT

## Authors

[m10rten](https://github.com/m10rten)
