PyROS
=====

+++

Confused ?
----------

rospy wants to put python inside ROS

![](assets/img/presentation.png)

PyROS wants to get ROS inside Python


---

Overview
========

- Why ?
- python & ROS packaging (catkin_pip)
- ROS is not everything (pyros_setup & rosimport)
- Python is dynamic

---

So, Why?
========

A bit of context: GoCart

![](assets/img/GOCART120.3_UI.jpg)


+++

GoCart:
- travels in a building
- takes the elevator
- interracts with people

Thanks:
- Yujin Robot : a long time ROS contributor
- Gocart Team and especially Daniel Stonier.

+++

But Why? really...
------------------

- User Perspective

![](assets/img/some diagram about User / robot interaction over network)


- Find some simple way to show information to the user...

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

@snap[west span-50]
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

@snap[west span-50]
@ul[spaced]
- Python (tests/docs included)
- pick up a webserver library (Rostful from BenKehoe)
- From code to a working website in a few days.
@ulend
@snapend

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

Rebooting... from 2015
----------------------


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


---

Little break : self-reflection time
-----------------------------------

- There are many way to package code for ROS.
- Different ways match different usecases
- Quick mention of [ros1_template @fa[external-link]](http://github.com/pyros-dev/ros1_template), might be useful when starting ROS development...


---

Problem: scale and isolation
----------------------------

@snap[west span-75]
@ul[spaced]
- 1 REST request
- 1 Linux process
- 1 ROS node
@ulend
@snapend

### Load on the "ROS system" depends on the ingress traffic from outside the system
### BAAAAD...

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
- a software attempt at isolation (via OS processes)
@ulend
@snapend

+++

Problem: Initialization
-----------------------

- ROS wants use to `source setup.bash`
- But that changes the environment a lot
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

- unittests working from anywhere.
- virtual environments become viable again.

---

Problem: Maintenance is too heavy for one
-----------------------------------------

- Not easy to find and recrut developers 
- Need web backend AND robotics interest / background.
- => so, do we really need a web server on the robot ?

+++

One solution: Inversion of Control
----------------------------------

## Do not control the robot, control its information.

+++

- The robot connects to a webserver (maintained by web developers)
- The robot becomes a simple webclient.

- pyros_msgs & pyros_schemas : web data validation for ROS
- Transparent service for ROS developers

+++

pyros_msgs
----------



+++

pyros_schemas
-------------




---
@title[feeling better, but why do I still feel lonely ?]

- Many, many, too many packages to integrate into ROS.
- Maintenance takes a lot of time.
- I am still just a team of one...
- Isn't there is a simpler way ?

+++
@title[rosimport]

- catkin is only useful to generate ROS message class
- python can do it on the fly, at import time.

!()[asciinema]

+++

- Bye build times.
- Bye custom deb/ros/py packaging.
- Hello python

---

Review
------

- C++ optimizes runtime
- Python optimizes devtime
- Use the right tool for the job

---

PyROS
-----

- pyros_setup: ROS setup at import time
- rosimport: message generation at import time
- more useful stuff...

---

Future ?
--------

- ROS development could be more interactive and accessible
- PyROS could bridge more systems (MQTT, [WAMP @fa[external-link]](https://wamp-proto.org/), etc.)
- FOSS depends on you.
- Actually it is already yours, don't lose it.







