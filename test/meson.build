test_names = [
  'hdr_histogram_test',
  'hdr_histogram_atomic_test',
  'hdr_atomic_test',
  'hdr_histogram_atomic_concurrency_test']

if enable_logging
  test_names += ['hdr_histogram_log_test']
endif

foreach tname : test_names
  t = executable(tname,
    [tname + '.c', 'minunit.c'],
    include_directories: include_directories('../src'),
    dependencies: hdr_histogram_dep)
  test(tname, t, workdir: meson.current_source_dir())
endforeach

executable('hdr_histogram_perf',
  ['hdr_histogram_perf.c', 'minunit.c'],
  include_directories: include_directories('../src'),
  dependencies: hdr_histogram_dep)

if get_option('build_benchmarks')
  benchmark = dependency('benchmark')
  executable('hdr_histogram_benchmark',
    'hdr_histogram_benchmark.cpp',
    dependencies: [hdr_histogram_dep, benchmark])
endif
