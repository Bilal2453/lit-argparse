# argparse

[![Build Status](https://travis-ci.org/luarocks/argparse.png?branch=master)](https://travis-ci.org/luarocks/argparse)
[![Coverage status](https://codecov.io/gh/luarocks/argparse/branch/master/graph/badge.svg)](https://codecov.io/gh/luarocks/argparse)

Argparse is a feature-rich command line parser for Lua inspired by argparse for Python.

Argparse supports positional arguments, options, flags, optional arguments, subcommands and more. Argparse automatically generates usage, help, and error messages, and can generate shell completion scripts.

## lit-argparse

This fork is a repackaging of the original work into the [Lit](https://github.com/luvit/lit) package manager.
The master branch was repurposed to host the final packaged files, for easier integration with Lit's git support.

An important modification was applied to add support for Luvi's 0-based `args` so you can do `Parser:parse()` without having to pass anything when using Luvit/Luvi. Previously you had to manually create an equivalent to `arg`.

## Contents

* [Example](#example)
* [Installation](#installation)
* [Tutorial](#tutorial)
* [Testing](#testing)
* [License](#license)

## Example

Simple example:

```lua
-- script.lua
local argparse = require "argparse"

local parser = argparse("script", "An example.")
parser:argument("input", "Input file.")
parser:option("-o --output", "Output file.", "a.out")
parser:option("-I --include", "Include locations."):count("*")

local args = parser:parse()
```

`args` contents depending on command line arguments:

```bash
$ luvit script.lua foo
```

```lua
{
   input = "foo",
   output = "a.out",
   include = {}
}
```

```bash
$ luvit script.lua foo -I/usr/local/include -Isrc -o bar
```

```lua
{
   input = "foo",
   output = "bar",
   include = {"/usr/local/include", "src"}
}
```

Error messages depending on command line arguments:

```bash
$ luvit script.lua foo bar
```

```
Usage: script [-h] [-o <output>] [-I <include>] <input>

Error: too many arguments
```

```bash
$ luvit script.lua --help
```

```
Usage: script [-h] [-o <output>] [-I <include>] <input>

An example. 

Arguments: 
   input                 Input file.

Options: 
   -h, --help            Show this help message and exit.
   -o <output>, --output <output>
                         Output file. (default: a.out)
   -I <include>, --include <include>
                         Include locations.
```

```bash
$ lua script.lua foo --outptu=bar
```

```
Usage: script [-h] [-o <output>] [-I <include>] <input>

Error: unknown option '--outptu'
Did you mean '--output'?
```

## Installation

### Using Lit

```bash
$ lit install Bilal2453/argparse
```

Alternatively, if you are using Lit with the git implementation you may use:

```bash
$ lit install https://github.com/Bilal2453/lit-argparse.git
```

### Without Lit

Download `argparse.lua` file and put it into the `deps` directory.

## Tutorial

The tutorial is available [online](http://argparse.readthedocs.org). If argparse has been installed using LuaRocks 2.1.2 or later, it can be viewed using `luarocks doc argparse` command.

Tutorial HTML files can be built using [Sphinx](http://sphinx-doc.org/): `sphinx-build docsrc doc`, the files will be found inside `doc/`.

## Testing

argparse comes with a testing suite located in `spec` directory. [busted](http://olivinelabs.com/busted/) is required for testing, it can be installed using LuaRocks. Run the tests using `busted` command from the argparse folder.

## License

argparse is licensed under the same terms as Lua itself (MIT license).
