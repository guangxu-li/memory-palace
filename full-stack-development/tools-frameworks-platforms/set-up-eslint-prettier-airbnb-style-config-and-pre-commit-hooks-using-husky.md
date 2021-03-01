# Set up ESLint, Prettier, Airbnb style config, and pre-commit Hooks using Husky

## Introduction

### ESLint and prettier

For using ESLint with prettier, there're three options as follow:

|  | [`prettier-eslint`](https://github.com/prettier/prettier-eslint) | [`eslint-plugin-prettier`](https://github.com/prettier/eslint-plugin-prettier) | [`eslint-config-prettier`](https://github.com/prettier/eslint-config-prettier) |
| :--- | :--- | :--- | :--- |
| What it is | A JavaScript module exporting a single function | An ESLint plugin. | An ESLint configuration. |
| What it does | Runs the code \(string\) through `prettier` then `eslint --fix`. The output is also a string. | Plugins usually contain implementations for additional rules that ESLint will check for. This plugin uses Prettier under the hood and will raise ESLint errors when your code differs from Prettier's expected output. | This config turns off formatting-related rules that might conflict with Prettier, allowing you to use Prettier with other ESLint configs like [`eslint-config-airbnb`](https://www.npmjs.com/package/eslint-config-airbnb). |
| How to use it | Either calling the function in your code or via [`prettier-eslint-cli`](https://github.com/prettier/prettier-eslint-cli) if you prefer the command line. | Add it to your `.eslintrc`. | Add it to your `.eslintrc`. |
| Is the final output Prettier compliant? | Depends on your ESLint config | Yes | Yes |
| Do you need to run `prettier`command separately? | No | No | Yes |
| Do you need to use anything else? | No | You may want to turn off conflicting rules using `eslint-config-prettier`. | No |

It's the recommended practice to let Prettier handle formatting and ESLint for non-formatting issues.`prettier-eslint` is not in the same direction as that practice, hence `prettier-eslint` is not recommended anymore. You can use `eslint-plugin-prettier` and `eslint-config-prettier` together.

### Husky

Husky is an NPM package that lets you run a set of commands or scripts before any git action. For eg `pre-push`, `pre-commit`, `pre-rebase`.

## Installation

### 1. Adding NPM to the project

```bash
npm init
```

Please add details about the project, and after that, a `package.json` and `package-lock.json` file will be created in your root folder.

### 2. Setting up ESLint

```bash
npm install eslint --save-dev
```

{% hint style="warning" %}
It's not recommended to install globally.
{% endhint %}

### 3. Setting up Prettier

```bash
npm install prettier --save-dev --save-exact
```

This command will install Prettier package locally. To custom rules create/edit file `.prettierrc` in the project's root directory or to ignore files create/edit file `.prettierignore`.

### 4. Install the style config

The Airbnb style guide is identified as `eslint-config-airbnb` in the NPM repository. This style guide has some peer dependencies that must be installed along with it. If you don't use React, you might want to consider installing `eslint-config-airbnb-base` which should have lesser peer dependencies.

#### 4.1 eslint-config-airbnb-base

 It requires `eslint`and `eslint-plugin-import`.

1. Install the correct versions of each package, which are listed by the command:

```bash
npm info "eslint-config-airbnb-base@latest" peerDependencies
```

If using **npm 5+**, use this shortcut

```bash
npx install-peerdeps --dev eslint-config-airbnb-base
```

If using **yarn**, you can also use the shortcut described above if you have npm 5+ installed on your machine, as the command will detect that you are using yarn and will act accordingly. Otherwise, run `npm info "eslint-config-airbnb-base@latest" peerDependencies` to list the peer dependencies and versions, then run `yarn add --dev <dependency>@<version>` for each listed peer dependency.

2. Add "extends": "airbnb-base" to `.eslintrc`.

### 5. Resolve conflicts between ESLint and Prettier

#### 5.1 Install \`eslint-config-prettier\` and \`eslint-plugin-prettier\`

```bash
npm install --save-dev eslint-config-prettier eslint-plugin-prettier
```

Then add `plugin:prettier/recommended` as the _last_ extension in your `.eslintrc.json`:

```text
{
  "extends": ["plugin:prettier/recommended"]
}
```

Exactly what does `plugin:prettier/recommended` do? Well, this is what it expands to:

```text
{
  "extends": ["prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "arrow-body-style": "off",
    "prefer-arrow-callback": "off"
  }
}
```

* `"extends": ["prettier"]` enables the config from `eslint-config-prettier`, which turns off some ESLint rules that conflict with Prettier.
* `"plugins": ["prettier"]` registers this plugin.
* `"prettier/prettier": "error"` turns on the rule provided by this plugin, which runs Prettier from within ESLint.
* `"arrow-body-style": "off"` and `"prefer-arrow-callback": "off"` turns off two ESLint core rules that unfortunately are problematic with this plugin – see the next section.

{% hint style="info" %}
Check [this](https://github.com/prettier/eslint-plugin-prettier#arrow-body-style-and-prefer-arrow-callback-issue) for `arrow-body-style` and `prefer-arrow-callback` issue.

[array-body-style and prefer-arrow-callback issues](https://github.com/prettier/eslint-plugin-prettier#arrow-body-style-and-prefer-arrow-callback-issue)
{% endhint %}

### 6. Configure ESLint and Prettier extension

1. Install ESLint and Prettier vscode extension.

2. Sample `.eslintrc.json`

```javascript
{   
    "extends": [
        "airbnb-base",
        "prettier"
    ],

    "plugins": ["prettier"],

    "rules": {
        "prettier/prettier": ["error", {}, {
            // pass eslint built-in prettier rules
            // to prettier extension rules file
            
            "usePrettierrc": true,
            "fileInfoOptions": {
                "withNodeModules": true
            }
        }]
    }
}
```

3. Add the following settings to vscode's setting file `seetings.json`.

```javascript
"[javascript]": {
    "editor.defaultFormatter": "dbaeumer.vscode-eslint"
 }
 
 "eslint.format.enable": true,
 "editor.formatOnType": true,
 "editor.codeActionsOnSave": {
     "source.fixAll.eslint": true
 }
```

### 7. Pre-Commit hooks check using Husky

Use package [`lint-staged`](https://github.com/okonet/lint-staged#examples) 

```bash
npx mrm lint-staged
```

Sample `package.json` 

```javascript
{
  "name": "My project",
  "version": "0.1.0",
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": "eslint --fix",
    "*.ts": "eslint --fix"
  }
}
```

Successful setup screenshot:

![](../../.gitbook/assets/image%20%28210%29.png)

