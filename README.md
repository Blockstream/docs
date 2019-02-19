# Blockstream docs

## Notes on using Jeykll on Github Pages

The [website](https://blockstream.github.io/docs) uses [Github Pages](https://pages.github.com/) to serve its content using the files within this repository.

Github Pages itself uses [Jeykyll](https://jekyllrb.com/) to generate static website content from simple "markdown" formatted files. Making changes to pages on the blockstream.github.io/docs site is as easy as editing one of this repository's ".md" files. Accepted changes will be automatically built and served to the website. 

## How to run this site locally and make changes to it

Install ruby, ruby-dev and build-essential packages:

~~~~
sudo apt-get install ruby ruby-dev build-essential
~~~~

Install the Jekyll and bundler gems:

~~~~
gem install jekyll bundler
~~~~

Fork the https://github.com/Blockstream/docs repository to your own GitHub account

Clone your fork to your local machine using a command such as:

~~~~
git clone https://github.com/yourgithubaccountname/docs.git
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

Build the site and start the local server:

~~~~
bundle exec jekyll serve 
~~~~

View the site by browsing to: http://127.0.0.1:4000

Note: changes you make to the .md files will be automatically rebuilt and there is no need to restart the server to view the changes. If you make any changes to the "_config.yml" file however, you will need to stop the server using Ctrl+C and start it again using the "bundle exec jekyll serve" command.

Note: The local build does not currently use the theme used by the main website. 

To propose changes:

Check the status of what files you have changed:

~~~~
git status
~~~~

Add all:

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

Within GitHub you can now propose a Pull Request to the Blockstream/docs repository.
