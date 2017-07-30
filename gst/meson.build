gst_nice_sources = [
  'gstnicesrc.c',
  'gstnicesink.c',
  'gstnice.c',
]

gst_nice_args = ['-DGST_USE_UNSTABLE_API']

libtype = 'shared' # FIXME: add library_format option or so

if libtype != 'shared'
  gst_nice_static = static_library('gstnice',
    gst_nice_sources,
    c_args: gst_nice_args,
    include_directories: nice_incs,
    dependencies: [nice_deps, gst_dep],
    link_with: libnice,
    install_dir: join_paths(get_option('libdir'), 'gstreamer-1.0'),
    install: true)
endif

if libtype != 'static'
  gst_nice_shared = shared_module('gstnice',
    gst_nice_sources,
    c_args : gst_nice_args,
    include_directories: nice_incs,
    dependencies: [nice_deps, gst_dep],
    link_with: libnice,
    install_dir: join_paths(get_option('libdir'), 'gstreamer-1.0'),
    install: true)
endif
