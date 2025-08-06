COMMIT = $(shell (grep "commit" go-fdo-client.spec | awk '{print $$3}'))

RPM_TARBALL=$(CURDIR)/rpmbuild/SOURCES/go-fdo-client-$(COMMIT).tar.gz
VENDOR_TARBALL=$(CURDIR)/rpmbuild/SOURCES/go-fdo-client-$(COMMIT)-vendor.tar.bz2
CONFIG_FILE=$(CURDIR)/rpmbuild/SOURCES/go-vendor-tools.toml

.PHONY: all
all: srpm rpm

$(RPM_TARBALL):
	mkdir -p $(CURDIR)/rpmbuild/SOURCES
	spectool -g --directory $(CURDIR)/rpmbuild/SOURCES ./go-fdo-client.spec

$(VENDOR_TARBALL): $(RPM_TARBALL)
	mkdir -p $(CURDIR)/rpmbuild/SOURCES
	go_vendor_archive create -c ./go-vendor-tools.toml -O $(VENDOR_TARBALL) $(RPM_TARBALL)

$(CONFIG_FILE):
	mkdir -p $(CURDIR)/rpmbuild/SOURCES
	cp -f ./go-vendor-tools.toml $(CURDIR)/rpmbuild/SOURCES

.PHONY: srpm
srpm: $(RPM_TARBALL) $(VENDOR_TARBALL) $(CONFIG_FILE)
	rpmbuild -bs --define "_topdir $(CURDIR)/rpmbuild" ./go-fdo-client.spec

.PHONY: rpm
rpm: $(RPM_SPECFILE) $(RPM_TARBALL) $(CONFIG_FILE)
	rpmbuild -bb --define "_topdir $(CURDIR)/rpmbuild" ./go-fdo-client.spec

.PHONY: clean
clean:
	rm -rf ./rpmbuild
