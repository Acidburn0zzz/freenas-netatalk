## Makefile.am for config/

SUBDIRS = pam
SUFFIXES = .tmpl .

TMPLFILES = afp.conf.tmpl dbus-session.conf.tmpl
GENFILES = afp.conf dbus-session.conf
CLEANFILES = $(GENFILES)
EXTRA_DIST = $(TMPLFILES) extmap.conf netatalk-dbus.conf

OVERWRITE_CONFIG = @OVERWRITE_CONFIG@

CONFFILES = extmap.conf

pkgconfdir = @PKGCONFDIR@

if HAVE_DBUS_GLIB
dbusservicedir = $(DBUS_SYS_DIR)
dbusservice_DATA = netatalk-dbus.conf
endif

#
# rule to parse template files
#

.tmpl:
	sed -e s@:UAMS_PATH:@${UAMS_PATH}@ \
	    -e s@:ETCDIR:@${pkgconfdir}@ \
	    -e s@:COMPILED_BACKENDS:@"$(compiled_backends)"@ \
	    -e s@:DEFAULT_CNID_SCHEME:@$(DEFAULT_CNID_SCHEME)@ \
		-e s@:LOCALSTATEDIR:@"$(localstatedir)"@ \
	    <$< >$@

#
# install configuration files
#

install-data-local: install-config-files
	mkdir -pm 0755 $(DESTDIR)$(localstatedir)/netatalk/
	mkdir -pm 0755 $(DESTDIR)$(localstatedir)/netatalk/CNID/
	$(INSTALL_DATA) $(srcdir)/README $(DESTDIR)$(localstatedir)/netatalk/
	$(INSTALL_DATA) $(srcdir)/README $(DESTDIR)$(localstatedir)/netatalk/CNID/

uninstall-local:
	@for f in $(CONFFILES) $(GENFILES); do \
		echo rm -f $(DESTDIR)$(pkgconfdir)/$$f.sample; \
		rm -f $(DESTDIR)$(pkgconfdir)/$$f.sample; \
	done
	rm -f $(DESTDIR)$(pkgconfdir)/dbus-session.conf
	rm -f $(DESTDIR)$(localstatedir)/netatalk/README
	rm -f $(DESTDIR)$(localstatedir)/netatalk/CNID/README
if USE_DEBIAN_SYSV
	rm -f $(DESTDIR)/etc/default/netatalk
endif

install-config-files: $(CONFFILES) $(GENFILES)
	$(mkinstalldirs) $(DESTDIR)$(pkgconfdir)
	@for f in $(CONFFILES) ; do \
		echo "$(INSTALL_DATA) $$f $(DESTDIR)$(pkgconfdir)/$$f.sample"; \
		$(INSTALL_DATA) $$f $(DESTDIR)$(pkgconfdir)/$$f.sample; \
	done
	@for f in $(GENFILES); do \
		if [ $$f != "dbus-session.conf" ]; then \
			echo "$(INSTALL_DATA) $$f $(DESTDIR)$(pkgconfdir)/$$f.sample"; \
			$(INSTALL_DATA) $$f $(DESTDIR)$(pkgconfdir)/$$f.sample; \
		fi; \
	done
	$(INSTALL_DATA) dbus-session.conf $(DESTDIR)$(pkgconfdir)/dbus-session.conf.netatalk
