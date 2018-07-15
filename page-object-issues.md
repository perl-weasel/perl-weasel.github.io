# PageObject issues

The PageObject pattern is a great move forward to disentangle the
DOM structure internals from the functional web application tests.
Most examples provided on the web solve only one problem where there
are a number to be solved.

Lets start by looking at [the summary of the PageObject pattern
principles from the SeleniumHQ
wiki](https://github.com/SeleniumHQ/selenium/wiki/PageObjects#summary)
(emphasis mine):

1. The public methods represent the services that the page offers
2. **Try not to expose the internals of the page**
3. Generally don't make assertions
4. **Methods return other PageObjects**
5. _Need not represent an entire page_
6. Different results for the same action are modelled as different methods

The principles above are employed to serve the design goal:

> Within your web app's UI there are areas that your tests interact with.
> A Page Object simply models these as objects within the test code.
> This reduces the amount of duplicated code and means that if the UI
> changes, the fix need only be applied in one place.



---------
There is a contradiction in the two bold-faced principles:
a page object should not be exposing its internals, yet *other* page objects
should be able to locate page objects on the page and create instances for
those page objects.
