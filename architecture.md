# Architecture

*Weasel* models a hierarchy of page objects on any given page to be tested.
A typical *Weasel* model for a login page could be:

```plain
LoginPage
   |
   +-> LoginSection
   |     |
   |     +-> Username control (Weasel::Widgets::HTML::Input/regular input mode)
   |     +-> Password control (Weasel::Widgets::HTML::Input/password mode)
   |     +-> Production/Test environment selector (Weasel::Widgets::HTML::Select)
   |     \-> Login button (Weasel::Widgets::HTML::Button)
   +-> OtherSection1
   \-> OtherSectionN
```

where each level of the hierarchy owns a set of children. Each child itself may
own a number of children. Each level encapsulates DOM internals into services
for the next higher level.

The need for the abstraction of the individual widgets becomes apparent when
the application switches from the use of SELECT tags to [DojoToolkit's
FilteringSelect]() or [Vuetify Selects](https://next.vuetifyjs.com/en/components/selects).
When making such a switch, the basic service of the widget remains the same
("select a value from a list"), but the DOM internals completely change.
*Weasel*'s widgets handle the abstraction of internals of (small) parts of
a page for the higher levels in the hierarchy to use.

## Concepts

### *Weasel*::Element

Elements represent a part of a page. They have 2 roles with associated
methods and properties:

1. Elements fullfil the role of DOM tags
2. Elements fullfil the role of PageObject

*As tags*, elements have methods to search for child-DOM elements, query and set
tag properties and all other operations generally supported by web
application testing frameworks.

*In their role of PageObjects*, elements (called Widgets in this role, but note
that this is the same object in a different role!) encapsulate DOM internals
and offer services to tests or to other PageObjects. E.g. "select a value from
a list" as offered by the SELECT tag (and all re-implementations in all web
frameworks thereof).

In order to increase encapsulation of DOM internals into Widgets, each Widget
publishes a CSS/XPath search pattern using an alias. This keeps the details of
the DOM internals inside the Widget while the search pattern can be used by
other Widgets without breaking the encapsulation.

* *Weasel*::FindExpanders

FindExpanders are functions that, given a set of arguments, return an XPath
expression to be used for finding one or more PageObjects of a given type.

One of the FindExpanders that Weasel comes with (`*password`), is implemented
as:

```perl
sub password_expander {
    my %args = @_;

    my @clauses;
    for my $clause (qw/ id name /) {
        push @clauses, "\@$clause='$args{$clause}'"
            if defined $args{$clause};
    }
    my $clause = @clauses ? join ' and ', ('', @clauses) : '';

    return ".//input[\@type='password' $clause]";
}
```

Which defines a function which takes two named arguments (`id` and `name`)
and returns an XPath expression to find an INPUT tag of type PASSWORD,
with the value for either or both of the attributes `id` and `name` specified
as given.

Later, this expander is registered as:

```perl
register_find_expander('*password', 'HTML', \&password_expander);
```

Code can then use the password expander without knowledge of the DOM
internals as:

```perl
  my $pwd_widget = $self->find('*password', name => 'login_password');
```

-------------------
* DOM element to *Weasel* Elements mappers (`Weasel::WidgetHandlers`)
* 

* Browser driver (`Weasel::DriverRole`)
