curpwd      := $(shell pwd)
kver        ?= $(shell uname -r)
fedoraver   := $(shell sed -n 's/.*Fedora release \([^ ]*\).*/\1/p' /etc/fedora-release 2>/dev/null || echo 0)

# Call from the repo root with:
# make -f .copr/Makefile srpm outdir=".out" spec="fedora/nct6687d-kmod.spec"
# make -f .copr/Makefile srpm outdir=".out" spec="fedora/nct6687d.spec"

# Runs in the root
srpm:
	# echo Yo
	dnf install -y git
	# ls -la
	# find .
	# cat /etc/fedora-release
	# git log


	$(eval commitcount := $(shell git rev-list --all --count 2>/dev/null))
	$(eval commithash  := $(shell git rev-parse --short HEAD 2>/dev/null))

	mkdir -p ${curpwd}/.tmp/nct6687d-1.0.${commitcount}/nct6687d
	cp LICENSE Makefile nct6687.c ${curpwd}/.tmp/nct6687d-1.0.${commitcount}/nct6687d
	cd .tmp && tar -czvf nct6687d-1.0.${commitcount}.tar.gz nct6687d-1.0.${commitcount} && cd -
	mkdir -p ${curpwd}/.tmp/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
	cp ${curpwd}/.tmp/nct6687d-1.0.${commitcount}.tar.gz ${curpwd}/.tmp/rpmbuild/SOURCES/
	echo 'nct6687' | tee ${curpwd}/.tmp/rpmbuild/SOURCES/nct6687.conf
	# cp fedora/*.spec ${curpwd}/.tmp/rpmbuild/SPECS/
	cp $(spec) ${curpwd}/.tmp/rpmbuild/SPECS/
	sed -i "s/MAKEFILE_PKGVER/${commitcount}/g" ${curpwd}/.tmp/rpmbuild/SPECS/*
	sed -i "s/MAKEFILE_COMMITHASH/${commithash}/g" ${curpwd}/.tmp/rpmbuild/SPECS/*
	# rpmbuild -bs --define "_topdir ${curpwd}/.tmp/rpmbuild" ${curpwd}/.tmp/rpmbuild/SPECS/nct6687d.spec
	rpmbuild -bs --define "_topdir ${curpwd}/.tmp/rpmbuild" ${curpwd}/.tmp/rpmbuild/SPECS/*.spec


	# $(MAKE) akmod/build
	# cp .tmp/rpmbuild/SRPMS/*.src.rpm $(outdir)
