## Process this file with automake to produce Makefile.in

SUBDIRS = utils

noinst_LIBRARIES = libtre.a
#libtre_a_LDFLAGS = -no-undefined -version-info 5:0:0 $(LDFLAGS)
libtre_a_LIBADD = $(LIBINTL)

noinst_HEADERS =                \
        tre-ast.h               \
        tre-compile.h           \
        tre-internal.h          \
        tre-match-utils.h       \
        tre-mem.h               \
        tre-parse.h             \
        tre-stack.h             \
        xmalloc.h

libtre_a_SOURCES =             \
        tre-ast.c               \
        tre-compile.c           \
        tre-match-backtrack.c   \
        tre-match-parallel.c    \
        tre-mem.c               \
        tre-parse.c             \
        tre-stack.c             \
        regcomp.c               \
        regexec.c               \
        regerror.c


ACLOCAL_AMFLAGS = -I m4

AC_CONFIG_AUX_DIR = utils

@GENERATE_CHANGELOG_RULES@
  dist-hook: dist-ChangeLog

distclean-local:
	@rm config.status configure config.log
	@rm Makefile
	@rm -r autom4te.cache/
	@rm aclocal.m4
	@rm Makefile.in
