project(
    'birdfont',
    'vala',
    'c',
    version: '2.33.6',
    license: 'GPL-3.0-or-later',
    meson_version: '>= 1.11.0',
    default_options: [
        'warning_level=1',
        'buildtype=debugoptimized',
    ],
)

gnome = import('gnome')
i18n = import('i18n')

libbirdfont_so_version = '36.0'
libbirdfont_so_version_major = '36'

libbirdgems_so_version = '0.0'
libbirdgems_so_version_major = '0'

vala_args = ['--enable-experimental']

if get_option('experimental_non_null')
    vala_args += ['--enable-experimental-non-null']
endif

add_project_arguments(vala_args, language: 'vala')
add_project_arguments('-DGETTEXT_PACKAGE="birdfont"', language: 'c')

cc = meson.get_compiler('c')
valac = meson.get_compiler('vala')

m_dep = cc.find_library('m', required: false)
posix_dep = valac.find_library('posix')

glib_dep = dependency('glib-2.0')
gobject_dep = dependency('gobject-2.0')
gio_dep = dependency('gio-2.0')
cairo_dep = dependency('cairo')
cairo_gobject_dep = dependency('cairo-gobject')
gdk_pixbuf_dep = dependency('gdk-pixbuf-2.0')
sqlite3_dep = dependency('sqlite3')
fontconfig_dep = dependency('fontconfig')
freetype_dep = dependency('freetype2')

gee_dep = dependency('gee-0.8', required: false)
if not gee_dep.found()
    gee_dep = dependency('gee-1.0')
endif

xmlbird_dep = dependency(
    'xmlbird',
    version: '>= 1.2.0',
    fallback: ['xmlbird', 'xmlbird_dep'],
    default_options: [
        'tests=false',
        'fuzzing=false',
    ],
)

gtk_dep = dependency('gtk+-3.0')
webkit_dep = dependency('webkit2gtk-4.1')
libnotify_dep = dependency('libnotify')

loadfont_vapi = files('loadfont.vapi')

subdir('libbirdgems')
subdir('libbirdfont')
subdir('birdfont')

if get_option('cli_tools')
    subdir('birdfont-autotrace')
    subdir('birdfont-export')
    subdir('birdfont-import')
endif

if get_option('tests') or get_option('fuzzing')
    subdir('birdfont-test')
endif

if get_option('tests')
    subdir('fixed-kerning-example')
endif

subdir('po')
subdir('resources')
