libnice = library('nice',
  link_whole: [libagent, libsocket, libstun, librandom],
  version : libversion,
  soversion : soversion,
  install: true)

install_headers('nice.h', subdir: 'nice')
nice_include = include_directories('.')

libnice_dep = declare_dependency(link_with : libnice,
  include_directories : [agent_include, nice_include])

# TODO: symbol script
am='''
libnice_la_LDFLAGS = \
	-export-symbols $(srcdir)/libnice.sym \
	$(LIBNICE_LT_LDFLAGS)

test-symbols.sh::
	chmod +x $(srcdir)/$@

libnice-symbols-test.c: libnice.sym
	rm -f $@
	while read s; do echo "void $$s(void) { }" ; done < $? > $@

libnice-symbols-test.o: libnice-symbols-test.c
	$(CC) $(CFLAGS) -c -o $@ $?

libnice.symbols: libnice-symbols-test.o
	rm -f $@
	$(top_srcdir)/scripts/make-symbol-list.sh $? > $@

check_SCRIPTS = test-symbols.sh
check_DATA = libnice.symbols

TESTS = $(check_SCRIPTS)

EXTRA_DIST = $(check_SCRIPTS) libnice.sym libnice.ver
'''

# pkg-config file
pkg = import('pkgconfig')
upnp_enabled_string = gupnp_igd_dep.found() ? 'true' : 'false'
pkg.generate(libnice,
  name: 'libnice',
  filebase: 'nice',
  subdirs: 'nice',
  description: 'ICE library',
  requires: glib_dep,
  requires_private: [gthread_dep, gupnp_igd_dep, gnutls_dep, syslibs],
  variables: ['upnp_enabled=@0@'.format(upnp_enabled_string)])
