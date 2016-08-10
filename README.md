# boost
cmake based plugable static compiled boost library
---

have you been tired of adding include directories and link search path over and over again?
have you been tired of explaining how to compile your boost dependended project to your college?

How here is CMake based boost and it's statically linked into your project.

how to use it?

simple

```
$ git submodule add https://github.com/microcai/boost.git third_party/boost
```

then add 

```
add_subdirectory(third_party/boost)
```

to your project CMakeLists.txt

then use

```
target_link_libraries("your target" ${BOOST_LIBRARIES} )
```
