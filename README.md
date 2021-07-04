
@Note, this documentation uses [simpex](http://github.com/grithin/simpex)


<!-- TOC -->

- [About](#about)
- [Naming Methodology](#naming-methodology)
	- [CamelCase Vs Underscore](#camelcase-vs-underscore)
	- [Preference Of Name Category Separation](#preference-of-name-category-separation)
	- [Naming Ambiguity](#naming-ambiguity)
	- [Class](#class)
		- [Is or About](#is-or-about)
	- [Functions](#functions)
		- [Standard Format](#standard-format)
		- [Affixes](#affixes)
		- [Convenience](#convenience)
		- [Parameter order](#parameter-order)
	- [Variables](#variables)
		- [Class](#class-1)
		- [Increments](#increments)
		- [Common](#common)
	- [Database](#database)
		- [Table Naming](#table-naming)
		- [Column Naming](#column-naming)
		- [Scope](#scope)
			- [Resolving context](#resolving-context)
		- [Table Referencing](#table-referencing)
- [Comments](#comments)
	- [Modern DocBlock](#modern-docblock)
	- [Extending DocBlock](#extending-docblock)
		- [Supplemental](#supplemental)
	- [Grouping](#grouping)
	- [Misc](#misc)
	- [Inner Sections](#inner-sections)
	- [Attention](#attention)
	- [Descripters (non-values)](#descripters-non-values)
		- [escaping](#escaping)
		- [Special types](#special-types)
	- [Parameters](#parameters)
		- [Positional and Non-positional](#positional-and-non-positional)
		- [Long Descriptions](#long-descriptions)
		- [Variable Value](#variable-value)
		- [Variable Params](#variable-params)
			- [Expandable Parameters](#expandable-parameters)
		- [Structure Comments](#structure-comments)
		- [Escaping](#escaping)
	- [Return](#return)
- [CRUD+L](#crudl)
- [Organisation of files for projects](#organisation-of-files-for-projects)
	- [Within pub/pri web projects](#within-pubpri-web-projects)
- [Spacing](#spacing)
- [Markdown Doc](#markdown-doc)
- [PHP](#php)
	- [HTML Integration](#html-integration)
- [JS](#js)
	- [This](#this)

<!-- /TOC -->


# About
Purpose driven standards for coding


# Naming Methodology

-	Use camel case for familiar or related or heavily used terms (classes, modules)
	-	except when the name becomes too dense to read quickly
-	Use `_` for everything else, including variables
-	use general to specific in most cases (Ex `country_state_city`)
-	use generic reference "item" when possible (ex: "item" instead of "blog") with potentially reuseable code
	-	allow "item" to take place of "entity" (it's easier to type and nearly as inclusive)


## CamelCase Vs Underscore
Underscores allow the nuances of parts of the name to be more readily identified, allowing for faster examination particularly when there are similarly named tokens.  For example,
-	`userAdd`
-	`usersAdd`
-	`user_add`
-	`users_add`

With `userAdd` and `usersAdd`, the inspector must first identify the separation point, and then see whether user is plural or not.  With `user_add` and `users_add`, the difference is immediately apparent.  While the difference in mental parsing is fractions of a second, this makes a difference in both preventing mistakes and in time when there are numerous camel cased, similar tokens.

The benefit of underscore also is present in long variable names:
-	`userWhoGotCaughtBeforeStart`
-	`userWhoDidNotGotCaughtBeforeStart`
-	`user_who_got_caught_before_start`
-	`user_who_did_not_caught_before_start`

When the variable name is long enough, the separations provided by camel casing become insufficient for quick reading.

As for the difficulty of typing, underscore is a uniform procedure, whereas, typing a capital letter has variable hand placement, resulting in approximately the same time to type.

As for function length, the difference is negligible for most purposes.

As for compatibility, underscore tends to be more compatible in different environments (which accept underscors but not case sensitivity)


### A Note On My History

Back in 2007, I wrote a coding standard that used camel casing.  My argument was that camel casing was shown in a study to be read faster.  In using that standard for over 10 years, I noticed issues, particularly when adopting the standard for naming long functions based on subject and predicate.  So, I experimented with underscores (not snake case).  In experimenting, I released the above notes, and revised my coding standard.

I think the reason camel casing can be shown to be read faster is that the mind identifies the function as a single token when using short names.  Something like "userAdd" is a single token in the mind, whereas "user_add" is two tokens.  However, I find the clarity more important than the trivial amount of time saved in reading, if indeed there is any time saved.




## Preference Of Name Category Separation

The characters of separation are often `.` or `-` or `:`.
```
Las Vegas, Nevada, United States, Earth
earth.us.nv.las_vegas
earth-us-nv-las_vegas
```

There are problems with these characters:

-	not all environments support them in names
-	a name part itself may contain the character, thus confusing part of a name part with the name part separation

`__` is almost never used within a name part, and it is almost always available as part of the naming characters.  The downside is that it is ugly and it is two characters.   However, it provides consistency and reliability.
```
earth__us__nv__las_vegas
```
-	In cases of multiple separation contexts, increase the number of  `_` as necessary



## Naming Ambiguity

__Possession Vs Pluralization__
There is a conflict between pluralization and possesion: `users_items`.  Is this
-	a user's items
-	mulltiple users' items?

Plural tends to be the standard interpretation.  To allow for possessive in a manner not ambiguous, a `z` is used where an `'s` would normally be used.  `z` is immediately recognized as awkward, forcing interpretation.
Using this convention, `s` is never used to indicate possession and can always be expected to mean pluralization

__Set Pairing__
The general format of naming is `{context_subject}_{primary_subject}` - a narrowing style. The pairing can still result in ambiguous cases
-	`itemsz_id`
	-	is this a single id of multiple items
	-	is this a single id for each of multiple items
-	`usersz_types`
	-	is this a type for each of multiple users
	-	is this multiple types for each of multiple users


To illustrate the issue of pairing sets, lets consider the possibilities:
-	1 <= y <= x <= z
-	1:1
	-	a single user with a single id
-	1:x
	-	a single user with multiple types
-	x:x
	-	multiple users each with a single id
-	x:z
	-	multiple users each with multiple types
	-	in reality, `z` here is just used to indicate an array, and `z` could be smaller than `x` if the arrays were mostly empty
-	x:1
	-	multiple roomates with a single house
-	x:y
	-	multiple users with some shared houses

`x:1` and `x:y` are normally rare.  If we allow the `primary_subject` to define the plurality, then something like `usersz_types` could be `x:x`, `x:y`, or `x:z` and `usersz_type` would be `x:1`.  However, since `x:1` and `x:y` are rare, we can use the assumption that `x:1` is not meant, and we can use the `context_subject` to define the plurality:
-	`usersz_type` multiple users each with a single type (x:x)
-	`usersz_types` multiple users each with multiple types (x:z)

__Possession Vs Category__
There is an additional case of ambiguity.  With something like, `user_types`, this could be:
1.	a single user's multiple types
2.	the general types that apply to any user
To prevent this case, always use possession if meaning #1 is intended when this ambiguity is possible: `userz_types`.

__general rules__
-	`userz_type` : an user's type
-	`userz_types` : an user's multiple types
-	`usersz_type` : type of each user
-	`usersz_types` : types of each user (each has multiple types)
-	`user_types` : the possible types that can apply to the abstract user

__context sensitive rules__
Some rules can be applied if there is no likely ambiguity given the subjects
-	`item_id`	the id of a single item
-	`item_ids` the ids of multiple items



## Class
Use camel casing.  

Classes fit the best use of camel casing - common, unmistakeable, brief tokens.  

If abbreviation is necessary, separate from remainder with `_`:
-	`US_Utah` instead of `USUtah`


### Is or About
If a class is about something (that contains many), not about any particular one, pluralize:  `Users`
If a class instance represents something, such as a user, name it as the thing would be called: `User`.  

Since an instance representing multiple of something, such as an instance representing multiples users, is rare, pluralized classes like `Users` is assumed to be about users, and not some instanceable selection of users.

Compounded, non possessive class names take on the broadest context:
With something like `user types`, there is a set of `types` that can be attributed to users (`user_type`) and there is a set of `types` which are attributed to specific users (`users_type`).  A `UserTypes` class would have the context of both, whereas `UsersTypes` would be a class specifically for handling types actually associated to users, perhaps constructed with a particular user.

On pluralized "about something" classes, on assumed context functions, assume singular subjects unless:
-	`Users.get` will get a single user
-	`Users.gets` will get multiple users
-	`Users.users_get`  will get multiple users



## Functions
See @{Naming Ambiguity}

### Standard Format

__General Format__
`subject` `act` `modifiers`
-	`bob_remove_from_users_table`

The point of stacking the subjects first is to name-categorize functions:
`type_add, type_delete, status_add, status_delete` vs `add_type, delete_type, add_status, delete_status`

__Singular Is Plural__
In cases where the word for the singular version is the same as the plural version, add a `s` to the plural version regardless:
-	`deers`
-	`fishs` (not an `es` )

__Pluralized Variability__
Regardless of how the language might pluralize a word (`ey` to `ies`), just add `s` to the singular form to indicate pluralization

__Return Plurality__
When a function returns multiple of something, there is an option in naming
-	`users_get`
-	`user_gets`
To keep inline with subject defining plurality of naming, user `users_get`

__Part Confusion__
In the rare case that a subject and act can be confused, use `__` to separate

__Sequence__
If a sequence of actions in a function can not be given a shortenned name, just list all the parts or the important parts separated with `__`
`user_delete__scoreboard_update__admin_notification`

__Subject Preference__
Stack the subject rather than the predicate: prefer `userz_type_get` over `user_get_type`

__Set Notion__
For some many to one relation, special syntax is required.  Say multiple users live in the same house, and a function is expected to return that house based on the users : `usersz_house`.  Ordinarily, this would be interpretted as getting a house per user.  To de-ambiguate, use a `_n` to indicate a many-to-fewer relation, and a `_1` to indicate a many to one relation:
-	`usersz_1_house`
-	`userz_n_cars`

__Referring To Class__
A function might involve a class instance, and it might be desirable to use the class name in the function name.  The class naming, however, makes exact representation in the function name look strange.  This is fine
-	class `UserType`, function `UserType_get`
The class reference can also be pluralized
-	class `UserType`, function `UserTypes_get`




### Affixes
`_by_x` describing the parameters
`_from_x` describing the parameter, wherein the parameter is a source for data rather than just a reference
`_to_x` describing the return
`_with_x` describing additional attributes of the return


### Convenience

-	The function can be just the noun, with the action presumed: `users` instead of `users_get`.
-	The function can exclude the noun when it is certain: `has_role` instead of `user_has_role`
-	`user_has_permission_id` needn't be `user_has_permission_by_permission_id`.  It is reasonable to condense the `by` portion.


### Parameter order



If possible, follow the name of the function.  Ex:
-	`is_user_in_group_id` : `$user_id, $group_id`

Otherwise, The subject first, then the predicateEx:
-	`has_role($user, $role)`
-	with `in_array`, the concern is the needle, and whether it is in some array.  So, it is `in_array($needle, $array)`






## Variables
See @{Naming Ambiguity}


### Class
When a variable represents a class instance (of a non-native class), capitalize first letter
`$User = new User` vs `$user = 'bob'`


### Increments
For incremental variables, like `i`, that overlap in nested loops, start with `i`, then add numbers starting from 1: `i, i1, i2, i3`


### Common
-	`dir`: string with trailing `/`
-	`path`: string with no trailing `/`



## Database
-	side note: it's not practical to encompass all possible table relation complexity via naming standards.

### Table Naming
Two styles of naming are acceptible:
__style 1, possessive tables__
It is assumed a table contains many records, so pluralization is not necesary.  Ex: `user` and not `users`.  Instead, `s` form is used to indicate possession, allowing for a {posession form} and a {type form}
-	_possession form_: `users_type` to list all the types a particular user has
-	_type form_: `user_type` to list the potential types any user could have

__style 2, pluralized tables__
Table names should be plural (ex: `users`).  When a table is a sub-table, remove the first table's pluralization (ex `user_comments`).  To indicate {_type form_}, remove pluralization (ex `user_type`).


Comparison:

style 1
-	post
-	post_type
-	posts_type
-	posts_comment
-	posts_comments_rating
-	post_comment_type
-	posts_comments_type

style 2
-	posts
-	post_type
-	post_types
-	post_comments
-	post_comment_ratings
-	post_comment_type
-	post_comment_types


Style 2 is more popular, probably because it matches expectability in language.  The downside to this form is it is less programmatically predictable.  With style 1, it will always be `TABLE's_'SUB_TABLE` for possessed tables.  With style 2, the base will be the result of `'depluralize('TABLE')'`.  Since the depluralize logic will probably be available in all areas where needed, this is not much of an issue.



### Column Naming
For columns, although mysql now allows any characters, postgres still has rather strict rules (http://stackoverflow.com/questions/5277230/are-there-any-restrictions-on-postgres-column-alias-names).  Consequently, special meaning is had through verbage instead of special characters.

__inline comments__
```simpex
'user_id__'comment
```

__self_reference__
`id__parent` referencing `id` in same table, but for tracking the parent

__foreign columns__
```simpex
table'_id'
\ fc = foreign column
'fc_'table'__'column \ no tables should have `__`, so first `__` is indicative of end of table name and start of column name.  In the case the referenced column has a comment, there might be double comments
```

__misc__
Column type un-necessitates column name qualifications like "date" in "created_date"

__external id__: since the name of an external id can conflict with naming conventions (`amazon_id`), prefix with `ext_` (`ext_amazon_id`).

__standard names__
-	`record_created` when the record, and not necessarily the represented object, was created
-	`record_updated` when the record, and not necessarily the represented object, was updated
-	`created`, `updated`, `occurred` happenings to the represented object
-	state indicator: prefixed with `is_`
-	action indicator: prefixed with `was_`, `will_`
	-	even for odd combos, like `was_occurred`
-	for some rows, there is a primary content field, that is considered the entity (ex: a "message" field in a "message" table)
	-	`text` for primary text content (instead of "message", "content", ...)
	-	`data` for interpretted primary content, affixed with type, ex: `data__json`


### Scope

Ignore custom base prefixes for tables in column naming.  `wp_user.id` referred to with `user_id` not `wp_user_id`.

#### Resolving context
Normal unprefixed columns:

-	priorities
	1.	stacked context (append name to self)
	`user.type_id` refers to the stacked context of `user_type`.`id`
	2.	followed by absolute context
	`user.type_id` refers to `type.id` if `user_type.id` doesn't exist
	3.	contexted baseline context (append name to baseline)
	for some module that has a prefix for all it's tables, append column afterwards: ex: `moduleX_item.type_id` refers to  `moduleX_type.id`

Prefixed with `_`
-	to disambiguate which of the 3 unprefixed columns types a column might be, a `_` can be used to indicate type #3
	-	this is usually particularly helpful if a baselined column name would conflict with absolute column name:
		-	ex:
			-	`moduleX_post._user_id` to `moduleX_user.id`
			-	`moduleX_post.user_id` to `user.id`
-	the `_` prefix for #3 is unnecessary unless the column is or might become ambiguous with #2 or #1
A special case exists of `_id`, where in the module prefix also exists as the primary item table
-	`moduleX_post._id` would refer to `moduleX.id`
(@note	if `moduleX.user_id` existed, a `moduleX_post._user_id` would still prefer `moduleX_user.id` if it existed)


Prefixed with `__`
-	use absolute context
	`user_comment.__type_id` refers to `type.id`

De-possession (`users` to `user`) on resolution of stacked context:
-	`users_comment.type_id` would refer to `user_comment_type.id` (style 1)



### Table Referencing
Use first letters of each word.  On collisions, start adding numbers from 1:
-	`user_folder uf left join unified_field uf1 left join unix_filesystem uf2`




# Comments

## Modern DocBlock

[Thoughts On PHPDocumentor](https://clrmo.com/2021/06/thoughts-on-phpdocumentor/)

The original DocBlock, with it's space-asterisk, existed to make the presense of the comment apparent in editors that didn't have syntax highlighting.  These days, developers use editors that have syntax highlighting, and such ornamentation is unnecessary.  That a comment starts with `/**` is enough to indicate it is a documentation comment.

## Custom Documentation Comments

### Working With current documentation generators

**Avoid Misinterpretation**

Custom comments can break documentation generators.  Consequently, some custom comment blocks like
```php
/** example
arbitraty code
*/
```
Should not be indicated as a doc block, and should use `/* example ... */` to prevent breaking the documentation generator with the code it will misinterpret within the example.


**Consider Order**

Various custom comment blocks are presented here.  Let's consider how existing documentation generators will interpret them

```php
/** params
< >
< >
*/
```

This will be interpretted like `summary = "params", description = "<>\n<>"`.  Although it can be useful to have this information presented somewhere in the documentation, and putting it in the description is fine, having the `params` part being labeled as the summary is bad, so make sure to put the summary before the custom documentation comment blocks.
```php
/** summary text
Description text
*/
/** params
< >
*/
```





### Supplemental

Additional comment blocks are supplemented here for a standard.  For example, to show an example code that uses a method, start with `/* example`
```php
/** example
$instance->my_method('blah')
*/
```
To abstract the method from the class, use `.` to indicate an instance method and use `:` to indicate a static method
```php
/** example
.my_method('blah')
:my_method('blah')
*/
```
To abstract the method itself, use `this`
```php
/** example
.this('blah')
:this('blah')
*/
```
To qualify/describe the example the example, add a description, separated with `, `:
```php
/** example, shows how to get x
*/

```


To illustrate multiple examlpes in a single block, use `/** examples`.  The means of separation is left to the programmer.


To document structured parameters, use `/** params` and see [Parameters](#parameters)

To document a structured return, use `/** return` and see [Return](#return)

Have a comment block devoted to non-machine-interpretted information about the code (like a long description) `/** about`


### Special Tokens
(under consideration)

Inside of an area that is supposed to be plain text, some special tokens can be used
-	@{ref: x} to reference a thing 'x' within the page or codebase
-	@{def: x} to define a thing 'x'


## Grouping
Grouping, hierarchical comments are relative to both the code and the hierarchy.  To indicate the level of hierarchy without compromising the code indentation level, a separated indentation is necessary.  

After the comment indicator, the indentation is in tabs (no spaces)

Further, the tab start of the comment should remain the same within the group

```ruby
#++	description {
blah()
if(true){
#++		description2 {
	blah()
#++		}
}
blah()
#++	}
```

Where a group without hierarchy is desired, a tab is not necessary:
```ruby
#+ description {
#+ }
```

There is an issue of sectionalizing code that already has `#+` style comments.  Two ways of doing this
1.	no-indent, inner `#+`, use brackets to determine
```ruby
#+ description {

#+ description2 {
#+ }

#+ }
```
2.	named comment group

```ruby
#[group name] description {
#[group name] }
```
Since the intent is to make the grouping unique within its brackets, the group name can just be a marker unique to the grouping area (not necessarily unique to the file)
```ruby
#[1] description {
...
#[2] description {
...
#[2] }
#[1] }
#[1] description {
...
#[1] }
#[] blah {

#[] }
```



If desirable, a section header comment can be used:
```ruby
#+++++++++++++++     Configuration     +++++++++++++++ {
#+++++++++++++++ }
```




Comments that describe a grouping comment should go after the grouping comment start, using `<` to indicate the comment is for a previous part

```rb
#+	bob's group{
#< bob's group is a bunch of collected snails
#+	}
```



## Misc
To draw attention to a not-grouping comment
```ruby
#+++++++++++++++     Configuration     +++++++++++++++
```

Table of contents.  Without an automated generator, from groups, it may be desirable to add a comment table of contents
```c
/** toc

bob's group
	sub group 1
	sub group b
bill's group

*/

```



## Inner Sections

The purpose of a commend is described at the commend start.  The purpose of this next comment is to display "Examples":
```coffee
### Examples
...
###
```

When a multiline comment contains multiple sections, use `@` to name and start each section
Sections defined within
```coffee
###
@example
	...
@note: ...
###
```

When the purpose needs delineation from further description, or if purpose on same line as details, use `:`
```coffee
# return : true or false
```



## Attention
```coffee
# **deprecated**, blah blah
```



## Descripters (non-values)

-	descripters should be next to and on the right of the value they describe, or in place of the value in the data structure
-	all non values contained in '<>'
```simpex
	'<'comment'>'
```
-	multiple non values can be contained in single '<>' by splitting with '|' or by separate '<>' tags
```simpex
	'<'comment1' | 'comment2'>'
	'<'comment1'><'comment2'>'
```


### escaping
a '>' following a special character is not considered a closing capsulater.  Special characters are `[^a-z0-9 ]`

```simpex
'< 2 \> 1 >'\here the comment is 2 > 1
```



### Special types
-	describing the value type
```simpex
	'< t:'type' >'
	'< type:'type' >'
```
-	Default value
```simpex
	' <d:'value' >'
	' <default:'value' >'
```
-	Default complex value using reference
```simpex
	'@ref	{bob: < bool >}'
	'< d:@ref >'
	'< default:@ref >'
```

-	flags
	-	optional
```simpex
		'< f:optional >'
		'< flag:optional >'
```
-	Optional
```simpex
<?>
```
-	Example value
```simpex
< ex:'value' >
```
-	local reference value
```simpex
'< @'referenceName' >'
```
-	global reference value
```simpex
'< @@'referenceName' >'
```




## Parameters

The parameters comment block should be defined with a multiline comment in the fashion `/* params`.
To describe a single parameter as a comment block, use `/* param PARAM_NAME`



Parameters documentation uses _Descripters_.

```coffee
### params
	< v1 > < t: string > < COMMENT >
	< v2 > < t: object >
		key1: < value >
		key2: < value >
	< v3 > < t:array >
		< child1 >
		< child2 >
		...
###
bob = (v1, v2, v3)->
```
-	syntax is intepretted with indents like coffeescript
	-	indented sequential lines are intended to represent the structure (like with coffeescript)
		-	`v2` is intepretted as an object by it's keys being in object property form.  `v3` is interpretted as an array because its keys are in array form
-	the parameters are specified with `< NAME > < COMMENT >`.  The use of the name, in addition to the position, is b/c the parameters can be both positional and named parameters (php 8 allows specifying positional parameters by name instead of position)
-	`...` used to indicate a repeating pattern of parameters



### Positional and Non-positional
Positional parameters are implied by their being on their own line, and it is expected all such parameters are present sequentially.

For the special case of non-positional named parameters (supported in some langauges), use a secondary, double newline separated object:
```coffee
### params

< v1 >
< v2 >

bob: < a value for bob >
sue: < a value for sue >

###
```

### Long Descriptions
In some cases, the `< >` describer part may be better separated into multiple lines.  To prevent this from being interpretted as multiple parameters, use `<( ...  )>`
```coffee
### params
< param1 >
< param2 > < description of param 2 > <(
	< continued description blocks for param 2 >
	< more description >
)>
< param3 >
###
```


### Variable Value
Use `||` to indicate OR'ed, and `()` for grouping

```coffee
### params
	< under_4> (1 || 2 || 3)
###

### params
	< under_4> (
		1
		||
		2
		||
		3
	)
###
```

However, with complex data which uses inferred strucure (wherein a newline indicates a new array item or dictionary item), newlines of the structure can conflict with newlines intended for logic semantics (grouping and OR'ing (`()` and `||`)) .  Consequently, newlines surrounding logic semantics (`()` and `||`) should be assumed to be for those symbols, not for structural inference, and, wherein a conflict may arise, a non-implied description of structure (explicitly using `{}` or `[]`) should be used for structure description.

```coffee
### params

< bob > (
	{
		< key > : < value >
	}
	||
	[
		< value >, ...
	]
)

###
```
-	`{}` is used explicitly, instead of relying on the presence of a `key:value` indicating structure
-	new lines surround both `()` and `||`, and do not imply data structure



### Variable Params
In the Case where the function can take different sets of parameters.

Use same syntax as [Variable Value](#Variable Value), but group the parameters used brackets:
```coffee
### params

(
	< value1 >
	< value2 >
)
||
(
	< value a >
)

###
```


#### Expandable Parameters
```coffee
### params
< value >
...
###
```




### Structure Comments
In cases like variable parameters, it may be useful to describe circumstances of the variability.  To do so, include a prefacing comment in it's own grouping


```coffee
### params

( (for providing multiple values)
	< value1 >
	< value2 >
)
||
( (for providing a single value)
	< value a >
)

###
```



### Escaping
It is sometimes desired to include symbols like `>` in the description.  As such, quotes are prioritized above brackets:
```coffee
###
< value `>` 10 >
< value '>' 10 >
< value ">" 10 >
< `value > 10` >
< ` > 10` >
###
```



## Return
Same syntax as params, including use of a variable return

```coffee
# scalar value
### return

< value >

###

# implied array
### return

< value >
< value >

###

# implied object
### return

key1: < value >
key2: < value >

###

# conditional return
### return

( (if search returns a car)
	< car object >
)
||
( (if search returnss a person)
	< person object >
)
||
( (if search returns mulltiple objects)
	[
		< object >, ...
	]
)

###
```





# CRUD+L
The presentation to others might not conform to standard language.  The url path is not a presentation to others.  However, the presents the issue of, on a "read" page, the title is "view".  However, considering that there is no necessary fidelity of back end naming to front end presentation naming, this is not an issue.  So, the issues are:
-	what should the front end presentation cal CRUD parts?
	whatever is appropriate for the situation
		-	generally: create, view, edit, delete, list
		-	messages: send, view, edit, delete, inbox
-	what should the back end call CRUMD parts?
	The convention, along with the actual db names, is CRUD.  This is fine.  So, create, read, update, delete, list.




# Organisation of files for projects
## Within pub/pri web projects
-	`public`
	-	common asset type folders (`js`, `css`, `img`, `video`, `audio`)
	-	`asset` folder - non-conforming asset, or, in case not desiring to make specific folder like `video` since insufficient significance
	-	`pkg` non-project assets and tools

-	`tool`: multi command use tools
-	`control`: web based system command interface
-	`cli` : command line interface scripts
-	`template`
-	`pkg` and/or `vendor` : tools not specific to the proejct
-	`storage` - instance specific storage (ie, particular to site or environment (production, testing) of site  )
-	`about` - documentation and deployment info
	-	`deployment` doc for understand how to deploy
-	`asset` static, non executed assets
-	All `*.ignore` are excluded from versioning
-	All `*.private` are excluded from versioning, such as
	-	`cli.private/`
	-	`doc.private/`


Server side different-than-project language can either go into `pkg` or into `control/cli` or `tool`




# Spacing






```js
// newline = +1 indentation.  Group enclosure = +1 indentation.  
//	Multiple group enclosures can be combined to a single indentation.
//	tab separate enclosure endings if combined on their own line
bill = {bob:{sue:{joe:'monkeys'}}}
bill = {bob:{sue:{joes:[
		'monkeys'
		'moos',
		'miles'
	]	}	}	}

// if items are apart from their enclosure line, keep the enclosure brackets separated (on separate lines) for additional items:
bill = [
		{
			bob: 123
		}
		,{
			sue: 456
		}
	]

// Both comma next to ending item and command next to first item have pros and cons when typing and editing
// allows for quick deletion:
bill = [
		{
			bob: 123
		}
		,{
			sue: 456
		}
	]
// allows for multiline editing of all items
bill = [
		{ bob: 123 },
		{ sue: 456 }
	]


// Multiline if, put bracket on separate line for clarity
if(bob
	&& sue
	&& bill
){
	doMonkeys()
}

// May be more convenient to separate `if` from condition line
if(
	bob
	&& sue
	&& bill
){
	doMonkeys()
}

```


## PHP PSR 12
It was a bit odd to me that a "Framework _Interoperability_ Group" would recommend a coding style that is apart from functionality.

I've seen a few reason why people excuse mandating a coding standard
1.	"to reduce cognitive friction when scanning code from different authors"
2.	coding style related commit diffs obscure actual code changes

The reality is, these two excuses hide the real issues.  Let's explore.

For issue #1, any code that adheres to some sane coding style has never caused me cognitive friction, even when there are mixed styles, so long as the indentation is the same.  I've worked on many different code bases with different styles, and the only time there is an issue is when the coder has abandoned clarity (such as not maintaining proper indents).  The real issue behind #1 is actually split into two issues:
1.	some programmers program without clarity
2.	some programmers encounter "cognitive friction" when seeing a coding standard apart from what they are used to

Requiring a specific coding standard gets rid of sub issues 1 and 2.  

But, I've never met a sub-issue-2 programmer.  Further, code without clarity is readily identifiable.  Forcing a coding standard helps prevent a bad programmer from showing up through their unclear code.

For issue #2, this is again split into two issues
1.	formatting changes should be a different commit
2.	formatting changes should not be a commit for code that is already clear

Let's say the `!!` was accidental, and someone with a different style edits the following code:
```
if(!!$is_active){
	#...
}
```
changes to
```
if ( !$is_active )
{
	#...
}
```
Even in this case, if this were a commit, it would still be clear what was changed.  

What becomes a problem is when a programmer modifies code for style purposes only, and commits that with non-style related changes.  It is a problem that pretty much would only occur with a bad programmer, who did something like, auto-style-format a file after making a change to the logic, and then committed the file.  The absence of mind not to realize that the logic change will be buried in the style changes does indeed point to a bad programmer.

Normally, when a programmer x with style 1 edits code from someone else that is styled with style 2, two outcomes might occur:
1. the sections of the code that programmer x modifies/adds become style 1
2. programmer x uses style 2 for this code

Either of these cases is not a problem so long as both style 1 and style 2 are clear and the indentation is the same.  Amusingly, with outcome #1, it makes it more immediately apparent there was a different programmer who worked on the code.  But, even in the case of #1, back in 2006, when i was working with svn, I wrote an svn hook that took the code I submitted and formatted in accordance to the project coding standard.  With such a hook or auto formatter, it wouldn't matter if the outcome was #1, because the code that made it to the project would remain standard to one style.

But, if outcome #1 occurred with no hook, the issue with mixing styles is primarily an issue of indentation type.  Given editors now will adapt the indentation type to the indentation type present in the  file, this is normally not a problem.

[Google](https://google.github.io/styleguide/cppguide.html) has some insights on coding standards

-	`The benefit of a style rule must be large enough to justify asking all of our engineers to remember it. `
-	`When something surprising or unusual is happening in a snippet of code (for example, transfer of pointer ownership), leaving textual hints for the reader at the point of use is valuable`
-	`"Just pick one and stop worrying about it"; the potential value of allowing flexibility on these points is outweighed by the cost of having people argue over them.`
-	` Consistency should not generally be used as a justification to do things in an old style without considering the benefits of the new style, or the tendency of the code base to converge on newer styles over time`
-	`The basic principle is: The more code that fits on one screen, the easier it is to follow and understand the control flow of the program. Use white space purposefully to provide separation in that flow.`

This last item is something that used to matter to me with smaller screens.  When you have functions defined like

```
function bob ()
{
	#...
}
```
That extra line  for the opening brace takes up space and accumulates across multiple functions, reducing the total amount of code on the screen.  I tried to find why people prefer this style, and I get:
1.	easy to visually spot the beginning of the code block
2.	the presence of the function/method is clearer

Both of these justifications are trivial.  For #1, that is what indentation is for.  For #2, get an editor that better highlights method/function definitions so they are clearer.

In reality, my impression is that some programmers get intimidated by dense code, and the extra newline for braces helps to reduce the intimidation.

When screens were smaller, and I couldn't rotate my screen 90 degrees to allow even more lines of code, extra newlines meant my capacity to see a large number of useful programming lines was reduced.  When you are getting the sense of a large class, the ability to rapidly scan it, scan through the methods, with the ability to arbitrarily drill in visually, is important.  

To magnify this issue, imagine having an editor that was only 5 lines tall, and think of how long it might take to understand a large class with thousands of lines.

But, as screens are large and rotatable now, the extra line isn't as much of a problem.


__In conclusion__
-	PSR-12 is good enough and will probably improve interoperabilty of code between developers
-	I have minor reservations about PSR 12
	-	The use of camel casing
	-	The [use of spaces](tabs-or-spaces).  However, with larger screens, 4 spaces does not usually push deep logic too far to the right.
	-	The use of a new line for starting `{`.  However, since this does not apply to conditionals, the amount of wasted vertical space is limited and acceptable
-	Different styles don't matter so long as they are clear.  The mixing of styles is mostly a problem when tabs and spaces are mixed, which can be prevented by a good editor.  Programmers have been modifying each others code for decades and different, clear, styles has not prevent this interoperability
-	PSR 12 should not be a FIG PSR.  The point of interoperability is whether some code will work in different environments.  PSR 12 is for how programmers react to code, not for how the code functions in different environments.





## Tabs or spaces

<!-- https://softwareengineering.stackexchange.com/questions/57/tabs-versus-spaces-what-is-the-proper-indentation-character-for-everything-in-e -->

I tried to get the argument for spaces, and all I get is
-	spaces show up the same regardless of editor
-	some editors do handle tabs well

Since most coding software allows for customization of tab size, this reason for using spaces in serious programming has always baffled me.  I've never seen a programmer use an editor where the tab character is a problem, but I've seen multiple system administrators using vi or nano that complain about the use of tabs.

There are cases where spaces are necessary, (lining up columns with variable character count starts), but, here, spacing is used for design, not for indication of flow level; and, spacing can be used along side tabs for that purpose
```
	var a = 1,
	    b = 2;
```

Given that indent size readability is variable (my preference is 3 spaces, or 2 for deeply nested code), it further baffles me that people would argue for mandating something that variably reduces readability (depending on person) for code.

In general, people (and google search) don't make a good case for spaces.  

**So, I will present the primary reason I suspect is behind using spaces.**

Back in the early days, you would likely be editing in an editor that did not provide syntax highlighting and did not easily allow configuring tab size.  In this situation, spaces were preferred, and a great deal of code was written with space indentation.  When editing this code, or when copying pieces of this code into new files, if you are using tabs, you will have an unwanted mix of tabs and spaces for indentation.  Given most people do not have their editor set to show the space and tab character as anything but empty space, the mix of spaces and tabs becomes variably problematic depending on what editor is looking at the file.  This mixing annoyance, given the previous standard of using spaces, would necessitate a standard for using only spaces (so as to be compatible with old code and to prevent the accidental mixing of tabs and spaces).

_So, the real reason for using spaces is because of legacy, and then conformity._

Even if you start a new project, because the legacy of old code mandated spaces, you will be met with the cases of:
1.	if you import code into your project, that code will likely be indented with spaces, resulting in the potential for accidental mixing of indentation style
2.	good/old programmers you hire will likely be accustomed to using spaces for indentation

So, the choice becomes
1.	use the better tab indentation but account for unintentional mixing with space indented libraries
2.	use the worse space indentation and don't worry about mixing

If you don't care that much, #2 is the better answer with team projects.  However, even with team projects, #1 is do-able.  Just write a commit hook that prevents mixing.

This situation also helps describe the type of people that think (not just prefer) one is better than the other:
-	spaces
	-	is an administrator
	-	is a designer
	-	un-questioningly conforming programmers
	-	veteran programmers who've dealt with the mixing issue and know the trade offs
-	tab
	-	programmers optimizing apparent functionality (good programmers that have not encountered the mixing issue, likely b/c of lack of variable team experience)
	-	idealists

This also explains the constant arguing.   When arguing occurs in the following combinations of people, there is likely no solution:
-	administrator with good programmer
-	designer with good programmer
-	un-questioningly conforming programmer with good programmer

What I found odd is that veteran programmers will unconsciously know this space tab trade off, but I have found no account of one of them describing it online or in person.

Personally, for my own projects, I use tabs for files I create, and I use whatever indentation is present for files originated by others.  I don't find indent mixing is an issue b/c my editor displays the space and tab characters in different colors, and I have a script for converting whenever I want to mix code.  But, as I said, for team projects, you have to consider what I mentioned above.


As a side note, I would have previously argued against 4 spaces.  In the past, I've dealt with logic indented deeply enough that the use of 4 spaces per indent resulted in the logic going off my screen, and I had to scroll horizontally.  But, with bigger screens, this has not been a problem.


__Misc Reasons For Tabs If Not Considering Teams And Legacy__
-	Less characters
-	Configurable width
-	No issue with accidental +-1 space






# Markdown Doc
Quoted section with markdown highlights
```
"""
bob _is_ here
"""
```

Section ending
```
# sect1
## sect2
blah sect 2 stuff
##<
sect 1 stuff
```




# PHP
## HTML Integration
```html
<?php
do_something()
?>
<div><?= $name ? $name : 'Guest' ?></div>
<?php	if($tier == 1){ ?>
<div>Special offers Tier 1</div>
<?php }elseif($tier == 2){ ?>
<div>Special Offers Tier 2</div>
<?php		if($recommendations){ ?>
<?php
			do_some_poorly_placed_logic_here();
			do_more_logic();
?>
<!-- recommendations html -->
<?php		}?>
<?php	}
```
-	HTML and PHP should be separated indented
-	Flow logic should be on it's own line, with observed indentation, surrounding by open and close tags
-	Although PHP code at the beginning and end of an HTML can start with no indent, all PHP mixed into HTML should start as a base with one indent.  This is to visually distinguish the code.





# JS
## This
Sometimes the parent `this` must be used.  Further, some times some parent's `this` even further up must be used.  To do this, the parent assigns `this` to some variable usable within the child context.  The naming of the arbitrary parent's `this` can be one of two things
1.	`that`.  Usually there is only a need to get a single parent's `this`, and which parent is expectable.  If there are multiple levels of `this` required, the additional levels can be identified by appending an incrementer (ex `that2`).
2.	an identifying name for the parent