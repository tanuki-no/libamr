cmake_minimum_required( VERSION 4.0.0 FATAL_ERROR )

# Явно выбираем CLang
set ( CMAKE_C_COMPILER        "clang" )

# Название проекта
project( "libamr"
  VERSION	0.0.1.0
  DESCRIPTION	"3GPP TS 26.073 – ANSI-C code for the Adaptive Multi Rate (AMR) speech codec"
  HOMEPAGE_URL	"https://github.com/tanuki-no/libamr"
  LANGUAGES	C )

# ABI
SET ( ABIMajor		7 )
SET ( ABIMinor		5 )
SET ( ABIPatch		0 )
SET ( ABIVersion	${ABIMajor}.${ABIMinor}.${ABIPatch} )

# Папки проекта
set ( Project_Include_Dir           "${CMAKE_SOURCE_DIR}/include" )
set ( Project_Source_Dir            "${CMAKE_SOURCE_DIR}/src" )

# Выбрать максимально свежий стандарт C
list( FIND CMAKE_C_COMPILE_FEATURES "c_std_23" __C23_IS_SUPPORTED )
list( FIND CMAKE_C_COMPILE_FEATURES "c_std_17" __C17_IS_SUPPORTED )
list( FIND CMAKE_C_COMPILE_FEATURES "c_std_11" __C11_IS_SUPPORTED )
list( FIND CMAKE_C_COMPILE_FEATURES "c_std_99" __C99_IS_SUPPORTED )
if ( ${__C23_IS_SUPPORTED} GREATER -1 )
  message (STATUS "Будет использоваться стандарт С 2023 ...")
  set (CMAKE_C_FLAGS                "-std=c23 ${CMAKE_C_FLAGS}"               )
elseif ( ${__C17_IS_SUPPORTED} GREATER -1 )
  message (STATUS "Будет использоваться стандарт С 2017 ...")
  set (CMAKE_C_FLAGS                "-std=c17 ${CMAKE_C_FLAGS}"               )
elseif ( ${__C11_IS_SUPPORTED} GREATER -1 )
  message (STATUS "Будет использоваться стандарт С 2011 ...")
  set (CMAKE_C_FLAGS                "-std=c11 ${CMAKE_C_FLAGS}"               )
elseif ( ${__C99_IS_SUPPORTED} GREATER -1 )
  message (STATUS "Будет использоваться стандарт С 1999 ...")
  set (CMAKE_C_FLAGS                "-std=c99 ${CMAKE_C_FLAGS}"               )
else()
  message ( FATAL "Слишком старый компилятор!"                                )
endif()

# Устанавить флаги
set ( CMAKE_C_FLAGS                 "-Wall ${CMAKE_C_FLAGS}"                  )
set ( CMAKE_C_FLAGS                 "${CMAKE_C_FLAGS} -O3 "                   )
set ( CMAKE_C_FLAGS                 "${CMAKE_C_FLAGS} -ffreestanding "        )

add_subdirectory ( src )

# Tests
if (BUILD_TESTS)

  enable_testing()
  include (CTest)
  add_subdirectory ( test )

endif (BUILD_TESTS)

# Конец файла
