## Setup

```
mkdir ~/Misc
cd ~/Misc
git clone https://github.com/davidhq/CodeLintEslint.git
```

```
cd ~/bin

ln -s ~/Misc/CodeLintEslint/node_modules/eslint/bin/eslint.js eslint
ln -s ~/Misc/CodeLintEslint/node_modules/prettier/bin-prettier.js prettier
```

```
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages
git clone https://github.com/davidhq/SublimeJsPrettier.git
cd SublimeJsPrettier
git checkout 7442289bd489eb4360659c5bc22409d563169c90
(for now!)
```

## Problems:

### Problem 1

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

```
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

### Problem 2

eslint setup is not really using svelte preprocessor.... check `.eslintrc.json`

```
~/Misc/CodeLintEslint/.eslintrc-options/.eslintrc-standard.json
```

see:

```
"files": ["*.todo-investigate-svelte-processor-directive-below-fails"],
"processor": "svelte3/svelte3",
```

try moving this line up to the correct "overrides" section ....
