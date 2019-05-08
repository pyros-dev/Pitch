PyROS
=====

+++

Confused ?
----------

Different perspectives:

- rospy wants to put python inside ROS
a.k.a Python as an afterthought

- PyROS wants to get ROS inside Python

---

Overview
--------

- Why ?
- python & ROS packaging (catkin_pip)
- 
- ROS is not everything 
- Python is dynamic (pyros_setup & rosimport)

---

So, Why?
--------

@snap[west span-50]
A bit of context: GoCart
@snapend

@snap[east span-50]
![](assets/img/GOCART120.3_UI_cropped.png)
@snapend

+++


@snap[west span-50]
GoCart:
- travels in a building
- takes the elevator
- interracts with people
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


- Find some way to show information to the user...

---

Mandatory Disclaimer
--------------------

- Cramming two years in 20 minutes

- Some things will be left out

- Beware : There be anachronisms

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

@snap[west]
@ul[spaced]
- Python (tests/docs included)
- pick up a webserver library (Rostful from BenKehoe)
- From code to a working website in a few days.
@ulend
@snapend

![](assets/img/first_connect.png)

---

# Your users care only about what they can see

---
@title[Python ?]

@snap[west span-50]
## Programming Language Dilemma
@snapend

@snap[east span-50]
![](assets/img/ProgLangTri.png)
@snapend

---

Your customers care only about : 
- what they can see and
- how <span style="color:green">FAST</span> you
- can <span style="color:green">CHANGE</span> it.

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

One solution: [catkin_pip @fa[external-link]](http://github.com/pyros-dev/catkin_pip)
------------------------

@snap[west span-75]
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


+++

rostful debug UI
----------------

Speed up changes and evolution, quickly build a debug web interface (jquery).



![](assets/img/debug_connect.png)


---

Little break : self-reflection time
-----------------------------------

- There are many way to package code for ROS.
- Different ways match different usecases

Zen of Python:
- There should be one-- and preferably only one --obvious way to do it.

Quick mention of [ros1_template @fa[external-link]](http://github.com/pyros-dev/ros1_template), might be useful when starting ROS development...


Note: 

- Could we do better ? the question remains open.

---

Problem: scale and isolation
----------------------------

@snap[west span-50]
@ul[spaced]
- 1 REST request
- 1 Linux process
- 1 ROS node
@ulend
@snapend

@snap[east span-50]
![](assets/img/multi_connect.png)
@snapend


@snap[south]
Load on the "ROS system" depends on the ingress traffic from outside the system
@snapend

+++

One solution: split
-------------------

- Web framework is useful for handling web and random traffic
- ROS is quite sensitive and must be kept safe and sound.
- Enter PyROS


+++

PyROS
-----

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

- ROS wants use to `source setup.bash`
- But that changes the current environment (how ?)
- Including PYTHONPATH
- => breaks vitualenvs
- => changes import behavior


+++

One solution: python
--------------------

- setup.bash is a shell script. Python can do the same.

+++

pyros_setup
-----------

ROS setup for python processes.

!()[asciinema]


+++

Benefits
--------

- unittests working from anywhere, no matter the environment.
- virtual environments become workable again.

One downside : Need to 'understand' Python.
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
- => so, do we really need a web server on the robot ?

+++

One solution: Inversion of Control
----------------------------------

## Do not control the robot, control its information.

![](assets/img/full_connect.png)


+++

- The robot connects to a webserver (maintained by web developers)
- The robot becomes a simple webclient.

- pyros_msgs & pyros_schemas : web data validation for ROS
- Transparent service for ROS developers

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

Conclusion ? Not Yet...
-----------------------

@ul[spaced]
- simpler ROS packaging for python code (catkin_pip)
- complex design to interface ROS with other systems (pyros)
- simpler webapp to get a debug web interface on ROS (rostful)
- simpler communication design for easier web integration (pure python)
- I am still a team of one...
@ulend


---

A team of one
-------------

- Many, many, too many packages to [integrate into ROS @fa[external-link]](http://repositories.ros.org/status_page/ros_indigo_default.html?q=alexv).
- Maintenance takes time...
- Isn't there is a simpler way ?

+++

rosimport
---------

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

- Bye build times.
- Bye custom deb/ros/py packaging.
- Hello python.

---

Review
------

- C++ optimizes runtime
- Python optimizes devtime
- Use the right tool for the job

---

PyROS, the good parts
---------------------

- rostful: web server for ROS.
- pyros_setup: ROS setup at import time
- rosimport: message generation at import time
- and more: [Dynamic dynamic_reconfigure @fa[external-link]](https://github.com/awesomebytes/ddynamic_reconfigure)

---

Future ?
--------

@ul[spaced]
- ROS-related development could be more interactive and accessible.
- PyROS could be simpler.
- PyROS could bridge more systems (MQTT, [WAMP @fa[external-link]](https://wamp-proto.org/), etc.)
- Free Software depends on you.
- It IS yours, thats what 'Free' means.
@ul[spaced]






