E1000=e1000/e1000-8.0.35
E1000E=e1000e/e1000e-3.2.7.1-zc
IGB=igb/igb-5.3.3.5-zc
IXGBE=ixgbe/ixgbe-5.0.4-zc
I40E=i40e/i40e-1.5.18-zc
FM10K=fm10k/fm10k-0.20.1-zc

all:
#	cd $(E1000)/src; make
	cd $(E1000E)/src; make
	cd $(IGB)/src; make
	cd $(IXGBE)/src; make
	cd $(I40E)/src; make
	cd $(FM10K)/src; make

clean:
#	cd $(E1000)/src; make clean
	cd $(E1000E)/src; make clean
	cd $(IGB)/src; make clean
	cd $(IXGBE)/src; make clean
	cd $(I40E)/src; make clean
	cd $(FM10K)/src; make clean
	-/bin/rm *.deb *.rpm


cleanup:
	sudo rm -rf /var/lib/dkms/*-zc  /lib/modules/`uname -r`/extra/*-zc.ko

dkms-deb-virtual:
	/bin/rm -rf tmp
	mkdir tmp
	cp -r pfring-drivers-zc-dkms tmp
	find tmp -name .git|xargs /bin/rm -rf	
	cd tmp; dpkg-deb -b pfring-drivers-zc-dkms .; mv *.deb ..

dkms-deb: cleanup dkms-deb-virtual
	sudo make -f Makefile.dkms.fm10k deb
	cp /var/lib/dkms/fm10k-zc/*/deb/*.deb .
	sudo make -f Makefile.dkms.i40e deb
	cp /var/lib/dkms/i40e-zc/*/deb/*.deb .
	sudo make -f Makefile.dkms.e1000e deb
	cp /var/lib/dkms/e1000e-zc/*/deb/*.deb .
	sudo make -f Makefile.dkms.igb deb
	cp /var/lib/dkms/igb-zc/*/deb/*.deb .
	sudo make -f Makefile.dkms.ixgbe deb
	cp /var/lib/dkms/ixgbe-zc/*/deb/*.deb .

dkms-rpm: cleanup dkms-rpm-virtual
	sudo make -f Makefile.dkms.fm10k rpm
	sudo make -f Makefile.dkms.i40e rpm
	sudo make -f Makefile.dkms.e1000e rpm
	sudo make -f Makefile.dkms.igb rpm
	sudo make -f Makefile.dkms.ixgbe rpm

dkms-rpm-virtual:
	rpmbuild -ba pfring-zc-rpm.spec
	cp ~/rpmbuild/RPMS/noarch/pfring-drivers-zc-dkms-*.noarch.rpm .
