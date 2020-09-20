# Terms
Term | Meaning | Use
--|--|--
reacter | fn that reacts to potential states | `function reacter_complete(){}`




# Naming Methodology

-	Use camel case for familiar or related or heavy use terms (classes, modules)
	-	except when the name becomes too dense to read quickly
-	Use `_` for everything else, including variables
-	use general to specific in most cases (Ex `country_state_city`)
-	Pluralization (see stdlang)
```
Use singular for general cases and categories.  That something is inherently plural and needn't be pluralised can be seen in:
-	a database table, which inherently has many
-	a folder "img", which wouldn't make sense only having 1
When reasonably potentially ambiguous, use plural.  
```
-	use generic reference "item" when possible (ex: "item" instead of "blog") with potentially reuseable code
	-	allow "item" to take place of "entity" (it's easier to type and nearly as inclusive)
-	variables containing path strings:
 	-	`dir`: trailing /
	-	`path`: no trailing /
-	Capitilize singular tools, such as classes, or named tool collections that do not change on a project basis (ex, `Amazon` ,even though not a class, is a bunch of general tools that won't change between projects).



## Preference of name category separation

The characters of separation are often `.` or `-` or `:`.
```
Las Vegas, Nevada, United States, Earth
earth.us.nv.las_vegas
earth-us-nv-las_vegas
```

There are problems with these characters:

-	not all environments support them in names
-	a name part itself may contain the character, thus confusing part of a name part with the name part separation

`__` is almost never used within a name part, and it is almost always available as part of the naming characters.  The downside is that it is ugly and it is two characters.   However, it provides consistency and reliability, and you have not encountered scenarios where there is a need for the separation to be just one character, so use this form of separation
```
earth__us__nv__las_vegas
```
-	In cases of multiple separation contexts, increase `_` as necessary



## Naming Ambiguity

__Possession Vs Pluralization__
There is a conflict between pluralization and possesion: `users_items`.  Is this
-	a user's items
-	mulltiple users' items?

Plural tends to be the standard interpretation.  To allow for possessive in a manner not ambiguous, a `z` is used where an `'s` would normally be used.  `z` is immediately recognized as awkward, forcing interpretation, which easily comes to be possessive.
Using this convention, `s` is never used to indicate possession and can alwasy be expected to mean pluralization

The general format of naming is `{context_subject}_{primary_subject}` - a narrowing style. The pairing can still result in ambiguous cases
-	`itemsz_id`
	-	is this a single id of multiple items
	-	is this a single id for each of multiple items
-	`usersz_types`
	-	is this a type for each of multiple users
	-	is this multiple types for each of multiple users

__Set Pairing__
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
Camel casing.  If abbreviation is necessary, separate from remainder with `_`:
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
the subject first, then the predicate `has_role($user, $role)`



## Variables
See @{Naming Ambiguity}


### Class
When a variable represents a class instance (of a non-native class), capitalize first letter
`$User = new User` vs `$user = 'bob'`


### Increments
For incremental variables, like `i`, that overlap in nested loops, start with `i`, then add numbers starting from 1: `i, i1, i2, i3`



## Database
-	no effort towards programmable meaning from naming: On names alone, without rather obtuse naming, interfering with the normal {name describing value} method, having a naming system that allows automated table connection mapping is not feasible for handling the potential complexity.  As such, it is not attempted.


### Table Naming
It is assumed a table contains many records, so pluralization is not necesary.  Ex: `user` and not `users`.  Instead, plural form is used to indicate possession.  `users_group` indicates a map between a user and a group, and `user_group` indicates a group in the `user` context.  So, if a user potentially had many types, we would have
-	`users_type` to list all the types a particular user had
-	`user_type` to list the potential types any user could have

Generally, when a table is possessed, it should have a possession indicator prefix. Ex `sales_source`.  And, a simple way to check is generally by mentally prefixing with `a` to see if it makes sense: `a sales source` (being sources attributed to a sale on a case by case bases) whereas `sale_source` being a definition of possible sources for sales.


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
A special case exists of `_id`, where in the module prefix also exists as the primary item table
-	`moduleX_post._id` would refer to `moduleX.id`
(@note	if `moduleX.user_id` existed, a `moduleX_post._user_id` would still prefer `moduleX_user.id` if it existed)


Prefixed with `__`
-	use absolute context
	`user_comment.__type_id` refers to `type.id`

De-possession (`users` to `user`) on resolution of stacked context:
-	`users_comment.type_id` would refer to `user_comment_type.id`


### Table Referencing
Use first letters of each word.  On collisions, start adding numbers from 1:
-	`user_folder uf left join unified_field uf1 left join unix_filesystem uf2`




# Parameter Order

If possible, follow the name of the function.  Ex:
-	`is_user_in_group_id` : `$user_id, $group_id`

Otherwise, the primary target (the thing of concern) should be first. Ex:
-	with `in_array`, the concern is the needle, and whether it is in some array.  So, it is `in_array($needle, $array)`




# Comments
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
/*?

bob's group
	sub group 1
	sub group b
bill's group

?*/

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



## Parameters
Described in coffee syntax, as inferred array, with starting and ending blank lines (read readability)
```coffee
### params

	< v1 > <t: string > < full name >
	< v2 > <t: int > < age >
	< v3 > < child array >
		< child >
			name:
			age:
			notes: [ <>, ... ]
			< variable key name > : <>
		...
	< v4 > <t: boolean> <d: true> <whether to create the user if doesn't exist>

###
bob = (v1, v2, v3, v4)->
```
-	comments indicated with `<>`
-	comments can be placeholder for data, or can precede the data
-	datastructure, as normal with coffeescript, is infered.  Here, `child array` is indicated by indentation without `:` and `child` dictionary is intefered by indentation with `:`.
-	explicit data structure sometimes useful for inlining, like with `notes`
-	`...` to indicate repetition of value - value which is indicated by empty comment `<>`


### Positional and Non-positional
Positional parameters are implied by their being on their own line, and it is expected all such parameters are present sequentially.

For the special case of non-positional named parameters, use a secondary, double newline separated object:
```coffee
### params

< v1 >
< v2 >

bob: < a value for bob >
sue: < a value for sue >

###
```

For readability, provide the first comment  (`<>`)  as the name of the parameter matching the name of the parameter in the function definition, and the following comments (on the same line) as the ones describing the  variable further

```coffee
### params
< name > < the full name of the person > <t: string >
< age > < the of the person at sign up >
###
fn = (name, age)->
	return
```



### Variable value
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
Case in which the function can take different sets of parameters,  wherein the number of parameter will change the expected values of the parameters at positions

Use same syntax as variable value, but at the parameter level:
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
Same syntax as params, including use of variable return and commenting

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



## Special types
-	describing the value type
```simpex
	'<t:'type'>'
	'<type:'type'>'
```
-	Default value
```simpex
	'<d:'value'>'
	'<default:'value'>'
```
-	Default complex value using reference
```simpex
	'@ref	{bob: <bool>}'
	'<d:@ref>'
	'<default:@ref>'
```

-	flags
	-	optional
```simpex
		'<f:optional>'
		'<flag:optional>'
```
-	Example value
```simpex
<ex:'value'>
```
-	local reference value
```simpex
'<@'referenceName'>'
```
-	global reference value
```simpex
'<@@'referenceName'>'
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
-	PHP code at the beginning of a HTML integrated file can start with 0 indentation
-	any PHP code after HTML must start with at least 1 tab indentation
-	any multi line PHP code must have it's `<?` and `?>` on lines to themselves
-	any logic lines must maintain their own indentation, specific to the PHP logic, and separate from the HTML indentation





# JS
## This
Sometimes the parent `this` must be used.  Further, some times some parent's `this` even further up must be used.  To do this, the parent assigns `this` to some variable usable within the child context.  The naming of the arbitrary parent's `this` can be one of two things
1.	`that`.  Usually there is only a need to get a single parent's `this`, and which parent is expectable.  If there are multiple levels of `this` required, the additional levels can be identified by appending an incrementer (ex `that2`).
2.	an identifying name for the parent