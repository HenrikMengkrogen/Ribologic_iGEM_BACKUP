# Team Aarhus University 2026 Software

If your team competes in the [**Software & AI** village](https://villages.igem.org) or wants to
apply for the [**Best Software** award](https://competition.igem.org/judging/awards/special), you **MUST** host all the
source code of your team's software tool in this repository, `main` branch. By the **Wiki Freeze**, a
[release](https://docs.gitlab.com/ee/user/project/releases/) will be automatically created as the judging artifact of
this software tool. You will be able to keep working on your software after the Grand Jamboree.

See the [Software Project](https://teams.igem.org/go/deliverables/software) page for the full requirements (including the
open-source license requirement; note that **Software & AI** village teams are not eligible for the Best Software award).

> If your team does not have any software tool, you can totally ignore this repository. If left unchanged, this
repository will be automatically deleted by the end of the season.

> **Using an AI assistant (e.g. Claude Code)?** Please read [.claude/RESPONSIBLE_AI_USE.md](.claude/RESPONSIBLE_AI_USE.md) first. You remain fully responsible for everything you commit: don't misrepresent what your tool does, never commit secrets, and review every change.

## Description
Let people know what your project can do specifically. Provide context and add a link to any reference visitors might
be unfamiliar with (for example your team wiki). A list of Features or a Background subsection can also be added here.
If there are alternatives to your project, this is a good place to list differentiating factors.

## Installation
Within a particular ecosystem, there may be a common way of installing things, such as using Yarn, NuGet, or Homebrew.
However, consider the possibility that whoever is reading your README is a novice and would like more guidance. Listing
specific steps helps remove ambiguity and gets people to using your project as quickly as possible. If it only runs in a
specific context like a particular programming language version or operating system or has dependencies that have to be
installed manually, also add a Requirements subsection.

## Usage
Use examples liberally, and show the expected output if you can. It's helpful to have inline the smallest example of
usage that you can demonstrate, while providing links to more sophisticated examples if they are too long to reasonably
include in the README.

## Data and large files
Keep this repository for **source code**. For datasets, machine-learning model weights, large media, and other heavy
artifacts, use [Zenodo](https://teams.igem.org/go/deliverables/software/zenodo) — it gives each upload a citable DOI and
is the recommended long-term archive for iGEM teams. Reference your Zenodo records from this README so judges and future
teams can find them.

## Contributing
State if you are open to contributions and what your requirements are for accepting them.

For people who want to make changes to your project, it's helpful to have some documentation on how to get started.
Perhaps there is a script that they should run or some environment variables that they need to set. Make these steps
explicit. These instructions could also be useful to your future self.

You can also document commands to lint the code or run tests. These steps help to ensure high code quality and reduce
the likelihood that the changes inadvertently break something. Having instructions for running tests is especially
helpful if it requires external setup, such as starting a Selenium server for testing in a browser.

## Authors and acknowledgment
Show your appreciation to those who have contributed to the project.

## License
This repository is licensed under the [Apache License 2.0](LICENSE) — a permissive
open-source license recommended for software (Creative Commons licenses are *not*
intended for source code). You are free to use, modify, and distribute this software,
provided you keep the license and attribution notices. If you prefer different terms
for your own tool, you may replace this license, but it must remain an
[OSI-approved open-source license](https://opensource.org/licenses).


# Ribologic RNA Mutation Program

A Rust-based RNA sequence design tool built with the ViennaRNA library.

The program uses ViennaRNA folding and minimum-free-energy (MFE) algorithms to generate RNA sequences that match a supplied dot-bracket secondary structure.

> **Note:** The `python/` folder and Python script are legacy files and are not used by the current program.

## Features

The program supports two sequence-generation modes:

1. **Generate from ambiguous nucleotides**  
   Generate sequences from an input sequence containing ambiguous RNA nucleotide symbols such as:

   - `N` — any nucleotide
   - `K` — `G` or `U`
   - `S` — `G` or `C`

2. **Generate from a preferred starting sequence**  
   When `RIBOSOMAL_RNA=True` is enabled in the program configuration, generation begins from the ribosomal large-subunit rRNA sequence, or what ever query sequence of desire.

   The output includes a percentage score indicating how much of the original sequence remains in the generated sequence.

## Supported platforms

The program is cross-platform. `setup.sh` detects your OS/CPU and stages the correct prebuilt (or freshly built) native libraries automatically.

| Platform | Architecture | Status |
|---|---|---|
| macOS | Intel (`x86_64-apple-darwin`) | Tested in CI |
| macOS | Apple Silicon (`aarch64-apple-darwin`) | Tested in CI |
| Linux | `x86_64-unknown-linux-gnu` |  Tested in CI |
| Linux | `aarch64-unknown-linux-gnu` | Should be supported by `setup.sh`, but not yet CI-tested |
| Windows | `x86_64-pc-windows-gnu` (via MSYS2 MinGW64) | Tested in CI |
| WSL2 | Treated as Linux | Supported by `setup.sh` |

On macOS and Linux, ViennaRNA is built from source by `setup.sh` since prebuilt static archives aren't distributed for these targets; GSL, MPFR, and GMP are pulled from your package manager (Homebrew or your Linux distro's package manager). On Windows, all native libraries (including ViennaRNA) are built from source using MSYS2 MinGW64.

## Requirements

### macOS

- Intel or Apple Silicon Mac
- Xcode Command Line Tools (`xcode-select --install`)
- Homebrew (installed automatically by `setup.sh` if missing)
- Git
- Approximately 500 MB of free disk space for the Rust toolchain and build files

### Linux

- `x86_64` or `arm64` Linux
- Git
- Rust toolchain
- Clang and `libclang` development files, required by Rust `bindgen`
- A supported package manager: `apt`, `dnf`, `yum`, `pacman`, or `zypper`

For Ubuntu or Debian-based Linux distributions:

```bash
sudo apt-get update
sudo apt-get install -y \
  build-essential \
  clang \
  libclang-dev \
  pkg-config \
  git \
  curl
```

### Windows

- Windows 10/11, `x86_64`
- [MSYS2](https://www.msys2.org/) installed
- Git (available inside the MSYS2 MinGW64 shell, or installed separately)
- The build must be run from the **"MSYS2 MinGW x64"** terminal specifically — not PowerShell, CMD, Git Bash, WSL, or the plain MSYS2 terminal

## Quick start: macOS

Clone the repository and run the setup script:
```bash
git clone https://gitlab.igem.org/2026/software/aarhus-university/ribologic-rna-sequence-generator.git && cd ribologic-rna-sequence-generator && bash setup.sh
```
The setup script installs required tools when needed and builds the project. This works the same way on both Intel and Apple Silicon Macs — `setup.sh` detects your CPU architecture and targets it automatically.

If Rust was not installed automatically, install it with:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
After installation, restart your terminal or load Rust into the current shell:
```bash
source "$HOME/.cargo/env"
```

## Quick start: Linux

Install the system requirements shown above, then install Rust if necessary:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
```
```bash
git clone https://gitlab.igem.org/2026/software/aarhus-university/ribologic-rna-sequence-generator.git
cd ribologic-rna-sequence-generator
./setup.sh
cargo run
```
This works on both `x86_64` and `arm64` Linux; `setup.sh` detects your architecture and builds ViennaRNA from source if a prebuilt archive isn't already vendored.

## Quick start: Windows (MSYS2 MinGW64)

1. Install [MSYS2](https://www.msys2.org/).
2. Open the **"MSYS2 MinGW x64"** terminal from the Start menu (this specific shell is required).
3. Clone the repository and run the setup script:
   ```bash
   git clone https://gitlab.igem.org/2026/software/aarhus-university/ribologic-rna-sequence-generator.git
   cd ribologic-rna-sequence-generator
   ./setup.sh
   ```
   This installs the MinGW toolchain, GMP, MPFR, and GSL via `pacman`, builds ViennaRNA from source, and adds the `x86_64-pc-windows-gnu` Rust target.
4. Run the program, targeting the GNU toolchain:
   ```bash
   cargo run --target x86_64-pc-windows-gnu
   ```

> Native Windows support currently covers `x86_64` only.

## Running the program

Run this command from the repository root, the directory containing `Cargo.toml`:
```bash
cargo run
```
For an optimized release build:
```bash
cargo run --release
```
On Windows, pass `--target x86_64-pc-windows-gnu` to both commands.

After the script starts running you will be asked to declare how many rounds it will run. Write a number and press enter.
If you dont write anything the default is set to three rounds.

After this you will be asked how many rounds in parallel the multi_start_hill_climb_design() will run. The default here is set to four.
Remember the more runs you decide to have the more cores you will use so choose this with consideration.

Once the script is finished running you will have the option to view the data as a scrollable element in terminal (y/n). 
This is rather useful instead of having to go trough multiple runs and files to find your desired candidate(s).

## Input and output files
Input files are located in:
```bash
main/misc/
```
They are named with the input_ prefix.

Each input file must contain:

* An RNA sequence
* A matching RNA secondary structure in dot-bracket notation

For example:
```bash
Sequence: GGGAAACCC
Structure : (((...)))
```
The sequence and structure must have the same length
Generated output files are written to:
```bash
main/misc/output/
```

## Native dependencies
The project uses the following native libraries:

* ViennaRNA — RNA secondary-structure prediction and MFE folding
* GMP — GNU Multiple Precision Arithmetic Library
* MPFR — multiple-precision floating-point arithmetic
* GSL — GNU Scientific Library

On macOS, Linux, and Windows, `setup.sh` builds and/or copies these into a target-specific directory under:
```bash
vendor/RNAlib/prebuilt/
```
Platform-specific libraries are stored in directories such as:
```bash
vendor/RNAlib/prebuilt/x86_64-unknown-linux-gnu/
vendor/RNAlib/prebuilt/aarch64-apple-darwin/
vendor/RNAlib/prebuilt/x86_64-pc-windows-gnu/
```
The Rust build script (`build.rs`) selects the correct prebuilt library directory for the current target platform.

## Continuous integration
Every push and pull request is built and tested across all supported platforms — macOS Intel, macOS Apple Silicon, Linux x86_64, and Windows x86_64 (MSYS2 MinGW64) — via GitHub Actions. See `.github/workflows/test-all-platforms.yml`.

## Project Structure
```bash
Ribosome_mut_program/
├── Cargo.toml
├── Cargo.lock
├── build.rs
├── setup.sh
├── src/
│   └── main.rs
├── main/
│   └── misc/
│       ├── input_*.txt
│       └── output/
├── vendor/
│   └── RNAlib/
│       ├── include/
│       │   └── ViennaRNA/          # ViennaRNA C headers
│       └── prebuilt/
│           ├── x86_64-apple-darwin/
│           ├── aarch64-apple-darwin/
│           ├── x86_64-unknown-linux-gnu/
│           ├── aarch64-unknown-linux-gnu/
│           │   ├── libRNA.a
│           │   ├── libgmp.a
│           │   ├── libmpfr.a
│           │   ├── libgsl.a
│           │   └── libgslcblas.a
│           └── x86_64-pc-windows-gnu/
│               ├── libRNA.a
│               ├── libgmp.a
│               ├── libmpfr.a
│               ├── libgsl.a
│               └── libgslcblas.a
└── .github/
    └── workflows/
        ├── build-linux-viennarna.yml
        └── test-all-platforms.yml

```


