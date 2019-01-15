# Terms
Term | Meaning | Use
--|--
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

## Class
Camel casing.  If abbreviation is necessary, separate from remainder with `_`:
-	`US_Utah` instead of `USUtah`

### Is or About
If a class is about something, not about any particular one, pluralize:  `Users`
If a class instance represents something, such as a user, name it as the thing would be called: `User`.  

Since an instance representing multiple of something, such as an instance representing multiples users, is rare, pluralized classes like `Users` is assumed to be about users, and not some instanceable selection of users.

Compounded, non possessive class names take on the broadest context:
With something like `user types`, there is a set of `types` that can be attributed to users (`user_type`) and there is a set of `types` which are attributed to specific users (`users_type`).  A `UserTypes` class would have the context of both, whereas `UsersTypes` would be a class specifically for handling types actually associated to users, perhaps constructed with a particular user.

On pluralized "about something" classes, on assumed context functions, assume singular subjects unless:
-	`Users.get` will get a single user
-	`Users.gets` will get multiple users
-	`Users.users_get`  will get multiple users

## Functions
### Standard Format, Possessive Tense

__General Format__
`subject` `act` `modifiers`
-	`bob_remove_from_users_table`

The point of stacking the subjects first is to name-categorize functions:
`type_add, type_delete, status_add, status_delete` vs `add_type, delete_type, add_status, delete_status`



__Plural vs Possession__
use `s` to indicate possession
-	`users_type_get` : get a user's type
-	`user_type_get` : get a `user_type`

This conflicts with pluralization, and there are multiple ways to pluralize
-	context based
-	affixing `s` to action
-	affixing `_s` to function name
-	using `ss` on subject

Context: Possession can be distinguished from plurality based on the following word
-	`users_type_find` : here, the folowing word is `type`, which can be possessed, and thus assumed is that `users` is possessive
-	`users_find` : the following word is `find`, which is not possessed, but applies to the subject, and thus `users` is plural

Affixing `s`:
-	`user_gets` : get multiple users

Affixing `_s` to function name:
-	`user_get` : get a user
-	`user_get_s` : get multiple users
(Although this is often easy enough to do in the language without needed to create a separate function)

Using `ss` on subject
-	`userss_type_get` : get the type for each user

There are some more scenarios to consider
-	`user_type_get` : get a user type (not specific to any particualr user)(see db naming)
-	`users_type_get` : user has single type
-	`users_types_get` : user has multiple types
-	`userss_type_get` : get, for each user, a type
-	`userss_types_get` : get, for each user, multiple types
@note: although `userss_type` will return many, potentially different types, it is kept as `type` to distinguish it from the additional scenario of each user having multiple types - since it is almost never the case that we want to reduce the set (ie, find a shared type between users)

__Part Confusion__
In the rare case that a subject and act can be confused, use `__` to separate



__Sequence__
In the scenario a name for a common sequence can not be determined, just list the sequence
`user_delete__scoreboard_update`

__Subject Preference__
Stack the subject rather than the predicate: prefer `users_type_get` over `user_get_type`

__Set Notion__
The standard for `s` application is based on the idea the situation is usually a one to many, each to each, or each to many.  It is possible for there to be a many to one scenario.  What if all the users had just one house?  `userss_house` is standardly interpretted as a house for each user.  And, what if a group of 20 users shared 4 houses, such that `userss_houses` was intended not to be each to many, entailing 20 or >20, but instead each to one of, entailing the 4 houses.  

In these rare cases, use an `_n` prefixes to indicate there is a numeracy concern: `userss_n_houses`, `userss_n_house`

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

### Plural and Singular
The desire here is to provide allowance for conventional naming along with the custom possession style naming:
`item_id` : an item's id (conventional styling)
`item_ids` : id of each item (conventional styling)
`items_id` : an item's id
`items_ids` : an item's multiple ids
`itemss_id` : id of each item
`itemss_ids` : ids of each item (each item has multiple ids)

### Class
When a variable represents a class instance, capitalize first letter
`$User = new User` vs `$user = 'bob'`



## Database
On names alone, without rather obtuse naming, interfering with the normal {name describing value} method, having a naming system that allows automated table connection mapping is not feasible for handling the potential complexity.  As such, it is not attempted.


### Table Naming
It is assumed a table contains many records, so `user` and not `users`.  Instead, plural form is used to indicate possession.  `users_group` indicates a map between a user and a group, and `user_group` indicates a group in the `user` context.  So, if a user potentially had many types, we would have
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
'fc_'table'__'column \ no tables should have two `__`, so first `__` is indicative of end of table name and start of column name
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
-	prefer stacked context
	`user.type_id` refers to the stacked context of `user_type`.`id`
-	followed by absolute context
	`user.type_id` refers to `type.id` if `user_type.id` doesn't exist
Prefixed with `__`
-	use absolute context
	`user_comment.__type_id` refers to `type.id`
Prefixed with `_`
-	refers to some base point.  This can vary depending upon the module
	-	`itemX_event_history._id`  refers to `itemX.id`.  This is based on the idea `event_history` is a module like table, and it should, within it's own context, know of some table `itemX_event_type` so as to refer to it with `_event_type_id`, but it does not know about what `itemX` might be named.


De-possession on resolution of stacked context:
-	`users_comment.type_id` would refer to `user_comment_type.id`


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
Described in coffee syntax, as inferred array
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


For special case of non-positional named parameters, can use secondary object:
```coffee
### params
< v1 >
< v2 >

bob: < a value for bob >
sue: < a value for sue >
###
```


### Variable value
Use `||` and `()`

```coffee
### params
	< under_4> (1 || 2 || 3)
###
```

However, with complex data which uses inferred strucure from newlines can conflict with newlines from the use of `()` and `||`.  Consequently, newlines surrounding `()` and `||` should be assumed to be for those symbols, and more verbose description should then be employed for the data description:
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

### Variable Params
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

bill = {bob:{sue:{joe:'monkeys'}}}
bill = {bob:{sue:{joes:['monkeys'
	'moos',
	'miles'	]	}	}	}

// This can be more convenient for editing the list contents
bill = {bob:{sue:{joes:[
	'monkeys'
	'moos',
	'miles'
]	}	}	}

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
