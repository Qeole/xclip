What is xclip?
==============

xclip is a command line utility that is designed to run on any system with an
X11 implementation. It provides an interface to X selections ("the clipboard")
from the command line. It can read data from standard in or a file and place it
in an X selection for pasting into other X applications. xclip can also print
an X selection to standard out, which can then be redirected to a file or
another program.

Using xclip
===========

Here are some ideas for things you can do with xclip:


* Copy your uptime into the selection for pasting:

  .. code-block:: shell-session

      uptime | xclip

* Copy your password file for pasting:

  .. code-block:: shell-session

      xclip /etc/passwd

* Save some text you have *Edit | Copied* in a web browser:

  .. code-block:: shell-session

      xclip -o -sel clip > webpage.txt

* Open a URL selected in an email client:

  .. code-block:: shell-session

      mozilla `xclip -o`

* Copy ``XA_PRIMARY`` to ``XA_CLIPBOARD``:

  .. code-block:: shell-session

      xclip -o | xclip -sel clip

* In command mode in vim, select some lines of text, then press ``Shift-:`` for
  an ex prompt, and use this command to copy the selected lines of text to the
  primary X selection:

  .. code-block:: shell-session

      !xclip -f

Using xclip for moving files
============================

The programs *xclip-copyfile*, *xclip-pastefile*, and *xclip-cutfile* can be
used for copying and moving files between different directories and
even machines, assuming that you have a working X11 connection. Here
are some examples:

Copying a file to a remote host
-------------------------------

.. code-block:: shell-session

    [maggie.lkpg.cendio.se ~]$ echo "A file created on ${HOSTNAME}" > file1
    [maggie.lkpg.cendio.se ~]$ xclip-copyfile file1
    [sofie.homeip.net ~/doc]$ xclip-pastefile
    file1
    [sofie.homeip.net ~/doc]$ cat file1
    A file created on maggie.lkpg.cendio.se

Copying an entire tree structure
--------------------------------

.. code-block:: shell-session

    [sofie.homeip.net ~]$ xclip-copyfile doc
    [maggie.lkpg.cendio.se ~/tmp]$ xclip-pastefile
    doc/
    doc/letter-mom-april.txt
    doc/file1
    doc/letter-dad-march.txt

Copying files with preserved path information
---------------------------------------------

.. code-block:: shell-session

    [maggie.lkpg.cendio.se ~]$ xclip-copyfile -p /etc/sysconfig/grub
    tar: Removing leading `/' from member names
    [sofie.homeip.net ~/tmp]$ xclip-pastefile
    etc/sysconfig/grub
    [sofie.homeip.net ~/tmp]$ ls etc/sysconfig/grub
    etc/sysconfig/grub

Moving files
------------

.. code-block:: shell-session

    [sofie.homeip.net ~]$ ls letter-brother-may.txt
    letter-brother-may.txt
    [sofie.homeip.net ~]$ xclip-cutfile letter-brother-may.txt
    [sofie.homeip.net ~]$ ls letter-brother-may.txt
    ls: cannot access letter-brother-may.txt: No such file or directory
    [sofie.homeip.net ~]$ cd doc
    [sofie.homeip.net ~/doc]$ xclip-pastefile
    letter-brother-may.txt

Features
========

* Reads data piped to standard in or files given as arguments
* Prints contents of selection to standard out
* Accesses the ``XA_PRIMARY``, ``XA_SECONDARY`` or ``XA_CLIPBOARD`` selection
* Accesses the cut-buffers
* Supports the ``INCR`` mechanism for large transfers
* Connects to the X display in ``$DISPLAY``, or specified with
  ``-display host:0``
* Waits for selection requests in the background

Selections
==========

For a good overview of what selections are about, have a look at
https://specifications.freedesktop.org/clipboards-spec/clipboards-latest.txt.
Short version:

* ``XA_PRIMARY`` contains the last text you highlighted
* Middle click pastes ``XA_PRIMARY``
* ``XA_CLIPBOARD`` contains text explicitly copied with *Edit | Copy*,
  ``Ctrl-C`` etc.
* *Edit | Paste* pastes ``XA_CLIPBOARD``
* xclip uses ``XA_PRIMARY`` unless you specify otherwise with ``-selection``

Can I help?
===========

Glad you asked! At this stage, I'm pretty happy with the features and
implementation, so if you have anything at all that should be done, I want to
hear about it. Doesn't matter how small, compiler warnings, segfaults, spelling
mistakes, whatever, I want to get it sorted out. xclip is not a big project,
I'd like to get all these things sorted out and then declare it "complete".

License
=======

GNU GPL, see the COPYING file for details.

Contact
=======

* Web:
  https://github.com/astrand/xclip

* Email:
  astrand@lysator.liu.se

Please email me about problems, experiences, patches, fixes, etc.
