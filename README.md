<h1>Nuonic API documentation repo</h1>
<p>This documentation is based on the awesome <a href="https://lord.github.io/slate">Slate</a> project.

Getting started
------------------------------

### Prerequisites

You're going to need:

 - **Linux or OS X** — Windows may work, but is unsupported.
 - **Ruby, version 2.0 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

1. Clone this repo to your local machine
2. `cd apidocs`
3. Initialize and start the local server:

```shell
bundle install
bundle exec middleman server
```

You can now see the docs at http://localhost:4567. Whoa! That was fast!

### Deploying changes

1. Merge your branch into `master`
2. `./deploy`
