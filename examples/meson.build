examples = ['simple-example', 'threaded-example', 'sdp-example']

foreach ex : examples
  executable(ex, '@0@.c'.format(ex),
    include_directories: nice_incs,
    dependencies: [libnice_dep, nice_deps],
    install: false)
endforeach
