[![npm version](https://badge.fury.io/js/cypress-parallel.svg)](https://badge.fury.io/js/cypress-parallel)

# cypress-parallel

Fork of cypress-parallel:
- Allows to disable included reporter when using mochawesome report generator, multi reporter and mochawesome-merge.
- Adds spliting tests by text included in test file `-S=@test,@second` (grep tag, name...)

Reduce up to 40% your Cypress suite execution time parallelizing the test run on the same machine.

|                                                          cypress                                                          |                                                      cypress-parallel                                                       |
| :-----------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------: |
| ![cy-serial-small](https://user-images.githubusercontent.com/38537547/114301114-92600a80-9ac3-11eb-9166-e95ae9cd5178.gif) | ![cy-parallel_small](https://user-images.githubusercontent.com/38537547/114301127-9db33600-9ac3-11eb-9bfc-c2096023bba7.gif) |

# Run your Cypress tests in parallel (locally)

## How it works

🔍 - Search for existing Cypress tests\
📄 - Read (if exists) a weight file\
⚖️ - Split spec files into different threads\
⚙️ - For each thread it runs the Cypress command you've passed as argument\
📈 - Wait for all threads to finish and collects the result in a single report

# How to use

## Install

```
npm i cypress-parallel -D
```

or

```
yarn add cypress-parallel -D
```

## Add a new script

In your `package.json` add a new script:

```typescript
"scripts" :{
  ...
  "cy:run": "cypress run", // It can be any cypress command with any argument
  "cy:parallel" : "cypress-parallel -s cy:run -t 2 -d '<your-cypress-specs-folder>' -a '\"<your-cypress-cmd-args>\"'"
  ...
}
```

### With Arguments

Sample:

```
-a '\"--config baseUrl=http://localhost:3000\"'
```

## Launch the new script

```
npm run cy:parallel
```

or 

Run with npx (no package installation needed)

```
npx cy:parallel -s cy:run -t 2 -d '<your-cypress-specs-folder>' -a '"<your-cypress-cmd-args>"'
```

### Scripts options

| Option            | Alias | Description                        | Type   |
| ----------------- | ----- | ---------------------------------- | ------ |
| --help            |       | Show help                          |        |
| --version         |       | Show version number                |        |
| --script          | -s    | Your npm Cypress command           | string |
| --args            | -a    | Your npm Cypress command arguments | string |
| --threads         | -t    | Number of threads                  | number |
| --specsDir        | -d    | Cypress specs directory            | string |
| --weightsJson     | -w    | Parallel weights json file         | string |
| --reporter        | -r    | Reporter to pass to Cypress.       | string |
| --reporterOptions | -o    | Reporter options                   | string |
| --reporterModulePath | -n    | Specify the reporter module path   | string |
| --disableParallelReporter | -f    | Disable included reporter | boolean |
| --bail            | -b    | Exit on first failing thread       | string |
| --verbose         | -v    | Some additional logging            | string |
| --strictMode      | -m    | Add stricter checks after running the tests           | boolean |
| --splitByText      | -S    | Split test by text included in the file           | string[] |

**NB**: If you use *cypress-cucumber-preprocesor*, please **disable** the *strictMode* to avoid possible errors:

```typescript
"scripts" :{
  ...
  "cy:parallel" : "cypress-parallel -s cy:run -t 4 -m false"
  ...
}
```

**NB**: If your *cypress-multi-reporters* module is not found on the same level as your Cypress suite (e.g. in a mono-repo) then you can specify the module directory for Cypress to search within.

```typescript
"scripts" :{
  ...
  "cy:parallel" : "cypress-parallel -s cy:run -t 4 -n .../../../node_modules/cypress-multi-reporters"
  ...
}
```

**NB**: If you disbaled included reporter, then use following config and actions after getting results in json from mochawesome reporter.

```typescript
 reporterOptions: {
  ...
  saveJson: true
  saveHtml: false
  ...
}
```

```typescript
"scripts" :{
  ...
  "cy:merge-json": "mocahwsome-merge <jsonsPath/*.json> -o <output>"
  "cy:generate-html": "marge <jsonPath> -o=<outputHTMLPath> -i" 
  ...
}
```

## Env variables

### CYPRESS_THREAD

You can get the current thread index by reading the `CYPRESS_THREAD` variable.

```javascript
 const threadIndex = process.env.CYPRESS_THREAD;
 // return 1, 2, 3, 4, ...
```

# Contributors

Looking for contributors.

# License

This project is licensed under the MIT license. See [LICENSE](LICENSE).
