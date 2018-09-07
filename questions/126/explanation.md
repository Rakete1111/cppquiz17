[basic.lookup.unqual]§6.4.1¶13 states "A name used in the definition of a static data member of class `X` (...) is looked up as if the name was used in a member function of `X`."

Even though the call `foo()` occurs outside the class, since `foo` is used in the definition of the static data member `foobar:x`, it is looked up as if `foo()` was called in a member function of `foobar`. If `foo()` was called in a member function of `foobar`, `foobar:foo()` would be called, not the global `foo()`.
