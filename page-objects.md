# PageObjects

The concept of *Page Objects* has been introduced as an abstraction layer between web application tests and the DOM that results from the web application's implementation: tests (especially BDD tests) should test for functionality, not exact DOM structure.

The basic idea is that a web page offers *Services* and that the services are the only thing which need intimate knowledge of the DOM structure ([as explained on the Selenium wiki](https://github.com/SeleniumHQ/selenium/wiki/PageObjects)). These services will be encapsulated in an object which knows how to map manipulation of those services to manipulation of the DOM.

## Example

An example of a PageObject could be the following `LoginPage` (translated from the Java example on the SeleniumHQ wiki):

```perl
package Test::MyWebApp::LoginPage;

use Moo;

has driver => (is => 'ro');

sub loginAs {
   my ($self, $user, $passwd) = @_;
   
   # Set the user and password and click 'Login'
   # Assert that the next - expected - page pops up
   # and return the next page object (HomePage?)
}
```

The tests then can access the login page as easy as:

```perl

# Return a homepage
sub test_login {
   my ($driver) = @_;
   my $login = Test::MyWebApp::LoginPage->new(driver => $driver);
   return $login->('testuser', 'testpassword');
}
```
