# META
~~~ini
description=Simple type variable introduction in function annotation
type=file
~~~
# SOURCE
~~~roc
app [main!] { pf: platform "../basic-cli/main.roc" }

identity : a -> a
identity = |x| x

main! = |_| {}
~~~
# PROBLEMS
**NOT IMPLEMENTED**
This feature is not yet implemented: canonicalize record expression

# TOKENS
~~~zig
KwApp(1:1-1:4),OpenSquare(1:5-1:6),LowerIdent(1:6-1:11),CloseSquare(1:11-1:12),OpenCurly(1:13-1:14),LowerIdent(1:15-1:17),OpColon(1:17-1:18),KwPlatform(1:19-1:27),StringStart(1:28-1:29),StringPart(1:29-1:50),StringEnd(1:50-1:51),CloseCurly(1:52-1:53),Newline(1:1-1:1),
Newline(1:1-1:1),
LowerIdent(3:1-3:9),OpColon(3:10-3:11),LowerIdent(3:12-3:13),OpArrow(3:14-3:16),LowerIdent(3:17-3:18),Newline(1:1-1:1),
LowerIdent(4:1-4:9),OpAssign(4:10-4:11),OpBar(4:12-4:13),LowerIdent(4:13-4:14),OpBar(4:14-4:15),LowerIdent(4:16-4:17),Newline(1:1-1:1),
Newline(1:1-1:1),
LowerIdent(6:1-6:6),OpAssign(6:7-6:8),OpBar(6:9-6:10),Underscore(6:10-6:11),OpBar(6:11-6:12),OpenCurly(6:13-6:14),CloseCurly(6:14-6:15),EndOfFile(6:15-6:15),
~~~
# PARSE
~~~clojure
(file (1:1-6:15)
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
		(type_anno (3:1-4:9)
			"identity"
			(fn (3:12-3:18)
				(ty_var (3:12-3:13) "a")
				(ty_var (3:17-3:18) "a")))
		(decl (4:1-4:17)
			(ident (4:1-4:9) "identity")
			(lambda (4:12-4:17)
				(args (ident (4:13-4:14) "x"))
				(ident (4:16-4:17) "" "x")))
		(decl (6:1-6:15)
			(ident (6:1-6:6) "main!")
			(lambda (6:9-6:15)
				(args (underscore))
				(record (6:13-6:15))))))
~~~
# FORMATTED
~~~roc
NO CHANGE
~~~
# CANONICALIZE
~~~clojure
(can_ir
	(d_let
		(def_pattern
			(p_assign (4:1-4:9)
				(pid 77)
				(ident "identity")))
		(def_expr
			(e_lambda (4:12-4:17)
				(args
					(p_assign (4:13-4:14)
						(pid 78)
						(ident "x")))
				(e_lookup_local (4:16-4:17) (pid 78))))
		(annotation (4:1-4:9)
			(signature 85)
			(declared_type
				(fn (3:12-3:18)
					(ty_var (3:12-3:13) "a")
					(ty_var (3:17-3:18) "a")
					"false"))))
	(d_let
		(def_pattern
			(p_assign (6:1-6:6)
				(pid 88)
				(ident "main!")))
		(def_expr
			(e_lambda (6:9-6:15)
				(args (p_underscore (6:10-6:11) (pid 89)))
				(e_runtime_error (1:1-1:1) "not_implemented")))))
~~~
# TYPES
~~~clojure
(inferred_types
	(defs
		(def "identity" 87 (type "*"))
		(def "main!" 93 (type "*")))
	(expressions
		(expr (4:12-4:17) 80 (type "*"))
		(expr (6:9-6:15) 92 (type "*"))))
~~~