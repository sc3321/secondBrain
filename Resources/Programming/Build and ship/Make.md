## 1 · Source reality

1. **You edit code** – `.cpp/.h` files live in `src/` & `include/`.
    
2. **You describe the build** – one `CMakeLists.txt` (or several, one per directory).
    



`add_library(core STATIC core.cpp)` 
`target_compile_features(core PUBLIC cxx_std_23)    # “needs C++-23”` 
`target_link_libraries(core PUBLIC Threads::Threads) add_executable(app main.cpp) target_link_libraries(app PRIVATE core)`

_Key idea –_ you express **intent**, not flags:  
_“This target needs the C++-23 language & pthreads.”_  
CMake will translate that intent into the correct switches later.

---

## 2 · `cmake -S . -B build` ⟶ **Configure + Generate**

|What CMake does (chronological)|Why it gives portability|
|---|---|
|Detects a compiler → sets `CMAKE_CXX_COMPILER_ID` (`GNU`, `Clang`, `MSVC` …)|Knows which mapping tables to load.|
|Loads platform/compiler modules, e.g. `Compiler/GNU.cmake`, `Platform/Windows-GNU.cmake`|These tables say: “`cxx_std_23` ⇒ `-std=c++23` on GCC, `/std:c++23` on MSVC”, “`Threads::Threads` ⇒ `-pthread` on POSIX, nothing on Windows”…|
|Populates **`CMakeCache.txt`** with all discovered paths, versions, feature tests|Re-running later re-uses the cache, so configure is fast and reproducible.|
|Writes a low-level build script (your chosen **generator**) into `build/`|On Linux that might be a `Makefile`; with `-G Ninja` it becomes `build.ninja`; on Windows it could be a `.vcxproj`.|

> **After this step CMake exits.**  
> The rest of the work is done by the backend it just wrote.

---

## 3 · `cmake --build build -j` ⟶ **Backend executes graph**

_If you generated Makefiles the backend is **GNU make**, if you used Ninja it’s **ninja**, etc._

Backend algorithm (same for both):

1. Read the dependency graph from the generated file.
    
2. For each requested top-level target (`app`) **walk backwards** and check timestamps.
    
3. Re-run a recipe if the target is missing _or_ older than any dependency.
    
4. Parallelise independent commands (`-j`) for speed.
    

Concrete example of one generated Makefile rule:

`src/core.cpp.o: src/core.cpp include/core.hpp`     
`/usr/bin/g++ -std=c++23 -pthread -c src/core.cpp -o src/core.cpp.o`

Backend itself does **no** C++ understanding; it’s just supervising shell commands.

---

## 4 · Compiler & Linker – the real builders

_Issued by the backend, in order:_

1. **Compile phase** – each `.cpp` → `.o` (machine code fragment, unresolved symbols allowed).
    
2. **Link phase** – collect all `.o` + libs → **executable / shared library**; resolve symbols, lay out final binary.
    

If you enabled `VERBOSE=1`, you can watch every command line exactly as executed.

---

## 5 · Typical daily workflow (chronological)

1. `Edit code .................................... (src/*.cpp, *.hpp)`
2. `cmake --build build -j ....................... Backend recompiles what changed`
3. `./build/app .................................. Run / test`

`↳ added new target or changed flags?`
   `cmake -S . -B build ........................... Regenerate backend`


## 6 · Where things go wrong & which layer to blame

|Symptom|Layer to inspect|Quick fix|
|---|---|---|
|New file not compiled|_CMake configuration_|Re-run configure; ensure file is in `add_library/ add_executable`.|
|Wrong flag (e.g. `-O0` instead of `-O3`)|_CMake target properties_|Check `target_compile_options` or cache variable; clear `CMakeCache.txt` if stale.|
|File rebuilds every time|_Backend / timestamps_|Clock skew, or generated header’s timestamp always newer.|
|“Undefined reference …”|_Link step_|Wrong order in `target_link_libraries`; static libs must follow objects.|

---

## 7 · Pocket Commands to remember

`cmake -G Ninja -S . -B build      # use Ninja for faster builds cmake --build build -- VERBOSE=1  # show every compile/link command ccmake build/                     # edit cache in a TUI (Unix) ninja -t graph | dot -Tpng > g.png  # visualise dependency graph`

---

## 8 · Mantra

> **CMake describes.  
> Backend checks & runs.  
> Compiler produces.**

If something breaks, ask: _Am I debugging the description, the execution plan, or the actual compilation?_  
That keeps the mental stack clear for years to come.