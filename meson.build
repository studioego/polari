project('polari', 'c',
  version: '3.26.0',
  meson_version: '>= 0.37.0',
  license: 'GPL2+'
)

app_id = 'org.gnome.Polari'

gnome = import('gnome')
i18n = import('i18n')

prefix = get_option('prefix')

bindir = get_option('bindir')
libdir = get_option('libdir')
datadir = get_option('datadir')

pkglibdir = join_paths(libdir, meson.project_name())
pkgdatadir = join_paths(datadir, meson.project_name())

appdatadir = join_paths(datadir, 'appdata')
desktopdir = join_paths(datadir, 'applications')
icondir = join_paths(datadir, 'icons')
schemadir = join_paths(datadir, 'glib-2.0', 'schemas')
servicedir = join_paths(datadir, 'dbus-1', 'services')
tpclientdir = join_paths(datadir, 'telepathy', 'clients')
girdir = join_paths(pkgdatadir, 'gir-1.0')
typelibdir = join_paths(pkglibdir, 'girepository-1.0')

gjs_console = find_program('gjs')

desktop_file_validate = find_program('desktop-file-validate', required: false)
appstream_util = find_program('appstream-util', required: false)

gio = dependency('gio-2.0', version: '>= 2.43.4')
gtk3 = dependency('gtk+-3.0', version: '>= 3.21.6')
telepathy_glib = dependency('telepathy-glib')
girepository = dependency('gobject-introspection-1.0')
gjs = dependency('gjs-1.0')

cc = meson.get_compiler('c')
if (cc.has_function('strcasestr'))
  add_project_arguments('-DHAVE_STRCASECSTR', language: 'c')
endif

subdir('src')
subdir('data')
subdir('po')
subdir('help')

meson.add_install_script('meson/meson-postinstall.sh')
