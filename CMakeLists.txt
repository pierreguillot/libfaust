## Create Faust Lib
message(STATUS "Faust Library")

## For Windows use static runtime instead of default dynamic runtime
if(MSVC)
  if(USE_STATIC_RUNTIME)
    set(CMAKE_CXX_FLAGS_RELEASE "/MT"  CACHE STRING  "FORCE CXX FLAGS" FORCE)
    set(CMAKE_C_FLAGS_RELEASE "/MT"  CACHE STRING  "FORCE C FLAGS" FORCE)
    set(CMAKE_CXX_FLAGS_DEBUG "/MTd"  CACHE STRING  "FORCE CXX FLAGS" FORCE)
    set(CMAKE_C_FLAGS_DEBUG "/MTd"  CACHE STRING  "FORCE C FLAGS" FORCE)
  endif()
endif()

## Force the use of stdc++ instead of libc++
if(USE_STDLIB)
  set(FAUST_LIBS  "stdc++"  CACHE STRING  "FAUST LIBRARIES" FORCE)
endif()

## Hardcoded path to LLVM
set(USE_LLVM_CONFIG  "off"  CACHE STRING "LLVM CONFIG" FORCE)
set(USE_LLVM_CONFIG  "./../llvm/lib/cmake/llvm"  CACHE STRING "LLVM CONFIG" FORCE)

## Hardcoded targets for faust
set(INCLUDE_STATIC      ON  CACHE STRING  "Include static library"      FORCE)
set(INCLUDE_EXECUTABLE  OFF CACHE STRING  "Include runtime executable"  FORCE)
set(INCLUDE_DYNAMIC     OFF CACHE STRING  "Include dynamic library"     FORCE)
set(INCLUDE_OSC         OFF CACHE STRING  "Include Faust OSC library"   FORCE)
set(INCLUDE_HTTP        OFF CACHE STRING  "Include Faust HTTPD library" FORCE)

## Hardcoded backends for faust
set(ASMJS_BACKEND  OFF                            CACHE STRING  "Include ASMJS backend"       FORCE)
set(C_BACKEND      OFF                            CACHE STRING  "Include C backend"           FORCE)
set(CPP_BACKEND    OFF                            CACHE STRING  "Include CPP backend"         FORCE)
set(FIR_BACKEND    OFF                            CACHE STRING  "Include FIR backend"         FORCE)
set(INTERP_BACKEND OFF                            CACHE STRING  "Include INTERPRETER backend" FORCE)
set(JAVA_BACKEND   OFF                            CACHE STRING  "Include JAVA backend"        FORCE)
set(JS_BACKEND     OFF                            CACHE STRING  "Include JAVASCRIPT backend"  FORCE)
set(LLVM_BACKEND   COMPILER STATIC DYNAMIC        CACHE STRING  "Include LLVM backend"        FORCE)
set(OLDCPP_BACKEND OFF                            CACHE STRING  "Include old CPP backend"     FORCE)
set(RUST_BACKEND   OFF                            CACHE STRING  "Include RUST backend"        FORCE)
set(WASM_BACKEND   OFF                            CACHE STRING  "Include WASM backend"        FORCE)

## Call the faust cmakelist.txt
add_subdirectory(./faust/build EXCLUDE_FROM_ALL)
