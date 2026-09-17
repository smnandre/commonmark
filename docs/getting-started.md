# Getting started

Register only the extensions needed by the current converter. This example
adds titled code blocks and a table of contents. After [installation](installation.md),
save it as `render.php` beside `vendor/` and run `php render.php`.

````php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\CommonMark\Extension\CodeBlockTitle\CodeBlockTitleExtension;
use Alto\CommonMark\Extension\TableOfContents\TableOfContentsExtension;
use League\CommonMark\Environment\Environment;
use League\CommonMark\Extension\HeadingPermalink\HeadingPermalinkExtension;
use League\CommonMark\Extension\CommonMark\CommonMarkCoreExtension;
use League\CommonMark\MarkdownConverter;

$environment = new Environment([
    'heading_permalink' => [
        'id_prefix' => '',
        'apply_id_to_heading' => true,
        'insert' => 'none',
    ],
]);
$environment->addExtension(new CommonMarkCoreExtension());
$environment->addExtension(new HeadingPermalinkExtension());
$environment->addExtension(new CodeBlockTitleExtension());
$environment->addExtension(new TableOfContentsExtension([
    'min_level' => 1,
]));

$converter = new MarkdownConverter($environment);

$markdown = <<<'MARKDOWN'
# Guide

@toc

## Install

```bash title="Terminal"
composer require alto/commonmark
```
MARKDOWN;

echo $converter->convert($markdown);
````

League's Heading Permalink extension adds the heading IDs. ALTO replaces
`@toc` with a linked list and wraps the code block in a captioned figure.
Use unique plain ASCII headings for this composition; see the current
[TOC limits](extensions/table-of-contents.md#heading-targets-and-limits).

The complete rendered HTML is:

```html
<h1 id="guide">Guide</h1>
<div class="table-of-contents" id="toc"><ul><li><a href="#guide">Guide</a></li><li><ul><li><a href="#install">Install</a></li></ul></li></ul></div>
<h2 id="install">Install</h2>
<figure class="code-block has-title" data-title="Terminal"><figcaption class="code-title">Terminal</figcaption><pre><code class="language-bash">composer require alto/commonmark
</code></pre></figure>
```

## Choose extensions

Use [All extensions](extensions/index.md) to select extensions by task. Each
detail page documents registration, input syntax, configuration, and output.

Extensions that read files need an explicit trusted base directory. Review
[Security](security.md) before enabling Import, Include, or Source for content
you do not fully control.
