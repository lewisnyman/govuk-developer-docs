# GOV.UK Developer Docs

Useful links to the repos and tools that are part of GOV.UK development.

Live at: [alphagov.github.com/govuk-developers](https://alphagov.github.com/govuk-developers)

![Screenshot of the docs](screenshot.png)

## Technical documentation

This is a static site, generated by Jekyll.

The [index](index.html) and [operations](operations.html) pages use the [Jekyll data files feature](https://jekyllrb.com/docs/datafiles/) to display content. It uses [content.json](_data/content.json) as the source. This JSON is generated with `rake build_data`, which parses the content [defined in YAML-files](content) and augments it with data from the GitHub API.

Since the GitHub data is not dynamic, you'll have to rebuild & redeploy to update the site.

### Dependencies

- Ruby 2.3.0

### Running locally

The first time you'll need to bundle:

```
bundle install
```

To run the app locally:

```
bundle exec jekyll serve --watch --baseurl ''
```

The app will appear at http://127.0.0.1:4567/.

### Data generation

```
rake build_data
```

You may need a GitHub auth token if you find yourself rate limited. You can create one here:

https://github.com/settings/tokens/new

It doesn't need any permissions.

Use it like this:

```
GITHUB_TOKEN=somethingsomething rake build_data
```

### Building the project

Build the site with:

```
bundle exec jekyll build
```

This will create a bunch of static files in `/build`.

## Licence

[MIT License](LICENCE.md)
