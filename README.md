# DMR Craftbox

DMR Craftbox is a desktop application for **producing DMR bursts and streams
with a sequencer**. It is made for **testing, learning and experimenting with
the DMR protocol designed by ETSI** (ETSI TS 102 361).

You build individual bursts in a parameter editor, add them to a sequencer
list, and then play that list out as a real signal — to a sound device, over
`rtl_tcp`, or into an IQ file.

<img width="1102" height="975" alt="Screen" src="https://github.com/user-attachments/assets/e064d501-3232-4e40-a32e-8bdfec01521b" />

---

## Features

### Burst editing

Fifteen burst types, each with its own parameter page. These are the exact
labels in the **Burst type** selector:

| | | |
|---|---|---|
| `VoiceLCHeader` | `DataHeader` | `Rate12Data` |
| `Rate34Data` | `Rate1Data` | `CSBK` |
| `MBCHeader` | `MBCContinuation` | `Idle` |
| `TerminatorWithLC` | `PIHeader` | `USBD` |
| `AppendedData` | `VoiceBurst` | `RcSync` |

Framing modes: **MS-Sourced**, **BS-Sourced**, **TDMA1**, **TDMA2** and
**RC Sync**, with an optional Reverse Channel row.

### Sequencer

Add, reorder, edit and save bursts as a sequence. Every item keeps the burst
it was built from, so double-clicking a row puts it back into the editor.
Sequences are saved to a binary `.dmrseq` file, which also carries the
toolbar and CACH state the bursts were built with.

### Outputs

DMR Craftbox can send the generated signal out three different ways:

| Output | What it does |
|---|---|
| **rtl_tcp** | Runs an `rtl_tcp` listener — point SDR# / SDR++ (or any rtl_tcp client) at it for a live stream |
| **IQ WAV file** | Produces a RIFF/WAVE file of complex baseband IQ, plus a JSON sidecar |
| **Sound device** | Streams straight into the machine's own local audio device |

**Save WAV** writes a single burst as an ordinary audio file, and
**Save Sequence WAV** writes the whole sequence — both work independently of
the selected output.

### IQ options

IQ export and `rtl_tcp` streaming share one set of IQ options: format,
frequency, sample rate, FM deviation, audio low-pass, IQ level and output
directory.

- The IQ functions have been **tested with SDR# and SDR++**.
- The **Mono PCM** option has been **tested with dsd-fme, dsd-neo**.
- The Audio device output has been **tested with dsd-fme, dsd-neo, DMRDecode**.

The **Format** combo offers exactly these five entries:

| Format | For |
|---|---|
| `signed 16-bit PCM - I = ch1, Q = ch2 (SDR#, SDR++)` | SDR# and SDR++ |
| `signed 16-bit PCM - Q = ch1, I = ch2` | swapped channel order |
| `32-bit float PCM - I = ch1, Q = ch2` | float, in-phase first |
| `32-bit float PCM - Q = ch1, I = ch2` | float, swapped |
| `Mono PCM (dsd-fme, dsd-neo, ...)` | single-channel 48 kHz baseband |

> **Note:** if you change the IQ format to the SDR format, the **IQ sample
> rate has to match the sample rate of the SDR** you are feeding.

---

## Translations

The application is **ready for translations**. The UI language is picked in
*Settings → Language*; compiled `dmr-craftbox_<locale>.qm` files are loaded
from the `translations/` folder next to the executable.

In the **release archives** (`.zip` for Windows, `.tar.gz` for Linux) you
will find everything a translator needs in the `translations/` folder next to
the executable — on Linux that is inside the `dmr-craftbox-linux/` directory:

```
translations/
├── dmr-craftbox_en.csv      # spreadsheet of every UI string
├── dmr-craftbox_en.ts       # Qt Linguist source
├── export_csv.py            # .ts -> .csv
├── import_csv.py            # .csv -> .ts
└── README.md                # full translator guide
```

The **HTML help file** is shipped too, under `help/`, so translators can see
each string in context:

```
help/
├── dmr-craftbox-help.html       # offline help
├── dmr-craftbox-help-index.json # search index
└── README.md
```

See translations/README.md for the step-by-step
workflow (CSV round-trip, Qt Linguist, and `lrelease`).

---

## Platforms

Tested on:

- **Windows**
- **Linux (Debian)**

### Linux notes

On Debian the Qt **5.15** runtime libraries are required — they are not part
of the archive:

```
sudo apt install libqt5widgets5 libqt5gui5 libqt5core5a libqt5network5 \
                 qt5-qpa-plugins
```

Then unpack the archive and run `./dmr-craftbox` from inside it. The DMR
protocol library (`libdmr.so`) and the offline help are already in the
archive, and audio backends (ALSA / PulseAudio) are loaded at runtime, so no
audio development packages are needed.

Windows ships self-contained: the archive contains the Qt DLLs, the MinGW
runtime and the platform plugins, so no separate install is required.

Full details are in `README-Linux.txt` inside the Linux archive.

---

## License

Copyright (c) **2026 Mastodon987**. All rights reserved.

DMR Craftbox is distributed under the
**Free Non-Commercial Software License v1.0** — free of charge for
non-commercial use. See [`LICENSE.md`](LICENSE.md) for the full text, also
available inside the application under *Help → License*.

Qt is used under the **GNU Lesser General Public License v3 (LGPLv3)** and is
dynamically linked; Qt is © The Qt Company Ltd.
