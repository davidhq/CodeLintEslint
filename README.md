## Very centralized setup for very pretty JavaScript

### Disclosure:

Author of this composite package only provided some glue in a few hours / now turning into days (a lot of experimentation, blog posts and dead ends...), this is hard work of many people, in total 8145+ files here... and a few days of gluing it all together in a non-fragile and efficient way.

"__Centralized__" means: on your own machine... one true source of what your JavaScript should look like ... before tackling this kind of new setup, I had config files and libraries all around my computer and I could not implement any changes or upgrades to the system (like adding `Svelte` support) **at all**. I barely got enough willpower to figure out how I have set up this thing one year ago, just to dismantle it and create this package which is clear, future-proof and should work as expected.

## Now back to the real thing:

# Very centralized setup for very pretty JavaScript 🚀

Goals:

To establish two aspects:

- linting (showing visually where code could be better) (via [ESLint](https://eslint.org), of course)
- rewriting code by pre-programmed standards (via [Prettier](https://prettier.io), of course)

For this code:

- Modern JavaScript (nodejs, browser)
- Svelte components
- Other frontend JS frameworks as well


## Setup

### Clone this repo

```bash
mkdir ~/Misc
cd ~/Misc
git clone https://github.com/davidhq/CodeLintEslint.git
```

### Install these Sublime Packages via SublimeText **Package Control**:

- [SublimeLinter](https://github.com/SublimeLinter/SublimeLinter)
- [SublimeLinter-eslint](https://github.com/SublimeLinter/SublimeLinter-eslint)
- [Svelte Syntax Highlighting](https://github.com/corneliusio/svelte-sublime)

Install this one manually:

```bash
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages
git clone https://github.com/davidhq/SublimeJsPrettier.git
cd SublimeJsPrettier
```

Do this:

```
git checkout 7442289bd489eb4360659c5bc22409d563169c90
```

... for now because master is still broken! -- See Problem 1


### Configure packages:

Open your SublimeText editor, then follow in the menu:

```
MENU: SublimeText (BOLD, left top corner) / Preferences / Package Settings / SublimeLinter / Settings
```

Paste in right window (or just add the bold key - args):

```
{
    // Linter specific settings.
    // More info: http://www.sublimelinter.com/en/stable/linter_settings.html
    // Linter specific settings except for "styles" can also be changed
    // in sublime-project settings.
    // What settings are available is documented in the readme of the
    // specific linter plugin.
    // Example:
    "linters": {
        // The name of the linter you installed
        "eslint": {
            "excludes": ["*/*.html"],
            "args": ["-c", "~/Misc/CodeLintEslint/.eslintrc.json",
                     "--resolve-plugins-relative-to", "~/Misc/CodeLintEslint"],
            "selector": "text.html.svelte, source.svelte - meta.attribute-with-value"
        }
  }
}
```

Open your SublimeText editor, then follow in the menu:

```
MENU: SublimeText (BOLD, left top corner) / Preferences / Settings (or OPTION+,)
```

Update or add (in right window) the key "js_prettier":

```json
"js_prettier":
  {
    "additional_cli_args":
    {
      "--config": "~/Misc/CodeLintEslint/prettier.config.js",
      "--plugin-search-dir": "~/Misc/CodeLintEslint"
    },
    "allow_inline_formatting": false,
    "auto_format_on_save": true,
    "auto_format_on_save_excludes":
    [
      "*.md"
    ],
    "debug": false,
    "node_path": "",
    "prettier_cli_path": "~/Misc/CodeLintEslint/node_modules/prettier/bin-prettier.js"
  },
```

### Create symlinks for calling prettier and eslint from command-line-interface

```bash
cd ~/bin

ln -s ~/Misc/CodeLintEslint/node_modules/eslint/bin/eslint.js eslint
ln -s ~/Misc/CodeLintEslint/node_modules/prettier/bin-prettier.js prettier
```


## Currently open problems:

(Welcome to help solve! ... but not neccessary, **will be done** no matter what ⚔️)

### Problem #1

Cannot make `.svelte` files prettier from **SublimeText** 😭

Manual `pretty` command like this works:

```bash
function pretty {
  ~/Misc/CodeLintEslint/node_modules/prettier/bin-prettier.js --config ~/Misc/CodeLintEslint/prettier.config.js --plugin-search-dir ~/Misc/CodeLintEslint "$@"
}
```

```
cd ~/.dmt/core/node/dmt-fasterix/svelte/src
pretty App.svelte
```

but via `PrettyJsSublime` it does not!

it executes this command:

```bash
~/Misc/CodeLintEslint/node_modules/prettier/bin-prettier.js --parser svelte --use-tabs false --loglevel debug --config ~/Misc/CodeLintEslint/prettier.config.js --plugin-search-dir ~/Misc/CodeLintEslint --with-node-modules ~/.dmt/core/node/dmt-fasterix/svelte/src/App.svelte
```

from `$HOME`

Problem output (it cannot find the svelte library to use for prettyfy-ing):

```
david@eclipse:~$ ~/Misc/CodeLintEslint/node_modules/prettier/bin-prettier.js --parser svelte --use-tabs false --loglevel debug --config ~/Misc/CodeLintEslint/prettier.config.js --plugin-search-dir ~/Misc/CodeLintEslint --with-node-modules ~/.dmt/core/node/dmt-fasterix/svelte/src/App.svelte
[debug] normalized argv: {"_":["/Users/david/.dmt/core/node/dmt-fasterix/svelte/src/App.svelte"],"color":true,"editorconfig":true,"use-tabs":false,"with-node-modules":true,"parser":"svelte","loglevel":"debug","config":"/Users/david/Misc/CodeLintEslint/prettier.config.js","plugin-search-dir":["/Users/david/Misc/CodeLintEslint"],"plugin":[],"ignore-path":".prettierignore","debug-repeat":0,"config-precedence":"cli-override"}
[debug] load config file from '/Users/david/Misc/CodeLintEslint/prettier.config.js'
[debug] loaded options `{"plugins":["svelte"],"printWidth":160,"singleQuote":true,"trailingComma":"none"}`
[error] Unable to expand glob patterns: /Users/david/.dmt/core/node/dmt-fasterix/svelte/src/App.svelte !**/.{git,svn,hg}/** !./.{git,svn,hg}/**
[error] Cannot find module 'svelte' from '/Users/david'
```

### Problem #2 (not yet sure if it's a problem)

Could not set up `prettier` plugins / rules in `.eslintrc.json` so that any possible differences between current setup and prettier would get marked better (or something!! ?)

Perhaps check `.eslintrc-options/.eslintrc-prettier-basic.json`

### Problem #3

Eslint does not work on normal .js files if selector svelte is used in SublimeLinter config!!

### Problem #4

prettier does not work on normal .js files either if plugin - svelte is setup in prettier.config.js
