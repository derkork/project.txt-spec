# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2020-07-14

# Breaking changes
* Special commands are now indicated with a caret (`^`) instead of a colon. This avoids several problems, for example URLs always contain a colon which is awkward if you have to escape it as you then cannot simply copy the URL from the text file to a browser. Also a colon is frequently used in sentences. It was a bad choice to use as a special command character. The caret is hopefully better suited for this, as it rarely occurs in normal text.
* All commands are now started with the special command character. Before the "define task" and "define person" commands were special in that they didn't require a special command character. This is now uniform so users don't need to remember when to use a special command indicator and when not.
* The special command for a person has changed from `+` to `^/` to avoid a collision with the do date command (which is `^+2020-10-10`) . Also thinking of `^/` as a waving person makes it easier to remember the command.
* Comments are now always commenting out the whole line. This avoids issues with hash characters in URLs which would otherwise have to be escaped.

# Non-breaking changes
* Renamed _instructions_ to _commands_.

## [1.0.0] - 2020-03-19
* Initial release.
