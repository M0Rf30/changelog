# Changelog

bin util to help managing a changelog

__wip__

# Install

```sh
mkdir -p $GOPATH/github.com/mh-cbon
cd $GOPATH/github.com/mh-cbon
git clone https://github.com/mh-cbon/changelog.git
cd changelog
glide install
go install
```

# Usage

The workflow would be so,

- make a new repo
- commit your stuff
- run `changelog init` to generate a `change.log` file
- before releasing run `changelog prepare`, it generates an `UNRELEASED` version
- review and edit the new `UNRELEASED` version changes in `change.log` file
- run `changelog finalize --version=x.x.x` to rename `UNRELEASED` version to its release version
- run `changelog md --out=CHANGELOG.md` to generate the new markdowned changelog file
- run `changelog deb --only=x.x.x` to get a the new version changelog and copy it somewhere else. tbd.

### intermediary changelog file

To work `changelog` uses an intermediary `changelog` file.

#### General overview

A `change.log` file contains a list `version` and their changes.

```
0.9.12-1

  * Initial release (Closes: #nnnn)
  * This is my first Debian package.

  - mh-cbon <mh-cbon@users.noreply.github.com>

-- Josip Rodin <joy-mg@debian.org>; Mon, 22 Mar 2010 00:37:31 +0100



0.9.12-0; distribution=unstable; urgency=low

  * Initial release (Closes: #nnnn)
  * This is my first Debian package.

  - mh-cbon <mh-cbon@users.noreply.github.com>

-- Josip Rodin <joy-mg@debian.org>; Mon, 22 Mar 2010 00:37:30 +0100
```

#### Version block

Each version is a formatted block of text such as

```
semver

  * Text of change #1
  * Text of change #2
  * ...

  - contributor #1 <mail@of.contributor.com>
  - contributor #2 <mail@of.contributor.com>

-- Packager name <mail@packager.org>; Release date
```

The most minimal version would be

```
semver

-- Packager name <mail@packager.org>; Release date
```

#### Version field

Each version starts with its version value, a valid `semver` identifier.

`semver` identifier can have additional tags, in the form of `tagname=value;`

```
semver; tag1=value; tag2=value;

-- Packager name <mail@packager.org>; Release date
```

It is allowed that a version starts by a non valid `semver` identifier,
in which case, the version is always sorted in first.

This possibility is offered to handle next `UNRELEASED` version.

```
a version name; tag1=value; tag2=value;

-- Packager name <mail@packager.org>; Release date
```

#### Version changes

Version changes immediately follow the `semver` identifier.

They start with a `space` followed by a star `*`, they can be multi-line.

The format is similar to `\s+\*\s+(.+)`

```
semver

  * This is valid
            * This is valid too, but ugly
  *This is not valid
*This is not valid either
  * This is a multiline entry
continuing here
  * This is another multiline entry
    nicer to read, leading white spaces will be trimmed
  * This is another multiline entry \
    with a backslash to get ride of the EOL

-- Packager name <mail@packager.org>; Release date
```

#### Version contributors

Version `contributors` immediately follow the list of `changes`.

They start with a `space` followed by an hyphen `-`.

The format is similar to `\s\+-\s+(.+)`.

It is not required to provide them
in the form of `name <email>`, but it is recommended.


```
semver

  - contributor #1 <mail@of.contributor.com>
  - contributor #2 <mail@of.contributor.com>

-- Packager name <mail@packager.org>; Release date
```

#### Version ender

Versions must end with a trailing line starting by a double hyphen `--`.

The trailing line provides `package author` and `release date` separated by a semicolon `;`

```
-- Packager name <mail@packager.org>; Release date
```

## CLI Usage

```sh
NAME:
   changelog - Changelog helper

USAGE:
   changelog <cmd> <options>

VERSION:
   0.0.0

COMMANDS:
     init      Initialize a new changelog file
     prepare   Prepare next changelog
     finalize  Take pending next changelog, apply a version on it
     test      Test to load your changelog file and report for errors or success
     export    Export the changelog using given template
     md        Export the changelog to Markdown
     help, h   Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --help, -h     show help
   --version, -v  print the version
```

#### Init

```sh
NAME:
   changelog init - Initialize a new changelog file

USAGE:
   changelog init [command options] [arguments...]

OPTIONS:
   --author value, -a value  Package author (default: "N/A")
   --email value, -e value   Package author email
   --since value, -s value   Since which tag should the changelog be generated
```

#### Prepare

```sh
NAME:
   changelog prepare - Prepare next changelog

USAGE:
   changelog prepare [command options] [arguments...]

OPTIONS:
   --author value, -a value  Package author
   --email value, -e value   Package author email
```

#### Finalize

```sh
NAME:
   changelog finalize - Take pending next changelog, apply a version on it

USAGE:
   changelog finalize [command options] [arguments...]

OPTIONS:
   --version value  Version revision
```

#### Export

```sh
NAME:
   changelog export - Export the changelog using given template

USAGE:
   changelog export [command options] [arguments...]

OPTIONS:
   --template value, -t value  Go template
   --version value             Only given version
   --out value, -o value       Out target (default: "-")
```

#### Md

```sh
NAME:
   changelog md - Export the changelog to Markdown

USAGE:
   changelog md [command options] [arguments...]

OPTIONS:
   --version value        Only given version
   --out value, -o value  Out target (default: "-")
```

#### Enable debug messages

To enable debug messages, just set `VERBOSE=change*`, `VERBOSE=*` before running the command.

```sh
VERBOSE=* changelog init
```

# Changelog

- 0.0.0: init
