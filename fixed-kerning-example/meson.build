kerning_example_exe = executable(
    'kerning-example',
    files(
        'fk.c',
        'fk-font-editor.c',
        'fk-test.c',
    ),
    dependencies: [m_dep],
)

test(
    'fixed-kerning',
    kerning_example_exe,
    workdir: meson.current_build_dir(),
    suite: 'unit',
)
