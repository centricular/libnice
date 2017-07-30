foreach t : ['parse', 'format', 'bind', 'conncheck', 'hmac']
  test_name = 'test-@0@'.format(t)
  exe = executable(test_name, test_name + '.c',
    include_directories: nice_incs,
    dependencies: syslibs, # ws32_32
    link_with: libstun)
  test(test_name, exe)
endforeach
