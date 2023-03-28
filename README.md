# Inko-argparse

This is an argument parsing library for the fascinating [Inko](https://inko-lang.org)
programming language. I've been experimenting with Inko for a few weeks and have really
enjoyed how simple and approachable it is.

The Inko standard library is pretty bare-bones, and is currently missing an argument
parsing solution. To help study the language, I decided to implement one.

The library is pretty basic and doesn't currently support subcommands/subparsers.
It supports most of the standard stuff, though:

- positional arguments
- optional arguments with -- and -
- autogenerating a help text from the arguments
- parsing boolean arguments
- parsing integer arguments
- parsing string arguments

## Installing

Inko (sort of) ships with a package manager named [ipm](https://docs.inko-lang.org/manual/main/getting-started/modules/#using-ipm)
but the feature is so new that it isn't available for the latest release of Inko.
So you'll need to [install Inko](https://docs.inko-lang.org/manual/main/getting-started/installation/#from-source)
from the [main](https://github.com/inko-lang/inko) branch.

Note that this means you're using a broken Inko compiler. So... good luck.

Once you have `ipm` on your path, simply run the following to install argparse:

```sh
ipm init # if you don't already have an inko.pkg
ipm add github.com/dusty-phillips/inko-argparse 0.0.1`
ipm sync
```

(check the tags on this repo to determine the latest tag in case I forget to update the README)

## Usage Example

```inko
import argparse::(ArgumentParser, ArgumentType)
import std::stdio::STDOUT


class async Main {
  fn async main {
    let stdout = STDOUT.new
    let parser = ArgumentParser.new("A Program for testing ArgumentParser")
    parser
      .add_argument("first_arg", ArgumentType.String, "The first arg in the command")
      .add_argument("int_arg", ArgumentType.Int, "The second argument in the command")
      .add_option("optional_arg", Option.Some("o"), ArgumentType.String, "An option you may or may not need")
      .add_option("bool_arg", Option.None, ArgumentType.Bool, "something you can pass or not")

    let parsed = try parser.parse_cli()
     else (message) {
      stdout.print("\n{message}\n\n{parser.help}")
      return
    }

    stdout.print(parsed.int_argument("int_arg").to_string)
    stdout.print(parsed.string_option("optional_arg").unwrap)
  }
}
```

now you can run e.g `inko run src/main.inko arg1 42 -o something --bool_arg`

See the [source code](blob/main/src/argparse.inko) for documentation on the various ArgumentParser methods.
(Inko doesn't have a documentation generator yet)
