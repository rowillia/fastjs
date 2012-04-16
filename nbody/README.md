NBody Optimizations
========================================================================================================================
- - -
# Command Run
java -jar ../third_party/compiler.jar  --compilation_level ADVANCED_OPTIMIZATIONS --js nbody.js --externs ../externs/html5.js > nbody-compiled.js
time d8 --nodebugger nbody/nbody-compiled.js -- 5000000

# Results

Speed up: 60%
Original: 7.98 s
Optimized: 4.98 s

#Techniques Used

- Re-wrote Bodies to store all of their data in a Float64Array instead of local variables.
- Allocate one slab of memory upfront and allocate a Float64Array in each Body that slices into that slab of memory
- Type annotated the code to run it through the Closure Compiler with ADVANCED_OPTIMIZATIONS.  This had no benefit before the rewrite of Body, but afterwards it allowed me to have sane things like constants for index locations without having to pay the cost at runtime of looking them up.
- Moved the second loop in advance inside of the first loop.  There was no need to loop over bodies again.

#Possible future improvements

- Don't allocate a Float64Array per Body and instead do the math ourselves each access.  I don't believe this would be faster however.

