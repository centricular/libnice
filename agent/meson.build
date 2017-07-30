agent_headers = [ # FIXME: unused? install?
  'address.h',
  'debug.h',
  'candidate.h',
  'component.h',
  'agent.h',
  'agent-priv.h',
  'stream.h',
  'conncheck.h',
  'discovery.h',
  'interfaces.h',
  'pseudotcp.h',
  'iostream.h',
  'inputstream.h',
  'outputstream.h',
]

agent_sources = [
  'address.c',
  'debug.c',
  'candidate.c',
  'component.c',
  'agent.c',
  'stream.c',
  'conncheck.c',
  'discovery.c',
  'interfaces.c',
  'pseudotcp.c',
  'iostream.c',
  'inputstream.c',
  'outputstream.c',
]

gnome = import('gnome')

# TODO: use mkenums_simple() once it lands in Meson
agent_enum_types_h = gnome.mkenums('agent-enum-types.h',
  sources: 'agent.h',
  fhead: '''
#ifndef __AGENT_ENUM_TYPES_H__
#define __AGENT_ENUM_TYPES_H__ 1

#include <glib-object.h>

G_BEGIN_DECLS
''',
  fprod: '''
/* enumerations from "@basename@" */
''',
  vhead: '''
GType @enum_name@_get_type (void) G_GNUC_CONST;
#define NICE_TYPE_@ENUMSHORT@ (@enum_name@_get_type())
''',
  ftail: '''
G_END_DECLS

#endif /* !AGENT_ENUM_TYPES_H */''',
  install_header: false)

agent_enum_types_c = gnome.mkenums('agent-enum-types.c',
  sources: 'agent.h',
  depends: agent_enum_types_h,
  fhead: '''
#include <config.h>
#include <glib-object.h>
#include "agent.h"
#include "agent-enum-types.h"
''',
  fprod: '''
/* enumerations from "@basename@" */
''',
  vhead: '''
GType
@enum_name@_get_type (void)
{
  static GType type = 0;
  if (!type) {
    static const G@Type@Value values[] = {
''',
  vprod: '''      { @VALUENAME@, "@VALUENAME@", "@valuenick@" },''',
  vtail: '''      { 0, NULL, NULL }
    };
    type = g_@type@_register_static ("@EnumName@", values);
  }
  return type;
}

''')

libagent = static_library('agent',
  agent_enum_types_c, agent_enum_types_h, agent_sources,
  c_args: ['-DG_LOG_DOMAIN="libnice"'],
  include_directories: nice_incs,
  dependencies: nice_deps,
  install: false)

# FIXME: introspection
makfile='''
pkginclude_HEADERS = \
	agent.h \
	candidate.h \
	debug.h \
	address.h \
	interfaces.h \
	pseudotcp.h \
	$(NULL)

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
