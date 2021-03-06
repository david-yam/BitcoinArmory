# Makefile for BitcoinArmory bitcoin wallet
SUBDIRS = cppForSwig
PREFIX = /usr/local

EXTRA_DIST = *.py *.md LICENSE LICENSE-ATI LICENSE-MIT \
	nginx_example.conf imgList.xml autogen.sh \
	armoryengine/*.py ui/*.py \
	img/* lang/* dpkgfiles/* \
	windowsbuild/* linuxbuild/* osxbuild/*

ACLOCAL_AMFLAGS = -I m4

lrelease:
	lrelease lang/*.ts

qrc_img_resources.py: imgList.xml
	pyrcc4 -o qrc_img_resources.py imgList.xml

copy-script:
	cp cppForSwig/ArmoryDB ./ArmoryDB

# SWIG code and requirements.
	cp cppForSwig/CppBlockUtils.py ./CppBlockUtils.py
if BUILD_DARWIN
	cp cppForSwig/.libs/libCppBlockUtils.dylib ./_CppBlockUtils.so
else
	cp cppForSwig/.libs/libCppBlockUtils.so ./_CppBlockUtils.so
endif

.PHONY: copy-script lrelease qrc_img_resources.py

all-local: copy-script lrelease qrc_img_resources.py

#target to clean up pre autotools installation left overs
uninstall-old: 
	rm -f $(DESTDIR)$(PREFIX)/bin/ArmoryDB 

install-exec-hook: uninstall-old
	mkdir -p $(DESTDIR)$(PREFIX)/lib/armory/ui
	mkdir -p $(DESTDIR)$(PREFIX)/lib/armory/armoryengine
	mkdir -p $(DESTDIR)$(PREFIX)/lib/armory/lang
	mkdir -p $(DESTDIR)$(PREFIX)/bin
	mkdir -p $(DESTDIR)$(PREFIX)/share/applications
	cp *.py *.so README.md $(DESTDIR)$(PREFIX)/lib/armory
	cp -r ui/* $(DESTDIR)$(PREFIX)/lib/armory/ui
	cp -r armoryengine/* $(DESTDIR)$(PREFIX)/lib/armory/armoryengine
	cp lang/*.qm $(DESTDIR)$(PREFIX)/lib/armory/lang

# Sometimes, uninstall-old deletes a valid ArmoryDB. Copy again to be safe.
	cp ArmoryDB $(DESTDIR)$(PREFIX)/bin

# No need to install test binaries.
	rm -f $(DESTDIR)$(PREFIX)/bin/ContainerTests
	rm -f $(DESTDIR)$(PREFIX)/bin/CppBlockUtilsTests
	rm -f $(DESTDIR)$(PREFIX)/bin/DB1kIterTest

# Skip Linux-specific steps on OSX.
if ! BUILD_DARWIN
	rsync -rupE --exclude="img/.DS_Store" img $(DESTDIR)$(PREFIX)/share/armory/
	sed "s: /usr: $(PREFIX):g" < dpkgfiles/armory > $(DESTDIR)$(PREFIX)/bin/armory 
	chmod +x $(DESTDIR)$(PREFIX)/bin/armory
endif

uninstall-hook: uninstall-old
	rm -rf $(DESTDIR)$(PREFIX)/lib/armory
	rm -rf $(DESTDIR)$(PREFIX)/bin/armory
	rm -f  $(DESTDIR)$(PREFIX)/share/applications/armory*
	rm -rf $(DESTDIR)$(PREFIX)/share/armory

clean-local:
	rm -f ArmoryDB
	rm -f CppBlockUtils.py
	rm -f _CppBlockUtils.so
	rm -f CppBlockUtils.pyc
