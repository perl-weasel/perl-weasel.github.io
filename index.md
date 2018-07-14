# Bridging DOM with Page Objects

## Web application testing in Perl

Weasel is a library to automate web application tests using Perl, much like [Watir](http://watir.com) for [Ruby](http://ruby-lang.org) and [Mink](http://mink.behat.org/en/latest/at-a-glance.html) for [PHP](http://php.net/).

Like [Mink](http://mink.behat.org/en/latest/at-a-glance.html), it abstracts away differences between browser emulators and browser drivers. Unlike any other web testing library, it bridges the gap between page elements (DOM elements) as returned by the web drivers and [page objects](page-objects) used to model your tests.

Weasel integrates with Perl's [BDD](https://en.wikipedia.org/wiki/Behavior-driven_development) testing framework ('[pherkin](https://github.com/pjlsergeant/test-bdd-cucumber-perl)') through its [pherkin-extension-weasel](https://github.com/perl-weasel/pherkin-extension-weasel) extension.

## Bridging the Page Object gap
[Page objects](page-objects) are a way to reduce the dependency between the exact layout of your pages and the implementation of your tests. Webapp testing drivers return DOM elements, but tests want page objects to access the page's services.

Weasel solves this problem: In Weasel, Page Objects *are* DOM elements. Developers build page objects as derivatives of a DOM primitive `Weasel::Element`, letting Weasel know how to recognize page sections as page objects. Then, when a page object queries the page for a specific element, it's automatically mapped to a page object. The page object is then returned.


## Using Weasel without page objects
