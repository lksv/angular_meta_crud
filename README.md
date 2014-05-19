Angular Meta CRUD
=================

The purpose of this project is to design set of libraries for fast development of
CRUD interfaces that are rendered on the client side.

Particularly:

* Generating forms
* Breadcrumbs
* Model (resource) metadata description
* Basic controller for CRUD operations (like
  [inherited_resources](https://github.com/josevalim/inherited_resources)
  on the client side.
* Design metadata format (in JSON) which can be used by server side:
  * to provide automatic form field (e.g. model field) validation
  * to generate a form
  * to describe routes (to be able to show breadcrumb, properly render
    and navigate among associations and controller's actions)


This idea was inspired and influenced by ruby gems:

* inherited_resources
* [SimpleForm](https://github.com/plataformatec/simple_form) and [Formtastic](https://github.com/justinfrench/formtastic)
* CRUD interfaces
  * [basepack](https://github.com/lksv/basepack)
  * rails_admin
  * active_admin

## Road Map

1. Describe overall design
2. Divide into separate (small) components which could be used individually
3. Model the API for recieving the metadata from the server. Define / choose JSON data structure format
4. Implement designed components

### Resource

Resource is a basic entity which represents data model.

Resource consists of:

* *fields* (e.g. `first_name`, `surname`, `email`)
* *actions* (e.g. `list`, `create`, `read`)
* *validations* (NOTE: not sure) -  validations are usually bound to
  the fields, but sometimes you need validations related to several
  fields (for example Ruby on Rails uses :base).
* *forms* - This defines structure how model (resource) is presented (rendered) for end user. It can be used for forms or parts of views. Each resource should have different views for different actions (e.g. `show`, `edit`) as well
  as several views for the same action (e.g. "view items as list", "view
  items as "grid of icons" or "fast edit", "detail edit").
  Let's use term *Form* for forms as well as the template rendered
  for `show` action (e.g. without the ability to edit) or even template
  renedered for `index` action).
* *Nesting* and routes. Resource could be nested in another resource (e.g. "Project has many
  tasks" and inverse "Tasks belongs to project"). Nesting of resources
  may or may not be reflected in (REST API) URLs of the resource. Most of the time
  nesting is reflected in the breadcrumb and you can access
  the same resource by several URLs.
* *authorization* Set of rules how to authorize a resource. (e.g. only some users can perform some resource
  actions => e.g. some users are not allowed to edit resource so they should not see "Edit" button). The presented
  resource view is dependant on the user roles (e.g. admin can see
  resource view with all fields, operator see almost all fields and customer see few particular fields).

### Field
Consist of:

* Field type: Some of the field types are text, string, integer, boolean, association, etc. Field type is used for storage of
  the field content.

  Special field types are associations:
  * belongs_to association - it should be presented as selectbox or offer possiblity to create new item.
  * has_many association - it should be presented as selectbox(multiple:
    true) or nested form.

* Filed presented type: How to present the value to the user in the form for example string type could be Richtext, textarea, textline.
  Filed type `text` could be presented as `<textarea>` or wysiwyg editor to the user.

* validations
* read_only
* virtual_fields(?) - e.g. getter/setter (on the client and the server, or server only).
* stripping(?) - e.g. spaces are removed form beginning and end of the input.

* Field behaviour:
  Visibility could be related to:
  * authorization
  * value of another fields (for Angular lets say "state of the form model")

The filed attributes are really numerous. Some of them shoud be more a
attributes of the form (e.g. show to show) and some of the model.

### Action

Notes:
* In app it is an:
  * action buttons in form and show and index page like 'Edit', 'Update', 'Show', 'Delete' but also 'Back to Overview', 'Cancel'
  * should be put in the nav bar.

Should be represented by:
* action name
* method (GET, PUT, POST, PATCH, DELETE)
* params:
   * name
   * mandatory/optional
* authorization (access controll) - on the client side it means (more or less) visibility control
* action label
* action icon



### Resource Nesting

Notes:
* resources could be nested by several routes
* user is usually aware of nesting by breadcrumb and/or nested form
  (e.g. gem [nested_form](https://github.com/ryanb/nested_form))
* nested resources usually reflects an resource association

In controller usually implemented by:
* array of paths. Each path define possible nesting.
e.g.

`Comment` is nested in `[Project, Task]`

or

`Comment` is nested in `[Project, Task] and [Project] and [File]`
The url can be:
```
/project/1/task/13/comments
/project/1/comments
/file/1/comments
```

## Form
For this project we are focused to the *Forms*.

Generaly Form consists of containers. This containers may be futher
nested. For example Form has several tabs and each tab should have
several sections (groups).

Form consist of fields. Each field contains all the information
how to present it to the user, how to build its model and the validation.

Form field has following attributes:

* required:
   * name   - model name
   * label
   * type -  input type (not field type, what will be rendered)
* common:
   * id
   * value  - default value for model name
   * placeholder
   * tooltip (help)
* futher:
  * editable / read_only / hidden / visible
  * required
  * options - for select box
  * template

## CRUD requirements

* I do not want to write validation rules twice (on server and clinet). See [Formtastic]() or [Simpleform]().
* Server side validation - invalid fields are propagated to the form.
* DRY: define common actions like (create, update, destory) by directive(?).
  See [Inherited resources](https://github.com/josevalim/inherited_resources),
  [angular-app crud](https://github.com/angular-app/angular-app/blob/master/client/src/common/directives/crud/crud.js).
  (e.g. update action should send PUT request. When suceess route to defined state and show flash message.
  When sever side validation fails highlight invalid fields (and show error message). etc.)

## Open questions

* How to manage, that only partial metadata shoud be used individually.
  e.g. server shoud provide only metadata for validation and that shoud
  be applied to all the client side forms (for that resource).

## Related projects, articles, APIS
* CRUD Interfaces:
  * [Basepack](https://github.com/lksv/basepack BasePack)
  * [rails admin](https://github.com/sferik/rails_admin)
  * [active admin](https://github.com/gregbell/active_admin)
* Angular Related:
  * [angular-schema-form](https://github.com/gaslight/angular-schema-form/)
    [Building AngularJS Forms with JSON Schema](http://gaslight.co/blog/building-angularjs-forms-with-json-schema)
  * [issue ui-input](https://github.com/angular-ui/angular-ui-OLDREPO/pull/191)
  * [simple field directive](http://plnkr.co/edit/3zMsNnpNfOFwExSqLj2I?p=preview) (in plunkr)
  * [advanced field directive](https://github.com/angular-app/Samples/tree/master/1820EN_10_Code/04_field_directive)
  * [angularjs form builder](https://github.com/Selmanh/angularjs-form-builder)
  * [angular forms - automatic forms generation according schema](https://github.com/mchapman/forms-angular)
  * [AngularJS CRUD Grid Series](http://blog.jongallant.com/p/angularjs-crud-grid-index.html#.U2Sv1HL7vFW)
  * [simple angular form builder](http://fiddle.jshell.net/ProLoser/bp3Qu/) (in jsfiddle)
  * [formular - a little advance form builder](https://github.com/007design/Formular) [example](http://007design.com/formular/)
  * [Angular Gotchas and Dynamic form generation with directives](https://www.youtube.com/watch?v=DnQHWJiB7m0) (second part of the video, 37:00), [part of source code](https://github.com/SmithSamuelM/angular-gold/blob/master/angular-gold.coffee)
  * [Dynamic form, list and components/widgets](https://groups.google.com/forum/#!searchin/angular/dynamic$20form/angular/YFVb5d54dY0/Bbx1EU-qRmUJ) [example](http://plnkr.co/edit/xC5T6d?p=preview)
* Angular Gotchas
  * interpolation of input's name:
    * [Issue #5736](https://github.com/angular/angular.js/issues/5736)
    * solution1: http://stackoverflow.com/a/12044600
    * solution2: custom directive, e.g. see https://github.com/SmithSamuelM/

* APIs:
  * [Specification for APIs](https://github.com/wordnik/swagger-spec)
  * [JSON Schema](http://json-schema.org/documentation.html)
    * [JSON Hyper-Schema](http://json-schema.org/latest/json-schema-hypermedia.html)
    * [JSON Schema Validation](http://json-schema.org/latest/json-schema-validation.html)
  * [JSON Hypermedia Api with forms and links](stackoverflow.com/questions/13542335/json-hypermedia-api-with-forms-and-links)



## TODO:

Read:

* [django modelform](https://docs.djangoproject.com/en/dev/topics/forms/modelforms/)
* [metawidget](http://metawidget.sourceforge.net/doc/reference/en/html/ch03s02.html)
