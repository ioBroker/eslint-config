# ioBroker ESLint Configuration

ESLint configuration package that provides standardized linting rules for ioBroker projects supporting JavaScript, TypeScript, React, and ESM modules.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Bootstrap and validate the repository:
- `npm install` -- installs dev dependencies. Takes ~5 seconds (already cached). NEVER CANCEL. Set timeout to 30+ seconds.
- `npx eslint -c eslint.config.mjs .` -- lints the configuration files themselves. Takes ~2 seconds. NEVER CANCEL. Set timeout to 30+ seconds.
- Test the configuration works by creating a test project (see validation section below)

### Core Commands:
- `npm run release` -- creates a release using the release script (for maintainers only)
- `npx eslint -c eslint.config.mjs . --fix` -- automatically fixes linting issues where possible

### Development and Testing:
ALWAYS validate changes by creating a test project and ensuring the ESLint configuration works correctly:

1. Create test directory: `mkdir /tmp/test-project && cd /tmp/test-project`

2. Create test package.json with minimal dependencies:
```json
{
  "name": "test-project",
  "version": "1.0.0",
  "scripts": {
    "lint": "eslint -c eslint.config.mjs ."
  },
  "devDependencies": {
    "@iobroker/eslint-config": "file:/home/runner/work/eslint-config/eslint-config",
    "eslint": "^9.34.0",
    "prettier": "^3.6.2"
  }
}
```

3. Create test eslint.config.mjs:
```javascript
import config from '@iobroker/eslint-config';
export default [...config];
```

4. Create test prettier.config.mjs:
```javascript
import prettierConfig from '@iobroker/eslint-config/prettier.config.mjs';
export default prettierConfig;
```

5. Create test JavaScript file to lint:
```javascript
/**
 * Test function
 *
 * @param name - The name parameter  
 * @returns A greeting message
 */
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet('World'));
```

6. Run validation:
   - `npm install` -- takes ~2-5 seconds. NEVER CANCEL. Set timeout to 30+ seconds.
   - `npm run lint` -- should show any linting issues (~2 seconds)
   - `npm run lint -- --fix` -- should auto-fix formatting issues
   - `npm run lint` -- should pass cleanly after auto-fix

### Testing TypeScript Support:
To test TypeScript linting, add these dependencies and files to your test project:

1. Update package.json to include TypeScript dependencies:
```json
"devDependencies": {
  "@iobroker/eslint-config": "file:/home/runner/work/eslint-config/eslint-config",
  "@eslint/eslintrc": "^3.3.1",
  "@eslint/js": "^9.34.0",
  "@typescript-eslint/eslint-plugin": "^8.40.0",
  "@typescript-eslint/parser": "^8.40.0",
  "eslint": "^9.34.0",
  "eslint-config-prettier": "^10.1.8",
  "eslint-plugin-jsdoc": "^54.1.1",
  "eslint-plugin-prettier": "^5.5.4",
  "globals": "^16.3.0",
  "prettier": "^3.6.2",
  "typescript-eslint": "^8.40.0",
  "typescript": "^5.0.0"
}
```

2. Create tsconfig.json:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["*.ts", "*.tsx"]
}
```

3. Create test TypeScript file:
```typescript
/**
 * Test TypeScript function
 *
 * @param name - The name parameter
 * @returns A greeting message
 */
function greet(name: string): string {
    return `Hello, ${name}!`;
}

console.log(greet('TypeScript'));
```

4. Test: `npx eslint -c eslint.config.mjs test.ts`

### Timing Expectations:
- npm install (fresh): ~15 seconds. NEVER CANCEL. Set timeout to 60+ seconds.
- npm install (cached): ~2-5 seconds. NEVER CANCEL. Set timeout to 30+ seconds.
- ESLint runs: ~2-5 seconds. NEVER CANCEL. Set timeout to 30+ seconds.
- Test project setup and validation: ~20 seconds total. NEVER CANCEL.

## Validation

ALWAYS test configuration changes by:
1. Creating a test project using the steps above
2. Testing with different file types (.js, .ts)
3. Verifying both error detection and auto-fix functionality work
4. Testing ESM configuration if making changes to that area
5. For React changes, note that full React testing requires additional setup

## Repository Structure

### Key Files:
- `eslint.config.mjs` -- Main ESLint configuration with rules for JS/TS
- `prettier.config.mjs` -- Prettier formatting configuration  
- `package.json` -- Package definition with peer dependencies
- `README.md` -- Usage documentation and examples
- `MIGRATION.md` -- Migration guide from ESLint 8.x to 9.x

### Configuration Exports:
- Default export: Base JavaScript/TypeScript configuration
- `esmConfig`: Additional rules for ESM modules
- `reactConfig`: Additional rules for React projects

### Important Directories:
- `.github/workflows/` -- CI/CD for publishing and releases
- No `src/`, `build/`, or `dist/` directories -- this is a configuration-only package

## Common Tasks

The following are outputs from frequently run commands. Reference them instead of viewing, searching, or running bash commands to save time.

### Repository root structure:
```
eslint.config.mjs       -- Main ESLint configuration
prettier.config.mjs     -- Prettier configuration
package.json           -- Package definition
README.md             -- Usage documentation  
MIGRATION.md          -- Migration guide
LICENSE               -- MIT license
.github/              -- CI/CD workflows
```

### Package.json scripts:
```json
{
  "scripts": {
    "release": "release-script -y --noPush"
  }
}
```

### ESLint configuration structure:
- Supports JavaScript (.js, .cjs, .mjs) and TypeScript (.ts, .tsx)
- Includes JSDoc, Prettier, TypeScript, React, and Unicorn plugins
- Exports additional configs for ESM and React projects
- Uses flat config format (ESLint 9.x)

## Configuration Usage Examples

### Basic JavaScript/TypeScript:
```javascript
import config from '@iobroker/eslint-config';
export default [...config];
```

### With ESM support:
```javascript  
import config, { esmConfig } from '@iobroker/eslint-config';
export default [...config, ...esmConfig];
```

### With React support:
```javascript
import config, { reactConfig } from '@iobroker/eslint-config';  
export default [...config, ...reactConfig];
```

### Required Peer Dependencies for Full Functionality:
When testing with TypeScript, React, or advanced features, install all peer dependencies listed in package.json:
- eslint >=9.32.0
- prettier >=3.6.2  
- All TypeScript ESLint packages for TypeScript support
- React packages for React support
- Unicorn package for ESM support

## Notes for Development

- This is a configuration package -- there is no application code to "build" or "run"
- Validation is done through linting and testing with sample projects
- Changes should be tested with multiple project types (JS, TS, React, ESM)
- ESLint 9.x flat config format is used throughout
- Auto-fix functionality should always be tested after making rule changes
- The package is published to npm automatically via GitHub Actions on merge to main
- Full React/TypeScript testing requires substantial peer dependency setup -- focus on basic JS/TS validation for most changes