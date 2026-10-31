install_data(
    'icons.birdfont',
    'bright.theme',
    'dark.theme',
    'high_contrast.theme',
    'key_bindings.xml',
    'ucd.sqlite',
    'codepages.sqlite',
    'Roboto-Regular.ttf',
    install_dir: get_option('datadir') / 'birdfont',
)

install_data(
    'birdfont-logo.svg',
    rename: 'birdfont.svg',
    install_dir: get_option('datadir') / 'icons/hicolor/scalable/apps',
)

# Added this for completion but I think this does not work anymore
if get_option('apport')
    install_data(
        'birdfont.py',
        'source_birdfont.py',
        install_dir: get_option('datadir') / 'apport/package-hooks',
    )
    install_data(
        'birdfont-crashdb.conf',
        install_dir: get_option('sysconfdir') / 'apport/crashdb.conf.d',
    )
endif

if host_machine.system() == 'linux'
    subdir('linux')
endif
