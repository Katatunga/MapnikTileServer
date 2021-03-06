Troubleshooting
=====================================

This page contains some advice about errors and problems commonly encountered
during the development of Mapnik Tile Server.

Can't install Docker on Windows
-------------------------------

To use Docker on Windows you need a PRO version.

Can't start docker container on Windows
---------------------------------------

When downloading the sourcecode via `Github Desktop
<https://desktop.github.com/>`_ it can happen, that every file is refactored for
windows usage, but when trying to run the code on a docker container (linux) it
will crash!

So to solve the issue, try to download via the ``CLI`` or via VS Code. If that doesn't work either, try 
downloading the .zip-Archive from Github.

bash: fork: retry
-----------------

When developing on a :ref:`ide_remote_server`, it can happen that you get the
error::

    bash: fork: retry: Die Ressource ist zur Zeit nicht verfügbar
    bash: fork: retry: Die Ressource ist zur Zeit nicht verfügbar
    bash: fork: retry: Die Ressource ist zur Zeit nicht verfügbar
    bash: fork: retry: Die Ressource ist zur Zeit nicht verfügbar

    (bash: fork: retry: the resource is temporarily unavailable)

To solve this error, expand the process limits of your target user. For the user
``foo`` the command is::

    $ echo 'foo             soft    nproc            100' | sudo tee --append /etc/security/limits.conf
    $ sudo reboot

After the reboot, it shouldn't show the error message again. If this message
isn't gone after restart, you may need to use a another hoster. On
:ref:`server_hoster` you can watch out for a new working hoster.

unable to find face-name 'unifont Medium' in FontSet 'fontset-0'
----------------------------------------------------------------

If the error ``unable to find face-name 'unifont Medium' in FontSet`` occurs, it
means that the old version of ``unifont``is missing. The team of 
``openstreetmap-carto`` added the new and old version of unifont as requirements
to load one of the two versions. So if you get an error like below, just
ignore it :) ::

    celeryworker_1   | Mapnik LOG> 2020-02-10 12:17:53: warning: unable to find face-name 'unifont Medium' in FontSet 'fontset-0'
    celeryworker_1   | Mapnik LOG> 2020-02-10 12:17:53: warning: unable to find face-name 'unifont Medium' in FontSet 'fontset-1'
    celeryworker_1   | Mapnik LOG> 2020-02-10 12:17:53: warning: unable to find face-name 'unifont Medium' in FontSet 'fontset-2'

How to delete just all django ohdm tables
----------------------------------------

To just delete django ``OHDM`` tables and not the other django tables like users
use::

    $ docker-compose -f local.yml run --rm django python manage.py migrate ohdm zero

Cannot start service
--------------------

When you try to start the containers and you get an error like::

    ERROR: for postgres  Cannot start service postgres: Ports are not available: listen tcp 127.0.0.1:5432: bind: Der Zugriff auf einen Socket war aufgrund der Zugriffsrechte des Sockets unzulĂ¤ssig.
    ERROR: Encountered errors while bringing up the project.

Then check if no other process is running on ``5432``, ``5555`` and ``8000``.

On linux & mac you can use::

    $ netstat -vanp tcp | grep 5432
    $ netstat -vanp tcp | grep 5555
    $ netstat -vanp tcp | grep 8000

On Windows use CMD::

    $ netstat -an

No such file or directory
-------------------------

When trying Docker on Windows for the first time, sometimes Windows will add ``\r``
on each file, but linux doesn't like it. If you get some error like below, try to download
the repo in a different way (See Can't start docker container on Windows)!::

    /usr/bin/env: 'python\r': No such file or directory

Docker doesn't have access to a drive (Docker Desktop for Windows)
------------------------------------------------------------------

Step 1: If during any step of the installation Docker fails to access a drive (probably stating it fails for unknown reasons), 
you can try to solve this by going to ::

    Docker > Settings > Resources > File Sharing

and selecting the drive to be shared.

Step 2: If that fails as well, first resetting Docker to factory defaults should help. Got to ::

    Docker > Troubleshooting > Reset to factory defaults
    
After resetting repeat step 1.
