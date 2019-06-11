# Blockstream Docs

Documenting our products and services is an ongoing effort. If you would like to contribute, or have found an error or omission in any of the existing documentation, there are a few ways in which you can help.

If you do decide to contribute, thank you for helping to improve the Blockstream Docs website.


## Submit an issue, idea or request

If you are not comfortable using GitHub and editing markdown files, you can still submit an issue, idea or request. You can do this using the [Issues](https://github.com/Blockstream/docs/issues) tab in the repository. There are a number of labels you can assign to an issue which help it get noticed by others who may be able to work on it.

If you do log an issue, please provide the following information to enable others to resolve it as easily as possible:

* A short description of the issue or idea you have.

* The page on which you found an issue, if applicable.


## Submitting content yourself

You can also fork this Blockstream Docs repository and use [Sphinx](http://www.sphinx-doc.org) to generate and run the site locally. This is useful if you want to add new content to the site and are comfortable using Git and markdown. 


### Sphinx installation instructions

Install sphinx and the sphinx_rtd_theme:

```
$ pip3 install sphinx
$ pip3 install sphinx_rtd_theme
```


### Adding, editing and viewing changes locally

Documentation is organized by products, which each have their own folder. Within the folders are `.rst` files that contain markdown that Sphinx uses to generate static html files.

After any changes are made, run the following from your `/blockstream-rtd/` directory in order to clear the contents of the output directory and rebuild the html files:

```
$ make clean && make html
```

To view the generated html files, navigate to `/blockstream-rtd/_build/html/` and open `index.html` in your browser.

Make a Pull Request for review when you have finished making changes.

As a contributor, you may want to help by picking up issues submitted by other members of the community and helping to resolve them.

