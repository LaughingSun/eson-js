# The ESON Object
adapted from [15.12](http://www.ecma-international.org/ecma-262/5.1/#sec-15.12)

The **ESON** is a class that contains two functions, **parse** and **stringify**, that are used to parse and construct JSON texts. There are static versions of these methods that work identically to the instanctialeted methods and offer bvackward compatibility with the JSON implementations.  While the instance methods offer pre-configured option implementation.  The JSON Data Interchange Format is described in RFC 4627 <[http://www.ietf.org/rfc/rfc4627.txt](http://www.ietf.org/rfc/rfc4627.txt)>. The JSON interchange format used in this specification is exactly that described by RFC 4627 with two exceptions:

*   The top level _JSONText_ production of the ECMAScript JSON grammar may consist of any_JSONValue_ rather than being restricted to being a _JSONObject_ or a _JSONArray_ as specified by RFC 4627.

*   Conforming implementations of **JSON.parse** and **JSON.stringify** must support the exact interchange format described in this specification without any deletions or extensions to the format. This differs from RFC 4627 which permits aa ESON parser to accept non-JSON forms and extensions.

The value of the [[Prototype]] internal property of the ESON class is the standard built-in Object prototype object ([15.2.4](http://www.ecma-international.org/ecma-262/5.1/#sec-15.2.4)). The value of the [[Class]] internal property of the ESON class is `**"ESON"**`. The value of the [[Extensible]] internal property of the ESON class is set to **true**.

The ESON class has a [[Construct]] internal property; which makes it possible to use the JSON object to instantiate pre-configured versions of the 2 methods.  Usage is new ESON( options-object ), which can be dropped in replacing either the JSON object or ESON class.

The ESON class has a [[Call]] internal property; that expects **this** to be an instance that inherits from the ESON class.

# The ESON Grammar is 100% backwards compatible to JSON Grammer (http://www.ecma-international.org/ecma-262/5.1/#sec-15.12.1)

ESON Grammar additonally supports constructors and configurators, as new native-or-custom-class [ '(' [ arguments ] ')' ] and native-or-custom-class.from( eson ), respectively.**ESON** ( `,` .**ESON** )
ESON.stringify produces a String that conforms to the following ESON grammar. ESON.parse accepts a String that conforms to the ESON grammar.


# The ESON Lexical Grammar

ESON is similar to ECMAScript source text in that it consists of a sequence of characters conforming to the rules of _SourceCharacter_. The ESON Lexical Grammar defines the tokens that make up a JSON text similar to the manner that the ECMAScript lexical grammar defines the tokens of an ECMAScript source text. The JSON Lexical grammar only recognises the white space character specified by the production _ESONWhiteSpace_. The ESON lexical grammar shares some productions with the ECMAScript lexical grammar. All nonterminal symbols of the grammar that do not begin with the characters “ESON” are defined by productions of the ECMAScript lexical grammar.

## Syntax

name                        | definition
--------------------------- |:----------
**_ESONWhiteSpace_**        | `/[\x20\f\t\r\n]*/`
**_ESONConstructWord_**     | `new`
**_ESONClassName_**         | /[\w$]+/
**_ESONConfigureWord_**     | `configure`
**_ESONBeginArguments_**    | `(`
**_ESONEndArguments_**      | `)`
**_ESONBeginMembers_**      | `{`
**_ESONEndMembers_**        | `}`
**_ESONBeginElements_**     | `[`
**_ESONEndElements_**       | `]`
**_ESONSeperator_**         | `,`
**_ESONPairer_**            | `:`
**_ESONAccessor_**          | `.`
**_ESONString_**            | `"` **_ESONStringCharacters_**? `"`
**_ESONStringCharacters_**  | **_ESONStringCharacter_** **_ESONStringCharacters_**?
**_ESONStringCharacter_**   | `/[^"\\\x00-0x1F]\|\\(["\/\\bfnrt]\|u[0-9A-Fa-f]{4}|u\{[0-9A-Fa-f]\})/`
**_ESONNumber_**            | `-`? _DecimalIntegerLiteral_ *_ESONFraction_*? _ESONExponentPart_?
**_ESONFraction_**          | `.` _DecimalDigits_
**_ESONExponentPart_**      | ( `E` | `e` ) ( `+` | `-` )? _DecimalIntegerLiteral_ 
**_ESONNullLiteral_**       | _NullLiteral_
**_ESONBooleanLiteral_**    | _BooleanLiteral_


# The ESON Syntactic Grammar

The ESON Syntactic Grammar defines a valid ESON text in terms of tokens defined by the ESON lexical grammar. The goal symbol of the grammar is _ESONText_.

## Syntax

name                        | definition
--------------------------- |:----------
**_ESONText_**              | **_ESONValue_**
**_ESONValue_**             | **_ESONObject_** or **_ESONConstruct_** or **_ESONConfigure_** or **_ESONArray_**
                            | or _ESONString_ or _ESONNumber_ or _ESONNullLiteral_ or _ESONBooleanLiteral_ 
**_ESONObject_**            | _ESONBeginMembers_ **_ESONMemberList_**? _ESONEndMembers_
**_ESONMemberList_**        | **_ESONMember_** ( _ESONSeperator_ **_ESONMemberList_** )
**_ESONMember_**            | _ESONString_ _ESONPairer_ _ESONValue_
**_ESONArray_**             | _ESONBeginElements_ **_ESONElementList_**? _ESONBEndElements_
**_ESONElementList_**       | **_ESONElement_** ( _ESONSeperator_ **_ESONElementList_** )
**_ESONMember_**            | _ESONString_ _ESONPairer_ _ESONValue_
**_ESONConstruct_**         | _ESONConstructWord_ _ESONClassName_ **_ESONArguments_**?
**_ESONConfigure_**         | _ESONClassName_ _ESONAccessor_ _ESONConfigureWord_ **_ESONArgumentList_**?
**_ESONArguments_**         | _ESONBeginArguments_ **_ESONArgumentList_**? _ESONEndArguments_
**_ESONArgumentList_**      | _ESONValue_ ( _ESONSeperator_ **_ESONArgumentList_** )?


# parse ( text [ , reviver ] )

The `**parse**` function parses a ESON text (a JSON-formatted String) and produces an ECMAScript value. The ESON format is a restricted form of ECMAScript literal. ESON objects are realized as ECMAScript objects, constructions or configurators. ESON arrays are realized as ECMAScript arrays. JSON strings, numbers, booleans, and null are realized as ECMAScript Strings, Numbers, Booleans, and **null**. ESON uses a more limited set of white space characters than _WhiteSpace_ and allows Unicode code points U+2028 and U+2029 to directly appear in _JSONString_ literals without using an escape sequence. For parsing, Object constructions and configurations will create an object if the used class is in scope with either a constructor or configurator (static `configure` method respectively.  Likewise for stringify-ing, if there is a constructor or toESON method in the object's prototype, those objects will be stringified using the configurator or constructor as the text.  The process of parsing is similar to [11.1.4](http://www.ecma-international.org/ecma-262/5.1/#sec-11.1.4) and[11.1.5](http://www.ecma-international.org/ecma-262/5.1/#sec-11.1.5) as constrained by the JSON grammar.

The optional _reviver_ parameter is a function that takes two to 3 parameters, (_key_, _value_, _raw_). It can filter and transform the results.  The additional _raw_ parameter will contain the unparsed text. It is called with each of the _key_ and _value_/_raw_ pairs produced by the parse, and its return value is used instead of the original value. If it returns what it received, the structure is not modified. If it returns **undefined** then the property is deleted from the result.

1.  Let _JText_ be #toString(_text_).

2.  Parse _JText_ using the ESON Syntactic Grammar. Throw a **SyntaxError** exception if _JText_ did not conform to the ESON grammar for the goal symbol _JSONText_.

3.  Let _unfiltered_ be the result of parsing and evaluating _JText_ as if it was the source text of an ECMAScript _Program_ but using _ESONString_ in place of _StringLiteral_. Note that since _JText_ conforms to the JSON grammar this result will be either a primitive value or an object that is defined by either an _ArrayLiteral_ or an _ObjectLiteral_.

4.  If [IsCallable](http://www.ecma-international.org/ecma-262/5.1/#sec-9.11)(_reviver_) is **true**, then

    1.  Let _root_ be a new object created as if by the expression `**new Object()**`, where `**Object**` is the standard built-in constructor with that name.

    2.  Call the [[DefineOwnProperty]] internal method of _root_ with the empty String, the PropertyDescriptor {[[Value]]: _unfiltered_, [[Writable]]: **true**, [[Enumerable]]: **true**, [[Configurable]]: **true**}, and **false** as arguments.

    3.  Return the result of calling the abstract operation Walk, passing _root_ and the empty String. The abstract operation Walk is described below.

5.  Else

    1.  Return _unfiltered_.

The abstract operation Walk is a recursive abstract operation that takes two parameters: a *holder* object and the String *name* of a property in that object. Walk uses the value of *reviver* that was originally passed to the above parse function.

1.  Let _val_ be the result of calling the [[Get]] internal method of _holder_ with argument _name_.

2.  If _val_ is an object, then

    1.  If the [[Class]] internal property of _val_ is `**"Array"**`

        1.  Set _I_ to 0.

        2.  Let _len_ be the result of calling the [[Get]] internal method of _val_ with argument `**"length"**`.

        3.  Repeat while _I_ < _len_,

            1.  Let _newElement_ be the result of calling the abstract operation Walk, passing _val_ and[ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_I_).

            2.  If _newElement_ is **undefined**, then

                1.  Call the [[Delete]] internal method of _val_ with [ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_I_) and **false** as arguments.

            3.  Else

                1.  Call the [[DefineOwnProperty]] internal method of _val_ with arguments[ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_I_), the [Property Descriptor](http://www.ecma-international.org/ecma-262/5.1/#sec-8.10) {[[Value]]: _newElement_, [[Writable]]: true, [[Enumerable]]: true, [[Configurable]]: true}, and **false**.

            4.  Add 1 to _I_.

    2.  Else

        1.  Let _keys_ be an internal [List](http://www.ecma-international.org/ecma-262/5.1/#sec-8.8) of String values consisting of the names of all the own properties of _val_ whose [[Enumerable]] attribute is **true**. The ordering of the Strings should be the same as that used by the **Object.keys** standard built-in function.

        2.  For each String _P_ in _keys_ do,

            1.  Let _newElement_ be the result of calling the abstract operation Walk, passing _val_ and_P_.

            2.  If _newElement_ is **undefined**, then

                1.  Call the [[Delete]] internal method of _val_ with _P_ and **false** as arguments.

            3.  Else

                1.  Call the [[DefineOwnProperty]] internal method of _val_ with arguments _P_, the[Property Descriptor](http://www.ecma-international.org/ecma-262/5.1/#sec-8.10) {[[Value]]: _newElement_, [[Writable]]: **true**, [[Enumerable]]:**true**, [[Configurable]]: **true**}, and **false**.

3.  Return the result of calling the [[Call]] internal method of _reviver_ passing _holder_ as the **this** value and with an argument list consisting of _name_ and _val_.

It is not permitted for a conforming implementation of `**JSON.parse**` to extend the JSON grammars. If an implementation wishes to support a modified or extended JSON interchange format it must do so by defining a different parse function.

NOTEIn the case where there are duplicate name Strings within an object, lexically preceding values for the same key shall be overwritten.

# stringify ( value [ , replacer [ , space ] ] )

The `**stringify**` function returns a String in JSON format representing an ECMAScript value. It can take three parameters. The first parameter is required. The *value* parameter is an ECMAScript value, which is usually an object or array, although it can also be a String, Boolean, Number or **null**. The optional *replacer* parameter is either a function that alters the way objects and arrays are stringified, or an array of Strings and Numbers that acts as a white list for selecting the object properties that will be stringified. The optional *space* parameter is a String or Number that allows the result to have white space injected into it to improve human readability.

These are the steps in stringifying an object:

1.  Let _stack_ be an empty [List](http://www.ecma-international.org/ecma-262/5.1/#sec-8.8).

2.  Let _indent_ be the empty String.

3.  Let _PropertyList_ and _ReplacerFunction_ be **undefined**.

4.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_replacer_) is Object, then

    1.  If [IsCallable](http://www.ecma-international.org/ecma-262/5.1/#sec-9.11)(_replacer_) is **true**, then

        1.  Let _ReplacerFunction_ be _replacer_.

    2.  Else if the [[Class]] internal property of _replacer_ is `**"Array"**`, then

        1.  Let _PropertyList_ be an empty internal [List](http://www.ecma-international.org/ecma-262/5.1/#sec-8.8)

        2.  For each value _v_ of a property of _replacer_ that has an array index property name. The properties are enumerated in the ascending array index order of their names.

            1.  Let _item_ be **undefined**.

            2.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_v_) is String then let _item_ be _v._

            3.  Else if [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_v_) is Number then let _item_ be [ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_v_).

            4.  Else if [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_v_) is Object then,

                1.  If the [[Class]] internal property of _v_ is `**"String"**` or `**"Number"**` then let _item_be [ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_v_).

            5.  If _item_ is not undefined and _item_ is not currently an element of _PropertyList_ then,

                1.  Append _item_ to the end of _PropertyList_.

5.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_space_) is Object then,

    1.  If the [[Class]] internal property of _space_ is `**"Number"**` then,

        1.  Let _space_ be [ToNumber](http://www.ecma-international.org/ecma-262/5.1/#sec-9.3)(_space_).

    2.  Else if the [[Class]] internal property of _space_ is `**"String"**` then,

        1.  Let _space_ be [ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_space_).

6.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_space_) is Number

    1.  Let _space_ be min(10, [ToInteger](http://www.ecma-international.org/ecma-262/5.1/#sec-9.4)(_space_)).

    2.  Set _gap_ to a String containing _space_ space characters. This will be the empty String if _space_ is less than 1.

7.  Else if [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_space)_ is String

    1.  If the number of characters in _space_ is 10 or less, set _gap_ to _space_ otherwise set _gap_ to a String consisting of the first 10 characters of _space_.

8.  Else

    1.  Set _gap_ to the empty String.

9.  Let _wrapper_ be a new object created as if by the expression `**new Object()**`, where `**Object**` is the standard built-in constructor with that name.

10.  Call the [[DefineOwnProperty]] internal method of _wrapper_ with arguments the empty String, the[Property Descriptor](http://www.ecma-international.org/ecma-262/5.1/#sec-8.10) {[[Value]]: _value_, [[Writable]]: **true**, [[Enumerable]]: **true**, [[Configurable]]: **true**}, and **false**.

11.  Return the result of calling the abstract operation _Str_ with the empty String and _wrapper_.

The abstract operation _Str_(_key_, _holder_, _raw_ ) has access to _ReplacerFunction_ from the invocation of the`**stringify**` method. Its algorithm is as follows:

1.  Let _value_ be the result of calling the [[Get]] internal method of _holder_ with argument _key_.

2.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_value_) is Object, then

    1.  Let _toJSON_ be the result of calling the [[Get]] internal method of _value_ with argument`**"toJSON"**`.

    2.  If [IsCallable](http://www.ecma-international.org/ecma-262/5.1/#sec-9.11)(_toJSON_) is **true**

        1.  Let _value_ be the result of calling the [[Call]] internal method of _toJSON_ passing _value_ as the **this** value and with an argument list consisting of _key_.

3.  If _ReplacerFunction_ is not **undefined**, then

    1.  Let _value_ be the result of calling the [[Call]] internal method of _ReplacerFunction_ passing _holder_as the **this** value and with an argument list consisting of _key_ and _value_.

4.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_value_) is Object then,

    1.  If the [[Class]] internal property of _value_ is `**"Number"**` then,

        1.  Let _value_ be [ToNumber](http://www.ecma-international.org/ecma-262/5.1/#sec-9.3)(_value_).

    2.  Else if the [[Class]] internal property of _value_ is `**"String"**` then,

        1.  Let _value_ be [ToString](http://www.ecma-international.org/ecma-262/5.1/#sec-9.8)(_value_).

    3.  Else if the [[Class]] internal property of _value_ is `**"Boolean"**` then,

        1.  Let _value_ be the value of the [[PrimitiveValue]] internal property of _value_.

5.  If _value_ is **null** then return `**"null"**`.

6.  If _value_ is **true** then return `**"true"**`.

7.  If _value_ is **false** then return `**"false"**`.

8.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_value_) is String, then return the result of calling the abstract operation _Quote_ with argument_value_.

9.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_value_) is Number

    1.  If _value_ is finite then return #toString.

    2.  Else, return `**"null"**`.

10.  If [Type](http://www.ecma-international.org/ecma-262/5.1/#sec-8)(_value_) is Object, and [IsCallable](http://www.ecma-international.org/ecma-262/5.1/#sec-9.11)(_value_) is **false**

    1.  If the [[Class]] internal property of _value_ is `**"Array"**` then

        1.  Return the result of calling the abstract operation _JA_ with argument _value_.

    2.  Else, return the result of calling the abstract operation _JO_ with argument _value_.

11.  Return **undefined**.

The abstract operation _Quote_(_value_) wraps a String value in double quotes and escapes characters within it.

1.  Let _product_ be the double quote character.

2.  For each character _C_ in _value_

    1.  If _C_ is the double quote character or the backslash character

        1.  Let _product_ be the concatenation of _product_ and the backslash character.

        2.  Let _product_ be the concatenation of _product_ and _C_.

    2.  Else if _C_ is backspace, formfeed, newline, carriage return, or tab

        1.  Let _product_ be the concatenation of _product_ and the backslash character.

        2.  Let _abbrev_ be the character corresponding to the value of _C_ as follows:

            name            | representing character
            --------------- | ------------------------------
            backspace       | **"b"**
            formfeed        | **"f"**
            newline         | **"n"**
            carriage return | **"r"**
            tab             | **"t"**

        3.  Let _product_ be the concatenation of _product_ and _abbrev_.

    3.  Else if _C_ is a control character having a code unit value less than the space character

        1.  Let _product_ be the concatenation of _product_ and the backslash character.

        2.  Let _product_ be the concatenation of _product_ and `**"u"**`.

        3.  Let _hex_ be the result of converting the numeric code unit value of _C_ to a String of four hexadecimal digits.

        4.  Let _product_ be the concatenation of _product_ and _hex_.

    4.  Else

        1.  Let _product_ be the concatenation of _product_ and _C_.

3.  Let _product_ be the concatenation of _product_ and the double quote character.

4.  Return _product_.

The abstract operation _JO_(_value_) serializes an object. It has access to the *stack*, *indent*, *gap*, _PropertyList_,_ReplacerFunction_, and *space* of the invocation of the stringify method.

1.  If _stack_ contains _value_ then throw a **TypeError** exception because the structure is cyclical.

2.  Append _value_ to _stack_.

3.  Let _stepback_ be _indent_.

4.  Let _indent_ be the concatenation of _indent_ and _gap_.

5.  If _PropertyList_ is not **undefined**, then

    1.  Let _K_ be _PropertyList_.

6.  Else

    1.  Let _K_ be an internal [List](http://www.ecma-international.org/ecma-262/5.1/#sec-8.8) of Strings consisting of the names of all the own properties of _value_whose [[Enumerable]] attribute is **true**. The ordering of the Strings should be the same as that used by the **Object.keys** standard built-in function.

7.  Let _partial_ be an empty [List](http://www.ecma-international.org/ecma-262/5.1/#sec-8.8).

8.  For each element _P_ of _K_.

    1.  Let _strP_ be the result of calling the abstract operation _Str_ with arguments _P_ and _value_.

    2.  If _strP_ is not **undefined**

        1.  Let _member_ be the result of calling the abstract operation _Quote_ with argument _P_.

        2.  Let _member_ be the concatenation of _member_ and the colon character.

        3.  If _gap_ is not the empty String

            1.  Let _member_ be the concatenation of _member_ and the _space_ character.

        4.  Let _member_ be the concatenation of _member_ and _strP_.

        5.  Append _member_ to _partial_.

9.  If _partial_ is empty, then

    1.  Let _final_ be `**"{}"**`.

10.  Else

    1.  If _gap_ is the empty String

        1.  Let _properties_ be a String formed by concatenating all the element Strings of _partial_ with each adjacent pair of Strings separated with the comma character. A comma is not inserted either before the first String or after the last String.

        2.  Let _final_ be the result of concatenating **"{"****,** _properties_, and `**"}"**`.

    2.  Else _gap_ is not the empty String

        1.  Let _separator_ be the result of concatenating the comma character, the line feed character, and _indent_.

        2.  Let _properties_ be a String formed by concatenating all the element Strings of _partial_ with each adjacent pair of Strings separated with _separator_. The _separator_ String is not inserted either before the first String or after the last String.

        3.  Let _final_ be the result of concatenating `**"{"**`, the line feed character, _indent_, _properties_, the line feed character, _stepback_, and `**"}**`".

11.  Remove the last element of _stack_.

12.  Let _indent_ be _stepback_.

13.  Return _final_.

The abstract operation _JA_(_value_) serializes an array. It has access to the *stack*, *indent*, *gap*, and *space* of the invocation of the stringify method. The representation of arrays includes only the elements between zero and `**array.length**` – 1 inclusive. Named properties are excluded from the stringification. An array is stringified as an open left bracket, elements separated by comma, and a closing right bracket.

1.  If _stack_ contains _value_ then throw a **TypeError** exception because the structure is cyclical.

2.  Append _value_ to _stack_.

3.  Let _stepback_ be _indent_.

4.  Let _indent_ be the concatenation of _indent_ and _gap_.

5.  Let _partial_ be an empty [List](http://www.ecma-international.org/ecma-262/5.1/#sec-8.8).

6.  Let _len_ be the result of calling the [[Get]] internal method of value with argument `**"length"**`.

7.  Let _index_ be 0.

8.  Repeat while _index_ < _len_

    1.  Let _strP_ be the result of calling the abstract operation _Str_ with arguments #toString(_index_) and_value_.

    2.  If _strP_ is **undefined**

        1.  Append `**"null"**` to _partial_.

    3.  Else

        1.  Append _strP_ to _partial_.

    4.  Increment _index_ by 1.

9.  If _partial_ is empty ,then

    1.  Let _final_ be `**"[]"**`.

10.  Else

    1.  If _gap_ is the empty String

        1.  Let _properties_ be a String formed by concatenating all the element Strings of _partial_ with each adjacent pair of Strings separated with the comma character. A comma is not inserted either before the first String or after the last String.

        2.  Let _final_ be the result of concatenating **"["****,** _properties_, and `**"]"**`.

    2.  Else

        1.  Let _separator_ be the result of concatenating the comma character, the line feed character, and _indent_.

        2.  Let _properties_ be a String formed by concatenating all the element Strings of _partial_ with each adjacent pair of Strings separated with _separator_. The _separator_ String is not inserted either before the first String or after the last String.

        3.  Let _final_ be the result of concatenating `**"["**`, the line feed character, _indent_, _properties_, the line feed character, _stepback_, and `**"]**`".

11.  Remove the last element of _stack_.

12.  Let _indent_ be _stepback_.

13.  Return _final_.

NOTE 1 ESON structures are allowed to be nested to any depth, but they must be acyclic. If *value* is or contains a cyclic structure, then the stringify function must throw a **TypeError**exception. This is an example of a value that cannot be stringified:

<pre style="margin-bottom: 0.5cm">**a = [];**
**a[0] = a;**
**my_text = ESON.stringify(a); // This must throw an TypeError.**</pre>

NOTE 2 Symbolic primitive values are rendered as follows:

*   The **null** value is rendered in JSON text as the String null.

*   The **undefined** value is not rendered.

*   The **true** value is rendered in JSON text as the String true.

*   The **false** value is rendered in JSON text as the String false.

NOTE 3 String values are wrapped in double quotes. The characters `**"**` and `**\**` are escaped with `**\**`prefixes. Control characters are replaced with escape sequences `**\u**`HHHH, or with the shorter forms, `**\b**` (backspace), `**\f**` (formfeed), `**\n**` (newline), `**\r**` (carriage return), `**\t**` (tab).

NOTE 4 Finite numbers are stringified as if by calling #toString)**(**_number_**)**. **NaN** and Infinity regardless of sign are represented as the String `**null**`.

NOTE 5Values that do not have a JSON representation (such as **undefined** and functions) do not produce a String. Instead they produce the undefined value. In arrays these values are represented as the String `**null**`. In objects an unrepresentable value causes the property to be excluded from stringification.

NOTE 6An object is rendered as an opening left brace followed by zero or more properties, separated with commas, closed with a right brace. A property is a quoted String representing the key or property name, a colon, and then the stringified property value. An array is rendered as an opening left bracket followed by zero or more values, separated with commas, closed with a right bracket.

# Errors

An implementation must report most errors at the time the relevant ECMAScript language construct is evaluated. An _early error_ is an error that can be detected and reported prior to the evaluation of any construct in the _Program_ containing the error. An implementation must report early errors in a _Program_prior to the first evaluation of that _Program_. Early errors in **eval** code are reported at the time `**eval**` is called but prior to evaluation of any construct within the **eval** code. All errors that are not early errors are runtime errors.

An implementation must treat any instance of the following kinds of errors as an early error:

*   Any syntax error.

*   Attempts to define an _ObjectLiteral_ that has multiple **get** property assignments with the same name or multiple **set** property assignments with the same name.

*   Attempts to define an _ObjectLiteral_ that has both a data property assignment and a **get** or **set**property assignment with the same name.

*   Errors in regular expression literals that are not implementation-defined syntax extensions.

*   Attempts in [strict mode code](http://www.ecma-international.org/ecma-262/5.1/#sec-10.1.1) to define an _ObjectLiteral_ that has multiple data property assignments with the same name.

*   The occurrence of a _WithStatement_ in [strict mode code](http://www.ecma-international.org/ecma-262/5.1/#sec-10.1.1).

*   The occurrence of an _Identifier_ value appearing more than once within a _FormalParameterList_ of an individual strict mode _FunctionDeclaration_ or _FunctionExpression_.

*   Improper uses of `**return**`, `**break**`, and `**continue**`.

*   Attempts to call [PutValue](http://www.ecma-international.org/ecma-262/5.1/#sec-8.7.2) on any value for which an early determination can be made that the value is not a [Reference](http://www.ecma-international.org/ecma-262/5.1/#sec-8.7) (for example, executing the assignment statement 3=4).

An implementation shall not treat other kinds of errors as early errors even if the compiler can prove that a construct cannot execute without error under any circumstances. An implementation may issue an early warning in such a case, but it should not report the error until the relevant construct is actually executed.

An implementation shall report all errors as specified, except for the following:

*   An implementation may extend program syntax and regular expression pattern or flag syntax. To permit this, all operations (such as calling `**eval**`, using a regular expression literal, or using the`**Function**` or `**RegExp**` constructor) that are allowed to throw **SyntaxError** are permitted to exhibit implementation-defined behaviour instead of throwing **SyntaxError** when they encounter an implementation-defined extension to the program syntax or regular expression pattern or flag syntax.

*   An implementation may provide additional types, values, objects, properties, and functions beyond those described in this specification. This may cause constructs (such as looking up a variable in the global scope) to have implementation-defined behaviour instead of throwing an error (such as**ReferenceError**).

*   An implementation may define behaviour other than throwing **RangeError** for `**toFixed**`,`**toExponential**`, and `**toPrecision**` when the *fractionDigits* or *precision* argument is outside the specified range.
