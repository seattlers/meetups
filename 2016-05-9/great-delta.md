Delta from Rust 1.0 to 1.8
pnathan
(@p_nathan on Twitter)


Summary of the summary:

Library standardized. Lifetimes tweaked. Verbosity increased. Windows
support added. Crossplatform added. No-platform added. Performance
increased.


1.1

Summary: Cleanups

* The std::fs module has been expanded to expand the set of functionality exposed:

    DirEntry now supports optimizations like file_type and metadata
    which don't incur a syscall on some platforms.

    A symlink_metadata function has been added.

    The fs::Metadata structure now lowers to its OS counterpart,
    providing access to all underlying information.


* The str::split_whitespace method splits a string on unicode
  whitespace boundaries.

* The Default implementation for Arc no longer requires Sync + Send.

* The Iterator methods count, nth, and last have been overridden for
  slices to have O(1) performance instead of O(n).

* In accordance with Rust's policy on arithmetic overflow abs now
  panics on overflow when debug assertions are enabled.

1.2

Summary:

Summary: DSTs land; wrapped arithmetic, better unicode.

* Dynamically-sized-type coercions allow smart pointer types like Rc
  to contain types without a fixed size, arrays and trait objects,
  finally enabling use of Rc<[T]> and completing the implementation of
  DST.

* The to_uppercase and to_lowercase methods on char now do unicode
  case mapping, which is a previously-planned change in behavior and
  considered a bugfix.

*  mem::align_of now specifies the minimum alignment for T, which is
   usually the alignment programs are interested in, and the same
   value reported by clang's alignof. mem::min_align_of is
   deprecated. This is not known to break real code.

* Patterns with ref mut now correctly invoke DerefMut when matching
  against dereferencable values.

* The Extend trait, which grows a collection from an iterator, is
  implemented over iterators of references, for String, Vec,
  LinkedList, VecDeque, EnumSet, BinaryHeap, VecMap, BTreeSet and
  BTreeMap. RFC.

* The matches and rmatches methods on str return iterators over
  substring matches.

* A number of methods for wrapping arithmetic are added to the
  integral types, wrapping_div, wrapping_rem, wrapping_neg,
  wrapping_shl, wrapping_shr. These are in addition to the existing
  wrapping_add, wrapping_sub, and wrapping_mul methods, and
  alternatives to the Wrapping type.. It is illegal for the default
  arithmetic operations in Rust to overflow; the desire to wrap must
  be explicit.

* fmt::Formatter implements fmt::Write, a fmt-specific trait for
  writing data to formatted strings, similar to io::Write.


1.3

Summary:

Lifetime defaults change; Duration API lands; FFI interface improved.

* The new object lifetime defaults have been turned on after a cycle
  of warnings about the change. Now types like &'a Box<Trait> (or &'a
  Rc<Trait>, etc) will change from being interpreted as &'a
  Box<Trait+'a> to &'a Box<Trait+'static>.

* The Duration API, has been stabilized. This basic unit of
  timekeeping is employed by other std APIs, as well as out-of-tree
  time crates.

* The behavior of size_of_val and align_of_val is more sane for
  dynamically sized types. Code that relied on the previous behavior
  is thought to be broken.

* Semicolons may now follow types and paths in macros.

* 'static variables may now be recursive.

* Box<str> and Box<[T]> both implement Clone.

* The owned C string, CString, implements Borrow and the borrowed C
  string, CStr, implements ToOwned. The two of these allow C strings
  to be borrowed and cloned in generic code.

* The Hash trait offers the default method, hash_slice, which is
  overridden and optimized by the implementations for scalars.

* The connect method on slices is deprecated, replaced by the new join
  method (note that both of these are on the unstable SliceConcatExt
  trait, but through the magic of the prelude are available to stable
  code anyway).

* DerefMut is implemented for String.

* process::Child gained the id method, which returns a u32
  representing the platform-specific process identifier.

* Porting Rust on Windows from the GNU toolchain to MSVC continues (1,
  2, 3, 4). It is still not recommended for use in 1.3, though should
  be fully-functional in the 64-bit 1.4 beta.

* The unused_mut, unconditional_recursion, improper_ctypes, and
  negate_unsigned lints are more strict.


1.4

Summary: Windows support; 'static lifetimes changed; use aliases; API
stablizations; fixed-size arrays are borrowable; improved FFI.

* Windows builds targeting the 64-bit MSVC ABI and linker (instead of
  GNU) are now supported and recommended for use.

* The str::lines and BufRead::lines iterators treat \r\n as line
  breaks in addition to \n.

* Loans of 'static lifetime extend to the end of a function.

* str::parse no longer introduces avoidable rounding error when
  parsing floating point numbers. Together with earlier changes to
  float formatting/output, "round trips" like f.to_string().parse()
  now preserve the value of f exactly. Additionally, leading plus
  signs are now accepted.

* use statements that import multiple items can now rename them, as in
  use foo::{bar as kitten, baz as puppy}.

* API Stabilizations, lots.

* Some APIs were deprecated: BTreeMap::with_b, BTreeSet::with_b,
  Option::as_mut_slice, Option::as_slice, Result::as_mut_slice,
  Result::as_slice, f32::from_str_radix, f64::from_str_radix.

* Borrow and BorrowMut are implemented for fixed-size arrays.

* extern fns with the "Rust" and "C" ABIs implement common traits
  including Eq, Ord, Debug, Hash.


1.5

Summary: API stablization. Numeric parsing improved; (A)rc<T>
improved.

* Such API stabilization, much wow.

* Implementations of AsRef and AsMut were added to Box, Rc, and
  Arc. Because these smart pointer types implement Deref, this causes
  breakage in cases where the interior type contains methods of the
  same name.

* Arc<T> and Rc<T> are covariant with respect to T instead of invariant.

* The parse method accepts a leading "+" when parsing integers.

* There are now From conversions between floating point types where
  the conversions are lossless.

* Thera are now From conversions between integer types where the
  conversions are lossless.

* AsMut is implemented for Vec.

1.6

Summary: Happy embedded developers.

* The #![no_std] attribute causes a crate to not be linked to the
  standard library, but only the core library, as described in
  RFC 1184. The core library defines common types and traits but has
  no platform dependencies whatsoever, and is the basis for Rust
  software in environments that cannot support a full port of the
  standard library, such as operating systems. Most of the core
  library is now stable.

* so much stable api

* The core library is stable, as are most of its APIs.

1.7

Summary: Fixes to lifetimes; API rename and stablizations; perf
improvements.

* Soundness fix relating to lifetimes and projections.

* API renaming.

* BTreeSet and its iterators, Iter, IntoIter, and Range are covariant
over their contained type.

* LinkedList and its iterators, Iter and IntoIter are covariant over
  their contained type.

* Perf improvements

1.8

Summary: Overloaded assignments; more API fixes; cross-platform API
deprecations; NOX set on stacks; perf improvements.

* Rust supports overloading of compound assignment statements like +=
  by implementing the (etc) traits

* Empty structs can be defined with braces, as in struct Foo { }, in
addition to the non-braced form, struct Foo;.

* Renamed/stabilized APIs.

* The Unix-specific raw modules, which contain a number of redefined C
  types are deprecated.

* btree_set::{IntoIter, Iter, Range} are covariant.

* Avoid quadratic growth in function size due to cleanups.

* 32-bit MSVC builds finally implement unwinding. i686-pc-windows-msvc
  is now considered a tier-1 platform.

* Executable stacks are disabled on Linux and BSD.

* The Rust Project now publishes binary releases of the standard
  library for a number of tier-2 targets:
  armv7-unknown-linux-gnueabihf, powerpc-unknown-linux-gnu,
  powerpc64-unknown-linux-gnu, powerpc64le-unknown-linux-gnu
  x86_64-rumprun-netbsd. These can be installed with tools such as
  multirust.

* It is no longer possible to match on empty enum variants using the
Variant(..) syntax. This has been a warning since 1.6.

* On Unix, stack overflow triggers a runtime abort instead of a SIGSEGV.

* Command::spawn and its equivalents return an error if any of its
  command-line arguments contain interior NULs.
