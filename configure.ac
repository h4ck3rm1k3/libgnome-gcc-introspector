dnl *******************************************
dnl *** Initialize automake and set version ***
dnl *******************************************

AC_PREREQ(2.63)
AC_INIT([libgnomegccintrospector],[0.1],[http://introspector.sf.net])
AC_CONFIG_SRCDIR([libgccintrospector-0.1.pc])
AC_CONFIG_MACRO_DIR([m4])
AC_PROG_LIBTOOL
AM_INIT_AUTOMAKE([foreign 1.9])
m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])],)

AC_PROG_MAKE_SET

GCCINTROSPECTOR_API_VERSION=0.1
AC_SUBST(GCCINTROSPECTOR_API_VERSION)

# Increment on interface addition. Reset on removal.
GCCINTROSPECTOR_AGE=3

# Increment on interface add, remove, or change.
GCCINTROSPECTOR_CURRENT=4

# Increment on source change. Reset when CURRENT changes.
GCCINTROSPECTOR_REVISION=0

AC_SUBST(GCCINTROSPECTOR_CURRENT)
AC_SUBST(GCCINTROSPECTOR_REVISION)
AC_SUBST(GCCINTROSPECTOR_AGE)

dnl ***************************
dnl *** Set debugging flags ***
dnl ***************************

debug_default=minimum

# Declare --enable-* args and collect ac_help strings
AC_ARG_ENABLE(debug, 
	      AS_HELP_STRING([--enable-debug=[no/minimum/yes]],
			     [turn on debugging (default=$debug_default)]),,
	      enable_debug=$debug_default)

# Set the debug flags
if test "x$enable_debug" = "xyes"; then
	test "$cflags_set" = set || CFLAGS="$CFLAGS -g"
	GCCINTROSPECTOR_DEBUG_FLAGS="-DG_ENABLE_DEBUG"
else
	if test "x$enable_debug" = "xno"; then
		GCCINTROSPECTOR_DEBUG_FLAGS="-DG_DISABLE_ASSERT -DG_DISABLE_CHECKS"
	fi
fi

AC_SUBST(GCCINTROSPECTOR_DEBUG_FLAGS)

# Set the maintainer flags
if test -d .git; then
	GCCINTROSPECTOR_MAINTAINER_FLAGS="-DG_DISABLE_DEPRECATED"
fi
AC_SUBST(GCCINTROSPECTOR_MAINTAINER_FLAGS)

dnl ***************************
dnl *** Checks for programs ***
dnl ***************************

AC_PROG_CC
AC_PROG_INSTALL

dnl ***********************
dnl *** Checks for glib ***
dnl ***********************

AM_PATH_GLIB_2_0(2.21.3,,,gobject gthread gio)
if test "$GLIB_LIBS" = ""; then
   AC_MSG_ERROR(GLIB 2.21.3 or later is required to build libgccintrospector)
fi
GLIB_CFLAGS="$GLIB_CFLAGS -DG_DISABLE_SINGLE_INCLUDES"


dnl *******************
dnl *** Misc checks ***
dnl *******************


dnl *********************
dnl *** GNOME support ***
dnl *********************
dnl AM_CONDITIONAL(BUILD_LIBGCCINTROSPECTOR_GNOME, test $with_gnome != no)


AC_SUBST(HAVE_GNOME)
AC_SUBST(GCONF_CFLAGS)
AC_SUBST(GCONF_LIBS)



dnl ***************
dnl *** gtk-doc ***
dnl ***************
GTK_DOC_CHECK([1.10])

dnl *****************************
dnl *** gobject-introspection ***
dnl *****************************
GOBJECT_INTROSPECTION_CHECK([0.6.7])

dnl *************************************
dnl *** Warnings to show if using GCC ***
dnl *************************************

AC_ARG_ENABLE(more-warnings,
	      AS_HELP_STRING([--disable-more-warnings], [Inhibit compiler warnings]),
	      set_more_warnings=no)

if test "$GCC" = "yes" -a "$set_more_warnings" != "no"; then
	CFLAGS="$CFLAGS \
		-Wall -Wstrict-prototypes -Wmissing-declarations \
		-Wmissing-prototypes -Wnested-externs -Wpointer-arith \
		-Wdeclaration-after-statement -Wformat=2 -Winit-self \
		-Waggregate-return -Wmissing-format-attribute"

	for option in -Wmissing-include-dirs -Wundef; do
		SAVE_CFLAGS="$CFLAGS"
		CFLAGS="$CFLAGS $option"
		AC_MSG_CHECKING([whether gcc understands $option])
		AC_COMPILE_IFELSE([AC_LANG_PROGRAM([], [])],
				  [has_option=yes],
				  [has_option=no])
		AC_MSG_RESULT($has_option)
		if test $has_option = no; then
			CFLAGS="$SAVE_CFLAGS"
		fi
	done
fi

dnl *************************
dnl *** Output Everything ***
dnl *************************

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([
	libgccintrospector-0.1.pc
	Makefile
	libgccintrospector/Makefile
	])
AC_OUTPUT
