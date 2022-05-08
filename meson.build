project('hdr_histogram', 'c',
  default_options: [
    'c_std=c99',
    'cpp_std=c++11',
  ])

cc = meson.get_compiler('c')

deps = [
  cc.find_library('m'),
  cc.find_library('rt'),
  dependency('threads')
]

zlib = dependency('zlib', required: get_option('logging'))
enable_logging = zlib.found()

add_project_arguments(
  '-Wno-unknown-pragmas', '-Wshadow', '-Winit-self',
  '-Wpedantic', '-Wmissing-prototypes', '-D_GNU_SOURCE',
  language: 'c')

src = files([
  'src/hdr_encoding.c',
  'src/hdr_histogram.c',
  'src/hdr_interval_recorder.c',
  'src/hdr_thread.c',
  'src/hdr_time.c',
  'src/hdr_writer_reader_phaser.c'])

if enable_logging
  src += ['src/hdr_histogram_log.c']
  deps += [zlib]
else
  src += ['src/hdr_histogram_log_no_op.c']
endif

hdr_histogram = library('hdr_histogram',
  src,
  implicit_include_directories: false,
  include_directories: include_directories('include'),
  dependencies: deps)

hdr_histogram_dep = declare_dependency(
  link_with: hdr_histogram,
  include_directories: include_directories('include'),
  dependencies: deps)

if get_option('build_programs')
  foreach p : ['hdr_decoder', 'hiccup']
    s = 'examples/' + p + '.c'
    executable(p, s, dependencies: hdr_histogram_dep)
  endforeach
endif

if get_option('build_tests')
  subdir('test')
endif
