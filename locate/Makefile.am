# The default database to build and search.
AUTOMAKE_OPTIONS = std-options
AM_CFLAGS = $(WARN_CFLAGS)
LOCATE_DB = $(localstatedir)/locatedb
localedir = $(datadir)/locale

AM_INSTALLCHECK_STD_OPTIONS_EXEMPT = frcode$(EXEEXT)
bin_PROGRAMS = locate
libexec_PROGRAMS = frcode
bin_SCRIPTS = updatedb
man_MANS = locate.1 updatedb.1 locatedb.5
BUILT_SOURCES = dblocation.texi
EXTRA_DIST = locatedb.h updatedb.sh $(man_MANS)
CLEANFILES = updatedb

DISTCLEANFILES = dblocation.texi
locate_SOURCES = locate.c word_io.c
nodist_locate_TEXINFOS = dblocation.texi

AM_CPPFLAGS = -I$(top_srcdir)/lib -I../gl/lib -I$(top_srcdir)/gl/lib -DLOCATE_DB=\"$(LOCATE_DB)\" -DLOCALEDIR=\"$(localedir)\"

LDADD = ../lib/libfind.a ../gl/lib/libgnulib.a $(LIB_CLOSE) $(LIBINTL)

$(PROGRAMS) $(LIBPROGRAMS): ../lib/libfind.a ../gl/lib/libgnulib.a

# We generate updatedb from the Makefile rather than with AC_OUTPUT in
# configure.ac in order to allow the user better control of what goes
# into it by setting Makefile variables.
updatedb: updatedb.sh Makefile
	rm -f $@
	find=`echo find|sed '$(transform)'`; \
	frcode=`echo frcode|sed '$(transform)'`; \
	sed \
	-e "s,@""bindir""@,$(bindir)," \
	-e "s,@""libexecdir""@,$(libexecdir)," \
	-e "s,@""LOCATE_DB""@,$(LOCATE_DB)," \
	-e "s,@""VERSION""@,$(VERSION)," \
	-e "s,@""PACKAGE_NAME""@,$(PACKAGE_NAME)," \
	-e "s,@""PACKAGE_BUGREPORT""@,$(PACKAGE_BUGREPORT)," \
	-e "s,@""PACKAGE_BUGREPORT_URL""@,$(PACKAGE_BUGREPORT_URL)," \
	-e "s,@""PACKAGE_URL""@,$(PACKAGE_URL)," \
	-e "s,@""find""@,$${find}," \
	-e "s,@""frcode""@,$${frcode}," \
	-e "s,@""SORT""@,$(SORT)," \
	-e "s,@""SORT_SUPPORTS_Z""@,$(SORT_SUPPORTS_Z)," \
	$(srcdir)/updatedb.sh > $@
	chmod +x $@

install-data-hook:
	$(top_srcdir)/build-aux/mkinstalldirs $(DESTDIR)$(localstatedir)

dblocation.texi:
	echo '@set LOCATE_DB $(LOCATE_DB)' > $@.tmp
	if test -f $@ && cmp $@.tmp $@ >/dev/null ; then \
	    rm $@.tmp ; \
	else \
	    mv $@.tmp $@ ; \
	fi
.PHONY: dblocation.texi

SUBDIRS = . testsuite

dist-hook: findutils-check-manpages

findutils-check-manpages:
	$(top_srcdir)/build-aux/man-lint.sh $(srcdir) $(man_MANS)

# Clean coverage files generated by running binaries built with
# gcc -fprofile-arcs -ftest-coverage
coverage-clean:
	$(RM) *.gcno *.gcda *.gcov *.lcov

clean-local: coverage-clean
