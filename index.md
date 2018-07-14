# Bridging DOM with Page Objects

## Web application testing in Perl

Weasel is a library to automate web application tests using Perl, much like [Watir](http://watir.com) for [Ruby](http://ruby-lang.org) and [Mink](http://mink.behat.org/en/latest/at-a-glance.html) for [PHP](http://php.net/).

Like [Mink](http://mink.behat.org/en/latest/at-a-glance.html), it abstracts away differences between browser emulators and browser drivers. Unlike any other web testing library, it bridges the gap between page elements (DOM elements) as returned by the web drivers and [page objects](page-objects) used in tests.

Weasel integrates with Perl's [BDD](https://en.wikipedia.org/wiki/Behavior-driven_development) testing framework ('[pherkin](https://github.com/pjlsergeant/test-bdd-cucumber-perl)') through its [pherkin-extension-weasel](https://github.com/perl-weasel/pherkin-extension-weasel) extension.

## PageObjects
[Page objects](page-objects) are a way to reduce the dependency between the exact layout of your pages and the implementation of your tests. Webapp testing drivers return DOM elements, but tests want page objects to access the page's services.

## Improving the PageObjects pattern
There are a [number of issues](page-object-issues) with the standard pattern that **Weasel** solves:

* No hard-coded page flow  
  Because the Weasel library knows how to map DOM elements to PageObjects,
  there's no need for PageObjects to know the type of the next page in the flow
* Well defined component re-use strategy  
  PageObjects need not represent an entire page; they represent an one or more
  services provided by a specific part of the page -- hence similar services in
  multiple pages can be abstracted out in a single PageObject
* Easy changing or replacing PageObjects  
  The CSS/XPath patterns required to find page objects are registered with Weasel
  under 'mnemonics' (a descriptive alias) -- when the implementation of the PageObject
  changes, only the CSS/XPath attached to the mnemonic needs to change, but none of
  the users of the component need to change
* Easily extensible
  **Weasel** comes with a set of PageObjects which encapsulate standard HTML tag services
  such as the SELECT and INPUT tags; replacement widgets (such as DojoToolkit's `FilteringSelect`)
  can add their CSS/XPath pattern to the `*select` mnemonic, ensuring either will be found



---------------------




## Bridging the Page Object gap

Weasel solves this problem: In Weasel, Page Objects *are* DOM elements. Developers build page objects as derivatives of a DOM primitive `Weasel::Element`, letting Weasel know how to recognize page sections as page objects. Then, when a page object queries the page for a specific element, it's automatically mapped to a page object. The page object is then returned.


## Using Weasel without page objects
