# List of modifications #

Here is the list of changed behavior:
  1. Tags \fax, \fay now works as they should. Allowed multiple instances in one line.
  1. Negative fontspacing now works from style definition.
  1. VSFilterMod correctly loads attached data if its first symbol in any string of UUE code is "[".
  1. Moveable \org:
    * \org(x1,y1);
    * \org(x1,y1,x2,y2`[`,t1,t2`]`) —  moves origin point from (x1,y1) to (x2,y2) in interval (t1,t2). Similar to \move parameters.