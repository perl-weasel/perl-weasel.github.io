# PageObjects

The concept of *Page Objects* has been introduced as an abstraction layer between web application tests and the DOM that results from the web application's implementation: tests (especially BDD tests) should test for functionality, not exact DOM structure.

The basic idea is that a web page offers the user *Services* (actions the application needs to perform). These services will be encapsulated in an object which knows how to map manipulation of those services to manipulation of the DOM.

## 
