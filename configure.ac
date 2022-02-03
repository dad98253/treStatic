#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([treStatic], [2.1], [w7og@yahoo.com])
AC_CONFIG_SRCDIR([tre.h])
AC_CONFIG_AUX_DIR(utils)
AC_CANONICAL_TARGET
AM_GNU_GETTEXT_VERSION(0.17)
LT_INIT([disable-shared])
#AC_PROG_CC([gcc cl cc])
AM_INIT_AUTOMAKE

# Checks for programs.
#AC_PROG_CXX
#AC_PROG_AWK
AC_PROG_CC
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
#AC_PROG_RANLIB

tre_version_1=`echo $PACKAGE_VERSION | cut -d . -f 1`
tre_version_2=`echo $PACKAGE_VERSION | cut -d . -f 2`
tre_version_3=`echo $PACKAGE_VERSION | cut -d . -f 3`
tre_version=$tre_version_1.$tre_version_2.$tre_version_3
AC_DEFINE_UNQUOTED(TRE_VERSION,  "$tre_version",  [ TRE version string.  ])
AC_DEFINE_UNQUOTED(TRE_VERSION_1, $tre_version_1, [ TRE version level 1. ])
AC_DEFINE_UNQUOTED(TRE_VERSION_2, $tre_version_2, [ TRE version level 2. ])
AC_DEFINE_UNQUOTED(TRE_VERSION_3, $tre_version_3, [ TRE version level 3. ])
AC_SUBST(TRE_VERSION, $tre_version)

# Options
AC_ARG_ENABLE(debug,
  AC_HELP_STRING(
    [--enable-debug],
    [enable development-time debugging @<:@default=disabled@:>@]),
  [ tre_debug="$enableval" ],
  [ tre_debug="no" ])
if test "$tre_debug" = "yes"; then
  AM_CONDITIONAL(TRE_DEBUG, true)
  AC_DEFINE(TRE_DEBUG, 1,
    [ Define if you want TRE to print debug messages to stdout. ])
else
  AM_CONDITIONAL(TRE_DEBUG, false)
  AC_DEFINE(NDEBUG, 1, [ Define if you want to disable debug assertions. ])
fi


AC_ARG_ENABLE(warnings,
  AC_HELP_STRING(
    [--disable-warnings],
    [disable C compiler warning messages @<:@default=enabled@:>@]),
  [ tre_cc_warnings="$enableval" ],
  [ tre_cc_warnings="yes" ])
if test "$tre_cc_warnings" = "yes"; then
  VL_PROG_CC_WARNINGS()
fi

AC_ARG_ENABLE(approx,
  AC_HELP_STRING(
    [--disable-approx],
    [disable the approximate matching functionality @<:@default=enabled@:>@]),
  [ tre_approx="$enableval" ],
  [ tre_approx="yes" ])
if test "$tre_approx" = "yes"; then
  AC_DEFINE(TRE_APPROX, 1,
    [ Define if you want to enable approximate matching functionality. ])
  AM_CONDITIONAL(TRE_APPROX, true)
else
  AM_CONDITIONAL(TRE_APPROX, false)
fi

if test "$tre_approx" = "yes"; then
  AC_ARG_ENABLE(agrep,
    AC_HELP_STRING([--disable-agrep],
      [Do not build and install the agrep tool @<:@default=install@:>@]),
    [ tre_agrep="$enableval" ],
    [ tre_agrep="yes" ])
else
  tre_agrep="no (requires approximate matching)"
fi

# Checks for compiler characteristics.
AC_C_CONST
AC_C_INLINE

# Checks for libraries.

# Checks for header files.
AC_FUNC_ALLOCA
AC_CHECK_HEADERS([libintl.h malloc.h])
AC_DEFINE(_GNU_SOURCE, 1, [ Define to enable GNU extensions in glibc ])
AC_HEADER_STDC

AC_DEFINE([TRE_REGEX_T_FIELD], [value],[ Define to a dummy field in the regex_t struct ])

AC_CHECK_FUNCS([isascii isblank])

AC_CHECK_HEADERS([getopt.h])
AC_CHECK_FUNCS([getopt_long],,
  [ # FreeBSD has a "gnugetopt" library.
    AC_CHECK_LIB([gnugetopt], [getopt_long],
                 [ AC_DEFINE([HAVE_GETOPT_LONG]) ]) ])

# Check whether wide character support should be enabled.
AC_ARG_ENABLE(wchar,
  AC_HELP_STRING(
    [--disable-wchar],
    [disable the wide character (wchar_t) support   @<:@default=detect@:>@]),
  [ tre_enable_wchar="$enableval" ],
  [ tre_enable_wchar="detect" ])

# Check whether libutf8 location has been given.
AC_ARG_WITH(libutf8,
  AC_HELP_STRING(
    [--with-libutf8@<:@=DIR@:>@],
    [search for libutf8 from DIR/include and DIR/lib])
AC_HELP_STRING(
    [--without-libutf8],
    [do not use libutf8 @<:@default=use if needed@:>@]),
  [ if test "$with_val" = "no"; then
      tre_libutf8="no"
    else
      tre_libutf8="yes"
      tre_libutf8_dirs="$with_libutf8"
      tre_libutf8_libs="-lutf8"
    fi ],
  [ tre_libutf8="detect"
    tre_libutf8_dirs="none /usr /usr/local"
    tre_libutf8_libs="none -lutf8" ])



if test "$tre_enable_wchar" != "no"; then

  # Wide character support is requested.  Check if we need libutf8.
  old_libs="$LIBS"
  old_ldflags="$LDFLAGS"
  old_cppflags="$CPPFLAGS"
  if test "$tre_libutf8" != "no"; then
    AC_MSG_CHECKING([for libutf8])
    found="no"
    # Go through directories in $tre_libutf8_dirs for CPPFLAGS and LDFLAGS.
    for try_dir in $tre_libutf8_dirs; do
      if test "$try_dir" = "none"; then
        LDFLAGS="$old_ldflags"
        CPPFLAGS="$old_cppflags"
      else
        LDFLAGS="$old_ldflags -L$try_dir/lib"
        CPPFLAGS="$old_cppflags -I$try_dir/include"
      fi
      # Go through libs in $tre_libutf8_libs.
      for try_lib in $tre_libutf8_libs; do
        if test "$try_lib" = "none"; then
          LIBS="$old_libs"
        else
          LIBS="$try_lib $old_libs"
        fi
        # Check if mbrtowc or utf8_mbrtowc is available with the current
        # CPPFLAGS, LDFLAGS, and LIBS.
        AC_LINK_IFELSE([AC_LANG_CALL([],[mbrtowc])],[found="yes"])
        if test "$found" = "yes"; then
          break;
        fi
        AC_LINK_IFELSE([AC_LANG_CALL([],[utf8_mbrtowc])],[found="yes"])
        if test "$found" = "yes"; then
          break;
        fi
      done
      if test "$found" = "yes"; then
        break;
      fi
    done

    # Report results.
    if test "$found" = "yes"; then
      if test "$LIBS" = "$old_libs"; then
        AC_MSG_RESULT([not needed])
        tre_libutf8="no"
      else
        AC_MSG_RESULT([using from $try_dir/{lib,include}])
        tre_libutf8="yes"
      fi
    else
      LIBS="$old_libs"
      LDFLAGS="$old_ldflags"
      CPPFLAGS="$old_cppflags"
      if test "$tre_libutf8" = "detect"; then
        AC_MSG_RESULT([no])
        tre_libutf8="no"
      else
        # Fail if libutf8 was requested but was not found.
        AC_MSG_ERROR([not found])
      fi
    fi
  fi

  if test "$tre_libutf8" = "yes"; then
    # We do need libutf8, use the libutf8 headers.
    tre_wchar="yes"
    AC_CHECK_HEADERS([libutf8.h])
  else
    # Use the POSIX headers.
    AC_CHECK_HEADERS([wchar.h wctype.h])
  fi

else
  tre_wchar_reason="disabled with --disable-wchar"
  tre_wchar="no ($tre_wchar_reason)"
fi


tre_includes="
#ifdef HAVE_WCHAR_H
#include <wchar.h>
#endif /* HAVE_WCHAR_H */
#ifdef HAVE_WCTYPE_H
#include <wctype.h>
#endif /* HAVE_WCTYPE_H */
#ifdef HAVE_LIBUTF8_H
#include <libutf8.h>
#endif /* HAVE_LIBUTF8_H */
"

if test "$tre_enable_wchar" != "no"; then
  # We need wchar_t and WCHAR_MAX
  AC_CHECK_TYPES([wchar_t],
    [ tre_wchar="yes"
      AX_DECL_WCHAR_MAX ],
    [ tre_wchar_reason="wchar_t type not found"
      tre_wchar="no ($tre_wchar_reason)" ],
    [ $tre_includes ])

  if test "$tre_wchar" = "yes"; then
    # We need wint_t
    AC_CHECK_TYPES([wint_t],,
      [ tre_wchar_reason="wint_t type not found"
        tre_wchar="no ($tre_wchar_reason)" ],
      [ $tre_includes ])
  fi

  if test "$tre_wchar" = "yes"; then
    # We may need mbstate_t
    AC_CHECK_TYPES([mbstate_t],,,[ $tre_includes ])
  fi

  if test "$tre_wchar" = "yes"; then
    # We need either wcsrtombs (preferred) or wcstombs
    found="no"
    AX_CHECK_FUNCS_COMP([wcsrtombs wcstombs],[found="yes"; break],,
                        [$tre_includes])
    if test "$found" = "no"; then
      tre_wchar_reason="no wcsrtombs or wcstombs found"
      tre_wchar="no ($tre_wchar_reason)"
    fi
  fi

  if test "$tre_wchar" = "yes"; then
    # We need all these
    AX_CHECK_FUNCS_COMP([iswlower iswupper towlower towupper wcschr \
                         wcscpy wcslen wcsncpy],
      [ tre_wchar="yes" ],
      [ tre_wchar_reason="$ac_func not found"
        tre_wchar="no ($tre_wchar_reason)"
        break ],
      [ $tre_includes ])
  fi
fi


case $host in
  *-mingw*)
    # wcsrtombs and wcstombs don't seem to work at all on MinGW.
    if test "$tre_libutf8" != "yes"; then
      tre_wchar_reason="Not supported on MinGW"
      tre_wchar="no ($tre_wchar_reason)"
    fi
    ;;
esac

# Fail if wide character support was specifically requested but is
# not supported on this system.
if test "$tre_enable_wchar" = "yes"; then
  if test "$tre_wchar" != "yes"; then
    AC_MSG_ERROR([Cannot enable wide character support: $tre_wchar_reason])
  fi
fi

if test "$tre_wchar" = "yes"; then
  AC_DEFINE(TRE_WCHAR, 1,
    [ Define to enable wide character (wchar_t) support. ])

  # We make use of these if they exist.
  AX_CHECK_FUNCS_COMP([wctype iswctype],,,[$tre_includes])
  AX_CHECK_FUNCS_COMP([iswascii iswblank],,,[$tre_includes])
fi

# Check for multibyte character set support
AC_ARG_ENABLE(multibyte,
  AC_HELP_STRING(
    [--disable-multibyte],
    [disable multibyte character set support @<:@default=detect@:>@]),
  [ tre_enable_multibyte="$enableval" ],
  [ tre_enable_multibyte="detect" ])

# Wide character support is required for multibyte character set support
if test "$tre_wchar" != "yes"; then
  if test "$tre_enable_multibyte" = "yes"; then
    AC_MSG_ERROR([Cannot enable multibyte character support because wide \
character support is not enabled ($tre_wchar_reason)])
  fi
fi

if test "$tre_enable_multibyte" != "no"; then
  if test "$tre_wchar" != "yes"; then
    tre_multibyte="no (requires wide character support)"
  else
    found="no"
    # We need either mbrtowc (preferred) or mbtowc
    AX_CHECK_FUNCS_COMP([mbrtowc mbtowc],[found="yes"; break],,[$tre_includes])
    if test "$found" = "no"; then
      tre_mbs_reason="no mbrtowc or mbtowc found"
      tre_multibyte="no ($tre_mbs_reason)"
    else
      tre_multibyte="yes"
    fi
  fi
else
  tre_multibyte="no (disabled with --disable-multibyte)"
fi

# Fail if multibyte character set support was specifically requested but
# is not supported on this system.
if test "$tre_enable_multibyte" = "yes"; then
  if test "$tre_multibyte" != "yes"; then
    AC_MSG_ERROR([Cannot enable multibyte character set support: \
$tre_mbs_reason])
  fi
fi

if test "$tre_multibyte" = "yes"; then
  AM_CONDITIONAL(TRE_MULTIBYTE, true)
  AC_DEFINE(TRE_MULTIBYTE, 1,
    [ Define to enable multibyte character set support. ])
else
  AM_CONDITIONAL(TRE_MULTIBYTE, false)
fi

AC_SYS_LARGEFILE

AM_GNU_GETTEXT([external])
#AC_LIBTOOL_TAGS([])
AC_LIBTOOL_WIN32_DLL
AM_DISABLE_STATIC
AC_PROG_LIBTOOL


# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T

# Checks for library functions.
AC_CHECK_FUNCS([memchr memset strchr strerror strtol])

AC_CONFIG_HEADERS([config.h tre-config.h])
#AC_CONFIG_HEADERS([tre-config.h])
AC_CONFIG_FILES([
Makefile
])

AC_OUTPUT


# Print configuration summary

cat <<EOF


Configuration summary
=====================

TRE is now configured as follows:

* Compilation environment

  CC       = $CC
  CFLAGS   = $CFLAGS
  CPP      = $CPP
  CPPFLAGS = $CPPFLAGS
  LD       = $LD
  LDFLAGS  = $LDFLAGS
  LIBS     = $LIBS
  Use alloca():                       $tre_use_alloca

* TRE options

  Development-time debugging:         $tre_debug
  System regex ABI compatibility:     $tre_system_abi
  Wide character (wchar_t) support:   $tre_wchar
  Multibyte character set support:    $tre_multibyte
  Approximate matching support:       $tre_approx
  Build and install agrep:            $tre_agrep

EOF

