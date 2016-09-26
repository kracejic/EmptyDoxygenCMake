# Empty doxygen CMake subproject


## Use in your project

~~~
cd yourproject
git clone EmptyDoxygenCMake doc
rm -rf doc/.git
~~~

Edit `doc/doxygen/Doxyfile.in` *INPUT* and *STRIP_FROM_PATH* to fit your
project. Also you can change where the documentation will be installed in
`doc/CMakeLists.txt` in the first *#TODO* section. 

To your main CMakeLists.txt you want to add `add_subdirectory(doc)` in order to
include documentation build.

## Standalone doc builder

Sometimes you just need to build docs from some pile of sources, and here can
this repository help you. You may not even have a CMake Project setuped yet, in
this case you need to uncomment code after second *#TODO* section in
`doc/CMakeLists.txt` and documentation will act as a standalone CMake project.

## Targets

You can use *doc* target to build and *install* target to install the
documentation.

# Used VARIABLES

In your main CMakeLists.txt you might want to setup also these:

~~~
project (ProjectName)
set(MAJOR_VERSION 1 CACHE STRING "major version" FORCE)
set(MINOR_VERSION 0 CACHE STRING "minor version" FORCE)
set(PATCH_VERSION 0 CACHE STRING "patch version" FORCE)
~~~



