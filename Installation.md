# Installation #

Installation is easy: Copy the class to a convenient place and include it from your script. Define the rules and messages using an array and initiate the class. Validate the `$_POST` array and it will return error messages when found.

# Details #
The implementation follows the logic provided by the jquery validation plugin available at http://bassistance.de/jquery-plugins/jquery-plugin-validation/. You can initiate an instance by passing it an options array, e.g.
```
$asOption = array(
    'rules' => array(
        'name' => 'required',
        'email' => array(
            'required' => '#name',
            'email' => true
        )
    ),
    'messages' => array(
        'name' => array( 'required' => 'name is required'),
        'email' => array(
            'required' => 'if you have a name, also provide your email',
            'email' => 'this is not an email address'
        )
    )
);
```
The keys refer to keys in (for instance) the POST array. As seen with the email.required option, the class supports some basic jquery selectors.

Then feed it to the validator:
```
$oValidator = new Validator($asOption);
```

And give it something to validate:
```
$oValidator->validate($_POST);
```

Validate() will return an array [=> error\_message](field.md) containing all invalid fields, and an empty array when all is well:
```
count($oValidator->validate($_POST)) > 0 ? 'errors':'no errors';
```

or validate a single element:
```
$oValidator->element('email', ''); #valid as it is only required iff name is present
$oValidator->element('email', 'foobar'); # invalid as it is not an email address
```
Element() will return bool valid

You can now also use this $asOption to power the js-validation, provided your (input type=)names match the ids:

```
(function ($){
    $(document.forms[0]).validate(<?php echo json_encode($asOption); ?>);
} (jQuery));
```

This is only a preliminary introduction, hope this helps. More examples will follow (hopefully) soon!