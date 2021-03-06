# ngx-semantic-version

[![npm](https://img.shields.io/npm/v/ngx-semantic-version.svg)](https://www.npmjs.com/package/ngx-semantic-version)
[![Dependency Status](https://david-dm.org/d-koppenhagen/ngx-semantic-version.svg)](https://david-dm.org/d-koppenhagen/ngx-semantic-version)
[![devDependency Status](https://david-dm.org/d-koppenhagen/ngx-semantic-version/dev-status.svg)](https://david-dm.org/d-koppenhagen/ngx-semantic-version?type=dev)
[![npm](https://img.shields.io/npm/l/ngx-semantic-version.svg)](https://www.npmjs.com/package/ngx-semantic-version)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Open Source Love](https://badges.frapsoft.com/os/v1/open-source.svg?v=102)](https://github.com/ellerbrock/open-source-badge/)

[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg?style=flat-square)](https://github.com/semantic-release/semantic-release)

[![npm](https://img.shields.io/npm/dw/ngx-semantic-version.svg)](https://www.npmjs.com/package/ngx-semantic-version)
[![npm](https://img.shields.io/npm/dm/ngx-semantic-version.svg)](https://www.npmjs.com/package/ngx-semantic-version)
[![npm](https://img.shields.io/npm/dy/ngx-semantic-version.svg)](https://www.npmjs.com/package/ngx-semantic-version)
[![npm](https://img.shields.io/npm/dt/ngx-semantic-version.svg)](https://www.npmjs.com/package/ngx-semantic-version)

[![GitHub forks](https://img.shields.io/github/forks/d-koppenhagen/ngx-semantic-version.svg?style=social&label=Fork)](https://github.com/d-koppenhagen/ngx-semantic-version/fork) [![GitHub stars](https://img.shields.io/github/stars/d-koppenhagen/ngx-semantic-version.svg?style=social&label=Star)](https://github.com/d-koppenhagen/ngx-semantic-version)

_ngx-semantic-version_ is an Angular Schematic that will add and configure _commitlint_, _commitizen_, _husky_ and _standard-version_ for creating commit messages in the _conventional commit_ format and automate your release and Changelog generation respecting _semver_.

The schematic will configure the following packages / services:

- [commitlint](https://commitlint.js.org)
- [husky](https://www.npmjs.com/package/husky)
- [commitizen](https://www.npmjs.com/package/commitizen)
- [standard-version](https://www.npmjs.com/package/standard-version)

## How to use

### Add the schematics to your project

Just run the following command:

```sh
ng add ngx-semantic-version
```

> if you have already configured one of the modules and you want to use the configuration provided
by ngx-semantic-version, you can use `--force` to override an existing configuration. Please check
the changes carefully using git after running with `--force`.

#### available options

| Flag                      | Description                                                        |
| ------------------------- | ------------------------------------------------------------------ |
| `--skipInstall`           | Skips installing new `node_modules` after modifying `package.json` |
| `--husky=false`           | Skips adding `husky` to the project                                |
| `--commitizen=false`      | Skips adding `commitizen` to the project                           |
| `--standardVersion=false` | Skips adding `standard-version` to the project                     |
| `--issuePrefix="<PREFIX>"`| Configure an issue prefix that should be checked by each commit    |
| `--force`                 | Override existing configurations if neccessary                     |

#### force including references by configuring an issue prefix

When adding the schematic using e.g. `--issuePrefix="PREFIX-"`, commitlint will be configured to use
the defined issue prefix in commit messages. Therefore the following configuration will be added
to the `commitlint.config.js` configuration file:

```js
module.exports = {
  // ...
  rules: {
    'references-empty': [2, 'never'],
  },
  parserPreset: {
    parserOpts: {
      issuePrefixes: ['PREFIX-'],
    },
  },
};
```

This is very helpful if you want to force the users to include always an reference to your issue
tracking system (in the example above the issue racking system will use this style: `PREFIX-1242`).

> The line `'references-empty': [2, 'never'],` will tell commitlint that an issue reference has
to be included always. You can change the value of `2` to `1` to just warn the user instead of
rejecting the commit messages. All configuration option are described in the official
[docs of commitlint](https://commitlint.js.org/#/reference-rules).

The prefix will be also configured for usage within _standard-version_ in your `package.json`:

```json
// ...
"standard-version": {
  "issuePrefixes": ["PREFIX-"],
}
```

> [You can specify further options for _standard-version_](https://github.com/conventional-changelog/conventional-changelog-config-spec/blob/master/versions/2.1.0/README.md
) that will be uses for the generated links
in `CHANGELOG.md` generation. You can adjust the configuration block `standard-version` in your
`package.json` and adjusts the options to satisfy your needs.

### Update the schematics

Run `ng update` to see if an update is available.
To proceed running an update of _ngx-semantic-version_, run the following command:

```bash
ng update ngx-semantic-version
```

> Updates will may touch your existing configuration. Please check the changes using git to verify the changes.

## What's included

### [commitlint](https://commitlint.js.org)

Commitlint will lint your commit message and check it against some common rules.
It will throw an error if the messages doesn't match with the rules.
This schematic will install the ruleset [`@commitlint/config-conventional`](https://npmjs.com/package/@commitlint/config-conventional) by default.

After adding the schematics you will be able to adjust the rules for your
personal needs by adjusting the added file `commitlint.config.js`.

You can find a description of supported adjustments in the
[official documentation](https://commitlint.js.org/#/reference-rules).

![commitizen cli](https://raw.githubusercontent.com/d-koppenhagen/ngx-semantic-version/master/assets/commitlint.svg?sanitize=true)

### [husky](https://www.npmjs.com/package/husky)

Husky allows us to hook into the git lifecycle using nodejs. It is used by _ngx-semantic-version_ to check a commit message right before storing it by using _commitlint_.
Therefore it will add this part to your `package.json`:

```json
...
"husky": {
  "hooks": {
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }
},
```

Husky uses the environment variable `HUSKY_GIT_PARAMS` containing the current git message you entered and it will pass this through _commitlint_ so it can be evaluated.

### [commitizen](https://www.npmjs.com/package/commitizen)

When having restrictions within the commit message text it can be difficult
to always satisfy the appropriate format. Commitizen will help you to build a
commit message always in the appropriate format by letting you configure the
final message via an interactive cli.

When _commitizen_ is installed globally (`npm i -g commitizen`) you can run it by
executing `git cz`.

![commitizen cli](https://raw.githubusercontent.com/d-koppenhagen/ngx-semantic-version/master/assets/commitizen.svg?sanitize=true)

Alternatively, if you are using NPM 5.2+ you can use npx instead of installing globally: `npx git-cz`.

_ngx-semantic-version_ will configure _commitizen_ in your `package.json`, so that is will use the _conventional changelog_ as well:

```json
...
"config": {
  "commitizen": {
    "path": "./node_modules/cz-conventional-changelog"
  }
}
```

> Tip: if you are using vscode, you can add the plugin [Visual Studio Code Commitizen Support](https://marketplace.visualstudio.com/items?itemName=KnisterPeter.vscode-commitizen) which will let you build the commit message directly via vscode.
> ![commitizen vscode plugin](https://raw.githubusercontent.com/d-koppenhagen/ngx-semantic-version/master/assets/commitizen-vscode.png)

### [standard-version](https://www.npmjs.com/package/standard-version)

Standard-version will create and update your app/package version and automatically
generate a `CHANGELOG.md` file and keep it up-to-date by using the git history.
It will check the messages for keywords (thanks to commitlint) and determine which part
of the semantic version will be increased. Furthermore it will add a tag for the version.

After adding this schematic, you can simply release a new version by running `npm run release`.
This will keep your `CHNAGELOG.md` up to date and releases a new version of your project.

_ngx-semantic-version_ will configure a new script in your `package.json` that can be used for releasing a new version:

```json
...
"scripts": {
  "release": "standard-version",
},
```

If you typically use `npm version` to cut a new release, do this instead:

```bash
npm run release
```

You should also consider use one of the following commands:

```bash
npm run release -- --first-release    # create the initial release and create the `CHANGELOG.md`
npm run release -- --prerelease       # create a pre-release instead of a regular one
npm run release -- --prerelease alpha # cut a new alpha release version
```

Check out the [official documentation](https://www.npmjs.com/package/standard-version#release-as-a-pre-release) for further information.

## Development

For development hints, have a look at [DEVELOPMENT.md](./DEVELOPMENT.md)
