.. _transparency-and-blending:

Transparency and Blending
=========================

Dealing with Depth-Sorting Note: this page is cut-and-pasted from a howto we
found. We'll polish it later.

Usually transparency works as expected in Panda automatically, but sometimes
it just seems to go awry, where a semitransparent object in the background
seems to partially obscure a semitransparent object in front of it. This is
especially likely to happen with large flat polygon cutouts, or when a
transparent object is contained within another transparent object, or when
parts of a transparent object can be seen behind other parts of the same
object.

The fundamental problem is that correct transparency, in the absence of
special hardware support involving extra framebuffer bits, requires drawing
everything in order from farthest away to nearest. This means sorting each
polygon--actually, each pixel, for true correctness--into back-to-front order
before drawing the scene.

It is, of course, impossible to split up every transparent object into
individual pixels or polygons for sorting individually, so Panda sorts objects
at the Geom level, according to the center of the bounding volume. This works
well 95% of the time.

You run into problems with large flat polygons, though, since these tend to
have parts that are far away from the center of their bounding volume. The
bounding-volume sorting is especially likely to go awry when you have two or
more large flats close behind the other, and you view them from slightly
off-axis. (Try drawing a picture, of the two flats as seen from the top, and
imagine yourself viewing them from different directions. Also imagine where
the center of the bounding volumes is.)

Now, there are a number of solutions to this sort of problem. No one solution
is right for every situation.

First, the easiest thing to do is to use M_dual transparency. This is a
special transparency mode in which the completely invisible parts of the
object aren't drawn into the Z-buffer at all, so that they don't have any
chance of obscuring things behind them. This only works well if the flats are
typical cutouts, where there is a big solid part (alpha == 1.0) and a big
transparent part (alpha == 0.0), and not a lot of semitransparent parts (0.0 <
alpha < 1.0). It is also a slightly more expensive rendering mode than the
default of M_alpha, so it's not enabled by default in Panda. But egg-palettize
will turn it on automatically for a particular model if it detects textures
that appear to be cutouts of the appropriate nature, which is another reason
to use egg-palettize if you are not using it already.

If you don't use egg-palettize (you really should, you know), you can just
hand-edit the egg files to put the line:


.. code-block:: text

    <Scalar> alpha { dual }

within the <Texture>
reference for the textures in question.

A second easy option is to use M_multisample transparency, which doesn't have
any ordering issues at all, but it only looks good on very high-end cards that
have special multisample bits to support full-screen antialiasing. Also, at
the present it only looks good on these high-end cards in OpenGL mode (since
our pandadx drivers don't support M_multisample explicitly right now). But if
M_multisample is not supported by a particular hardware or panda driver, it
automatically falls back to M_binary, which also doesn't have any ordering
issues, but it always has jaggy edges along the cutout edge. This only works
well on texture images that represent cutouts, like M_dual, above.

If you use egg-palettize, you can engage M_multisample mode by putting the
keyword "ms" on the line with the texture(s). Without egg-palettize, hand-edit
the egg files to put the line:


.. code-block:: text

    <Scalar> alpha { ms }

within the <Texture>
reference for the textures in question.

A third easy option is to chop up one or both competing models into smaller
pieces, each of which can be sorted independently by Panda. For instance, you
can split one big polygon into a grid of little polygons, and the sorting is
more likely to be accurate for each piece (because the center of the bounding
volume is closer to the pixels). You can draw a picture to see how this works.
In order to do this properly, you can't just make it one big mesh of small
polygons, since Panda will make a mesh into a single Geom of tristrips;
instead, it needs to be separate meshes, so that each one will become its own
Geom. Obviously, this is slightly more expensive too, since you are
introducing additional vertices and adding more objects to the sort list; so
you don't want to go too crazy with the smallness of your polygons.

A fourth option is simply to disable the depth write on your transparent
objects. This is most effective when you are trying to represent something
that is barely visible, like glass or a soap bubble. Doing this doesn't
improve the likelihood of correct sorting, but it will tend to make the
artifacts of an incorrect sorting less obvious. You can achieve this by using
the transparency option "blend_no_occlude" in an egg file, or by explicitly
disabling the depth write on a loaded model with
node_path.set_depth_write(false). You should be careful only to disable depth
write on the transparent pieces, and not on the opaque parts.

A final option is to make explicit sorting requests to Panda. This is often
the last resort because it is more difficult, and doesn't generalize well, but
it does have the advantage of not adding additional performance penalties to
your scene. It only works well when the transparent objects can be sorted
reliably with respect to everything else behind them. For instance, clouds in
the sky can reliably be drawn before almost everything else in the scene,
except the sky itself. Similarly, a big flat that is up against an opaque wall
can reliably be drawn after all of the opaque objects, but before any other
transparent object, regardless of where the camera happens to be placed in the
scene. See howto.control_render_order.txt for more information about
explicitly controlling the rendering order.
