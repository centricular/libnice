agent_headers = [
  'address.h',
  'agent.h',
  'candidate.h',
  'debug.h',
  'interfaces.h',
  'pseudotcp.h',
]
install_headers(agent_headers, subdir : 'nice')
agent_include = include_directories('.')

agent_sources = [
  'address.c',
  'agent.c',
  'candidate.c',
  'component.c',
  'conncheck.c',
  'debug.c',
  'discovery.c',
  'inputstream.c',
  'interfaces.c',
  'iostream.c',
  'outputstream.c',
  'pseudotcp.c',
  'stream.c',
]

gnome = import('gnome')

agent_enum_types = gnome.mkenums_simple('agent-enum-types', sources : agent_headers)
agent_enum_types_c = agent_enum_types[0]
agent_enum_types_h = agent_enum_types[1]

libagent = static_library('agent',
  agent_enum_types, agent_sources,
  c_args: ['-DG_LOG_DOMAIN="libnice"'],
  include_directories: nice_incs,
  dependencies: nice_deps,
  install: false)

# TODO: introspection
todo='''
# GObject introspection
# We need --accept-unprefixed because of PseudoTcp and TurnServer.
-include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ARGS = --add-include-path=$(srcdir) --warn-all --accept-unprefixed
INTROSPECTION_COMPILER_ARGS = --includedir=$(srcdir)

if HAVE_INTROSPECTION
introspection_sources = $(pkginclude_HEADERS)

Nice-0.1.gir: libagent.la
Nice_0_1_gir_INCLUDES = GObject-2.0 Gio-2.0
Nice_0_1_gir_EXPORT_PACKAGES = nice
Nice_0_1_gir_CFLAGS = $(AM_CFLAGS)
Nice_0_1_gir_LIBS = libagent.la
Nice_0_1_gir_FILES = $(introspection_sources)
INTROSPECTION_GIRS += Nice-0.1.gir

girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)
endif'''
