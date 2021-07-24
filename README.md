# About
This repo is my attempt at making a JSON library for the [Grain](https://grain-lang.org) programming language.

# API structure
I think that it makes sense to design the API very explicitly so it could be used as basic blocks for higher level libraries, but not on a too low level either. So more like a sort of DOM representation, not a pull/event parser and not an auto-magic "object binding" kind of library either. Hopefully it could serve as a building block for more sophisticated libraries.

# Building
For now you need to have Grain already installed and on the path. Then just launch `grain ./test.gr`.

# Resources
- [The JSON spec](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)

- When you think you've done a good job optimizing the code, look at [this](https://www.infoq.com/presentations/simdjson-parser/) and think again. Not that I'd go into this crazy level of optimization. Just leaving a note here for inspiration.

# TODO
- [ ] Formatting
	- [x] Bools
	- [x] Numbers
	- [x] String escaping
		- [x] Unicode hex sequence escaping
	- [x] Arrays
	- [x] Objects
	- [x] toString
	- [ ] Simplify and optimize
		Having parsing working before attempting these optimizations would be nice
		because it would allow for easier benchmarking.
		- [ ] Switch to using Buffer instead of custom StringWriter solution.
		    Currently waiting for it to be merged into Grain's stdlib.
			Whether to keep a StringWriter module at all still TBD.
		- [ ] Maybe reduce allocations.
			Candidates:
			- [x] emitUTF16EscapeSequence allocates to make an intermediate hex string
			- [x] emitEscapedString always allocates an array with String.explode
				need to submit PR for codepoint iteration
			- [ ] formatted numbers are converted to intermediate string
		- [x] Maybe write unicode codepoints for constant strings like "{", "}", ":" etc.
		    The benefit is measurable but small.
		- [x] Maybe avoid Option allocation inside the recursion
			or is Option packed into the pointer like simple numbers?
			Not an issue since variants without data get reused.
- [ ] Parsing
	- [x] Nulls
	- [x] Booleans
	- [x] Arrays
	- [x] Objects
	- [ ] String escape sequence parsing
	- [ ] Numbers
- [x] License
- [ ] Tests
- [ ] Docs

# Credits
Parsing code loosely based on one written by [jozanza](https://github.com/jozanza) and provided on grain's Discord.
