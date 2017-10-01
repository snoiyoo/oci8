srcdir = /home/suwit/oracle/oci8-2.0.4
builddir = /home/suwit/oracle/oci8-2.0.4
top_srcdir = /home/suwit/oracle/oci8-2.0.4
top_builddir = /home/suwit/oracle/oci8-2.0.4
EGREP = grep -E
SED = /bin/sed
CONFIGURE_COMMAND = './configure'
SHLIB_SUFFIX_NAME = so
SHLIB_DL_SUFFIX_NAME = so
RE2C = exit 0;
AWK = gawk
shared_objects_oci8 = oci8.lo oci8_lob.lo oci8_statement.lo oci8_collection.lo oci8_interface.lo
PHP_PECL_EXTENSION = oci8
OCI8_SHARED_LIBADD = -Wl,-rpath,/usr/lib/oracle/11.2/client64/lib -L/usr/lib/oracle/11.2/client64/lib -lclntsh
OCI8_DIR = /usr/lib/oracle/11.2/client64/lib
OCI8_ORACLE_VERSION = 11.1
PHP_MODULES = $(phplibdir)/oci8.la
all_targets = $(PHP_MODULES)
install_targets = install-modules install-headers
prefix = /usr
exec_prefix = $(prefix)
libdir = ${exec_prefix}/lib
prefix = /usr
phplibdir = /home/suwit/oracle/oci8-2.0.4/modules
phpincludedir = /usr/include/php
CC = gcc
CFLAGS = -g -O2
CFLAGS_CLEAN = $(CFLAGS)
CPP =
CPPFLAGS = -DHAVE_CONFIG_H
CXX = g++
CXXFLAGS = -g -O2
EXTENSION_DIR = /usr/lib64/php/modules
PHP_EXECUTABLE = /usr/bin/php
EXTRA_LDFLAGS =
EXTRA_LIBS =
INCLUDES = -I/usr/include/php -I/usr/include/php/main -I/usr/include/php/TSRM -I/usr/include/php/Zend -I/usr/include/php/ext -I/usr/include/php/ext/date/lib -I/usr/include/oracle/11.2/client64
LFLAGS =
LDFLAGS =
SHARED_LIBTOOL =
LIBTOOL = $(SHELL) $(top_builddir)/libtool
SHELL = /bin/sh
INSTALL_HEADERS =
mkinstalldirs = $(top_srcdir)/build/shtool mkdir -p
INSTALL = $(top_srcdir)/build/shtool install -c
INSTALL_DATA = $(INSTALL) -m 644

DEFS = -DPHP_ATOM_INC -I$(top_builddir)/include -I$(top_builddir)/main -I$(top_srcdir)
COMMON_FLAGS = $(DEFS) $(INCLUDES) $(EXTRA_INCLUDES) $(CPPFLAGS) $(PHP_FRAMEWORKPATH)

all: $(all_targets) 
	@echo
	@echo "Build complete."
	@echo "(It is safe to ignore warnings about tempnam and tmpnam)."
	@echo
	
build-modules: $(PHP_MODULES)

libphp5.la: $(PHP_GLOBAL_OBJS) $(PHP_SAPI_OBJS)
	$(LIBTOOL) --mode=link $(CC) $(CFLAGS) $(EXTRA_CFLAGS) -rpath $(phptempdir) $(EXTRA_LDFLAGS) $(LDFLAGS) $(PHP_RPATHS) $(PHP_GLOBAL_OBJS) $(PHP_SAPI_OBJS) $(EXTRA_LIBS) $(ZEND_EXTRA_LIBS) -o $@
	-@$(LIBTOOL) --silent --mode=install cp libphp5.la $(phptempdir)/libphp5.la >/dev/null 2>&1

libs/libphp5.bundle: $(PHP_GLOBAL_OBJS) $(PHP_SAPI_OBJS)
	$(CC) $(MH_BUNDLE_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS) $(LDFLAGS) $(EXTRA_LDFLAGS) $(PHP_GLOBAL_OBJS:.lo=.o) $(PHP_SAPI_OBJS:.lo=.o) $(PHP_FRAMEWORKS) $(EXTRA_LIBS) $(ZEND_EXTRA_LIBS) -o $@ && cp $@ libs/libphp5.so

install: $(all_targets) $(install_targets)

install-sapi: $(OVERALL_TARGET)
	@echo "Installing PHP SAPI module:       $(PHP_SAPI)"
	-@$(mkinstalldirs) $(INSTALL_ROOT)$(bindir)
	-@if test ! -r $(phptempdir)/libphp5.$(SHLIB_DL_SUFFIX_NAME); then \
		for i in 0.0.0 0.0 0; do \
			if test -r $(phptempdir)/libphp5.$(SHLIB_DL_SUFFIX_NAME).$$i; then \
				$(LN_S) $(phptempdir)/libphp5.$(SHLIB_DL_SUFFIX_NAME).$$i $(phptempdir)/libphp5.$(SHLIB_DL_SUFFIX_NAME); \
				break; \
			fi; \
		done; \
	fi
	@$(INSTALL_IT)

install-modules: build-modules
	@test -d modules && \
	$(mkinstalldirs) $(INSTALL_ROOT)$(EXTENSION_DIR)
	@echo "Installing shared extensions:     $(INSTALL_ROOT)$(EXTENSION_DIR)/"
	@rm -f modules/*.la >/dev/null 2>&1
	@$(INSTALL) modules/* $(INSTALL_ROOT)$(EXTENSION_DIR)

install-headers:
	-@if test "$(INSTALL_HEADERS)"; then \
		for i in `echo $(INSTALL_HEADERS)`; do \
			i=`$(top_srcdir)/build/shtool path -d $$i`; \
			paths="$$paths $(INSTALL_ROOT)$(phpincludedir)/$$i"; \
		done; \
		$(mkinstalldirs) $$paths && \
		echo "Installing header files:          $(INSTALL_ROOT)$(phpincludedir)/" && \
		for i in `echo $(INSTALL_HEADERS)`; do \
			if test "$(PHP_PECL_EXTENSION)"; then \
				src=`echo $$i | $(SED) -e "s#ext/$(PHP_PECL_EXTENSION)/##g"`; \
			else \
				src=$$i; \
			fi; \
			if test -f "$(top_srcdir)/$$src"; then \
				$(INSTALL_DATA) $(top_srcdir)/$$src $(INSTALL_ROOT)$(phpincludedir)/$$i; \
			elif test -f "$(top_builddir)/$$src"; then \
				$(INSTALL_DATA) $(top_builddir)/$$src $(INSTALL_ROOT)$(phpincludedir)/$$i; \
			else \
				(cd $(top_srcdir)/$$src && $(INSTALL_DATA) *.h $(INSTALL_ROOT)$(phpincludedir)/$$i; \
				cd $(top_builddir)/$$src && $(INSTALL_DATA) *.h $(INSTALL_ROOT)$(phpincludedir)/$$i) 2>/dev/null || true; \
			fi \
		done; \
	fi
	
install-su: install-pear

test: all 
	-@if test ! -z "$(PHP_EXECUTABLE)" && test -x "$(PHP_EXECUTABLE)"; then \
		TEST_PHP_EXECUTABLE=$(PHP_EXECUTABLE) \
		TEST_PHP_SRCDIR=$(top_srcdir) \
		CC="$(CC)" \
			$(PHP_EXECUTABLE) -d 'open_basedir=' -d 'safe_mode=0' -d 'output_buffering=0' -d 'memory_limit=-1' $(top_srcdir)/run-tests.php -d 'extension_dir=modules/' -d `( . $(PHP_MODULES) ; echo extension=$$dlname)` tests/; \
	elif test ! -z "$(SAPI_CLI_PATH)" && test -x "$(SAPI_CLI_PATH)"; then \
		TEST_PHP_EXECUTABLE=$(top_builddir)/$(SAPI_CLI_PATH) \
		TEST_PHP_SRCDIR=$(top_srcdir) \
		CC="$(CC)" \
			$(top_builddir)/$(SAPI_CLI_PATH) -d 'open_basedir=' -d 'safe_mode=0' -d 'output_buffering=0' -d 'memory_limit=-1' $(top_srcdir)/run-tests.php $(TESTS); \
	else \
		echo "ERROR: Cannot run tests without CLI sapi."; \
	fi

clean:
	find . -name \*.lo -o -name \*.o | xargs rm -f
	find . -name \*.la -o -name \*.a | xargs rm -f 
	find . -name \*.so | xargs rm -f
	find . -name .libs -a -type d|xargs rm -rf
	rm -f libphp5.la $(SAPI_CLI_PATH) $(OVERALL_TARGET) modules/* libs/*

distclean: clean
	rm -f config.cache config.log config.status Makefile.objects Makefile.fragments libtool main/php_config.h stamp-h php5.spec sapi/apache/libphp5.module buildmk.stamp
	$(EGREP) define'.*include/php' $(top_srcdir)/configure | $(SED) 's/.*>//'|xargs rm -f
	find . -name Makefile | xargs rm -f

.PHONY: all clean install distclean test
.NOEXPORT:
oci8.lo: /home/suwit/oracle/oci8-2.0.4/oci8.c
	$(LIBTOOL) --mode=compile $(CC)  -I. -I/home/suwit/oracle/oci8-2.0.4 $(COMMON_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS)  -c /home/suwit/oracle/oci8-2.0.4/oci8.c -o oci8.lo 
oci8_lob.lo: /home/suwit/oracle/oci8-2.0.4/oci8_lob.c
	$(LIBTOOL) --mode=compile $(CC)  -I. -I/home/suwit/oracle/oci8-2.0.4 $(COMMON_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS)  -c /home/suwit/oracle/oci8-2.0.4/oci8_lob.c -o oci8_lob.lo 
oci8_statement.lo: /home/suwit/oracle/oci8-2.0.4/oci8_statement.c
	$(LIBTOOL) --mode=compile $(CC)  -I. -I/home/suwit/oracle/oci8-2.0.4 $(COMMON_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS)  -c /home/suwit/oracle/oci8-2.0.4/oci8_statement.c -o oci8_statement.lo 
oci8_collection.lo: /home/suwit/oracle/oci8-2.0.4/oci8_collection.c
	$(LIBTOOL) --mode=compile $(CC)  -I. -I/home/suwit/oracle/oci8-2.0.4 $(COMMON_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS)  -c /home/suwit/oracle/oci8-2.0.4/oci8_collection.c -o oci8_collection.lo 
oci8_interface.lo: /home/suwit/oracle/oci8-2.0.4/oci8_interface.c
	$(LIBTOOL) --mode=compile $(CC)  -I. -I/home/suwit/oracle/oci8-2.0.4 $(COMMON_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS)  -c /home/suwit/oracle/oci8-2.0.4/oci8_interface.c -o oci8_interface.lo 
$(phplibdir)/oci8.la: ./oci8.la
	$(LIBTOOL) --mode=install cp ./oci8.la $(phplibdir)

./oci8.la: $(shared_objects_oci8) $(OCI8_SHARED_DEPENDENCIES)
	$(LIBTOOL) --mode=link $(CC) $(COMMON_FLAGS) $(CFLAGS_CLEAN) $(EXTRA_CFLAGS) $(LDFLAGS) -o $@ -export-dynamic -avoid-version -prefer-pic -module -rpath $(phplibdir) $(EXTRA_LDFLAGS) $(shared_objects_oci8) $(OCI8_SHARED_LIBADD)

