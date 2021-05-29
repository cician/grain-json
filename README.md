# About
This repo is my attempt at making a JSON library for the [Grain](https://grain-lang.org) programming language.

# API structure
I think that it makes sense to design the API very explicitly so it could be used as basic blocks for higher level libraries, but not on a too low level either. So more like a sort of DOM representation, not a pull/event parser and not an auto-magic "object binding" kind of library either. Hopefully it could serve as a building block for more sophisticated libraries.

# Building
For now you need to have Grain already installed and on the path. Then just launch `grain ./test.gr`.

# Resources
- [The JSON spec](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)

# TODO
- [ ] Formatting
	- [x] Bools
	- [ ] Numbers
		For now just using grain's toString of Float64, which works,
		but may not be a good idea.
	- [ ] String escaping
		- Unicode hex sequence escaping
		- Simplify and/or optimize
	- [x] Arrays
	- [ ] Objects
		I need an adequate data structure. For now I could make one from a combination of a Map and a List or just accept a Map and live with semi-random property ordering, but even there I'm blocked by this bug: https://github.com/grain-lang/grain/issues/665.
	- [ ] toString
		For now I'm just doing only printing to the standard output.
		Need a string buffer or something similar.
- [ ] Parsing
	TBD