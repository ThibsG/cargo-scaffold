# Cargo-scaffold

`cargo-scaffold` is a flexible and easy-to-use developper tool to let you scaffold a project. It's fully configurable without writing any line of code. It generates any kind of projects with a developer friendly CLI.

<p align="center"><img src="./logo.svg?raw=true" style="width: 35%; height: 35%;" /></p>

## Features

- Scaffold a project in seconds
- Declarative
- User interactions automatically generated
- Not only for Rust crate/project. It's completely language agnostic

<p align="center"><img src="./preview.gif?raw=true"/></p>

## Installation

```
cargo install cargo-scaffold
```

## Usage

You can scaffold your project from any `cargo-template` scaffold located locally in a directory or in a git repository

```
# Locally
cargo scaffold your_template_dir

# From git repository
cargo scaffold https://github.com/username/template.git
```

Here are the available options for `cargo scaffold`:

```
USAGE:
    cargo-scaffold scaffold [FLAGS] [OPTIONS] <template>

FLAGS:
    -a, --append        Append files in the existing directory, do not create directory with the project name
    -f, --force         Override target directory if it exists
    -h, --help          Prints help information
    -p, --passphrase    Specify if your SSH key is protected by a passphrase
    -V, --version       Prints version information

OPTIONS:
    -n, --name <name>
            Specify the name of your generated project (and so skip the prompt asking for it)

    -d, --target-directory <target-directory>    Specifiy the target directory

ARGS:
    <template>    Specifiy your template location
```

## Write your own template

To let you scaffold and generate different projects the only mandatory part is to have a `.scaffold.toml` file at the root of the template directory. This file is used to document and add user interactions for your template. In your template's directory each files and directories will be copy/pasted to your generated project but updated using [Handlebars templating](https://handlebarsjs.com/).

### Template description

Here is an example of `.scaffold.toml` file:

```toml
# Basic template informations
[template]
name = "test"
author = "Benjamin Coenen <5719034+bnjjj@users.noreply.github.com>"
version = "0.1.0"

# Exclude paths you do not want copy/pasted in the generated project
exclude = [
    "./target"
]

# Notes to display at the end of the generation
notes = """
Have fun using this template called {{name}} ! Here is the description: {{description}}
"""

# Parameters are basically all the variables needed to generate your template using templating.
# It will be displayed as prompt to interact with user (thanks to the message subfield).
# All the parameters will be available in your templates as variables (example: `{{description}}`).
[parameters]
    # [parameters.name] is already reserved
    [parameters.feature]
    type = "string"
    message = "What is the name of your feature ?"
    required = true

    [parameters.gender]
    type = "select"
    message = "Which kind of API do you want to scaffold ?"
    values = ["REST", "graphql"]

    [parameters.dependencies]
    type = "multiselect"
    message = "Which dependencies do you want to use ?"
    values = ["serde", "anyhow", "regex", "rand", "tokio"]

    [parameters.description]
    type = "string"
    message = "What is the description of your feature ?"
    default = "Here is my default description"

    [parameters.show_description]
    type = "boolean"
    message = "Do you want to display the description ?"

    [parameters.limit]
    type = "integer"
    message = "What is the limit ?"
```

Here is the list of different types you can use for your parameter: `string`, `integer`, `float`, `boolean`, `select`, `multiselect`.

### Templating

In any files inside your template's directory you can use [Handlebars templating](https://handlebarsjs.com/guide/). Please refer to that documentation for all the syntax about templating. If you're looking for custom helpers in Handlerbars you can check the [documentation here](https://github.com/davidB/handlebars_misc_helpers). Here is a basic example if you want to display the parameter named `description` and if the boolean parameter `show_description` is set to `true` as described in the previous section.

```
{{#if show_description}} {{description}} {{/if}}
{{#forRange 5}}
Repeat this line 5 times with the current {{@index}}{{/forRange}}
```

> You can also put templating in path for directory or filename into your template (example: a file called `{{name}}.rs` would be generated with the right name).

## Credits

Thanks [@Arlune](https://github.com/Arlune) for this awesome logo and all reviewers.

## Alternatives

- [ffizer](https://github.com/ffizer/ffizer)
