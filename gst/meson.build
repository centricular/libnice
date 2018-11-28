gst_nice_sources = [
  'gstnicesrc.c',
  'gstnicesink.c',
  'gstnice.c',
]

gst_nice_args = ['-DGST_USE_UNSTABLE_API']
plugins_install_dir = join_paths(get_option('libdir'), 'gstreamer-1.0')

libgstnice = library('gstnice',
  gst_nice_sources,
  c_args : gst_nice_args,
  include_directories: nice_incs,
  dependencies: [nice_deps, gst_dep],
  link_with: libnice,
  install_dir: plugins_install_dir,
  install: true)

if get_option('default_library') == 'shared'
  # If we don't build static plugins there is no need to generate pc files
  subdir_done()
endif

pkgconfig = import('pkgconfig')
plugins_pkgconfig_install_dir = join_paths(plugins_install_dir, 'pkgconfig')
pkgconfig.generate(libgstnice, install_dir : plugins_pkgconfig_install_dir)
