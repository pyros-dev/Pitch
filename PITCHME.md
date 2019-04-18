# PyROS

---

### rospy wants to get python inside ROS

![](assets/img/presentation.png)

### PyROS wants to get ROS inside Python

---
@title[But Why?]

## A bit of context: GoCart

![](assets/img/some pictures from Yujin)

## Yujin Robot : a long time ROS contributor
### Shoutout to the team and especially Daniel Stonier.
---
@title[But Why? really...]

## User Perspective

![](assets/img/some diagram about User / robot interaction over network)


## Find some simple way to show information to the user...

---
@title[Mandatory Disclaimer]

## Cramming two years in 25 minutes

## Some things will be left out

## Beware : There be anachronisms

---
@title[All hands on deck! work is starting...]


@snap[west span-50]
@ul[spaced text-white]
- C++
- CMake
- make/ninja
- IDE/Text Editor
- Test Framework
- Libraries
- System packages
- CVS : svn/git
- Documentation System
- ...
- catkin
- ROS packages
- change the code !
- source setup.bash !
- ...
- And we wanna a webserver, really ?
@ulend
@snapend

@snap[east span-50]
@ul[spaced text-white]
- Python (tests/docs included)
- pick up a webserver library (Rostful from BenKehoe)
- From code to a working webssite in a few days.
@ulend
@snapend

## Your users care only about what they can see
---
@title[Python ?]

@snap[west span-50]
## Programming Language Dilemma
@snapend

@snap[east span-50]
![](assets/ProgLangTri.png)
@snapend

## Your customers care only about how fast can you change what they can see

---?color=#E58537
@title[Rebooting... from 2015]

@snap[west span-45]
@ul[spaced text-white]
- Ubuntu 14.04 Trusty
- Python2.7
- ROS Indigo
@ulend
@snapend


@snap[west span-45]
@ul[spaced text-white]
- Ubuntu packages: apt
- Python packages: pip
- ROS packages: apt ? distutils ? custom ?
@ulend
@snapend

---
@title[Problem: change setup.py code]


@ul[spaced text-white]
- catkin will parse setup.py
- setup.py must have a spcific form
- setup.py must rely on distutils only.
@ulend


---
@title[One solution: catkin_pip]


@snap[west span-75]
@ul[spaced text-white]
- builds a standard (PYPA) python package for ROS
- uses pip to install dependencies
- => most packages do not need any changes in setup.py
- Minimal patches for third party releases.
@ulend
@snapend

## Ported many python packages to [ROS packages @fa[external-link]](http://repositories.ros.org/status_page/ros_indigo_default.html?q=alexv)

---
@title[rostful evolves]

## With standard packages in ROS, I can turn rostful into a 'standard' python package.

## Import Flask and build on top, just as a usual flask developer would.


---
@title[rostful debug UI]

## Speed up changes and evolution, quickly build a debug web interface (jquery).


---
@title[Little break : self-reflection time]


## There are many way to package code for ROS.

## Different ways match different usecases

## Want to mention ros1_template, might be useful when starting a ros package...


---
@title [Problem: scale and isolation]

@snap[west span-75]
@ul[spaced text-white]
- 1 REST request
- 1 Linux process
- 1 ROS node
@ulend
@snapend

## Load on the "ROS system" depends on the ingress traffic from outside the system
## BAAAAD...

---
@title [One solution: split]

## Web framework is useful for handling web and random traffic

## ROS is quite sensitive and must be kept safe and sound.

## Enter PyROS


---
@title[PyROS]

@snap[west span-75]
@ul[spaced text-white]
- message-passing multiprocess system
- interfaces between other systems
- a software attempt at isolation
@ulend
@snapend

---
@title [Problem: Initialization of processes ?]

## ROS wants use to `source setup.bash`
## But that changes the environment a lot
## Including PYTHONPATH
## => breaks vitualenvs
## => changes import behavior


---
@title [One solution: python]

## setup.bash is a shell script. Python can do the same.

## Enter pyros_setup : ROS setup for python processes.

!()[asciinema]

---
@title [Problem: Maintenance is too heavy for one]

## Not easy to find and recrut developers 
## Need web backend AND robotics interest / background.
## => so, do we really need a web server on the robot ?

---
@title[One solution: Inversion of Control]

## Do not control the robot, control its information.

## The robot connects to a webserver (maintained by web developers)
## The robot becomes a simple webclient.

## pyros_msgs & pyros_schemas : web data validation for ROS
## Transparent service for ROS developers

---
@title[feeling better, but why do I still feel lonely ?]

- Many, many, too many packages to integrate into ROS.
- Maintenance takes a lot of time.
- I am still just a team of one...
- Isn't there is a simpler way ?

---
@title[rosimport]

- catkin is only useful to generate ROS message class
- python can do it on the fly, at import time.

!()[asciinema]

# Bye bye
- build times.
- complex ros/python packaging.

