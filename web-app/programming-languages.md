# Testing

* **Test Pyramid**
  * User Interface: client-side interface where the user accesses the system
  * Integration: REST API's and third-party integrations
  * Unit: independent parts of code functionality
* Further reading: [https://martinfowler.com/articles/practical-test-pyramid.html](https://martinfowler.com/articles/practical-test-pyramid.html)

### Front-end Unit Testing

* Karma
  * Browser-based testing
  * Spawns a web server and executes source code against test code
  * Configuration file \(usually `karma.conf.js`\) specifies which files to watch
  * Triggers tests whenever files change
  * Reports results back to the server
    * Can be published to Coveralls.io \([https://coveralls.io](https://coveralls.io)\)
  * Developed by the Angular team
  * [https://karma-runner.github.io](https://karma-runner.github.io)
  * Install \(globally\): `npm install -g karma-cli`
  * Install \(in project\): `npm install karma@latest –save-dev`
  * Initialize \(in project\): `karma init`
* Mocha
* Chai
  * [https://www.chaijs.com](https://www.chaijs.com)
* Jest
* Jasmine

## Mocking Functions

## Mocking Data

## Mocking Async Calls

## Mocking Rendered Components



