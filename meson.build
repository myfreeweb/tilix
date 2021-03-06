project(
    'com.gexperts.Tilix', 'd',
    version: '1.9.3',
    license: 'MPL2',
    default_options: 'buildtype=debugoptimized',
    meson_version: '>= 0.50.0'
)

compiler = meson.get_compiler('d')

message('Using compiler ' + compiler.get_id())

if compiler.get_id() == 'gcc'
    error('Tilix can not be compiled with GDC at time, since the standard library version of all GDC versions is currently (2016) too old. Sorry.')
endif

 if compiler.get_id() == 'llvm'
  extra_args = ['-vcolumns','-d-version=StdLoggerDisableTrace']
  d_link_args = []
else
  extra_args = []
  d_link_args = []
endif

gnome = import('gnome')
i18n = import('i18n')

prefix = get_option('prefix')
bindir = prefix / get_option('bindir')
datadir = prefix / get_option('datadir')
pkgdatadir = datadir / 'tilix' # TODO: this probably should be meson.project_name() eventually
schemadir = datadir / 'glib-2.0' / 'schemas'
iconsdir = datadir / 'icons' / 'hicolor'
appdir = datadir / 'applications'

tilix_sources = [
    'source/gx/gtk/actions.d',
    'source/gx/gtk/cairo.d',
    'source/gx/gtk/clipboard.d',
    'source/gx/gtk/color.d',
    'source/gx/gtk/dialog.d',
    'source/gx/gtk/resource.d',
    'source/gx/gtk/settings.d',
    'source/gx/gtk/threads.d',
    'source/gx/gtk/util.d',
    'source/gx/gtk/vte.d',
    'source/gx/gtk/x11.d',
    'source/gx/i18n/l10n.d',
    'source/gx/tilix/bookmark/bmchooser.d',
    'source/gx/tilix/bookmark/bmeditor.d',
    'source/gx/tilix/bookmark/bmtreeview.d',
    'source/gx/tilix/bookmark/manager.d',
    'source/gx/tilix/prefeditor/advdialog.d',
    'source/gx/tilix/prefeditor/bookmarkeditor.d',
    'source/gx/tilix/prefeditor/common.d',
    'source/gx/tilix/prefeditor/prefdialog.d',
    'source/gx/tilix/prefeditor/profileeditor.d',
    'source/gx/tilix/prefeditor/titleeditor.d',
    'source/gx/tilix/terminal/actions.d',
    'source/gx/tilix/terminal/activeprocess.d',
    'source/gx/tilix/terminal/advpaste.d',
    'source/gx/tilix/terminal/exvte.d',
    'source/gx/tilix/terminal/layout.d',
    'source/gx/tilix/terminal/monitor.d',
    'source/gx/tilix/terminal/password.d',
    'source/gx/tilix/terminal/regex.d',
    'source/gx/tilix/terminal/search.d',
    'source/gx/tilix/terminal/terminal.d',
    'source/gx/tilix/terminal/util.d',
    'source/gx/tilix/application.d',
    'source/gx/tilix/appwindow.d',
    'source/gx/tilix/closedialog.d',
    'source/gx/tilix/cmdparams.d',
    'source/gx/tilix/colorschemes.d',
    'source/gx/tilix/common.d',
    'source/gx/tilix/constants.d',
    'source/gx/tilix/customtitle.d',
    'source/gx/tilix/encoding.d',
    'source/gx/tilix/preferences.d',
    'source/gx/tilix/session.d',
    'source/gx/tilix/shortcuts.d',
    'source/gx/tilix/sidebar.d',
    'source/gx/util/array.d',
    'source/gx/util/path.d',
    'source/gx/util/string.d',
    'source/secret/Collection.d',
    'source/secret/Item.d',
    'source/secret/Prompt.d',
    'source/secret/Schema.d',
    'source/secret/SchemaAttribute.d',
    'source/secret/Secret.d',
    'source/secret/Service.d',
    'source/secret/Value.d',
    'source/secretc/secret.d',
    'source/secretc/secrettypes.d',
    'source/x11/X.d',
    'source/x11/Xlib.d',
    'source/app.d'
]

source_root = meson.source_root()
sources_dir = include_directories('source/')

# Dependencies
gtkd_dep = dependency('gtkd-3', version: '>=3.8.5')
vted_dep = dependency('vted-3', version: '>=3.8.5')
xlib_dep = dependency('x11')
libunwind_dep = dependency('libunwind')

subdir('po')
subdir('data')

# Build & Test
executable('tilix',
    [tilix_sources, gresource],
    include_directories : sources_dir,
    dependencies : [gtkd_dep,
                    vted_dep,
                    xlib_dep,
                    libunwind_dep],
    d_args: extra_args,
    link_args: d_link_args,
    install : true
)

# FIXME: Unittests are currently broken
#tilix_test_exe = executable('tilix_test',
#    [tilix_sources],
#    include_directories : [sources_dir],
#    dependencies : [gtkd_dep,
#                    vted_dep,
#                    xlib_dep],
#    d_args: compiler.unittest_args()
#)
#test('tilix_test', tilix_test_exe)

meson.add_install_script('meson_post_install.py')
