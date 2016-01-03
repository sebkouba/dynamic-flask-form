# Dynamic Flask Form
Having struggled to find a workind example of a WTForm in Flask with
a dynamic form, I created this one. It is cobbled together from a few
Stackoverflow and Google Group discussions.

### Basic Concepts
- FieldList and FormField are used to nest one Form inside another
- **`phones`** is a relation in the **`User Model`** creating a link to **`Phone entries`**.
- **`PhoneForm`** deals with the Phone fields only
- **`CombinedForm`** includes the **`User`** and **`Phone`** fields
- The `CombinedForm` is populated through the `User` model and related `phones`


### Pain Points
#### User Model
I had named the `phones` relation `phone` for a while which broke the populate_obj
function. The naming must match for the mechanism to work. Duh.

#### PhoneForm
This is the form that is nested in the 'main' Form. It is not directly exposed
or rendered. A common problem here is the CSRF token being missing from this form.
Options to solve this problem are [explained here](http://stackoverflow.com/questions/15649027/wtforms-csrf-flask-fieldlist).
I chose to subclass wtforms Form which does not require the token.

#### CombinedForm
`phones = FieldList(FormField(PhoneForm, default=lambda: Phone()))`
That is the line of code where it all broke down for me. Once there is a working
example it's hopefully not so mysterious. Setting the default to `lambda: Phone()`
was difficult for me to get to (see [Fixing populate_obj](https://groups.google.com/forum/#!msg/wtforms/5KQvYdLFiKE/TSgHIxmsI8wJ))

#### Index
Simply loading one User and taking it from there is a means to focus on the actual
problem. Messing with a dropdown for different users just distracts from the main
point here.

I'm not sure if aassing a new Phone instance where the User has no phones is the
best possible solution but it meant I could keep using the JavaScript Code as it was.

#### Javascript
I simply fixed example code that I found. There is probably a neater way of copying the fields
that I haven't found. I would gladly take suggestions in this case as well as the rest of the code.


### Resources
[Half working example](https://gist.github.com/kageurufu/6813878)
[Fixing populate_obj](https://groups.google.com/forum/#!msg/wtforms/5KQvYdLFiKE/TSgHIxmsI8wJ)
[Error display in View](http://wtforms.simplecodes.com/docs/1.0.1/crash_course.html#displaying-errors)
[FormField Documenation](http://wtforms.simplecodes.com/docs/0.6.1/fields.html#wtforms.fields.FormField)