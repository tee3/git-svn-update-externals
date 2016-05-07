``git-svn-update-externals``
============================

Overview
--------

This is a script to enhance ``git-svn`` to honor ``svn:externals``
using Git.

This program recursively ``git svn clone`` s all ``svn:externals``
references for a given Git repository created using ``git-svn`` and
places them in the same directory structure as Subversion would.  This
requires access to the Subversion repository from which the repository
came since the script uses ``git svn show-externals`` to find the
``svn:externals`` references.

The script clones all ``svn:externals`` completely assuming the
standard layout of Subversion projects and checks out the correct
trunk, tag, or branch to support the ``svn:externals`` reference.
Note that each Git repository representing an ``svn:externals``
reference will have a detached head in the same way as a Git submodule
does.

This may work with earlier versions of Git and ``git-svn``, but has
not been tested.

Usage
-----

1. Clone your root repository with ``git-svn`` as desired.

   .. code:: sh

      $ git svn clone -s http://svn.hostname.com/svn/path/to/root root-svn.git

2. Run ``git-svn-update-externals`` at the root of the cloned
   repository.

   .. code:: sh

      $ cd root-svn.git
      $ git svn-update-externals

3. (optional) Install a post-checkout hook to update externals on
   checkout.  This will be slow, but does guarantee that every
   checkout will work as it should with the correct version of
   externals.

   .. code:: sh

      $ vi .git/hooks/post-checkout
      git svn-update-externals

Assumptions
-----------

* All referenced ``svn:externals`` URLs are accessible when running
  the script.

* All referenced ``svn:externals`` projects are standard-layout
  Subversion projects.

Notes
-----

* ``git-svn-update-externals`` can be called repeatedly without issue.

* ``git-svn-update-externals`` can take a long time to run, especially
  over a network.

Requirements
------------

* Python 2.6
* Git 2.8.0
* ``git-svn`` 2.8.0
