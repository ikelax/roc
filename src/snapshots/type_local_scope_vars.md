# META
~~~ini
description=Function-local type variables in separate scopes
type=file
~~~
# SOURCE
~~~roc
app [main!] { pf: platform "../basic-cli/main.roc" }

outer : a -> a
outer = |x| {
    inner : b -> b
    inner = |y| y

    inner(x)
}

main! = |_| {}
~~~
# PROBLEMS
**PARSE ERROR**
A parsing error occurred: `expected_expr_close_curly_or_comma`
This is an unexpected parsing error. Please check your syntax.

Here is the problematic code:
**type_local_scope_vars.md:6:5:6:12:**
```roc
    inner = |y| y
```


**UNEXPECTED TOKEN IN EXPRESSION**
The token **= |** is not expected in an expression.
Expressions can be identifiers, literals, function calls, or operators.

Here is the problematic code:
**type_local_scope_vars.md:6:11:6:14:**
```roc
    inner = |y| y
```


**UNEXPECTED TOKEN IN EXPRESSION**
The token  is not expected in an expression.
Expressions can be identifiers, literals, function calls, or operators.

Here is the problematic code:
**type_local_scope_vars.md:9:1:9:1:**
```roc
}
```


**INVALID LAMBDA**
The body of this lambda expression is not valid.

**UNUSED VARIABLE**
Variable ``x`` is not used anywhere in your code.

If you don't need this variable, prefix it with an underscore like `_x` to suppress this warning.
The unused variable is declared here:
**type_local_scope_vars.md:4:10:4:11:**
```roc
outer = |x| {
```


**INVALID STATEMENT**
The statement **expr** is not allowed at the top level.
Only definitions, type annotations, and imports are allowed at the top level.

**INVALID STATEMENT**
The statement **expr** is not allowed at the top level.
Only definitions, type annotations, and imports are allowed at the top level.

**INVALID STATEMENT**
The statement **expr** is not allowed at the top level.
Only definitions, type annotations, and imports are allowed at the top level.

**INVALID STATEMENT**
The statement **expr** is not allowed at the top level.
Only definitions, type annotations, and imports are allowed at the top level.

**NOT IMPLEMENTED**
This feature is not yet implemented: canonicalize record expression

# TOKENS
~~~zig
KwApp(1:1-1:4),OpenSquare(1:5-1:6),LowerIdent(1:6-1:11),CloseSquare(1:11-1:12),OpenCurly(1:13-1:14),LowerIdent(1:15-1:17),OpColon(1:17-1:18),KwPlatform(1:19-1:27),StringStart(1:28-1:29),StringPart(1:29-1:50),StringEnd(1:50-1:51),CloseCurly(1:52-1:53),Newline(1:1-1:1),
Newline(1:1-1:1),
LowerIdent(3:1-3:6),OpColon(3:7-3:8),LowerIdent(3:9-3:10),OpArrow(3:11-3:13),LowerIdent(3:14-3:15),Newline(1:1-1:1),
LowerIdent(4:1-4:6),OpAssign(4:7-4:8),OpBar(4:9-4:10),LowerIdent(4:10-4:11),OpBar(4:11-4:12),OpenCurly(4:13-4:14),Newline(1:1-1:1),
LowerIdent(5:5-5:10),OpColon(5:11-5:12),LowerIdent(5:13-5:14),OpArrow(5:15-5:17),LowerIdent(5:18-5:19),Newline(1:1-1:1),
LowerIdent(6:5-6:10),OpAssign(6:11-6:12),OpBar(6:13-6:14),LowerIdent(6:14-6:15),OpBar(6:15-6:16),LowerIdent(6:17-6:18),Newline(1:1-1:1),
Newline(1:1-1:1),
LowerIdent(8:5-8:10),NoSpaceOpenRound(8:10-8:11),LowerIdent(8:11-8:12),CloseRound(8:12-8:13),Newline(1:1-1:1),
CloseCurly(9:1-9:2),Newline(1:1-1:1),
Newline(1:1-1:1),
LowerIdent(11:1-11:6),OpAssign(11:7-11:8),OpBar(11:9-11:10),Underscore(11:10-11:11),OpBar(11:11-11:12),OpenCurly(11:13-11:14),CloseCurly(11:14-11:15),EndOfFile(11:15-11:15),
~~~
# PARSE
~~~clojure
(file (1:1-11:15)
	(app (1:1-1:53)
		(provides (1:6-1:12) (exposed_item (lower_ident "main!")))
		(record_field (1:15-1:53)
			"pf"
			(string (1:28-1:51) (string_part (1:29-1:50) "../basic-cli/main.roc")))
		(packages (1:13-1:53)
			(record_field (1:15-1:53)
				"pf"
				(string (1:28-1:51) (string_part (1:29-1:50) "../basic-cli/main.roc")))))
	(statements
		(type_anno (3:1-4:6)
			"outer"
			(fn (3:9-3:15)
				(ty_var (3:9-3:10) "a")
				(ty_var (3:14-3:15) "a")))
		(decl (4:1-6:12)
			(ident (4:1-4:6) "outer")
			(lambda (4:9-6:12)
				(args (ident (4:10-4:11) "x"))
				(malformed_expr (6:5-6:12) "expected_expr_close_curly_or_comma")))
		(malformed_expr (6:11-6:14) "expr_unexpected_token")
		(lambda (6:13-6:18)
			(args (ident (6:14-6:15) "y"))
			(ident (6:17-6:18) "" "y"))
		(apply (8:5-8:13)
			(ident (8:5-8:10) "" "inner")
			(ident (8:11-8:12) "" "x"))
		(malformed_expr (1:1-1:1) "expr_unexpected_token")
		(decl (11:1-11:15)
			(ident (11:1-11:6) "main!")
			(lambda (11:9-11:15)
				(args (underscore))
				(record (11:13-11:15))))))
~~~
# FORMATTED
~~~roc
app [main!] { pf: platform "../basic-cli/main.roc" }

outer : a -> a
outer = |x|
	|y| y

inner(x)


main! = |_| {}
~~~
# CANONICALIZE
~~~clojure
(can_ir
	(d_let
		(def_pattern
			(p_assign (4:1-4:6)
				(pid 77)
				(ident "outer")))
		(def_expr
			(e_lambda (4:9-6:12)
				(args
					(p_assign (4:10-4:11)
						(pid 78)
						(ident "x")))
				(e_runtime_error (6:5-6:12) "lambda_body_not_canonicalized")))
		(annotation (4:1-4:6)
			(signature 87)
			(declared_type
				(fn (3:9-3:15)
					(ty_var (3:9-3:10) "a")
					(ty_var (3:14-3:15) "a")
					"false"))))
	(d_let
		(def_pattern
			(p_assign (11:1-11:6)
				(pid 94)
				(ident "main!")))
		(def_expr
			(e_lambda (11:9-11:15)
				(args (p_underscore (11:10-11:11) (pid 95)))
				(e_runtime_error (1:1-1:1) "not_implemented")))))
~~~
# TYPES
~~~clojure
(inferred_types
	(defs
		(def "outer" 89 (type "*"))
		(def "main!" 99 (type "*")))
	(expressions
		(expr (4:9-6:12) 81 (type "*"))
		(expr (11:9-11:15) 98 (type "*"))))
~~~