## Process this file with automake to produce Makefile.in


INCLUDES = 				\
	-DG_LOG_DOMAIN=\"libgccintrospector\" 	\
	-I$(top_srcdir)			\
	$(GCCINTROSPECTOR_DEBUG_FLAGS)		\
	$(GCCINTROSPECTOR_MAINTAINER_FLAGS)	\
	$(GLIB_CFLAGS)			\
	$(XML_CFLAGS)			\
	$(GCONF_CFLAGS)			\
	$(LIBPROXY_CFLAGS)		\
	$(SQLITE_CFLAGS)		\
	$(GNOME_KEYRING_CFLAGS)		\
	$(LIBGCRYPT_CFLAGS)		\
	$(LIBGNUTLS_CFLAGS)

MARSHAL_GENERATED = gccintrospector-marshal.c gccintrospector-marshal.h
MKENUMS_GENERATED = gccintrospector-enum-types.c gccintrospector-enum-types.h

gccintrospector-marshal.h: gccintrospector-marshal.list
	$(AM_V_GEN) ( $(GLIB_GENMARSHAL) --prefix=gccintrospector_marshal $(srcdir)/gccintrospector-marshal.list --header > gccintrospector-marshal.tmp \
	&& mv gccintrospector-marshal.tmp gccintrospector-marshal.h ) \
	|| ( rm -f gccintrospector-marshal.tmp && exit 1 )

gccintrospector-marshal.c: gccintrospector-marshal.h
	$(AM_V_GEN) ( (echo '#include "gccintrospector-marshal.h"'; $(GLIB_GENMARSHAL) --prefix=gccintrospector_marshal $(srcdir)/gccintrospector-marshal.list --body) > gccintrospector-marshal.tmp \
	&& mv gccintrospector-marshal.tmp gccintrospector-marshal.c ) \
	|| ( rm -f gccintrospector-marshal.tmp && exit 1 )

gccintrospector-enum-types.h: $(gccintrospector_headers)
	$(AM_V_GEN) ( cd $(srcdir) && $(GLIB_MKENUMS) --template gccintrospector-enum-types.h.tmpl \
		$(gccintrospector_headers) ) > gccintrospector-enum-types.h.tmp \
	&& mv gccintrospector-enum-types.h.tmp gccintrospector-enum-types.h \
	|| rm -f gccintrospector-enum-type.h.tmp

gccintrospector-enum-types.c: $(libgnomegccintrospectorinclude_HEADERS)
	$(AM_V_GEN) ( cd $(srcdir) && $(GLIB_MKENUMS) --template gccintrospector-enum-types.c.tmpl \
		$(gccintrospector_headers) ) > gccintrospector-enum-types.c.tmp \
	&& mv gccintrospector-enum-types.c.tmp gccintrospector-enum-types.c \
	|| rm -f gccintrospector-enum-type.c.tmp

BUILT_SOURCES = $(MARSHAL_GENERATED) $(MKENUMS_GENERATED)

CLEANFILES = $(MARSHAL_GENERATED) $(MKENUMS_GENERATED)

libgnomegccintrospectorincludedir = $(includedir)/libgnomegccintrospector-0.1/libgnomegccintrospector

gccintrospector_headers =			\
	gccintrospector.h			

libgnomegccintrospectorinclude_HEADERS =	\
	$(gccintrospector_headers)		\
	gccintrospector-enum-types.h

lib_LTLIBRARIES = libgnomegccintrospector-0.1.la

libgnomegccintrospector_0_1_la_LDFLAGS =	\
	-version-info $(GCCINTROSPECTOR_CURRENT):$(GCCINTROSPECTOR_REVISION):$(GCCINTROSPECTOR_AGE) -no-undefined

libgnomegccintrospector_0_1_la_LIBADD =			


libgnomegccintrospector_0_1_la_SOURCES =		\
	$(BUILT_SOURCES)		\
	gccintrospector-test.c			



libgnomegccintrospectorgnomeincludedir = $(includedir)/libgnomegccintrospector-gnome-0.1/libgnomegccintrospector

libgnomegccintrospectorgnomeinclude_HEADERS =	\
	gccintrospector-gnome.h		\
	gccintrospector-gnome-features.h

lib_LTLIBRARIES += libgnomegccintrospector-gnome-0.1.la

libgnomegccintrospector_gnome_0_1_la_LDFLAGS = $(libgnomegccintrospector_0_1_la_LDFLAGS)

libgnomegccintrospector_gnome_0_1_la_LIBADD =		\
	libgnomegccintrospector-0.1.la			\
	$(GLIB_LIBS)			\
	$(GCONF_LIBS)			\
	$(LIBPROXY_LIBS)		\
	$(SQLITE_LIBS)			\
	$(GNOME_KEYRING_LIBS)

libgnomegccintrospector_gnome_0_1_la_SOURCES =		\
	gccintrospector-gnome-features.c		


#
# Introspection support
#
include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ARGS = --add-include-path=.
INTROSPECTION_COMPILER_ARGS = --includedir=.


# Core library
gi_gccintrospector_files = \
	$(filter-out gccintrospector.h gccintrospector-enum-types.% gccintrospector-marshal.%,\
	   $(gccintrospector_headers) $(filter-out %.h, $(libgnomegccintrospector_0_1_la_SOURCES)))
gi_built_gccintrospector_files = gccintrospector-enum-types.h

Gccintrospector-0.1.gir: libgnomegccintrospector-0.1.la
Gccintrospector_0_1_gir_INCLUDES = Gio-2.0
Gccintrospector_0_1_gir_CFLAGS = $(INCLUDES)
Gccintrospector_0_1_gir_LIBS = libgnomegccintrospector-0.1.la
Gccintrospector_0_1_gir_FILES = \
	$(addprefix $(srcdir)/, $(gi_gccintrospector_files)) \
        $(foreach f,$(gi_built_gccintrospector_files), \
	   $(if $(shell test -f $(addprefix $(srcdir)/,$(f)) && echo yes), \
	      $(addprefix $(srcdir)/,$(f)), \
	      $(f)))

INTROSPECTION_GIRS += Gccintrospector-0.1.gir



# GNOME extensions
gi_gccintrospector_gnome_files = $(filter-out gccintrospector-gnome.h,\
		          $(libgnomegccintrospectorgnomeinclude_HEADERS) \
	                  $(filter-out %.h, $(libgnomegccintrospector_gnome_0_1_la_SOURCES)))
GccintrospectorGNOME-0.1.gir: libgnomegccintrospector-gnome-0.1.la Gccintrospector-0.1.gir
GccintrospectorGNOME_0_1_gir_SCANNERFLAGS = --strip-prefix=gccintrospector
GccintrospectorGNOME_0_1_gir_INCLUDES = Gccintrospector-0.1
GccintrospectorGNOME_0_1_gir_CFLAGS = $(INCLUDES)
GccintrospectorGNOME_0_1_gir_LIBS = libgnomegccintrospector-gnome-0.1.la
GccintrospectorGNOME_0_1_gir_FILES = $(addprefix $(srcdir)/,$(gi_gccintrospector_gnome_files))

INTROSPECTION_GIRS += GccintrospectorGNOME-0.1.gir



girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

CLEANFILES += $(gir_DATA) $(typelib_DATA)



EXTRA_DIST=				\
	gccintrospector-marshal.list		\
	gccintrospector-enum-types.h.tmpl		\
	gccintrospector-enum-types.c.tmpl
