# workflow-test

[![build](https://github.com/muhamadto/workflow-test/actions/workflows/build.yml/badge.svg)](https://github.com/muhamadto/workflow-test/actions/workflows/build.yml)
[![CodeQL](https://github.com/muhamadto/workflow-test/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/muhamadto/workflow-test/actions/workflows/codeql-analysis.yml)
[![publish](https://github.com/muhamadto/workflow-test/actions/workflows/publish.yml/badge.svg?branch=main&event=release)](https://github.com/muhamadto/workflow-test/actions/workflows/publish.yml)

forked from [masahirompp/workflow-test](https://github.com/masahirompp/workflow-test). Please
find [copyright notice and license](https://github.com/masahirompp/aws-cdk-webpack-lambda-function/blob/master/LICENSE)
in the original repository.

This library provides AWS CDK constructs for Lambda function bundled using webpack.

## Quick Start

1. install using yarn:

   ```sh
   yarn add -D workflow-test aws-cdk-lib aws-cdk constructs webpack webpack-cli
   # npm i -D workflow-test aws-cdk-lib aws-cdk constructs webpack webpack-cli
   ```

   Note: webpack@5 required.

1. add webpack.config.js:

   ```js
   module.exports = {
     mode: "production", // production or development
     target: "node",
     externals: [/^aws-sdk(\/.+)?$/], // important!!!
     devtool: "source-map", // if needed
     optimization: { minimize: false }, // if needed
     // for TypeScript
     module: {
       rules: [
         {
           test: /\.ts$/,
           use: {
             loader: "ts-loader",
             options: {
               configFile: "your/path/to/tsconfig.json", // if needed
               // colors: true,
               // logInfoToStdOut: true,
               // logLevel: 'INFO',
               transpileOnly: true,
             },
           },
           exclude: /node_modules/,
         },
       ],
     },
     resolve: {
       extensions: [".js", ".ts"],
     },
   };
   ```

1. (Optional) add tsconfig.json for lambda

   ```json
   {
     "extends": "../ ... /tsconfig.json",
     "compilerOptions": {
       "importHelpers": false,
       "target": "ES2018",
       "noEmit": false
     }
   }
   ```

1. your cdk source code:

   ```typescript
   import { WebpackFunction } from "workflow-test";

   new WebpackFunction(this, "YourFunction", {
     entry: "your/path/to/function.ts",
     config: "your/path/to/webpack.config.js",
   });
   ```

## Options

### entry: string (required)

Path to the entry file (JavaScript or TypeScript).

### config: string (required)

Path to webpack config file.

### handler: string

The name of the exported handler in the entry file.

default: "handler"

### runtime: lambda.Runtime

The runtime environment. Only runtimes of the Node.js family are supported.

default: NODEJS_14

### buildDir: string

The build directory.

default: `.build` in the entry file directory

### ensureUniqueBuildPath: boolean

Control whether the build output is placed in a unique directory (sha256 hash) or not. This can be disabled to simplify development and debugging.

default: true

### ...other options

All other properties of lambda.Function are supported, see also the [AWS Lambda construct library](https://github.com/aws/aws-cdk/tree/master/packages/%40aws-cdk/aws-lambda).

## Run tests

```sh
yarn build
yarn test
```
