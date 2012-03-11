## Process this file with automake to produce Makefile.in
ACLOCAL_AMFLAGS = -I m4

SUBDIRS = libgccintrospector 
#tests docs

EXTRA_DIST =			\
	libgnomegccintrospector-0.1.pc.in	\
	m4/introspection.m4
#	gtk-doc.make		

DISTCHECK_CONFIGURE_FLAGS =  --enable-introspection
#--enable-gtk-doc

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libgccintrospector-0.1.pc

