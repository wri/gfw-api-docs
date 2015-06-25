# GFW API Docs

# Local dev

Clone the repo then install the requirements (requires Ruby 2.1):

```bash
bundle install
```

To run on localhost:

```
bundle exec middleman server
== The Middleman is loading
== The Middleman is standing watch at http://0.0.0.0:4567
== Inspect your site configuration at http://0.0.0.0:4567/__middleman/
```

# Publish

We publish to GitHub pages, two steps:

1. Push commits to master
2. ```bundle exec rake publish```

Step 2 actually compiles everything down and makes it live at [](http://wri.github.io/gfw-api-docs)


