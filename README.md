checkargjs
==========

**Checkarg.js is a mixin for underscore.js that provides literate argument checking.**

To install just copy checkarg.js into your js directory and include it in a script tag.

Note: This library is intended for lightweight function argument checking, it is not intended
a full-featured form-validator.

##Example

```javascript
function example(optional, aString, alertIfUndef) {
    // An optional argument provided with a default. (using _ OOP wrapper)
    this.optional = _(optional).checkarg().withDefault("default");

    // A mandatory string argument. Validate it is a string, and throw an error if it isn't.
    this.aString = _.checkarg(aString).withValidator(_.isString).throwNoArg("aString");

    // An argument that may be undefined, but should be noted if it is. Note the use of check()
    // to allow the 'undefined' value to pass through.
    this.alertIfUndef = _.checkarg(alertIfUndef).failure(function(a) {
        console.log("example:alertIfUndef argument undefined");
        return a;
    }).check();
}
```

##Usage

To use call `_.checkarg(arg)` on your function argument in a chain terminating in one
of the following methods:

    withDefault(default): Will validate and return `arg` if validation succeeds or
        return `default` if it fails.

    throwError(message): Will validate and return `arg` if validation succeeds or 
        `throw new Error(message)` if it fails.

    throwNoArg(fieldname): A helper method that is equivalent to 
        `throw new Error("Argument missing: " + fieldname)`

    check(): A helper method that is equivalient to calling `withDefault(undefined)`.
        This is probably only useful in conjunction with the customisation hooks below.

There are three customisation hooks to allow customisation of the validation/return path.

    validator(validator): `validator` is of type `arg -> boolean`. Call this to use a
        custom validation function in the terminating methods.  The default is !_.isUndefined.

    failure(callback, [context]): `callback` is of type `arg -> arg|void`. It will be invoked
        whenever `arg` fails to validate prior to either returning its result or throwing an
        Error depending on the terminating method.

    success(callback, [context]): `callback` is of type `arg -> arg|void`. It will be invoked
        whenever `arg` validates prior to returning its result.

Note that where necessary a default value can be differentiated from an equivalent passed value
based on which callback is invoked.

## Contributors
 - [Recurse](http://github.com/recurse) - Initial Author and official recipient of any blame.

## LICENSE

(MIT License)
Copyright (c) 2013 The University of Queensland, Australia

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
