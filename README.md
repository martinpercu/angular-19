# Angular-19 2025


## 01-Eslint-Prettier

- We start cleaning unused imports:
```sh
ng generate @angular/core:cleanup-unused-imports
```
- This check usued import and delete them.

- Install eslint for Angular
```sh
ng add @angular-eslint/schematics
```
- ... then to show errors and warnings
```sh
ng lint
```
- ... to fix them (not all but most of warnings)
```sh
ng lint --fix
```

- Install Prettier only for development
```sh
npm i prettier -D
```
- Now in package.json in scripts add ===> "format": "prettier --write ."
- Now run
```sh
npm run format
```
- Also if we want to specify the format in a different way we creat a new file in the root .prettierrc.json. Here we specify how we want to be formatting the "npm run format".

- To make Eslint and Prettier works fine together in terminal intall like this:
```sh
npm i prettier-eslint eslint-config-prettier eslint-plugin-prettier -dev
```
- Then in esling.config.js add const prettierRules = require("eslint-plugin-prettier/recommended"). Then add this prettierRules in the extends[] for .ts and the the extends[] for .html




