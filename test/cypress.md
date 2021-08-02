# Cypress

## Overview

{% hint style="info" %}
### What you'll learn <a id="What-you-ll-learn"></a>

* What Cypress is and how it helps with testing
* Basic usage
{% endhint %}

Cypress is a JavaScript End to End Testing framework.

{% embed url="https://www.cypress.io" %}

It builds on Mocha and Chai, which are baked into it.

## Installation

* `describe` and `it` come from [Mocha](https://mochajs.org/)
* `expect` comes from [Chai](http://www.chaijs.com/)

**Install:**

```text
npm install cypress --save-dev
```

**Run:**

```text
# Using full path:
./node_modules/.bin/cypress open

# Or with npm bin shortcut:
$(npm bin)/cypress open
```

**Add to package.json scripts:**

```text
{
  "scripts": {
    "cypress:open": "cypress open"
  }
}
```

**Run from project root:**

```text
npm run cypress:open
```



