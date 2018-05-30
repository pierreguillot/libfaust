## Create Faust Lib
message(STATUS "Faust Library")

## For Windows use static runtime instead of default dynamic runtime
if(MSVC)
    set(CompilerFlags
        CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS CMAKE_C_FLAGS_DEBUG CMAKE_C_FLAGS_RELEASE)
    foreach(CompilerFlag ${CompilerFlags})
      string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
    endforeach()
endif()

## Save the llvm directory and change it for subdirectory
if(DEFINED LLVM_DIR)
  set(LLVM_DIR        "./../.${LLVM_DIR}")
endif()

## Hardcoded targets for faust
set(INCLUDE_STATIC      ON  CACHE STRING  "Include static library"      FORCE)
set(INCLUDE_EXECUTABLE  OFF CACHE STRING  "Include runtime executable"  FORCE)
set(INCLUDE_DYNAMIC     OFF CACHE STRING  "Include dynamic library"     FORCE)
set(INCLUDE_OSC         OFF CACHE STRING  "Include Faust OSC library"   FORCE)
set(INCLUDE_HTTP        OFF CACHE STRING  "Include Faust HTTPD library" FORCE)

## Hardcoded backends for faust
set(ASMJS_BACKEND  COMPILER STATIC DYNAMIC        CACHE STRING  "Include ASMJS backend" FORCE)
set(C_BACKEND      COMPILER STATIC DYNAMIC        CACHE STRING  "Include C backend"         FORCE)
set(CPP_BACKEND    COMPILER STATIC DYNAMIC        CACHE STRING  "Include CPP backend"       FORCE)
set(FIR_BACKEND    COMPILER STATIC DYNAMIC        CACHE STRING  "Include FIR backend"       FORCE)
set(INTERP_BACKEND COMPILER STATIC DYNAMIC        CACHE STRING  "Include INTERPRETER backend" FORCE)
set(JAVA_BACKEND   COMPILER STATIC DYNAMIC        CACHE STRING  "Include JAVA backend"      FORCE)
set(JS_BACKEND     COMPILER STATIC DYNAMIC        CACHE STRING  "Include JAVASCRIPT backend" FORCE)
set(LLVM_BACKEND   COMPILER STATIC DYNAMIC        CACHE STRING  "Include LLVM backend"      FORCE)
set(OLDCPP_BACKEND COMPILER STATIC DYNAMIC        CACHE STRING  "Include old CPP backend"   FORCE)
set(RUST_BACKEND   COMPILER STATIC DYNAMIC        CACHE STRING  "Include RUST backend"      FORCE)
set(WASM_BACKEND   COMPILER STATIC DYNAMIC        CACHE STRING  "Include WASM backend"  FORCE)

## Call the faust cmakelist.txt
add_subdirectory(./faust/build EXCLUDE_FROM_ALL)

if(MSVC)
    set_property(TARGET staticlib APPEND_STRING PROPERTY COMPILE_FLAGS " /EHsc ")
    set_property(TARGET staticlib APPEND_STRING PROPERTY COMPILE_FLAGS " /D WIN32 ")

    set(CompilerFlags
        CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS CMAKE_C_FLAGS_DEBUG CMAKE_C_FLAGS_RELEASE)
    foreach(CompilerFlag ${CompilerFlags})
        set(STATIC_LIB_CURRENT_FLAGS)
        get_target_property(STATIC_LIB_CURRENT_FLAGS staticlib ${CompilerFlags})
        string(REPLACE "/MD" "/MT" STATIC_LIB_${CompilerFlags} "${${STATIC_LIB_CURRENT_FLAGS}}")
        set_target_properties(staticlib PROPERTIES ${CompilerFlags} ${STATIC_LIB_CURRENT_FLAGS})
        message(STATUS "${CompilerFlags}: ${STATIC_LIB_CURRENT_FLAGS}")
    endforeach()
endif()