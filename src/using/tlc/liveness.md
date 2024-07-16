# Liveness checks
if TLC is configured to check three temporal formulas A,B,C;
TLC does *not* check if A, B or C are identical formulas. If TLC is set to
check A, A, A (it will still create three OrderOfSolutions). It is up to the
user to prevent this.