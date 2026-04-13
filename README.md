*This project has been created as part of the 42 curriculum by edsole-a.*

# Born2beroot

## Description

Born2beroot is a system administration project that introduces the fundamentals of virtualization and server configuration. The goal is to set up a minimal Linux virtual machine following strict rules: no graphical interface, enforced security policies, a configured firewall, SSH access, and a strong password policy — all managed without ever touching a GUI.

The project covers partitioning with LVM, configuring sudo, setting up UFW, and writing a monitoring script that broadcasts system info every 10 minutes via `wall`.

### Operating System Choice

This project was completed using **Debian**.

| | Debian | Rocky Linux |
|---|---|---|
| **Base** | Independent | RHEL-based |
| **Package manager** | `apt` / `.deb` | `dnf` / `.rpm` |
| **Default MAC** | AppArmor | SELinux |
| **Learning curve** | Gentler | Steeper |
| **Community** | Very large, general-purpose | Enterprise-focused |
| **Stability** | Very stable, conservative updates | Enterprise-grade stability |
| **Best for** | Beginners, servers, versatility | Enterprise environments, RHEL compatibility |

Debian was chosen because its documentation is extensive, its community is massive, and it is generally more approachable for a first server setup. Rocky Linux, while robust and enterprise-ready, brings the added complexity of SELinux and an RPM-based ecosystem that is harder to navigate without prior enterprise Linux experience.

### AppArmor vs SELinux

Both are **Mandatory Access Control (MAC)** frameworks that enforce security policies beyond traditional Unix permissions.

| | AppArmor | SELinux |
|---|---|---|
| **Model** | Path-based | Label-based |
| **Default on** | Debian/Ubuntu | RHEL/Rocky/Fedora |
| **Configuration** | Profile files, relatively readable | Policies and contexts, complex |
| **Ease of use** | Easier to write and debug profiles | Steeper learning curve |
| **Granularity** | Per-application profiles | System-wide label enforcement |
| **Failure mode** | Complain mode available | Can lock out the system if misconfigured |

AppArmor ships with Debian and restricts programs by name/path. SELinux, used on Rocky, labels every object on the system and enforces access based on those labels — far more granular, but significantly harder to configure correctly.

### UFW vs firewalld

Both are **front-ends** for the kernel's `netfilter` packet filtering system — they don't replace iptables, they manage it.

| | UFW | firewalld |
|---|---|---|
| **Full name** | Uncomplicated Firewall | firewalld |
| **Default on** | Debian/Ubuntu | RHEL/Rocky/Fedora |
| **Backend** | iptables / nftables | nftables (via D-Bus) |
| **Configuration** | Simple CLI commands | Zones and services, XML configs |
| **Dynamic rules** | No (requires reload) | Yes (rules apply live) |
| **Ease of use** | Very simple | More complex but more flexible |

UFW is intentionally simple — `ufw allow 4242` is all it takes to open a port. firewalld introduces the concept of zones (trusted, public, dmz, etc.) which is more powerful but overkill for a single minimal server.

### VirtualBox vs UTM

| | VirtualBox | UTM |
|---|---|---|
| **Platform** | Windows, Linux, macOS | macOS only |
| **Architecture** | x86/x86\_64 | x86\_64, ARM64, and emulation |
| **Apple Silicon** | Poor/no support natively | Native ARM64 support |
| **Hypervisor type** | Type 2 | Type 2 (uses QEMU/HVF) |
| **Performance on M-chip** | Very poor | Good |
| **License** | GPL / PUEL | Open source (GPLv3) |
| **Use at 42** | Standard on Intel Macs/PCs | Used on Apple Silicon Macs |

VirtualBox is the standard tool at 42, but on Apple Silicon machines it either does not run or performs poorly. UTM, built on QEMU with Apple's Hypervisor framework, runs natively on M-series chips and is the go-to alternative on those machines.

---

## Instructions

This project produces a `.vdi` (or `.utm`) virtual machine file — there is nothing to compile.

### Submitting

Only your `signature.txt` file must be present at the root of your Git repository. It contains the SHA1 signature of your virtual machine disk.

To generate it:

**On Linux:**
```bash
sha1sum <your_vm_name>.vdi
```

**On macOS:**
```bash
shasum <your_vm_name>.utm/Images/disk-0.qcow2
```

Paste the output hash into `signature.txt` and push it.

> ⚠️ Do not push your `.vdi` or `.utm` file. Only `signature.txt` goes in the repo.

### Evaluating / Running the VM

1. Open VirtualBox (or UTM on Apple Silicon).
2. Import or open the virtual machine.
3. Start it and log in with your configured user.
4. The monitoring script (`monitoring.sh`) runs automatically every 10 minutes via `cron`.

To check it manually:
```bash
sudo /usr/local/bin/monitoring.sh
```

To verify the cron job:
```bash
sudo crontab -u root -l
```

---

## Resources

### Documentation

- [Debian Official Documentation](https://www.debian.org/doc/)
- [Rocky Linux Documentation](https://docs.rockylinux.org/)
- [Linux man pages online](https://man7.org/linux/man-pages/)
- [AppArmor Wiki](https://wiki.ubuntu.com/AppArmor)
- [SELinux Project](https://selinuxproject.org/)
- [UFW Documentation (Ubuntu)](https://help.ubuntu.com/community/UFW)
- [firewalld Documentation](https://firewalld.org/documentation/)
- [LVM Administrator's Guide – Red Hat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/index)
- [SSH Hardening Guide](https://www.ssh.com/academy/ssh/sshd_config)
- [VirtualBox User Manual](https://www.virtualbox.org/manual/)
- [UTM Documentation](https://docs.getutm.app/)

### Articles & Tutorials

- [The Linux Command Line – William Shotts (free online)](https://linuxcommand.org/tlcl.php)
- [DigitalOcean – How To Set Up a Firewall with UFW](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu)
- [DigitalOcean – An Introduction to LVM](https://www.digitalocean.com/community/tutorials/an-introduction-to-lvm-concepts-terminology-and-operations)
- [Baeldung – Linux password policies with PAM](https://www.baeldung.com/linux/pam-configure-password-policy)

### AI Usage
AI was used to assist with this project in the following ways:

- **README generation** — the structure and content of this file was drafted with AI assistance and reviewed manually.
- **Conceptual clarification** — used to clarify differences between AppArmor/SELinux, UFW/firewalld, and LVM concepts before implementing them.


AI was **not** used to directly configure the virtual machine, generate shell commands to run blindly, or bypass understanding of any evaluated concept.
