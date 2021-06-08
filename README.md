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
      - run: npm CI
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true
      - name: Build Projects
      - if: github.event_name == 'push'
      - run: npm run build
      - name: Deploy to Staging
        if: github.event_name == 'push'
        run: npx surge --project ./build --domain silent-apparatus.surge.sh
        env:
          SURGE_LOGIN: ${{ secrets.SURGE_LOGIN }}
          SURGE_TOKEN: ${{ secrets.SURGE_TOKEN }}
```
