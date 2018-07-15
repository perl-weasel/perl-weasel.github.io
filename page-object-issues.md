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

In summary: changes to the DOM internals ("page internals" above) should
impact only a few (preferrably a single) page object's implementation.

All examples around the web show page objects as modelling entire pages.
Although that works for simple pages which only use strict HTML tags,
this becomes a problem when an application is implemented as Single Page
Application (SPA) or when a page uses a widget framework such as
[Vue-widgets](https://github.com/FlowzPlatform/vue-widgets#readme)
or [DojoToolkit](http://dojotoolkit.org/). As widgets from those
toolkits often require specific code for finding the DOM root and
interaction, employing an entire-page page object pattern introduces
code duplication again.

Using the italics marked principle, these widgets can be abstracted
into their own page objects. When separating the design like this,
the services of the widgets (i.e. selecting an option, in case of
a SELECT widget) won't be implemented repeatedly in every page
object using the widget. The principle *Need not represent an
entire page* will likely then become *Most often will represent
part of a page*.




---------
There is a contradiction in the two bold-faced principles:
a page object should not be exposing its internals, yet *other* page objects
should be able to locate page objects on the page and create instances for
those page objects.
