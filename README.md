<h1 align="center">Automation Testing with Cypress
</h1>

</p>

<p align="center">
Learn Cypress for end-to-end web automation with Cypress Components such as fixtures, plugins, screenshots and videos to build framework using Cypress + Docker + Cucumber + Jenkins, and API testing.
</p>

# 🚀 Table of contents

- [🚀 Table of contents](#-table-of-contents)
  - [Cypress Overview](#cypress-overview)
  - [Prerequisites](#prerequisites)
    - [Node.js and npm](#nodejs-and-npm)
    - [Browsers Supported](#browsers-supported)
    - [Cypress version](#cypress-version)
  - [Set up](#set-up)
    - [Cypress Installation \& Setup](#cypress-installation--setup)
    - [Project Structure](#project-structure)
  - [Plugins](#plugins)
    - [Prettier](#prettier)
    - [XPath](#xpath)
  - [End-2End Testing](#end-2end-testing)
    - [Assertions](#assertions)
    - [Locator](#locator)
    - [Waiting](#waiting)
    - [UI](#ui)
    - [Actions](#actions)
    - [Aliasing](#aliasing)
    - [Connectors](#connectors)
    - [Cookies](#cookies)
  - [Cypress Framework Designing](#cypress-framework-designing)
    - [Hook](#hook)
    - [Test Data with Fixture (Json, Excel, Csv)](#test-data-with-fixture-json-excel-csv)
      - [JSON - Reading data](#json---reading-data)
      - [EXCEL - Reading Data](#excel---reading-data)
      - [CSV - Reading Data](#csv---reading-data)
    - [Custom commands \& Global Configurations setup](#custom-commands--global-configurations-setup)
      - [Create Custom commands](#create-custom-commands)
      - [Global Configurations setup](#global-configurations-setup)
    - [Page Object Model](#page-object-model)
  - [Dashboard Setup](#dashboard-setup)
  - [Mochawesome Report Set up](#mochawesome-report-set-up)
    - [Mochawesome report](#mochawesome-report)
    - [Set up Mochawesome report](#set-up-mochawesome-report)
    - [Add Failed test screenshot in Mochawesome report](#add-failed-test-screenshot-in-mochawesome-report)
  - [API Testing](#api-testing)
    - [API Testing](#api-testing-1)
    - [Rest API](#rest-api)
  - [Mocks \& Stubs](#mocks--stubs)
    - [Mocks?](#mocks)
    - [Using mocks in real time](#using-mocks-in-real-time)
  - [Docter Intergration with Cypress](#docter-intergration-with-cypress)
    - [Docker](#docker)
    - [Install Docker](#install-docker)
    - [Running Cypress Tests inside Docker](#running-cypress-tests-inside-docker)
    - [Docker File \& Docker Compose](#docker-file--docker-compose)
      - [Dockerfile](#dockerfile)
      - [Docker Compose](#docker-compose)
  - [Cucumber - BDD Framework](#cucumber---bdd-framework)
  - [CICD Intergration](#cicd-intergration)
    - [CICD](#cicd)
    - [Jenkins?](#jenkins)

## Cypress Overview

Cypress is a popular and easy-to-use JavaScript testing framework for web applications.

- Developer & QA friendly and open source and free (except Dashboard)
- Suitable for unit, integration, and end-to-end testing
- Simple installation and setup. No dependencies, downloads, or code changes needed.
- Write tests easily and execute in real time as you build your web application.
- Run test locally easily debug tests in CI locally, built-in parallelism and load balancing.
- Record CI test data, screenshots, and video and view aggregated, next-level insights in your Dashboard.
- Time travel, debugging, automatic waiting, screenshots, videos, cross-browser testing, parallelization, mocking, stubbing, và flake detection.
- Cypress limitations: Single browser per test, No multi-tab support, JavaScript-only, No Safari or IE support, Limited iFrame support
- Better than Selenium in speed, reliability, and features

## Prerequisites

### Node.js and npm

- Ensure you have `Node.js` (version 18x or 20+) and npm (version 5.6 or later) installed

> 📌 _**Node.js** is a runtime environment for JavaScript that Cypress requires as an npm module._
>
> - _Download `Node.js`: https://nodejs.org/en/download/_
> - _Check versions: `node -v` and `npm -v`_

### Browsers Supported

Cypress supports the browser versions:

- Chrome-family (`Chrome 80+, Edge 80+, Electron 15.5.1+`)
- WebKit (`Safari engine`, unofficially supported)
- Firefox (`Firefox 86+`)

> 📌 _Limited Browser Support (not Safari or IE)_

### Cypress version

_[Latest version](https://github.com/cypress-io/cypress/releases): `13.6.3` (Jan 16, 2024)_

> 📌 _Check the installed Cypress version: `npx cypress --version`_

## Set up

### Cypress Installation & Setup

> _Ensuring `Node.js` and initialize the project using `npm init -y` which creates a `package.json` file before installing Cypress_

1. Install cypress: `npm install cypress --save-dev`
2. Opens the Test Runner UI: `npx cypress open`
3. Run Cypress: `npx cypress run`
4. Customization options: `npx cypress run --browser chrome --reporter mocha --record --key 123456789`

   - `npx cypress run --browser <chrome, firefox, edge>`
   - `npx cypress run --spec <cypress\e2e\MyTest.js>`
   - `npx cypress run --debug`
   - `npx cypress run --reporter <mocha, junit, nyan, json`>`
   - `npx cypress run --screenshots`
   - `npx cypress run --ci-build-id <id>`
   - `npx cypress run --headed`

   _Adding npm Scripts_

   ```json
   {
     "scripts": {
       "cypress:open": "npx cypress open --record --browser chrome"
     }
   }
   ```

### Project Structure

```struture
cypress
├── fixtures  Stores data for tests (e.g., for data-driven testing).
│   └── data.json
├── support (Custom commands or Cypress configuration modifications)
│   ├── commands.js (Custom Cypress commands)
│   ├── e2e.js (Global setup/teardown for E2E tests)
├── e2e (End-to-end test files)
│   ├── login.spec.js
│   ├── ...
│   ├── pages (Page Object Model)
│		│   └── LoginPage.js
│   └── common-step-definitions (Cucumber BDD)
│            ├── a.feature
│            └── a.js
├── downloads 
│   └── records.csv (Possibly downloaded results)
├── screenshots
│   └── app.cy.js/ (Test-specific folders)
│       └── Navigates to main menu (failures).png
├── videos
│   └── app.cy.js.mp4
├── reporters (Custom Mocha reporters)
│   └── custom.js
├── plugins  (Configuring plugins)
│   └── index.js
├── .prettierrc.json
├── node_modules (Cypress Module)
├── cypress.config.js (Default configuration file (can be overridden))
├── package-lock.json
├── package.json (Cypress Dependencies)

```

## Plugins
### Prettier
Install prettier for automated code formatting
```
npm install cypress prettier
```
Prettier Configuration: 
Create a `.prettierrc` file to define formatting preferences (e.g., no semicolons, single quotes, tabs).
```JSON
{
  "semi": false,
  "singleQuote": true,
  "useTabs": true,
  "tabWidth": 2,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "trailingComma": "es5"
}

```
  > Prettier Configuration Options:
  > - `"semi": false`: Disables semicolons at the end of statements
  > - `"singleQuote": true` Single quotes for strings
  > - `"useTabs": true` Uses single quotes for strings
  > - `"tabWidth": 2` Indents with tabs, each tab being 2 spaces wide 
  > - `"bracketSpacing": true` Spaces around brackets in object literals and function calls
  > - `"arrowParens": "avoid"` Avoids parentheses around arrow functions unless needed for readability
  > - `"trailingComma": "es5"` Trailing commas allowed in object literals for ES5 compatibility

### XPath

```
npm install -D cypress-xpath
```

support file/`e2e.js`:

```
require('cypress-xpath');
```

## End-2End Testing

### Assertions
Assertions are used to verify the expected behavior of your web application. Compare the actual state of the application with the expected state.
- **Implicit Assertions**: Use _built-in Cypress commands_ like `.should()` to verify element properties or content.
  - `.should()`: assertion about the current subject
  - `.and()`: chain multiple assertions together`
- **Explicit Assertions**: Use _Chai-jQuery Assertions_ to perform more complex validations.
  - `.expect()`: BDD assertion about a specified subject
  - `.assert`: TDD assertion about a specified subject.

**Common Assertion Types**:`
  > - **Title Assertion:** `cy.title().should('contain', 'Website Title')`: - Assert the page title 
  > - **URL Assertion:** `cy.url().should('include', '/home')`: Assert the current URL
  > - **Existence Assertions:** `.should('exist')` or `.should('not.exist') to verify the presence or absence of an element.
  > - **Visibility/Hidden Assertions**: `.should('be.visible')` or  `.should('be.hidden')` to check if an element is visible on the screen
  > - **Disabled/Enabled Assertions:** `.should('be.disabled')` OR `.should('be.enabled')` disabled or enable this element from input
  > - **Attributes Assertions**: Assert the value of element attributes
  >     - **Text Content:** `.should('have.text', 'Text Content')`: Assert the text content of an element.
  >     - **Value**`.should('have.value', 'ABC')`: Assert the value of an input field.
  >     - **Length** `.should('have.length', number)`: Assert length of an array or string
  >     - **CSS**: `.should('have.css', 'property', 'value')`: Assert the CSS class on an element.
  >     - **Attr**: `.should('have.attr', 'href', 'expectedHref')`
  > - **State Assertions**
  >   - **Checked**`.should('be.checked')`: Assert Radio/Checkbox is checked
  >   - **Selected:** `.should('be.selected')`

### Locator

- **Locators**: Web page elements have corresponding components in the DOM used for identification. Locators identify elements based on these DOM components.
- Opening the DOM:
  - Right-click element > `Inspect`
  - Press `F12` key
- Locator Strategies: Prioritize `ID`, then `Name`, then `Class` for uniqueness.
- Constructing Locators:
  - `CSS`: Faster than XPath. (e.g., `input[name="password"]`).
  - `XPath`: Slower than CSS, use only if CSS fails. (e.g., `//input[@name="password"]`).
### Waiting
- 1. Waiting for a Specific Amount of Time:
- 2. Waiting for a Specific Network Request:
### UI

### Actions

- `.type()`: Types text into input fields or textareas.
- `.focus()`or `.blur().`: Focus and blur elements
- `.clear()`: Clear input fields
- `.submit()`: Submit forms
- `.click()`: Click elements.Handles hidden/disabled elements.
- `.dblclick()`/`.rightclick()`: Double-Click/Right-Click
- `.check()`/`.uncheck()`: Radio/Checkbox Selection
- `.select()`: Dropdown Selection
- `.scrollIntoView()`/ `cy.scrollTo()`: Scroll elements (scroll in view / scroll the window specific position)
- `.trigger()`: Trigger input range

### Aliasing

- Aliasing DOM Elements:
  - `cy.get('#Seleclor').as('aliasName')`: Assign an alias to a DOM element to avoid repeating
  - `cy.get('@aliasName')`: Retrieves the same button element again using the alias.
- Aliasing Network Requests:
  - `cy.intercept('GET', '**/comments/*').as('@aliasName')`: Use `.intercept()` to define an alias for a specific network request (GET, POST, etc.) based on URL patterns.

### Connectors

- `.each()`: iterate over an array of elements
- `.its()`: get properties on the current subject
- `.invoke()`: invoke a function on the current subject
- `.spread()`: spread an array as individual args to callback function
- `.then()`

### Cookies

- `cy.getCookie()` - get a browser cookie
- `cy.getCookies()` - get browser cookies for the current domain
- `cy.getAllCookies()` - get all browser cookies
- `cy.setCookie()` - set a browser cookie
- `cy.clearCookie()` - clear a browser cookie
- `cy.clearCookies()` - clear browser cookies for the current domain
- `cy.clearAllCookies()` - clear all browser cookies

## Cypress Framework Designing

### Hook

Hooks in Cypress: Functions that execute before or after test cases, used for setup and teardown.

- Useful set up pre-conditions and clean up post-conditions for tests.
- 4 types of hooks (execution Order)
  - Use `before()` initial setup before all tests.
  - Use `beforeEach()` setup before each individual test.
  - Use `afterEach()` Runs after each individual test.
  - Use `after()` Runs after all tests are complete.

### Test Data with Fixture (Json, Excel, Csv)

#### JSON - Reading data
- **Test data**: The input values used to test software functionality.
- **Hardcoding test data**: A poor practice that makes tests inflexible and difficult to maintain.
- `Fixtures`: Files containing test data, often in JSON format, to organize and reuse data effectively.
- **Steps for using fixtures in Cyprus**:
  1. **Create a JSON file** within the `fixtures` folder:
     `fixtures/LoginData.json`
     ```
     {
         "email":"cypressdemo@gmail.com",
         "password":"cypressdemo",
     }
     ```
  2. **Load the fixture & Access test data** in your test:
     `TC_Login.cy.js`
     ```
     cy.fixture('LoginData').as('data')
     cy.get('input[type="email"]').type(this.data.email)
     ```

#### EXCEL - Reading Data

- **Excel** can also be used to manage test data.
  - Manage test data in Excel: Familiar tool for many users.
  - Organize data in sheets: Separate test cases or data categories.
  - Keep tests concise: Avoid hardcoding data in test files.
- **Read Data from Excel in Cypress**:

  1.  **Install xlsx**: `npm install xlsx`
  2.  **Create file Excel**: `fixtures/Data.xlsx`
  3.  **Create `plugins` folder**: _root directory_

  - `plugins/read-xlsx.js`: Contains code to read Excel data and convert into JSON rows.

    ```
    const fs = require("fs");
    const XLSX = require("xlsx");

    const read = ({ file, sheet }) => {
      const buf = fs.readFileSync(file);
      const workbook = XLSX.read(buf, { type: "buffer" });
      const rows = XLSX.utils.sheet_to_json(workbook.Sheets[sheet]);
      return rows;
    };

    module.exports = {
      read,
    };

    ```

  - `plugins/index.js`: Registers the readXlsx task for use in Cypress tests

    ```
    const readXlsx = require("./read-xlsx");

    module.exports = (on, config) => {
        on("task", {
            readXlsx: readXlsx.read,
      });
    };
    ```

  4. `cypress.config.js`: Requires the `plugins/index.js` file to enable custom plugins

     ```
     const { defineConfig } = require("cypress");

     module.exports = defineConfig({
     e2e: {
     		baseAPIUrl: "https://reqres.in",
     		testIsolation: true,
     		experimentalStudio: true,
     		setupNodeEvents(on, config) {
     			return require('./cypress/plugins/index.js')(on, config)
     		},
     	 },
     });
     ```

  5. **Load Data from Excel & Access**

  - `TC_ReadExcel.cy.js`: Create a variable to store test data & Call `cy.task('readXlsx', { fileName, sheetName })`, call the Excel reading task. to Fetch data
    ```
    var data;
    beforeEach(function () {
      data = cy
        .task("readXlsx", {
          file: "cypress/fixtures/conduitExcelData.xlsx",
          sheet: "Sheet1",
      })
      .then((rows) => {
        data = rows;
      });
    });
    ```
  - Access data using `data[row][column]` (e.g., `data[0].username`).
    ```
    cy.get('input[name="email"]').type(data[0].username);
    cy.get('input[name="password"]').type(data[0].password);
    ```

#### CSV - Reading Data

- **CSV for test data**: Simple format for storing tabular data.
  - Simple format: Easy to create and maintain CSV files.
  - Readable data: Data is organized in a clear tabular structure.
  - No external dependencies: CSV is a standard format, no need for additional libraries.
- **Steps**:

1. **Install the neat-csv plugin**: `npm install -d neat-csv@5`
2. **Create a CSV file**: `fixtures/CSVData.csv` structure data in columns, separated by commas.
3. **Read Data from CSV in Cypress**:
   ```
   //Import the plugin in your test file
   const neatCsv = require('neat-csv')
   describe("CSV test", function () {
    var table;
    beforeEach(function () {
      // Load the CSV data using cy.fixture
      cy.fixture('CSVData.csv')
      .then(neatCsv)
      .then(data => {
        table = data
      })
    });
   ```
4. **Access into data**
   ```
   // Use email and password in your test
   cy.get('input[type="email"]').type(table[0].username);
   ```

### Custom commands & Global Configurations setup

#### Create Custom commands

- Problem: Repeated login code in multiple tests leads to redundancy and wasted time
- **Custom commands**: Reduces the login code from multiple lines to a single line per test:
  - Reduces code duplication and improves test maintainability.
  - Makes tests more concise and easier to read.
- **Steps**
  1. Create a Custom Command:
  - `cypress/support/commands.js`
    ```
    // Custom command to log in a user
    Cypress.Commands.add('login', (email, password) => {
      cy.get('input[name=email]').type(email);
      cy.get('input[name=password]').type(password);
     cy.get('form').submit();
    });
    ```
  - Import `import './commands'` in `cypress/support/index.js` to make the custom commands available
  2. Call the Custom Command in Tests:
     ```
     cy.login('user@example.com', 'password123')
     ```

#### Global Configurations setup

- Accessed from the Cypress Test Runner under Project Settings -> Resolved Configurations.
  - Examples:
    - `baseUrl`: Base URL for website under test (can be set in `cypress.config.js`).
    - `screenshotOnFailure`: Take screenshots on test failures (default: `true`).
    - `video`: Record video of test runs (default: `true`).
- **Overriding Configurations**:
  - **`cypress.config.js`**: Edit default values for the entire project.
    - Set `screenshotOnFailure` and `video` to `false` in `cypress.config.js`.
    - Set `baseUrl` to the Conduit website URL.
  - **Test File**: Override specific configurations this test only using `Cypress.config()`.
    ```
    Cypress.config({ baseUrl: 'yourUrl' }) //set a custom base URL
    ```
    - _(Incorrectly tries to override `video` to `true`, but Cypress doesn't allow overriding read-only configurations)_

### Page Object Model

- Problem: Maintaining tests becomes difficult when locators (code used to find UI elements) change frequently.
- **Page Object Model (POM)**: A design pattern that helps manage UI automation code.
  - Separate classes (Page Objects) for each web page.
  - Centralize locators and actions within Page Objects.
  - Benefits:
    - Improved maintainability: Locators are stored in one place (page object files).
    - Reusability: Page objects can be reused across multiple test cases.
    - Readability: Tests are easier to understand due to clear separation of concerns.
- **Implementation POM**:

  - Page Classes: Represent individual pages of the application.
  - Locators: Stored within page classes for easy access and updates.
  - Methods: Encapsulate actions performed on page elements (e.g., clicking buttons, entering text).
  - **Setup POM**:

  1. Create a `page` object class for each web page. - `e2e/pages/LandingPage.js`:

     ```
     class LandingPage{              //Class: Define a class for the page, PascalCase name

       getSignin(){return 'Sign in'} //Locator Methods: return locators for elements

       clickSigninButton(){         // Action Methods: perform actions on those elements
         cy.contains(this.getSignin()).click()
       }
     }
     export default LandingPage    // Export default
     ```

  2. Create Test Spec: Use the page object class methods in your test cases. - `e2e/TC_LandingPage.cy.js`:

     ```
     import LandingPage from '../pages/landingPages' // Import the necessary page classes.
     describe('POM Test', () => {
      const landingPages = new LandingPage()        // Create instances of the page classes

      beforeEach(function () {
      	cy.visit('/')
      })

      it('Conduit Login', () => {                  // Write test cases using methods from the page classes
      	landingPages.clickSigninButton()
      })
     })
     ```

## Dashboard Setup

- **Cypress Dashboard**: cloud-based: Run, Maintain, Record, Fix Tests in CI Faster and Easier
  - Parallelization: Run tests faster on multiple machines simultaneously.
  - Grouping: Organize tests for easier management and reporting.
  - Insights: Analyze test performance to identify bottlenecks.
  - Easy Setup: Record, manage, and monitor tests with screenshots and videos.
  - CI/CD Integration: Run tests in Jenkins, GitHub, Docker, etc.
  - Test Optimization: Visualize execution and optimize load balancing.
  - Automatic Balancing: Distribute tests across machines for best performance.
  - GitHub Integration: Run tests automatically on pull requests for faster feedback.
    > 📌Limitations: Paid service: Free tier with limited features (50 users, 500 tests/month). Paid plans offer more features and higher limits.
- **Set up Cypress Dashboard**:
  1. Open Test Runner: Run `npm cypress open`
  2. Connect to Cypress Cloud Project:
     - "Runs" tab
     - Log in using GitHub or Google.
     - Create a new project (or choose an existing one).
     - _Copy command_ is displayed to run dashboard
  3. Update Configuration
     - `projectId` is automatously added to the `cypress.config.js` file.
     - _Paste command_ is provided to be added to the `package.json` file.
       ```
       "scripts": {
           "test-dashboard":"npx cypress run --record ..."
       }
       ```
  4. Run Tests with Recording: `npm run test-dashboard`
  5. View Test Results
     - After test completion, a unique URL is provided.
     - Open the URL in a browser to view the Cypress Cloud dashboard.
     - The dashboard displays details of each test (passed, failed, skipped) with screenshots and videos for failures.

## Mochawesome Report Set up

### Mochawesome report

- Mocha Awesome:
  - A reporter for the Mocha JavaScript testing framework.
  - Generates interactive HTML/CSS reports.
  - Completely free of cost, unlike Cypress dashboard.
  - Alternative to Cypress's paid dashboard feature.
- Features:
  - Free: No cost unlike Cypress's dashboard.
  - Easy to integrate: Works with existing Mocha frameworks.
  - Clean and modern design: Simple yet informative reports.
  - Nested test support: Shows details for individual tests and suites.
  - Before/after hook details: Displays execution status of before/after hooks.
  - Stack traces for failed tests: Provides detailed error information.
  - Filters: Filter reports by test status (e.g., failed only).
  - Mobile-friendly: Responsive design for viewing on any device.
  - Offline viewing: Can be saved and viewed without an internet connection.

### Set up Mochawesome report

1. **Install dependencies:**

- Mocha: `npm install -D mocha`
- Cypress-multi-reporter: `npm install -D cypress-multi-reporters`
- Mocha Awesome: `npm install -D mochawesome`
- Mochawesome-merge (optional): `npm install -D mochawesome-merge` and `npm install -D mochawesome-report-generator`

2. **Configure `Cypress.config.js`**:

- Add a reporter tag set to `cypress-multi-reporters`
- Define reporter options within an object:
  - Set `reporterEnabled` to `mochaAwesome`
  - Configure `mochaAwesomeReporterOptions`:
    - Set `reportDir` to the desired report directory (e.g., `cypress/reports/mocha`)
    - Set other options like `quiet`, `overwrite`, `html`, and `json` as needed

3. **Add scripts to `package.json`**

- `clean:reports`: Cleans the report directory before each test run.
- `pretest`: Runs `clean:reports` before tests start.
- `combined:reports`: Merges individual JSON reports into a single report.
- `generate:report`: Generates the Mocha Awesome report from the merged JSON.
- `posttest`: Runs `combined:reports` and `generate:report` after tests finish.
- `test`: Runs tests and executes post-test scripts.
  ```
  "scripts": {
    "cypress run": "cypress run",
    "clean-reports": "if [[ $OSTYPE == 'msys' ]]; then rmdir /s /q cypress/reports; else rm -rf cypress/reports; fi && mkdir cypress/reports && mkdir cypress/reports/mocha",
    "pretest": "npm run clean-reports",
    "combined-reports": "mocha-merge cypress/reports/mocha/*.json > report.json",
    "generate-report": "marge cypress/reports/mocha/report.json -f -o cypress/reports/mocha",
    "posttest": "npm run combined-reports && npm run generate-report",
    "test": "npm run cypress run | npm run posttest"
  }
  ```

4. **Run tests:**
   > 📌(Manually create **"`reports`" folder** if running for the first time)

- Execute `npm run test` to run the tests and generate the report
- View generated report in browser

### Add Failed test screenshot in Mochawesome report

1. **Update `Cypress.config.js`:**

- Change the screenshotsFolder to cypress/reports/mocha-report/assets.

2. **Add Event Listener in `support/e2e.js`:**

- Import `addContext` from `mocha-awesome`.
- Add a `cy.on('test:after:run', (test, runnable) => { ... }) task`.
- Inside the `task`:
  - Check if `test.state === 'failed'`.
  - Construct the screenshot path using `assets/<span class="math-inline">\{Cypress\.spec\.name\}/</span>{runnable.parent.title} // ${test.title} (failed).png.`
  - Call `addContext(test, screenshot)` to attach the screenshot to the report.

3. **Run Tests:**

- Execute `npm run test` to run tests and generate the report.

4. **View Report:**

- Open the generated Mocha Awesome report to see failed tests with attached screenshots.

## API Testing

### API Testing

- APIs (Application Programming Interfaces):
  - Enable communication and data exchange between software systems.
  - Act as the backbone of web UIs, handling actions within websites.
  - It's essential for web UIs, as every action on a website triggers API calls to the server.
- API Testing:
  - 3 layers of in a typical application: Presentation (UI), _Business (API)_, Database.
  - Validating APIs to check the functionality, reliability, performance, and security of APIs.
  - Involves sending calls to the API, observing responses, and comparing them to expected results.
  - Focuses on the _business logic layer_, different from GUI testing (doesn't focus on the UI)
  - API testing tools include `Postman`, `Katalon`, `Rest Assured`, `Apache JMeter`, `Karate`, and `SoapUI`.
  - Cypress allows direct API testing without third-party tools.

### Rest API

- RESTful APIs (Representational State Transfer API):
  - API Built on the REST architectural style (Representational State Transfer).
  - Provide simple access to web services for developers.
  - Enable creation of diverse web applications with CRUD functionalities (Create, Read, Update, Delete).
  - Using HTTP methods with server interactions.
- CRUD operations with REST APIs
  - REST APIs are used to develop web applications that involve CRUD operations (Create, Read, Update, Delete) on data.
- HTTP Methods in REST APIs
  - `GET`:  retrieve information or resources from the server.
    ```
    it('GET Method',function(){
      cy.request('GET','https://reqres.in/api/users?page=2').then((response)=>{
          expect(response.status).equal(200)
          expect(response.body.data[0].first_name).equal('Michael')
      })
    })
    ```
  - `POST`: create new resources, often represented as adding data to a database.
    ```
    it('POST Method',function(){
      var user={
          "name": "morpheus",
          "job": "leader"
      }
      cy.request('POST','https://reqres.in/api/users',user).then((response)=>{
          expect(response.status).equal(201)
          expect(response.body.name).equal(user.name)
          expect(response.body.job).equal(user.job)
      })
    })
    ```
  - `PUT`: update existing resources, modifying existing data.
    ```
    it('PUT Method',function(){
      var user={
          "name": "vignesh",
          "job": "tester"
      }
      cy.request('PUT','https://reqres.in/api/users/2',user).then((response)=>{
          expect(response.status).equal(200)
          expect(response.body.name).equal(user.name)
          expect(response.body.job).equal(user.job)
      })
    })
    ```
  - `DELETE`: remove resources.
    ```
    it('DELETE Method',function(){
      cy.request('DELETE','https://reqres.in/api/users/2').then((response)=>{
          expect(response.status).equal(204)
      })
    })
    ```

## Mocks & Stubs

### Mocks?

- Mocks are a type of test level that simulate the behavior of external dependencies(e.g., APIs, databases) without relying on their actual response.
- Verify that a call was made without relying on the actual response from the dependency.
- Used in both unit and e2e testing to mock API responses.
- Benefits of Using Mocks
  - Accurate test results: Tests are independent of real API responses.
  - Isolation: Mock responses can be controlled, enabling testing of specific scenarios, even edge cases.
  - Faster test execution: Tests run independently of external systems-> faster execution times.
  - Reliable testing: Mocks are always available, even if backend servers are down or unstable.
  - Testing edge cases: Mocks can simulate specific scenarios or error conditions that are difficult to reproduce in real environments.
  - No backend dependency: Tests can run without a functioning backend server
- Example: A web UI (front-end) that interacts with a backend server. If the server returns an error, the front-end test might fail due to an unexpected response. Mocks can provide a simulated (but controlled) API response, ensuring the front-end test runs successfully regardless of the server's state.

### Using mocks in real time

1. **Identify the API calls to mock:**

- Use the Chrome DevTools (Network tab) to identify the API calls made by the application.
- Pay attention to the request method (GET, POST, etc.), URL, and response data.

2. **Create Mock Data `fixture` file:**:

- Create a JSON file in your Cypress project's `fixture` folder to store the mock response data.
- This file will contain the data you want to return instead of the actual API response.

3. **Write the Cypress Test mock the API call & Add Assertions**:

- Use the `cy.intercept()` method to intercept the API call and provide the mock response data.
- Specify the request method (e.g., `'GET'`, `'POST'`), URL pattern (e.g., `'/api/tags'`), and the `fixture` file containing the mock data.
- After mocking the API, write assertions like `cy.get()` and `should()` interact with the UI elements and assert their state.

  ```
    it('Article Mock',function(){
      cy.intercept('GET','/api/articles/*',{fixture:'Mocks-Article.json'})
      cy.contains('Your Feed').should('be.visible')
      cy.contains('Global Feed').click()
      cy.get('.col-md-9').should('contain','Maksim Esteban')
  })

  it('Login User Mock',function(){
      cy.intercept('POST','/api/users/login',{fixture:'Mocks-Login.json'})
      cy.get('a[href^="#@"]').should('contain','Maksim Esteban')
  })
  ```

## Docter Intergration with Cypress

### Docker

- Docker is a leading platform for creating and using software containers.
- Containers bundle everything an application needs to run (code, libraries, system tools) into a single package.
- This makes applications portable and efficient to deploy across different environments.
- Benefits of Docker:
  - Simplified Deployment: Build an application once and run it anywhere with Docker. No need to configure the app for different platforms.
  - Consistent Testing: Test applications in a container mimicking the production environment, reducing surprises when deploying.
  - Portability: Docker containers run seamlessly on Windows, Mac, Linux, and other platforms.
  - Version Control: Docker provides built-in version control, allowing developers to revert to previous versions if needed.
- Docker streamlines application development and deployment by offering a standardized approach to packaging and running applications.

### Install Docker

1. **Download the Installer**: [Install Docker Desktop on Windows](https://docs.docker.com/desktop/install/windows-install/).
2. **Install Docker**: Run the downloaded installer and clicking "Next"
3. **Verify Docker Desktop**: Check the window taskbar for a small whale icon. **Right-click** it. **See "Docker Desktop is running"** with a green symbol, Docker is installed successfully.
4. **Additional Work after Docker Installation on Windows**: It will throw error related to Kernel update. To fix [Windows-WSL2-Install](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)

### Running Cypress Tests inside Docker

- Prerequisites:
  - Docker Desktop installed and running
  - Cypress project with tests written
- Steps:
  1. **Open Terminal:**
  - In Visual Studio Code, go to Terminal and create a new terminal.
  - **Windows**: Ensure the terminal is set to Command Prompt (not PowerShell).
  2. **Run Cypress Test:**
  - Verify Docker Container Status: `docker ps -a`
  - Execute Cypress Tests: `docker run -it -v "%cd%":/e2e -w /e2e --entrypoint=cypress cypress/included:latest run`
    > 📌Instead of `cypress/included:latest` -> Replace `cypress/included:12.12.0`- actual Cypress version installed on laptop. Mac: (Same command, replace `%CD%` with `$PWD`)
    >
    > - `docker run`: Creates a new Docker container.
    > - `-it`: Allows interactive input and pseudo-tty allocation.
    > - `-v`: Creates a volume to share the current directory (%CD% for Windows, $(pwd) for Mac) with the /e2e directory inside the container.
    > - `-w /e2e`: Sets the working directory within the container to /e2e.
    > - `--entrypoint=cypress`: Overrides the default container entry point with cypress.
    > - `cypress/included:latest`: Specifies the Docker image containing Cypress (pulls the latest version if not already present).
    > - `run`: Executes Cypress within the container, running the tests.
    > - `--browser <browser_name>`: Run tests in a specific browser (Chrome or Firefox)

### Docker File & Docker Compose

- Dockerfile and Docker Compose, essential tools for building and running containerized applications.
- Dockerfile creates individual Docker images.
- Docker Compose simplifies managing multi-container applications by defining their configuration in a single file.

#### Dockerfile

- A text file containing instructions for building a Docker image.
- Defines the base image, software installations, environment configuration, and more.
- Components of a Dockerfile:
  1. Base Image: The foundation for your image (e.g., Ubuntu, Alpine Linux, Node.js).
  2. Instructions: Commands to install software, configure environment:
      - `FROM`: Specifies the base image.
      - `RUN`: Runs commands to install software, configure environment, etc.
      - `COPY` or `ADD`: Copies files from the host machine into the image.
      - `WORKDIR`: Sets the working directory inside the container.
      - `EXPOSE`: Defines ports the container listens on.
      - `CMD` or `ENTRYPOINT`: Sets the default command to run when the container starts.
  3. Layering: Each instruction creates a layer in the image. Only changed layers are rebuilt on modifications, making builds efficient.
  4. Best Practices: Minimize layers, use non-root user, clean up temporary files, use multi-stage builds.
- **Implementing DockerFile for WINDOWS:**
  ```#Below in place of <Cypress Version> replace with cypress version installed in your system
  FROM cypress/included:<Cypress Version>
  WORKDIR /app
  COPY . /app
  RUN npm install
   ```

#### Docker Compose

- A tool for defining and running multi-container applications.
- Uses a single YAML file (`docker-compose.yml`) to specify services, networks, and volumes.
- Components of a Docker Compose File:
  - **Services**: Individual containers with details like image, ports, volumes, etc.
  - **Networks**: Custom networks for container communication.
  - **Volumes**: Persistent data storage between container runs using named volumes or bind mounts.
  - **Environmental variables**: Configuration settings for containers.
  - **Scaling**: Running multiple instances of a service for load balancing and scaling.
  - **Dependencies**: Defines dependencies between services (e.g., web service depending on a database service).
  - **Command line interface (CLI)**: Provides commands to manage containers and services (start, stop, rebuild, scale).
  - **Port mapping**: Specifies port mapping between containers and the host machine for external accessibility.
  - **Configuration** overrides: Using multiple compose files for different environments
- **Implementing DockerCompose for WINDOWS:**
  ```
  version: '3'
  services: 
  e2e:
    image: cypress
    build: .
    container_name: cypress
    command: npx cypress run
    volumes:
      - ./:/app
  ```
- Run the Docker Compose command: `docker-compose up` 
## Cucumber - BDD Framework

## CICD Intergration
### CICD
- CI/CD is a continuous improvement process, not a magic solution. 
- **CI (Continuous Integration)**: Code from multiple developers is merged into a central repository frequently (multiple times a day). Automated processes check code quality, build code, run tests and run automated tests (unit tests), and generate reports. 
- **CD (Continuous Delivery/Deployment**): An extension of CI where code changes are automatically prepared for release. 
  - Continuous Delivery: Code is automatically deployed to testing environments and undergoes further testing (UI, performance, security). Deployment to production is manual. 
  - Continuous Deployment: Every successful change is automatically deployed to production without human intervention. 
- Benefits: 
  - Higher quality: Fewer bugs due to early detection and automated testing. 
  - Lower risk releases: Less chance of major bugs reaching production. 
  - Lower cost: Fixing bugs in development is cheaper than in production. 
  - Faster time to market: Quicker delivery of new features and updates. 
- Popular tools: Jenkins, GitLab, CircleCI, Travis CI 
### Jenkins? 
  - **Jenkins:**
    - Jenkins is a powerful and versatile CI/CD tool with user-friendly features and extensive plugin support. 
    - Its distributed architecture enables efficient scaling and builds for various platforms. 
    - An open-source automation server for continuous integration and continuous delivery (CI/CD). 
    - Written in Java, runs on Windows, macOS, and Unix-like OS. 
    - Key Features: 
      - Easy Installation: Self-contained, ready-to-run with packages for various OS. 
      - Easy Configuration: User-friendly web interface with error checks and built-in help. 
      - Plugins: Hundreds of plugins in its update center, integrating with various CI/CD tools. 
      - Distributed: Distributes work across multiple machines for faster builds, tests, and deployments. 
  - **Jenkins Pipelines**
    - Automates the process from code commit to deployment, covering CI and CD. 
      1. Development (code push) 
      2. Code commit 
      3. Building code
      4. Running tests 
      5. Releasing code 
      6. Deploying code to production 
  - **Jenkins Architecture** 
    - Master: 
      - Pulls code from the version control system (e.g., Git) upon code commit.
      - Distributes tasks (build, test) to slaves via TCP/IP. 
    - Slaves (Agents): 
      - Execute builds and tests on request from master (build, test). 
      - Produce test reports. 
    - Benefits of Distributed Architecture: Enables parallel execution of tasks across multiple machines, significantly speeding up builds and tests. 