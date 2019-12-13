*********
Changelog
*********

This is a brief overview of the changes introduced by each version.

For the full changelog refer to the git history available at
https://github.com/voretaq7/hatop/commits/

HATop 0.8
=========

This is a Python 3 release of HATop. This series does not attempt to
maintain backward compatibility with Python 2.x systems.

It includes new features such as the ability to use HAProxy TCP sockets
and support for a ~/.hatop configuration file.

HATop 0.8.0
-----------

:Date: TODO

- The first public release of the 0.8.x series




HATop 0.7
=========

This is the first public series after reaching a feature complete state.


HATop 0.7.7
-----------

:Date: Oct 05, 2010

- Feature: Display hotkey footer when pressing ENTER on selected service.

- Feature: Use string identifier ("pxname/svname") instead of numerical
  identifier ("#iid/#sid") for hotkey actions.

- Bugfix: Display cursor and focus input if started in CLI mode.

- Bugfix: Reload stat data if number of proxies or services has changed.

- Bugfix: Prevent infinite size sync if screen size is larger than supported.
  (Jérémy Bonnet)

- Docs: Add notes to INSTALL for the man page. (James Briggs)

- Docs: Fix hatop(1) man page and document the new hotkey footer.


HATop 0.7.6
-----------

:Date: Aug 20, 2010

- Bugfix: Support terminals lacking different cursor visibilities.
  (Matt Behrens)

- Bugfix: Handle empty or incomplete info and stat data.

- Bugfix: Handle unknown proxy names with the -p / --proxy filter options.

- Docs: Add initial hatop(1) man page


HATop 0.7.5
-----------

:Date: Aug 18, 2010

- Bugfix: Pressing ENTER on the embedded CLI could result in display
  corruption. (Cyril Bonté)

- Docs: Add common packaging files CHANGES and INSTALL

- Docs: Add keybind reference in KEYBINDS


HATop 0.7.4
-----------

:Date: Aug 16, 2010

- Bugfix: Fix the expected stat CSV format. (Jim Riggs)


HATop 0.7.3
-----------

:Date: Aug 16, 2010

- Bugfix: Restore compatibility with Python 2.4 and 2.5.


HATop 0.7.2
-----------

:Date: Aug 16, 2010

- Bugfix: Handle socket connections to incompatible HAProxy versions.


HATop 0.7.1
-----------

:Date: Aug 16, 2010

- Bugfix: Prevent timeout changes to the internal interactive session used to
  query for stats.


HATop 0.7.0
-----------

:Date: Aug 15, 2010

- The first public and feature complete version.

.. vim: tw=78
