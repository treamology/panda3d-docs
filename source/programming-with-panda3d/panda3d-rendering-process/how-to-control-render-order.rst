.. _how-to-control-render-order:

How to Control Render Order
===========================

In most simple scenes, you can naively attach geometry to the scene graph and
let Panda decide the order in which objects should be rendered. Generally, it
will do a good enough job, but there are occasions in which it is necessary to
step in and take control of the process.

To do this well, you need to understand the implications of render order. In a
typical OpenGL- or DirectX-style Z-buffered system, the order in which
primitives are sent to the graphics hardware is theoretically unimportant, but
in practice there are many important reasons for rendering one object before
another.

Firstly, state sorting is one important optimization. This means choosing to
render things that have similar state (texture, color, etc.) all at the same
time, to minimize the number of times the graphics hardware has to be told to
change state in a particular frame. This sort of optimization is particularly
important for very high-end graphics hardware, which achieves its advertised
theoretical polygon throughput only in the absence of any state changes; for
many such advanced cards, each state change request will completely flush the
register cache and force a restart of the pipeline.

Secondly, some hardware has a different optimization requirement, and may
benefit from drawing nearer things before farther things, so that the Z-buffer
algorithm can effectively short-circuit some of the advanced shading features
in the graphics card for pixels that would be obscured anyway. This sort of
hardware will draw things fastest when the scene is sorted in order from the
nearest object to the farthest object, or "front-to-back" ordering.

Finally, regardless of the rendering optimizations described above, a
particular sorting order is required to render transparency properly (in the
absence of the specialized transparency support that only a few graphics cards
provide). Transparent and semitransparent objects are normally rendered by
blending their semitransparent parts with what has already been drawn to the
framebuffer, which means that it is important that everything that will appear
behind a semitransparent object must have already been drawn before the
semitransparent parts of the occluding object is drawn. This implies that all
semitransparent objects must be drawn in order from farthest away to nearest,
or in "back-to-front" ordering, and furthermore that the opaque objects should
all be drawn before any of the semitransparent objects.

Panda achieves these sometimes conflicting sorting requirements through the
use of bins.

Cull Bins
---------

The CullBinManager is a global object that maintains a list of all of the cull
bins in the world, and their properties. Initially, there are five default
bins, and they will be rendered in the following order:

=========== ==== ================
Bin Name    Sort Type
=========== ==== ================
background  10   BT_fixed
opaque      20   BT_state_sorted
transparent 30   BT_back_to_front
fixed       40   BT_fixed
unsorted    50   BT_unsorted
=========== ==== ================

When Panda traverses the scene graph each frame for rendering, it assigns each
Geom it encounters into one of the bins defined in the CullBinManager. (The
above lists only the default bins. Additional bins may be created as needed,
using either the ``CullBinManager::add_bin()``
method, or the Config.prc
``cull-bin`` variable.)

You may assign a node or nodes to an explicit bin using the
``NodePath::set_bin()`` interface. set_bin()
requires two parameters, the bin name and an integer sort parameter; the sort
parameter is only meaningful if the bin type is BT_fixed (more on this below),
but it must always be specified regardless.

If a node is not explicitly assigned to a particular bin, then Panda will
assign it into either the "opaque" or the "transparent" bin, according to
whether it has transparency enabled or not. (Note that the reverse is not
true: explicitly assigning an object into the "transparent" bin does not
automatically enable transparency for the object.)

When the entire scene has been traversed and all objects have been assigned to
bins, then the bins are rendered in order according to their sort parameter.
Within each bin, the contents are sorted according to the bin type.

If you want simple geometry that's in back of something to render in front of
something that it logically shouldn't, add the following code to the model
that you want in front:



.. code-block:: python

    model.setBin("fixed", 0)
    model.setDepthTest(False)
    model.setDepthWrite(False)



The above code will only work for simple models. If your model self occludes
(parts of the model covers other parts of the model), the code will not work
as expected. An alternative method is to use a
:ref:`display region <display-regions>` with
``displayRegion.clearDepthActive(True)``.

The following bin types may be specified:

BT_fixed
   Render all of the objects in the bin in a fixed order specified by the
   user. This is according to the second parameter of the NodePath.set_bin()
   method; objects with a lower value are drawn first.
BT_state_sorted
   Collects together objects that share similar state and renders them
   together, in an attempt to minimize state transitions in the scene.
BT_back_to_front
   Sorts each Geom according to the center of its bounding volume, in linear
   distance from the camera plane, so that farther objects are drawn first.
   That is, in Panda's default right-handed Z-up coordinate system, objects
   with large positive Y are drawn before objects with smaller positive Y.
BT_front_to_back
   The reverse of back_to_front, this sorts so that nearer objects are drawn
   first.
BT_unsorted
   Objects are drawn in the order in which they appear in the scene graph, in
   a depth-first traversal from top to bottom and then from left to right.
