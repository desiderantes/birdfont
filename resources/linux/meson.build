install_data(
    'birdfont_window_icon.png',
    install_dir: get_option('datadir') / 'birdfont',
)

desktop_file = i18n.merge_file(
    input: 'birdfont.desktop',
    output: 'birdfont.desktop',
    type: 'desktop',
    po_dir: '../../po',
    install: true,
    install_dir: get_option('datadir') / 'applications',
)

desktop_utils = find_program('desktop-file-validate', required: false)
if desktop_utils.found()
    test('Validate desktop file', desktop_utils, args: [desktop_file])
endif

appstream_file = i18n.merge_file(
    input: 'birdfont.appdata.xml',
    output: 'birdfont.appdata.xml',
    po_dir: '../../po',
    install: true,
    install_dir: get_option('datadir') / 'metainfo',
)

appstreamcli = find_program('appstreamcli', required: false, disabler: true)
test(
    'Validate appstream file',
    appstreamcli,
    args: ['validate', '--no-net', '--explain', appstream_file],
)

install_data(
    'birdfont.xml',
    install_dir: get_option('datadir') / 'mime/packages',
)

foreach size : ['48x48', '128x128', '256x256']
    install_data(
        size / 'birdfont.png',
        install_dir: get_option('datadir') / 'icons/hicolor' / size / 'apps',
    )
endforeach

man_pages = ['birdfont.1']
if get_option('cli_tools')
    man_pages += [
        'birdfont-autotrace.1',
        'birdfont-export.1',
        'birdfont-import.1',
    ]
endif
install_man(man_pages)

gnome.post_install(
    gtk_update_icon_cache: true,
    update_desktop_database: true,
    update_mime_database: true,
)
