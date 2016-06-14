Rust 1.9 Changelog

#[deprecated] can be added to a library, for compiler notes

fn item types are zero sized, and each fn names a unique type. This
will break code that transmutes fns, so calling transmute on a fn type
will generate a warning for a few cycles, then will be converted to an
error.


Field and method resolution understand visibility, so private fields
and methods cannot prevent the proper use of public fields and
methods.

The parser considers unicode codepoints in the PATTERN_WHITE_SPACE
category to be whitespace.

Breaking Changes

It is illegal to define methods with the same name in overlapping
inherent impl blocks.


APIs

Std::panic unified and deployed. Panic can be collected and managed now.

Mutable methods for collections stabilized

Network Socket apis for TCP/udp stabilized

Libs

Most types implement fmt::Debug


Cargo

Cargo can run concurrently; also, now can pass flags to rustc

New Binaries:

mips-unknown-linux-musl, mipsel-unknown-linux-musl, and i586-pc-windows-msvc
