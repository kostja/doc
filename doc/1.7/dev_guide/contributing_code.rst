.. _contributing_code:

-------------------------------------------------------------------------------
Contributing code
-------------------------------------------------------------------------------

This section describes primary code-related contributions.

The code contribution process workflow:

1. Get the latest source tree and learn how to build Tarantool
2. Find an issue to work on or open a new one
3. Work on you issue
4. Check guidelines
5. Submit your patch for the code review
6. Participate in the code review
7. Get your changes merged to the trunk

Finding an issue
-----------------

Any defect, even minor, if it changes the user-visible server behavior, needs
a bug report. Report a bug at http://github.com/tarantool/tarantool/issues.

When reporting a bug, try to come up with a test case right away.

WIP

Creating a Patchset
--------------------

Separate each logical change into a separate patch.

For example, if your changes include both bug fixes and performance
enhancements, separate those changes into two or more patches. If your changes
include an API update, and a new feature which uses that new API, separate
those into two commits.

On the other hand, if you make a single change to numerous files, group those
changes into a single patch. Thus a single logical change is contained within
a single patch.

The point to remember is that each patch should make an easily understood
change that can be verified by reviewers. Each patch should be justifiable
on its own merits. If one patch depends on another patch in order for a change
to be complete, that is OK.

When dividing your change into a series of patches, take special care to ensure
that the kernel builds and runs properly after each patch in the series.
Developers using git bisect to track down a problem can end up splitting your
patch series at any point; they will not thank you if you introduce bugs
in the middle.

If you cannot condense your patch set into a smaller set of patches, then
only post say 15 or so at a time and wait for review and integration.

Submitting Changes for Review
-----------------------------

Prerequisites
~~~~~~~~~~~~~

Assume that you already have a local git branch with your work committed to.
The preferred way to send your branches for the peer review is by
using `git-sendmail(1) <https://git-scm.com/docs/git-send-email>`_.
`GitHub's PullRequests <https://github.com/tarantool/tarantool/pulls>`_ are
accepted too, but Tarantool Team prefer to use email to review and discuss
patches.

Getting git-send-email
^^^^^^^^^^^^^^^^^^^^^^

``git send-email`` is an extension to ``git``. You probably already have
``git`` installed, but that's not necessarily enough to have also the
``send-email`` command is available. Some Linux distributions split software
into multiple packages according to their policies. You can check if
``send-email`` command is available by running ``git send-email --help``.
If it shows the man page for ``git-send-email``, then send-email is available.
Otherwise, you need to install the send-email command. Your distribution
probably has a package for it.

Debian/Ubuntu:

.. code-block:: bash

    apt install -y git-email

Fedora/RHEL:

.. code-block:: bash

    yum install -y git-email

Configuring git Name and Email Address
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You should tell ``git`` your name and email address.
You have probably done this already, but if not, run these commands at the
root directory of a git repository.

.. code-block:: bash

    git config user.name <your real name>
    git config user.email <your email>

By default ``git config`` changes the local configuration of your current
repository (see ``.git/config`` file). Use ``git config`` with ``--global``
option to change the global configuration in your home dir (``~/.gitconfig``).

Configuring git-send-email
^^^^^^^^^^^^^^^^^^^^^^^^^^

``git send-email`` sends the emails through your SMTP server, so you need to
configure the server parameters. Here is example how to configure email
for **tarantool.org** domain hosted by ```mail.ru```:

.. code-block:: bash

    git config sendemail.smtpUser <your email>
    git config sendemail.smtpPass <your password>
    git config sendemail.smtpEncryption tls
    git config sendemail.smtpServer smtp.mail.ru
    git config sendemail.smtpServerPort 587
    git config sendemail.to patches@tarantool.org
    git config sendemail.confirm true
    git config sendemail.annotate true
    git config sendemail.suppresscc self

Please get an application password if you have multi-factor authentication.

Tarantool Team uses **patches@** maillist to review and discuss patches.

Sending Simple Branches
~~~~~~~~~~~~~~~~~~~~~~~

If you have the write access to Tarantool's git repository, **please push
a git branch with your work to git repository first**. Use
``gh-NNNN-short-name`` convention for branch names, where NNNN is a
`GitHub Issue <https://github.com/tarantool/tarantool>`_ number. If there are
no open issue related to your patch, please ask yourself why are your doing
this and then open a ticket. See :ref:`admin-bug_reports` for details.

.. code-block:: bash

    git push github local-branch-name:gh-NNNN-short-name

Sending the last commit in the current branch:

.. code-block:: bash

    git send-email -1

Sending some other commit:

.. code-block:: bash

    git send-email -1 <commit reference>

Sending a Patchset for Review
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sending the last 10 commits in the current branch:

.. code-block:: bash

    git send-email -10 --cover-letter --annotate

The ``--cover-letter`` option creates an extra mail that will be sent before
the actual patch mails. You can add write some introduction to the patch set
in the cover letter. If you need to explain the patches, be sure to include
the explanations also in the commit messages, because the cover letter text
won't be recorded in the git history. If you don't think any introduction or
explanation is necessary, it's fine to only have the shortlog that is included
in the cover letter by default, and only set the "Subject" header to something
sensible.

The ``--annotate`` option causes an editor to be started for each of
the mails, allowing you to edit the mails and add comments to patches.

Adding Patch Version Information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default the patch mails will have "[PATCH]" in the subject
(or "[PATCH n/m]", where n is the sequence number of the patch and m is
the total number of patches in the patch set). When sending updated versions
of patches, the version should be indicated: "[PATCH v2]" or "[PATCH v2 n/m]".
To do this, use the -v option. Here's an example (you may want to add
``--annotate`` to add notes to the patch about what changed in the new
version):

.. code-block:: bash

    git send-email -v2 -1

Adding Extra Notes to Patch Mails
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sometimes it's convenient to annotate patches with some notes that are not
meant to be included in the commit message. For example, one might want to
write "I'm not sure if this should be committed yet, because..." in a patch,
but the text doesn't make sense in the commit message. Such messages can be
written below the three dashes "---" that are in every patch after the commit
message. Use the ``--annotate`` option with git send-email to be able to edit
the mails before they are sent.

Formatting and Sending in Two Steps
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Instead of using the ``--annotate`` option, one can first run
``git format-patch`` to create text file(s) (with the ``-o`` option to select
a directory where the text files are stored). These files can be inspected
and edited, and when that is done, one can then use ``git send-email``
(without the -1 option) to send them.

Participating in Code Reviews
-----------------------------

Your patch will almost certainly get comments from reviewers on ways in which
the patch can be improved. You must respond to those comments; ignoring
reviewers is a good way to get ignored in return. Review comments or questions
that do not lead to a code change should almost certainly bring about a comment
or changelog entry so that the next reviewer better understands what is
going on.

Be sure to tell the reviewers what changes you are making and to thank them
for their time. Code review is a tiring and time-consuming process, and
reviewers sometimes get grumpy. Even in that case, though, respond politely
and address the problems they have pointed out.

Trunk Check-in Criteria
-----------------------

WIP

Based on [1_] and [2_].

.. _1: https://www.kernel.org/doc/html/latest/process/submitting-patches.html
.. _2: https://www.freedesktop.org/wiki/Software/PulseAudio/HowToUseGitSendEmail/
