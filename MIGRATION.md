# ESLint Migrations Guide

## Migration from ESLint 8.x.x to 9.x.x

The following steps are recommended when migrating from an existing ESLint 8.x.x configuration using `.eslint.rc` configuration file to an ESLint 9.x.x setup using `@iobroker/eslint-config`.
The steps are valid for vanilla Javascript and TypeScript repositories. If you are using ESM modules or React the ESLint configuration needs to include some more modules. Please see `README.md` for details.

- [ ] Clone your repository to a local workspace
- [ ] Verify that current (old) ESLint configuration is working

  Execute `npm run lint` and check for errors. Consider fixing all existing errors before starting the migration.
      
- [ ] remove packages no longer needed

      npm uninstall eslint
      npm uninstall eslint-config-prettier
      npm uninstall eslint-plugin-prettier
      npm uninstall prettier


- [ ] install iobroker standard rules

      npm i @iobroker/eslint-config --save-dev

- [ ] remove old configuration files

      .eslintignore
      .eslintrc.json
      .prettierignore
      .prettierrc.js
      
- [ ] create new configuration file `eslint.config.mjs` for ESLint in the root directory of the repository
  ```js
  // ioBroker eslint template configuration file for js and ts files
  // Please note that esm or react based modules need additional modules loaded.
  import config from '@iobroker/eslint-config';
  
  export default [
      ...config,
  
      {
          // specify files to exclude from linting here
          ignores: [
              '*.test.js', 
              'test/**/*.js', 
              '*.config.mjs', 
              'build', 
              'admin/build', 
              'admin/words.js',
              'admin/admin.d.ts',
              '**/adapter-config.d.ts'     
          ] 
      },
  
      {
          // you may disable some 'jsdoc' warnings - but using jsdoc is highly recommended
          // as this improves maintainability. jsdoc warnings will not block buiuld process.
          rules: {
              // 'jsdoc/require-jsdoc': 'off',
          },
      },
      
  ];
   ```
  
- [ ] create new configuration file 'prettier.config.mjs' for Prettier in the root directory of the repository
  ```js
  // iobroker prettier configuration file
  import prettierConfig from '@iobroker/eslint-config/prettier.config.mjs';
  
  export default {
      ...prettierConfig,
      // uncomment next line if you prefer double quotes
      // singleQuote: false,
  }
  ```
  
- [ ] check and eventually adapt script definition at package.json
   
  Your 'lint' script definition at package.json should read like this
  ```json
  {
      "scripts": {
          "lint": "eslint -c eslint.config.mjs ."
      }
  }
  ```

- [ ] update .npmignore (if still in use)

  If you still use .npmignore and not yet switched to use files section within package.json, add the following files to `.npmignore`

      eslint.config.mjs
      prettier.config.mjs
          
- [ ] check functionality by executing
   
      npm run lint

  Please note that the execution of ESLint 9 checks will last longer than previous executions. You might get errors and warnings due to new rules.
  Feel free to try `npm run lint -- --fix` to perform automatic fixes.


