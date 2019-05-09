## Installation instructions

```
$ pip3 install sphinx
$ pip3 install sphinx_rtd_theme
```

## Writing the docs

Source docs are organized by product in the `/source/` folder. Add `.rst` files and link them in `blockstream-rtd/index.rst`

Left side navigation bar is created via Headings styles, read [this](https://documentation-style-guide-sphinx.readthedocs.io/en/latest/style-guide.html) for more info.

For an example where multiple `.rst` files are used within the same product, check Liquid setup (`/_source/liquid`)

### Generating index.html

After any changes are made, from your `/blockstream-rtd/`, run:
```
$ make clean && make html
```
Navigate to `/blockstream-rtd/_build/html/` and open `index.html` in your browser.


### Modifying sphinx_rtd_theme

The `sphinx_rtd_theme` is customizable on both the page level and on a global level. 

To see all the possible configuration options read the configuring docs [here](https://sphinx-rtd-theme.readthedocs.io/en/latest/installing.html).
