Mod-UI forked for MOD Docker 2023
======

This is a modified version of mod-ui that enables the change of the pedalboards and the snapshot using MIDI Program Change commands. 
The Pedalboards can be changed using MIDI Channel 1, the Snapshots can be changed using MIDI Channel 2. 
The values for the Program Change start at 0 (zero), so using for instance Channel 1 PC 0 it loads the first pedalboard in your library and so on... the pedalboards are ordered in a not so clear way in mod-ui, so it's better if you call them like "001 - First song", "002 - Second song" and so on, this way you can keep them in the desired order.

Little Bug: 
Currently there is a bug in mod-ui, when you change the pedalboard via MIDI, the page refreshes, but the title on the top left keep the previous pedalboard name. If you refresh the mod-ui page then it updates correctly. I tried to fix it but i had no luck, sorry. 

mod-ui
======

This is the UI for the MOD software. It's a webserver that delivers an HTML5 interface and communicates with mod-host.
It also communicates with the MOD hardware, but does not depend on it to run.

Install
-------

There are instructions for installing in a 64-bit Debian based Linux environment.
It will work in x86, other Linux distributions and Mac, but you might need to adjust the instructions.

The following packages will be required::

    $ sudo apt-get install virtualenv python3-pip python3-dev git build-essential libasound2-dev libjack-jackd2-dev liblilv-dev libjpeg-dev zlib1g-dev

NOTE: libjack-jackd2-dev can be replaced by libjack-dev if you are using JACK1; libjpeg-dev is needed for python-pillow, at least on my system.

Start by cloning the repository::

    $ git clone git://github.com/moddevices/mod-ui
    $ cd mod-ui

Create a python virtualenv::

    $ virtualenv modui-env
    $ source modui-env/bin/activate

Install python requirements::

    $ pip3 install -r requirements.txt

Compile libmod_utils::

    $ make -C utils

Run
---

Before running the server, you need to activate your virtualenv
(if you have just done that during installation, you can skip this step, but you'll need to do this again when you open a new shell)::

    $ source modui-env/bin/activate

mod-ui depends on mod-host and the JACK server running in order to make sound. So after you have JACK setup and running, in another terminal do::

    $ mod-host -n -p 5555 -f 5556

If you do not have mod-host, you can tell mod-ui to fake the connection to the audio backend.
You will not get any audio, but you will be able to load plugins, make connections, save pedalboards and all that. For this, run::

    $ export MOD_DEV_HOST=1

And now you are ready to start the webserver::

    $ export MOD_DEV_ENVIRONMENT=0
    $ python3 ./server.py

Setting the environment variables is needed when developing on a PC.
Open your browser and point to http://localhost:8888/.
