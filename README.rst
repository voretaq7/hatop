*************
Documentation
*************

*HATop is an interactive ncurses client for HAProxy*

    Author:  John Feuerstein <john@feurix.com>

    License: GPLv3

    Project URL: http://feurix.org/projects/hatop/

    Mirror URL: http://code.google.com/p/hatop/

    Development URL: http://labs.feurix.org/admin/hatop/


What is HATop?
==============

HATop is an interactive ncurses client and real-time monitoring,
statistics displaying tool for the HAProxy TCP/HTTP load balancer.

HATop's appearance is similar to top(1). It supports various modes
for detailed statistics of all configured proxies and services in near
realtime. In addition, it features an interactive CLI for the haproxy
unix socket. This allows administrators to control the given haproxy
instance (change server weight, put servers into maintenance mode, ...)
directly out of hatop and monitor the results immediately.

*It is important to understand that when multiple haproxy processes are started
on the same socket, any process may pick up the request and thus hatop will
output stats owned solely by that process.  The current haproxy-internal
process id is displayed top right.*


Installation
============

See ``INSTALL`` or refer to :ref:`install`


Command line options
====================

Invoking hatop without options or with ``-h / --help`` results in:

::

  $ hatop --help
  Usage: hatop (-s SOCKET| -t HOST:PORT) [OPTIONS]...

  Options:
    --version             show program's version number and exit
    -h, --help            show this help message and exit

    Mandatory:
      -s SOCKET, --unix-socket=SOCKET
                          path to the haproxy unix socket
      -t TCP_SOCKET, --tcp-socket=TCP_SOCKET
                          address of the haproxy tcp stats socket

    Optional:
      -i INTERVAL, --update-interval=INTERVAL
                          update interval in seconds (1-30, default: 3)
      -m MODE, --mode=MODE
                          start in specific mode (1-5, default: 1)
      -n, --read-only     disable the cli and query for stats only

    Filters:
      Note: All filter options may be given multiple times.

      -f FILTER, --filter=FILTER
                          stat filter in format "<iid> <type> <sid>"
      -p PROXY, --proxy=PROXY
                          proxy filter in format "<pxname>"


Display mode reference
======================

See also: :ref:`screenshots`

::

  ID  Mode    Description

  1   STATUS  The default mode with health, session and queue statistics
  2   TRAFFIC Display connection and request rates as well as traffic stats
  3   HTTP    Display various statistical information related to HTTP
  4   ERRORS  Display health info, various error counters and downtimes
  5   CLI     Display embedded command line client for the unix socket


Keybind reference
=================

See also: :ref:`keybinds`

::

  Key             Action

  Hh?             Display this help screen
  CTRL-C / Qq     Quit

  TAB             Cycle mode forwards
  SHIFT-TAB       Cycle mode backwards
  ALT-n / ESC-n   Switch to mode n, where n is the numeric mode id
  ESC-ESC         Jump to previous mode

  ENTER           Display hotkey menu for selected service
  SPACE           Copy and paste selected service identifier to the CLI

You can scroll the stat views using ``UP / DOWN / PGUP / PGDOWN / HOME / END``.

The reverse colored cursor line is used to select a given service instance.

An unique identifier ``[#<iid>/<#sid>]`` of the selected
service is displayed bottom right.

You can hit ``SPACE`` to copy and paste the identifier in string format
``pxname/svname`` to the CLI for easy re-use with some commands.

For example:

1. Open the CLI
2. Type "disable server "
3. Switch back to some stat view using TAB / SHIFT-TAB
4. Select the server instance using UP / DOWN
5. Hit SPACE

The result is this command line::

    > disable server <pxname>/<svname>

Hotkeys for common administrative actions
-----------------------------------------
::

  Hotkey      Action

  F4          Restore initial server weight

  F5          Decrease server weight:     - 10
  F6          Decrease server weight:     -  1
  F7          Increase server weight:     +  1
  F8          Increase server weight:     + 10

  F9          Enable server (return from maintenance mode)
  F10         Disable server (put into maintenance mode)

Hotkey actions and server responses are logged on the CLI viewport.

You can scroll the output on the CLI view using ``PGUP / PGDOWN``.

A brief keybind reference is logged there directly after startup...


Header reference
================

See also: :ref:`screenshots`

::

  Node        configured name of the haproxy node
  Uptime      runtime since haproxy was initially started
  Pipes       pipes are currently used for kernel-based tcp slicing
  Procs       number of haproxy processes
  Tasks       number of actice process tasks
  Queue       number of queued process tasks (run queue)
  Proxies     number of configured proxies
  Services    number of configured services

In multiple modes
-----------------
::

  NAME        name of the proxy and his services
  W           configured weight of the service
  STATUS      service status (UP/DOWN/NOLB/MAINT/MAINT(via)...)
  CHECK       status of last health check (see status reference below)

In STATUS mode
--------------
::

  ACT         server is active (server), number of active servers (backend)
  BCK         server is backup (server), number of backup servers (backend)
  QCUR        current queued requests
  QMAX        max queued requests
  SCUR        current sessions
  SMAX        max sessions
  SLIM        sessions limit
  STOT        total sessions

In TRAFFIC mode
---------------
::

  LBTOT       total number of times a server was selected
  RATE        number of sessions per second over last elapsed second
  RLIM        limit on new sessions per second
  RMAX        max number of new sessions per second
  BIN         bytes in (IEEE 1541-2002)
  BOUT        bytes out (IEEE 1541-2002)

In HTTP mode
------------
::

  RATE        HTTP requests per second over last elapsed second
  RMAX        max number of HTTP requests per second observed
  RTOT        total number of HTTP requests received
  1xx         number of HTTP responses with 1xx code
  2xx         number of HTTP responses with 2xx code
  3xx         number of HTTP responses with 3xx code
  4xx         number of HTTP responses with 4xx code
  5xx         number of HTTP responses with 5xx code
  ?xx         number of HTTP responses with other codes (protocol error)

In ERRORS mode
--------------
::

  CF          number of failed checks
  CD          number of UP->DOWN transitions
  CL          last status change
  ECONN       connection errors
  EREQ        request errors
  ERSP        response errors
  DREQ        denied requests
  DRSP        denied responses
  DOWN        total downtime


Health check status reference
=============================
::

  UNK         unknown
  INI         initializing
  SOCKERR     socket error
  L4OK        check passed on layer 4, no upper layers testing enabled
  L4TMOUT     layer 1-4 timeout
  L4CON       layer 1-4 connection problem, for example
              "Connection refused" (tcp rst) or "No route to host" (icmp)
  L6OK        check passed on layer 6
  L6TOUT      layer 6 (SSL) timeout
  L6RSP       layer 6 invalid response - protocol error
  L7OK        check passed on layer 7
  L7OKC       check conditionally passed on layer 7, for example 404 with
              disable-on-404
  L7TOUT      layer 7 (HTTP/SMTP) timeout
  L7RSP       layer 7 invalid response - protocol error
  L7STS       layer 7 response error, for example HTTP 5xx

