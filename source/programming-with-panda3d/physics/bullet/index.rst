.. _bullet:

Using Bullet with Panda3D
=========================

Bullet is a modern and open source physics engine used in many games or
simulations. Bullet can be compiled on many platforms, among them Windows,
Linux and MacOSX. Bullet features include collision detection, rigid body
dynamics, soft body dynamics and a kinematic character controller.

This section is about how to use the Panda3D Bullet module.

Table of Contents
-----------------


a. [[Bullet Hello World]]
b. [[Bullet Debug Renderer]]
c. [[Bullet Collision Shapes]]
d. [[Bullet Collision Filtering]]
e. [[Bullet Continuous Collision Detection]]
f. [[Bullet Queries]]
g. [[Bullet Ghosts]]
h. [[Bullet Character Controller]]
i. [[Bullet Constraints]]
j. [[Bullet Vehicles]]
k. [[Bullet Softbodies]]
l. [[Bullet Softbody Rope]]
m. [[Bullet Softbody Patch]]
n. [[Bullet Softbody Triangles]]
o. [[Bullet Softbody Tetrahedron]]
p. [[Bullet Softbody Config]]
q. [[Bullet Config Options]]
r. [[Bullet FAQ]]
s. [[Bullet Samples]]


All Bullet classes are prefixed with "Bullet". A list of all classes and their
methods can be found within the `API
reference <https://www.panda3d.org/reference/python/namespace!panda3d.bullet>`__.
However, the class and function descriptions are still missing.

Note:The Panda3D Bullet module makes great effort to integrate Bullet physics
as tightly as reasonably possible with the core Panda3D classes. However, when
implementing collision detection and physics, you can not mix Panda3D's
internal physics & collision system, ODE, PhysX and Bullet. More explicitly:
Bullet bodies won't collide with ODE bodies and CollisionNodes.

Samples on how to use the Panda3D Bullet module can be found in the following
archive: https://www.panda3d.org/download/noversion/bullet-samples.zip


.. only:: cpp

    Note: All samples are currently available in Python code only.



.. toctree::
   :maxdepth: 2

   hello-world
   debug-renderer
   collision-shapes
   collision-filtering
   ccd
   queries
   ghosts
   character-controller
   constraints
   vehicles
   softbodies
   softbody-rope
   softbody-patch
   softbody-triangles
   softbody-tetrahedron
   softbody-config
   config-options
   faq
   samples
