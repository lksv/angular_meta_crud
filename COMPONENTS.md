
#Template Components

Template usually references:

 * navbar
 * bredcrumb
 * action buttons:
   * top - Add, Filter, Export, Import, e.g. defined on collection
   * bottom - Save, Cancel, Save and Next, Destroy, e.g. defined on resource
 * form
 * view type form switcher -- isn't it part of form itself?


##Navbar
* Left completelly to be deined by user by developer.
* Should be usefull to define automatically dropdown menu with list of all (non-nested)
resources.

## Breadcrumb

Needs to known:

* [association chain](https://github.com/lksv/basepack/blob/eb0e0499a8a4015ca5910f3848993970daa5d621/app/views/basepack/base/_header.html.haml#L7-L22).
  Association chain is also defined by the rote (their url).
* translation for all paremts resources (singular & plural)

## Action Buttons

* solve authorization
* sorted list of buttons, for each is needed:
  * what to render: label, icon
  * Action. It should be:
    * URL & method -- for client site, its bettr to use state than
      URL/route. But it only implies automatic mapping between  resource and route.
    * What controller's action to call. This controllers action somting
      do. If succeed it redirect somewhere and show flash messaged. If
      fails shows flashmessage and stay on the same page.
  * Action should be dynammicaly enabled/disabled. Usually base on
    validation status and dirty checking.
  * confirmation, text (e.g. for Delete action show dialog with "Are you
    sure?")

## Form

Form means any reprezentation of resource or resource collection.
E.g. Not only edit form but also Show template and index page.

One resource can have multiple forms for a particular action. It could
depends on:
* authorization
* user's chois (e.g. index page usually has to switch between views
  items as list, view items as grid of icons).
* resource itself

Form shoud by devided to the groups. Each group has a label.


Form shoul contan nested form.

## View type switcher (form switcher)

* 


