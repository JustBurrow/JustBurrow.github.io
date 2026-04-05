# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal technical blog hosted on GitHub Pages at `blog.lul.kr`. Built with Jekyll 4.3.2 using the Minima 2.5 theme. Content is written in Korean (Markdown).

## Commands

```bash
# Install dependencies
bundle install

# Serve locally with live reload
bundle exec jekyll serve --livereload

# Build for production
bundle exec jekyll build
```

## Blog Post Conventions

Front matter for posts:
```yaml
---
layout: post
title: "포스트 제목"
---
```

Posts are published by merging feature branches into `master`. GitHub Pages builds and deploys automatically.

## References

1. [GitHub Pages 문서 - GitHub 문서][1]
2. [Jekyll • Simple, blog-aware, static sites | Transform your plain text into static websites and blogs][2]
3. [Bundler: The best way to manage a Ruby application's gems][3]
4. [Alistair Mavin EARS: Easy Approach to Requirements Syntax | Official Guide][4]
5. [RFC 2119 - Key words for use in RFCs to Indicate Requirement Levels][5]

[1]: https://docs.github.com/pages
[2]: https://jekyllrb.com
[3]: https://bundler.io
[4]: https://alistairmavin.com/ears/
[5]: https://datatracker.ietf.org/doc/html/rfc2119