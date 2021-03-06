CMAKE_MINIMUM_REQUIRED(VERSION 3.1)

SET(PROJECT_NAME hyq-rbprm)
SET(PROJECT_DESCRIPTION "Database for hyq robot using hpp-rbprm")
SET(PROJECT_URL "")
SET(CUSTOM_HEADER_DIR "hpp/hyq-rbprm")
SET(CXX_DISABLE_WERROR true)
SET(PROJECT_USE_CMAKE_EXPORT TRUE)

INCLUDE(cmake/hpp.cmake)
INCLUDE(cmake/python.cmake)

COMPUTE_PROJECT_ARGS(PROJECT_ARGS LANGUAGES CXX)
PROJECT(${PROJECT_NAME} ${PROJECT_ARGS})

FINDPYTHON()
ADD_PROJECT_DEPENDENCY(example-robot-data REQUIRED)

ADD_LIBRARY(${PROJECT_NAME} INTERFACE)
INSTALL(TARGETS ${PROJECT_NAME} EXPORT ${TARGETS_EXPORT_NAME} DESTINATION lib)

install(DIRECTORY data/ DESTINATION share/${PROJECT_NAME})

STRING(REGEX REPLACE "-" "_" PY_NAME ${PROJECT_NAME})

SET(${PROJECT_NAME}_PYTHON_FILES
  __init__.py
  hyq.py
  hyq_abstract.py
  hyq_contact6D.py
  )

FOREACH(SOURCE ${${PROJECT_NAME}_PYTHON_FILES})
  PYTHON_INSTALL_ON_SITE(${PY_NAME} ${SOURCE})
ENDFOREACH(SOURCE ${${PROJECT_NAME}_PYTHON_SOURCES})

INSTALL(FILES package.xml DESTINATION share/${PROJECT_NAME})
