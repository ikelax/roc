# META
~~~ini
description=Type annotation missing parentheses for type application
type=file
~~~
# SOURCE
~~~roc
module [nums]

nums : List U8
~~~
# PROBLEMS
**PARSE ERROR**
Type applications require parentheses around their type arguments.

I found a type followed by what looks like a type argument, but they need to be connected with parentheses.

Instead of:
    **List U8**

Use:
    **List(U8)**

Other valid examples:
    `Dict(Str, Num)`
    `Result(a, Str)`
    `Maybe(List(U64))`

Here is the problematic code:
**type_annotation_missing_parens.md:3:15:3:15:**
```roc
nums : List U8
```


# TOKENS
~~~zig
KwModule(1:1-1:7),OpenSquare(1:8-1:9),LowerIdent(1:9-1:13),CloseSquare(1:13-1:14),Newline(1:1-1:1),
Newline(1:1-1:1),
LowerIdent(3:1-3:5),OpColon(3:6-3:7),UpperIdent(3:8-3:12),UpperIdent(3:13-3:15),EndOfFile(3:15-3:15),
~~~
# PARSE
~~~clojure
(file (1:1-3:15)
	(module (1:1-1:14)
		(exposes (1:8-1:14) (exposed_item (lower_ident "nums"))))
	(statements
		(type_anno (3:1-3:15) "nums" (ty "List"))
		(malformed_stmt (3:13-3:15) "expected_colon_after_type_annotation")))
~~~
# FORMATTED
~~~roc
module [nums]

nums : List
~~~
# CANONICALIZE
~~~clojure
(can_ir "empty")
~~~
# TYPES
~~~clojure
(inferred_types (defs) (expressions))
~~~