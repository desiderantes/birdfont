birdfont_test_exe = executable(
    'birdfont-test',
    files('TestRunner.vala'),
    dependencies: [libbirdfont_dep],
)

if get_option('tests')
    test(
        'svg-inkscape',
        birdfont_test_exe,
        args: ['SVG', files('inkscape.svg')],
        workdir: meson.project_source_root(),
        suite: 'integration',
    )

    test(
        'svg-illustrator',
        birdfont_test_exe,
        args: ['SVG', files('illustrator.svg')],
        workdir: meson.project_source_root(),
        suite: 'integration',
    )

    test(
        'bf-example',
        birdfont_test_exe,
        args: [
            'BF',
            meson.project_source_root() / 'fixed-kerning-example/fk-example.birdfont',
        ],
        workdir: meson.project_source_root(),
        suite: 'integration',
    )

    benchmark(
        'speed',
        birdfont_test_exe,
        args: ['speed'],
        workdir: meson.project_source_root(),
        suite: 'benchmark',
    )
endif

if get_option('fuzzing')
    test(
        'fuzz-svg-inkscape',
        birdfont_test_exe,
        args: ['SVG', files('inkscape.svg')],
        workdir: meson.project_source_root(),
        suite: 'fuzz',
    )

    test(
        'fuzz-svg-illustrator',
        birdfont_test_exe,
        args: ['SVG', files('illustrator.svg')],
        workdir: meson.project_source_root(),
        suite: 'fuzz',
    )

    test(
        'fuzz-bf-seed',
        birdfont_test_exe,
        args: [
            'BF',
            meson.project_source_root() / 'fixed-kerning-example/fk-example.birdfont',
        ],
        workdir: meson.project_source_root(),
        suite: 'fuzz',
    )

    afl_fuzz = find_program('afl-fuzz', required: false)

    if afl_fuzz.found()
        fuzz_svg_cmd = '''
            set -e
            SEED_DIR="@0@/fuzz_svg_in"
            OUT_DIR="@0@/fuzz_svg_out"
            mkdir -p "$SEED_DIR" "$OUT_DIR"
            cp "@1@" "@2@" "$SEED_DIR/"
            EXTRA_ARGS=""
            for dict in /usr/share/afl/dictionaries/xml.dict /usr/share/doc/american-fuzzy-lop/dictionaries/xml.dict; do
              if [ -f "$dict" ]; then
                EXTRA_ARGS="$EXTRA_ARGS -x $dict"
                break
              fi
            done
            if ! grep -qa "__AFL_SHM_ID" "@3@" "@4@" 2>/dev/null; then
              echo "Note: Binary built without AFL++ instrumentation; running in non-instrumented mode (-n)."
              echo "Tip: Reconfigure with CC=afl-clang-fast or CC=afl-gcc-fast for coverage-guided fuzzing."
              EXTRA_ARGS="$EXTRA_ARGS -n"
            fi
            export AFL_SKIP_CPUFREQ=1
            export AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1
            exec "@5@" -i "$SEED_DIR" -o "$OUT_DIR" $EXTRA_ARGS -- "@3@" SVG @@
        '''.format(
            meson.current_build_dir(),
            meson.current_source_dir() / 'inkscape.svg',
            meson.current_source_dir() / 'illustrator.svg',
            birdfont_test_exe.full_path(),
            libbirdfont.full_path(),
            afl_fuzz.full_path(),
        )

        run_target(
            'fuzz-svg',
            command: [find_program('sh'), '-c', fuzz_svg_cmd],
            depends: [birdfont_test_exe, libbirdfont],
        )

        fuzz_bf_cmd = '''
            set -e
            SEED_DIR="@0@/fuzz_bf_in"
            OUT_DIR="@0@/fuzz_bf_out"
            mkdir -p "$SEED_DIR" "$OUT_DIR"
            cp "@1@" "$SEED_DIR/"
            EXTRA_ARGS=""
            for dict in /usr/share/afl/dictionaries/xml.dict /usr/share/doc/american-fuzzy-lop/dictionaries/xml.dict; do
              if [ -f "$dict" ]; then
                EXTRA_ARGS="$EXTRA_ARGS -x $dict"
                break
              fi
            done
            if ! grep -qa "__AFL_SHM_ID" "@2@" "@3@" 2>/dev/null; then
              echo "Note: Binary built without AFL++ instrumentation; running in non-instrumented mode (-n)."
              echo "Tip: Reconfigure with CC=afl-clang-fast or CC=afl-gcc-fast for coverage-guided fuzzing."
              EXTRA_ARGS="$EXTRA_ARGS -n"
            fi
            export AFL_SKIP_CPUFREQ=1
            export AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1
            exec "@4@" -i "$SEED_DIR" -o "$OUT_DIR" $EXTRA_ARGS -- "@2@" BF @@
        '''.format(
            meson.current_build_dir(),
            meson.project_source_root() / 'fixed-kerning-example/fk-example.birdfont',
            birdfont_test_exe.full_path(),
            libbirdfont.full_path(),
            afl_fuzz.full_path(),
        )

        run_target(
            'fuzz-bf',
            command: [find_program('sh'), '-c', fuzz_bf_cmd],
            depends: [birdfont_test_exe, libbirdfont],
        )
    else
        foreach target_name : ['fuzz-svg', 'fuzz-bf']
            run_target(
                target_name,
                command: [
                    find_program('sh'),
                    '-c',
                    'echo "afl-fuzz was not found in PATH. Please install AFL++ (afl++ / american-fuzzy-lop) to run iterative fuzzing."; exit 1',
                ],
                depends: birdfont_test_exe,
            )
        endforeach
    endif
endif
