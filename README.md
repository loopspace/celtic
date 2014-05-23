Celtic
======

This is a TikZ/PGF package for drawing Celtic knots.  The basic idea
is the same as my [knots](http://www.ctan.org/pkg/spath3) library:
after drawing the full paths then they are redrawn in the vicinity of
the crossings to get the over/under order correct.

The new feature of this package is the ability to generate a Celtic
knot from a list of "walls" inside a rectangle.
Given a rectangle together with a list of vertical and horizontal
walls inside it, a Celtic knot can be generated wherein the paths
travel diagonally bouncing off the walls and the edges of the
rectangle.  The crossings alternate.

The implementation works by carrying out the about algorithm.
It picks a starting point and direction, and then procedes to trace
out the path.  If it comes back to its starting point, it stops and
looks for another starting point.
It continues this until all the available starting points are used up.

The path is actually built as a sequence of segments.  Each segment
starts and ends under another segment and consists of a single over
crossing.  The coordinate of the over-crossing is remembered so that
the segment can be redrawn clipped to a neighbourhood of this
crossing.  As the drawings are so stylised, the shape of the clip
region can be tightly controlled making it possible to use very thick
paths.

The user interface to this implementation is via a single command
`\CelticDrawPath`.  It has a single mandatory argument which is a
key/value list setting various parameters:

* `size={<width>,<height>}`
* `width=<width>`
* `height=<height>` These set the size of the knot.  The sizes should
   be even (strange things might happen if not).
* `crossings=<list>`
* `symmetric crossings=<list>`
* `ignore crossings=<list>`
* `ignore symmetric crossings=<list>` These specify a list of
   crossings to be considered.  The first two set the crossings to be
   of a particular type.  The second two mean that these crossings
   will not be considered as possible starting points for paths (this
   can be used to carve out a region).  The crossings are specified as
   a semi-colon-delimited list.  For the first two, the terms in the
   list have the format `<x>,<y>,<type>` where `(x,y)` is the
   coordinate of the crossing and `<type>` is one of `|` or `-` to
   designate the type of crossing.  For ignoring, the terms are of the
   format `<x>,<y>`.  Either `<x>` or `<y>` can be a range, specified
   as `<x0>:<x1>`.  Because crossings lie only at certain points (`x +
   y` must be odd), the ranges actually step by `2`.
* `flip` changes the over/under order of crossings.
* `max steps` because the algorithm is recursive, it has a built-in
   bailout.  If the path does not return to its origin after `max
   steps` segments, the algorithm stops.  This can lead to incomplete
   paths.
* `style=<style>` is passed on to `\tikzset`.
* `at=<coord>` is used to locate the lower left corner of the knot.
* `inner clip`
* `outer clip` These are used to set the size of the clipping region
   for the crossings.  They are added to the line width to create a
   diamond region in which the crossing is redrawn.  As the
   expectation is that the paths are drawn with the `double` option,
   there are two clips applied: one for each part of the path.  This
   avoids artefacts being visible when anti-aliasing is in effect.
