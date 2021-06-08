# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)

# react-app

##### Setting up Our Repository

Step 1: Create folder .github\workflows

step 2: Create the develop branch
https://github.com/NikhilSharma-NS/react-app/tree/develop

step 3:
https://github.com/NikhilSharma-NS/react-app/settings/branch_protection_rules/new
add branch protection rules for master

https://github.com/NikhilSharma-NS/react-app/settings/branch_protection_rules/20854687

Step 4
branch rule for develop branch
https://github.com/NikhilSharma-NS/react-app/settings/branch_protection_rules/20854736

Step 5:

create folder .github\workflows
and create the file ci.yml

```
name: CI
on:
  pull_request:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true



```

Step 6:

npm install --save-dev --save-exact prettier
npx prettier --check "**/\*.js" -> to check the format
npx prettier --write "**/\*.js" -> to fix the format
npx prettier --write "\*_/_.{js,jsx,yml,yaml,json,css,scss,md}"

```
npx prettier --write "**/*.{js,jsx,yml,yaml,json,css,scss,md}"
```

#### Creating Develop merge pull requets

Step 1:

surge

Step 2:
surge token

step 3:

surge whoami

Step 4:

```
name: CI
on:
  pull_request:
    branches: [develop]
  push:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build
      - name: Deploy to Staging
        if: github.event_name == 'push'
        run: npx surge --project ./build --domain silent-apparatus.surge.sh
        env:
          SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
          SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}





```

##### Caching NPM Dependencies

Step 1:

```
name: CI
on:
  pull_request:
    branches: [develop]
  push:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm CI
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Build Projects
        if: github.event_name == 'push'
      - run: npm run build
      - name: Deploy to Staging
        if: github.event_name == 'push'
        run: npx surge --project ./build --domain silent-apparatus.surge.sh
        env:
          SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
          SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}

```

##### Uploading Artifacts in Our Workflows

Step1:

```
name: CI
on:
  pull_request:
    branches: [develop]
  push:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Upload Test Coverage
        uses: actions/upload-artifact@v1
        with:
          name: code-coverage
          path: coverage
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build
      - name: Upload Build Folder
        if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
      - name: Deploy to Staging
        if: github.event_name == 'push'
        run: npx surge --project ./build --domain silent-apparatus.surge.sh
        env:
          SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
          SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}

```

##### Semantic Versioning and Conventional Commits

Step 1:

Semantic Versioning : 2.1.3

Major Version
(Breaking Changes) : X.1.3

Minor Version
New Feature(No Breaking Changes) : 2.X.3

Patch Version
Bug fix : 2.1.X

Step 2:

Conventional Commits

<type>[optional scope]:<description>

https://semver.org/
https://www.conventional/commits.org/en/v1.0.0/

##### Installing semantic-release in Our Project

Step1: release.config.js

```
module.exports = {
	branches: "master",
	repositoryUrl: "https://github.com/NikhilSharma-NS/react-app",
	plugins: [
		"@semantic-release/commit-analyzer", "@semantic-release/release-notes-generator",
		"@semantic-release/github"
		]
}
```

Step2:

```
name: CI
on:
  pull_request:
    branches: [develop]
  push:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm CI
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Upload Test Coverage
        uses: actions/upload-artifact@v1
        with:
          name: code-coverage
          path: coverage
      - name: Build Projects
      - if: github.event_name == 'push'
      - run: npm run build
      - name: Upload Build Folder
      - if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
      - uses: actions/download-artifact

      - name: Deploy to Staging
        if: github.event_name == 'push'
        run: npx surge --project ./build --domain silent-apparatus.surge.sh
        env:
          SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
          SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}
```

Step 3:

npx semantic-release

##### Running Semantic release in Our Workflow

Step 1:

```
name: CI
on:
  pull_request:
    branches: [develop, master]
  push:
    branches: [develop, master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Upload Test Coverage
        uses: actions/upload-artifact@v1
        with:
          name: code-coverage
          path: coverage
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build
      - name: Upload Build Folder
        if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
      - name: Create a Release
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      # - name: Deploy to Staging
      #   if: github.event_name == 'push'
      #   run: npx surge --project ./build --domain silent-apparatus.surge.sh
      #   env:
      #     SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
      #     SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}

```

##### Uploading Release Assets

step1:

```
module.exports = {
  branches: "master",
  repositoryUrl: "https://github.com/NikhilSharma-NS/react-app",
  plugins: [
    "@semantic-release/commit-analyzer", "@semantic-release/release-notes-generator",
    ["@semantic-release/github", {
        assets: [
            { path: "build.zip", label: "Build" },
            { path: "coverage.zip", label: "Coverage" }
        ]
    }]
    ]
};

```

Step2:

```
name: CI
on:
  pull_request:
    branches: [develop, master]
  push:
    branches: [develop, master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Upload Test Coverage
        uses: actions/upload-artifact@v1
        with:
          name: code-coverage
          path: coverage
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build
      - name: Upload Build Folder
        if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
      - name: ZIP Assets
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: |
          zip -r build.zip ./build
          zip -r coverage.zip ./coverage
      - name: Create a Release
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - name: Deploy to Staging
        if: github.event_name == 'push'
        run: npx surge --project ./build --domain silent-apparatus.surge.sh
        env:
          SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
          SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}


```

##### Deploying to Production when Pushing to master

Step 1:

```
name: CI
on:
  pull_request:
    branches: [develop, master]
  push:
    branches: [develop, master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Upload Test Coverage
        uses: actions/upload-artifact@v1
        with:
          name: code-coverage
          path: coverage
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build
      - name: Upload Build Folder
        if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
      - name: ZIP Assets
        if: github.event_name == 'push' && giathub.ref == 'refs/heads/master'
        run: |
          zip -r build.zip ./build
          zip -r coverage.zip ./coverage
     - name: Create a Release
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      # - name: Deploy to Staging
      #   if: github.event_name == 'push' && github.ref == 'refs/heads/develop'
      #   run: npx surge --project ./build --domain silent-apparatus.surge.sh
      # - name: Deploy to Production
      #   if: github.event_name == 'push' && github.ref == 'refs/heads/master'
      #   run: npx surge --project ./build --domain enormous-poison-prod.sh

```

##### Uploading Code Coverage Reports to Codecov

Step1:

http://codecov.in/

Step 2:
https://app.codecov.io/gh/NikhilSharma-NS/react-app

```

name: CI
on:
  pull_request:
    branches: [develop, master]
  push:
    branches: [develop, master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Cache node-modules
        uses: actions/cache@v1
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-
      - name: Use NodeJS
        uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Upload Test Coverage
        uses: actions/upload-artifact@v1
        with:
          name: code-coverage
          path: coverage
      - name: Build Projects
        if: github.event_name == 'push'
        run: npm run build
      - name: Upload Build Folder
        if: github.event_name == 'push'
        uses: actions/upload-artifact@v1
        with:
          name: build
          path: build
      - name: ZIP Assets
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: |
          zip -r build.zip ./build
          zip -r coverage.zip ./coverage
      - name: Create a Release
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      # - name: Deploy to Staging
      #   if: github.event_name == 'push'
      #   run: npx surge --project ./build --domain silent-apparatus.surge.sh
      #   env:
      #     SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
      #     SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}
      # - name: Deploy to Production
      #   if: github.event_name == 'push' && github.ref == 'refs/heads/master'
      #   run: npx surge --project ./build --domain enormous-poison-prod.sh
      - name: Upload Coverage Reports
        if: github.event_name == 'push' && github.ref == 'refs/heads/master'
        run: npx codecov
        env:
          CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}

```

Step 3:
https://app.codecov.io/gh/NikhilSharma-NS/react-app/


##### Validating Our Comit Messages with Commitlint and Commitizen

