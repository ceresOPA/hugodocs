+++
# type = "docs"
title = "Game Shortcode"
linkTitle = "Game"
date = 2022-11-15T21:16:48+08:00
# description = "" # Used by description meta tag, summary will be used instead if not set or empty.
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
carousel = false
categories = ["Shortcode"]
tags = ["Game"]
series = ["Docs"]
images = []
+++

A detailed description of game shortcode.

<!--more-->

## Usage

```markdown
{{</* game "GAME_URL" */>}}
```

or via named parameters

| Parameter | Default | Description
|:-:|:-:|---
| `src` | - | The game URL. Required.
| `trigger` | `auto` | Set it as `manual` if you want to load the game manually.

```markdown
{{</* game src="GAME_URL" trigger=manual */>}}
```

## Examples

### Auto Load

{{< game "https://v6p9d9t4.ssl.hwcdn.net/html/6581665/FeatherParkWeb/index.html" >}}

### Manual Load

{{< game src="https://v6p9d9t4.ssl.hwcdn.net/html/6581665/FeatherParkWeb/index.html" trigger=manual >}}
