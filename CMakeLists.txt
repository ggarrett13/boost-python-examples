cmake_minimum_required(VERSION 2.8.3)

# TODO: Allow for choice between python 3.6 and python 2.7 for compilation.
FIND_PACKAGE(PythonInterp 3 REQUIRED)

# TODO: Boost component for python must only contain major. Modify and push
# suggestion on github.
#
#set(BOOST_LIBRARYDIR ~/Software/boost_1_61_0/stage/lib/)
#set(BOOST_ROOT ~/Software/boost_1_61_0/)
#find_package(Boost 1.55.0
#        COMPONENTS thread date_time system unit_test_framework filesystem regex python3 REQUIRED)

#set(PYTHON_LIBRARY )
#
#PYTHON_VERSION=`python -c "import sys;t='{v[0]}.{v[1]}'.format(v=list(sys.version_info[:2]));sys.stdout.write(t)";`
#PYTHON_LIBRARY=/usr/local/Frameworks/Python.framework/Versions/$PYTHON_VERSION/lib/libpython$PYTHON_VERSION.dylib
#PYTHON_INCLUDE_DIR=/usr/local/Frameworks/Python.framework/Versions/$PYTHON_VERSION/Headers/
#cmake -DPYTHON_LIBRARY=$PYTHON_LIBRARY -DPYTHON_INCLUDE_DIR=$PYTHON_INCLUDE_DIR -DCMAKE_BUILD_TYPE=DEBUG .. && make && make test

#set(DPYTHON_LIBRARY /usr/lib/x86_64-linux-gnu/libpython3.6m.so)
#set(DPYTHON_INCLUDE_DIR /usr/include/python3.6m)

if (PYTHONINTERP_FOUND)
  if (UNIX AND NOT APPLE)
    if (PYTHON_VERSION_MAJOR EQUAL 3)
        # NOTE: As far as is known, building boost.python3 provides the lib files
        # in the form of python3 and not python3.6 as is suggested by original
        # code.
        FIND_PACKAGE(Boost COMPONENTS python${PYTHON_VERSION_MAJOR})
        FIND_PACKAGE(PythonInterp 3)
        FIND_PACKAGE(PythonLibs 3 REQUIRED)
    else()
        FIND_PACKAGE(Boost COMPONENTS python)
        FIND_PACKAGE(PythonInterp)
        FIND_PACKAGE(PythonLibs REQUIRED)
    endif()
  else()
    if (PYTHON_VERSION_MAJOR EQUAL 3)
        FIND_PACKAGE(Boost COMPONENTS python${PYTHON_VERSION_MAJOR}${PYTHON_VERSION_MINOR})
        FIND_PACKAGE(PythonInterp 3)
        FIND_PACKAGE(PythonLibs 3 REQUIRED)
    else()
        FIND_PACKAGE(Boost COMPONENTS python${PYTHON_VERSION_MAJOR}${PYTHON_VERSION_MINOR})
        FIND_PACKAGE(PythonInterp)
        FIND_PACKAGE(PythonLibs REQUIRED)
    endif()
  endif()
else()
    message("Python not found")
endif()

message(STATUS "PYTHON_LIBRARIES = ${PYTHON_LIBRARIES}")
message(STATUS "PYTHON_EXECUTABLE = ${PYTHON_EXECUTABLE}")
message(STATUS "PYTHON_INCLUDE_DIRS = ${PYTHON_INCLUDE_DIRS}")
message(STATUS "Boost_LIBRARIES = ${Boost_LIBRARIES}")
#include(tudatLinkLibraries)

ENABLE_TESTING()
INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS} ${PYTHON_INCLUDE_DIRS})
LINK_LIBRARIES(${Boost_LIBRARIES} ${PYTHON_LIBRARIES} ${}) # Deprecated but so convenient!

ADD_SUBDIRECTORY(01-HelloWorld)
ADD_SUBDIRECTORY(02-ExposingClasses)
ADD_SUBDIRECTORY(03-Constructors)
ADD_SUBDIRECTORY(04-ClassMembers)
ADD_SUBDIRECTORY(05-Inheritance)
ADD_SUBDIRECTORY(06-VirtualFunctionsInPython)
ADD_SUBDIRECTORY(07-Operators)
ADD_SUBDIRECTORY(08-CallPolicies)
ADD_SUBDIRECTORY(09-Overloading)
ADD_SUBDIRECTORY(10-Embedding)
ADD_SUBDIRECTORY(11-Iterators)
ADD_SUBDIRECTORY(12-Exceptions)
ADD_SUBDIRECTORY(13-AutoInstantiation)
