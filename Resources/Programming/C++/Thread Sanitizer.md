
Thread sanitizer keeps track of events in a global state and an be used to determine a "happens before" relation or common lock between 2 memory access events. 

It does this with a global vector clock, monitoring times and relative times between threads for memory accesses. A comparison of the vector clock for different threads can reveal data_race scenarios. 

Thread sanitizer optimised using "epochs" to record these memory accesses in a group instead of individual entries for every memory access. This is a performance optimisation. 