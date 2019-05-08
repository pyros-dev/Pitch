PyROS
=====

---

Confused ?
----------

Different perspectives:

@snap[west span-50]
@ul[spaced]
- rospy wants to put python inside ROS

a.k.a 'Python as an afterthought'
@ulend
@snapend

@snap[east span-50]
@ul[spaced]
- PyROS wants to get ROS inside Python

a.k.a Use the right tool for the right job
@ulend
@snapend
---

Overview
--------

- Why ?
- python & ROS packaging (catkin_pip)
- 
- ROS is not everything 
- Python is dynamic (pyros_setup & rosimport)

---


@snap[north-west]
So, Why?
--------
@snapend

@snap[west span-50]
A bit of context: GoCart
@snapend

@snap[east span-50]
![](assets/img/GOCART120.3_UI_cropped.png)
@snapend

---


@snap[west span-50]
GoCart:
@ul[spaced]
- travels in a building
- takes the elevator
- interracts with people
@ulend
@snapend

@snap[east span-50]
![](assets/img/GOCART120.3_UI_cropped.png)
@snapend


Note:

- Yujin Robot : a long time ROS contributor
- Gocart Team and especially Daniel Stonier.

+++

But Why, really?
------------------

User Perspective

![](assets/img/what_connect.png)


Find some way to show information to the user...

---

Mandatory Disclaimer
--------------------

- Cramming two years in 20 minutes

- Some things will be left out

- Beware : Here be Anachronisms

---

Let the work begin...
---------------------

+++

@snap[west]
@ul[spaced]
- C++, CMake, make/ninja, IDE/Text Editor
- CVS : svn/git, Test Framework, Libraries
- System packages, Documentation System
- ...
- catkin, ROS packages
- `source setup.bash` !
- change your code for ROS !
- ...
- And we wanna a webserver, really ?
@ulend
@snapend

+++

@snap[north]
@ul[spaced]
- Python (tests/docs included)
- pick up a webserver library (Rostful from BenKehoe)
- From code to a working website in a few days.
@ulend
@snapend


@snap[south]
![](assets/img/first_connect.png)
@snapend

---
@title[Python ?]

@snap[west span-50]
## Programming Language Dilemma
@snapend

@snap[east span-50]
![](assets/img/ProgLangTri.png)
@snapend

---
@ul[spaced]
@snap[west span-50]
Your customers care only about : 
- what they can see and
- how <span style="color:green">FAST</span> you
- can <span style="color:green">CHANGE</span> it.
@snapend

@snap[east span-50]
![](assets/img/ProgLangTri.png)
@snapend
@ulend

---


@snap[north]
Rebooting... from 2015
----------------------
@snapend

@snap[west span-45]
@ul[spaced]
- Ubuntu 14.04 Trusty
- Python2.7
- ROS Indigo
@ulend
@snapend


@snap[east span-45]
@ul[spaced]
- Ubuntu packages: apt
- Python packages: pip
- ROS packages: apt ? distutils ? custom ?
@ulend
@snapend

+++

Problem: change setup.py code
-----------------------------

@ul[spaced]
- catkin will parse setup.py
- setup.py must have sPeCiFiC code style.
- setup.py must rely on distutils only.
@ulend


+++

@snap[north]
One solution: [catkin_pip @fa[external-link]](http://github.com/pyros-dev/catkin_pip)
------------------------
@snapend

@snap[west]
@ul[spaced]
- builds a standard (PYPA) python package for ROS
- uses pip to install dependencies
- => most packages do not need any changes in setup.py
- Minimal patches for third party releases.
@ulend
@snapend

+++

Ported many python packages to [ROS packages @fa[external-link]](http://repositories.ros.org/status_page/ros_indigo_default.html?q=alexv)

+++

rostful evolves
---------------

With recent python packages in ROS, I can :

- turn rostful into a 'standard' python package.
- `import flask` and build a usual flask WSGI app.
- potentially use rostful for non-ROS things...

+++

rostful debug UI
----------------

@snap[west]
@ul[spaced]
- This speeds up changes and evolution
- I quickly build a debug web interface (jquery).
@ulend
@snapend


![](assets/img/debug_connect.png)


---

Little break : self-reflection time
-----------------------------------

- There are many way to package code for ROS.
- Different ways match different usecases

Quick mention of [ros1_template @fa[external-link]](http://github.com/pyros-dev/ros1_template), might be useful when starting ROS development...

--

Zen of Python
-------------

There should be one-- and preferably only one --obvious way to do it.


Note: 

- Could we do better ? the question remains open.

---

Problem: scale and isolation
----------------------------

@ul[spaced]
@snap[west span-50]
- 1 REST request
- 1 Linux process
- 1 ROS node
@snapend

@snap[east span-50]
![](assets/img/multi_connect.png)
@snapend


@snap[south span-100]
Load on the "ROS system" depends on the ingress traffic from outside the system
@snapend
@ulend

+++

One solution: split
-------------------

@ul[spaced]
- Web framework is useful for handling web and random traffic
- ROS is quite sensitive and must be kept safe and sound.
- Enter PyROS
@ulend

+++

@snap[north]
PyROS
-----
@snapend

@snap[west span-75]
@ul[spaced]
- message-passing multiprocess system
- interfaces between distributed systems
- a software attempt at isolation (via network-aware OS processes)
- physical (hardware) isolation possible if necessary.
@ulend
@snapend

Note:

- Started pure python development
- Encountered quite a few annoying ROS quirks.

+++

Problem: Initialization
-----------------------

@ul[spaced]
- ROS wants use to `source setup.bash`
- But that changes the current environment (how ?)
- Including PYTHONPATH
- => breaks vitualenvs
- => changes import behavior
@ulend


+++

One solution: python
--------------------

@ul[spaced]
setup.bash is a shell script.

Python can do the same.
@ulend

+++

pyros_setup
-----------

ROS setup for python processes.

![](assets/img/pyros_setup_take1.svg)

Note:

- Demo TODO ?

+++

Benefits
--------

- unittests working from anywhere, no matter how you launch them.
- virtual environments become workable again.

+++

Downsides
---------

One need a broader perspective:
- A script is a program.
- Your system environment is the global state.

Note:
- a program is a script
- written in assembler 
- interpreted by your hardware.

---

Problem: Maintenance becomes too heavy for one
-----------------------------------------

- Not easy to find and recrut developers 
- Need web backend AND robotics interest / background.
- => do we really need a web server on the robot ?

+++

One solution: Inversion of Control
----------------------------------

## Do not control the robot, control its information.

@ul[spaced]
![](assets/img/full_connect_local.png)
![](assets/img/full_connect.png)
@ulend

+++

@ul[spaced]
- The robot connects to a webserver (maintained by web developers)
- The robot becomes a simple webclient.

- pyros_msgs & pyros_schemas : web data validation for ROS
- Web Server proxied via local ROS service.
@ulend

+++

pyros_msgs
----------

- [pyros_msgs @fa[external-link]](http://github.com/pyros-dev/pyros-msgs)
- typechecks and converts between python types and ROS.
- simple python package
- custom typechecker but could rely on an existing one.

+++

pyros_schemas
-------------

- [pyros_schemas @fa[external-link]](http://github.com/pyros-dev/pyros-schemas)
- typechecks and converts between web data (json) to python types (dict).
- simple python package
- relies on python/web packages.

---

Conclusion ?
------------

@ul[spaced]
- simpler ROS packaging for python code (catkin_pip)
- middleware to interface ROS with other systems (pyros)
- simple webapp to get a debug web interface on ROS (rostful)
- simple communication design for easier web integration (pure python)
- I am still a team of one...
@ulend


---

A team of one
-------------


@ul[spaced]
- Many, many, too many packages to [integrate into ROS @fa[external-link]](http://repositories.ros.org/status_page/ros_indigo_default.html?q=alexv).
- Maintenance takes too much time...
- Isn't there is a simpler way ?
@ulend

---

@snap[north]
rosimport
---------
@snapend

@snap[west span-50]
@ul[spaced]
- most of the usual ROS workflow has been automated into python.
- actually, catkin is only useful to generate ROS message class.
- but... python can do it on the fly ! at import time.
@ulend
@snapend

@snap[east span-50]
![](assets/img/rosimport_take1.svg)
@snapend

+++


@ul[spaced]
- Bye build times.
- Bye custom deb/ros/py packaging.
- Hello python.
@ulend

---

Review
------

@ul[spaced]
- C++ optimizes runtime
- Python optimizes devtime
- Use the right tool for the job
@ulend

---

Be dynamic
----------

@ul[spaced]
- rostful: web server for ROS.
- pyros_setup: ROS setup at import time
- rosimport: message generation at import time
- [Dynamic dynamic_reconfigure @fa[external-link]](https://github.com/awesomebytes/ddynamic_reconfigure)
- roslaunch to come ???
@ulend

---

Future ?
--------

@ul[spaced]
- ROS-related development could be more interactive and accessible.
- PyROS could be simpler.
- PyROS could bridge more systems (MQTT, [WAMP @fa[external-link]](https://wamp-proto.org/), etc.)
- Free Software depends on you.
- It IS yours, thats what 'Free' means.
@ulend






