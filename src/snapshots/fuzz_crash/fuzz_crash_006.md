# META
~~~ini
description=fuzz crash
type=file
~~~
# SOURCE
~~~roc
 HO||v
~~~
# PROBLEMS
**ASCII CONTROL CHARACTER**
ASCII control characters are not allowed in Roc source code.

**ASCII CONTROL CHARACTER**
ASCII control characters are not allowed in Roc source code.

**MISSING HEADER**
Roc files must start with a module header.

For example:
        module [main]
or for an app:
        app [main!] { pf: platform "../basic-cli/platform.roc" }

Here is the problematic code:
**fuzz_crash_006.md:1:2:1:5:**
```roc
 HO||v
```


**INVALID STATEMENT**
The statement **expr** is not allowed at the top level.
Only definitions, type annotations, and imports are allowed at the top level.

# TOKENS
~~~zig
UpperIdent(1:2-1:4),OpBar(1:4-1:5),OpBar(1:6-1:7),LowerIdent(1:7-1:8),EndOfFile(1:8-1:8),
~~~
# PARSE
~~~clojure
(file (1:2-1:8)
	(malformed_header (1:2-1:5) "missing_header")
	(statements
		(lambda (1:4-1:8)
			(args)
			(ident (1:7-1:8) "" "v"))))
~~~
# FORMATTED
~~~roc
|| v
~~~
# CANONICALIZE
~~~clojure
(can_ir "empty")
~~~
# TYPES
~~~clojure
(inferred_types (defs) (expressions))
~~~