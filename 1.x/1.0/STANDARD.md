# THE NIX FOUNDATION — NIX STANDARD

**Document ID:** `NIX-STAN-001`

**Version:** `1.0`

**Certification Program:** `NIX Certified` (`https://github.com/The-NIX-Foundation/NIX-Certified`)

**Scope:** Independent operating systems, distributions, kernels, subsystems, libraries, and userland utilities seeking NIX Foundation conformance

**Language:** English (Normative)

**Compliance Level:** Mandatory within the selected profile (`MUST`, `MUST NOT`, `SHALL`, `SHALL NOT`)

---

## 1. NORMATIVE PREAMBLE AND CERTIFICATION GOVERNANCE

This document defines **The NIX Standard (`NIX-STAN-001`)**, an open, vendor-neutral technical specification maintained by **The NIX Foundation** to certify authentic UNIX-compliant software and operating systems worldwide. Analogous in governance to the Single UNIX Specification—yet strictly dedicated to genuine UNIX minimalism, software freedom, and zero-telemetry engineering—this standard establishes the exact criteria required for any third-party or community project to earn and display the **`NIX Certified`** seal.

Certification is open to **all** independent operating systems, distributions, kernels, and userland utilities—regardless of whether they are developed inside or outside The NIX Foundation—provided they conform to 100% of the normative requirements defined herein.

### 1.1 Definitions and Normative Keywords

The key words `MUST`, `MUST NOT`, `REQUIRED`, `SHALL`, `SHALL NOT`, `SHOULD`, `SHOULD NOT`, `RECOMMENDED`, `MAY`, and `OPTIONAL` in this document are to be interpreted as described in RFC 2119 and RFC 8174.

* **NIX Certified Software:** Any standalone executable, library, daemon, or subsystem—from any vendor, community, or author—that conforms to all applicable requirements of Sections 2, 3, 4, 8, 9, 10, 12, 13, 14, 15, 18, 20, 21, and 23 of this standard for its declared tier and profile.

* **NIX Certified Operating System (`NIX Certified OS`):** Any complete operating system distribution that conforms to 100% of this standard, including the NIX Filesystem Hierarchy Standard (Section 5), the Essential UNIX Core Utilities Mandate (Section 6), the Minimalist Kernel/Init Architecture (Section 7), and all UNIX runtime, terminal, process, signal, security, and operational subsystems defined throughout this specification.

* **Bloatware:** Any code, asset, dependency, daemon, background service, graphical wrapper, or abstraction layer that does not directly serve the primary, documented single responsibility of the software component, or that introduces unnecessary resource consumption (CPU cycles, resident memory, disk footprint, or startup latency) without deterministic user-invoked utility.

* **Telemetry:** Any mechanism—active, passive, synchronous, asynchronous, opt-in, opt-out, anonymized, or aggregated—that collects, serializes, stores, or transmits behavioral, diagnostic, usage, hardware-fingerprinting, crash-reporting, or operational metadata from a local system to an external or remote endpoint without an explicit, synchronous, user-invoked invocation of a standard network utility (e.g., `curl`, `ssh`, `rsync`).

* **Process Group (`PGID`) and Session (`SID`):** Fundamental UNIX process organization units. A session groups one or more process groups associated with a single controlling terminal (or detached from any terminal), while a process group coordinates foreground/background job control and signal delivery (`killpg(3)`).

* **Controlling Terminal (`CTTY`):** The terminal device (`termios` line discipline) associated with a session leader, mediating line editing (`ICANON`), echo (`ECHO`), interrupt (`VINTR`), quit (`VQUIT`), suspend (`VSUSP`), and `SIGHUP` hangup delivery upon session termination.

### 1.2 Alignment with The NIX Foundation Manifesto

In accordance with **The NIX Foundation Manifesto (v1.0)**, the `NIX Certified` mark guarantees four inviolable properties to every end user:

1. **User Sovereignty Over Execution:** The human operator possesses absolute, unmediated authority over the machine. Software acts solely as an explicit instrument of user intent, never as an autonomous agent acting on behalf of vendors, maintainers, or remote infrastructure.

2. **Radical Transparency and Inspectability:** Every state transition, configuration parameter, inter-process message, and filesystem mutation must be observable and modifiable using standard, human-readable text tools.

3. **Minimalism as a Security Primitive:** Code that does not exist cannot contain vulnerabilities, cannot leak data, and cannot fail. Complexity is treated as a defect until proven mathematically or operationally unavoidable.

4. **Permanence and Predictability:** Interfaces, command-line flags, exit codes, and file formats must remain stable across releases. Breaking backwards compatibility without a major architectural revision is prohibited.

### 1.3 Relationship to UNIX, POSIX, SUS, BSD, and Linux

`UNIX` is a registered certification and trademark ecosystem administered by The Open Group. A product may claim to be a UNIX system only when it is entitled to do so under the applicable trademark and certification rules. NIX Foundation certification does not confer that entitlement.

`POSIX` specifies portable operating-system interfaces and utility behavior. The Single UNIX Specification (SUS) incorporates and extends relevant standards and defines the basis for formal UNIX product certification. A NIX profile may select POSIX editions and options, but NIX conformance is not equivalent to passing the SUS certification process.

BSD systems and Linux systems are not interchangeable implementations. They expose different kernel interfaces, device-management facilities, service-management conventions, filesystem layouts, and administrative tools. A portable standard must define a common required interface and explicitly identify implementation-specific extensions.

NIX Certified therefore means **conformant to the published NIX profile**, not “official UNIX”, “UNIX-certified”, or “100% UNIX”. A certified product's public claim MUST use the exact NIX certification tier and version recorded in the registry.

### 1.4 Normative precedence and interpretation

The selected conformance profile determines which platform-specific requirements apply. Within a profile, the specific requirement takes precedence over a general design principle only when the specific requirement explicitly identifies the principle it qualifies. Sections 16 through 24 clarify scope and verification; they do not silently cancel earlier requirements. If two normative clauses cannot be reconciled under this rule, the requirement is ambiguous and MUST be corrected by a published revision before it is used to fail a candidate. Requirements MUST be testable, scoped, and justified by a published interface or an explicitly identified NIX policy choice.

A requirement MUST NOT be described as “POSIX-required” unless the cited POSIX edition actually requires it. A NIX policy requirement MUST be labelled as such. An implementation-specific feature MUST be labelled Linux-specific, BSD-specific, or otherwise platform-specific.

The terms “UNIX-like”, “UNIX philosophy”, “POSIX-conforming”, “SUS-conforming”, and “UNIX-certified” MUST NOT be used as synonyms. Documentation MUST state which of these claims is being made and what evidence supports it.

### 1.5 Conformance classes

NIX certification distinguishes source-level portability, utility conformance, library/API conformance, kernel-interface conformance, and complete operating-system conformance. A product MUST NOT claim a higher class merely because one component passes a lower class.

A conformance report MUST identify the exact release, source revision, build configuration, target architecture, ABI, enabled options, test-suite revision, exceptions, and test environment.

### 1.6 Normative references

The certification authority SHOULD maintain a dated bibliography of the POSIX edition, SUS edition, ISO C edition, relevant RFCs, filesystem specifications, and platform ABI documents used by each profile. References MUST include stable titles and publication identifiers where available.

Implementations MUST follow the edition named by their selected profile. A newer edition MAY be supported, but MUST NOT silently change the required baseline for an already-certified profile.

---

## 2. THE UNIX PHILOSOPHY MANDATE (AXIOMATIC ENGINEERING RULES)

Every software artifact seeking **`NIX Certified`** status **MUST** strictly obey all classical and modern formulations of the UNIX Philosophy (McIlroy, Thompson, Ritchie, Pike, Raymond, and Gancarz). Deviations for "convenience," "modernity," or "abstraction" automatically disqualify the candidate from certification.

### 2.1 The Seventeen Canonical Axioms of NIX Certified Software

#### Axiom 1: Modularity and Single Responsibility

* Every program **MUST** do one thing and do it well.

* When a new requirement emerges, engineers **MUST** build a fresh, standalone tool rather than complicating existing programs by adding new, unrelated features or monolithic subcommands.

* Components that couple unrelated responsibilities without documented necessity, independent configuration, or meaningful administrative control SHOULD be avoided. An init or service-management suite MUST be evaluated by its actual architecture, enabled components, dependencies, and behaviour—not by its name or project identity. No init system is prohibited solely because it is commonly described as “monolithic”.

* Multicall userland binaries (such as `BusyBox` or `Toybox`) that dispatch independent, single-purpose UNIX applets based on `argv[0]` strictly to reduce storage or memory footprint on constrained systems are permitted, provided each invoked applet preserves clean UNIX CLI semantics, exit codes, and stream behavior.

#### Axiom 2: Composition and Pipeability

* Every program **MUST** expect its output to become the input to another, as yet unknown, program.

* Programs **MUST NOT** clutter output with extraneous banners, ASCII art headers, decorative borders, progress bars, or interactive prompts when `stdout` is not attached to a terminal (`!isatty(STDOUT_FILENO)`).

* Utilities that process streams SHOULD support standard input and output where this is meaningful. A utility MUST follow the operand and stream behavior specified by its governing standard; `-` is not a universal POSIX operand convention. Diagnostics SHOULD be written to standard error.

#### Axiom 3: Plain Text as the Universal Interface

* Software **MUST** store configuration, state, logs, and inter-process data in plain, line-oriented, UTF-8 / ASCII text formats.

* Proprietary binary configuration databases, opaque registry hives, and undocumented binary log formats are strictly forbidden.

* Text streams **MUST** be structured so they can be parsed trivially using standard NIX filters (`grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `tr`).

#### Axiom 4: The Rule of Silence

* When a program has nothing surprising, erroneous, or explicitly requested to say, it **MUST** say nothing.

* Successful execution of state-mutating commands (such as `cp`, `mv`, `rm`, `mkdir`, `chmod`, `chown`, `ln`) **MUST** produce zero bytes on `stdout` and `stderr` unless verbose mode (`-v`) is explicitly requested by the operator.

#### Axiom 5: The Rule of Repair

* When a program must fail, it **MUST** fail noisily, immediately, and as early as possible.

* Programs **MUST NOT** attempt silent fallback heuristics, guess malformed user intent, or continue executing in a corrupted state.

* Every failure **MUST** emit a concise, actionable diagnostic message to `stderr` formatted as:
  `<program_name>: <context/target>: <strerror_or_reason>\n`
  and immediately terminate with a non-zero exit code (`1` through `125`, adhering to sysexits conventions where applicable).

#### Axiom 6: The Rule of Separation (Policy vs. Mechanism)

* Software **MUST** cleanly separate low-level mechanism (how an operation is performed) from high-level policy (when, why, and for whom it is performed).

* Core libraries and daemons **MUST** expose clean, minimal C ABIs or text-based IPC sockets without hardcoding administrative policies.

#### Axiom 7: The Rule of Simplicity and Clarity

* Engineers **MUST** design for simplicity; complexity is permitted only where it is strictly mandated by the problem domain.

* Clarity of implementation **MUST** always take precedence over cleverness, micro-optimizations that obscure control flow, or deep inheritance hierarchies.

* Explicit control flow (procedural C99/C11 or POSIX shell) is preferred over implicit magic, reflection, runtime code generation, or heavy macro metaprogramming.

#### Axiom 8: The Rule of Parsimony

* Large programs **MUST** be written only when it is demonstrably clear by measurement and architectural proof that nothing else can solve the problem.

* If a task can be accomplished in 50 lines of POSIX shell or 200 lines of C using existing pipes, creating a 50,000-line framework daemon is a certification failure.

#### Axiom 9: The Rule of Transparency and Visibility

* Software **MUST** be designed to make inspection, debugging, and tracing effortless.

* Internal state **SHOULD** be exposed via virtual filesystems (`/proc`, `/sys`), plain-text status files, or deterministic Unix domain sockets (`AF_UNIX`) rather than opaque RPC protocols.

#### Axiom 10: The Rule of Robustness

* Robustness is the child of transparency and simplicity, never of defensive bloat.

* All input buffers, environment variables, command-line arguments, and file descriptors **MUST** be bounds-checked and validated at system boundaries without dynamic unbounded allocations.

#### Axiom 11: The Rule of Representation

* Knowledge **MUST** be folded into data structures and plain-text tables so program logic remains flat, simple, and robust.

* Hardcoding policy rules, file paths, or translation tables inside imperative `if/else` chains is prohibited where declarative configuration tables suffice.

#### Axiom 12: The Rule of Least Surprise

* In interface design, software **MUST** always do the least surprising thing.

* Standard flags **MUST** retain their historical UNIX semantics across all `NIX Certified` tools:

  * `-f`: Force / suppress non-existent file errors

  * `-i`: Interactive confirmation before destructive action

  * `-r` / `-R`: Recursive directory traversal

  * `-v`: Verbose diagnostic output

  * `-n`: Dry-run / numeric output / line numbers (context-standard)

  * `-q`: Quiet / suppress standard output

  * `-h`: Human-readable sizes (when paired with size outputs) or standard help (when sole flag)

  * `--`: End of option flags; all subsequent arguments treated strictly as positional operands

#### Axiom 13: The Rule of Economy

* Programmer time and operator cognitive load are scarce resources; software **MUST** automate repetitive labor through composable scripts and standard build tools (`make`), never through wizard-driven GUIs.

#### Axiom 14: The Rule of Generation

* Hand-hacking repetitive boilerplate is prohibited; engineers **MUST** write programs to generate code, parsers, and lookup tables (`lex`/`flex`, `yacc`/`bison`, `awk`, `m4`) whenever deterministic generation reduces human error.

#### Axiom 15: The Rule of Optimization

* Prototype first in clean, portable code; polish and optimize only after profiling proves a bottleneck exists. Make it work, make it right, make it fast—strictly in that order.

#### Axiom 16: The Rule of Diversity

* Software **MUST** distrust all claims of "one true way." Certified systems **MUST** support pluggable shells, alternative text editors, replaceable init systems, and multiple filesystems without artificial coupling.

#### Axiom 17: The Rule of Extensibility

* Software **MUST** be designed for the future, ensuring data formats and protocols include clean extension hooks (such as `#` comments, `key=value` pairs, and `*.d/` drop-in configuration directories) so future tooling can interoperate without breaking existing parsers.

---

## 3. OPEN SOURCE LICENSING AND SOFTWARE FREEDOM MANDATE

Every line of code, documentation, build script, and asset distributed in a **`NIX Certified`** product **MUST** guarantee complete, unrestricted user freedom.

### 3.1 Approved Licenses

NIX certification MUST publish its actual freedom policy rather than imply that one short allow-list is universally sufficient. Unless a stricter product profile is declared, software MUST use a licence that grants the rights to use, study, modify, and redistribute the covered work. The licence MUST be identified with an SPDX identifier where one exists.

Permissive examples include ISC, BSD-2-Clause, BSD-3-Clause, and MIT. Copyleft examples include GPL-2.0-only, GPL-2.0-or-later, GPL-3.0-only, GPL-3.0-or-later, LGPL-2.1-only, LGPL-2.1-or-later, LGPL-3.0-only, and LGPL-3.0-or-later. These examples are not an exhaustive legal compatibility determination.

The project MUST preserve third-party notices and comply with the exact terms of every dependency. A licence MUST NOT be labelled “OSI-approved” or “FSF-approved” unless that status is verified against the relevant organisation's current list. Source availability alone does not make a licence free or open source.

### 3.2 Prohibition of Proprietary Code and Peripheral Firmware Policy

* **Zero Proprietary CPU Executables or Kernel Modules:** `NIX Certified` distributions and software **MUST NOT** include, download, unpack, or depend upon out-of-tree proprietary kernel modules (`.ko` / kernel loadable objects), closed-source userland binaries, or obfuscated shared objects (`.so` / `.a`) executing on the host CPU.

* **Passive Peripheral Firmware Exception:** Passive microcode or firmware blobs stored in `/lib/firmware` for peripheral hardware initialization (such as Wi-Fi chipsets, GPUs, or storage controllers) **MAY** be permitted only when strictly required to initialize physical hardware and governed by a free redistribution license (such as the upstream `linux-firmware` redistributable licenses). The core operating system **MUST** boot and operate its base system without requiring proprietary firmware blobs.

* **Reproducible Builds:** Every binary artifact in a certified release **SHOULD** be bit-for-bit reproducible from its published source tree using documented open-source toolchains (`SOURCE_DATE_EPOCH` compliance and deterministic archive timestamps).

* **Complete Source Availability:** Every certified binary release **MUST** provide public, unrestricted access to its exact corresponding source code, build recipes (`Makefile` / `configure`), and cryptographic detached signatures (Ed25519 / OpenPGP).

### 3.3 Right to Inspect, Modify, and Redistribute

* No technical restriction, cryptographic lock, or legal clause in a `NIX Certified` artifact **MAY** forbid a user from reading source code, patching binaries, recompiling with custom flags, running modified kernels, or redistributing derivative works under compatible open-source licenses.

---

## 4. ZERO-BLOATWARE AND ZERO-TELEMETRY ENFORCEMENT POLICY

To hold the **`NIX Certified`** seal, a system or application **MUST** enforce a strict prohibition on undisclosed telemetry and unjustified bloatware, subject only to formal exceptions published under Section 22.3 across every layer—from bootloader to userland shell.

### 4.1 Zero-Telemetry Mandate (Absolute Prohibition)

1. **No Network Phone-Home:** No `NIX Certified` software component **MAY** initiate an outbound network connection unless the user explicitly invoked a network-oriented command (e.g., `ssh`, `ping`, `curl`, `wget`, `pkg`) with an explicit target host or configured repository.

2. **No Crash Reporters or Analytics:** Automatic crash uploaders, anonymous usage counters, installation UUID generators, update-check pings, captive-portal background probes, and hardcoded DNS-over-HTTPS fallback resolvers are **STRICTLY FORBIDDEN**.

3. **No Hardware Fingerprinting:** Software **MUST NOT** hash MAC addresses, CPU serial numbers, motherboard UUIDs, or disk serial numbers to generate persistent machine identifiers for external tracking. (`/etc/machine-id`, if present solely for local compatibility, **MUST** be ephemeral or zeroed unless explicitly configured by the administrator).

4. **Compile-Time and Runtime Verification:**

   * Verification MUST combine source review, dependency inspection, configuration review, runtime observation, packet capture where appropriate, and documented threat modelling. Symbol scans and string searches MAY identify leads but MUST NOT be treated as proof that a program does or does not communicate.

   * Local networking, user-requested network operations, server software, package retrieval, time synchronisation, and protocol implementations MUST be evaluated according to declared function and user/admin configuration. The mere presence of a socket API or TLS library is not proof of telemetry.

   * Undisclosed collection or transmission of usage, diagnostic, behavioural, or device-identifying data is prohibited by this policy. A verified violation triggers suspension and a documented review; revocation follows the published appeal and correction process.

### 4.2 Zero-Bloatware Architecture Standards

1. **Dependency Minimalism:**

   * Core userland utilities **MUST** link solely against the system C standard library (`libc`, e.g., musl libc or BSD libc) and POSIX math/realtime libraries (`libm`, `librt`) where strictly required.

   * Core utilities **MUST NOT** depend on C++ runtimes, Python, Perl, Node.js, JVM, Electron, D-Bus, GLib, or XML/JSON parsing libraries for basic system operations.

   * Utilities explicitly designated as network tools in Section 6.6 (`curl`, `wget`, `ssh`, `sshd`, `scp`) **MAY** link against minimalist, reviewable TLS libraries (such as `libtls`, `LibreSSL`, or `BearSSL`).

2. **System Profiles and Footprint Measurements:**

   * Individual core utilities SHOULD be measured and their binary sizes reported for the target architecture, ABI, compiler, and build mode. The `64 KiB` dynamically linked and `128 KiB` statically linked figures are non-binding engineering targets unless a published footprint profile explicitly adopts them.

   * **`NIX Certified Core OS` (Base Runtime Profile):** A candidate MAY request this optional footprint profile. If it does, the profile MUST publish a reproducible hardware/VM baseline, included package set, filesystem/image accounting method, boot target, and memory measurement method. Any numeric disk or RAM ceiling applies only when explicitly published in that profile revision; it is not a universal NIX OS requirement.

   * **`NIX Certified Dev OS` (Development Profile):** A candidate MAY request this optional footprint profile. If it does, the profile MUST define the included compiler, build tools, headers, manuals, optional data packs, disk accounting method, and runtime measurement conditions. Numeric ceilings apply only when explicitly published in that profile revision and MUST be justified by repeatable measurements.

3. **No Unsolicited Background Services:**

   * An idle `NIX Certified OS` installation MUST NOT run optional background services—such as indexing, desktop search, automatic package updates, or sound brokers—unless the service is required by the declared installation role or explicitly enabled by the administrator. The enablement mechanism MUST be the native, documented mechanism of the selected platform and service manager; `/etc/rc.conf` and `/etc/sv/` are examples, not universal required paths.

---

## 5. NIX OPERATING SYSTEM FILESYSTEM HIERARCHY STANDARD (NIX-FHS)

**Profile note:** The directory tree in this section is a reference profile. Linux-only entries are not requirements for BSD or other kernels. A conforming implementation MUST publish its mapping and MUST NOT create meaningless compatibility directories solely to satisfy a checklist.

A NIX OS profile defines a portable base hierarchy and separately lists platform-specific paths. It MUST NOT require Linux virtual filesystems or Linux device-management directories from a BSD implementation. The tree below is a reference layout, not a claim that every listed path is mandated by POSIX or shared by historical UNIX systems. Each profile MUST state which paths are required, optional, or platform-specific.

### 5.1 Complete Canonical Directory Tree

```text
/
├── bin/                        # Essential user command binaries (available in single-user mode)
├── boot/                       # Static files of the boot loader and kernel images
│   ├── grub/                   # Bootloader configuration and modules (if GRUB is used)
│   └── efi/                    # EFI System Partition (ESP) mount point
├── dev/                        # Device nodes (character, block, pseudo-devices)
│   ├── block/                  # Symlinks to block devices by major:minor [Linux-based implementations only]
│   ├── char/                   # Symlinks to character devices by major:minor [Linux-based implementations only]
│   ├── disk/                   # Persistent block device symlinks
│   │   ├── by-id/              # Hardware/serial-based device links
│   │   ├── by-label/           # Filesystem label-based device links
│   │   ├── by-partuuid/        # GPT partition UUID device links
│   │   └── by-uuid/            # Filesystem UUID device links
│   ├── fd/                     # Symlink to /proc/self/fd (file descriptors 0, 1, 2...)
│   ├── input/                  # Input event devices (kbd, mice, eventX)
│   ├── mapper/                 # Device-mapper control and encrypted/LVM volumes
│   ├── net/                    # TUN/TAP network tunnel devices
│   ├── pts/                    # Pseudo-terminal slave devices (devpts)
│   ├── shm/                    # POSIX shared memory (tmpfs)
│   └── snd/                    # ALSA/OSS sound device nodes
├── etc/                        # Host-specific system-wide plain-text configuration
│   ├── cron.d/                 # Modular periodic cron job definitions
│   ├── cron.daily/             # Daily maintenance scripts
│   ├── cron.hourly/            # Hourly maintenance scripts
│   ├── cron.monthly/           # Monthly maintenance scripts
│   ├── cron.weekly/            # Weekly maintenance scripts
│   ├── default/                # Default parameter overrides for system scripts
│   ├── init.d/                 # System V / BSD rc service control scripts
│   ├── modprobe.d/             # Kernel module loading options and blacklists [Linux-based implementations only]
│   ├── network/                # Static network interface definitions
│   │   ├── if-down.d/          # Scripts executed after interface teardown
│   │   ├── if-post-down.d/     # Post-teardown hooks
│   │   ├── if-pre-up.d/        # Pre-initialization hooks
│   │   └── if-up.d/            # Scripts executed after interface bring-up
│   ├── profile.d/              # Modular POSIX shell login environment scripts
│   ├── rc.d/                   # Runlevel / BSD-style startup scripts
│   ├── security/               # Resource limits (limits.conf) and access controls
│   ├── skel/                   # Template skeleton files copied to new user $HOME
│   ├── ssh/                    # OpenSSH daemon and client configuration & host keys
│   ├── ssl/                    # TLS root certificates and local PKI configuration
│   │   ├── certs/              # Trusted PEM root CA certificates
│   │   └── private/            # Mode 0700 directory for host private keys
│   ├── sv/                     # Service supervision directories (runit / s6 style)
│   ├── sysctl.d/               # Modular kernel runtime parameter overrides
│   └── udev/                   # Device manager rules (mdev / eudev) [Linux-based implementations only]
│       └── rules.d/            # Administrator custom device rules [Linux-based implementations only]
├── home/                       # User home directories
│   └── <username>/             # Per-user personal files and dotfiles
│       ├── .cache/             # Non-essential user cached data
│       ├── .config/            # User-specific application configs
│       ├── .local/             # User-specific data and executables
│       │   ├── bin/            # User-installed local binaries ($PATH)
│       │   ├── lib/            # User-installed local libraries
│       │   └── share/          # User-specific architecture-independent data
│       └── .ssh/               # Mode 0700 user SSH keys and authorized_keys
├── lib/                        # Essential shared libraries and kernel modules
│   ├── firmware/               # Passive hardware microcode/firmware images (redistributable only)
│   ├── modules/                # Loadable kernel modules
│   │   └── <kernel-release>/   # Version-matched kernel object tree (.ko / kernel modules)
│   └── security/               # PAM/NSS shared object modules
├── lib64/                      # 64-bit dynamic linker (ld-musl-x86_64.so.1 / ld-linux)
├── media/                      # Mount points for removable media (cdrom, usb)
├── mnt/                        # Temporary mount point for ad-hoc administrator mounts
├── opt/                        # Add-on self-contained application software packages
├── proc/                       # Kernel and process information virtual filesystem (procfs)
│   ├── bus/                    # Kernel bus information (pci, usb, input)
│   ├── driver/                 # Active kernel driver status files
│   ├── fs/                     # Mounted filesystem tunables and statistics
│   ├── irq/                    # Hardware interrupt routing and affinity
│   ├── net/                    # Network stack tables (arp, dev, route, tcp, udp)
│   ├── self/                   # Symlink to current process PID directory
│   ├── sys/                    # Runtime writable kernel parameters (sysctl)
│   │   ├── fs/                 # Filesystem limits (file-max, inotify)
│   │   ├── kernel/             # Core kernel tunables (hostname, pid_max, panic)
│   │   ├── net/                # IPv4/IPv6 networking stack parameters
│   │   └── vm/                 # Virtual memory and swap tunables
│   └── <pid>/                  # Per-process runtime state (cmdline, env, fd, maps, status)
├── root/                       # Home directory for the superuser (UID 0)
├── run/                        # Volatile runtime variable data (tmpfs, cleared on boot)
│   ├── lock/                   # Device and subsystem lock files
│   ├── mount/                  # Userspace mount state (utab)
│   ├── service/                # Active symlinks to supervised services
│   └── user/                   # Per-UID runtime directories (mode 0700)
├── sbin/                       # Essential system administration binaries (superuser tools)
├── srv/                        # Site-specific data served by this system
│   ├── ftp/                    # FTP server root data
│   ├── git/                    # Bare Git repositories served over SSH/HTTP
│   └── www/                    # HTTP/HTTPS web root directories
├── sys/                        # Kernel device model and hardware topology (sysfs) [Linux-based implementations only]
│   ├── block/                  # Block devices exposed by the kernel [Linux-based implementations only]
│   ├── bus/                    # Hardware buses (pci, usb, platform, i2c, spi) [Linux-based implementations only]
│   ├── class/                  # Device classes (net, tty, block, thermal, power_supply) [Linux-based implementations only]
│   ├── dev/                    # Major:minor device number mappings [Linux-based implementations only]
│   ├── devices/                # Canonical physical device hierarchy tree [Linux-based implementations only]
│   ├── firmware/               # ACPI, Devicetree, and EFI firmware tables [Linux-based implementations only]
│   ├── fs/                     # Filesystem-specific sysfs attributes (cgroup, ext4, fuse) [Linux-based implementations only]
│   ├── kernel/                 # Kernel debugging, security, and livepatch attributes [Linux-based implementations only]
│   ├── module/                 # Loaded kernel module parameters and reference counts [Linux-based implementations only]
│   └── power/                  # System sleep states and power management controls [Linux-based implementations only]
├── tmp/                        # World-writable temporary files (mode 1777, sticky bit)
├── usr/                        # Secondary hierarchy for shareable, read-only user data
│   ├── bin/                    # Non-essential user command binaries
│   ├── include/                # Standard C header files
│   │   ├── arpa/               # Internet protocol definitions (inet.h)
│   │   ├── asm/                # Architecture-specific kernel UAPI headers
│   │   ├── bits/               # Libc architecture/implementation headers
│   │   ├── linux/              # Kernel user-space API headers [Linux-based implementations only]
│   │   ├── net/                # Network interface headers (if.h, route.h)
│   │   ├── netinet/            # TCP/IP protocol headers (in.h, ip.h, tcp.h, udp.h)
│   │   └── sys/                # POSIX system headers (types.h, stat.h, socket.h, wait.h)
│   ├── lib/                    # Object files, static libraries (.a), and shared libraries (.so)
│   │   └── pkgconfig/          # Library compilation metadata (.pc files)
│   ├── libexec/                # Internal binaries executed by other programs (not in $PATH)
│   ├── local/                  # Tertiary hierarchy for local administrator-compiled software
│   │   ├── bin/                # Locally compiled executables
│   │   ├── etc/                # Configuration files for /usr/local software
│   │   ├── include/            # Locally installed header files
│   │   ├── lib/                # Locally installed libraries
│   │   ├── man/                # Locally installed manual pages
│   │   ├── sbin/               # Locally compiled system administration binaries
│   │   ├── share/              # Locally installed architecture-independent data
│   │   └── src/                # Local source code builds
│   ├── sbin/                   # Non-essential system administration binaries and daemons
│   ├── share/                  # Architecture-independent shared data
│   │   ├── doc/                # Package documentation and READMEs
│   │   ├── licenses/           # Full open-source license texts per installed package
│   │   ├── man/                # UNIX Manual Pages hierarchy
│   │   │   ├── man1/           # Section 1: User commands
│   │   │   ├── man2/           # Section 2: System calls
│   │   │   ├── man3/           # Section 3: C library functions
│   │   │   ├── man4/           # Section 4: Special files and device drivers (/dev)
│   │   │   ├── man5/           # Section 5: File formats and conventions (/etc)
│   │   │   ├── man6/           # Section 6: Games and screensavers
│   │   │   ├── man7/           # Section 7: Overviews, conventions, and miscellaneous
│   │   │   └── man8/           # Section 8: System administration commands (/sbin)
│   │   ├── misc/               # Miscellaneous architecture-independent data
│   │   ├── terminfo/           # Compiled terminal capability database
│   │   └── zoneinfo/           # IANA timezone database files
│   └── src/                    # System and kernel source code trees
└── var/                        # Variable data files persisting across reboots
    ├── backups/                # Automated backups of critical system files (/etc/passwd)
    ├── cache/                  # Application cache data (safe to delete without data loss)
    ├── empty/                  # Mode 0555 empty chroot jail directory for privilege separation
    ├── lib/                    # Persistent state information modified by programs
    │   └── pkg/                # Installed package database (plain-text manifests)
    ├── local/                  # Variable data for /usr/local software
    ├── lock/                   # Symlink to /run/lock
    ├── log/                    # Plain-text system and daemon log files
    │   ├── auth.log            # Authentication and privilege escalation logs
    │   ├── boot.log            # System initialization transcript
    │   ├── cron.log            # Scheduled task execution logs
    │   ├── dmesg               # Kernel ring buffer boot snapshot
    │   ├── kern.log            # Kernel messages
    │   ├── messages            # General non-critical system messages
    │   ├── syslog              # Master plain-text system log
    │   └── wtmp                # Login/logout historical accounting records
    ├── mail/                   # User mbox mail spools (/var/mail/<username>)
    ├── opt/                    # Variable data for /opt packages
    ├── run/                    # Symlink to /run
    ├── spool/                  # Tasks awaiting processing
    │   ├── cron/               # User crontabs (/var/spool/cron/crontabs)
    │   └── mail/               # Symlink to /var/mail
    └── tmp/                    # Temporary files preserved between system reboots (mode 1777)
```

### 5.2 Essential `/etc` Configuration Files Specification

Every **`NIX Certified OS`** installation **MUST** populate `/etc` with standard, human-readable, plain-text configuration files:

* `/etc/passwd` — Colon-delimited user account database (`name:password:UID:GID:GECOS:directory:shell`).

* `/etc/shadow` — Mode `0600` (owned by `root:shadow`) password hashes and aging policy.

* `/etc/group` — Colon-delimited group definitions (`group_name:password:GID:user_list`).

* `/etc/gshadow` — Mode `0600` secure group password and administrator definitions.

* `/etc/fstab` — Static filesystem mount table (`<fs_spec> <mount_point> <type> <options> <dump> <pass>`).

* `/etc/hosts` — Static IP-to-hostname lookup table (`127.0.0.1 localhost`).

* `/etc/hostname` — Single-line canonical system hostname.

* `/etc/resolv.conf` — DNS resolver nameserver and search domain directives.

* `/etc/nsswitch.conf` — Name Service Switch database resolution order (`files dns`).

* `/etc/services` — IANA network service names and port/protocol mappings.

* `/etc/protocols` — Internet protocol names and numbers (`ip`, `icmp`, `tcp`, `udp`).

* `/etc/profile` — System-wide POSIX Bourne shell login initialization script.

* `/etc/shells` — List of valid login shells recognized by `chsh` and login daemons.

* `/etc/issue` — Pre-login terminal identification banner.

* `/etc/motd` — Post-login Message of the Day plain-text file.

* `/etc/os-release` — Key-value operating system identification metadata (`NAME`, `VERSION`, `ID`, `VERSION_ID`).

* `/etc/inittab` — System V / BusyBox init process table (`id:runlevels:action:process`).

* `/etc/rc.conf` — Master system configuration variables (hostname, keymap, network interfaces, enabled daemons).

* `/etc/sysctl.conf` — Persistent kernel runtime parameters loaded at boot via `sysctl -p`.

* `/etc/doas.conf` / `/etc/sudoers` — Mode `0440` privilege escalation rules.

* `/etc/crontab` — System-wide periodic task schedule (`minute hour dom month dow user command`).

* `/etc/timezone` & `/etc/localtime` — System timezone name and symlink to `/usr/share/zoneinfo/...`.

* `/etc/mtab` — Symlink to `/proc/self/mounts` reflecting active kernel mounts.

### 5.3 Essential `/dev` Device Nodes Specification

Every **`NIX Certified OS`** `/dev` filesystem **MUST** expose standard UNIX device nodes:

* `/dev/null` — Character device `(1, 3)` on Linux (or native null device on BSD), mode `0666` (data sink; discards writes, returns `EOF` on read).

* `/dev/zero` — Character device `(1, 5)` on Linux (or native zero device on BSD), mode `0666` (infinite stream of `0x00` bytes).

* `/dev/full` — Character device `(1, 7)` on Linux (or native full device on BSD), mode `0666` (returns `ENOSPC` on every write).

* `/dev/random` & `/dev/urandom` — Character devices `(1, 8)` and `(1, 9)` on Linux (or native CSPRNG devices on BSD), mode `0666` (kernel CSPRNG entropy sources).

* `/dev/tty` — Character device `(5, 0)` on Linux (or native controlling TTY device on BSD), mode `0666` (controlling terminal of the current process).

* `/dev/console` — Character device `(5, 1)` on Linux (or native console device on BSD), mode `0600` (system kernel console).

* `/dev/ptmx` — Character device `(5, 2)` on Linux (or native PTY master/clone device on BSD), mode `0666` (pseudo-terminal master multiplexer).

* `/dev/tty0` through `/dev/tty63` — Character devices `(4, 0..63)` on Linux (or native virtual console terminals on BSD).

* `/dev/ttyS0` through `/dev/ttyS3` — Character devices `(4, 64..67)` on Linux (or native serial UART ports such as `/dev/cuau*` / `/dev/cua*` on BSD).

* `/dev/stdin`, `/dev/stdout`, `/dev/stderr` — Symlinks to `/proc/self/fd/0`, `/proc/self/fd/1`, and `/proc/self/fd/2` (or `/dev/fd/0..2` on BSD).

* `/dev/core` — Symlink to `/proc/kcore` (on Linux) or native kernel core device where supported.

* `/dev/loop0` through `/dev/loop7` — Block devices `(7, 0..7)` on Linux (or equivalent disk/memory image devices such as `/dev/md*` / `/dev/vnd*` on BSD).

* Primary block storage devices exposed according to the native kernel naming convention (e.g., `/dev/sd*`, `/dev/nvme*`, `/dev/vd*`, or BSD `/dev/ada*`, `/dev/da*`) and numbered partitions.

### 5.4 Permissions, Ownership, and Sticky-Bit Invariants

* `/` **MUST** be owned by `root:root` with mode `0755`.

* `/tmp` and `/var/tmp` **MUST** be owned by `root:root` with mode `1777` (sticky bit enforced so users can only unlink their own files).

* `/root` **MUST** be owned by `root:root` with mode `0700`.

* `/etc/shadow` and `/etc/gshadow` **MUST** be mode `0600` (or `0640` with group `shadow`), never world-readable.

* Setuid privilege escalation binaries (`/bin/su`, `/usr/bin/doas`, or `/usr/bin/sudo`) **MUST** have mode `04755` and sanitize dynamic linker environment variables (`LD_PRELOAD`, `LD_LIBRARY_PATH`).

---

## 6. ESSENTIAL UNIX CORE UTILITIES SPECIFICATION

**Profile note:** Utility names and required behavior MUST be tied to a named POSIX/SUS edition or a declared NIX extension. GNU, BSD, BusyBox, and other implementations differ; non-standard flags MUST NOT be presented as universal UNIX semantics.

In strict adherence to the **Rule of Parsimony (Axiom 8)** and **Zero-Bloatware Mandate (Section 4.2)**, a **`NIX Certified OS`** **MUST NOT** bloat the base system with redundant, overlapping, or non-essential utilities. Instead, the base system **MUST** provide the **Essential UNIX Core Utilities**—the canonical, irreducible set of POSIX tools required to operate, administer, script, and recover an authentic UNIX environment.

Every required utility below **MUST** reside in `/bin`, `/sbin`, `/usr/bin`, or `/usr/sbin`, comply with POSIX.1-2017 flag semantics plus the canonical BSD/GNU extensions explicitly listed in the tables below, support standard pipes (`stdin`/`stdout`), and obey the Rule of Silence.

### 6.1 File and Directory Manipulation (`/bin`, `/usr/bin`)

| Command | Location | Normative Specification & Essential Flags | 
| ----- | ----- | ----- | 
| `ls` | `/bin/ls` | List directory contents (`-a`, `-A`, `-l`, `-h`, `-R`, `-t`, `-S`, `-r`, `-d`, `-i`, `-n`, `-1`, `-F`). Defaults to `-1` when `stdout` is not a TTY. | 
| `cp` | `/bin/cp` | Copy files and directories (`-r`/`-R`, `-p`, `-a`, `-f`, `-i`, `-n`, `-v`, `-L`, `-P`). | 
| `mv` | `/bin/mv` | Move or rename files across filesystems (`-f`, `-i`, `-n`, `-v`). | 
| `rm` | `/bin/rm` | Unlink files and directories (`-f`, `-i`, `-r`/`-R`, `-d`, `-v`, `--one-file-system`, mandatory `--preserve-root`). | 
| `mkdir` | `/bin/mkdir` | Create directories (`-p`, `-m <mode>`, `-v`). | 
| `rmdir` | `/bin/rmdir` | Remove empty directories (`-p`, `-v`). | 
| `touch` | `/bin/touch` | Update file access/modification timestamps or create empty files (`-a`, `-m`, `-c`, `-r <ref>`, `-t <stamp>`). | 
| `ln` | `/bin/ln` | Create hard and symbolic links (`-s`, `-f`, `-i`, `-n`, `-v`). | 
| `chmod` | `/bin/chmod` | Change file mode bits using octal or symbolic notation (`-R`, `-f`, `-v`). | 
| `chown` | `/bin/chown` | Change file owner and group (`-R`, `-h`, `-H`, `-L`, `-P`, `-f`, `-v`). | 
| `chgrp` | `/bin/chgrp` | Change file group ownership (`-R`, `-h`, `-f`, `-v`). | 
| `stat` | `/usr/bin/stat` | Display inode and filesystem metadata (`-L`, `-f`, `-c <format>`). | 
| `readlink` | `/bin/readlink` | Resolve symbolic links (`-f`, `-e`, `-m`, `-n`). | 
| `basename` | `/usr/bin/basename` | Strip directory prefix and optional suffix from filenames. | 
| `dirname` | `/usr/bin/dirname` | Strip last non-slash component from file path. | 
| `mktemp` | `/bin/mktemp` | Atomically create temporary files or directories with `0600`/`0700` permissions (`-d`, `-u`, `-p <dir>`). | 
| `dd` | `/bin/dd` | Convert and copy raw byte streams and block devices (`if=`, `of=`, `bs=`, `count=`, `skip=`, `seek=`, `conv=notrunc,noerror,sync,fsync`). | 
| `sync` | `/bin/sync` | Flush kernel filesystem write buffers to persistent storage (`sync(2)`). | 
| `mkfifo` | `/usr/bin/mkfifo` | Create named pipes (FIFOs) via `mkfifo(3)` (`-m <mode>`). | 
| `mknod` | `/bin/mknod` | Create block (`b`), character (`c`), or FIFO (`p`) device nodes (`-m <mode>`). | 

### 6.2 Text Streams, Search, and Filtering (`/bin`, `/usr/bin`)

| Command | Location | Normative Specification & Essential Flags | 
| ----- | ----- | ----- | 
| `cat` | `/bin/cat` | Concatenate files and print to `stdout` (`-n`, `-b`, `-s`, `-u`, `-v`). | 
| `head` | `/bin/head` | Output the first `N` lines or bytes of files (`-n <lines>`, `-c <bytes>`, `-q`, `-v`). | 
| `tail` | `/bin/tail` | Output the last `N` lines/bytes and follow live appends (`-n <lines>`, `-c <bytes>`, `-f`, `-F`). | 
| `grep` | `/bin/grep` | Filter lines matching Basic (`-G`), Extended (`-E`), or Fixed (`-F`) regular expressions (`-i`, `-v`, `-c`, `-l`, `-n`, `-o`, `-q`, `-r`, `-w`, `-x`). | 
| `sed` | `/bin/sed` | POSIX Stream Editor (`-n`, `-e <script>`, `-f <file>`, `-i`, `-E`). | 
| `awk` | `/usr/bin/awk` | POSIX pattern scanning and text processing language (`-F <fs>`, `-v <var>=<val>`, `-f <progfile>`). | 
| `cut` | `/usr/bin/cut` | Extract byte (`-b`), character (`-c`), or delimited field (`-f`, `-d`, `-s`) columns. | 
| `sort` | `/usr/bin/sort` | Sort lines of text files (`-n`, `-r`, `-u`, `-k <keydef>`, `-t <sep>`, `-f`, `-c`). | 
| `uniq` | `/usr/bin/uniq` | Report or omit repeated adjacent lines (`-c`, `-d`, `-u`, `-i`). | 
| `tr` | `/usr/bin/tr` | Translate, squeeze (`-s`), or delete (`-d`) characters from `stdin` to `stdout` (`-c`). | 
| `wc` | `/usr/bin/wc` | Count newlines (`-l`), words (`-w`), bytes (`-c`), and characters (`-m`). | 
| `diff` | `/usr/bin/diff` | Compare files line by line (`-u`, `-c`, `-r`, `-N`, `-q`, `-i`, `-w`). | 
| `patch` | `/usr/bin/patch` | Apply unified or context `diff` patches (`-p<num>`, `-R`, `-d <dir>`, `-i <file>`). | 
| `tee` | `/usr/bin/tee` | Duplicate `stdin` to `stdout` and one or more files (`-a`, `-i`). | 
| `xargs` | `/usr/bin/xargs` | Build and execute command lines from `stdin` (`-0`, `-I <str>`, `-n <max>`, `-P <procs>`, `-r`). | 
| `od` | `/usr/bin/od` | Dump binary/text files in octal, hexadecimal, decimal, and ASCII (`-A`, `-j`, `-N`, `-t`). | 
| `sha256sum` | `/usr/bin/sha256sum` | Compute and verify SHA-256 cryptographic digests (`-c`, `--status`). | 

### 6.3 POSIX Shell and Scripting Primitives (`/bin`, `/usr/bin`)

| Command | Location | Normative Specification & Essential Flags | 
| ----- | ----- | ----- | 
| `sh` | `/bin/sh` | Strict POSIX.1-2017 compliant Bourne shell (`dash` / `ash` / `mksh`). Startup `< 2 ms`, zero bloated dependencies. | 
| `echo` | `/bin/echo` | Write arguments to `stdout`. Supporting `-n` and `-e` is a mandatory extension beyond strict POSIX.1-2017 `echo` (while `printf` remains preferred for portable escape sequences). | 
| `printf` | `/usr/bin/printf` | Format and print data per C `printf` specifiers (`%s`, `%d`, `%x`, `%c`, `%b`). | 
| `test` / `[` | `/bin/test` | Evaluate conditional file, string, and integer expressions (`-e`, `-f`, `-d`, `-L`, `-r`, `-w`, `-x`, `-z`, `-n`, `=`, `!=`, `-eq`, `-lt`, `-gt`). | 
| `true` / `false` | `/bin/true` | Exit unconditionally with status `0` (`true`) or `1` (`false`). | 
| `env` | `/usr/bin/env` | Execute a utility in a modified environment (`-i`, `-u <name>`). | 
| `pwd` | `/bin/pwd` | Print canonical working directory path (`-L`, `-P`). | 
| `sleep` | `/bin/sleep` | Suspend execution for an interval of time (`nanosleep(2)`). | 
| `date` | `/bin/date` | Display or set system date and time (`-u`, `-d`, `-r`, `+FORMAT`). | 
| `expr` | `/usr/bin/expr` | Evaluate arithmetic, string, and POSIX regex expressions. | 

### 6.4 Process, Memory, and Identity Management (`/bin`, `/sbin`, `/usr/bin`, `/usr/sbin`)

| Command | Location | Normative Specification & Essential Flags | 
| ----- | ----- | ----- | 
| `ps` | `/bin/ps` | Report snapshot of active processes from `/proc` (`-e`, `-f`, `-l`, `-u`, `-p`, `-o`, `aux`). | 
| `top` | `/usr/bin/top` | Real-time process and resource monitor (`-b`, `-d <delay>`, `-n <iter>`, `-p <pid>`). | 
| `kill` | `/bin/kill` | Send signals to processes or process groups (`-s <sig>`, `-<sig>`, `-l`). | 
| `pgrep` / `pkill` | `/usr/bin/pgrep` | Look up or signal processes by name/attributes (`-f`, `-u`, `-x`, `-n`, `-o`). | 
| `uptime` | `/usr/bin/uptime` | Display system uptime, active user count, and load averages. | 
| `free` | `/usr/bin/free` | Display physical and swap memory usage from `/proc/meminfo` (`-b`, `-k`, `-m`, `-h`). | 
| `id` | `/usr/bin/id` | Print real and effective UID/GID values and names (`-u`, `-g`, `-G`, `-n`, `-r`). | 
| `whoami` | `/usr/bin/whoami` | Print effective user name. | 
| `who` | `/usr/bin/who` | Display currently logged-in users. | 
| `su` | `/bin/su` | Substitute user and group identity (`-`, `-l`, `-c <cmd>`, `-s <shell>`). | 
| `doas` / `sudo` | `/usr/bin/doas` | Minimalist privilege escalation governed by `/etc/doas.conf` or `/etc/sudoers`. | 
| `passwd` | `/usr/bin/passwd` | Update user authentication credentials in `/etc/shadow` (`-l`, `-u`, `-d`). | 
| `useradd` / `userdel` | `/usr/sbin/useradd` | Create and remove user accounts (`-u`, `-g`, `-G`, `-d`, `-m`, `-s`, `-r`). | 
| `groupadd` / `groupdel` | `/usr/sbin/groupadd` | Create and remove system/user groups (`-g`, `-r`). | 
| `tty` / `stty` | `/bin/stty` | Print terminal device name (`tty`) and configure terminal line settings (`stty -a`, `raw`, `cooked`, `sane`, `echo`). | 

### 6.5 Filesystem, Storage, Kernel, and Init Administration (`/bin`, `/sbin`, `/usr/sbin`)

| Command | Location | Normative Specification & Essential Flags | 
| ----- | ----- | ----- | 
| `df` | `/bin/df` | Report mounted filesystem disk space usage (`-h`, `-i`, `-k`, `-P`, `-T`). | 
| `du` | `/usr/bin/du` | Estimate file and directory space usage (`-a`, `-c`, `-d <depth>`, `-h`, `-k`, `-s`, `-x`). | 
| `mount` / `umount` | `/bin/mount` | Attach and detach filesystems (`-t <type>`, `-o <opts>`, `-r`, `-w`, `-a`, `--bind`, `-l`). | 
| `fsck` | `/sbin/fsck` | Check and repair filesystems (`-A`, `-f`, `-n`, `-p`, `-y`). | 
| `mkfs` | `/sbin/mkfs` | Format block devices with standard filesystems (`mkfs.ext4`, `mkfs.vfat`, `mkfs.xfs`). | 
| `swapon` / `swapoff` | `/sbin/swapon` | Enable and disable swap devices and files (`-a`). | 
| `fdisk` | `/sbin/fdisk` | Partition table manipulator for MBR and GPT disks. | 
| `modprobe` / `lsmod` | `/sbin/modprobe` | Load, unload (`-r`), and inspect kernel modules in `/lib/modules/$(uname -r)/` [Linux-based implementations only]. | 
| `sysctl` | `/sbin/sysctl` | Read and modify runtime kernel parameters in `/proc/sys` (`-a`, `-p`, `-w`) [or native BSD `sysctl`]. | 
| `dmesg` | `/bin/dmesg` | Inspect and control the kernel ring buffer (`-c`, `-H`, `-l`, `-T`, `-w`). | 
| `uname` | `/bin/uname` | Print kernel and machine architecture identifiers (`-a`, `-s`, `-n`, `-r`, `-v`, `-m`). | 
| `hostname` | `/bin/hostname` | Display or set the system hostname (`-f`, `-s`, `-F <file>`). | 
| `init` / `reboot` / `poweroff` / `halt` | Platform-defined | PID 1 entry point and clean system halt/reboot commands, using the platform's documented paths and interfaces. | 
| `getty` / `login` | `/sbin/getty` | Initialize TTY lines and authenticate local user sessions. | 
| `crond` / `crontab` | `/usr/sbin/crond` | Lightweight POSIX scheduled task daemon and crontab editor. | 
| `syslogd` / `logger` | `/sbin/syslogd` | Plain-text system logging daemon writing to `/var/log/` and CLI log emitter. | 

### 6.6 Search, Archival, Networking, and Text Editing (`/bin`, `/sbin`, `/usr/bin`)

| Command | Location | Normative Specification & Essential Flags | 
| ----- | ----- | ----- | 
| `find` | `/usr/bin/find` | Traverse directory trees (`-name`, `-type`, `-size`, `-mtime`, `-perm`, `-maxdepth`, `-print0`, `-exec`, `-delete`). | 
| `tar` | `/bin/tar` | POSIX `ustar`/`pax` archive utility (`-c`, `-x`, `-t`, `-f`, `-v`, `-z`, `-j`, `-J`, `-C`). Zstandard (`zstd`) support **MUST** work via external filter (`-I zstd`) or UNIX pipeline (`zstd -d \| tar -xf -`), and **MAY** also provide `--zstd`. | 
| `cpio` | `/bin/cpio` | Copy files to/from `newc`/`odc` archives for `initramfs` (`-o`, `-i`, `-d`, `-H`, `-0`). | 
| `gzip` / `xz` / `zstd` | `/bin/gzip` | Standard lossless compression/decompression utilities (`-d`, `-c`, `-k`, `-f`). | 
| `ip` | `/sbin/ip` | Configure network interfaces, IP addresses, and routing tables (`ip link`, `ip addr`, `ip route`) [Linux-based implementations; BSD-based implementations **MAY** provide `ifconfig` and `route` instead]. | 
| `ss` | `/sbin/ss` | Inspect active TCP, UDP, and UNIX domain sockets (`-t`, `-u`, `-x`, `-l`, `-a`, `-n`, `-p`) [Linux-based implementations; BSD-based implementations **MAY** provide `netstat` and/or `sockstat` instead]. | 
| `ping` | `/bin/ping` | Send ICMP `ECHO_REQUEST` packets (`-c`, `-i`, `-W`, `-I`, `-4`, `-6`). | 
| `nc` | `/usr/bin/nc` | Raw TCP, UDP, and UNIX domain socket utility (`-l`, `-p`, `-u`, `-v`, `-w`, `-z`, `-U`). | 
| `curl` / `wget` | `/usr/bin/curl` | Non-interactive HTTP/HTTPS transfer tool (zero telemetry, verifies `/etc/ssl/certs`). | 
| `ssh` / `sshd` / `scp` | `/usr/bin/ssh` | Cryptographic remote shell, daemon, and secure file copy suite. | 
| `vi` / `ed` | `/bin/vi` | POSIX visual terminal editor (`vi`) and standard UNIX line editor (`ed`). | 
| `less` | `/usr/bin/less` | Terminal file pager (`-R`, `-S`, `-N`, `-i`). | 
| `man` | `/usr/bin/man` | Format and display UNIX manual pages from `/usr/share/man` (`man 1..8`). | 
| `make` / `cc` / `ld` | `/usr/bin/make` | POSIX build automation, ISO C compiler driver, and linker toolchain. |

---

## 7. SYSTEM ARCHITECTURE, KERNEL, AND INIT SPECIFICATIONS

### 7.1 Kernel Architecture

1. **Kernel Architecture:** A `NIX Certified OS` MAY use a monolithic, modular, microkernel, hybrid, or other documented kernel architecture. The candidate MUST document its architecture, module or driver model, supported extension mechanism, and security boundaries. Linux `/lib/modules/$(uname -r)/` conventions MUST NOT be imposed on BSD or other platform profiles.

2. **Hardened Kernel Defaults:**

   * `kernel.kptr_restrict = 2` (hide kernel pointers in `/proc` and `dmesg` from unprivileged users).

   * `kernel.dmesg_restrict = 1` (restrict `dmesg` access to `CAP_SYSLOG`).

   * `kernel.yama.ptrace_scope = 1` (restrict `ptrace` attachment to direct child processes).

   * `fs.protected_hardlinks = 1` and `fs.protected_symlinks = 1` (prevent TOCTOU link attacks in `/tmp`).

   * `fs.suid_dumpable = 0` (disable core dumps for setuid binaries).

   * `net.ipv4.conf.all.rp_filter = 1` (strict reverse-path filtering against IP spoofing).

   * `net.ipv4.tcp_syncookies = 1` (SYN flood protection).

   * **Kernel Compatibility Note:** The `sysctl` parameters, `/proc`, and `/sys` paths listed above apply to Linux-based kernels. Distributions built on BSD kernels **MUST** enforce equivalent hardening controls via BSD `sysctl` settings (such as `kern.sugid_coredump=0` and `security.bsd.see_other_uids=0`) and native BSD kernel security mechanisms.

3. **Zero Proprietary Kernel Modules:** Out-of-tree proprietary or tainted kernel modules (`TAINT_PROPRIETARY_MODULE`) **SHALL NOT** be shipped in any `NIX Certified OS` release.

### 7.2 Init System and Service Supervision

**Binding policy for all profiles:** NIX does not approve or prohibit an init system by project name. `systemd`, OpenRC, SysVinit, runit, s6, BusyBox init, BSD native init/rc frameworks, and other implementations are eligible when they meet the requirements below. No other clause in this standard may be interpreted as a blanket ban on one of these projects.


1. **PID 1 Responsibilities:** The process running as PID 1 MUST provide or delegate the platform-required early userspace responsibilities, including adoption/reaping of orphaned child processes, orderly system state transitions, and coordination of boot and shutdown. Mounting filesystems, device setup, service startup, and signal handling MAY be delegated to documented helpers. The standard does not impose a source-line limit or require a particular implementation language, script layout, or stage directory. Platform-specific virtual filesystems MUST be mounted only where supported and required by that platform.

2. **Service Supervision:**

   * Long-running services MUST have a documented lifecycle mechanism that supports start, stop, status or equivalent observability, and defined failure handling. Supervision MAY be provided by the init system, a separate supervisor, native service scripts, or another documented platform mechanism. The standard does not require `/etc/sv/`, `run`/`finish` scripts, or a particular service-manager project.

   * Service logs MUST be inspectable using documented tools and MUST have documented retention and rotation behaviour. Human-readable text logs MUST be available for essential system events. Structured or binary journal formats MAY be used as supplementary stores, provided essential records are not locked exclusively in an undocumented or inaccessible format.

   * Socket activation, message buses, and service-manager APIs are not categorically prohibited. They MUST be documented, controllable by the administrator, and must not create undisclosed network activity or prevent recovery and service management through the platform's documented administrative interface.

### 7.3 Inter-Process Communication (IPC) Standards

* Local IPC **MUST** utilize standard POSIX primitives:

  1. Anonymous pipes (`pipe(2)`) and named FIFOs (`mkfifo(3)`)

  2. UNIX Domain Sockets (`AF_UNIX`, `SOCK_STREAM` / `SOCK_DGRAM`) placed in `/run/<service>.sock` with explicit filesystem permissions (`0600` or `0660`)

  3. POSIX Shared Memory (`shm_open(3)`) and Semaphores (`sem_open(3)`)

  4. Standard POSIX Signals (`SIGTERM`, `SIGHUP` for config reload, `SIGUSR1`, `SIGUSR2`)

---

## 8. PROCESS MODEL, SIGNALS, TERMINALS, AND JOB CONTROL

A genuine UNIX operating system is defined not merely by files and commands, but by its process, signal, terminal, and job-control abstractions. Every **`NIX Certified OS`** **MUST** faithfully implement these semantics.

### 8.1 Process Lifecycle and File Descriptor Inheritance
* **Fork-Exec Model:** Process creation **MUST** follow `fork(2)` (or `vfork(2)`/`clone(2)` with POSIX semantics) followed by `execve(2)` (or the `exec*` family), preserving PID/PPID relationships, current working directory, root directory (`chroot`), `umask(2)`, real/effective/saved UIDs and GIDs, and supplementary groups.
* **Close-on-Exec (`O_CLOEXEC` / `FD_CLOEXEC`):** File descriptors opened for internal housekeeping, lock files, or temporary handles **SHOULD** set `FD_CLOEXEC` so descriptors do not leak unpredictably across `execve(2)` boundaries, while standard streams (`0`, `1`, `2`) and intentional pipeline descriptors remain inherited as specified.
* **Zombie Reaping:** Parent processes that spawn children **MUST** reap exited children via `wait(2)` / `waitpid(2)` so the process table is never exhausted by orphaned zombies; PID 1 **MUST** continuously reap reparented orphans.

### 8.2 POSIX Signal Semantics
Every **`NIX Certified OS`** **MUST** support standard POSIX signal handling (`sigaction(2)`, `sigprocmask(2)`, `sigsuspend(2)`, `kill(2)`), including at minimum:
* `SIGHUP` (1): Hangup detected on controlling terminal or controlling process (conventionally triggers configuration reload in daemons).
* `SIGINT` (2): Interactive attention signal (`Ctrl-C`).
* `SIGQUIT` (3): Interactive termination signal (`Ctrl-\`), generating a core dump when permitted (`RLIMIT_CORE` > 0 and non-setuid).
* `SIGILL` (4), `SIGTRAP` (5), `SIGABRT` (6), `SIGBUS` (7), `SIGFPE` (8), `SIGSEGV` (11), `SIGSYS` (31): Hardware/software fault and abort signals.
* `SIGKILL` (9): Uncatchable, unignorable termination signal.
* `SIGPIPE` (13): Broken pipe write (writing to a pipe or socket whose reader has closed; default action terminates process so pipelines exit cleanly).
* `SIGALRM` (14): Timer expiration signal (`alarm(2)` / `setitimer(2)`).
* `SIGTERM` (15): Graceful termination request (default signal sent by `kill` when no signal is specified).
* `SIGCHLD` / `SIGCLD` (17): Child process stopped, terminated, or continued.
* `SIGCONT` (18): Continue execution if stopped.
* `SIGSTOP` (19): Uncatchable, unignorable stop signal.
* `SIGTSTP` (20): Interactive stop signal (`Ctrl-Z`).
* `SIGTTIN` (21) & `SIGTTOU` (22): Background process terminal read/write stop signals when job control is active.
* `SIGUSR1` (10 / arch-specific) & `SIGUSR2` (12 / arch-specific): Application-defined user signals.
* `SIGWINCH` (28): Terminal window size change notification (`TIOCGWINSZ`).

### 8.3 Terminal Line Discipline (`termios`), PTYs, and Job Control
* **Termios Interface:** Terminal devices (`/dev/tty`, `/dev/ttyS*`, `/dev/pts/*`) **MUST** implement POSIX `termios` (`tcgetattr(3)`, `tcsetattr(3)`, `cfmakeraw(3)`, `cfsetispeed(3)`, `cfsetospeed(3)`).
* **Canonical vs. Raw Mode:** Line editing (`ICANON`, `ECHO`, `ECHOE`, `ECHOK`, `ECHONL`, `ISIG`, `IEXTEN`) **MUST** work deterministically in canonical mode and disable cleanly in raw mode (required by `vi`, `less`, `ssh`, and line editors).
* **Pseudo-Terminals (`PTY`):** Master/slave pseudo-terminal allocation (`/dev/ptmx` + `/dev/pts/<n>`) **MUST** function so remote shells (`sshd`), multiplexers, and interactive programs operate identically to physical TTYs.
* **Sessions and Foreground Process Groups:** Sessions (`setsid(2)`) and process groups (`setpgid(2)`, `tcsetpgrp(3)`, `tcgetpgrp(3)`) **MUST** enforce foreground vs. background terminal access so background jobs do not silently steal keyboard input or corrupt terminal state.

---

## 9. ENVIRONMENT, LOCALE, EXIT CODES, AND PERMISSION INVARIANTS

### 9.1 Standard Environment Variables
Every interactive and script execution environment on a **`NIX Certified OS`** **MUST** honor standard UNIX environment variables:
* `PATH`: Colon-separated executable search path, defaulting to `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` (or `/usr/bin:/bin` in minimal/recovery mode).
* `HOME`: Absolute path to the current user's home directory.
* `USER` and `LOGNAME`: Active login username.
* `SHELL`: Path to the user's preferred shell (e.g., `/bin/sh`).
* `TERM`: Terminal type identifier (e.g., `vt100`, `xterm-256color`, `linux`), matched against `/usr/share/terminfo`.
* `IFS`: Internal Field Separator used by `/bin/sh` word splitting (default `<space><tab><newline>`).
* `TMPDIR`: Directory for temporary file creation when set and writable; otherwise `/tmp`.
* `TZ`: Timezone specification (e.g., `UTC`, `America/Sao_Paulo`, or `:/usr/share/zoneinfo/...`).
* `LC_ALL`, `LANG`, `LC_CTYPE`, `LC_COLLATE`, `LC_TIME`, `LC_NUMERIC`, `LC_MESSAGES`: Locale categories; `C` or `POSIX` **MUST** always be supported as deterministic, byte-oriented, locale-independent baselines.

### 9.2 Standard Exit Codes and `sysexits` Conventions
Every `NIX Certified` utility **MUST** return predictable numeric exit statuses:
* `0`: Success (or pattern matched in `grep`; true in `test`/`[`; differences absent in `cmp`).
* `1`: General error, command failure, or no match found (`grep` when no lines match).
* `2`: Misuse of shell builtins/commands or syntax/option error (standard for `grep` on error, `test` syntax error, etc.).
* `126`: Command found but not executable (`EACCES` / non-regular non-executable file).
* `127`: Command not found in `PATH` (`ENOENT`).
* `128 + N`: Process terminated by signal `N` (e.g., `130` for `SIGINT` = `128 + 2`, `143` for `SIGTERM` = `128 + 15`).
* When applicable, diagnostic tools and daemons **MAY** use BSD `<sysexits.h>` codes (`EX_USAGE=64`, `EX_DATAERR=65`, `EX_NOINPUT=66`, `EX_UNAVAILABLE=69`, `EX_SOFTWARE=70`, `EX_OSERR=71`, `EX_CANTCREAT=73`, `EX_IOERR=74`, `EX_TEMPFAIL=75`, `EX_PROTOCOL=76`, `EX_NOPERM=77`, `EX_CONFIG=78`).

### 9.3 File Modes, `umask`, `setuid`/`setgid`, Sticky Bit, and `chroot`
* **Mode Bits:** Standard UNIX permission bits (`rwxrwxrwx` = `0777`) plus `setuid` (`04000`), `setgid` (`02000`), and sticky bit (`01000`) **MUST** be enforced by the VFS layer.
* **`umask(2)`:** Process creation masks (`0022` typical default) **MUST** mask newly created files and directories predictably according to POSIX rules.
* **Privilege Boundaries:** `setuid` / `setgid` binaries **MUST** drop elevated privileges permanently (`setuid(getuid())` / `setgid(getgid())`) as soon as the privileged operation completes, and **MUST** clear or sanitize dangerous environment variables (`LD_*`, `IFS`, `ENV`, `BASH_ENV`, `GCONV_PATH`, `NLSPATH`, `PATH`).
* **`chroot(2)` Isolation:** `chroot(2)` **MUST** restrict path resolution to the specified root directory and require `chdir("/")` / safe working-directory handling so processes cannot escape via relative paths.

---

## 10. NORMATIVE BEHAVIORAL SPECIFICATION FOR ESSENTIAL UTILITIES

To prevent subtle incompatibilities between implementations (such as GNU vs. BusyBox vs. Toybox vs. BSD tools), **`NIX Certified OS`** distributions **MUST** satisfy the behavioral requirements below across core utilities.

### 10.1 Stream Filters and Text Processing (`cat`, `head`, `tail`, `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `tr`, `wc`, `diff`, `patch`, `tee`, `xargs`)
* **`cat`:** Reads each file operand sequentially (or `stdin` when no operands or `-` is given) and writes raw bytes to `stdout` without altering line endings unless `-v`, `-E`, `-T`, or `-A` is specified.
* **`head` & `tail`:** Default to `10` lines when neither `-n` nor `-c` is given. `tail -f` **MUST** keep reading newly appended bytes as files grow; `tail -F` **MUST** detect file replacement/rotation by path and reopen cleanly.
* **`grep`:** **MUST** support multiple `-e PATTERN` expressions and `-f PATTERN_FILE`, print matching lines by default, exit `0` if at least one line matches, `1` if no lines match, and `>1` on error.
* **`sed`:** **MUST** support address ranges, `s/regexp/replacement/flags` (including `g`, `p`, `w`, and numeric `N`), `d`, `p`, `a`, `i`, `c`, `q`, `y`, and `-n` (suppress automatic pattern space printing). In-place edits (`-i`) **SHOULD** write via a temporary file in the target directory and atomically `rename(2)` to avoid partial-write corruption.
* **`awk`:** **MUST** implement POSIX `awk` fields (`$0`, `$1`..`$NF`), `NR`, `FNR`, `NF`, `FS`, `OFS`, `RS`, `ORS`, associative arrays, `BEGIN`/`END` blocks, `getline`, `printf`, and standard built-in string/math functions (`substr`, `index`, `split`, `gsub`, `sub`, `match`, `length`, `tolower`, `toupper`).
* **`cut`:** **MUST** handle `-d <char>` and `-f <list>` (1-based field indices and ranges like `1,3-5`), printing lines without delimiter when `-s` is omitted and skipping them when `-s` is set.
* **`sort` & `uniq`:** `sort` **MUST** support numeric (`-n`), reverse (`-r`), unique (`-u`), case-insensitive (`-f`), field keys (`-k`), and custom delimiters (`-t`), and **MUST** spill to temporary files in `TMPDIR` rather than failing on large inputs. `uniq` compares adjacent lines and supports `-c` (prefix count), `-d` (only duplicates), and `-u` (only non-repeated lines).
* **`tr`:** Operates on `stdin` and `stdout` (no file operands). Supports character ranges (`a-z`), `-d` (delete characters in SET1), `-s` (squeeze repeated characters), and `-c` (complement of SET1).
* **`wc`:** Outputs counts in canonical order: lines (`-l`), words (`-w`), bytes (`-c`), characters (`-m`), followed by filename when file operands are given.
* **`diff` & `patch`:** `diff -u` **MUST** produce standard unified diffs (`---`/`+++` headers and `@@ -a,b +c,d @@` hunks) consumable by `patch -pN`.
* **`tee`:** **MUST** write all `stdin` bytes to `stdout` and every specified file even if one output file encounters an error, reporting errors to `stderr` and exiting non-zero.
* **`xargs`:** **MUST** handle whitespace-quoted tokens and `-0` (`NUL`-delimited streams from `find -print0`), exit `0` when `stdin` is empty (unless a command fails), and return `123` if any invoked command exits `1..125`, `124` if a command exits `255`, `126` if not executable, or `127` if not found.

### 10.2 File, Directory, Archival, and Storage Tools (`ls`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`, `ln`, `chmod`, `chown`, `find`, `tar`, `cpio`, `mount`, `umount`)
* **`ls`:** **MUST** output one entry per line (`-1`) when `stdout` is not a TTY, display hidden files (`.*`) only with `-a` or `-A` (where `-A` omits `.` and `..`), and show permissions, link count, owner, group, size, timestamp, and name/symlink target with `-l`.
* **`cp`, `mv`, `rm`:** **MUST** obey the Rule of Silence on success. `rm` **MUST** refuse `rm -rf /` unless explicitly overridden, and `mv` **MUST** preserve permissions and timestamps when moving across filesystems via copy-and-unlink.
* **`find`:** **MUST** evaluate expressions left-to-right with short-circuiting (`-a` higher precedence than `-o`), support `-print` (default action when no action is given) and `-print0`, and safely execute `-exec cmd {} +` and `-exec cmd {} \;`.
* **`tar` & `cpio`:** `tar` **MUST** create (`-c`), list (`-t`), and extract (`-x`) standard `ustar`/`pax` archives, support external compression filters via `-I` and pipelines (`zstd -d | tar -xf -`), preserve permissions with `-p`, and prevent path-traversal extraction (`../` or leading `/` attacks) by default or warn clearly. `cpio` **MUST** support `-o` and `-i` for `initramfs` and archive pipelines.
* **`mount` & `umount`:** `mount` **MUST** support `/etc/fstab` lookup by mount point or device, `-t <fstype>`, `-o <options>` (including `ro`, `rw`, `noexec`, `nosuid`, `nodev`, `remount`, `bind`), and update mount state cleanly; `umount` **MUST** flush dirty buffers and detach cleanly or fail with a clear diagnostic (`EBUSY`, `EPERM`).

### 10.3 Shells, Editors, Pagers, Manuals, and Build Tools (`sh`, `vi`, `ed`, `less`, `man`, `make`, `cc`, `ld`)
* **`sh`:** **MUST** implement POSIX shell grammar: pipelines (`|`), logical lists (`&&`, `||`), background execution (`&`), subshells `(...)`, brace grouping `{ ...; }`, parameter expansion (`${var}`, `${var:-word}`, `${var:=word}`, `${var:?word}`, `${var:+word}`, `${#var}`, `%`, `%%`, `#`, `##`), command substitution `$(...)` and `` `...` ``, arithmetic expansion `$((...))`, `trap`, `read`, `shift`, `export`, `unset`, `return`, `exit`, `exec`, `eval`, and `local` (where supported).
* **`vi` & `ed`:** `/bin/ed` **MUST** provide full classical UNIX line-editor commands (`a`, `i`, `c`, `d`, `s`, `p`, `n`, `l`, `r`, `w`, `q`, line addresses `.`, `$`, `/re/`, `?re?`). `/bin/vi` **MUST** provide modal command/insert/ex-colon editing over arbitrary terminal sizes using `TERM`/`terminfo` or ANSI escape sequences without requiring an X11/Wayland display server.
* **`less` & `man`:** `less` **MUST** support paging forward/backward, `/` search, and `q` quit without dumping garbage on non-TTY pipes (falling back cleanly to `cat` when `!isatty(STDOUT_FILENO)`). `man` **MUST** render `roff`/`mdoc` manual pages from `/usr/share/man/man[1-8]/`.
* **`make`, `cc`, `ld`:** A `NIX Certified OS` providing the `NIX Certified Dev OS` profile **MUST** provide POSIX `make`, an ISO C99/C11 compiler (`cc`), assembler, and linker (`ld`) capable of building NIX-compliant C programs natively on the target machine.

---

## 11. BOOT SEQUENCE, INITIALIZATION, AND SYSTEM SERVICES

### 11.1 Deterministic Boot Stages
Every **`NIX Certified OS`** **MUST** follow a transparent, inspectable, text-driven boot sequence:
1. **Firmware & Bootloader:** BIOS/UEFI firmware loads the bootloader (e.g., GRUB, Syslinux, or EFI stub) which loads the kernel image (and optional compressed `cpio` `initramfs`) into memory and passes kernel command-line parameters (`console=...`, `root=...`, `ro`, `init=/sbin/init`).
2. **Kernel Initialization:** Kernel initializes CPU architecture, memory management, scheduler, VFS, device drivers, and mounts root filesystem (or runs `/init` inside `initramfs` to mount real root and `switch_root` / `chroot`).
3. **PID 1 (`/sbin/init`):** Starts as PID `1`, mounts `/proc`, `/sys` (on Linux), `/dev`, and `/run` if not already mounted, sets up signal handlers, and invokes the system initialization script (`/etc/rc.init` or runlevel/stage scripts).
4. **System Configuration & Filesystems:** Sets hostname from `/etc/hostname`, applies `sysctl` settings from `/etc/sysctl.conf`, checks root/local filesystems via `fsck` where configured, mounts remaining filesystems in `/etc/fstab` (`mount -a`), activates swap (`swapon -a`), and cleans stale lock/temporary files in `/run` and `/tmp`.
5. **Network & Services:** Configures loopback and administrator-selected interfaces using the platform's documented network configuration mechanism; starts logging and scheduled-task services when included and enabled; and starts other enabled services through the selected platform's documented service-management mechanism. Paths such as `/etc/network`, `/etc/rc.conf`, `/etc/sv/`, and `/etc/rc.d/` are platform examples, not universal requirements.
6. **Getty & Login:** Spawns `getty` instances on configured virtual consoles (`/dev/tty1`..`/dev/tty6`) and serial consoles (`/dev/ttyS0`), presenting `/etc/issue` and invoking `login` -> `/bin/sh` upon authentication.

### 11.2 Clean Shutdown, Reboot, and Single-User Recovery
* `reboot`, `poweroff`, `halt`, and `shutdown` **MUST** signal PID 1 (or invoke `reboot(2)` directly when run by PID 1 / single-user root), terminate active services cleanly (`SIGTERM` followed by `SIGKILL` after grace timeout), unmount filesystems cleanly, and execute `sync(2)` before powering off or rebooting.
* Passing `single`, `S`, `-s`, or `init=/bin/sh` on the kernel command line **MUST** allow an authorized administrator to reach a single-user recovery shell without non-essential daemons.

### 11.3 Logging (`syslog`) and Scheduled Tasks (`cron`)
* **Inspectable Logs:** Essential system and service events MUST be available in a documented, inspectable form. A plain-text export or reader MUST be provided for essential records. A binary journal MAY be used as an additional storage format, but MUST NOT be the sole means of accessing essential records if its format or reader is unavailable to administrators. Log rotation and retention MUST be documented; `/var/log/`, `/etc/cron.daily/`, and `svlogd` are examples, not universal requirements.
* **Cron:** `/usr/sbin/crond` **MUST** parse standard 5-field cron schedules (`minute hour day-of-month month day-of-week`) in `/etc/crontab`, `/etc/cron.d/*`, and `/var/spool/cron/crontabs/<user>`, executing commands with a minimal sanitized environment and logging execution results to `/var/log/cron.log`.

---

## 12. NETWORKING STACK, IPC, AND C LIBRARY / ABI STANDARDS

### 12.1 Networking Stack and Userland Tools
* A `NIX Certified OS` **MUST** support standard TCP/IP networking (`IPv4` and `IPv6`, loopback `127.0.0.1` / `::1`, Ethernet/Wi-Fi/tunnel interfaces) configured via plain text and standard tools (`ip`/`ss` on Linux or `ifconfig`/`route`/`netstat` on BSD, plus `ping`, `nc`, `curl`/`wget`, `ssh`/`sshd`/`scp`).
* Name resolution **MUST** respect `/etc/nsswitch.conf`, `/etc/hosts`, and `/etc/resolv.conf` predictably, with zero hardcoded external telemetry hosts or forced DNS interception.
* Firewalling (`nft` or `iptables`/`pf`) **SHOULD** be available so administrators can enforce default-deny or custom packet filter policies.

### 12.2 Inter-Process Communication (IPC)
* Programs **MUST** prefer composable UNIX IPC mechanisms:
  * Pipes (`|`) and named FIFOs (`mkfifo(3)`) for streaming data.
  * UNIX Domain Sockets (`AF_UNIX`, `SOCK_STREAM` / `SOCK_DGRAM`) under `/run/` with explicit file mode permissions (`0600` or `0660`) for local client-server communication.
  * POSIX signals (`SIGTERM`, `SIGHUP`, `SIGUSR1`, `SIGUSR2`) for asynchronous control.
  * POSIX shared memory (`shm_open(3)`) and semaphores (`sem_open(3)`) only when high-throughput shared buffers are justified.

### 12.3 Standard C Library (`libc`) and System Headers
* The system `libc` **MUST** provide standard POSIX.1-2017 and ISO C99/C11 headers under `/usr/include/` (`stdio.h`, `stdlib.h`, `string.h`, `unistd.h`, `fcntl.h`, `errno.h`, `signal.h`, `time.h`, `dirent.h`, `grp.h`, `pwd.h`, `termios.h`, `sys/stat.h`, `sys/types.h`, `sys/wait.h`, `sys/socket.h`, `sys/un.h`, `netinet/in.h`, `arpa/inet.h`).
* Dynamic linking **MUST** use a clean, standard dynamic loader (`ld-musl-*.so.1` or system `ld.so`) with full RELRO (`-Wl,-z,relro -Wl,-z,now`) and stack protection (`-fstack-protector-strong`) on security-sensitive binaries.

---

## 13. SOFTWARE DEVELOPMENT, BUILD, AND PACKAGING STANDARDS

### 13.1 Source Repository Layout Standard

Every standalone software repository seeking **`NIX Certified`** status **SHOULD** conform to the following canonical layout:

```text
<repository-root>/
├── LICENSE                     # Full verbatim OSI-approved open-source license text
├── README.md                   # Concise technical overview, build instructions, and usage
├── Makefile                    # POSIX-compliant top-level build, test, install, and clean targets
├── config.mk                   # User-editable compiler, CFLAGS, LDFLAGS, and PREFIX definitions
├── src/                        # Primary C/POSIX shell source files
├── include/                    # Public and internal C header files (.h)
├── man/                        # Troff/mdoc manual pages (*.1, *.3, *.5, *.8)
└── tests/                      # Automated POSIX shell/C unit and integration test harness
```

### 13.2 Compilation and Code Hygiene Requirements

1. **Language Standard:** Core systems software **MUST** be written in ISO C99 or C11 (`-std=c99` or `-std=c11`) or strict POSIX.1-2017 Shell (`#!/bin/sh`).

2. **Mandatory Compiler Flags:** C builds **SHOULD** compile cleanly without warnings using strict diagnostic and hardening flags supported by the target compiler and platform (see Section 18.2.9), such as:

   ```makefile
   CFLAGS = -std=c11 -O2 -Wall -Wextra -Wpedantic -Werror \
            -Wshadow -Wpointer-arith -Wcast-qual -Wstrict-prototypes \
            -Wmissing-prototypes -Wformat=2 -Wformat-security \
            -D_POSIX_C_SOURCE=200809L -fstack-protector-strong -fPIE
   LDFLAGS = -Wl,-z,relro -Wl,-z,now -pie
   ```

3. **Standard Makefile Targets:** Projects SHOULD provide documented build, test, install, and clean targets. POSIX make does not require every project to implement a target named `uninstall`, `distclean`, or `check`; those are NIX project-policy recommendations, not portable make requirements.

4. **Manual Pages:** Every user command, file format, library function, and system administration utility **SHOULD** include a clean `man` page (`man1`, `man3`, `man5`, `man8`) installed under `/usr/share/man/`.

### 13.3 Package Management Standards

1. **Plain-Text Manifests:** Any package manager shipped in a `NIX Certified OS` **MUST** store its local package database in human-readable plain text under `/var/lib/pkg/` with SHA-256 file manifests.

2. **Zero Implicit Network Calls:** Package managers **NEVER** refresh indexes, check for updates, or contact remote mirrors in the background unless explicitly invoked by the operator.

3. **Cryptographic Integrity:** Remote package repositories **SHOULD** sign index metadata and package archives using open-source cryptographic keys (Ed25519 or OpenPGP) and verify SHA-256 digests before extracting files.

---

## 14. THE `NIX CERTIFIED` SEAL, CERTIFICATION VERIFICATION, AND GOVERNANCE

Any operating system distribution, kernel, or software project—developed by any organization, team, or independent author—may apply for **`NIX-Certified`** status via the public certification repository (`https://github.com/The-NIX-Foundation/NIX-Certified`).

### 14.1 Certification Tiers

1. **`NIX Certified Utility`:** Awarded to individual CLI tools, daemons, shells, and editors that conform to Sections 2, 3, 4, 8, 9, 10, 12, 13, 14, 15, 18, 20, 21, and 23.

2. **`NIX Certified Library`:** Awarded to standalone C libraries and runtime components that conform to Sections 2, 3, 4, 8, 9, 12, 13, 14, 15, 18, 21, and 23 for their declared ABI.

3. **`NIX Certified Kernel`:** Awarded to standalone operating system kernels that conform to Sections 2, 3, 4, 7.1, 8, 12, 14, 15, 16, 18, 21, and 23 for their declared configuration and architecture.

4. **`NIX Certified OS`:** Awarded to complete operating system distributions that conform to Sections 2 through 24 in full for their selected profile (such as `NIX Certified Core OS` or `NIX Certified Dev OS`), including the applicable NIX-FHS directory hierarchy (Section 5), the Essential UNIX Core Utilities suite (Section 6), and the Kernel/Init/Process/Terminal/Operational subsystems (Sections 7 through 13).

### 14.2 Certification Verification Procedures

To earn or renew the **`NIX Certified`** seal, a candidate release MUST receive a certification outcome of `Pass` or `Pass with Published Exceptions` under Section 22. Warnings MUST be resolved or recorded with their impact; an exception is valid only when approved and published under Section 22. These verification procedures provide evidence and do not replace required human review:

1. **License & Source Freedom Gate:**

   * Verify that licence files and SPDX identifiers are present where applicable, then evaluate the exact licence terms and dependency obligations under Section 3. An OSI or FSF approval claim MUST be checked against that organisation's own current list.

   * Verify complete source code availability and zero proprietary CPU binaries/modules or `ALL RIGHTS RESERVED` restrictions (while honoring the passive redistributable `/lib/firmware` policy in Section 3.2).

2. **Zero-Telemetry & Network Behaviour Gate:**

   * Perform initial screening via symbol scans (`readelf -s`, `objdump -T`) and static string/source reviews to identify network symbols (`getaddrinfo`, `res_query`, `curl_easy_init`, `SSL_connect`), hardcoded endpoints, or analytics hooks, combined with runtime syscall tracing (`strace`) and packet observation per Section 4.1.4 and Section 24.9.

   * Verify that any outbound network activity or `AF_INET`/`AF_INET6` socket usage is strictly attributable to declared network functions or explicit administrator/user invocation, and that local-only daemons restrict `socket()` usage to `AF_UNIX`/`AF_LOCAL`.

3. **Zero-Bloatware & Dependency Gate:**

   * Verify `ldd` / `readelf -d` dynamic dependency trees of all `/bin` and `/sbin` binaries contain strictly `libc.so` (and `libm.so` / `libz.so` / `libcrypto.so` where explicitly justified). Utilities explicitly designated as network tools in Section 6.6 (`curl`, `wget`, `ssh`, `sshd`, `scp`) **MAY** link against minimalist, reviewable TLS libraries (such as `libtls`, `LibreSSL`, or `BearSSL`).

   * Measure binary size, installed image size, and runtime memory for any footprint profile claimed by the candidate. These measurements MUST use the baseline, architecture, build mode, and test conditions defined by that profile. A general NIX certification MUST NOT fail solely because it exceeds a footprint target that is not part of its selected profile.

4. **FHS, UNIX Semantics & Essential Core Utilities Gate (For `NIX Certified OS`):**

   * Verify existence, permissions (`0755`, `1777`, `0700`, `0600`), and ownership of every applicable directory and device node in Section 5.

   * Verify POSIX signal, terminal (`termios`/PTY), process group, permission/umask/setuid, and boot/shutdown behavior required by Sections 7, 8, 9, 10, and 11.

   * Execute POSIX conformance test vectors against every essential utility listed in Section 6 (`ls`, `cp`, `mv`, `rm`, `cat`, `grep`, `sed`, `awk`, `find`, `tar`, `ps`, `kill`, `mount`, `umount`, `ip`/`ifconfig`, `sh`, `vi`, `man`, etc.).

5. **Reproducibility & Determinism Gate:**

   * Perform two independent builds in distinct paths with `SOURCE_DATE_EPOCH=1700000000` and verify identical SHA-256 digests across output artifacts.

### 14.3 Conformance Verification Matrix

| Verification Domain | Verification Method | Pass Criterion | Applies To |
| :--- | :--- | :--- | :--- |
| Open Source License | SPDX scan & `LICENSE` check | Approved OSI/FSF license; zero proprietary CPU code/modules | All Candidates |
| Zero Telemetry | Source review, symbol screening (`readelf`/`objdump`), runtime tracing & packet capture | Zero undisclosed collection/transmission of telemetry; all network activity attributable per Section 4.1.4 | All Candidates |
| Local `AF_UNIX` Sockets | `strace` / AST/CFG inspection | `socket()` domain restricted to `AF_UNIX`/`AF_LOCAL` in local binaries | Local Utilities & Daemons |
| Minimalist TLS Exception | `ldd` / `readelf -d` allowlist | Only approved Section 6.6 network tools link `libtls`/`LibreSSL`/`BearSSL` | Network Utilities |
| Resource Footprint | Reproducible binary/image/RAM measurement | Report measured values against the limits of the selected footprint profile; no unrequested universal ceiling | Candidates claiming a footprint profile |
| NIX-FHS & `/dev` Nodes | Filesystem & inode verification script | Required directories, permissions (`0755`, `1777`, `0700`, `0600`), and `/dev` nodes present | `NIX Certified OS` |
| Signals, TTY & Job Control | Automated POSIX C/PTY test suite | `sigaction`, `termios`, PTYs, process groups, and `SIGPIPE`/`SIGTERM`/`SIGINT` conform | `NIX Certified OS` |
| Essential Core Utilities | Flag & pipeline test suite | All Section 6 tools pass pipe (`stdin`/`stdout`), exit-code, and flag checks | `NIX Certified OS` |
| Reproducible Build | Dual build with `SOURCE_DATE_EPOCH` | Identical SHA-256 checksums across independent builds | All Candidates |

### 14.4 Issuance, Public Registry, and Revocation

* **Public Certification Ledger:** Upon passing certification review and peer review, the candidate release hash, version, and compliance report are published in the public `NIX Certified` registry (`https://github.com/The-NIX-Foundation/NIX-Certified`), authorizing the project to display the official **`NIX Certified`** badge and conformance statement.

* **Per-Version Validity:** Certification applies strictly to reviewed release versions (`MAJOR.MINOR.PATCH`). Unreviewed versions or downstream modifications **MUST NOT** claim `NIX Certified` status until re-verified.

* **Immediate Revocation:** If any certified release is subsequently discovered to contain telemetry, proprietary CPU binaries/modules, or violations of `NIX-STAN-001`, The NIX Foundation **SHALL** revoke its certification in the public ledger and require immediate removal of the `NIX Certified` mark.

---

## 15. NORMATIVE CONFORMANCE STATEMENT

**Important:** The declaration below is valid only for an active NIX Foundation certification recorded in the public registry. It MUST NOT be interpreted as a UNIX trademark or SUS certification claim.

A vendor, maintainer, or distribution MAY include the following conformance declaration in its official documentation. It MUST NOT place the full declaration in `/etc/os-release` unless the operating system's metadata specification permits the field and format used:

> *"This product conforms to The NIX Standard (`NIX-STAN-001`, version 1.0), profile [PROFILE], certification tier [TIER], for release [RELEASE]. Its current certification status and conformance scope are recorded by The NIX Foundation at https://github.com/The-NIX-Foundation/NIX-Certified. This statement does not represent UNIX trademark authorization or The Open Group certification."*


---

## 16. PORTABILITY PROFILES AND PLATFORM BOUNDARIES

### 16.1 Common Base Profile

The Common Base Profile defines the interfaces expected from every NIX OS candidate, regardless of kernel family.

16.1.1 The implementation MUST document its process model, executable format, system-call ABI, C library, and supported application binary interfaces.

16.1.2 The implementation MUST provide a documented method to compile and execute a minimal C program using the platform's supported C environment.

16.1.3 The implementation MUST document the supported shell and the exact shell language baseline.

16.1.4 The implementation MUST identify which utilities are required by the selected POSIX profile and which are optional extensions.

16.1.5 The implementation MUST provide stable documentation for user accounts, groups, permissions, process control, signals, terminals, and filesystems.

16.1.6 The implementation MUST document the boot procedure and the supported recovery procedure.

16.1.7 The implementation MUST identify all non-portable interfaces exposed to applications.

16.1.8 The implementation SHOULD make portable interfaces the default for examples and sample programs.

16.1.9 The implementation MUST NOT require Linux-specific `/proc` or `/sys` paths for applications that claim Common Base portability.

16.1.10 The implementation MUST NOT require BSD-specific `sysctl` MIBs for applications that claim Common Base portability.

### 16.2 Linux Platform Profile

16.2.1 A Linux candidate MUST identify the kernel version and configuration used for the tested release.

16.2.2 Linux-specific interfaces MUST be labelled as such in documentation.

16.2.3 The candidate MUST document the role of `/proc`, `/sys`, `/dev`, and the selected device manager.

16.2.4 The candidate MUST document its module-loading policy and the source and licence of out-of-tree modules, if any.

16.2.5 The candidate MUST document whether it uses cgroups, namespaces, seccomp, capabilities, or other Linux-specific facilities.

16.2.6 Linux-specific requirements MUST NOT be imposed on the BSD profile.

16.2.7 A Linux implementation MAY use any init or service-management system that satisfies Sections 7.2, 11, and 20.

16.2.8 No init or service-management implementation MUST be automatically accepted or disqualified by name; the certification review MUST evaluate documented behaviour, dependencies, policy, security, and administrator control.

16.2.9 The candidate MUST describe its kernel upgrade and rollback process.

16.2.10 The candidate SHOULD provide kernel configuration provenance and reproducible build instructions.

### 16.3 BSD Platform Profile

16.3.1 A BSD candidate MUST identify the BSD project, release branch, kernel version, and userland version.

16.3.2 A BSD candidate MUST document its native boot loader and init/service framework.

16.3.3 A BSD candidate MUST document its native device-node management and device enumeration model.

16.3.4 A BSD candidate MUST document its native network configuration and firewall facilities.

16.3.5 A BSD candidate MUST NOT be required to provide Linux kernel modules or Linux `/sys` semantics.

16.3.6 A BSD candidate MUST NOT be required to provide `systemctl`, `modprobe`, `udev`, or Linux-specific syscalls unless it explicitly claims a compatibility layer implementing them.

16.3.7 BSD extensions MAY be exposed when clearly documented and when portable application interfaces remain available.

16.3.8 The candidate MUST document compatibility constraints for software written specifically for Linux.

16.3.9 The candidate SHOULD preserve native administration tools rather than imitate another operating system without a compatibility need.

16.3.10 The candidate MUST test its native package and ports/build framework as part of the OS certification review.

### 16.4 Historical UNIX and Research Systems

16.4.1 Historical systems MUST be assessed against a profile appropriate to their era and documented interfaces.

16.4.2 A historical system MUST NOT be judged against facilities that did not exist in its declared target environment unless it claims a modern compatibility profile.

16.4.3 Research systems MAY define an experimental profile with clearly marked non-standard interfaces.

16.4.4 Experimental status MUST be visible in the certification record.

16.4.5 A research profile MUST NOT be advertised as full POSIX or SUS conformance without evidence against the applicable test suite.

---

## 17. POSIX AND SUS CONFORMANCE METHODOLOGY

### 17.1 Edition Selection

17.1.1 Each profile MUST name the exact POSIX edition and technical corrigenda used as its baseline.

17.1.2 Each profile MUST list options and optional facilities that are required, supported, or unsupported.

17.1.3 The profile MUST distinguish mandatory interfaces from optional interfaces.

17.1.4 The profile MUST identify any NIX extension that goes beyond the selected POSIX baseline.

17.1.5 A product MUST NOT claim complete POSIX conformance solely because it implements a familiar subset of shell commands.

17.1.6 A product MUST NOT claim SUS certification based solely on NIX conformance.

17.1.7 The certification report MUST state the test-suite name, version, invocation, and result.

### 17.2 Application Interface Tests

17.2.1 Tests MUST cover required headers and declarations.

17.2.2 Tests MUST cover required library functions and their specified error conditions.

17.2.3 Tests MUST cover file descriptor semantics and inheritance.

17.2.4 Tests MUST cover process creation, waiting, and termination.

17.2.5 Tests MUST cover signal delivery and signal masks.

17.2.6 Tests MUST cover pipes, FIFOs, and required IPC interfaces.

17.2.7 Tests MUST cover terminal control where terminals are supported.

17.2.8 Tests MUST cover filesystem metadata and permission semantics.

17.2.9 Tests MUST cover locale-sensitive behaviour and the C/POSIX locale.

17.2.10 Tests MUST cover error returns and `errno` values where specified by the selected standard.

### 17.3 Shell and Utility Tests

17.3.1 Shell tests MUST be derived from the selected POSIX shell language requirements.

17.3.2 Tests MUST distinguish syntax errors, expansion errors, command lookup failures, and execution failures.

17.3.3 Utility tests MUST verify standard input, standard output, and standard error behaviour where specified.

17.3.4 Utility tests MUST verify option parsing, operand handling, and `--` handling only where the governing specification defines them.

17.3.5 Tests MUST verify exit status for success, no-match, invalid-input, and operational-error cases.

17.3.6 Tests MUST include filenames containing spaces, tabs, newlines, leading hyphens, and non-ASCII characters where supported by the locale and filesystem.

17.3.7 Tests MUST include empty input and large input cases.

17.3.8 Tests SHOULD include interrupted system calls and full or failing output streams.

17.3.9 Tests MUST distinguish standard requirements from GNU, BSD, and NIX-specific extensions.

17.3.10 Test results MUST be retained as machine-readable artifacts and human-readable summaries.

### 17.4 Test Environment Integrity

17.4.1 Test runs MUST record the hardware or virtual-machine environment.

17.4.2 Test runs MUST record kernel, libc, compiler, and tool versions.

17.4.3 Tests MUST be repeatable from a documented clean state.

17.4.4 Tests that require network access MUST identify endpoints and explain why network access is required.

17.4.5 Offline tests SHOULD be the default for core conformance.

17.4.6 Tests MUST NOT silently modify the host system outside a declared disposable test environment.

17.4.7 Destructive tests MUST run only in an isolated virtual machine, container with appropriate safeguards, or disposable filesystem image.

17.4.8 The test harness MUST identify skipped tests and the reason for each skip.

17.4.9 A skipped mandatory test MUST prevent a full pass unless the certification authority grants a documented exception.

17.4.10 The certification review MUST preserve logs sufficient for independent review.

---

## 18. SECURITY, PRIVILEGE, AND TRUST BOUNDARIES

### 18.1 Secure Defaults

18.1.1 Security defaults MUST be documented and inspectable.

18.1.2 Network services MUST be disabled by default unless the installation role explicitly requires them.

18.1.3 Remote administrative access MUST require explicit configuration.

18.1.4 Default credentials MUST NOT be shipped for general-purpose installations.

18.1.5 Administrative interfaces MUST enforce authentication and authorization appropriate to their threat model.

18.1.6 Privileged components MUST minimize the code executed with elevated authority.

18.1.7 Privileged helpers MUST validate all untrusted input.

18.1.8 Privileged helpers MUST avoid invoking commands through an uncontrolled search path.

18.1.9 Sensitive temporary files MUST be created atomically with restrictive permissions.

18.1.10 Security-sensitive failures MUST fail closed when continuing would violate the documented security boundary.

### 18.2 Memory and Input Safety

18.2.1 C and C++ components MUST validate lengths before copying or indexing buffers.

18.2.2 Integer arithmetic used for allocation sizes MUST be checked for overflow.

18.2.3 Parsers MUST impose documented limits on nesting, record size, and total input where unbounded input could exhaust resources.

18.2.4 Parsers MUST reject malformed encodings according to their declared format rules.

18.2.5 Network-facing parsers MUST treat all remote data as untrusted.

18.2.6 Error paths MUST release resources and preserve system integrity.

18.2.7 Security tests SHOULD include fuzzing for parsers and protocol handlers.

18.2.8 Sanitizers SHOULD be used in development builds where supported.

18.2.9 Compiler hardening flags SHOULD be selected according to compiler and platform support.

18.2.10 A candidate MUST document known limitations of its hardening configuration.

### 18.3 Cryptography and Key Management

18.3.1 Cryptographic algorithms MUST be selected from reputable, currently maintained standards.

18.3.2 A product MUST NOT invent its own cryptographic primitive or protocol for production security.

18.3.3 Private keys MUST be protected by appropriate filesystem permissions and access controls.

18.3.4 Key generation MUST use a cryptographically secure random source.

18.3.5 Certificate validation MUST verify the intended identity and trust chain.

18.3.6 Software update signatures MUST cover the metadata necessary to prevent substitution and rollback attacks.

18.3.7 Key rotation and revocation procedures MUST be documented.

18.3.8 Test keys MUST NOT be accepted as production trust anchors.

18.3.9 Cryptographic failures MUST be reported without exposing secret key material.

18.3.10 The system MUST document its trust roots and how administrators can change them.

---

## 19. FILESYSTEM, STORAGE, AND DATA INTEGRITY

### 19.1 Filesystem Semantics

19.1.1 The OS profile MUST identify supported filesystem types and their intended use.

19.1.2 The OS MUST document case sensitivity and filename encoding behaviour.

19.1.3 The OS MUST document symbolic link and hard link semantics.

19.1.4 The OS MUST document mount options and their security implications.

19.1.5 The OS MUST document filesystem permission and ownership semantics.

19.1.6 Filesystem tools MUST report partial failures accurately.

19.1.7 Filesystem repair tools MUST document whether they are safe on mounted filesystems.

19.1.8 Backup and restore procedures MUST be documented.

19.1.9 A certified OS MUST NOT promise crash consistency beyond what its filesystem and storage stack provide.

19.1.10 Durability guarantees MUST distinguish successful writes from data known to have reached stable storage.

### 19.2 Atomic Updates

19.2.1 Configuration updates SHOULD use write-to-temporary-file followed by atomic rename where supported.

19.2.2 Applications MUST document when updates can be observed partially.

19.2.3 Package managers MUST avoid leaving the package database in an ambiguous state after interruption.

19.2.4 Package managers SHOULD use transactions or recoverable state transitions.

19.2.5 Package managers MUST record package identity, version, architecture, origin, and integrity metadata.

19.2.6 Package managers MUST verify package integrity before installation.

19.2.7 Package managers MUST authenticate repository metadata according to their threat model.

19.2.8 Package managers MUST provide a way to inspect planned changes before applying them.

19.2.9 Automatic update behaviour MUST be explicitly configurable by the administrator.

19.2.10 No package manager may claim reproducibility merely because package archives have checksums.

### 19.3 Backup and Recovery

19.3.1 The system documentation MUST identify critical configuration and state locations.

19.3.2 The system MUST document recovery from a failed package transaction.

19.3.3 The system SHOULD provide a documented method to verify backup integrity.

19.3.4 Restore procedures MUST identify ownership, permissions, extended attributes, and special files that are preserved or omitted.

19.3.5 Recovery media SHOULD permit inspection and repair without booting the installed system.

19.3.6 Recovery procedures MUST avoid overwriting user data without clear operator confirmation.

19.3.7 The certification report MUST record recovery tests performed.

---

## 20. NETWORKING, SERVICES, AND PRIVACY

### 20.1 Network Behaviour

20.1.1 Every network-capable component MUST document the network functions it provides.

20.1.2 Outbound connections MUST be attributable to a documented service or an explicit user/admin action.

20.1.3 Background network activity MUST be documented and configurable.

20.1.4 The system MUST NOT transmit telemetry contrary to the NIX privacy policy.

20.1.5 DNS configuration and resolver behaviour MUST be documented.

20.1.6 Proxy configuration behaviour MUST be documented where supported.

20.1.7 TLS certificate verification MUST NOT be disabled by default.

20.1.8 Network tools MUST report connection and authentication failures accurately.

20.1.9 Services MUST bind only to the addresses and interfaces required by their configured role.

20.1.10 Firewall defaults MUST be documented; the certification MUST NOT assume one firewall implementation across all kernels.

### 20.2 Privacy Controls

20.2.1 The system MUST disclose every category of data collected by default.

20.2.2 The system MUST disclose the purpose, destination, retention, and control mechanism for collected data.

20.2.3 Optional diagnostics MUST be opt-in unless a profile explicitly permits another model and provides a clear disclosure.

20.2.4 Users MUST be able to inspect and disable optional data collection.

20.2.5 Disabling telemetry MUST NOT disable unrelated essential functionality.

20.2.6 Device identifiers MUST NOT be transmitted for tracking without explicit consent.

20.2.7 Crash reports MUST be inspectable before transmission where technically practical.

20.2.8 Privacy documentation MUST be versioned with the product release.

20.2.9 The certification review MUST test the default installation, not only a privacy-hardened configuration.

20.2.10 Privacy claims MUST be supported by evidence and bounded by the certification scope.

### 20.3 Service Supervision

20.3.1 Service definitions MUST be inspectable by administrators.

20.3.2 Service startup order MUST be documented where ordering affects correctness.

20.3.3 Services MUST have a documented stop and restart procedure.

20.3.4 Supervision MUST make failed services observable.

20.3.5 Restart policies MUST avoid uncontrolled restart loops.

20.3.6 Service logs MUST have documented location, format, and rotation behaviour.

20.3.7 Services MUST run with the least privilege practical for their function.

20.3.8 Service managers MAY differ between platform profiles.

20.3.9 The standard MUST NOT prescribe a single init implementation as a universal UNIX requirement.

20.3.10 Administrators MUST be able to determine which services are enabled and why.

---

## 21. DOCUMENTATION, RELEASE ENGINEERING, AND COMPATIBILITY

### 21.1 Documentation Requirements

21.1.1 Every certified release MUST publish installation and upgrade instructions.

21.1.2 Every certified release MUST publish supported hardware and architecture information.

21.1.3 Every certified release MUST publish known limitations.

21.1.4 Every user-facing command MUST have usage documentation.

21.1.5 Configuration files MUST document syntax, defaults, and precedence.

21.1.6 Administrative procedures MUST identify required privileges.

21.1.7 Security-sensitive configuration MUST explain its consequences.

21.1.8 Documentation MUST distinguish normative requirements from recommendations.

21.1.9 Documentation MUST identify platform-specific behaviour.

21.1.10 Documentation SHOULD include examples that are safe to run in a disposable environment.

### 21.2 Versioning and Compatibility

21.2.1 The project MUST publish a versioning policy.

21.2.2 Public interfaces MUST have documented compatibility expectations.

21.2.3 Breaking changes MUST be documented before or with the release.

21.2.4 Configuration format changes MUST include migration guidance.

21.2.5 The project SHOULD provide a deprecation period for widely used interfaces.

21.2.6 Security fixes MAY require compatibility exceptions where necessary to protect users.

21.2.7 The certification record MUST be tied to an immutable release identifier or cryptographic digest.

21.2.8 Rebuilt binaries MUST not inherit a prior certification automatically unless their identity and build provenance are verified.

21.2.9 Downstream patches MUST be disclosed in the certification application.

21.2.10 Certification MUST be reviewed when changes materially affect a certified interface.

### 21.3 Reproducible and Verifiable Builds

21.3.1 Build instructions MUST identify required tools and versions.

21.3.2 Build scripts MUST NOT fetch undeclared source inputs during an offline build.

21.3.3 Build inputs SHOULD be pinned and checksummed.

21.3.4 Build outputs SHOULD be reproducible across independent build environments.

21.3.5 A reproducibility claim MUST state the scope of compared artifacts.

21.3.6 Timestamps, locale, filesystem ordering, and build paths SHOULD be normalized where feasible.

21.3.7 Build provenance SHOULD identify source revision, compiler, linker, and build flags.

21.3.8 Signed release artifacts MUST have verifiable signatures.

21.3.9 Signing keys MUST be protected and their rotation policy documented.

21.3.10 Reproducibility failures MUST be documented rather than concealed.

---

## 22. CERTIFICATION PROCESS, EXCEPTIONS, AND MAINTENANCE

### 22.1 Application Package

22.1.1 An application MUST identify the product and requested certification tier.

22.1.2 An application MUST identify the target profile and supported architectures.

22.1.3 An application MUST provide source repositories and immutable release references.

22.1.4 An application MUST provide build and test instructions.

22.1.5 An application MUST provide a software bill of materials or equivalent dependency inventory.

22.1.6 An application MUST disclose proprietary components and firmware.

22.1.7 An application MUST disclose network endpoints used by default services.

22.1.8 An application MUST provide security and privacy documentation.

22.1.9 An application MUST provide known deviations and requested exceptions.

22.1.10 An application MUST identify a responsible maintainer contact.

### 22.2 Certification Outcomes

22.2.1 A result MUST be one of: Pass, Pass with Published Exceptions, Fail, or Incomplete.

22.2.2 A Pass MUST mean all mandatory tests for the requested profile passed.

22.2.3 A Pass with Published Exceptions MUST list every exception, rationale, affected component, and expiration or review date.

22.2.4 A Fail MUST list the failed requirement and supporting evidence.

22.2.5 An Incomplete result MUST identify missing evidence or unexecuted tests.

22.2.6 An incomplete certification review MUST NOT be represented as certification.

22.2.7 Certification evidence MUST be retained according to a published retention policy.

22.2.8 The applicant MUST have an opportunity to respond to factual errors.

22.2.9 Reviewers MUST disclose conflicts of interest.

22.2.10 The Foundation SHOULD publish an appeal process.

### 22.3 Exceptions and Waivers

22.3.1 Exceptions MUST be narrow and specific.

22.3.2 An exception MUST NOT silently redefine a mandatory requirement.

22.3.3 Each exception MUST include a risk assessment.

22.3.4 Each exception MUST identify compensating controls where applicable.

22.3.5 Exceptions MUST have a review date.

22.3.6 Exceptions MUST be visible in the public registry.

22.3.7 An exception MUST NOT permit a false claim of POSIX, SUS, or UNIX certification.

22.3.8 An exception MAY be withdrawn when the underlying issue is corrected.

22.3.9 Emergency security exceptions MUST be reviewed after the emergency.

22.3.10 The certification authority MUST record who approved an exception and why.

### 22.4 Registry and Use of the Mark

22.4.1 The public registry MUST identify the certified product, release, profile, tier, and status.

22.4.2 The registry MUST link to the certification summary.

22.4.3 The registry MUST identify exceptions and expiration dates.

22.4.4 The NIX Certified mark MUST be used only for the product and release recorded.

22.4.5 Marketing MUST NOT imply endorsement beyond the certified scope.

22.4.6 A product MUST NOT use the NIX mark in a manner likely to confuse users about UNIX trademark ownership or The Open Group affiliation.

22.4.7 Suspended and revoked entries MUST remain identifiable in the registry.

22.4.8 The Foundation MUST publish the process for correcting registry errors.

22.4.9 Certification status MUST be checked against the registry before making a current claim.

22.4.10 The Foundation SHOULD publish machine-readable registry data.

---

## 23. CONFORMANCE CLAIMS AND REQUIRED DISCLOSURES

### 23.1 Permitted Claims

23.1.1 A product that conforms only to a utility profile MAY claim “NIX Certified Utility” for the named release.

23.1.2 A product that conforms to a library profile MAY claim “NIX Certified Library” for the named ABI and release.

23.1.3 A product that conforms to a kernel profile MAY claim “NIX Certified Kernel” for the named configuration and architecture.

23.1.4 A complete OS MAY claim “NIX Certified OS” only after conforming to the selected OS profile.

23.1.5 Every claim MUST include the NIX Standard identifier and version.

23.1.6 Every claim MUST link to the public registry entry.

23.1.7 Every claim MUST identify the certified release.

23.1.8 A product MUST NOT claim “UNIX Certified” on the basis of NIX certification.

23.1.9 A product MUST NOT use The Open Group's certification marks without authorization.

23.1.10 A product MUST NOT imply that NIX Foundation is The Open Group or an affiliate.

### 23.2 Required Public Summary

23.2.1 The public summary MUST state the certification tier.

23.2.2 The public summary MUST state the profile and version.

23.2.3 The public summary MUST state the architectures tested.

23.2.4 The public summary MUST state the POSIX/SUS claims, if any, separately.

23.2.5 The public summary MUST list material exceptions.

23.2.6 The public summary MUST identify the certification date.

23.2.7 The public summary MUST identify the release digest or immutable reference.

23.2.8 The public summary MUST link to security advisories relevant to the release.

23.2.9 The public summary SHOULD include test coverage information.

23.2.10 The public summary MUST avoid unsupported absolute claims such as “100% UNIX”.

---

## 24. FINAL TECHNICAL PRINCIPLES

24.1 NIX conformance is demonstrated by evidence, not branding.

24.2 Portability is defined against named interfaces and editions, not resemblance to a historical system.

24.3 UNIX philosophy is an engineering influence and design discipline; it is not itself a formal certification test suite.

24.4 POSIX conformance requires testing the specified interfaces and behaviours, not merely providing familiar command names.

24.5 SUS certification and UNIX trademark rights remain separate from NIX Foundation certification.

24.6 Linux-specific and BSD-specific facilities MUST be documented as such.

24.7 No single filesystem tree, init system, C library, shell, or kernel architecture is universally required by all UNIX-derived systems.

24.8 Minimalism MUST be evaluated against documented purpose, maintainability, security, and measured resource use; arbitrary byte ceilings MUST NOT substitute for engineering evidence.

24.9 Zero telemetry is a NIX privacy policy choice and MUST be verified through multiple forms of evidence; binary symbol scans alone are insufficient.

24.10 Open-source licensing MUST be evaluated by the exact licence terms and dependency obligations.

24.11 Security controls MUST be proportionate to the threat model and must not be reduced to a fixed compiler-flag checklist.

24.12 Certification MUST be reproducible, reviewable, versioned, and limited to the tested product scope.

24.13 The Foundation MUST correct this standard when test evidence demonstrates ambiguity, contradiction, or an obsolete requirement.

24.14 Implementers SHOULD contribute test cases and proposed clarifications publicly.

24.15 This standard MUST be read together with the current NIX Certified programme rules and the profile selected by the applicant.

---

## APPENDIX A. IMPLEMENTER'S RELEASE CHECKLIST

A.1 Confirm the requested certification tier.

A.2 Confirm the exact NIX Standard version.

A.3 Confirm the target platform profile.

A.4 Confirm the target architecture and ABI.

A.5 Confirm the selected POSIX baseline.

A.6 Confirm the selected SUS claims, if any.

A.7 Confirm the source revision is immutable.

A.8 Confirm all dependencies are inventoried.

A.9 Confirm all licences are identified.

A.10 Confirm notices and attributions are preserved.

A.11 Confirm proprietary components are disclosed.

A.12 Confirm firmware policy is documented.

A.13 Confirm build dependencies are documented.

A.14 Confirm build instructions work from a clean checkout.

A.15 Confirm offline build behaviour is documented.

A.16 Confirm test commands are documented.

A.17 Confirm skipped tests are explained.

A.18 Confirm test logs are retained.

A.19 Confirm known failures are disclosed.

A.20 Confirm installation instructions are current.

A.21 Confirm upgrade instructions are current.

A.22 Confirm recovery instructions are tested.

A.23 Confirm default services are inventoried.

A.24 Confirm outbound network activity is inventoried.

A.25 Confirm privacy documentation matches observed behaviour.

A.26 Confirm service privilege boundaries are reviewed.

A.27 Confirm filesystem layout is profile-compliant.

A.28 Confirm platform-specific paths are identified.

A.29 Confirm shell and utility baselines are identified.

A.30 Confirm exit status behaviour is tested.

A.31 Confirm process and signal behaviour is tested.

A.32 Confirm terminal behaviour is tested where applicable.

A.33 Confirm locale behaviour is tested.

A.34 Confirm permission behaviour is tested.

A.35 Confirm package metadata integrity is tested.

A.36 Confirm signature verification is tested.

A.37 Confirm update behaviour is administrator-controlled.

A.38 Confirm reproducibility claims are scoped.

A.39 Confirm release signatures are verifiable.

A.40 Confirm certification claims use approved wording.

A.41 Confirm public registry information is accurate.

A.42 Confirm exceptions are listed.

A.43 Confirm all exceptions have review dates.

A.44 Confirm maintainers can respond to certification findings.

A.45 Confirm the release is ready for independent review.

---

## APPENDIX B. REVIEWER'S EVIDENCE CHECKLIST

B.1 Record product name and release identifier.

B.2 Record source repository and immutable revision.

B.3 Record build environment.

B.4 Record target platform and architecture.

B.5 Record selected conformance profile.

B.6 Record selected POSIX edition.

B.7 Record all test-suite versions.

B.8 Record test commands and environment variables.

B.9 Record all pass results.

B.10 Record all fail results.

B.11 Record all skipped tests.

B.12 Record all waived requirements.

B.13 Record all deviations from the reference layout.

B.14 Record all platform-specific interfaces.

B.15 Record dependency inventory results.

B.16 Record licence review results.

B.17 Record firmware review results.

B.18 Record build reproducibility results.

B.19 Record binary provenance results.

B.20 Record package signature verification results.

B.21 Record service inventory results.

B.22 Record network observation results.

B.23 Record telemetry review results.

B.24 Record privacy disclosure review results.

B.25 Record privilege-boundary review results.

B.26 Record filesystem test results.

B.27 Record process and signal test results.

B.28 Record terminal test results.

B.29 Record utility test results.

B.30 Record shell test results.

B.31 Record locale test results.

B.32 Record upgrade test results.

B.33 Record recovery test results.

B.34 Record documentation review results.

B.35 Record all security findings.

B.36 Record severity and impact for each finding.

B.37 Record the applicant's response.

B.38 Record the reviewer's disposition.

B.39 Record the final outcome.

B.40 Record the date and responsible reviewers.

---

## APPENDIX C. TERMS THAT MUST NOT BE CONFUSED

C.1 UNIX is a trademark and certification ecosystem; it is not a synonym for every POSIX-like system.

C.2 POSIX is a family of standards for portable operating-system interfaces and utilities.

C.3 SUS is the Single UNIX Specification and is associated with formal UNIX product certification.

C.4 BSD identifies a family of operating systems and historical software lineage.

C.5 Linux identifies an operating-system kernel; complete Linux distributions include additional userland and system components.

C.6 UNIX-like is a descriptive term and does not itself indicate formal certification.

C.7 NIX Certified is a separate Foundation conformance mark.

C.8 UNIX philosophy describes design principles and does not establish trademark rights.

C.9 A shell is a command interpreter; it is not the kernel.

C.10 A kernel is the privileged core that manages hardware and system resources; it is not a complete operating system by itself.

C.11 A userland is the collection of programs and libraries operating outside the kernel.

C.12 An ABI defines binary-level conventions and is distinct from a source API.

C.13 An API defines interfaces visible to software developers.

C.14 A filesystem hierarchy is a pathname organization and does not alone establish POSIX conformance.

C.15 A utility having the same name as a POSIX utility does not prove that it implements the required semantics.

C.16 A reproducible build is not automatically a secure build.

C.17 A signed binary is not automatically trustworthy unless the signing key and build provenance are trusted.

C.18 Open source does not automatically mean public domain.

C.19 Free software and open-source terminology overlap but have distinct institutional definitions.

C.20 A privacy claim requires evidence about actual system behaviour.

---

## APPENDIX D. PROFILE DECLARATION TEMPLATE

D.1 The product name MUST be stated.

D.2 The product version MUST be stated.

D.3 The source revision MUST be stated.

D.4 The target architecture MUST be stated.

D.5 The kernel family MUST be stated.

D.6 The kernel version MUST be stated.

D.7 The C library MUST be stated.

D.8 The ABI MUST be stated.

D.9 The shell MUST be stated.

D.10 The init/service manager MUST be stated.

D.11 The filesystem profile MUST be stated.

D.12 The utility implementation MUST be stated.

D.13 The POSIX edition MUST be stated.

D.14 Optional POSIX facilities MUST be listed.

D.15 Non-standard extensions MUST be listed.

D.16 Unsupported interfaces MUST be listed.

D.17 Network services MUST be listed.

D.18 Default outbound connections MUST be listed.

D.19 Telemetry policy MUST be stated.

D.20 Firmware policy MUST be stated.

D.21 Licensing policy MUST be stated.

D.22 Build procedure MUST be stated.

D.23 Test procedure MUST be stated.

D.24 Certification evidence location MUST be stated.

D.25 Known limitations MUST be stated.

D.26 Exceptions MUST be stated.

D.27 Certification tier MUST be stated.

D.28 Registry entry MUST be stated after approval.

## APPENDIX E. NORMATIVE CONSISTENCY AND CHANGE CONTROL

E.1 **Single source of policy.** Each cross-cutting policy MUST have one primary normative section. Other sections MAY summarize it, but summaries MUST refer to the primary section and MUST NOT create a different rule.

E.2 **No name-based product bans.** Unless a profile explicitly states otherwise with a technical rationale, a product, project, daemon, init system, kernel, libc, shell, or service manager MUST NOT be prohibited or approved solely by its name.

E.3 **Requirement classification.** Every requirement MUST be classifiable as one of: common mandatory requirement, profile-specific mandatory requirement, optional capability, recommendation, or informative example. Examples MUST NOT be interpreted as requirements.

E.4 **Platform scope.** A path, command, syscall, kernel parameter, service-manager interface, or device node that is not common to all selected platforms MUST be labelled with its platform scope. A platform-specific example MUST NOT be used as a universal conformance test.

E.5 **Exceptions.** Exceptions MUST be handled only through Section 22.3. A waiver MUST identify the exact clause, affected profile, release, risk, compensating control, approver, and review date.

E.6 **Conflict resolution.** When two mandatory clauses appear inconsistent, the reviewer MUST first apply the selected profile and the explicit scope of each clause. If the conflict remains, the reviewer MUST record the issue as an ambiguity and MUST NOT fail the candidate on the basis of either conflicting interpretation until the Foundation publishes a clarification or revision.

E.7 **Change impact.** Every revision MUST include a change log identifying modified normative clauses, affected profiles, compatibility impact, and any test vectors that changed.

E.8 **Cross-reference review.** Before publication, the Foundation MUST verify section references, defined terms, normative keywords, tables, profile applicability, and consistency between the standard and the certification verification procedures.

E.9 **No silent override.** A later section does not automatically override an earlier section merely because it appears later. Overrides MUST explicitly identify the clause being modified and state the replacement rule.

E.10 **Certification transition.** A revision that changes a mandatory requirement MUST define an effective date and a transition period for already-certified releases. Security-critical corrections MAY define an accelerated transition with a published rationale.
