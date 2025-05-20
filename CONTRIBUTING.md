# Contributing to MCP Portal

We love your input! We want to make contributing to the MCP Portal as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing new features
- Writing new content
- Becoming a maintainer

## Development Process

We use GitHub to host code, to track issues and feature requests, as well as accept pull requests.

### Pull Requests

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Pull Request Requirements

- Update the README.md with details of changes if applicable
- Update documentation if needed
- The PR should work with GitHub Pages deployment

## Adding Content

### Blog Posts

To add a new blog post:

1. Create a new Markdown file in `content/blog/`
2. Use the following front matter template:

```markdown
---
title: "Your Post Title"
date: YYYY-MM-DDT10:00:00-07:00
draft: false
tags: ["tag1", "tag2"]
categories: ["Category"]
---

Your content here...
```

### Documentation

Documentation files are stored in `content/docs/`. Use the following front matter template:

```markdown
---
title: "Your Documentation Title"
date: YYYY-MM-DDT10:00:00-07:00
draft: false
weight: 10
---

Your documentation content here...
```

The `weight` parameter determines the order in the navigation.

## Local Development

To run this site locally:

1. Install [Hugo](https://gohugo.io/installation/) (extended version)
2. Clone the repository
3. Run `hugo server -D` from the project root
4. Visit `http://localhost:1313` in your browser

## License

By contributing, you agree that your contributions will be licensed under the project's MIT License.

## Code of Conduct

Please note that this project is released with a Contributor Code of Conduct. By participating in this project you agree to abide by its terms.
