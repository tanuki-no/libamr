# Сборка проекта

# Используем С
ENABLE_LANGUAGE ( C )

# Заголовочные файлы
include_directories(
    ${Project_Include_Dir}
    )

# Исходники
set(
    Sources
    "*.c"
    )


# Shared library
IF ( BUILD_SHARED_LIBS )

  ADD_LIBRARY (
    ${LibraryName} SHARED
    ${Sources}
    ${Public_Include} )

  SET_TARGET_PROPERTIES (
    ${LibraryName}		PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/bin"
    COMPILE_FLAGS		"${EggCxxFlags}"
    LINK_FLAGS			"${EggLdFlags}"
    VERSION			"${ABIVersion}"
    SOVERSION			"${ABIMajor}" )

ENDIF ()

# Static library
IF ( BUILD_STATIC_LIBS )

SET_TARGET_PROPERTIES (
  ${LibraryName}		PROPERTIES
  ARCHIVE_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/lib"
  LIBRARY_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/lib"
  RUNTIME_OUTPUT_DIRECTORY	"${CMAKE_BINARY_DIR}/bin"
  COMPILE_FLAGS			"${EggCxxFlags}" )

  ADD_LIBRARY (
    ${LibraryName}	STATIC
    ${Sources}
    ${PublicIncludes} )

ENDIF ()

# Install library
INSTALL (
  TARGETS     ${LibraryName} LIBRARY
  DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR} )

# Конец файла
