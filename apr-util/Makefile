APR_TOOLCHAIN_PREFIX = $(TOOLCHAIN_INSTALL_DIR)/usr/local

Project               = apr-util
UserType              = Administrator
ToolType              = Commands
GnuAfterInstall       = install-plist post-install
Extra_Configure_Flags = --bindir=$(APR_TOOLCHAIN_PREFIX)/bin \
                        --with-apr=$(OBJROOT)/../apr --with-expat=/usr --without-berkeley-db --disable-util-dso --with-ldap

include $(MAKEFILEPATH)/CoreOS/ReleaseControl/GNUSource.make

Install_Flags  = DESTDIR=$(DSTROOT)
Install_Target = install

post-install:
	$(CP) $(DSTROOT)/usr/lib/libaprutil-1.0.dylib $(SYMROOT)
	$(STRIP) -x $(DSTROOT)/usr/lib/libaprutil-1.0.dylib
	$(RM) $(DSTROOT)/usr/lib/aprutil.exp
	$(RM) $(DSTROOT)/usr/lib/libaprutil-1.la
	$(RM) $(DSTROOT)/usr/lib/libaprutil-1.a

# Automatic Extract & Patch
AEP_Project    = apr-util
AEP_Version    = 1.5.4
AEP_ProjVers   = $(AEP_Project)-$(AEP_Version)
AEP_Filename   = $(AEP_ProjVers).tar.bz2
AEP_ExtractDir = $(AEP_ProjVers)
AEP_Patches    = LT_VERSION.diff ldap-deprecation.diff \
                 apu-config.in.diff \
                 CVE-2017-12618.diff

# Extract the source.
install_source::
	$(TAR) -C $(SRCROOT) -jxf $(SRCROOT)/$(AEP_Filename)
	$(RMDIR) $(SRCROOT)/$(Project)
	$(MV) $(SRCROOT)/$(AEP_ExtractDir) $(SRCROOT)/$(Project)
	for patchfile in $(AEP_Patches); do \
		(cd $(SRCROOT)/$(Project) && patch -p0 -F0 < $(SRCROOT)/files/$$patchfile) || exit 1; \
	done

OSL = $(DSTROOT)/usr/local/OpenSourceLicenses

install-plist:
	$(MKDIR) $(OSL)
	$(INSTALL_FILE) $(Sources)/LICENSE $(OSL)/$(Project).txt
