birdgems_sources = files(
    'birdgems.vala',
    'fit_cubic.c',
    'GGVecLib.c',
)

libbirdgems = shared_library(
    'libbirdgems',
    birdgems_sources,
    name_prefix: '',
    dependencies: [glib_dep, gobject_dep, posix_dep, m_dep],
    vala_header: 'birdgems.h',
    vala_vapi: 'libbirdgems.vapi',
    version: libbirdgems_so_version,
    soversion: libbirdgems_so_version_major,
    install: true,
)

libbirdgems_dep = declare_dependency(
    link_with: libbirdgems,
    include_directories: include_directories('.'),
    dependencies: [glib_dep, gobject_dep],
)
