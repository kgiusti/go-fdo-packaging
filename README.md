RPM spec files for the FIDO Device Onboard server and client tools.

This is a proof of concept. At some point this repo will be removed.

_THIS IS NOT OFFICIALLY ASSOCIATED WITH THE FIDO-DEVICE-ONBOARD GITHUB PROJECT_

The RPMs are not built from the fido-device-onboard sources.  Rather
these are being built from a presonal fork of those sources. This is a
proof-of-concept and is not recommended for production use.

The "official" fido-device-onboard sources are here:

https://github.com/fido-device-onboard

The RPMs are configured to build from the source found at my personal fork:

https://github.com/kgiusti/go-fdo-client
https://github.com/kgiusti/go-fdo-server

---

Note: the go-mods-to-bundled-provides.py script is a tool to generate
the Provides: statements in the spec files, using the vendor sources
in the project's source tree.  It originally comes from the Coreos
ignition project:

https://src.fedoraproject.org/rpms/ignition.git
