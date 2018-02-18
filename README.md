[![Build Status](https://travis-ci.org/nh13/snakeparse.svg?branch=master)](https://travis-ci.org/nh13/snakeparse)
[![codecov](https://codecov.io/gh/nh13/snakeparse/branch/master/graph/badge.svg)](https://codecov.io/gh/nh13/snakeparse)
[![License](http://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/nh13/snakeparse/blob/master/LICENSE)
[![Language](http://img.shields.io/badge/language-python-brightgreen.svg)](http://www.python.org/)

Snakeparse
====

Making [Snakemake](https://bitbucket.org/snakemake/snakemake) workflows into full-fledged command line tools since 1999.

***Warning: this project is under active development; the API will change!***

## Why Snakeparse?

- I wanted a single command line utility that could organize and execute multiple Snakemake workflows
- I wanted to have my workflow-specific arguments be parsed on the command line (ex. with [argparse](https://docs.python.org/3/library/argparse.html))
- I wanted an API or library to configure how to group and organize the workflows, and how to display them on the command line.
- I didn't want to write this library more than once.

## Demo

![Snakeparse Demo](https://github.com/nh13/snakeparse/blob/master/examples/snakeparse-demo.gif)

## Installation

Python 3.6 or higher is required.

Snakemake 4.4.0 or higher is required.

To clone the repository: `git clone https://github.com/nh13/snakeparse.git`.

To install locally: `python setup.py install`.

The command line utility can be run with `snakeparse`

## Conda Recipe

See the [conda-recipe](https://github.com/nh13/snakeparse/tree/conda-recipe) branch.

## API Documentation

Coming soon!

See the [source API documentation](https://github.com/nh13/snakeparse/blob/master/src/snakeparse/api.py) in the meantime.

## Testing

To run tests, run `coverage run -m unittest discover -s src`

To obtain test coverage, run `codecov`.

## Snakeparse on the Command Line

To get started, run `snakeparse --help`.

More documentation is coming soon, but see the [source API documentation](https://github.com/nh13/snakeparse/blob/master/src/snakeparse/api.py) in the meantime.

## Example

1. [Using `argparse` and a custom method named snakeparser](https://github.com/nh13/snakeparse/blob/master/examples/argparse/method/README.md).
2. [Using `argparse` and a concrete sub-class of SnakeParser](https://github.com/nh13/snakeparse/blob/master/examples/argparse/class/README.md).

For more examples, see [this link](https://github.com/nh13/snakeparse/blob/master/examples/).

The example below is from (1).

### Setup

Add the following to your Snakemake file (snakefile):

[`Source: examples/argparse/method/write_message.smk`](https://github.com/nh13/snakeparse/blob/master/examples/argparse/method/write_message.smk)

```python
# Import your custom parser
from write_message_snakeparser import snakeparser

# Get the arguments from the config file; this should always succeed.
args = snakeparser().parse_config(config=config)

rule all:
    input:
        'message.txt'

# A simple rule to write the message to the output
rule message:
    output: 'message.txt'
    shell: 'echo {args.message} > {output}'
```

### Execution

#### Snakeparse Command Line Execution

You can run the installed `snakeparse` utility as follows:

```snakeparse --snakefile examples/argparse/method/write_message.smk -- --message 'Hello World!'```

or

```snakeparse --snakefile-globs examples/argparse/method/*smk -- WriteMessage --message 'Hello World!'```

#### Programmatic Execution

```python
config = SnakeParseConfig(snakefile_globs='~/examples/argparse/method/*smk')
SnakeParse(args=sys.argv[1:], config=config).run()
```

or alternatively `SnakeParse` accepts leading configuration arguments:

```python
args = ['--snakefile-globs', '~/examples/argparse/method/*smk'] + sys.argv[1:]
SnakeParse(args=args, config=config).run()
```
