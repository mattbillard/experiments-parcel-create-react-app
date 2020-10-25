# Parcel Create React App

## Goal
Use Parcel for the standard Create React App

## Steps to Reproduce this Experiment

### 1) New Create React App (CRA)
- Generate a new Create React App (From the official docs)  
  `npx create-react-app 1-create-react-app --template typescript`

### 2) Create and download a new Parcel app 
- Go to https://createapp.dev/parcel
- Choose: Parcel, React, Jest, Babel, TypeScript, CSS, Sass, Prettier. (PostCSS was having trouble when I did this experiment but might be working now)
- Name: 2-parcel-createapp.dev
- Download, unzip 

### 3) Create Parcel + CRA 
- Duplicate the Parcel app but replace its src/ and public/ dirs with those from CRA. Run the following
  ```
  cp -r 2-parcel-createapp.dev                    3-parcel-create-react-app

  rm -rf 3-parcel-create-react-app/src
  rm -rf 3-parcel-create-react-app/node_modules

  cp -r 1-create-react-app/src                    3-parcel-create-react-app
  cp -r 1-create-react-app/public                 3-parcel-create-react-app

  mv 3-parcel-create-react-app/public/index.html  3-parcel-create-react-app/src
  ```
- Open 3-parcel-create-react-app/package.json
  - Change the name to  
    `3-parcel-create-react-app`



### 4) Fix the app so it will run
- Open 3-parcel-create-react-app/package.json
  - Add missing dependency that CRA src needs  
    `"web-vitals": "^0.2.4",`
- Open 3-parcel-create-react-app/src/index.html
  - Find and replace  
    `%PUBLIC_URL%`  
    to  
    `../public`  
  - Add before </body>  
    `<script src="./index.tsx"></script>`
- The app should now run and be visible in the browser at `http://localhost:1234/`
  ```
  cd 3-parcel-create-react-app/
  npm i 
  npm start 
  ```


### 5) Fix Jest tests 
- Copy some files from CRA that are required
  ```
  cd ../1-create-react-app/
  npm run eject 
  y
  cd ..
  mkdir -p 3-parcel-create-react-app/config
  cp -r 1-create-react-app/config/jest        3-parcel-create-react-app/config
  ```
- Add the following dependencies from CRA to 3-parcel-create-react-app/package.json
  ```
  "@testing-library/jest-dom": "^4.2.4",
  "@testing-library/react": "^9.3.2",
  "@testing-library/user-event": "^7.1.2",
  "@types/jestπ": "^24.0.0",
  "@types/node": "^12.0.0",
  "babel-preset-react-app": "^9.1.2",
  "camelcase": "^5.3.1",
  "jest-environment-jsdom-fourteen": "1.0.1",
  "jest-watch-typeahead": "0.4.2",
  "react-app-polyfill": "^1.0.6"
  ```
- Also add the following jest config from CRA to 3-parcel-create-react-app/package.json
  ```
  "jest": {
    "roots": [
      "<rootDir>/src"
    ],
    "collectCoverageFrom": [
      "src/**/*.{js,jsx,ts,tsx}",
      "!src/**/*.d.ts"
    ],
    "setupFiles": [
      "react-app-polyfill/jsdom"
    ],
    "setupFilesAfterEnv": [
      "<rootDir>/src/setupTests.ts"
    ],
    "testMatch": [
      "<rootDir>/src/**/__tests__/**/*.{js,jsx,ts,tsx}",
      "<rootDir>/src/**/*.{spec,test}.{js,jsx,ts,tsx}"
    ],
    "testEnvironment": "jest-environment-jsdom-fourteen",
    "transform": {
      "^.+\\.(js|jsx|ts|tsx)$": "<rootDir>/node_modules/babel-jest",
      "^.+\\.css$": "<rootDir>/config/jest/cssTransform.js",
      "^(?!.*\\.(js|jsx|ts|tsx|css|json)$)": "<rootDir>/config/jest/fileTransform.js"
    },
    "transformIgnorePatterns": [
      "[/\\\\]node_modules[/\\\\].+\\.(js|jsx|ts|tsx)$",
      "^.+\\.module\\.(css|sass|scss)$"
    ],
    "modulePaths": [],
    "moduleNameMapper": {
      "^react-native$": "react-native-web",
      "^.+\\.module\\.(css|sass|scss)$": "identity-obj-proxy"
    },
    "moduleFileExtensions": [
      "web.js",
      "js",
      "web.ts",
      "ts",
      "web.tsx",
      "tsx",
      "json",
      "web.jsx",
      "jsx",
      "node"
    ],
    "watchPlugins": [
      "jest-watch-typeahead/filename",
      "jest-watch-typeahead/testname"
    ]
  }
  ```
- Fix .babelrc
  - Jest works with CommonJS modules (aka require statements), not ESM modules (aka import statements)
  - Open 3-parcel-create-react-app/.babelrc
  - Change  
    `modules: false`  
    to  
    `modules: "commonjs"`
- Fix setupTests.ts
  - Open 3-parcel-create-react-app/src/setupTests.ts
  - Change  
    `import '@testing-library/jest-dom';`  
    to  
    `import '@testing-library/jest-dom/extend-expect';`
- Tests should now run successfully
  - If the test do not run, it might be because CRA and/or Parcel have changed since this repo was made. If that happens, try doing a diff with this project 
  - Run the following 
    ```
    cd 3-parcel-create-react-app
    npm i 
    npm run test
    ```
