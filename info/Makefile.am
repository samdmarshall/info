# $Id: Makefile.am 5255 2013-04-15 21:39:09Z gray $
# Makefile.am for texinfo/info.
#
# Copyright 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2012
# Free Software Foundation, Inc.
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

# Use `ginfo' for building to avoid confusion with the standard `info'
# target.  Removes the `g' in the install rule before applying any
# user-specified name transformations.
bin_PROGRAMS = ginfo infokey
transform = s/ginfo/info/; $(program_transform_name)

localedir = $(datadir)/locale
infodir2 = $(datadir)/info

AM_CPPFLAGS = 					\
  -I$(top_srcdir)				\
  -I$(top_srcdir)/gnulib/lib			\
  -I$(top_builddir)/gnulib/lib			\
  -DLOCALEDIR=\"$(localedir)\" 			\
  -DINFODIR=\"$(infodir)\"			\
  -DINFODIR2=\"$(infodir2)\"

LDADD = $(top_builddir)/gnulib/lib/libgnu.a $(TERMLIBS) $(LIBINTL)
infokey_LDADD = $(top_builddir)/gnulib/lib/libgnu.a $(LIBINTL)

EXTRA_DIST = README pcterm.c

ginfo_SOURCES = dir.c display.c display.h doc.h dribble.c dribble.h \
  echo-area.c echo-area.h \
  filesys.c filesys.h footnotes.c footnotes.h gc.c gc.h \
  indices.c indices.h info-utils.c info-utils.h info.c info.h infodoc.c \
  infomap.c infomap.h infopath.c m-x.c man.c man.h nodemenu.c nodes.c nodes.h \
  search.c search.h session.c session.h signals.c signals.h \
  tag.c tag.h termdep.h terminal.c terminal.h tilde.c tilde.h \
  variables.c variables.h window.c window.h
nodist_ginfo_SOURCES = doc.c funs.h
infokey_SOURCES = infokey.c infokey.h key.h
nodist_infokey_SOURCES = key.c funs.h

# The files `doc.c', `key.c' and `funs.h' are created by ./makedoc run over the
# source files which contain DECLARE_INFO_COMMAND.  `funs.h' is a header file
# listing the functions found.  `doc.c' is a structure containing pointers
# to those functions along with completable names and documentation strings.
#
generated_sources = doc.c key.c funs.h

# We need a linear ordering to prevent race condition in parallel make.
# See the node ``(automake)Multiple Outputs'' for an explanation.
# Make sure this matches with order in which makedoc.c creates these
# files.
doc.c: key.c
key.c: funs.h

# These files are not distributed.
DISTCLEANFILES = $(generated_sources)

noinst_PROGRAMS = makedoc
makedoc_SOURCES = makedoc.c

# Files with Info commands defined that makedoc should read.
cmd_sources = $(srcdir)/session.c $(srcdir)/echo-area.c $(srcdir)/infodoc.c \
  $(srcdir)/m-x.c $(srcdir)/indices.c $(srcdir)/nodemenu.c \
  $(srcdir)/footnotes.c $(srcdir)/variables.c

# Make the target only funs.h even though we create all the
# $(generated_sources) so that parallel makes won't do the rm && makedoc
# more than once.
funs.h: makedoc$(EXEEXT) $(cmd_sources)
	rm -f $(generated_sources)
	$(top_builddir)/$(native_tools)/info/makedoc $(cmd_sources)

# The following hack is necessary to hint make before the automatic
# dependencies are built.
BUILT_SOURCES = funs.h
