# Blockstream Docs

The Blockstream Docs [website](https://blockstream.github.io/docs) uses [Github Pages](https://pages.github.com/) to serve the files within this repository as static web pages.

Github Pages uses [Jekyll](https://jekyllrb.com/) to generate static website content from simple "markdown" formatted files. Making changes to pages on the Blockstream Docs site is as easy as editing one of the ".md" files within this repository. Accepted changes will be automatically built and served through the Blockstream Docs website. 


## How to run the site locally and propose changes

You can run a local copy of the Blockstream Docs site and test any changes you make before making a pull request. Following the steps below will allow you to set up and run a local copy of the site and will pull its theme files from the Blockstream docs theme repository, so you can see exactly how your changes will look once live.


### If you have not run the site locally before...

Install the Jekyll Ruby Gem and dependancies by following the [instructions](https://jekyllrb.com/docs/installation/) on the Jekyll website.

Fork the https://github.com/Blockstream/docs repository to your own GitHub account

Clone your fork to your local machine:

~~~~
git clone https://github.com/<yourgithubaccountname>/docs.git
~~~~

Move into the repository directory:

~~~~
cd docs
~~~~

Move into the docs directory:

~~~~
cd docs
~~~~

Install the dependencies required by the website's Gemfile:

~~~~
bundle install
~~~~


### If you have already set up Jekyll using the instructions above...

Follow the steps below each time you want to test your changes.

Build the site and start the local server:

~~~~
bundle exec jekyll serve 
~~~~

View the site by browsing to: http://127.0.0.1:4000

Note: For some changes to take effect, you may need to restart the server. Use Ctrl+c to stop the server and start it again using the "bundle exec jekyll serve" command.


### To propose changes to Blockstream Docs

Check the status of what files you have changed:

~~~~
git status
~~~~

Add the files you want to commit (or use --all to add them all):

~~~~
git add --all
~~~~

Commit:

~~~~
git commit -m "Your commit message here"
~~~~

Push to your forked repository:

~~~~
git push
~~~~

Within GitHub you can now make a Pull Request to the Blockstream/docs repository.
