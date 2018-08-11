# Lia Docs
The official Lia documentation page.

## TODO
- [ ] Add GIFs
- [ ] Remove examples of info/warning box from Tutorial BASIC Part 1
- [ ] Review the config.toml settings (Description field specifically)
- [ ] Update the color scheme to match Lia brand colors
- [ ] Update logo
- [ ] Add favicon

## Development
The website is created using [Hugo](https://gohugo.io/) and the 
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