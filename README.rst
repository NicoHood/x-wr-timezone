X-WR-TIMEZONE
=============

.. image:: https://app.travis-ci.com/niccokunzmann/x-wr-timezone.svg?branch=master
   :target: https://app.travis-ci.com/github/niccokunzmann/x-wr-timezone
   :alt: Travis Build and Tests Status

.. image:: https://badge.fury.io/py/x-wr-timezone.svg
   :target: https://pypi.python.org/pypi/x-wr-timezone
   :alt: Python Package Version on Pypi

.. image:: https://img.shields.io/pypi/dm/x-wr-timezone.svg
   :target: https://pypi.python.org/pypi/x-wr-timezone#downloads
   :alt: Downloads from Pypi



Some calendar providers introduce the non-standard ``X-WR-TIMEZONE`` parameter
to ICS calendar files.
Strict interpretations according to RFC 5545 ignore the ``X-WR-TIMEZONE``
parameter.
This causes the times of the events to differ from those
which make use of ``X-WR-TIMEZONE``.

This module aims to bridge the gap by converting calendars
using ``X-WR-TIMEZONE`` to a strict RFC 5545 calendars.
So, let's put our heads together and solve this problem for everyone!

Some features of the module are:

- Easy install with Python's ``pip``.
- Command line conversion of calendars.
- Piping of calendar files with ``wget`` of ``curl``.

Some of the requirements are:

- Calendars without ``X-WR-TIMEZONE`` are kept unchanged.
- Passing calendars twice to this module does not change them.

Install
-------

Install using ``pip``:

.. code:: shell

    python3 -m pip install x-wr-timezone

Command Line Usage
------------------

You can standardize the calendars using your command line interface.
The examples assume that ``in.ics`` is a calendar which may use
``X-WR-TIMEZONE``. whereas ``out.ics`` does not require ``X-WR-TIMEZONE``
for proper display.

.. code-block:: shell

    cat in.is | x-wr-timezone > out.ics
    x-wr-timezone in.ics out.ics
    curl https://example.org/in.ics | x-wr-timezone > out.ics
    wget -O- https://example.org/in.ics | x-wr-timezone > out.ics

Python
------

After you have installed the library, you an import it.

.. code:: python

    import x_wr_timezone

The function ``to_standard()`` converts an ``icalendar`` object.

.. code:: python

    x_wr_timezone.to_standard(an_icalendar)

Here is a full example which does about as much as this module is supposed to do:

.. code-block:: python

    import icalendar # installed wih x_wr_timezone
    import x_wr_timezone

    with open("in.ics", 'rb') as file:
        calendar = icalendar.from_ical(file.read())
    new_calendar = x_wr_timezone.to_standard(calendar)
    # you could use the new_calendar variable now
    with open('out.ics', 'wb') as file:
        file.write(new_calendar.to_ical())


``to_standard(calendar, timezone=None, in_place=False)`` has these parameters:

- ``calendar`` is the ``icalendar`` calendar object.
- ``timezone`` is an optional time zone. By default, the time zone in 
    ``calendar['X-WR-TIMEZONE']`` is used to check if the calendar needs
    changing.
    When ``timezone`` is not ``None`` however, ``calendar['X-WR-TIMEZONE']``
    will not be tested and it is assumed that the ``calendar`` should be
    changed as if ``calendar['X-WR-TIMEZONE']`` had the value of ``timezone``.
    This does not add or change the value of ``calendar['X-WR-TIMEZONE']``.
    You would need to do that yourself.
- ``in_place`` is set to ``False`` by default so that a changed copy
    of the ``calendar`` argument is returned.
    Set ``in_place=True`` to have the ``calendar`` argument be changed and
    returned.

Development
-----------

1. Clone the `repository <https://github.com/niccokunzmann/x-wr-timezone>`_ or its fork and ``cd x-wr-timezone``.
2. Optional: Install virtualenv and Python3 and create a virtual environment.
    .. code-block:: shell

        virtualenv -p python3 ENV
        source ENV/bin/activate
3. Install the packages.
    .. code-block:: shell

        pip install -r test-requirements.txt -e .
4. Run the tests
    .. code-block:: shell

        pytest

New Releases
------------

To release new versions,

1. edit the Changelog Section
2. edit setup.py, the ``__version__`` variable
3. create a commit and push it
4. Wait for `Travis <https://app.travis-ci.com/github/niccokunzmann/x-wr-timezone>`_ to finish the build.
5. run
    .. code-block:: shell

        python3 setup.py tag_and_deploy
6. notify the issues about their release

Testing
*******

This project's development is driven by tests.
Tests assure a consistent interface and less knowledge lost over time.
If you like to change the code, tests help that nothing breaks in the future.
They are required in that sense.
Example code and ics files can be transferred into tests and speed up fixing bugs.

You can view the tests in the `test folder
<https://github.com/niccokunzmann/x-wr-timezones/tree/master/test>`_.
If you have a calendar ICS file for which this library does not
generate the desired output, you can add it to the ``test/calendars``
folder and write tests for what you expect.
If you like, `open an issue <https://github.com/niccokunzmann/x-wr-timezone/issues>`_ first, e.g. to discuss the changes and
how to go about it.

Changelog
---------
- v0.0.1
    - Initial release supports DTSTART, DTEND, EXDATE, RDATE, RECURRENCE-ID attributes of events.
    - Command line interface as `x-wr-timezone`.

Related Work
------------

TODO: Quote issues and blog posts.

Related Software
----------------

This module uses the ``icalendar`` library for parsing calendars.
This library is used by ``python-recurring-ical-events``
to get events at specific dates..

License
-------

This software is licensed under LGPLv3, see the LICENSE file.
