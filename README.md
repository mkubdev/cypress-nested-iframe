# Using Cypress with iframes
 ⚙️ An example of how to use [Cypress](https://www.cypress.io/) to target nested elements within iframes.

With limited iframe support from Cypress [[Issue #136](https://github.com/cypress-io/cypress/issues/136)], the following workaround in this repo allowed to target elements and interact with iframes during tests.

### Explanation

The solution is to create an iframe command that returns a promise upon iframe loading completion. These commands, aliased as `iframe()`, can be chained together to deal with nested iframes.

```js
// support/commands.js

Cypress.Commands.add('iframe', { prevSubject: 'element' }, $iframe => {
  return new Cypress.Promise(resolve => {
      $iframe.ready(function() {
        resolve($iframe.contents().find('body'));
      });
  });
});
```

Here is an example of usage:

```js
// One iframe
cy.get("#iframe").iframe().find("#target").type("HELLO WORLD");

// Nested iframes
cy.get("#firstFrame").iframe().find("#secondFrame").iframe().find('#target').type('HELLO WORLD');
```

### Links

[Cypress.io](https://www.cypress.io/)
