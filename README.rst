python-pulse-control (pulsectl module)
======================================

Python 2.X ctypes bindings for PulseAudio_ (libpulse), forked from pulsemixer_
project (which had this code bundled) to re-use this awesome stuff in other
projects.

.. _PulseAudio: https://wiki.freedesktop.org/www/Software/PulseAudio/
.. _pulsemixer: https://github.com/GeorgeFilipkin/pulsemixer/

|

.. contents::
  :backlinks: none



Usage
-----

Simple example::

  from pulsectl import Pulse

  with Pulse('volume-increaser') as pulse:
    for sink in pulse.sink_list():
      pulse.volume_change_all_chans(sink, 10)

Listening for server state change events::

  from pulsectl import Pulse, PulseLoopStop

	with Pulse('volume-increaser') as pulse:
		# print 'Event types:', ', '.join(pulse.event_types)
		# print 'Event facilities:', ', '.join(pulse.event_facilities)
		# print 'Event masks:', ', '.join(pulse.event_masks)

		def print_events(ev):
			print 'Pulse event:', ev
			### Be sure to raise PulseLoopStop when event_listen() should return
			# raise PulseLoopStop

		pulse.event_callback_set(print_events, 'all')
		pulse.event_listen()

Misc other tinkering::

  >>> from pulsectl import Pulse
  >>> pulse = Pulse('my-client-name')

  >>> pulse.sink_list()
  [<PulseSinkInfo at 7f85cfd053d0 - desc='Built-in Audio', index=0L, mute=0, name='alsa-speakers', channels=2, volumes='44.0%, 44.0%'>]

  >>> pulse.sink_input_list()
  [<PulseSinkInputInfo at 7fa06562d3d0 - index=181L, mute=0, name='mpv Media Player', channels=2, volumes='25.0%, 25.0%'>]

  >>> pulse.source_list()
  [<PulseSourceInfo at 7fcb0615d8d0 - desc='Monitor of Built-in Audio', index=0L, mute=0, name='alsa-speakers.monitor', channels=2, volumes='100.0%, 100.0%'>,
   <PulseSourceInfo at 7fcb0615da10 - desc='Built-in Audio', index=1L, mute=0, name='alsa-mic', channels=2, volumes='100.0%, 100.0%'>]

  >>> sink = pulse.sink_list()[0]
  >>> pulse.volume_change_all_chans(sink, -10)
  >>> pulse.volume_set_all_chans(sink, 50)

  >>> help(pulse)
  ...

Current code logic is that all methods are invoked through the Pulse instance,
and everything returned from these are "Pulse-Something-Info" objects - thin
wrappers around C structs that describe the thing, without any methods attached.

Module is relatively new and these high-level interfaces might change in the future.



Installation
------------

It's a regular package for Python 2.7 (not 3.X).

Using pip_ is the best way::

  % pip install pulsectl

If you don't have it, use::

  % easy_install pip
  % pip install pulsectl

Alternatively (see also `pip2014.com`_ and `pip install guide`_)::

  % curl https://raw.github.com/pypa/pip/master/contrib/get-pip.py | python2
  % pip install pulsectl

Or, if you absolutely must::

  % easy_install pulsectl

But, you really shouldn't do that.

Current-git version can be installed like this::

  % pip install 'git+https://github.com/mk-fg/python-pulse-control.git#egg=pulsectl'

Note that to install stuff in system-wide PATH and site-packages, elevated
privileges are often required.
Use "install --user", `~/.pydistutils.cfg`_ or virtualenv_ to do unprivileged
installs into custom paths.

.. _pip: http://pip-installer.org/
.. _pip2014.com: http://pip2014.com/
.. _pip install guide: http://www.pip-installer.org/en/latest/installing.html
.. _~/.pydistutils.cfg: http://docs.python.org/install/index.html#distutils-configuration-files
.. _virtualenv: http://pypi.python.org/pypi/virtualenv



Links
-----

* pulsemixer_ - initial source for this project (embedded in the tool).

* `libpulseaudio <https://github.com/thelinuxdude/python-pulseaudio/>`_ -
  low-level bindings module, auto-generated from pulseaudio header files.

  Branches there have bindings for different (newer) pulseaudio versions.