# About
This repo is my attempt at making a JSON library for the [Grain](https://grain-lang.org) programming language. The objective is to merge it into Grain's standard library.

# API structure
I think that it makes sense to design the API very explicitly so it could be used as basic blocks for higher level libraries, but not on a too low level either. So more like a sort of DOM representation, not a pull/event parser and not an auto-magic "object binding" kind of library either. Hopefully it could serve as a building block for more sophisticated libraries.

# Building
You need to have Grain already installed and on the path. Then just launch `grain ./json.test.gr` to run tests.

# Resources
- [The JSON spec](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)

- When you think you've done a good job optimizing the code, look at [this](https://www.infoq.com/presentations/simdjson-parser/) and think again. Not that I'd go into this crazy level of optimization. Just leaving a note here for inspiration.

# Credits
Parsing code loosely based on one written by [jozanza](https://github.com/jozanza) and provided on grain's Discord.
