The original technique, which converts a 2D vector image to a regularly
sampled signed distance field, represented as a GPU texture pixmap, 
rendered by a GPU shader:
<http://www.valvesoftware.com/publications/2007/SIGGRAPH2007_AlphaTestedMagnification.pdf>

An interesting extension:
"Sharp corners can be preserved, however, by using more than
one channel of the texture to represent different edges intersecting
within a texel. For instance, with two channels, the intersection
of two edges can be accurately represented by performing a logical
AND in the pixel shader. In Figure 8, we have stored these two edge
distances in the red and green channels of a single texture, resulting
in a well-preserved pointy corner."

A multi-channel variant which uses RGB subpixels to render vector images
more precisely:
<https://github.com/Chlumsky/msdfgen>

Pure speculation:
What if I used X/Y/Z distances, instead of a single scalar distance?
Would that help with rendering corners, and does that makes sense?
* (The X/Y/Z distances could easily be infinity when a small sphere is nearby
  along a diagonal.)
* rotation would screw up this property?

What if my distance field computes 2 numbers: distance to the nearest surface,
and distance to the nearest singularity?
