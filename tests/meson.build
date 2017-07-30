nice_tests = [
  'test-pseudotcp',
  'test-pseudotcp-fin',
  'test-pseudotcp-fuzzy',
  'test-bsd',
  'test',
  'test-address',
  'test-add-remove-stream',
  'test-build-io-stream',
  'test-io-stream-thread',
  'test-io-stream-closing-write',
  'test-io-stream-closing-read',
  'test-io-stream-cancelling',
  'test-io-stream-pollable',
  'test-send-recv',
  'test-socket-is-based-on',
  'test-priority',
  'test-fullmode',
  'test-restart',
  'test-fallback',
  'test-thread',
  'test-dribble',
  'test-new-dribble',
  'test-tcp',
  'test-icetcp',
  'test-credentials',
  'test-turn',
  'test-drop-invalid',
  'test-nomination',
]

foreach tname : nice_tests
  if tname.startswith('test-io-stream') or tname.startswith('test-send-recv')
    extra_src = ['test-io-stream-common.c']
  else
    extra_src = []
  endif
  exe = executable('nice-@0@'.format(tname),
    '@0@.c'.format(tname), extra_src,
    c_args: '-DG_LOG_DOMAIN="libnice-tests"',
    include_directories: nice_incs,
    dependencies: [nice_deps, libm],
    link_with: libnice,
    install: false)

  test(tname, exe)
endforeach

if using_gst
  gst_check = dependency('gstreamer-check-1.0', required: false)
  if gst_check.found()
    exe = executable('nice-test-gstreamer',
      'test-gstreamer.c', extra_src,
      c_args: '-DG_LOG_DOMAIN="libnice-tests"',
      include_directories: nice_incs,
      dependencies: [nice_deps, gst_check, libm],
      link_with: libnice,
      install: false)
    test('test-gstreamer', exe)
  endif
endif

am='''
TESTS_ENVIRONMENT=\
	GST_PLUGIN_PATH=${GST_PLUGIN_PATH}:$(top_builddir)/gst

AM_TESTS_ENVIRONMENT = \
	G_MESSAGES_DEBUG=all \
	NICE_DEBUG=all;

dist_check_SCRIPTS = \
	check-test-fullmode-with-stun.sh \
  'test-pseudotcp-random.sh

TESTS = $(check_PROGRAMS) $(dist_check_SCRIPTS)

all-local:
	chmod a+x $(srcdir)/check-test-fullmode-with-stun.sh
	chmod a+x $(srcdir)/test-pseudotcp-random.sh
'''
