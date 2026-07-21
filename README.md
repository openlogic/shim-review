This repo is for review of requests for signing shim. To create a request for review:

- clone this repo (preferably fork it)
- edit the template below
- add the shim.efi to be signed
- add build logs
- add any additional binaries/certificates/SHA256 hashes that may be needed
- commit all of that
- tag it with a tag of the form "myorg-shim-arch-YYYYMMDD"
- push it to GitHub
- file an issue at https://github.com/rhboot/shim-review/issues with a link to your tag
- approval is ready when the "accepted" label is added to your issue

Note that we really only have experience with using GRUB2 or systemd-boot on Linux, so
asking us to endorse anything else for signing is going to require some convincing on
your part.

As of 20 October 2025, shims sent to Microsoft will be signed with the 2011 and 2023 keys. For each shim you submit, you will receive two copies back, each signed by a different key. Here is the latest information from Microsoft: https://techcommunity.microsoft.com/blog/hardware-dev-center/signing-with-the-new-2023-microsoft-uefi-certificates-what-submitters-need-to-kn/4455787

New signing requirements have also taken effect, and are available here: https://techcommunity.microsoft.com/blog/hardware-dev-center/updated-microsoft-uefi-signing-requirements/1062916 Please note that undergoing this shim review exempts you from yearly security audits, as long as your shim only hands off to open source boot loaders.

Hint: check the [docs](./docs/) directory in this repo for guidance on submission and getting your shim signed.

Here's the template:

*******************************************************************************
### What organization or people are asking to have this signed?
*******************************************************************************
Organization name and website:  
Perforce Software, Inc. [ http://perforce.com ]

*******************************************************************************
### What's the legal data that proves the organization's genuineness?
The reviewers should be able to easily verify, that your organization is a legal entity, to prevent abuse.
Provide the information, which can prove the genuineness with certainty.
*******************************************************************************
Company/tax register entries or equivalent:  
(a link to the organization entry in your jurisdiction's register will do)  

Result page is not directly linkable so please search for File Number 6256601
on this page: https://icis.corp.delaware.gov/ecorp/entitysearch/NameSearch.aspx

File Number: 6256601
Entity Name: PERFORCE SOFTWARE, INC.
Entity Kind: Corporation
Residency: Domestic
Incorporation Date / Formation Date: 12/20/2016
Entity Type: General
State: Delaware

The public details of both your organization and the issuer in the EV certificate used for signing .cab files at Microsoft Hardware Dev Center File Signing Services.  
(**not** the CA certificate embedded in your shim binary)

Example:

```
Issuer: O=MyIssuer, Ltd., CN=MyIssuer EV Code Signing CA
Subject: C=XX, O=MyCompany, Inc., CN=MyCompany, Inc.
```

```
Issuer: C = US, O = "DigiCert, Inc.", CN = DigiCert Trusted G4 Code Signing RSA4096 SHA384 2021 CA1
Subject: jurisdictionC = US, jurisdictionST = Delaware, businessCategory = Private Organization, serialNumber = 6256601, C = US, ST = California, L = Alameda, O = "Perforce Software, Inc.", CN = "Perforce Software, Inc."
```

*******************************************************************************
### What product or service is this for?
*******************************************************************************
OpenLogic by Perforce provides post-EoL support and security patching for
CentOS to assist our customers in maintaining secure systems while they migrate
to newer, current Linux distributions.

*******************************************************************************
### What's the justification that this really does need to be signed for the whole world to be able to boot it?
*******************************************************************************
OpenLogic by Perforce's customers have hundreds of thousands of systems
worldwide, which employ a mix of non-SecureBoot capable and SecureBoot enabled
systems from an unknown number of hardware vendors.  Providing our customers
with patches which do not boot in a SecureBoot-enabled environment would result
in catastrophic outages.  CentOS 7 and 8's packages were signed and recognized
by SecureBoot and we wish to continue providing the same SecureBoot security
capability after we patch the now-EoL CentOS packages for newly associated
vulnerabilities.  Our customers require the ability to install our package
updates without disabling SecureBoot or installing private/3rd-party MOKs.

*******************************************************************************
### Why are you unable to reuse shim from another distro that is already signed?
*******************************************************************************
EL7 and EL8 are EoL, so no additional security patches will be released for
CentOS 7 or 8 by Red Hat or the community.  We patch the EoL packages that are
bundled with CentOS 7 and 8, including shim, grub and kernel.  This requires us
to sign our own packages.

*******************************************************************************
### Who is the primary contact for security updates, etc.?
The security contacts need to be verified before the shim can be accepted. For subsequent requests, contact verification is only necessary if the security contacts or their PGP keys have changed since the last successful verification.

An authorized reviewer will initiate contact verification by sending each security contact a PGP-encrypted email containing random words.
You will be asked to post the contents of these mails in your `shim-review` issue to prove ownership of the email addresses and PGP keys.
*******************************************************************************
- Name: Richard Alloway
- Position: Principal Enterprise Architect and Enterprise Linux Team Lead
- Email address: ralloway@perforce.com
- PGP key fingerprint: D16C 446D 0A09 74F2 F045  B23C 4613 8509 C526 2402

(Key should be signed by the other security contacts, pushed to a keyserver
like keyserver.ubuntu.com, and preferably have signatures that are reasonably
well known in the Linux community.)

*******************************************************************************
### Who is the secondary contact for security updates, etc.?
*******************************************************************************
- Name: Tim Carroll
- Position: Director, Sales Engineering
- Email address: tcarroll@perforce.com
- PGP key fingerprint: 193D 7EE4 8CEF C4BD B75D  75E4 45FC AAE9 D158 BCE9

(Key should be signed by the other security contacts, pushed to a keyserver
like keyserver.ubuntu.com, and preferably have signatures that are reasonably
well known in the Linux community.)

*******************************************************************************
### Were these binaries created from the 16.1 shim release tar?
Please create your shim binaries starting with the 16.1 shim release tar file: https://github.com/rhboot/shim/releases/download/16.1/shim-16.1.tar.bz2

This matches https://github.com/rhboot/shim/releases/tag/16.1 and contains the appropriate gnu-efi source.

Make sure the tarball is correct by verifying your download's checksum
(SHA256, SHA512) with the following ones:

```
46319cd228d8f2c06c744241c0f342412329a7c630436fce7f82cf6936b1d603  shim-16.1.tar.bz2
ca5f80e82f3b80b622028f03ef23105c98ee1b6a25f52a59c823080a3202dd4b9962266489296e99f955eb92e36ce13e0b1d57f688350006bba45f2718f159fb  shim-16.1.tar.bz2
```

Make sure that you've verified that your build process uses that file
as a source of truth (excluding external patches) and its checksum
matches. You can also further validate the release by checking the PGP
signature: there's [a detached
signature](https://github.com/rhboot/shim/releases/download/16.1/shim-16.1.tar.bz2.asc)

The release is signed by the maintainer Peter Jones - his master key
has the fingerprint `B00B48BC731AA8840FED9FB0EED266B70F4FEF10` and the
signing sub-key in the signature here has the fingerprint
`02093E0D19DDE0F7DFFBB53C1FD3F540256A1372`. A copy of his public key
is included here for reference:
[pjones.asc](https://github.com/rhboot/shim-review/blob/main/pjones.asc)

Once you're sure that the tarball you are using is correct and
authentic, please confirm this here with a simple *yes*.

A short guide on verifying public keys and signatures should be available in the [docs](./docs/) directory.
*******************************************************************************
yes

*******************************************************************************
### URL for a repo that contains the exact code which was built to result in your binary:
Hint: If you attach all the patches and modifications that are being used to your application, you can point to the URL of your application here (*`https://github.com/YOUR_ORGANIZATION/shim-review`*).

You can also point to your custom git servers, where the code is hosted.
*******************************************************************************
https://github.com/openlogic/shim-review/tree/shim-review_16.1

*******************************************************************************
### What patches are being applied and why:
Mention all the external patches and build process modifications, which are used during your building process, that make your shim binary be the exact one that you posted as part of this application.
*******************************************************************************
Only a patch for the EL7 dos2unix flags

*******************************************************************************
### Do you have the NX bit set in your shim? If so, is your entire boot stack NX-compatible and what testing have you done to ensure such compatibility?

See https://techcommunity.microsoft.com/t5/hardware-dev-center/nx-exception-for-shim-community/ba-p/3976522 for more details on the signing of shim without NX bit.
*******************************************************************************
NX is disabled across the boot chain

*******************************************************************************
### What exact implementation of Secure Boot in GRUB2 do you have? (Either Upstream GRUB2 shim_lock verifier or Downstream RHEL/Fedora/Debian/Canonical-like implementation)
Skip this, if you're not using GRUB2.
*******************************************************************************
Downstream RHEL/Fedora-like implementation

*******************************************************************************
### Do you have fixes for all the following GRUB2 CVEs applied?
**Skip this, if you're not using GRUB2, otherwise make sure these are present and confirm with _yes_.**

* 2020 July - BootHole
  * Details: https://lists.gnu.org/archive/html/grub-devel/2020-07/msg00034.html
  * CVE-2020-10713
  * CVE-2020-14308
  * CVE-2020-14309
  * CVE-2020-14310
  * CVE-2020-14311
  * CVE-2020-15705
  * CVE-2020-15706
  * CVE-2020-15707
* March 2021
  * Details: https://lists.gnu.org/archive/html/grub-devel/2021-03/msg00007.html
  * CVE-2020-14372
  * CVE-2020-25632
  * CVE-2020-25647
  * CVE-2020-27749
  * CVE-2020-27779
  * CVE-2021-3418 (if you are shipping the shim_lock module)
  * CVE-2021-20225
  * CVE-2021-20233
* June 2022
  * Details: https://lists.gnu.org/archive/html/grub-devel/2022-06/msg00035.html, SBAT increase to 2
  * CVE-2021-3695
  * CVE-2021-3696
  * CVE-2021-3697
  * CVE-2022-28733
  * CVE-2022-28734
  * CVE-2022-28735
  * CVE-2022-28736
  * CVE-2022-28737
* November 2022
  * Details: https://lists.gnu.org/archive/html/grub-devel/2022-11/msg00059.html, SBAT increase to 3
  * CVE-2022-2601
  * CVE-2022-3775
* October 2023 - NTFS vulnerabilities
  * Details: https://lists.gnu.org/archive/html/grub-devel/2023-10/msg00028.html, SBAT increase to 4
  * CVE-2023-4693
  * CVE-2023-4692
* February 2025
  * Details: https://lists.gnu.org/archive/html/grub-devel/2025-02/msg00024.html, SBAT increase to 5
  * CVE-2024-45774
  * CVE-2024-45775
  * CVE-2024-45776
  * CVE-2024-45777
  * CVE-2024-45778
  * CVE-2024-45779
  * CVE-2024-45780
  * CVE-2024-45781
  * CVE-2024-45782
  * CVE-2024-45783
  * CVE-2025-0622
  * CVE-2025-0624
  * CVE-2025-0677
  * CVE-2025-0678
  * CVE-2025-0684
  * CVE-2025-0685
  * CVE-2025-0686
  * CVE-2025-0689
  * CVE-2025-0690
  * CVE-2025-1118
  * CVE-2025-1125
*******************************************************************************
yes

*******************************************************************************
### If shim is loading GRUB2 bootloader, and if these fixes have been applied, is the upstream global SBAT generation in your GRUB2 binary set to 5?
Skip this, if you're not using GRUB2, otherwise do you have an entry in your GRUB2 binary similar to:  
`grub,5,Free Software Foundation,grub,GRUB_UPSTREAM_VERSION,https://www.gnu.org/software/grub/`?
*******************************************************************************
Yes. Our GRUB2 binary's SBAT section includes `grub,5,Free Software
Foundation,grub,2.02,https://www.gnu.org/software/grub/`, in addition to our
own appended `grub.openlogic,1,OpenLogic,grub2,2.02-169_ol000.el7,mail:ralloway@perforce.com`
entry and the preserved `grub.rh,2,Red Hat,grub2,2.02-169_ol000.el7,mailto:secalert@redhat.com`
entry (see the SBAT entries listed later in this document).

*******************************************************************************
### Were old shims hashes provided to Microsoft for verification and to be added to future DBX updates?
### Does your new chain of trust disallow booting old GRUB2 builds affected by the CVEs?
If you had no previous signed shim, say so here. Otherwise a simple _yes_ will do.
*******************************************************************************
No previous signed shims

*******************************************************************************
### If your boot chain of trust includes a Linux kernel:
### Is upstream commit [1957a85b0032a81e6482ca4aab883643b8dae06e "efi: Restrict efivar_ssdt_load when the kernel is locked down"](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=1957a85b0032a81e6482ca4aab883643b8dae06e) applied?
### Is upstream commit [75b0cea7bf307f362057cc778efe89af4c615354 "ACPI: configfs: Disallow loading ACPI tables when locked down"](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=75b0cea7bf307f362057cc778efe89af4c615354) applied?
### Is upstream commit [eadb2f47a3ced5c64b23b90fd2a3463f63726066 "lockdown: also lock down previous kgdb use"](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=eadb2f47a3ced5c64b23b90fd2a3463f63726066) applied?
Hint: upstream kernels should have all these applied, but if you ship your own heavily-modified older kernel version, that is being maintained separately from upstream, this may not be the case.  
If you are shipping an older kernel, double-check your sources; maybe you do not have all the patches, but ship a configuration, that does not expose the issue(s).
*******************************************************************************
The efi and ACPI patches are not present but, per Red Hat, the EL7 kernel is not affected
The kgdb lockdown patch has been applied.

*******************************************************************************
### How does your signed kernel enforce lockdown when your system runs with Secure Boot enabled?
Hint: If it does not, we are not likely to sign your shim.
*******************************************************************************
Red Hat enforced lockdown in the EL7 kernel very early (Aug 2013): "efi: Enable secure boot lockdown automatically when enabled in firmware"

*******************************************************************************
### Do you build your signed kernel with additional local patches? What do they do?
*******************************************************************************
Yes.  We patch CVEs which were not patched while the EL7 kernel was actively maintained.

*******************************************************************************
### Do you use an ephemeral key for signing kernel modules?
### If not, please describe how you ensure that one kernel build does not load modules built for another kernel.
*******************************************************************************
Yes.  We use ephemeral keys.

*******************************************************************************
### If you use vendor_db functionality of providing multiple certificates and/or hashes please briefly describe your certificate setup.
### If there are allow-listed hashes please provide exact binaries for which hashes are created via file sharing service, available in public with anonymous access for verification.
*******************************************************************************
Yes. Our `VENDOR_DB_FILE` (`openlogic-and-centos-db.esl`) is a combined EFI
Signature List with three entries: our own OpenLogic CA (which signs our
GRUB2/kernel/fwupdate builds via a separate leaf signing cert), plus both of
CentOS's own production Secure Boot CA generations ("CentOS Secure Boot (CA
key 1)" and "CentOS Secure Boot CA 2"). The two CentOS CA entries exist so
that customers who update only our shim (and possibly GRUB2) package, while
still running their existing CentOS-signed kernel and/or fwupdate packages,
continue to boot without interruption.

We additionally use `VENDOR_DBX_FILE` (`openlogic_dbx.esl`) to hash-block
every distinct `grub2-efi`/`grub2-efi-x64` build ever shipped for CentOS 7
(23 builds across CentOS 7.0-7.9, spanning both the `os/` and `updates/`
trees on vault.centos.org) — see the next question for why this is
necessary despite trusting the CentOS CA.

*******************************************************************************
### If you are re-using the CA certificate from your last shim binary, you will need to add the hashes of the previous GRUB2 binaries exposed to the CVEs mentioned earlier to vendor_dbx in shim. Please describe your strategy.
This ensures that your new shim+GRUB2 can no longer chainload those older GRUB2 binaries with issues.

If this is your first application or you're using a new CA certificate, please say so here.
*******************************************************************************
This is our first application, and our own OpenLogic CA/leaf certificate is
new. However, our `VENDOR_DB_FILE` also trusts CentOS's own Secure Boot CA
(for backwards compatibility, described above), and that CA has already
signed GRUB2 builds affected by the CVEs listed earlier — CentOS 7's GRUB2
never received the SBAT-tracked fixes from June 2022 onward, and none of its
builds carry an SBAT section at all (CentOS 7 predates SBAT), so those CVEs
can't be addressed by SBAT generation and must be blocked by hash instead.

Our strategy: `VENDOR_DBX_FILE` (`openlogic_dbx.esl`) contains the
Authenticode PE-hash of every distinct `grub2-efi`/`grub2-efi-x64` build ever
shipped for CentOS 7 — 23 builds total, enumerated across all 10 CentOS 7.x
point releases (7.0 through 7.9) and both the `os/` and `updates/` trees on
vault.centos.org (the package was renamed `grub2-efi` to `grub2-efi-x64`
starting with 7.4). This ensures our shim cannot chainload any GRUB2 binary
CentOS has ever shipped, regardless of which CVEs it is or isn't patched
against — only GRUB2 built and signed by us (or a future CentOS build we
haven't yet enumerated, at which point we'd add its hash) can boot. The
CentOS CA entries in `VENDOR_DB_FILE` remain meaningful for the kernel and
fwupdate binaries they've signed, which are not affected by these GRUB2 CVEs.

*******************************************************************************
### Is the Dockerfile in your repository the recipe for reproducing the building of your shim binary?
A reviewer should always be able to run `docker build .` to get the exact binary you attached in your application.

Hint: Prefer using *frozen* packages for your toolchain, since an update to GCC, binutils, gnu-efi may result in building a shim binary with a different checksum.

If your shim binaries can't be reproduced using the provided Dockerfile, please explain why that's the case, what the differences would be and what build environment (OS and toolchain) is being used to reproduce this build? In this case please write a detailed guide, how to setup this build environment from scratch.
*******************************************************************************
Yes

*******************************************************************************
### Which files in this repo are the logs for your build?
This should include logs for creating the buildroots, applying patches, doing the build, creating the archives, etc.
*******************************************************************************
root.log.shim.20260721-092112.centos7.log
build.log.shim.20260721-092112.centos7.log

From a mock build (centos+epel-7-x86_64) of shim-16.1-1_ol001.el7.src.rpm
above, confirmed matching (build.log shows `VENDOR_DB_FILE=.../openlogic-and-
centos-db.esl VENDOR_DBX_FILE=.../openlogic_dbx.esl` on every compile
invocation, and the `pesign -h -P -i shimx64.efi`/`shimia32.efi` calls at the
end of %install match shim.spec exactly).

*******************************************************************************
### What changes were made in the distro's secure boot chain since your SHIM was last signed?
For example, signing new kernel's variants, UKI, systemd-boot, new certs, new CA, etc..

Skip this, if this is your first application for having shim signed.
*******************************************************************************

*******************************************************************************
### What is the SHA256 hash of your final shim binary?
*******************************************************************************
2afb856d0e59284bdc942fe7ba320d51844278c2c0fdc884881778459bae6c78  shimia32.efi
47e4c7b7a3773572e48ead668b728e257a5eef0ca5871a013811f7e22448577e  shimx64.efi

Rebuilt 260721 from the current shim.spec (VENDOR_DBX_FILE included) via
`shim-16.1-1_ol001.el7.src.rpm`, also checked into this repo. Verified: both
binaries are correctly unsigned (no Authenticode signature -- this is
expected, per Microsoft's own docs at learn.microsoft.com/en-us/windows-hardware/drivers/dashboard/file-signing-reqs,
the EV certificate signs the submission CAB file, not the shim binary
itself), and their embedded VENDOR_DB_FILE/VENDOR_DBX_FILE sections are
byte-identical to openlogic-and-centos-db.esl/openlogic_dbx.esl as packaged
in the SRPM above. The SBAT section also matches this document's entries
exactly (`shim.openlogic,1,OpenLogic,shim,16.1-1_ol001,ralloway@perforce.com`).

*******************************************************************************
### How do you manage and protect the keys used in your shim?
Describe the security strategy that is used for key protection. This can range from using hardware tokens like HSMs or Smartcards, air-gapped vaults, physical safes to other good practices.
*******************************************************************************
We are hosting the keys via DigiCert HSM with restricted access.

*******************************************************************************
### Do you use EV certificates as embedded certificates in the shim?
A _yes_ or _no_ will do. There's no penalty for the latter.
*******************************************************************************
Yes.

*******************************************************************************
### Are you embedding a CA certificate in your shim?
A _yes_ or _no_ will do. There's no penalty for the latter. However,
if _yes_: does that certificate include the X509v3 Basic Constraints
to say that it is a CA? See the [docs](./docs/) for more guidance
about this.
*******************************************************************************
Yes. Our own Perforce/OpenLogic CA (which signs our GRUB2/kernel/fwupdate
builds via a leaf signing cert) includes the X509v3 Basic Constraints CA:TRUE
extension. We also embed both of CentOS's own production Secure Boot CA
generations ("CentOS Secure Boot (CA key 1)" and "CentOS Secure Boot CA 2")
so that customers who update to our shim (and possibly GRUB2) package, but
continue running their existing CentOS-signed kernel and/or fwupdate
packages, will continue to boot. As described above, we pair this with a
`VENDOR_DBX_FILE` hash-blocking every CentOS-signed GRUB2 build ever shipped,
since GRUB2 (unlike kernel/fwupdate) has known CVEs the CentOS CA's signed
builds were never patched against.

*******************************************************************************
### Do you add a vendor-specific SBAT entry to the SBAT section in each binary that supports SBAT metadata ( GRUB2, fwupd, fwupdate, systemd-boot, systemd-stub, shim + all child shim binaries )?
### Please provide the exact SBAT entries for all binaries you are booting directly through shim.
Hint: The history of SBAT and more information on how it works can be found [here](https://github.com/rhboot/shim/blob/main/SBAT.md). That document is large, so for just some examples check out [SBAT.example.md](https://github.com/rhboot/shim/blob/main/SBAT.example.md)

If you are using a downstream implementation of GRUB2 (e.g. from Fedora or Debian), make sure you have their SBAT entries preserved and that you **append** your own (don't replace theirs) to simplify revocation.

**Remember to post the entries of all the binaries. Apart from your bootloader, you may also be shipping e.g. a firmware updater, which will also have these.**

Hint: run `objcopy --dump-section .sbat=/dev/stdout YOUR_EFI_BINARY` to get these entries. Paste them here. Preferably surround each listing with three backticks (\`\`\`), so they render well.
*******************************************************************************
shim:
```
sbat,1,SBAT Version,sbat,1,https://github.com/rhboot/shim/blob/main/SBAT.md
shim,4,UEFI shim,shim,1,https://github.com/rhboot/shim
shim.openlogic,1,OpenLogic,shim,16.1-1_ol001,ralloway@perforce.com
```
grub2:
```
sbat,1,SBAT Version,sbat,1,https://github.com/rhboot/shim/blob/main/SBAT.md
grub,5,Free Software Foundation,grub,2.02,https://www.gnu.org/software/grub/
grub.rh,2,Red Hat,grub2,2.02-169_ol000.el7,mailto:secalert@redhat.com
grub.openlogic,1,OpenLogic,grub2,2.02-169_ol000.el7,mail:ralloway@perforce.com
```
MokManager:
```
sbat,1,SBAT Version,sbat,1,https://github.com/rhboot/shim/blob/main/SBAT.md
shim,4,UEFI shim,shim,1,https://github.com/rhboot/shim
shim.openlogic,1,OpenLogic,shim,16.1-1_ol001,ralloway@perforce.com
```
fwupdate (fwupx64.efi — the one EFI binary shared by both the `fwupd` and
`fwupdate` packages; `fwupd` itself has no `.efi` of its own): N/A. Confirmed
via both the upstream `fwupdate-12` source (no "sbat" anywhere; the final
build step's `objcopy -j` section whitelist would strip a `.sbat` section
even if one existed) and the actual shipped binary (no `.sbat` section
present). Upstream `rhboot/fwupdate` was last released in 2018 and the
project's repo saw its last commit in March 2021, right as SBAT was being
introduced, and was superseded by `fwupd` before SBAT existed — there is no
upstream SBAT implementation to adopt. It is signed with our leaf cert.

kernel: N/A (the kernel image itself does not carry an SBAT section; it is
signed with our leaf cert)

*******************************************************************************
### If shim is loading GRUB2 bootloader, which modules are built into your signed GRUB2 image?
Skip this, if you're not using GRUB2.

Hint: this is about those modules that are in the binary itself, not the `.mod` files in your filesystem.
*******************************************************************************
```
acpi all_video archelp at_keyboard backtrace bitmap bitmap_scale blscfg boot
bufio cat chain configfile connectefi crypto cryptodisk datetime disk
diskfilter echo efifwsetup efi_gop efinet efi_netfs efi_uga ext2 extcmd fat
font fshelp gcry_crc gcry_rijndael gcry_rsa gcry_serpent gcry_sha256 gcry_sha512
gcry_whirlpool gettext gfxmenu gfxterm gzio halt http increment iso9660 jpeg
keylayouts linux loadenv loopback lsefimmap luks lvm mdraid09 mdraid1x minicmd
mmap mpi net normal part_apple part_gpt part_msdos password_pbkdf2 pbkdf2 png
priority_queue procfs reboot regexp search search_fs_file search_fs_uuid
search_label serial sleep syslinuxcfg terminal terminfo test tftp trig usb
usbserial_common usbserial_ftdi usbserial_pl2303 usbserial_usbdebug verifiers
video video_bochs video_cirrus video_colors video_fb xfs
```

*******************************************************************************
### If you are using systemd-boot on arm64 or riscv, is the fix for [unverified Devicetree Blob loading](https://github.com/systemd/systemd/security/advisories/GHSA-6m6p-rjcq-334c) included?
*******************************************************************************
We are not using systemd-boot on arm64 or riscv.

*******************************************************************************
### What is the origin and full version number of your bootloader (GRUB2 or systemd-boot or other)?
*******************************************************************************
We adapted grub2-2.02-169 from EL8 to satisfy the CVE patching requirements (Our full version is grub2-2.02-169_ol000.el7)

*******************************************************************************
### If your shim launches any other components apart from your bootloader, please provide further details on what is launched.
Hint: The most common case here will be a firmware updater like fwupd.
*******************************************************************************
fwupdate (fwupx64.efi — the one EFI binary shared by fwupd and fwupdate),
MokManager

*******************************************************************************
### If your GRUB2 or systemd-boot launches any other binaries that are not the Linux kernel in SecureBoot mode, please provide further details on what is launched and how it enforces Secureboot lockdown.
Skip this, if you're not using GRUB2 or systemd-boot.
*******************************************************************************
grub2 verifies signatures on booted kernels via shim. fwupd does not include code to launch other binaries, it can only load UEFI Capsule updates.

*******************************************************************************
### How do the launched components prevent execution of unauthenticated code?
Summarize in one or two sentences, how your secure bootchain works on higher level.
*******************************************************************************
All components have .sbat self checks and SecureBoot validation.

*******************************************************************************
### Does your shim load any loaders that support loading unsigned kernels (e.g. certain GRUB2 configurations)?
*******************************************************************************
No.

*******************************************************************************
### What kernel are you using? Which patches and configuration does it include to enforce Secure Boot?
*******************************************************************************
We are using the CentOS 7 kernel (3.10.0-1160.119.1), patched and rebuilt by
us since it is EoL upstream. Secure Boot lockdown itself is enforced by
Red Hat's own long-standing EL7 patch that automatically enables lockdown
when Secure Boot is active in firmware (applied since August 2013, per the
answer above). On top of that, we've added our own patch that closes a gap
in the existing lockdown coverage: kgdb_handle_exception() now declines to
enter the debugger once get_securelevel() > 0, mirroring the securelevel
checks the EL7 kernel already applies to kexec, MSR/IO-port access,
/dev/mem, and hibernation, and matching upstream's own
"lockdown: also lock down previous kgdb use" fix referenced earlier in this
document.

*******************************************************************************
### What contributions have you made to help us review the applications of other applicants?
The reviewing process is meant to be a peer-review effort and the best way to have your application reviewed faster is to help with reviewing others. We are in most cases volunteers working on this venue in our free time, rather than being employed and paid to review the applications during our business hours. 

A reasonable timeframe of waiting for a review can reach 2-3 months. Helping us is the best way to shorten this period. The more help we get, the faster and the smoother things will go.

For newcomers, the applications labeled as [*easy to review*](https://github.com/rhboot/shim-review/issues?q=is%3Aopen+is%3Aissue+label%3A%22easy+to+review%22) are recommended to start the contribution process.
*******************************************************************************
[your text here]

*******************************************************************************
### Add any additional information you think we may need to validate this shim signing application.
*******************************************************************************
[your text here]
