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

* Browser driver (`Weasel::DriverRole`)
* *Weasel* Elements alias DOM elements (`Weasel::Element`)
* *Weasel* Elements DOM search patterns (`Weasel::FindExpanders`)
* DOM element to *Weasel* Elements mappers (`Weasel::WidgetHandlers`)
* 

