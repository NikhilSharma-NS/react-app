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
      - run: npm CI
      - run: npm run format:check
      - run: npm test -- --coverage
        env:
          CI: true

          
```

Step 6:
