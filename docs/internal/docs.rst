########################
Documentation Guidelines
########################

This `Sphinx <https://www.sphinx-doc.org/en/master/>`_-based documentation is hosted on `Read the Docs <https://readthedocs.org>`_ and can be located `here <https://opendds.readthedocs.io/en/latest/>`__.
It can also be built locally.
To do this follow the steps in the following section.

********
Building
********

Run :ghfile:`docs/build.py`, passing the kinds of documentation desired.
Multiple kinds can be passed, and they are documented in the following sections.

.. _docs-requirements:

Requirements
============

The script requires `Python 3.6 or later <https://www.python.org/downloads/>`__ and an internet connection if the script needs to download dependencies or check the validity of external links.

You might receive a message like this when running for the first time::

    build.py: Creating venv...
    The virtual environment was not created successfully because ensurepip is not
    available.  On Debian/Ubuntu systems, you need to install the python3-venv
    package using the following command.

        apt install python3.9-venv

If you do, then follow the directions it gives, remove the ``docs/.venv`` directory, and try again.

HTML
====

HTML documentation can be built and viewed using ``docs/build.py -o html``.
If it was built successfully, then the front page will be at ``docs/_build/html/index.html``.

A single page variant is also available using ``docs/build.py -o singlehtml``
If it was built successfully, then the page will be at ``docs/_build/singlehtml/index.html``.

PDF
===

.. note:: This has additional dependencies on LaTeX that are documented `here <https://www.sphinx-doc.org/en/master/usage/builders/index.html#sphinx.builders.latex.LaTeXBuilder>`__.

PDF documentation can be built and viewed using ``docs/build.py -o pdf``.
If it was built successfully, then the PDF file will be at ``docs/_build/latex/opendds.pdf``.

Dash
====

Documentation can be built for `Dash <https://kapeli.com/dash>`_, `Zeal <https://zealdocs.org/>`_, and other Dash-compatible applications using `doc2dash <https://github.com/hynek/doc2dash>`_.
The command for this is ``docs/build.py dash``.
This will create a ``docs/_build/OpenDDS.docset`` directory that must be manually moved to where other docsets are stored.

Strict Checks
=============

``docs/build.py strict`` will promote Sphinx warnings to errors and check to see if links resolve to a valid web page.

.. note::

  The documentation includes dynamic links to files in the GitHub repo created by :rst:role:`ghfile`.
  These links will be invalid until the git commit they were built under is pushed to a Github fork of OpenDDS.
  This also means running will cause those links to marked as broken.
  A workaround for this is to pass ``-c master`` or another commit, branch, or tag that is desired.

Building Manually
=================

It is recommended to use ``build.py`` to build the documentation as it will handle dependencies automatically.
If necessary though, Sphinx can be ran directly.

To build the documentation the dependencies need to be installed first.
Run this from the ``docs`` directory to do this::

  pip3 install -r requirements.txt

Then ``sphinx-build`` can be ran.
For example to build the HTML documentation::

  sphinx-build -M html . _build

****************
RST/Sphinx Usage
****************

* See `Sphinx reStructuredText Primer <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`__ for basic RST usage.
* Inline code such as class names like ``DataReader`` and other symbolic text such as commands like ``ls`` should use double backticks: ````TEXT````.
  This distinguishes it as code, makes it easier to distinguish characters, and reduces the chance of needing to escape characters if they happen to be special for RST.
* `One sentence per line should be perfered. <https://rhodesmill.org/brandon/2012/one-sentence-per-line/>`__
  This makes it easier to see what changed in a ``git diff`` or GitHub PR and easier to move sentences around in editors like Vim.
  It also avoids inconsistencies involving what the maximum line length is.
  This might make it more annoying to read the documentation raw, but that's not the intended way to do so anyway.

Special Links
=============

There are a few shortcuts for linking to GitHub and OMG that are custom to OpenDDS.
These come in the form of `RST roles <https://docutils.sourceforge.io/docs/ref/rst/roles.html>`__ and are implemented in :ghfile:`docs/sphinx_extensions/links.py`.

.. rst:role:: ghfile

  .. code-block:: rst

    :ghfile:`README.md`

    :ghfile:`the \`\`README.md\`\` File <README.md>`

    :ghfile:`the support section of the \`\`README.md\`\` File <README.md#support>`

    :ghfile:`check out the available support <README.md#support>`

    :ghfile:`java/docs/overview.html`

  Turns into:

    :ghfile:`README.md#support`

    :ghfile:`README.md`

    :ghfile:`the \`\`README.md\`\` File <README.md>`

    :ghfile:`the support section of the \`\`README.md\`\` File <README.md#support>`

    :ghfile:`check out the available support <README.md#support>`

    :ghfile:`java/docs/overview.html`

  The path passed must exist, be relative to the root of the repository, and will have to be committed, if it's not already.
  If there is a URL fragment in the path, like ``README.md#support``, then it will appear in the link URL.

  It will try to point to the most specific version of the file:

  * If ``-c`` or ``--gh-links-commit`` was passed to ``build.py``, then it will use the commit, branch, or tag that was passed along with it.
  * Else if the OpenDDS is a release it will calculate the release tag and use that.
  * Else if the OpenDDS is in a git repository it will use the commit hash.
  * Else it will use ``master``.

  If the file ends in ``.html``, there will be an additional link to the file that uses https://htmlpreview.github.io/ so the file can be viewed directly in a web browser.

.. rst:role:: ghissue

  .. code-block:: rst

    :ghissue:`213`

    :ghissue:`this is the issue <213>`

    :ghissue:`this is **the issue** <213>`

  Turns into:

    :ghissue:`213`

    :ghissue:`this is the issue <213>`

    :ghissue:`this is **the issue** <213>`

.. rst:role:: ghpr

  .. code-block:: rst

    :ghpr:`1`

    :ghpr:`this is the PR <1>`

    :ghpr:`this is **the PR** <1>`

  Turns into:

    :ghpr:`1`

    :ghpr:`this is the PR <1>`

    :ghpr:`this is **the PR** <1>`

.. rst:role:: ghrelease

  ``ghrelease`` links to a release on Github using the git tag.
  Note that this behaves differently than the other roles here.
  Without the syntax ```Link text <TARGET>``` syntax, it uses the contents as link text and link to the release tag for the current version, assuming there is one.
  This syntax should only be used in a ``.. ifconfig:: is_release`` directive.
  Also it never parses the contents as inline markup.

  .. code-block:: rst

    :ghrelease:`This is the release`

    :ghrelease:`This is the release <DDS-3.24>`

  Turns into:

    :ghrelease:`This is the release`

    :ghrelease:`This is the release <DDS-3.24>`

.. rst:role:: omgissue

  .. code-block:: rst

    :omgissue:`DDSXTY14-29`

    :omgissue:`this is the issue <DDSXTY14-29>`

    :omgissue:`this is **the issue** <DDSXTY14-29>`

  Turns into:

    :omgissue:`DDSXTY14-29`

    :omgissue:`this is the issue <DDSXTY14-29>`

    :omgissue:`this is **the issue** <DDSXTY14-29>`

CMake Domain
============

For :doc:`/building/cmake` there's a custom CMake `Sphinx domain <https://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html>`__ in :ghfile:`docs/sphinx_extensions/cmake.py`.
All of the directives can and should have RST content nested in them.

.. rst:directive:: .. cmake:func:: NAME

  For public CMake functions.

.. rst:directive:: .. cmake:func:arg:: NAME [SIGNATURE]

  For arguments and options for public CMake functions.
  Should be nested in :rst:dir:`cmake:func` of the function the argument belongs to.

.. rst:role:: cmake:func

  Reference a :rst:dir:`cmake:func` by name or reference a :rst:dir:`cmake:func:arg` by function name followed by argument name in parentheses.
  For example:

  .. code-block:: rst

    :cmake:func:`opendds_target_sources`

    :cmake:func:`opendds_target_sources(OPENDDS_IDL_OPTIONS)`

  Turns into:

    :cmake:func:`opendds_target_sources`

    :cmake:func:`opendds_target_sources(OPENDDS_IDL_OPTIONS)`

.. rst:directive:: .. cmake:var:: NAME

  For public variables

.. rst:role:: cmake:var

  Reference a :rst:dir:`cmake:var` by name.

.. rst:directive:: .. cmake:prop:: NAME

  For properties on CMake targets, or possibly other kinds of properties, that we're looking for or creating for the user.

.. rst:role:: cmake:prop

  Reference a :rst:dir:`cmake:prop` by name.

.. rst:directive:: .. cmake:tgt:: NAME

  A target meant for users that be a library or executable that is imported or exported.

.. rst:role:: cmake:tgt

  Reference a :rst:dir:`cmake:tgt` by name.

.. _docs-news:

****
News
****

The :doc:`news for a release (aka release notes) </news>` is created from separate reStructuredText files called *fragments*.
This makes it possible to change the news without everyone editing one file.
Managing the news fragments is handled as part of the Sphinx documentation to make it easy to link to other relevant documents and files in the source code.
It also makes it possible to easily preview the news before a release.

News Fragments
==============

To add content to the news for a release, a fragment file with the content must be created in :ghfile:`docs/news.d`.
It can be named anything as long as it's reasonably unique, doesn't start with an underscore, and has an ``.rst`` file extension.
The branch name of the PR for this change might be a good name, for example: ``fix_rtps_segfault.rst``.

The following is a simple news fragment example:

.. literalinclude:: /news.d/_example.rst
  :caption: :ghfile:`docs/news.d/_example.rst`
  :language: rst

Fragments contain RST content for the news along with some RST directive-like metadata.
Lines starting with ``#`` are ignored as comments.
Content for the news must be formatted as a `list <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#lists-and-quote-like-blocks>`__.
It will usually be one list item, but can be multiple items if they are separate changes from a user perspective.

.. note::

  If using nested lists, reStructuredText requires these to have empty lines before and after as shown in the example above.

Content must be contained somewhere within a section, indicated by ``news-start-section`` and ``news-end-section``.
Having text outside sections that isn't a comment or a special news directive is an error.

Content should provide a reasonable amount of context to users so they can figure out if and how a change will impact or benefit them.
Some of this can be achieved through :ref:`subsections <docs-news-subsections>`.
Content should link to the rest of the Sphinx documentation and to source files using ``:ghfile:`` if relevant.

PRs will usually have just have one fragment file, but can have more then one.
For example, say there is a PR that is making changes associated with an existing fragment file, but also has changes that are separate from the PRs in that fragment.
This might indicate a new PR should be made for those changes, but if that's impractical, a new separate fragment file for those changes can be made.

Directives
----------

news-prs
^^^^^^^^

``news-prs`` should declare all the PRs that make up the changes to OpenDDS described in the fragment.
It's an error to omit ``news-prs`` from a fragment or to have more than one ``news-prs`` directive.
Do not add ``#`` at the start of the PR numbers.
Please add follow-on PRs separated by spaces as they are created.

.. code-block:: rst

  .. news-prs: 1002 1003 1008

If the changes don't have a PR associated with them, such as a policy change, then put ``none`` instead:

.. code-block:: rst

  .. news-prs: none

news-start-section and news-end-section
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``news-start-section`` and ``news-end-section`` directives contain the news content and define how they're grouped.
All sections with the same name across all the fragments are merged together in the final result.
The top-level sections must be one of the following:

``Additions``
  New user-relevant features

``Platform Support and Dependencies``
  Additional support or changes to support for OS, C++ standards, compilers, and dependencies.

``Deprecations``
  User-relevant features that are being planned on being removed, but removing now would break a significant number of user's use cases.
  This marks them for removal in the next major version.

``Removals``
  User-relevant features removed that were either deprecated, "experimental", or "internal" that users might be relying on anyways.

``Security``
  Fixes for issues that might compromise the security of a system using OpenDDS.
  This doesn't need to be related to :doc:`DDS Security </devguide/dds_security>`.

``Fixes``
  User-relevant fixes not related to security or documentation.

``Documentation``
  Significant changes to documentation, either in the primary Sphinx documentation or in secondary documentation.
  This can be documenting an existing feature or major changes to existing documentation.
  Documentation for a new feature should be linked in the news item for it, not here.
  Minor corrections like spelling and changes to internal documentation probably don't need to be mentioned in the news.

``Notes``
  Changes that might be relevant to users, but might not fit in any of the other sections.
  This could be a change in policy or change outside the OpenDDS source code that doesn't have a PR associated with it.

.. _docs-news-subsections:

Sections can be nested as subsections.
There is no requirement for subsections or restrictions on their names, but it's suggested to group otherwise separate changes if they are all impact the same part of OpenDDS.
For example:

.. code-block:: rst

  .. news-start-section: Fixes
  - Non-RTPS Fix
  .. news-start-section: RTPS
  - RTPS Fix
  .. news-start-section: RTPS Relay
  - RTPS Relay Fix
  - Another RTPS Relay Fix
  .. news-end-section
  - Another RTPS Fix
  .. news-end-section
  - Another Non-RTPS Fix
  .. news-end-section

Will result in:

  - Non-RTPS Fix (:ghpr:`1000`)

  - RTPS

    - RTPS Fix (:ghpr:`1000`)

    - RTPS Relay

      - RTPS Relay Fix (:ghpr:`1000`)
      - Another RTPS Relay Fix (:ghpr:`1000`)

    - Another RTPS Fix (:ghpr:`1000`)

  - Another Non-RTPS Fix (:ghpr:`1000`)

This will allow other fragments to add to "RTPS" or "RTPS Relay".

news-rank
^^^^^^^^^

Rank is an integer used to help sort content and sections.
Setting the rank higher using ``news-rank`` can headline a change within a section.
This is optional and if omitted or if rank is the same, then items are ranked by the oldest PR number.

Once used, ``news-rank`` applies to both subsections and content directly in the section between the ``news-start-section`` and the ``news-end-section``.
Content in subsections will need a separate ``news-rank`` to change the order, as sorting of subsections is separate.
When sections have different ranks across fragments (or even in the same fragment), then the largest of ranks is used.

For an example of all that, let us say we had the following fragments:

.. code-block:: rst
  :caption: These are the older changes

  .. news-prs: 1111
  .. news-start-section: Additions
  # Rank 0 because it's a new section
  - Improved logging in some cases.
  # Lets give XTypes as a section some priority
  .. news-rank: 20
  .. news-start-section: XTypes
  # Rank 0 because it's a new section
  - Added ability to randomize ``DynamicData``
  .. news-rank: 10
  - Added lossless casting from any type to any other type in ``DynamicData``.
  .. news-end-section
  # This gets the same rank 20 as the XTypes section
  - Made logging worse in some cases.
  .. news-end-section

.. code-block:: rst
  :caption: These are the newer changes

  .. news-prs: 2222
  .. news-start-section: Additions
  # Rank 0, the rank 0 older item in the other fragment will get priority
  - Unoptimized all code that's not related to pigeons
  # Let's say we want to give this highest priority
  .. news-rank: 50
  - Added a new transport for IP over messenger pigeon (:rfc:`1149`)
  # This will be overruled by the rank 20 XTypes in the other fragment
  .. news-rank: 10
  .. news-start-section: XTypes
  .. news-rank: 100
  - New pigeon-based ``DynamicData``
  .. news-end-section
  .. news-end-section

These will result in:

  - Added a new transport for IP over messenger pigeon (:rfc:`1149`) (:ghpr:`2222`) [Rank 50]
  - Made logging worse in some cases. (:ghpr:`1111`) [Rank 20]

  - XTypes [Rank 20]

    - New pigeon-based ``DynamicData`` (:ghpr:`2222`) [Rank 100]
    - Added lossless casting from any type to any other type in ``DynamicData``. (:ghpr:`1111`) [Rank 10]
    - Added ability to randomize ``DynamicData`` (:ghpr:`1111`) [Rank 0]

  - Improved logging in some cases. (:ghpr:`1111`) [Rank 0]
  - Unoptimized all code that's not related to pigeons (:ghpr:`2222`) [Rank 0]

The ranks are included in previews of the news as an aid to deciding what the rank of new news content should be.
The preview can be viewed in :doc:`/news` or alternatively by running ``docs/news.py preview``.
The ranks are not included in the final news for a release.

One final thing to note is that top-level sections, like "Additions", have a fixed rank that can't be changed so they always appear in the same order.

Generating the News
===================

Before a release, a preview of the whole news for the next release will always be available in :doc:`/news`.
It's also possible to see the source of that preview by running ``docs/news.py preview`` or ``docs/news.py preview-all``.
During a release the fragments are permanently committed to :ghfile:`docs/news.d/_releases` and :ghfile:`NEWS.md` and the fragment files in :ghfile:`docs/news.d` are removed.

.. seealso:: :doc:`release`
