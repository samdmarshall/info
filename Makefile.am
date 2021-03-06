# $Id: Makefile.am 5244 2013-03-20 23:22:05Z karl $
# Makefile.am for texinfo.
# Process this file with automake to produce Makefile.in in all directories.
#
# Copyright 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011,
# 2012, 2013 Free Software Foundation, Inc.
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

# Find our headers and gnulib headers.
ACLOCAL_AMFLAGS = -I gnulib/m4

# Additional files to distribute.
EXTRA_DIST = ChangeLog.46 INSTALL.generic README-hacking \
  system.h gnulib/m4/gnulib-cache.m4 \
  contrib djgpp

dist-hook:
	cd $(top_distdir)/contrib/perldoc-all && $(MAKE) distclean
	rm -rf `find $(distdir) -type d -name .svn`

# This is to prevent texinfo.tex from being included in the top-level
# distribution directory.
TEXINFO_TEX = doc/texinfo.tex

SUBDIRS =
if TOOLS_ONLY
  # Build native tools only.
  SUBDIRS += gnulib/lib install-info tp util
if HAVE_TERMLIBS
    SUBDIRS += info
else
# DOS/Windows don't need TERMLIBS to build Info
if HOST_IS_WINDOWS
  SUBDIRS += info
endif
endif
else
  # All subdirectories.
  # Do libs first since the C programs depend on it.
  # Do doc and man last so things will be built when we get there.
  # Others are alphabetical.
  SUBDIRS += $(native_tools) gnulib/lib
if HAVE_TERMLIBS
  SUBDIRS += info
else
# DOS/Windows don't need TERMLIBS to build Info
if HOST_IS_WINDOWS
  SUBDIRS += info
endif
endif
  SUBDIRS += install-info po po_document tp Pod-Simple-Texinfo util \
             doc man
endif

# One special target for installers to use by hand if desired.
install-tex:
	cd doc && $(MAKE) TEXMF=$(TEXMF) install-tex

distclean-local:
	-test -d "$(native_tools)" && rm -rf "$(native_tools)"

dist-local:
	cp -fpR $(srcdir)/po_document/*.po $(srcdir)/po_document/LINGUAS texi2html/po_document/
	cp -fpR $(srcdir)/po/*.po $(srcdir)/po/LINGUAS texi2html/po_messages/

# Don't install native tools for cross compile.
if TOOLS_ONLY
install:

endif

# From coreutils.
# Verify that all source files using _() are listed in po/POTFILES.in.
# Run this before making pretests, as well as official releases, so that
# translators will see changed strings.
po-check:
	@if test -f po/POTFILES.in; then				\
	  grep -E -v '^(#|$$)' po/POTFILES.in				\
	    | grep -v '^src/false\.c$$' | sort > $@-1;			\
	  files=;							\
	  for file in $$($(CVS_LIST_EXCEPT))				\
	      `find [a-z]* -name '*.[ch]' -o -name '*.p[lm]'`; do	\
	    case $$file in						\
            contrib/* ) continue;;					\
	    djgpp/* | makeinfo/* | man/* | texi2html/* ) continue;;	\
            tp/maintain/* ) continue;;					\
	    esac;							\
	    case $$file in						\
	    *.[ch] | *.p[lm])						\
	      base=`expr " $$file" : ' \(.*\)\..'`;			\
	      { test -f $$base.l || test -f $$base.y; } && continue;;	\
	    esac;							\
	    files="$$files $$file";					\
	  done;								\
	  grep -E -l '\b(N?__?|gdt|gettext *)\([^)"]*("|$$)' $$files	\
	    | sort -u > $@-2;						\
	  diff -u $@-1 $@-2 || exit 1;					\
	  rm -f $@-1 $@-2;						\
	fi
