Description
################################################################################

Repo hosting the Premiere Pro CC SDK Guide RST docs, linked into a http://readthedocs.io system hosted at http://ppro-plugin-sdk.aenhancers.com

----

Contribution
################################################################################

Contributors are welcome and encouraged to suggest fixes, adjustments, notes/warnings, and anything else that may help the project.

----

Internal References
********************************************************************************

Anchors should be defined at each page setting relative to the root folder; the anchor for the "Application" page within the JS Object Reference should be::

  .. _jsobjref/Application

And the anchor for a child item (property, method or example)::

  .. _jsobjref/Application.open

Then, to link to these items from other pages, we use::

  :ref:`jsobjref/Application`

or::

  :ref:`jsobjref/Application.open`

If you want different text than the title the anchor points to::

  :ref:`Check this out! <jsobjref/Application.open>`

----

External Links
********************************************************************************

These should follow the following structure::

  `Link Text <http://www.aenhancers.com>`__

----

Tables
********************************************************************************

Function parameter tables should have following order::

  +---------------+------+-----------------------------+
  |   Parameter   | Type |         Description         |
  +===============+======+=============================+
  | ``parameter`` | Type | What does the parameter do? |
  +---------------+------+-----------------------------+

Use `Table Formatter <https://marketplace.visualstudio.com/items?itemName=shuworks.vscode-table-formatter>`_ for VSCode for easier table formating.

----

Admonitions Usage
********************************************************************************

Currently, the following `admonitions <http://docutils.sourceforge.net/docs/ref/rst/directives.html#admonitions>`_ are in use in this project.

Try to keep one piece of data per note, for easier parsing.

::

  .. note::
    Notes detail version added, and/or relevant pieces of information.

  .. tip::
    Tips supply helpful suggestions on usage or behaviours.

  .. warning::
    Warnings convey negative behaviours, or when something won't work the way you'd expect.

----

Build HTML Locally
################################################################################

You may want to build the HTML locally before pushing, in order to ensure that the result is what you'd expect. These files aren't included in the git repo, nor are they used online; this is solely to create a local, offline version of the online docs.

- Install ``Python 2.7``
- Install ``pip``
- Navigate to the project directory and use the command ``pip install -r requirements.txt``
- Build the docs using ``make html``

----

Licensing & Ownership
################################################################################

This project exists for educational purposes only.

All content is copyright Adobe Systems Incorporated.
