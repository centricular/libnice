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
  'test-udp-turn-fragmentation',
  'test-priority',
  'test-fullmode',
  'test-different-number-streams',
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

tenv = environment()
tenv.set('BUILT_WITH_MESON', '1')

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
    link_with: [libagent, libstun, libsocket, librandom],
    install: false)
  set_variable(tname.underscorify(), exe)
  test(tname, exe, env: tenv)
endforeach

if gst_dep.found()
  gst_check = dependency('gstreamer-check-1.0', required: get_option('gstreamer'),
                         fallback : ['gstreamer', 'gst_check_dep'])
  if gst_check.found()
    exe = executable('nice-test-gstreamer',
      'test-gstreamer.c', extra_src,
      c_args: '-DG_LOG_DOMAIN="libnice-tests"',
      include_directories: nice_incs,
      dependencies: [nice_deps, gst_check, libm],
      link_with: libnice,
      install: false)
    test('test-gstreamer', exe, env: tenv)
  endif
endif

if find_program('sh', required : false).found()
  dd = find_program('dd', required : false)
  diff = find_program('diff', required : false)
  if dd.found() and diff.found()
    test('test-pseudotcp-random', find_program('test-pseudotcp-random.sh'),
      args: test_pseudotcp,
      env: tenv)
  endif

  test('test-fullmode-with-stun', files('check-test-fullmode-with-stun.sh'),
    args: [stund_exe, test_fullmode],
    env: tenv)
endif

debugenv = environment()
debugenv.set('G_MESSAGES_DEBUG', 'all')
debugenv.set('NICE_DEBUG', 'all')
add_test_setup('debug', env: debugenv)
