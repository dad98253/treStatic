#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.71])
AC_INIT([treStatic], [2.1], [w7og@yahoo.com])
AC_CONFIG_SRCDIR([tre.h])
AC_CONFIG_HEADERS([config.h])
LT_INIT([disable-shared])
AC_PROG_CC([gcc cl cc])
AM_INIT_AUTOMAKE

# Checks for programs.
AC_PROG_CXX
AC_PROG_AWK
AC_PROG_CC
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_RANLIB

# Checks for libraries.

# Checks for header files.
AC_FUNC_ALLOCA
AC_CHECK_HEADERS([libintl.h malloc.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T

# Checks for library functions.
AC_CHECK_FUNCS([memchr memset strchr strerror strtol])

#AC_CONFIG_HEADERS([config.h tre-config.h])
#AC_CONFIG_HEADERS([tre-config.h])
AC_CONFIG_FILES([
Makefile
])

AC_OUTPUT
