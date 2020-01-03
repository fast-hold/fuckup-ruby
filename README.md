# use-ruby

This action downloads a prebuilt ruby and adds it to `$PATH`.

It currently supports the latest stable versions of MRI, JRuby and TruffleRuby.

See https://github.com/eregon/ruby-install-builder/blob/metadata/versions.json
for the available Ruby versions.

The action works for the `ubuntu-16.04`, `ubuntu-18.04` and `macos-latest` GitHub-hosted runners.  
`windows-latest` is not yet supported.

The prebuilt rubies are generated by https://github.com/eregon/ruby-install-builder.

## Usage

### Single Job

```yaml
name: My workflow
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: eregon/use-ruby-action@master
      with:
        ruby-version: ruby-2.6
    - run: ruby -v
```

### Matrix

```yaml
name: My workflow
on: [push]
jobs:
  test:
    strategy:
      fail-fast: false
      matrix:
        os: [ ubuntu-latest, macos-latest ]
        ruby: [ 'ruby-2.6', 'ruby-2.7', 'truffleruby-19.3.0', 'jruby-9.2.9.0' ]
    runs-on: ${{ matrix.os }}
    steps:
    - uses: actions/checkout@v2
    - uses: eregon/use-ruby-action@master
      with:
        ruby-version: ${{ matrix.ruby }}
    - run: ruby -v
```

### Supported Version Syntax

* engine-version like `ruby-2.6.5` and `truffleruby-19.3.0`
* short version like `2.6`, automatically using the latest release matching that version (`2.6.5`)
* version only like `2.6.5`, assumes MRI for the engine
* engine only like `truffleruby`, uses the latest stable release of that implementation

### All Stable Versions

With that, we can test on all stable releases of MRI, JRuby and TruffleRuby with:

```yaml
name: My workflow
on: [push]
jobs:
  test:
    strategy:
      fail-fast: false
      matrix:
        os: [ ubuntu-latest, macos-latest ]
        ruby: [ '2.4', '2.5', '2.6', '2.7', 'truffleruby', 'jruby' ]
    runs-on: ${{ matrix.os }}
    steps:
    - uses: actions/checkout@v2
    - uses: eregon/use-ruby-action@master
      with:
        ruby-version: ${{ matrix.ruby }}
    - run: ruby -v
```

## Efficiency

It takes about 5 seconds to setup the given Ruby.

## Limitations

* Currently does not work on Windows since the builder doesn't build on Windows.
  https://github.com/MSP-Greg/actions-ruby is an alternative on Windows.
* This action currently only works with GitHub-hosted runners, not private runners.
