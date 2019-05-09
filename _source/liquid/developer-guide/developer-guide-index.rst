.. _developer-guide:

Developer Guide
===============

The code examples listed below show you how to make use of the most common features of Liquid. Each example uses the command line and the Liquid Client application (liquid-cli) to issue commands to either the Liquid daemon (liquidd) or Liquid Core GUI (liquid-qt), depending on which you are running. 

The example code can be modified to work in other languages like Python, C#, Ruby, Go, Perl, and Java by following the steps in the :ref:`App Examples <liquid-app-examples>` section and issuing the appropriate RPC commands to Liquid using your chosen language instead of via liquid-cli.

It is also worth noting that any examples on the `Elements website`_ will also work on Liquid, as Liquid is built on the Elements codebase. You just need to replace the call to elements-cli with a call to liquid-cli. The Elements website also includes instructions on how to run all the basic_ and more advanced_ code examples listed here (and more) from within one file, that you can copy and paste and run from the terminal.

.. _`Elements website`: https://elementsproject.org/elements-code-tutorial/application-development
.. _`basic`: https://elementsproject.org/elements-code-tutorial/easy-run-code
.. _`advanced`: https://elementsproject.org/elements-code-tutorial/advanced-examples

.. contents:: Table of Contents
   :local:

----------

.. include:: ./basic-commands.rst

----------

.. include:: ./confidential-transactions.rst

----------

.. include:: ./issued-assets.rst

----------

.. include:: ./reissuing-assets.rst

----------

.. include:: ./raw-transaction-assets.rst

----------

.. include:: ./raw-issued-assets.rst

----------

.. include:: ./proof-of-issuance.rst

