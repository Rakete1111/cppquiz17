The destructor of a `future` returned from `async` is required to block until the `async` task has finished (see elaboration below). Since we don't assign the `future`s that are returned from `async()` to anything, they are destroyed at the end of the full expression (at the end of the line in this case). [class.temporary]§15.2¶4 in the standard: "Temporary objects are destroyed as the last step in evaluating the full-expression (§4.k) that (lexically) contains the point where they were created."

This means that the first async call is guaranteed to finish execution before `async()` is called the second time, so, while the assignments themselves may happen in different threads, they are synchronized.

Elaboration on synchronization:
According to [futures.async]§33.6.9¶5 of the standard:
Synchronization: Regardless of the policy argument,
[...]
If the implementation chooses the launch::async policy,
— the associated thread completion synchronizes with (§4.1) the return from the first function that successfully detects the ready status of the shared state or with the return from the last function that releases the shared state, whichever happens first.

In this case, the destructor of `std::future<>` returned by the `async()` call is "the last function that releases the shared state", therefore it synchronizes with (waits for) the thread completion.

Scott Meyers writes more about this <http://scottmeyers.blogspot.com/2013/03/stdfutures-from-stdasync-arent-special.html>
