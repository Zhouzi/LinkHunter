# linkhunter

Detect links that real users actually type.

linkhunter is focussed on matching links from user inputs. To do so, it considers three type of links: emails (someone@domain.com), urls with a protocol (http://site.com/) and finally user-typed url (site.com). Matching the last is particularly hard since almost everything could actually be an url. To do it properly, linkhunter does two things: ensure it starts by a valid domain name and stop on punctuation mark. It means that linkhunter is able to match:

* `http://twitter.com/` in `Have a look at http://twitter.com/`
* `github.com` in `Feel free to submit an issue on github.com!`
* `me@domain.com` in `Contact me@domain.com`

The library is also made to avoid matching partial urls. It means that `github.com/angular.js` won't be matched at all because there's no way to be sure that the dot before "js" is part of the url and not a punctuation mark.

*Note that linkhunter is meant to extract links from user's input but not to strictly validate urls or emails.
For example, `user@domain` is considered to be a valid email per the specifications but not by linkhunter.*

* [Usage](https://github.com/Zhouzi/linkhunter#usage)
* [Documentation](https://github.com/Zhouzi/linkhunter/wiki)
* [Known "Limitations"](https://github.com/Zhouzi/linkhunter#known-limitations)
* [Contributing](https://github.com/Zhouzi/linkhunter/blob/gh-pages/CONTRIBUTING.md)
* [Change Log](https://github.com/Zhouzi/linkhunter#change-log)



## Usage

1. Include the linkhunter.min.js file by whether:
  * Downloading the distributed file: [linkhunter.min.js](https://raw.githubusercontent.com/Zhouzi/linkhunter/master/dist/linkhunter.min.js)
  * Installing via bower: `bower install linkhunter`
2. Link it in your markup: `<script src="path/to/linkhunter.min.js"></script>`



## Known "Limitations"

* Domains such as Twitter's `t.co` are matched, meaning `a.bc` and `q.we` too.
* Match user typed urls if followed or not by a punctuation mark and a space so `github.com` is matched in `github.com!` and `github.com! Some text...` but not in `github.com!Some text...`.
* User typed urls can't contain a punctuation mark after the first slash. It means that `github.com` is properly matched in `Go to github.com! And let me know` (and not `github.com!`). It also means that `github.com/angular.js` is not considered as a valid "user typed" url because we can't be sure whether what's behind the punctuation mark (the dot in this case) is part of the url or not. Adding the protocol fix it so `http://github.com/angular.js` is perfectly valid.



## Change log

### 3.0.0 - Unreleased

* [x] The usage of linkhunter as a constructor currently makes no sense and just add an useless step. The api should be directly available.
    * [x] Change linkhunter to be an object instead of a Class.
    * [x] Rename the files and update imports.
    * [x] Update specs.
    * [x] Change the lib's name to be `linkhunter` instead of `LinkHunter`.
    * [x] Update documentation.
* [x] Transforming links into Link objects makes it hard, if not impossible, to chain operations (e.g. beautify+shorten). Moving those methods to linkhunter would make it really easy and decrease the overall complexity.
    * [x] Move LinkClass' methods to linkhunter.
    * [x] Remove useless options (e.g. forceCleanUp).
    * [x] Update specs.
    * [x] Update documentation.
* [x] Simplify the `operation` option of `linkhunter.linky()` by adding: `{ withProtocol: false, beautify: false, shorten: false, cleanUp: false }`.
    * [x] Add tests.
    * [x] Update documentation.
* [x] Update the angular wrapper.
* [x] Update demo page.
* [ ] Add support for non-browser environment.

### 2.0.1 - 2015-07-04

* Update README.md on usage.
* Update bower's ignore.

### 2.0.0 - 2015-07-04

* `mailto:` is now considered to be emails' protocol, meaning `Link('email@domain.com').withProtocol()` now returns `mailto:email@domain.com`.
* The `targetBlank` option of `.linky()` is replaced by `target` to allow any target value.
* Improved the email matching.
 * It's now using the "domain pattern" regexp and match properly `firstname.lastname@email.address-site.com`.
 * Added more tests to the specs.
* `.getLinks()` now ignore emails by default.
* `.looksLikeALink()` now ignore emails by default.
* Added the `forceCleanUp` option to `.shorten()` (true by default).
* linkhunter is now available as a bower component: `bower install linkhunter`.

### 1.1.0 - 2015-06-30

* Lots of improvements in the regexp
    * Regular urls (the ones with a protocol) now also benefit from the domain pattern. Which means that everything that's before the first slash must looks like a domain name so `http://.` is no more matched for example.
    * Partial urls are no more matched by the "user typed" regexp. Meaning `github.com/angular` is no more matched from `github.com/angular.js`. In such case the url is completely dropped because:
        1. Matching partial urls is pointless and would surely lead to mistakes.
        2. We can't be sure whether what's behind the punctuation mark (the dot in this case) is part of the url or not.
* Added a `.replaceLinks(callback)` method so your could benefit from linkhunter's regexps to replace links the way you want to. This is method is now also used by `.linky()`.
* `.linky()` now supports more options like beautify, shorten and clean up.
* Every link object has now a `.originalHasProtocol` property which is true when the original url has a protocol.
* linkhunter is now also available as an Angular module.
* Fixed an issue where `.getLinks()` wasn't setting emails' type to "email".

### 1.0.0 - 2015-06-28

First release.
