# deps

shared dependencies for Synthizer and its components

This submodule contains a CMakeLists.txt which builds everything that needs building, and uses CMake FetchContent to
allow itself to be used in either Synthizer or Synthcore by ensuring that it only builds once.

We could fetch these individually if we wanted, but it is helpful to IDEs to have them available, and when used as a
submodule we avoid fetching every time someone has to regenerate the CMake scripts.  Plus, this setup makes them easily
vendored, and thus allows builds which don't require network access.

Specifically, the CMakeLists.txt at the root of this project does:

```
FetchContent_declare(...)
```

parent repositories are responsible for the `FetchContent_MakeAvailable` calls, in order to only include what they use
when they need it.

In many cases, we add our own CMakeLists.txt file to the deps/third_party folder to e.g. provide an interface target.


The third-party dependencies are as follows. None require binary attribution: 

- boost_partial is a header-only subset of Boost extracted with
  [BCP](https://www.boost.org/doc/libs/1_80_0/tools/bcp/doc/html/index.html).
- [concurrentqueue](https://github.com/cameron314/concurrentqueue) is what it sounds like it is.
- cpp-11-on-multicore is a set of utilities from [here](https://github.com/preshing/cpp11-on-multicore) for, e.g.,
  semaphores.
- [dr_libs](https://github.com/mackron/dr_libs) are a set of audio-related libraries, primarily decoders for MP3, Wav,
  and Flac.
- [hedley](https://nemequ.github.io/hedley/_) is a library for abstracting over C++ compiler differences.
- [Miniaudio](https://github.com/mackron/miniaudio) is a library for audio device I/O.
- [pdqsort](https://github.com/orlp/pdqsort) is pattern-defeating quicksort, an unstable sorting algorithm that requires
  no additional memory.
- [WDL](https://www.cockos.com/wdl/) is an SDK from Cockos, the authors of Reaper, with various DSP-related utilities.
