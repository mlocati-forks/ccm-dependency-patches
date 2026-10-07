[![Tests](https://github.com/concretecms/dependency-patches/actions/workflows/tests.yml/badge.svg)](https://github.com/concretecms/dependency-patches/actions/workflows/tests.yml)
# Dependency patches for concrete5 and Concrete CMS

concrete5 v8 and Concrete CMS v9+ use a lot of third party libraries, installed via Composer.

Internal changes in newer PHP versions require to upgrade some of those composer packages, but some of them are no more compatible with the PHP versions we support, or they haven't been fixed yet.

Some of those packages also have security vulnerabilities that are fixed only in versions we can't use.

This `dependency-patches` project contains those required patches, so that concrete5 and Concrete CMS can still use them.


## How to use

The official releases of concrete5 and Concrete CMS that can be downloaded from https://www.concretecms.org/download already contain the patches included in `dependency-patches`.

If you use a composer-based concrete5/Concrete CMS installation, the `composer.json` file of your project needs these settings:

```json
{
  "require": {
    "concretecms/dependency-patches": "^1"
  },
  "config": {
    "allow-plugins": {
      "mlocati/composer-patcher": true
    },
    "audit": {
      "ignore": {
        "PKSA-w9tt-7782-78jx": "league/flysystem CVE-2026-102601: fixed by concretecms/dependency-patches"
      }
    }
  },
  "extra": {
    "allow-subpatches": [
      "concretecms/dependency-patches"
    ]
  }
}
```

- `require`: needed only for concrete5 before 8.5.13 and for Concrete CMS 9.0.x (later versions already require `dependency-patches`)
- `allow-plugins`: lets Composer run the plugin that applies the patches
- `allow-subpatches`: lets that plugin apply the patches defined by `dependency-patches`
- `audit`: see [Security advisories](#security-advisories)

New patches are included only in new versions of `dependency-patches`: keep it up to date (`composer update concretecms/dependency-patches`).


## Security advisories

Some of the patches fix security vulnerabilities in packages that are no longer updated for the PHP versions we support.

Composer knows nothing about patches: it still considers the patched versions as vulnerable, so `composer audit` reports them and `composer update` may refuse to install them.

The `audit`.`ignore` setting tells Composer which advisories to ignore: it must be in the `composer.json` file of your project (it doesn't work in the `composer.json` files of dependencies).

Keep it aligned with the [`composer.json` of Concrete CMS](https://github.com/concretecms/concretecms/blob/HEAD/composer.json): the reason next to each advisory says whether it's fixed by `dependency-patches` or ignored because Concrete CMS doesn't use the affected feature (for example the Twig sandbox). In the second case, ignore it only if your project doesn't use that feature either.

The patches are applied only to the package versions listed in the `composer.json` of this repository: don't ignore these advisories if you install other versions of those packages.


## How to add a new patch

See [CONTRIBUTING.md](CONTRIBUTING.md).
