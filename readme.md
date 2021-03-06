# EZ Modal

### Easy Modals for Meteor

```
$ meteor add hitchcott:ez-modal
```

This package adds an `EZModal` helper to the client. When called, it spawns a Bootstrap 3 modal within the DOM, and comes with a number of customisations in terms of content and display.

It uses `Blaze.renderWithData` to provide a clean reactive appendature to the DOM, and it removes itself from the DOM when the modal is closed.

It supports dynanmic templates for header, body and footer.

Please ensure you have Bootstrap 3 (and it's javascript) enabled within your project. Built and tested with  [mizzao:bootstrap-3](https://github.com/mizzao/meteor-bootstrap-3).

## Examples

Check out http://ez-modal.meteor.com/ for some interactive examples.


### Simple Usage
```coffeescript
EZModal 'Hello World'
```
### Pass a second parameter that is run when the modal is closed
```coffeescript
EZModal 'Thank you', -> console.log 'modal closed'
```
### Pass an object instead of a string for more options
```coffeescript
EZModal
  title: 'Sorry!'
  body: 'You are not authorized to do that'
  classes: 'purple'
  fade: false
  backdrop: false
```

### Generate Buttons

```coffeescript
Template.checkout.events
  'click .empty-basket' : ->
    EZModal
      title: 'Please Confirm'
      body: 'Are you sure you wish to empty the cart?'
      leftButtons: [
        color: 'danger'
        html: 'Cancel'
      ]
      rightButtons: [
        color: 'primary'
        html: 'Yes'
        fn: (e, tmpl) ->
          emptyBasket()
          @EZModal.modal('hide')
      ]
```
### Specify custom templates by passing a template key

```coffeescript
EZModal
  # Pass reactive data to templates with optional dataContext
  dataContext: someData
  headerTemplate: 'myHeaderTempalte'
  bodyTemplate: 'myBodyTemplate'
  footerTemplate: 'myFooterTemplate'
```
### Chain Modals
```coffeescript
# The 'EZModal' jquery refernce is automatially added to template context
# It allows you to hook into modal events from the child templates
Template.myFooterTemplate.events
  'click .submit' : ->
    # @EZModal part of 'this' scope
    @EZModal.modal('hide').on 'hidden.bs.modal', ->
      # New EZ Modal spawn after old one is hidden
      EZModal
        'title' : "Thank You"
```
### Do other stuff with the $ object
```coffeescript
$myModal = EZModal
  show: false
  body: 'Testing'

$myModal.modal('show')
```

### Form

EZModal now has support for forms. Use `EZFormModal` and specify a template with a `<form>` as the root node. The `<form>` should *not* include a submit button. The callback will return a serialized data object based on the input `name`s.

```
EZFormModal
  title: 'Send Transaction'
  template: 'txForm'
  data: { from: defaultAccount.address }
, (err, data) ->
  console.log('got data', data)
```

## API

```coffeescript
EZModal

    # styling
    classes: String # classes added to modal div
    size: null # use `sm` or `lg` for larger or smaller width
    fade: true # fade in/out animation
    backdrop: true # show dark backdrop overlay

    # simple content
    title: String # text to appear in the modal head (if template undefined)
    body: String # text to appear in the modal body (if template undefined)
    bodyHtml: String # html to appear in the modal body (if template undefined)

    # templates
    dataContext: Object # data to pass to templates & helpers
    template: String # template key for whole modal; if defined, overrides below
    headerTemplate: String # template key for header area
    bodyTemplate: String # template key for body area
    footerTemplate: String # template key for footer area
    hideFooter: false # footer visibility

    # generate buttons in footer area
    # float left
    leftButtons: [ as below ]
    # float right
    rightButtons: [
        html: String # button text 
        color: String # button class eg. primary / danger
        fn: Function # click event handler
    ]
    buttonColor: options.buttonColor || 'primary'
    buttonSize: options.buttonSize || null
    hideFooter: false # footer visibility
    buttonColor: String # eg. default
    buttonSize: String # eg. lg / sm


    # misc
    show: true # show modal on init
    keyboard: false # allow keyboard (esc) to hide

 , ->
   # callback on modal close
```

## Credits

[Chris Hitchcott](http://github.com/hitchcott), Oct 2014

MIT License 2014
