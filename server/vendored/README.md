Using go-vendor-tools
=====================

Kudos to Miguel Martin for figuring this out:

Install the necessary go vendor tooling:

`$ dnf install go2rpm scancode-toolkit go-vendor-tools+scancode`

Create a config file for the go-vendor-tools called _go-vendor-tools.toml_


`$ cat go-vendor-tools.toml`
```
[archive]
compression_type = "bz2"

[licensing]
detector = "scancode"

[licensing.detector_config]
multiple = "true"
```

Run the go2rpm comnand:

`$ go2rpm --download --name go-fdo-server --profile vendor github.com/fido-device-onboard/go-fdo-server`

Fixup the resultant specfile: make sure the for loop in the %build section is commented or removed,
add GO111MODULE=on, fixup #FIXMEs, etc.

```
%build
%global gomodulesmode GO111MODULE=on
#for cmd in cmd/* ; do
#  %gobuild -o %{gobuilddir}/bin/$(basename $cmd) %{goipath}/$cmd
#done
%gobuild -o %{gobuilddir}/bin/go-fdo-server %{goipath}
```

Build the rpm with:

`rpmbuild -bb --define "_topdir ${PWD}/rpmbuild" --define "_sourcedir ${PWD}" go-fdo-server.spec`

The output will be in _rpmbuild/RPMS/$(uname -m)_

Copy the two tarfiles created by the go2rpm command (one contains the
target sources, the other the vendored sources) to your
rpmbuild/SOURCES directory.

Copy the _go-vendor-tools.toml_ file to the SOURCES directory as well.

Copy the _go-fdo-server.spec_ file to the rpmbuild/SPECS file.

You should be able to build the rpm, as well as the source rpm:

```
$ rpmbuild -bb rpmbuild/SPECS/go-fdo-server.spec
$ rpmbuild -bs rpmbuild/SPECS/go-fdo-server.spec
```
