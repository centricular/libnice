libnice = library('nice', 'nice.h',
  link_whole: [libagent, libsocket, libstun, librandom],
  version : libversion,
  soversion : soversion,
  install: true)

am='''
libnice_la_DEPENDENCIES = \
	$(top_builddir)/agent/libagent.la \
	libnice.sym

libnice_la_LIBADD = \
	$(GLIB_LIBS) \
	$(GUPNP_LIBS) \
	$(top_builddir)/agent/libagent.la

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
pkgconf = configuration_data()
pkgconf.set('prefix', get_option('prefix'))
pkgconf.set('exec_prefix', '${prefix}')
pkgconf.set('libdir', '${prefix}/@0@'.format(get_option('libdir')))
pkgconf.set('includedir', '${prefix}/@0@'.format(get_option('includedir')))
pkgconf.set('VERSION', meson.project_version())
pkgconf.set('UPNP_ENABLED', using_gupnp ? 'true' : 'false')

# Requires
pkgconf.set('NICE_PACKAGES_PUBLIC', 'glib-2.0 >= @0@ gio-2.0 >= @0@ gobject-2.0 >= @0@'.format(glib_req))
pkgconf.set('GUPNP_PACKAGES_PUBLIC', '')

# Requires.private
pkgconf.set('NICE_PACKAGES_PRIVATE', 'gthread-2.0 gnutls >= @0@'.format(gnutls_req))
pkgconf.set('GUPNP_PACKAGES_PRIVATE', using_gupnp ? 'gupnp-igd-1.0 >= @0@'.format(gupnp_igd_req) : '')

pkg_install_dir = join_paths(get_option('libdir'), 'pkgconfig')

configure_file(input : 'nice.pc.in',
  output : 'nice.pc',
  configuration : pkgconf,
  install_dir : pkg_install_dir)
