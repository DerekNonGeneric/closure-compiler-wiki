<DIV class="">
<H3><A name="JavaScript_Types" id="JavaScript_Types">JavaScript Types</A></H3>
<SPAN class="link_button" id="link-JavaScript_Types__button" name="link-JavaScript_Types__button"><A class="internal" href="?showone=JavaScript_Types#JavaScript_Types">
          link
</A><A class="external" href="?external=y&amp;showone=JavaScript_Types#JavaScript_Types">
          link
</A></SPAN><SPAN class="showhide_button" onclick="javascript:ShowHideByName('JavaScript_Types')" name="JavaScript_Types__button" id="JavaScript_Types__button">▶</SPAN>
<DIV style="display:inline;" class="">Encouraged and enforced by the compiler.</DIV>
<DIV class=""><DIV class="stylepoint_body" name="JavaScript_Types__body" id="JavaScript_Types__body" style="display: none">
<a name="JsTypes"></a>
<p>When documenting a type in JSDoc, be as specific and accurate as
          possible. The types we support are based on the
<a href="http://www.google.com/url?sa=D&amp;q=http://wiki.ecmascript.org/doku.php?id=spec:spec">
            EcmaScript 4 spec</a>.</p>
<P class="">
<SPAN class="stylepoint_subsection">The JavaScript Type Language</SPAN>
<p>The ES4 proposal contained a language for specifying JavaScript
            types. We use this language in JsDoc to express the types.</p>

<p></p>
<table border="1" style="border-collapse:collapse" cellpadding="4">
            
<tr>
<th>Syntax Name</th>
<th>Syntax</th>
<th>Description</th>
<th style="min-width: 200px">Allows null?</th>
<th>Deprecated Syntaxes</th>
</tr>
            
            
<tr>
<td>Primitive Type</td>
<td>
                  There are 5 primitive types in JavaScript:
<code>{null}</code>,
<code>{undefined}</code>,
<code>{boolean}</code>,
<code>{number}</code>, and
<code>{string}</code>.
</td>
<td>Simply the name of a type.</td>
<td>Only<code>{null}</code>.<br>
                  The other primitive types are not nullable.</td>
<td></td>
</tr>

<tr>
<td>Instance Type</td>
<td>
<code>{Object}</code><br>
                  An instance of Object, or null.<p></p>
<code>{Function}</code><br>
                  An instance of Function, or null.<p></p>
<code>{EventTarget}</code><br>
                  An instance of a constructor that implements the EventTarget
                  interface, or null.
</td>
<td>An instance of a constructor or interface function.<p></p>

                Constructor functions are functions defined with the
<code>@constructor</code> JSDoc tag.
                Interface functions are functions defined with the
<code>@interface</code> JSDoc tag.<p></p>

<p>By default, instance types will accept null, but including the
<code>?</code> is recommended because it is more explicit.
</p>

<p>Whenever possible, avoid using<code>Object</code> in favor
                  of a more specific existing type or a newly defined record
                  type.<br>
                  Also avoid using<code>Function</code> in favor of the
                  more specific<code>function(...): ...</code>.</p>
</td>
<td>Yes.</td>
<td></td>
</tr>

<tr>
<td>Enum Type</td>
<td>
<code>{goog.events.EventType}</code><br>
                  One of the properties of the object literal initializer
                  of<code>goog.events.EventType</code>.
</td>
<td>
<p>An enum must be initialized as an object literal, or as an
                    alias of another enum, annotated with the<code>@enum</code>
                    JSDoc tag. The properties of this literal are the instances
                    of the enum. The syntax of the enum is defined
<a href="#enums">below</a>.</p>
<p>Note that this is one of the few things in our type system
                    that were not in the ES4 spec.</p>
</td>
<td>Depends on the referenced type.<code>@enum {string}</code>
                  or<code>@enum {number}</code> is not nullable by default,
                  while<code>@enum {Object}</code> is.</td>
<td></td>
</tr>

<tr>
<td>Type Application</td>
<td>
<code>{?Array&lt;string&gt;}</code><br>A nullable array of
                  strings.<p></p>
<code>{!Object&lt;string, number&gt;}</code><br>A non-null
                  object in which the keys are strings and the values are
                  numbers.<p></p>
<code>{!goog.structs.Set&lt;!YourType&gt;}</code><br>A non-null
                  Set of non-null instances of YourType.
</td>
<td>Parameterizes a type, by applying a set of type arguments
                  to that type. The idea is analogous to generics in Java. The
                  dot before the<code>&lt;</code> (e.g.
<code>{!Array.&lt;string&gt;}</code>) is optional.
</td>
<td>
                  Depends on the type being parameterized (<code>?Object</code>
                  vs.<code>!Object</code>) and on the type arguments used to
                  parameterize it (<code>?YourType</code> vs.
<code>!YourType</code>,<code>number</code> vs.
<code>?number</code>).
</td>
<td></td>
</tr>

<tr>
<td>Type Union</td>
<td>
<code>{(number|boolean)}</code><br>A number or a boolean.
</td>
<td>Indicates that a value might have type A OR type B.<p></p>

                  The parentheses may be omitted at the top-level
                  expression, but the parentheses should be included in
                  sub-expressions to avoid ambiguity.<br>
<code>{number|boolean}</code><br>
<code>{function(): (number|boolean)}</code>
</td>
<td>Only when any of the types in the union is already
                  nullable.</td>
<td>
<code>{(number,boolean)}</code>,<br>
<code>{(number||boolean)}</code>
</td>
</tr>

<tr>
<td>Nullable type</td>
<td>
<code>{?number}</code><br> A number or null.
</td>
<td>
<p>Shorthand for the union of the null type with any
                    other type. This is just syntactic sugar.</p>
<p>Note that the following are already nullable, and thus
                    prepending<code>?</code> is redundant, but is recommended
                    so that the intent is clear and explicit:
<DIV class=""><PRE>?Object, ?Array, ?Function</PRE></DIV></p>
</td>
<td>Yes.</td>
<td>
<code>{number?}</code>
</td>
</tr>

<tr>
<td>Non-nullable type</td>
<td>
<code>{!Object}</code><br> An Object, but never the
<code>null</code> value.
</td>
<td>
<p>Filters null out of nullable types. Most often used
                    with instance types, which are nullable by default.</p>
<p>Note that the following are already non-nullable, and thus
                    prepending<code>!</code> is redundant:
<DIV class=""><PRE class="badcode">!number, !string, !boolean, !{foo: string}, !function()</PRE></DIV></p>
</td>
<td>No.</td>
<td>
<code>{Object!}</code>
</td>
</tr>

<tr>
<td>Record Type</td>
<td>
<code>{{myNum: number, myObject}}</code>
<br>An anonymous type with the given type members.
</td>
<td>
<p>Indicates that the value has the specified members with the
                    specified types. In this case,<code>myNum</code> with a
                    type<code>number</code> and<code>myObject</code> with any
                    type.</p>
<p>Notice that the braces are part of the type syntax. For
                    example, to denote an<code>Array</code> of objects that
                    have a<code>length</code> property, you might write
<code>Array&lt;{length}&gt;</code>.</p>
</td>
<td>No.</td>
<td></td>
</tr>

<tr>
<td>Function Type</td>
<td>
<code>{function(string, boolean)}</code><br>
                  A function that takes two arguments (a string and a boolean),
                  and has an unknown return value.<br>
</td>
<td>
<p>Specifies a function.</p>
<p>Also note the difference between<code>{function()}</code>
                    and<code>{Function}</code>. The latter is an instance type
                    and is nullable by default.<code>function(...)</code>
                    should be used instead of<code>Function</code> whenever
                    possible because it provides more type information about its
                    parameters and return value.</p>
</td>
<td>No.</td>
<td></td>
</tr>

<tr>
<td>Function Return Type</td>
<td>
<code>{function(): number}</code><br>
                  A function that takes no arguments and returns a number.<br>
</td>
<td>Specifies a function return type.</td>
<td>
<code>function(...)</code> is not nullable. Nullability
                  of the return type is determined by the type itself.</td>
<td></td>
</tr>

<tr>
<td>Function<code>this</code> Type</td>
<td>
<code>{function(this:goog.ui.Menu, string)}</code><br>
                  A function that takes one argument (a string), and executes
                  in the context of a goog.ui.Menu.
</td>
<td>Specifies the context type of a function type.</td>
<td>
<code>function(...)</code> is not nullable.
<code>this</code> can be null.</td>
<td></td>
</tr>

<tr>
<td>Function<code>new</code> Type</td>
<td>
<code>{function(new:goog.ui.Menu, string)}</code><br>
                  A constructor that takes one argument (a string), and
                  creates a new instance of goog.ui.Menu when called
                  with the 'new' keyword.
</td>
<td>Specifies the constructed type of a constructor.</td>
<td>
<code>function(...)</code> is not nullable.</td>
<td></td>
</tr>

<tr>
<td>Variable arguments</td>
<td>
<code>{function(string, ...number): number}</code><br>
                  A function that takes one argument (a string), and then a
                  variable number of arguments that must be numbers.
</td>
<td>Specifies variable arguments to a function.</td>
<td>
<code>function(...)</code> is not nullable. Nullability
                  of the arguments is determined by the type annotation
                  after the<code>...</code>
</td>
<td></td>
</tr>

<tr>
<td>
<a name="var-args-annotation"></a>
                  Variable arguments (in<code>@param</code> annotations)
</td>
<td>
<code>@param {...number} var_args</code><br>
                  A variable number of arguments to an annotated function.
</td>
<td>
                  Specifies that the annotated function accepts a variable
                  number of arguments.
</td>
<td>Determined by the type annotation after the ellipsis.</td>
<td></td>
</tr>

<tr>
<td>Function<a href="#optional">optional arguments</a>
</td>
<td>
<code>{function(?string=, number=)}</code><br>
                  A function that takes one optional, nullable string and one
                  optional number as arguments. The<code>=</code> syntax is
                  only for<code>function</code> type declarations.
</td>
<td>Specifies optional arguments to a function.</td>
<td>
<code>function(...)</code> is not nullable. Nullability of
                  arguments is determined by the unadorned type annotation.
                  See<a href="#optional">nullable vs. optional</a> for more
                  information.</td>
<td></td>
</tr>

<tr>
<td>
<a name="optional-arg-annotation"></a>
                  Function<a href="#optional">optional arguments</a>
                  (in<code>@param</code> annotations)
</td>
<td>
<code>@param {number=} opt_argument</code><br>
                  An optional parameter of type<code>number</code>.
</td>
<td>Specifies that the annotated function accepts an optional
                  argument.</td>
<td>Same as above.</td>
<td></td>
</tr>

<tr>
<td>The ALL type</td>
<td><code>{*}</code></td>
<td>Indicates that the variable can take on any type.</td>
<td>Yes.</td>
<td></td>
</tr>

<tr>
<td>The UNKNOWN type</td>
<td><code>{?}</code></td>
<td>Indicates that the variable can take on any type,
                    and the compiler should not type-check any uses of it.</td>
<td>Yes.</td>
<td></td>
</tr>
            
</table>
</P>
<P class="">
<SPAN class="stylepoint_subsection">Types in JavaScript</SPAN>
<p></p>
<table border="1" style="border-collapse:collapse" cellpadding="4">
            
<tr>
<th>Type Example</th>
<th>Value Examples</th>
<th>Description</th>
</tr>
            
            

<tr>
<td>number</td>
<td>
<DIV class=""><PRE>1
1.0
-5
1e5
Math.PI</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>Number</td>
<td>
<DIV class=""><PRE>new Number(true)</PRE></DIV>
</td>
<td>
<a href="#Wrapper_objects_for_primitive_types">
                    Number object
</a>
</td>
</tr>

<tr>
<td>string</td>
<td>
<DIV class=""><PRE>'Hello'
"World"
String(42)</PRE></DIV>
</td>
<td>
                  String value
</td>
</tr>

<tr>
<td>String</td>
<td>
<DIV class=""><PRE>new String('Hello')
new String(42)</PRE></DIV>
</td>
<td>
<a href="#Wrapper_objects_for_primitive_types">
                    String object
</a>
</td>
</tr>

<tr>
<td>boolean</td>
<td>
<DIV class=""><PRE>true
false
Boolean(0)</PRE></DIV>
</td>
<td>
                  Boolean value
</td>
</tr>

<tr>
<td>Boolean</td>
<td>
<DIV class=""><PRE>new Boolean(true)</PRE></DIV>
</td>
<td>
<a href="#Wrapper_objects_for_primitive_types">
                    Boolean object
</a>
</td>
</tr>

<tr>
<td>RegExp</td>
<td>
<DIV class=""><PRE>new RegExp('hello')
/world/g</PRE></DIV>
</td>
<td>
</td>
</tr>

<tr>
<td>Date</td>
<td>
<DIV class=""><PRE>new Date
new Date()</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>
<span class="internal"><i>preferred:</i><br></span>
                  null
<span class="internal">
<br><br>
<i>deprecated:</i><br>
                    Null
</span>
</td>
<td>
<DIV class=""><PRE>null</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>
<span class="internal"><i>preferred:</i><br></span>
                  undefined
<span class="internal">
<br><br>
<i>deprecated:</i><br>
                    Undefined
</span>
</td>
<td>
<DIV class=""><PRE>undefined</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>void</td>
<td>
<DIV class=""><PRE>function f() {
  return;
}</PRE></DIV>
</td>
<td>No return value</td>
</tr>

<tr>
<td>Array</td>
<td>
<DIV class=""><PRE>['foo', 0.3, null]
[]</PRE></DIV>
</td>
<td>Untyped Array</td>
</tr>

<tr>
<td>Array&lt;number&gt;</td>
<td>
<DIV class=""><PRE>[11, 22, 33]</PRE></DIV>
</td>
<td>
                  An Array of numbers
</td>
</tr>

<tr>
<td>Array&lt;Array&lt;string&gt;&gt;</td>
<td>
<DIV class=""><PRE>[['one', 'two', 'three'], ['foo', 'bar']]</PRE></DIV>
</td>
<td>Array of Arrays of strings</td>
</tr>

<tr>
<td>Object</td>
<td>
<DIV class=""><PRE>{}
{foo: 'abc', bar: 123, baz: null}</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>Object&lt;string&gt;</td>
<td>
<DIV class=""><PRE>{'foo': 'bar'}</PRE></DIV>
</td>
<td>
                  An Object in which the values are strings.
</td>
</tr>

<tr>
<td>Object&lt;number, string&gt;</td>
<td>
<DIV class=""><PRE>var obj = {};
obj[1] = 'bar';</PRE></DIV>
</td>
<td>
                  An Object in which the keys are numbers and the values are
                  strings.<p></p>Note that in JavaScript, the keys are always
                  implicitly converted to strings, so
<code>obj['1'] == obj[1]</code>.
                  So the key will always be a string in for...in loops. But the
                  compiler will verify the type of the key when indexing into
                  the object.
</td>
</tr>

<tr>
<td>Function</td>
<td>
<DIV class=""><PRE>function(x, y) {
  return x * y;
}</PRE></DIV>
</td>
<td>
<a href="#Wrapper_objects_for_primitive_types">
                    Function object
</a>
</td>
</tr>

<tr>
<td>function(number, number): number</td>
<td>
<DIV class=""><PRE>function(x, y) {
  return x * y;
}</PRE></DIV>
</td>
<td>function value</td>
</tr>

<tr>
<td><a name="constructor-tag">SomeClass</a></td>
<td>
<DIV class=""><PRE>/** @constructor */
function SomeClass() {}

new SomeClass();</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>SomeInterface</td>
<td>
<DIV class=""><PRE>/** @interface */
function SomeInterface() {}

SomeInterface.prototype.draw = function() {};</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>SomeRecord</td>
<td>
<DIV class=""><PRE>/** @record */
function SomeInterface() {}

SomeInterface.prototype.draw = function() {};</PRE></DIV>
</td>
<td>
                  Like an interface, but is checked using structural equality
                  only. Any value with matching properties is convertible to the
                  record type.
</td>
</tr>

<tr>
<td>project.MyClass</td>
<td>
<DIV class=""><PRE>/** @constructor */
project.MyClass = function () {}

new project.MyClass()</PRE></DIV>
</td>
<td></td>
</tr>

<tr>
<td>project.MyEnum</td>
<td>
<DIV class=""><PRE>/** @enum {string} */
project.MyEnum = {
  /** The color blue. */
  BLUE: '#0000dd',
  /** The color red. */
  RED: '#dd0000'
};</PRE></DIV>
</td>
<td>
<a name="enums">Enumeration</a><p></p>
                  JSDoc comments on enum values are optional.
</td>
</tr>

<tr>
<td>Element</td>
<td>
<DIV class=""><PRE>document.createElement('div')</PRE></DIV>
</td>
<td>Elements in the DOM.</td>
</tr>

<tr>
<td>Node</td>
<td>
<DIV class=""><PRE>document.body.firstChild</PRE></DIV>
</td>
<td>Nodes in the DOM.</td>
</tr>

<tr>
<td>HTMLInputElement</td>
<td>
<DIV class=""><PRE>htmlDocument.getElementsByTagName('input')[0]</PRE></DIV>
</td>
<td>A specific type of DOM element.</td>
</tr>
            
</table>
</P>

<P class="">
<SPAN class="stylepoint_subsection">Type Casts</SPAN>
<p>In cases where type-checking doesn't accurately infer the type of
            an expression, it is possible to add a type cast comment by adding a
            type annotation comment and enclosing the expression in
            parentheses. The parentheses are required.</p>

<DIV class=""><PRE>/** @type {number} */ (x)</PRE></DIV>
</P>

<P class="">
<SPAN class="stylepoint_subsection">Nullable vs. Optional Parameters and Properties</SPAN>
<a name="optional"></a>
<p>Because JavaScript is a loosely-typed language, it is very
            important to understand the subtle differences between optional,
            nullable, and undefined function parameters and class
            properties.</p>

<p>Instances of classes and interfaces are nullable by default.
          For example, the following declaration</p>

<DIV class=""><PRE>/**
 * Some class, initialized with a value.
 * @param {Object} value Some value.
 * @constructor
 */
function MyClass(value) {
  /**
   * Some value.
   * @private {Object}
   */
  this.myValue_ = value;
}</PRE></DIV>

<p>tells the compiler that the<code>myValue_</code> property holds
            either an Object or null.  If<code>myValue_</code> must never be
            null, it should be declared like this:</p>

<DIV class=""><PRE>/**
 * Some class, initialized with a non-null value.
 * @param {!Object} value Some value.
 * @constructor
 */
function MyClass(value) {
  /**
   * Some value.
   * @private {!Object}
   */
  this.myValue_ = value;
}</PRE></DIV>

<p>This way, if the compiler can determine that somewhere in the code
<code>MyClass</code> is initialized with a null value, it will issue
            a warning.</p>

<div class="internal">
<p>You may see type declarations like these in legacy code:</p>
<DIV class=""><PRE>@type {Object?}
@type {Object|null}</PRE></DIV>
</div>

<p>Optional parameters to functions may be undefined at runtime, so if
          they are assigned to class properties, those properties must be
          declared accordingly:</p>

<DIV class=""><PRE>/**
 * Some class, initialized with an optional value.
 * @param {!Object=} opt_value Some value (optional).
 * @constructor
 */
function MyClass(opt_value) {
  /**
   * Some value.
   * @private {!Object|undefined}
   */
  this.myValue_ = opt_value;
}</PRE></DIV>

<p>This tells the compiler that<code>myValue_</code> may hold an
            Object, or remain undefined.</p>

<p>Note that the optional parameter<code>opt_value</code> is declared
            to be of type<code>{!Object=}</code>, not
<code>{!Object|undefined}</code>.  This is because optional
            parameters may, by definition, be undefined.  While there is no harm
            in explicitly declaring an optional parameter as possibly undefined,
            it is both unnecessary and makes the code harder to read.</p>

<p>Finally, note that being nullable and being optional are orthogonal
            properties.  The following four declarations are all different:</p>

<DIV class=""><PRE>/**
 * Takes four arguments, two of which are nullable, and two of which are
 * optional.
 * @param {!Object} nonNull Mandatory (must not be undefined), must not be null.
 * @param {?Object} mayBeNull Mandatory (must not be undefined), may be null.
 *     ({Object} would mean the same thing, but is not as explicit.)
 * @param {!Object=} opt_nonNull Optional (may be undefined), but if present,
 *     must not be null!
 * @param {?Object=} opt_mayBeNull Optional (may be undefined), may be null.
 *     ({Object=} would mean the same thing, but is not as explicit.)
 */
function strangeButTrue(nonNull, mayBeNull, opt_nonNull, opt_mayBeNull) {
  // ...
};</PRE></DIV>
</P>

<P class="">
<SPAN class="stylepoint_subsection">Typedefs</SPAN>
<a name="Typedefs"></a>
<p>Sometimes types can get complicated. A function that accepts
            content for an Element might look like:</p>

<DIV class=""><PRE>/**
 * @param {string} tagName
 * @param {(string|Element|Text|Array&lt;Element&gt;|Array&lt;Text&gt;)} contents
 * @return {!Element}
 */
goog.createElement = function(tagName, contents) {
  ...
};</PRE></DIV>

<p>You can define commonly used type expressions with a
<code>@typedef</code> tag. For example,</p>

<DIV class=""><PRE>/** @typedef {(string|Element|Text|Array&lt;Element&gt;|Array&lt;Text&gt;)} */
goog.ElementContent;

/**
 * @param {string} tagName
 * @param {goog.ElementContent} contents
 * @return {!Element}
 */
goog.createElement = function(tagName, contents) {
...
};</PRE></DIV>
</P>

<P class="">
<SPAN class="stylepoint_subsection">Template types</SPAN>
<a name="Template_types"></a>
<p>The compiler has limited support for template types. It can only
            infer the type of<code>this</code> inside an anonymous function
            literal from the type of the<code>this</code> argument and whether the
<code>this</code> argument is missing.</p>

<DIV class=""><PRE>/**
 * @param {function(this:T, ...)} fn
 * @param {T} thisObj
 * @param {...*} var_args
 * @template T
 */
goog.bind = function(fn, thisObj, var_args) {
...
};
// Possibly generates a missing property warning.
goog.bind(function() { this.someProperty; }, new SomeClass());
// Generates an undefined this warning.
goog.bind(function() { this.someProperty; });</PRE></DIV>
</P>
</DIV></DIV>
</DIV>