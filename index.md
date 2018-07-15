# Web-app testing in Perl

## Bridging DOM with Page Objects

*Weasel* is a library to automate web application tests using Perl, much like [Watir](http://watir.com) for Ruby and [Mink](http://mink.behat.org/en/latest/at-a-glance.html) for PHP.

Like [Mink](http://mink.behat.org/en/latest/at-a-glance.html), it abstracts away differences between browser emulators and browser drivers. Unlike any other web testing library, it bridges the gap between page elements (DOM elements) as returned by the web drivers and [page objects](page-objects) used in tests.

*Weasel* integrates with Perl's [BDD](https://en.wikipedia.org/wiki/Behavior-driven_development) testing framework ('[pherkin](https://github.com/pjlsergeant/test-bdd-cucumber-perl)') through its [pherkin-extension-weasel](https://github.com/perl-weasel/pherkin-extension-weasel) extension.

## PageObjects
[Page objects](page-objects) are a way to reduce the dependency between the exact layout of your pages and the implementation of your tests. Webapp testing drivers return DOM elements, but tests want page objects to access the page's services.

## Improving the PageObjects pattern
There are a [number of issues](page-object-issues) with the standard pattern that **Weasel** solves:

* **No hard-coded page flow**  
  *Weasel* knows how to map the DOM to PageObjects
* **Well defined component re-use strategy**  
  *Weasel* maps any part of a page to a PageObject, encouraging re-use of PageObjects
* **Easy changing or replacing PageObjects**  
  *Weasel* uses aliases for CSS/XPath PageObject search patterns, flexibly adapting to DOM changes
* **Easily extensible**  
  *Weasel* built-in aliases for standard HTML tags can be replaced with custom implementations

Example code:

```perl
package PageObject::App::Login;

use Moo;
extends 'Weasel::Element';

sub login {
    my ($self, $user, $passwd) = @_;

    $self->find('*labeled', text => 'User')->send_keys($user);
    $self->find('*labeled', text => 'Password')->send_keys($passwd);
    $self->find('*button', text => 'Login')->click;
    
    return $self;
};

1;
```
The [architecture page](architecture) describes how *Weasel* improves on
the generic ideas of PageObjects by providing even better encapsulation
and abstraction.

## Single-Page Applications
The fact that *Weasel* doesn't require a PageObject to model an entire page and encourages multiple PageObjects on a single page (in a nested hierarchy), *Weasel* is very well suited to model interaction with single-page web applications.

## Using *Weasel* without page objects
Although the biggest benefit in *Weasel* obviously is its support for the PageObject pattern, there's no requirement to use its web driver abstraction and CSS/XPath alias support with PageObjects.

## Community resources

* Chat: [Matrix's `perl-weasel` channel](https://riot.im/app/#/room/#perl-weasel:matrix.org)
* Mailing list: [perl-weasel@googlegroups.com](https://groups.google.com/forum/#!forum/perl-weasel) or [subscribe](perl-weasel-subscribe@googlegroups.com)
