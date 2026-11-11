# MBROLA NG - language data

The language sources of [MBROLA NG](https://github.com/Mbrola-NG/Mbrola-NG):
one folder per language.

| Folder | Language |
|--------|----------|
| `hr/`  | Croatian (also read for Serbian, Bosnian and Montenegrin text) |
| `en/`  | English (US pronunciation) |
| `es/`  | Spanish (Spain and Latin America) |

This repository is a submodule of the main repository (`languages/`). A
language is **text only**: no code is needed to add one. The engine contains
no knowledge of any language; `langc` (the language compiler, built in the
main repository) turns a folder into `<code>.dat`, and every compile runs the
folder's `tests.txt` through the real engine.

## What is in a language folder

Every folder holds the same 21 files:

    abbreviations  accents   acronyms  classes   clitics   dates     emoji
    g2p            language  lexicon   numbers   numcontext phonemes postlex
    prosody        scripts   spelling  syllables symbols    tests     units

(all `.txt`). A folder is recognized as a language by its `language.txt`.
The formats are described in `ANALYSIS.txt` of the main repository
(sections 8 and 9, Appendices B and D). The quickest start is to copy the
folder of a similar language and edit it.

## Working on a language

This repository cannot be compiled on its own. Clone the main repository with
its submodules and work in `languages/`:

    git clone --recurse-submodules https://github.com/Mbrola-NG/Mbrola-NG
    build.cmd x64                                         (Windows; or python build.py core)

    build\x64\langc languages\xx -o xx.dat                compile and run tests.txt
    build\x64\mbtts -l languages\xx --phonemes "text"     check the pronunciation
    build\x64\mbtts -l xx.dat --wav out.wav -v path\to\voice "text"

`tests.txt` is the contract of a language: write tests for what you add,
including words that have caused trouble. A language whose tests fail does not
compile, and a language that does not compile blocks every build.

Every pull request is compiled and tested on GitHub with the engine of the
main repository (`.github/workflows/test.yml`), so you see at once whether
your tests pass.

## Adding a language (for example `de`)

1. **This repository.** Add `languages/de/` with a `language.txt` and the other
   files. Nothing else needs to be registered: the build scripts (`build.cmd`,
   CMake, `build.py`, the installer, the NVDA add-on, the Android and Apple
   builds) pick up every folder that has a `language.txt`.
2. **[Resources](https://github.com/Mbrola-NG/Resources)**: without an entry in
   `catalog/catalog.json` the language is compiled but never shown in an app.
   Add the language (`code`, `sapi_lcid`, `android_locale`, `apple_locale`,
   `names`, the list of its voices) and each voice (files with SHA-256, license
   text and summary, and a `phoneme_map` when the voice uses other symbols than
   the language).
3. **Voice samples**: add a sentence for the language to `TEXT` in
   `tools/make_voice_samples.py` of the main repository, run it, and put the
   printed `"sample"` entries into the catalog (see the README of Resources).
4. **Optional, in the main repository**: the sentences of the "Try" button
   (`SampleTexts` in `Engine.kt` and `PreviewPlayer.swift`; other languages
   fall back to English) and the list of languages that
   `apple/Tests/EngineTests.swift` expects.

A pull request for a new language therefore touches this repository, Resources
and, for steps 3 and 4, the main repository. Say so in the description and
link the three.

## Licenses

The language data is licensed under the **GNU General Public License,
version 2 or (at your option) any later version** (`LICENSE`), the license of
MBROLA NG itself. New files carry the header

    # Copyright (c) <year> <your name>
    # SPDX-License-Identifier: GPL-2.0-or-later

By sending a pull request you agree that your contribution is released under
that license.

Some data comes from other sources and keeps its own license; **the notice in
the file header must stay with the data** and is reproduced in the packages:

| File | Source | License |
|------|--------|---------|
| `en/lexicon.txt` | CMU Pronouncing Dictionary | BSD-style (Carnegie Mellon University) |
| `*/emoji.txt` | Unicode CLDR emoji names | Unicode License v3 |

Do not add word lists or pronunciations copied from sources whose license does
not allow redistribution under these terms (most commercial dictionaries), and
name the source and license of any data you convert in the file header.

MBROLA **voices** are not part of this repository or of MBROLA NG. Each voice
has its own license, usually "only with MBROLA, not for commercial use".
