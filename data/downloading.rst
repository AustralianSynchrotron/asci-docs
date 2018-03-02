Downloading
###########

SFTP Clients
============

You can download your data from the Australian Synchrotron SFTP service.

1. Install and run a SFTP client such as `Filezilla <https://filezilla-project.org/download.php?show_all=1>`_,
   `Cyberduck <https://cyberduck.io/>`_ or `WinSCP <https://winscp.net/>`_.
2. Configure your client with the following settings:

   | **Host / Server:** sftp.synchrotron.org.au
   | **Port:** 22
   | **Protocol:** SFTP
   | **Username:** *Your Portal email address*
   | **Password:** *Your Portal password*

3. When you connect you will be presented with a list of folders for each experiment you have
   attended.

For example, the Filezilla configuration would look like:

.. image:: /_static/filezilla.jpg

To download files or folders, simply drag them from the right pane to the left pane. You can
see the progress of the transfer under "Queued files" at the bottom of the window.

**Note:** Data produced on ASCI will only be available for download if it has been saved in the
experiment folder.


rsync
=====

Rather than downloading your data with a graphical client you can use the command-line
tool rsync. This has advantages including:

* You can run rsync on a folder that is already partially downloaded and it will only copy new or
  changed files
* rsync can resume interrupted download
* rsync downloading can be automated

Installing rsync
----------------

macOS / Linux
~~~~~~~~~~~~~

rsync comes pre-installed with macOS and many Linux distributions (try typing "rsync" into
your terminal). If it is not installed, you can get it with::

   # Debian / Ubuntu:
   sudo apt-get install rsync

   # CentOS / Fedora:
   sudo yum install rsync


Windows
~~~~~~~

rsync can be installed with `Cygwin <https://www.cygwin.com/>`_.


Using rsync
-----------

Run the following command in your terminal::

   rsync -rtzP <your_email_address>@sftp.synchrotron.org.au:/data/<epn>/<path_to_folder> .

For example::

   rsync -rtzP alice@example.edu@sftp.synchrotron.org.au:/data/1234/frames/angles/scan1 .

The flags that are used here are:

* ``-r`` : copy folders are as well as files
* ``-t`` : copy timestamps on files (makes re-syncing faster)
* ``-z`` : compress files before transferring
* ``-P`` : enable resuming partially downloaded files

For more options available to rsync, run ``man rsync``.


Using SSH keys to avoid typing password to rsync
------------------------------------------------

rsync allows you to download your data without being prompted for a password by using SSH.
This is especially useful if you want to set up automated data syncing.

To install keys, first generate them if you haven't already::

   ssh-keygen


Then copy the key with::

   ssh-copy-id -i ~/.ssh/id_rsa <your_email_address>@sftp.synchrotron.org.au

From then on you won't be prompted for a password when you run rsync.

If you are on macOS you will need to install ``ssh-copy-id`` by:

1. Installing `homebrew <https://brew.sh/>`_.
2. Running::

      brew install ssh-copy-id
