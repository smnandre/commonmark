# Table of contents

Table of Contents replaces an `@toc` marker with a linked list built from the
document headings. With League's core renderer, also register Heading Permalink
as shown below so those links have rendered HTML targets.

## Install and register

```bash
composer require alto/commonmark-table-of-contents
```

The same class is included in `alto/commonmark`.

```php
use Alto\CommonMark\Extension\TableOfContents\TableOfContentsExtension;
use League\CommonMark\Environment\Environment;
use League\CommonMark\Extension\HeadingPermalink\HeadingPermalinkExtension;
use League\CommonMark\Extension\CommonMark\CommonMarkCoreExtension;

$environment = new Environment([
    'heading_permalink' => [
        'id_prefix' => '',
        'apply_id_to_heading' => true,
        'insert' => 'none',
    ],
]);
$environment->addExtension(new CommonMarkCoreExtension());
$environment->addExtension(new HeadingPermalinkExtension());
$environment->addExtension(new TableOfContentsExtension());
```

## Add a table of contents

```markdown
# Guide

@toc {min: 2, max: 3}

## Install

### Requirements

## Usage
```

`min` and `max` filter heading levels for that marker. Use
`ordered: true` to render an ordered list. The directive can have up to three
leading spaces.

Multiple markers in one document each receive their own filtered heading
list.

## Configure defaults

```php
new TableOfContentsExtension([
    'min_level' => 2,
    'max_level' => 4,
    'style' => 'ordered',
    'class' => 'toc-nav',
    'id' => 'main-toc',
    'title' => 'Contents',
    'marker' => '@contents',
]);
```

- `style` accepts `bullet` or `ordered`.
- `class` and `id` configure the wrapper.
- `title` adds an `h2` before the list.
- `marker` replaces the default `@toc` marker.

## Heading targets and limits

The current ALTO extension calculates TOC fragments but does not attach HTML
`id` attributes recognized by League's core heading renderer. The registration
above supplies those attributes through League's bundled Heading Permalink
extension, with no prefix or extra visible permalink.

This composition works for unique plain ASCII headings such as `Install` and
`Usage`. ALTO's TOC slugging does not disambiguate duplicates and can differ
from League's normalization for non-ASCII text, punctuation, or inline markup.
Check rendered fragment links for your headings; do not assume this setup
solves those cases. These are current limits, not a general anchor contract.

See [Heading level](heading-level.md) when levels must be transformed and
[Content slicer](content-slicer.md) when headings must also create sections.
