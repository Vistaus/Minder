project('com.github.phase1geo.minder', ['vala', 'c'],
    version : '1.7.3',
    license: 'GPL-3.0'
)

add_project_arguments([
        '-DGETTEXT_PACKAGE="@0@"'.format(meson.project_name())
    ],
    language: 'c',
)

gnome = import('gnome')
i18n = import('i18n')

gresource = gnome.compile_resources(
    'minder-resources', 'data/' + meson.project_name() + '.gresource.xml',
    source_dir: 'data',
)

conf = configuration_data()
conf.set_quoted('GETTEXT_PACKAGE', meson.project_name())
configure_file(output: 'config.h', configuration: conf)
config_h_dir = include_directories('.')

c_args = [
  '-include', 'config.h',
]

sources = files()

subdir('data')
subdir('po')
subdir('src')
subdir('tests')

# If the GTK version is higher than 3.22, add a define to indicate that
gtk_dep = dependency('gtk+-3.0')
if gtk_dep.version().version_compare('>=3.22.0')
  add_project_arguments(['--define=GTK322'], language: 'vala')
endif

# If the libarchive version is greater than 3.3.3, add a define for cross-compile purposes
archive_dep = dependency('libarchive')
if archive_dep.version().version_compare('>=3.3.3')
  add_project_arguments(['--define=LIBARCHIVE340'], language: 'vala')
  warning('Using LIBARCHIVE: ' + archive_dep.version())
endif

# Add the math library
cc = meson.get_compiler('c')
math_dep = cc.find_library('m', required: false)

# Add the Markdown library
cc = meson.get_compiler('c')
libmarkdown = cc.find_library('markdown', required: true)

add_project_arguments([
        '--vapidir', join_paths(meson.current_source_dir(), 'vapi')
    ],
    language: 'vala',
)

dependencies = [
    dependency('gobject-2.0'),
    dependency('glib-2.0'),
    dependency('cairo'),
    dependency('gee-0.8'),
    dependency('granite'),
    dependency('gtk+-3.0'),
    dependency('libxml-2.0'),
    dependency('libarchive'),
    dependency('gtksourceview-3.0'),
    libmarkdown,
    math_dep
]

e = executable('com.github.phase1geo.minder',
    sources,
    gresource,
    c_args: c_args,
    dependencies: dependencies,
    vala_args: [
        meson.source_root() + '/vapi/libmarkdown.vapi'
    ],
    install : true
)

#test('tests', e, args : ['--run-tests'])

meson.add_install_script('meson/post_install.py')
