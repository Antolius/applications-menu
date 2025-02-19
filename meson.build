project(
  'slingshot',
  'vala', 'c',
  version : '2.10.2'
)

i18n = import('i18n')
gnome = import('gnome')

add_project_arguments(
    '-DGETTEXT_PACKAGE="@0@"'.format(meson.project_name()),
    language: 'c'
)

asresources = gnome.compile_resources(
    'as-resources', 'data/icons.gresource.xml',
    source_dir: 'data',
    c_name: 'as'
)

prefix = get_option('prefix')
datadir = join_paths(prefix, get_option('datadir'))
libdir = join_paths(prefix, get_option('libdir'))

add_project_arguments(
    '-DPKGDATADIR="@0@"'.format(join_paths(get_option('prefix'), get_option('libdir'), 'wingpanel')),
    language: 'c'
)

pluginsdir = join_paths(prefix, get_option('libdir'), 'io.elementary.wingpanel.applications-menu')
add_project_arguments(
    '-DPLUGINSDIR="@0@"'.format(pluginsdir),
    language: 'c'
)

glib_dep = dependency('glib-2.0')
gee_dep = dependency('gee-0.8')
gio_dep = dependency('gio-2.0')
gtk_dep = dependency('gtk+-3.0')
granite_dep = dependency('granite', version: '>=6.1.0')
gobject_dep = dependency('gobject-2.0')
libsoup_dep = dependency('libsoup-2.4')
gio_unix_dep = dependency('gio-unix-2.0')
json_glib_dep = dependency('json-glib-1.0')
switchboard_dep = dependency('switchboard-2.0')
libhandy_dep = dependency('libhandy-1', version: '>= 0.83.0')
wingpanel_dep = dependency('wingpanel', version: '>=2.1.0')
posix_dep = meson.get_compiler('vala').find_library('posix')

plank_dep = dependency('plank', required: false)
if plank_dep.version().version_compare('>=0.10.9')
    add_project_arguments('--define=HAS_PLANK', language: 'vala')
endif

zeitgeist_dep = []

if get_option('with-zeitgeist')
    zeitgeist_dep = dependency('zeitgeist-2.0')
    add_project_arguments('--define=HAVE_ZEITGEIST', language: 'vala')
endif

config_data = configuration_data()
config_data.set_quoted('LOCALEDIR', join_paths(get_option('prefix'), get_option('localedir')))
config_data.set_quoted('GETTEXT_PACKAGE', meson.project_name())
config_file = configure_file(
    input: 'src/Config.vala.in',
    output: '@BASENAME@',
    configuration: config_data
)

subdir('src')
subdir('data')
subdir('po')

meson.add_install_script('meson/post_install.py')
