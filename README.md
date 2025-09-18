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



## 02-Environments-dev-prod

- Angular will create 2 environments by default dev and prod run this:
```sh
ng generate environments
```
- This will create a folder environments with 2 files environments.ts & environments.development.ts
- In environments.development.ts wi will add in const environment the vars for development like this:
```sh
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000',
};
```
- For easy import the environments add "@environments/*": ["./src/environments/*"]
in the tsconfig.json. Now with a line like this (using @environments):
```sh
import { environment } from '@environments/environment';
```
- Check the category.service.ts and product.service.ts to see the imports.


## 03-Environment-staging

- Create a new environment.staging.ts in the environment folder. Ths will be something n the middle between dev and prod.
```sh
export const environment = {
  production: true,
  apiUrl: 'https://staging-api.escuelas.co',
};
```
- To use this environment wi need to modify manually the "angular.json" .
- In the angular.json find "projects==>store==>architect==>build==>configuration" there there are "production" and "developer". We must add "staging" and copy the "fileReplacements" from the "developer" and change the line starting with "with". ==> something like this
```sh
"staging": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.staging.ts"
                }
              ]
            },
```
- In angular.json make the same but now in "projects==>store==>architect==>serve==>configuration"
```sh
"serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "configurations": {
            "production": {
              "buildTarget": "store:build:production"
            },
            "staging": {
              "buildTarget": "store:build:staging"
            },
            "development": {
              "buildTarget": "store:build:development"
            }
          },
          "defaultConfiguration": "development"
        },
```

- To run or build differents environments
```sh
ng serve -c staging
ng build -c development
```
- Remember by default serve is development and build is production. This is the logic we found in the angular.json




## 04-Local-vars

- In product-detail.components.html we have the "product()" signal several times. To make only on time the signal computation we will create a local var "@let dataProduct = product();" to compute once and use this var "dataProduct" to render the component. Then an @if to avoid the '?' every time.
From this:
```sh
      {{ product()?.title }}
```
To this:
- In product-detail.components.html
```sh
@let dataProduct = product();
@if(dataProduct){
  {{ dataProduct.title }}
}
```
- In header.component.html ... is the same but in this case with the "cart()" signal.
```sh
@let dataCart = cart();
@if(dataCart){
  {{ dataCart.length }}
}
```






