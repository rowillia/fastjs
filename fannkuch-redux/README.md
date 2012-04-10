NBody Optimizations
========================================================================================================================
- - -
# Command Run
time dd8 --nodebugger fannkuch-redux/fannkuch-redux.js -- 11

# Results

Speed up: 37%
Original: 7.105 s
Optimized: 5.154 s

#Techniques Used

- Used one Int32Array to store all 4 arrays.  This accounted for ~75% of the overall improvement.
- Unrolled 2 loops one level.  The second loop was unrolled in a way that let us re-use the bounds check from the previous run.
- Added |0 to all integer addition.  This circumvents the need to overflow check the result of the addition.  This gave us another 10% performance improvement.

#Possible future improvements

- There are tons of bounds checks in the generated code that seem redundant.  for example,

var foo = new Int2Array(new Array(1000));
for(int i = 0l i < 100l ++i) {
  foo[i] = i;
}

will bounds check EVERY iteration of the loop, when in reality it only needs to bounds check at the top of the loop.  Granted, this only works for loops of this form, but I feel like that probably constitutes a significant portion of all loops.
