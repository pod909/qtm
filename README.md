# Quick Type Markup

Quick Type Markup is a way of specifying type and format restrictions in a single brief type definition.

## Examples

3<[12.4<!<90.9]<40

${option 1|option 2|option 3}

## Basic types

To define the basic type of include a type char as follows:

```json
{
    "string":"$",
    "hidden string":"*",
    "email address string":"@",
    "url string":":",
    "date":"D",
    "time":"T",
    "date-time":"&",
    "month":"M",
    "week":"W",
    "color":"C",
    "file":"F",
    "image":"I",
    "boolian":"%",
    "real":"!",
    "integer":"~",
    "unsigned":"#",
    "container":"+",
    "ordered list":"="
}
```

## Fixed width

To define a fixed number of characters for a string include a number in brackets after the string type char ($).

```json
{
    "fixed-10-char-string":"$(10)"
}
```

To define a fixed number of integer digits for a number include a number after the type char (%, ~ or #).

```json
{
    "fixed-10-digit-integer":"~(10)",
    "fixed-8-digit-unsigned-integer":"#(8)"
}
```

To define a fixed number decimal digits for a real number include a period (.) and number after the number specifying the integer (%).

```json
{
    "real number with a 3 digit intiger and 2 digit decimal":"%(3.2)",
}
```

### Padding

To define the padding for a fixed or minimum length value add the following after the fixed width number:
- For left justified (right padded) <, or for a right justified (left padded) >value
- Followed the char to pad with.

```json
{
    "10-digit-int-right-justified-and-padded-with-0":"#(10>0)",
    "minimum-15-char-string-left-justified-with-spaces":"15<$(< )",
}
```

## Variable width strings

To define the minimum number or chars in a string add a number and then a < to the left of the type char.
To define the maximum number or chars in a string add a < and then a number to the right of the type char.

```json
{
    "max-length-20":"$<20",
    "min-length-15":"15<$",
    "min-length-32-max-length-40":"32<$<40"
}
```

## Minimum and maximum number values

To define the minimum and/or maximum value for a number add a < and number to the left and/or right of the type char.

```json
{
    "real-less-than-or-equal-to-24.7":"%<24.7",
    "int-greater-than-or-equal-to-minus-67":"67<~",
    "uint-between-12-and-34":"12<#<34"
}
```

## String pattern

To define the patter match for the contents of a string add a regex statment surrounded by / after the string type char.

```json
{
    "alpha-numeric-string":"$/[a-zA-Z0-9]*/"
}
```

## Enumerations

To define a list of possible values add curly brackets ({}) at the end of the type definition.
Include a pipe delineated list inside them.
When the list follows a number type char it signifies an emueration.

Idnetify the default item by putting a * at the end of it's name.

```json
{
    "enumerated-list":"#{Monday*|Tuesday|Thursday}",
    "list-of-string-options-for-a-fixed-width-string":"$(8< ){Monday|Tuesday|Thursday}"
}
```

## Repetition

To define the number of times something can repeat wrap the type definition in square bracket ([]).

To specify a fixed number of repititions follow the square brackets with a number in brackets.

```json
{
    "10-strings":"[$](10)"
}
```

To specify the minimum number of repetitions place a number followed by a < to the left of the sqaure brackets.
To specify the maximum number of repetitions place a < followed by a number to the right of the square brackets.

## Mandatory

To define something as place a * at the end of its name.

## McKeenman gramma

```
typedef
    typechar
    primative fixed
    min primative
    min primative '(' padding ')'
    primative max
    min primative max
    min primative max '(' padding ')'
    concaternation

repetition
    '[' typedef ']' repfixed
    repmin '[' typedef ']'
    '[' typedef ']' repmax
    repmin '[' typedef ']' repmax

typechar
    primative
    object

primative
    '$'
    '$' pattern
    '$' enumeration
    '*'
    '*' pattern
    '*' enumeration
    '@'
    '%'
    '~'
    '#'
    '#' enumeration

object
    '@'
    '+'

fixed
    '(' number ')'
    '(' number padding ')'

min
    number '<'

max
    '<' number

padding
    '<' anychar
    '>' anychar

pattern
    '/' pstring '/'

repfixed
    '(' integer ')'

repmin
    integer '<'

repmax
    '<' integer

enumeration
    '{' listitems '}'

listitems
    listitem
    listitem '|' listitems

listitem
    string
    string '*'

number
    integer
    integer '.' decimal

integer
    onenine
    onenine decimal
    integer '0'

decimal
    onenine
    decimal onenine
    '0' decimal

pstring
    panychar
    panychar string

panychar
    '\' pescchar
    '\' regexmeta
    char - '\' - pescchar

pescchar
    '/'
    '\'

lstring
    lanychar
    lanychar string

lanychar
    '\' lescchar
    char - '\' - lescchar

lescchar
    '|'
    '}'
    '\'

regexmeta
    '^'
    '$'
    '.'
    '|'
    '?'
    '*'
    '+'
    '('
    ')'
    '['
    ']'
    '{'
    '}'
    'd'
    'Q'
    'E'
    't'
    'r'
    'n'
    'a'
    'e'
    'f'
    'v'
    'c'
    'x'
    'u'
    'R'

char
    '0020' . '10ffff'
```


# Quick Schema Markup

Quick Schema Markup is a way of creating data schema using Quick Type Markup to annotate an object template in JSON or XML/HTML.

## Marking up JSON

To mark up a JSON object template include the Quick Type Markup data types in the values of each property.

### Example

```json
{
    "customer*":
    {
        "name*":"$<100",
        "address*" :
        {
            "lines":[ "$<50", "[$<100]<2" ],
            "postcode*":"5<$<10"
        },
        "phoneno":"99<#<10000",
        "type":"${Owner*|Renter|Livein}"
    }
}
```

## XML/HTML

To mark up XML/HTML include a Quick Type Markup data type in the value of attributes and 
a qtm attibute with a Quick Type Markup data type in the value for elements.

### HTML example

```html
<form>
    <fieldset name="customer*" qtm="@">
        <input name="name*" qtm="$<100".>
        <fieldset name="address*" qtm="@">
            <fieldset name="lines" qtm="+">
                <input name="lines1*" qtm="$<50"/>
                <input name="lines2" qtm="$<100"/>
                <input name="lines3" qtm="$<100"/>
            </fieldset>
            <input name="postcode*" qtm="5<$<10"/>
        </fieldset>
        <input name="phoneno" qtm="99<#<10000"/>
        <input name="type" qtm="${Owner*|Renter|Livein}"/>
    </fieldset>
</form>
```
