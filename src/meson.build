script_conf = configuration_data()
script_conf.set('GJS', gjs_console.path())

script = 'polari-accounts'
configure_file(
  input: script + '.in',
  output: script,
  configuration: script_conf
)

data_resources = gnome.compile_resources(
  'data-resources',
  '../data/@0@.data.gresource.xml'.format(app_id),
  source_dir: '../data',
  c_name: 'data_resources'
)

lib_resources = gnome.compile_resources(
  'lib-resources',
  '../data/@0@.lib.gresource.xml'.format(app_id),
  source_dir: '../data',
  c_name: 'lib_resources'
)

src_resources = gnome.compile_resources(
  'src-resources',
  '@0@.src.gresource.xml'.format(app_id),
  c_name: 'src_resources'
)

exeargs = [
  '-DPACKAGE_NAME="polari"',
  '-DPACKAGE_VERSION="@0@"'.format(meson.project_version()),
  '-DPREFIX="@0@"'.format(prefix),
  '-DLIBDIR="@0@"'.format(join_paths(prefix, libdir)),
  '-DPKGLIBDIR="@0@"'.format(join_paths(prefix, pkglibdir))
]
polari = executable('polari', ['polari.c', src_resources, data_resources],
  dependencies: [gio, girepository, gjs],
  c_args: exeargs,
  install: true
)

libsources = [
  'lib/polari-drag-helper.c',
  'lib/polari-drag-helper.h',
  'lib/polari-message.h',
  'lib/polari-message.c',
  'lib/polari-room.c',
  'lib/polari-room.h',
  'lib/polari-util.c',
  'lib/polari-util.h'
]

libsources_private = [
  'lib/polari-message-private.h'
]

libargs = [
  '-DG_LOG_USE_STRUCTURED',
  '-DG_LOG_DOMAIN="Polari"'
]
libpolari = shared_library('polari-1.0', libsources + libsources_private + lib_resources,
  dependencies: [gio, gtk3, telepathy_glib, telepathy_logger, tracker],
  c_args: libargs,
  install: true,
  install_dir: pkglibdir
)

polari_log_tool = executable('polari-log-tool', 'polari-log-tool.c',
  dependencies: [gio, telepathy_glib, tracker],
  link_with: libpolari,
  install_rpath: ':'.join([join_paths(prefix, libdir),
                           join_paths(prefix, pkglibdir)]),
  install: true
)

gnome.generate_gir(libpolari,
  sources: libsources,
  nsversion: '1.0',
  namespace: 'Polari',
  symbol_prefix: 'polari',
  identifier_prefix: 'Polari',
  includes: ['Gio-2.0', 'Gtk-3.0', 'TelepathyGLib-0.12', 'TelepathyLogger-0.2', 'Tracker-2.0'],
  extra_args: '--quiet',
  install_dir_gir: girdir,
  install_dir_typelib: typelibdir,
  install: true
)
