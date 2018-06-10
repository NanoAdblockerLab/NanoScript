# System Calls

These are system functions available to content script scope.


## Events and Timers

### WaitEvent(enum name)

1 argument expected

`name` - One of: `ready`, `load`

Suspend execution of current script until an event is fired. Does nothing if
the event was already fired.

### WaitSec(uint7 time)

1 argument expected

`time` - The amount of `1` seconds to wait

Suspend execution for about `time` seconds.

### WaitDSec(uint7 time)

1 argument expected

`time` - The amount of `0.1` seconds

Suspend execution for about `time * 0.1` seconds.


## DOM Manipulation

### ElemFind(string query)

1 argument expected

`query` - The query selector.

Write the amount of elements that match `query` into `r[0]`.

### ElemClick(string query)

1 argument expected

`query` - The query selector.

Click all elements that match `query`.
