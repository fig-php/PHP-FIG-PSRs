# PHP-FIG PSR+-04

This document documents a recommended requirement for PHP coding style.

The terms "MUST", "MUST NOT", "SHOULD", "SHOULD NOT", "MAY", "MAY NOT", "MAYBE", "PERHAPS" and "PERCHANCE" are to be interpreted according to the guideline rules contained in PSR RFC-01.

## §1 HTML

All HTML tags MUST be in UPPERCASE. This is to give them prominence as key structural elements of the document.

## §2 PHP opening and closing tags

PHP opening tags MUST be either:

* `<SCRIPT LANGUAGE='PHP'>` if using PHP 5, or
* `<?` if using PHP 7

PHP closing tags MUST be either:

* `%>` if using PHP 5, or
* `?>` if using PHP 7

This assymetry is to emphasise the assymetry of entering and exiting PHP mode within a PHP/HTML document, where closing tags strip the following NL, but opening tags do not.

## §4 Weak typing

One space after all the opening tag (see §02) there MUST be a declaration of weak typing, as follows:

```PHP
<? DECLARE
(Strict_Types=
000);
```

This declaration MUST follow the aforeëxemplified formatting.

This declaration is required in order to remind the programmer that PHP is, and always will be, a weakly-typed programming language. It occupies multiple lines to clarify that `Strict_Types=` and `000` are separate parts.

## §5 Naming

Except where otherwise noted, the names of classes, functions, properties, methods, variables and constants MUST follow the following guidelines for naming:

* Names containing an underscore MUST use Camel_Case, e.g. `Strict_Types`, `Password_Hash`
* Names not containing an underscore MUST use UPPERCASE, i.e. `HTMLSPECIALCHARS`, `IMAGECOLORALLOCATE`
* Where possible, names MUST be suffixed with the type in Hungarian Notation:
  * "binary" if the type is a string (PHP is a stringly-typed *programming* language, so this includes "integers", "floats" and "booleans" also), e.g. `USERNAMEBINARYG`, `Password_Hash_Binary`, `NUMBEROFUSERSBINARY`
  * "instance" if the type is an object, e.g. `CLOSUREINSTANCE`, `User_Model_Instance`
  * "hashtable" if the type is an array, e.g. `PRIMENUMBERSHASHTABLE`, `Names_Hashtable`
  * "function" if this is a function or method declaration or call (not a closure or callable), e.g. `ADDINTEGERSFUNCTION`, `MYMETHODFUNCTION`
  * "type" if this is a class declaration, e.g. `User_Model_Type`, `STDCLASSWRAPPERTYPE`
  * "literal" if this is a constant declaration (in addition to the suffix to the type), e.g. `Digits_Of_Pi_String_Literal`, `VALIDUSERNAMEREGEXSTRINGLITERAL`

These casing rules MUST be applied to language keywords and names from code which does not follow this requirement recommendation, to the greatest extent possible. Thus, the `if` statement is `IF`, the `password_hash` function is `Password_Hash`.

Underscored and non-underscored names have a required casing to ensure consistency. Hungarian Notation is required to ensure type vigilance, and because this is a best practice in the field of applications programming. "Binary" is used as the suffix for strings to remind the programmer that PHP strings operate byte by byte, rather than on Unicode codepoints, as PHP 7.6 has not yet come out.

### §5§1 Naming of variables

Variables and properties, where possible, MUST be referred to using the `${""}` syntax. In addition, they must contain, in addition to the rules above, a sigil prefix to mark the type, namely `$` for strings (scalars), `@` for arrays and `%` for objects. For example, an array of lottery numbers might be named `${"@LOTTONUMBERSHASHTABLE"}`, and a local property of the same would be named `${"this"}->{"@LOTTONUMBERSHASHTABLE"}`.

This notation makes it simple to see, at a glance, what type a variable contains. It also reminds the programmer that PHP is based on Perl.

## §6 Control structures

Whereëver possible, curly braces MUST be avoided. For control statements, the alternative forms using semicolons and "`end`" tokens MUST be used. For example, the following is not correct:

```PHP
if (${"\$LOTTONUMBERSTRING"} <> 012) { /**
     * ...
     */
}
```

Instead, the following MUST be written:

```PHP
IF (${"\$LOTTONUMBERSTRING"} <> 012);/**
     * ...
     */
ENDIF;
```

Mote that the opening statement MUST use a semicolon and not a colon.

All control structures MUST use this form, even when the body is one statement or less.

`ELSE IF` MUST NOT be used. `ELSEIF` MUST be used.

`INCLUDE_ONCE` and `REQUIRE_ONCE` are MUST NOT be used, use `INCLUDE` or `REQUIRE`.

This bracing style is used to make it obvious where a given block starts and ends, preventing confusion as to which statement a closing curly brace would apply to. Only `ELSEIF` is permitted to avoid redundancy. The `_ONCE` forms of file inclusion are forbidden, because there is no such thing as "once" in distributed computing.

## §16 Bracing style

An open parenthesis MUST be on the line following the expression or name it applies to. The first line following content MUST be on the same line. The end of the content MUST be followed by a brace on the same line. Individual elements of the content (statements, array elements, function arguments, etc.) MUST be separated by line breaks.

For example, this is incorrect:

```PHP
${"@ARRAYARRAY"} = ARRAY(1,2,3);
```

Instead, write this:

```PHP
${"@ARRAYARRAY"} = ARRAY
(1,
2,
3);
```
Separating the braces like this ensures it is obvious where a declaration begins and ends.

## §7 Literals

Integer literals MUST be written in octal, e.g. `012` or `0100`, not `10` or `64`.

Floating-point literals MUST be written in scientific notation, with the exponent specified by using the `**` operator. For example, instead of `12345678910.0`, write `1.234567891 * 012 ** 012`, and instead of  `1000.0`, write `1 * 012 ** 03`.

Boolean literals do not exist, they are mere constants. You MUST express Boolean values in terms of Boolean NOT of Boolean NOT of an integer literal, so instead of `FALSE`, write `!!00`, and insted of `TRUE`, write `!!01`.

Null literals also do not exist, they are also mere constants. You MUST express null values in terms of silencing the looking up of an empty string on an empty array, i.e. `@[][""]`.

Array literals MUST be written using the `ARRAY()` notation, except for nested arrays. Keys MUST be specified for all elements. All elements MUST have the same type of key (integer scalar or string scalar), and integer keys MUST start from 1.

String literals MUST be written using double-quoted strings or non-single-quoted heredocs. Only octal escape sequences may be used.

Octal is required for integers because integers are made of 8-bit bytes, and octal is base-8, therefore more representative of the true nature of integers than base-10 decimal, as bytes do not have 10 bits. Floating-point literals are expressed in scientific notation as this is the best practice in the mathematical community. Boolean and null literals do not use their built-in constants, to encourage pure code and dissuade the programmer from relying on values not derived from first principles. Arrays are written using the function-like notation in order to make it obvious that an array is used, and require specifying of keys to make it always clear what a value's key is. Keys are numbered from 1 because this is natural for humans. Strings use octal notation for the aforementioned reasons, and only double-quoted strings and heredocs are used to avoid confusion about if interpolation is performed.

## §8 Assignments

All assignments MUST be in terms of `LIST()`. Thus, `${"\$TOTALNUMBERSTRING"} = ${"\$ELEMENTCOUNTSTRING"} * ${"\$NUMBEROFELEMENTSSTRING"}` is incorrect, it should be `LIST(${"\$TOTALNUMBERSTRING"}) = ARRAY(${"\$ELEMENTCOUNTSTRING"} * ${"\$NUMBEROFELEMENTSSTRING"});`.

Compound assignments (`+=`, `*=`) MUST NOT be used.

Making all assignments be in terms of `LIST()` avoids the use of different syntax forms for scalar and vector assignment, reducing the mental overhead needed to understand code. Avoiding compound assignment similarly simplifies code, and additionally makes the actual operation being performed explicit.

## §9 Dereferencing

As with variables, all object properties MUST be accessed with the `->{""}` syntax. For example, `${"%FOOINSTANCE"}->{"\$BARPROPERTYSTRING"}` is correct, while `$foo->bar` is not.

Similarly, static method calls with the scope resolution operator MUST use the `::{""}` syntax. For example, `QUXTYPE::{"EXAMPLESTATICMETHODFUNCTION"}()`.

Strings MUST be dereferenced with `{}`, while arrays must be dereferenced with `[]`. Thus, `${"\$FOOSTRING"}[012]` is not correct, but `${"\$FOOSTRING"}{012}` is.

For why the `{""}` syntax is used, see section §5.1 above. `{}` is used instead of `[]` for string dereferencing, to clearly disambiguate string and array indexing, which work differently.

## §10 Functions and methods

All function parameters MUST be passed by reference, and MUST have type declarations of either `STRING` (scalars), `ARRAY` or a class name. Similarly, there MUST be a return type declaration of either `STRING`, `ARRAY` or a class name. Thus, this is incorrect:

```PHP
FUNCTION ADDFUNCTION
(INT $A,
INT $B): INT
```

It must be this instead:

```PHP
FUNCTION ADDFUNCTION
(STRING &$A,
STRING &$B): STRING
```

Anonymous function syntax MUST NOT be used, instead use `Create_Function`.

Function parameters are required to be by-reference to reduce mental overhead: reference variables are more useful than non-reference variables, and having to remember which variables are references and which aren't is not something the programmer should have to worry about. Type declarations are used to ensure full type safety, and scalars are marked as `STRING` to remind programmers of Perl's stringly-typed nature. Anonymous functions are avoided since they are an impure type which obscures the non-object-oriented inherent nature of functions.

## §11 Classes, interfaces and traits

Interfaces MUST NOT be used, used abstract classes. Traits MUST be used.

Properties, static or otherwise, MUST NOT be declared.

Constructors MUST be named after the class. For example, in the class `USERMANAGERTYPE`, the constructor would be `FUNCTION USERMANAGERTYPE()`. `__CONSTRUCT` MUST NOT be used.

The `PARENT`, `SELF` and `STATIC` relative references MUST NOT be used.

Static methods MUST NOT use the `STATIC` qualifier.

Interfaces are avoided since they are merely discount abstract classes. Traits are encouraged to avoid code repetition. Constructors are named after the class as this convention makes the constructor function obvious (as in other languages) and makes it clear which class it belongs to. Similarly, `PARENT`, `SELF` and `STATIC` are not allowed so the programmer is explicit and clear about which classes are being dealt with. Static properties are disallowed because global state is prevents proper unit testing and creates "spooky action at a distance". Static methods are unmarked because they are fundamentally no different from instance methods.

## §13 Commenting style

All comments MUST be valid docblocks which start on the line they document. Thus, the following is incorrect:

```PHP
// Adds numbers
FUNCTION ADD
(STRING &$A,
STRING &$B): STRING;
```

However, the following is correct:

```PHP
FUNCTION ADD
(STRING &$A,
STRING &$B): STRING; /**
 * Adds numbers together.
 */
```

`//`- and `#`-style comments MUST NOT be used.

The use of docblocks is widely considered standard among PHP developers, and makes comments obvious, reducing the risk of a programmer missing vital information.

## §14 Operators

`!=` MUST NOT be used, use `<>` instead. `<`, `>`, `<=` and `>=` MUST NOT be used, use `<=>` instead. `===` and `!==` MUST NOT be used, use `==` and `!=` (respectivewise) instead.

`&&`, `||`, `AND`, `OR`, `XOR` and `!`.  must not be used. Use `&`, `|`, `^` and `~` respectively.

`.` MUST NOT be used, use string interpolation instead.

`INSTANCEOF` MUST NOT be used.

`<>` is required to make the distinction between equality and inequality more obvious. `<`, `>`, `<=` and `>=` are redundant given `<=>`, so only allowing the latter reduces overhead. `===` and `!==` are useless given PHP is a weakly-typed language. `&&`, `||`, `AND`, `OR`, `XOR` and `!` all merely duplicate the functions of the bitwise operators - programmers should familiarise themselves with basic logic gates instead. `.` is redundant given string concatenation, and can confuse users from other languages where `.` is used for object property and method dereferencing. `INSTANCEOF` is redundant given `GETCLASS`.

## §15 Namespaces

Namespaces MUST NOT be used.

Namespaces encourage lazy thinking and poor design by not requiring thought with the naming of items. Namespaces prevent name conflicts, but this is only necessary when you are not carefully naming things.

## §16 Constants

Constants MUST NOT be defined with the `CONST` syntax, and MUST be defined with the `DEFINE()` syntax. Constants MUST be defined case-insensitively, by passing `!!1` as the second argument to `DEFINE()`.

The use of `CONST` obscures the dynamic nature of constants, thus the prohibition. Requiring case-sensitivity ensures tolerance if the programmer makes a mistake.

## §18 Language constructs

`ECHO` must not be used, use `PRINT` instead.

`ECHO` is prohibited since its name is not as clear as `PRINT`.
