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


## 05-Cumulative-Layout-Shift-NgOptimizedImage

- In the product-detail.components.ts import "NgOptimizedImage".
```sh
import { NgOptimizedImage } from '@angular/common';
```
- In product-detail.components.html we have [src]="cover()" to get the image.
```sh
  [src]="cover()"
```
- Change for:
```sh
  [ngSrc]="cover()"
  width="68"
  height="68"
```
Is important to give a size to keep this size from the beginning.

BUT this is OK only for the carrousel. Because is not responsive.
The main picture in this case we use "fill n true

- From this:
```sh
<div class="w-full rounded border border-gray-200">
  <img
    alt="ecommerce"
    class="w-full object-cover object-center"
    [src]="cover()" />
</div>
```
- Change for:
```sh
<div class="w-full rounded border border-gray-200">
  <div class="relative h-80">
    <img
      alt="ecommerce"
      [fill]="true"
      class="w-full object-cover object-center"
      [ngSrc]="cover()" />
  </div>
</div>
```
Here we add a "father with class relative and we add the high in 80rem

- In the product.component.html we have almost the same. The main difference is we will have a lot of images. So we will add "loading="lazy" to render in the lazy way. Lik this
```sh
 <div class="relative h-36 sm:h-60 md:h-48">
  <img
    [ngSrc]="product.images[0]"
    [alt]="product.title"
    [fill]="true"
    loading="lazy"
    class="h-full w-full object-cover object-center group-hover:opacity-75" />
</div>
```
- With this will load the image when need it.
