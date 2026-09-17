# Contributing

This repository maintains extensions for League CommonMark, including the
umbrella package and the source for the split packages. Use PHP 8.3 or later
and Composer. Run `composer install`, then:

```sh
php vendor/bin/php-cs-fixer fix --dry-run --diff --sequential
composer sa
composer test
```

`composer cs` applies formatting; use dry-run for verification. Test extension
registration in a League `Environment`, complete HTML output, and composition
with the core extension. For filesystem extensions, use a temporary trusted
root and cover missing files, traversal, and configured limits.

Keep package names and replace declarations aligned with `composer.json` and
the split-package manifests. Update the relevant [extension guide](docs/extensions/index.md)
and changelog for public changes. Browser interaction in Tabs needs rendered
verification in addition to checking emitted HTML and ARIA relationships.
