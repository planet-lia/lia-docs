# Lia Docs
The official Lia documentation website.

The website if available on: [https://docs.liagame.com](https://docs.liagame.com).

We encourage community collaboration, we are always glad to receive pull requests. 
If you would like to contribute, please read the [development](#development)
section.

## Development
All development is done on the *dev* branch. The *master* branch represents
the [production documentation](https://docs.liagame.com).

We also have a development (dev branch) page that is used for final review of
the changes to be pushed to the production documentation, available here:
[https://docs-dev.liagame.com](https://docs-dev.liagame.com)


The website is created using [Hugo](https://gohugo.io) and the 
[Material Docs theme](https://github.com/digitalcraftsman/hugo-material-docs).

Unless you have a good reason, **do not** change the `hugo-material-docs` theme. 
The theme has been forked and many changes have been made. If you need to add/change
something, there exists a pretty good chance it is possible in the project and
not the forked theme. Consult with the project maintainers (open an issue) to
make sure.

### A Note on Static Files
Anything in the folder `static` will be treated as the root path of the final build, eg. 
in source: `static/static/images/logo.png` will become in the build:
`http(s)://<BASE_WEBSITE_ROOT>/static/images/logo.png`.
Hopefully this explains the duplicate static folder.

# License
All files are released under the Apache License version 2.0.
