find_package(Gradle 2.2)

set (GRADLE_WORKDIR ${CMAKE_CURRENT_BINARY_DIR}/.gradle)
set (SYSLOG_DEPS_DIR ${PROJECT_BINARY_DIR}/modules/java)

if (GRADLE_FOUND)
  OPTION(ENABLE_GRADLE "Enable Java modules" ON)
else()
  OPTION(ENABLE_GRADLE "Enable Java modules" OFF)
endif()

if (ENABLE_GRADLE)
  add_custom_target(BuildJavaModules ALL
                    COMMAND ${GRADLE_EXECUTABLE} --project-cache-dir ${GRADLE_WORKDIR} -g ${GRADLE_WORKDIR} -p ${CMAKE_CURRENT_SOURCE_DIR} -PsyslogBuildDir=${CMAKE_CURRENT_BINARY_DIR} -PsyslogDepsDir=${SYSLOG_DEPS_DIR} build copyJars
  )
  add_dependencies(BuildJavaModules mod-java)

  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/built-jars/ DESTINATION lib/syslog-ng/java-modules)
  set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES ".gradle")
  set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "built-jars")
endif()
