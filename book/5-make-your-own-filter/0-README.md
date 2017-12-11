# How to make your own custom filter?
As mentioned earlier, a filter must take at least one input et retrieve at least one output. In between, the method `.update()` must be called. The only exception to that are `io` filters which are opening or writing from/to a file or an HTML5 canvas.  
Most of the time, *regular* filters output one a PixpipeContainer, but in certain cases we can imagine having a filter that perform statistics and outputs a report.
