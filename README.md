
[![Unix Build Status][ci-img]][ci-url]
[![Windows Build Status][ci-win-img]][ci-win-url]
[![Code Climate][clim-img]][clim-url]
[![NPM][npm-img]][npm-url]

# haraka-plugin-domain-aliases

This plugin is derived from "aliases" plugin: https://github.com/haraka/haraka-plugin-aliases

Every functionality of haraka-plugin-aliases is available in haraka-plugin-domain-aliases. Additionally, with this plugin, it is possible to convey all the incoming e-mails for a domain to another domain.

& IMPORTANT: this plugin must appear in  `config/plugins`  before other plugins that run on hook_rcpt

& WARNING: DO NOT USE THIS PLUGIN WITH queue/smtp_proxy.

## Configuration
#### Available actions:
- drop
- alias
	- to (required)
- domain-alias
	- to (required)

### Aliases
JSON file inside the config folder must be filled with pre-determined types of "actions". Example:
```
{ "test1" : { "action" : "drop" } }
```

Aliases of 'user', '@host' and 'user@host' possible:
```
{ "demo" : { "action" : "drop" } }

{ "@example.com" : { "action": "alias", "to": ["test@domain.com"] } }

{ "demo@example.com" : { "alias", "to": ["haraka@example.com"] } }
```

For more information, please check: https://github.com/haraka/haraka-plugin-aliases.

### Domain Aliases
Maps the alias domain to the domain specified in the "to" option.
Both the original alias and the recipient (target) alias should only consist of host, in the form of @host. Example:
```
{ "@haraka.test" : { "action" : "domain-alias", "to" : "@domain.com" } }
```
This configuration will forward any e-mail sent to user@haraka.test to user@domain.com.

#### Alias matching order
Larger and more specific aliases match first and "alias" action has precedence over "domain-alias".
```
{
 "user@haraka.test" : { "action" : "alias", "to" : "user2@domain.com" },
 "@haraka.test" : { "action" : "domain-alias", "to" : "@example.com" }
}
or
{
 "@haraka.test" : { "action" : "domain-alias", "to" : "@example.com" },
 "user@haraka.test" : { "action" : "alias", "to" : "user2@domain.com" }
}
```
Both configurations above will work the same. If the recipient's e-mail address  of the sent mail is user@haraka.test, the mail will be forwarded to user2@domain.com. However, if the address is non.user@haraka.test, it will be forwarded to non.user@example.com.



### Example Configuration
```
{
    "test1" : { "action" : "drop" },
    "test2" : { "action" : "alias", "to" : "non-test2" },
    "test3" : { "action" : "alias", "to" : "test3" },
    "test4" : { "action" : "alias", "to" : "non-test4@domain.com" },
    "test5@domain.com" : { "action" : "alias", "to" : "non-test5@success.com" }
    "test6@domain.com" : { "action" : "alias", "to" : "test6@success.com" },
    "@domain.com" : { "action" : "domain-alias", "to" : "@example.com" }
}
```

<!-- leave these buried at the bottom of the document -->
[ci-img]: https://github.com/haraka/haraka-plugin-domain-aliases/workflows/Plugin%20Tests/badge.svg
[ci-url]: https://github.com/haraka/haraka-plugin-domain-aliases/actions?query=workflow%3A%22Plugin+Tests%22
[ci-win-img]: https://github.com/haraka/haraka-plugin-domain-aliases/workflows/Plugin%20Tests%20-%20Windows/badge.svg
[ci-win-url]: https://github.com/haraka/haraka-plugin-domain-aliases/actions?query=workflow%3A%22Plugin+Tests+-+Windows%22
[clim-img]: https://codeclimate.com/github/haraka/haraka-plugin-domain-aliases/badges/gpa.svg
[clim-url]: https://codeclimate.com/github/haraka/haraka-plugin-domain-aliases
[npm-img]: https://nodei.co/npm/haraka-plugin-domain-aliases.png
[npm-url]: https://www.npmjs.com/package/haraka-plugin-domain-aliases
