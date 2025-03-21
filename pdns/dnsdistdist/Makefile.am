AM_CPPFLAGS += $(SYSTEMD_CFLAGS) \
	$(LUA_CFLAGS) \
	$(LIBEDIT_CFLAGS) \
	$(LIBSODIUM_CFLAGS) \
	$(FSTRM_CFLAGS) \
	$(YAHTTP_CFLAGS) \
	$(NET_SNMP_CFLAGS) \
	$(NGHTTP2_CFLAGS) \
	$(LIBCAP_CFLAGS) \
	-I$(top_srcdir)/ext/protozero/include \
	-DSYSCONFDIR=\"${sysconfdir}\" \
	-DBOOST_CONTAINER_USE_STD_EXCEPTIONS

ACLOCAL_AMFLAGS = -I m4

SUBDIRS=ext/ipcrypt \
	ext/yahttp

CLEANFILES = \
	htmlfiles.h.tmp \
	htmlfiles.h \
	dnsdist-lua-ffi-interface.inc

dnslabeltext.cc: dnslabeltext.rl
	$(AM_V_GEN)$(RAGEL) $< -o dnslabeltext.cc

BUILT_SOURCES=htmlfiles.h \
	dnsdist-lua-ffi-interface.inc \
	dnslabeltext.cc

htmlfiles.h: $(srcdir)/html/* $(srcdir)/incfiles
	$(AM_V_GEN)$(srcdir)/incfiles $(srcdir) > $@.tmp
	@mv $@.tmp $@

dnsdist-lua-ffi-interface.inc: dnsdist-lua-ffi-interface.h
	$(AM_V_GEN)echo 'R"FFIContent(' > $@
	@cat $< >> $@
	@echo ')FFIContent"' >> $@
SRC_JS_FILES := $(wildcard src_js/*.js)
MIN_JS_FILES := $(patsubst src_js/%.js,html/js/%.min.js,$(SRC_JS_FILES))

html/js/%.min.js: src_js/%.js
	uglifyjs $< > $@

min_js: $(MIN_JS_FILES)

if HAVE_RE2
# Some versions of pkg_config add -std=c11++, override that
AM_CPPFLAGS += $(RE2_CFLAGS)
AM_CPPFLAGS += -std=c++17
endif

if HAVE_DNS_OVER_TLS
if HAVE_LIBSSL
AM_CPPFLAGS += $(LIBSSL_CFLAGS)
endif

if HAVE_GNUTLS
AM_CPPFLAGS += $(GNUTLS_CFLAGS)
endif
endif

if HAVE_LIBCRYPTO
AM_CPPFLAGS += $(LIBCRYPTO_INCLUDES)
endif

if HAVE_CDB
AM_CPPFLAGS += $(CDB_CFLAGS)
endif

if HAVE_LMDB
AM_CPPFLAGS += $(LMDB_CFLAGS)
endif

if HAVE_DNS_OVER_HTTPS
if HAVE_LIBSSL
AM_CPPFLAGS += $(LIBSSL_CFLAGS)
endif

if HAVE_LIBH2OEVLOOP
AM_CPPFLAGS += $(LIBH2OEVLOOP_CFLAGS)
endif
endif

EXTRA_DIST=COPYING \
	   dnslabeltext.rl \
	   dnsdistconf.lua \
	   dnsmessage.proto \
	   dnstap.proto \
	   README.md \
	   delaypipe.cc delaypipe.hh \
	   html \
	   incfiles \
	   src_js \
	   dnsdist.service.in \
	   lua_hpp.mk \
	   bpf-filter.main.ebpf \
	   bpf-filter.qname.ebpf \
	   bpf-filter.ebpf.src \
	   DNSDIST-MIB.txt \
	   devpollmplexer.cc \
	   epollmplexer.cc \
	   kqueuemplexer.cc \
	   portsmplexer.cc \
	   cdb.cc cdb.hh \
	   ext/lmdb-safe/lmdb-safe.cc ext/lmdb-safe/lmdb-safe.hh \
	   ext/protozero/include/* \
	   builder-support/gen-version

bin_PROGRAMS = dnsdist

if UNIT_TESTS
noinst_PROGRAMS = testrunner
TESTS_ENVIRONMENT = env BOOST_TEST_LOG_LEVEL=message SRCDIR='$(srcdir)'
TESTS=testrunner
else
check-local:
	@echo "Unit tests are not enabled"
	@echo "Run ./configure --enable-unit-tests"
endif

dnsdist-web.$(OBJEXT): htmlfiles.h
dnsdist-lua-ffi.$(OBJEXT): dnsdist-lua-ffi-interface.inc

dnsdist_SOURCES = \
	base64.hh \
	bpf-filter.cc bpf-filter.hh \
	burtle.hh \
	cachecleaner.hh \
	capabilities.cc capabilities.hh \
	circular_buffer.hh \
	connection-management.hh \
	credentials.cc credentials.hh \
	dns.cc dns.hh \
	dns_random.hh \
	dnscrypt.cc dnscrypt.hh \
	dnsdist-backend.cc \
	dnsdist-cache.cc dnsdist-cache.hh \
	dnsdist-carbon.cc dnsdist-carbon.hh \
	dnsdist-console.cc dnsdist-console.hh \
	dnsdist-discovery.cc dnsdist-discovery.hh \
	dnsdist-dnscrypt.cc \
	dnsdist-dnsparser.cc dnsdist-dnsparser.hh \
	dnsdist-downstream-connection.hh \
	dnsdist-dynblocks.cc dnsdist-dynblocks.hh \
	dnsdist-dynbpf.cc dnsdist-dynbpf.hh \
	dnsdist-ecs.cc dnsdist-ecs.hh \
	dnsdist-healthchecks.cc dnsdist-healthchecks.hh \
	dnsdist-idstate.cc dnsdist-idstate.hh \
	dnsdist-kvs.hh dnsdist-kvs.cc \
	dnsdist-lbpolicies.cc dnsdist-lbpolicies.hh \
	dnsdist-lua-actions.cc \
	dnsdist-lua-bindings-dnscrypt.cc \
	dnsdist-lua-bindings-dnsparser.cc \
	dnsdist-lua-bindings-dnsquestion.cc \
	dnsdist-lua-bindings-kvs.cc \
	dnsdist-lua-bindings-network.cc \
	dnsdist-lua-bindings-packetcache.cc \
	dnsdist-lua-bindings-protobuf.cc \
	dnsdist-lua-bindings-rings.cc \
	dnsdist-lua-bindings.cc \
	dnsdist-lua-ffi-interface.h dnsdist-lua-ffi-interface.inc \
	dnsdist-lua-ffi.cc dnsdist-lua-ffi.hh \
	dnsdist-lua-inspection-ffi.cc dnsdist-lua-inspection-ffi.hh \
	dnsdist-lua-inspection.cc \
	dnsdist-lua-network.cc dnsdist-lua-network.hh \
	dnsdist-lua-rules.cc \
	dnsdist-lua-vars.cc \
	dnsdist-lua-web.cc \
	dnsdist-lua.cc dnsdist-lua.hh \
	dnsdist-mac-address.cc dnsdist-mac-address.hh \
	dnsdist-nghttp2.cc dnsdist-nghttp2.hh \
	dnsdist-prometheus.hh \
	dnsdist-protobuf.cc dnsdist-protobuf.hh \
	dnsdist-protocols.cc dnsdist-protocols.hh \
	dnsdist-proxy-protocol.cc dnsdist-proxy-protocol.hh \
	dnsdist-random.cc dnsdist-random.hh \
	dnsdist-rings.cc dnsdist-rings.hh \
	dnsdist-rules.cc dnsdist-rules.hh \
	dnsdist-secpoll.cc dnsdist-secpoll.hh \
	dnsdist-session-cache.cc dnsdist-session-cache.hh \
	dnsdist-snmp.cc dnsdist-snmp.hh \
	dnsdist-svc.cc dnsdist-svc.hh \
	dnsdist-systemd.cc dnsdist-systemd.hh \
	dnsdist-tcp-downstream.cc dnsdist-tcp-downstream.hh \
	dnsdist-tcp-upstream.hh \
	dnsdist-tcp.cc dnsdist-tcp.hh \
	dnsdist-web.cc dnsdist-web.hh \
	dnsdist-xpf.cc dnsdist-xpf.hh \
	dnsdist.cc dnsdist.hh \
	dnslabeltext.cc \
	dnsname.cc dnsname.hh \
	dnsparser.hh dnsparser.cc \
	dnstap.cc dnstap.hh \
	dnswriter.cc dnswriter.hh \
	doh.hh doh.cc \
	dolog.hh \
	ednscookies.cc ednscookies.hh \
	ednsoptions.cc ednsoptions.hh \
	ednssubnet.cc ednssubnet.hh \
	ext/json11/json11.cpp \
	ext/json11/json11.hpp \
	ext/libbpf/libbpf.h \
	ext/luawrapper/include/LuaContext.hpp \
	fstrm_logger.cc fstrm_logger.hh \
	gettime.cc gettime.hh \
	htmlfiles.h \
	iputils.cc iputils.hh \
	libssl.cc libssl.hh \
	lock.hh \
	logging.hh \
	misc.cc misc.hh \
	mplexer.hh \
	namespaces.hh \
	noinitvector.hh \
	packetcache.hh \
	pdnsexception.hh \
	pollmplexer.cc \
	protozero.cc protozero.hh \
	proxy-protocol.cc proxy-protocol.hh \
	qtype.cc qtype.hh \
	remote_logger.cc remote_logger.hh \
	sholder.hh \
	snmp-agent.cc snmp-agent.hh \
	sodcrypto.cc sodcrypto.hh \
	sstuff.hh \
	stat_t.hh \
	statnode.cc statnode.hh \
	svc-records.cc svc-records.hh \
	tcpiohandler-mplexer.hh \
	tcpiohandler.cc tcpiohandler.hh \
	threadname.hh threadname.cc \
	uuid-utils.hh uuid-utils.cc \
	xpf.cc xpf.hh

testrunner_SOURCES = \
	base64.hh \
	bpf-filter.cc bpf-filter.hh \
	cachecleaner.hh \
	circular_buffer.hh \
	connection-management.hh \
	credentials.cc credentials.hh \
	dns.cc dns.hh \
	dnscrypt.cc dnscrypt.hh \
	dnsdist-backend.cc \
	dnsdist-cache.cc dnsdist-cache.hh \
	dnsdist-dnsparser.cc dnsdist-dnsparser.hh \
	dnsdist-downstream-connection.hh \
	dnsdist-dynblocks.cc dnsdist-dynblocks.hh \
	dnsdist-dynbpf.cc dnsdist-dynbpf.hh \
	dnsdist-ecs.cc dnsdist-ecs.hh \
	dnsdist-idstate.cc dnsdist-idstate.hh \
	dnsdist-kvs.cc dnsdist-kvs.hh \
	dnsdist-lbpolicies.cc dnsdist-lbpolicies.hh \
	dnsdist-lua-bindings-dnsquestion.cc \
	dnsdist-lua-bindings-kvs.cc \
	dnsdist-lua-bindings.cc \
	dnsdist-lua-ffi-interface.h dnsdist-lua-ffi-interface.inc \
	dnsdist-lua-ffi.cc dnsdist-lua-ffi.hh \
	dnsdist-lua-network.cc dnsdist-lua-network.hh \
	dnsdist-lua-vars.cc \
	dnsdist-mac-address.cc dnsdist-mac-address.hh \
	dnsdist-nghttp2.cc dnsdist-nghttp2.hh \
	dnsdist-protocols.cc dnsdist-protocols.hh \
	dnsdist-proxy-protocol.cc dnsdist-proxy-protocol.hh \
	dnsdist-random.cc dnsdist-random.hh \
	dnsdist-rings.cc dnsdist-rings.hh \
	dnsdist-rules.cc dnsdist-rules.hh \
	dnsdist-session-cache.cc dnsdist-session-cache.hh \
	dnsdist-svc.cc dnsdist-svc.hh \
	dnsdist-tcp-downstream.cc \
	dnsdist-tcp.cc dnsdist-tcp.hh \
	dnsdist-xpf.cc dnsdist-xpf.hh \
	dnsdist.hh \
	dnslabeltext.cc \
	dnsname.cc dnsname.hh \
	dnsparser.hh dnsparser.cc \
	dnswriter.cc dnswriter.hh \
	dolog.hh \
	ednscookies.cc ednscookies.hh \
	ednsoptions.cc ednsoptions.hh \
	ednssubnet.cc ednssubnet.hh \
	ext/luawrapper/include/LuaContext.hpp \
	gettime.cc gettime.hh \
	iputils.cc iputils.hh \
	misc.cc misc.hh \
	namespaces.hh \
	noinitvector.hh \
	pdnsexception.hh \
	pollmplexer.cc \
	proxy-protocol.cc proxy-protocol.hh \
	qtype.cc qtype.hh \
	sholder.hh \
	sodcrypto.cc \
	sstuff.hh \
	stat_t.hh \
	statnode.cc statnode.hh \
	svc-records.cc svc-records.hh \
	test-base64_cc.cc \
	test-connectionmanagement_hh.cc \
	test-credentials_cc.cc \
	test-delaypipe_hh.cc \
	test-dnscrypt_cc.cc \
	test-dnsdist-connections-cache.cc \
	test-dnsdist-dnsparser.cc \
	test-dnsdist_cc.cc \
	test-dnsdistbackend_cc.cc \
	test-dnsdistdynblocks_hh.cc \
	test-dnsdistkvs_cc.cc \
	test-dnsdistlbpolicies_cc.cc \
	test-dnsdistluanetwork.cc \
	test-dnsdistnghttp2_cc.cc \
	test-dnsdistpacketcache_cc.cc \
	test-dnsdistrings_cc.cc \
	test-dnsdistrules_cc.cc \
	test-dnsdistsvc_cc.cc \
	test-dnsdisttcp_cc.cc \
	test-dnsparser_cc.cc \
	test-iputils_hh.cc \
	test-luawrapper.cc \
	test-mplexer.cc \
	test-proxy_protocol_cc.cc \
	testrunner.cc \
	threadname.hh threadname.cc \
	uuid-utils.hh uuid-utils.cc \
	xpf.cc xpf.hh

dnsdist_LDFLAGS = \
	$(AM_LDFLAGS) \
	$(PROGRAM_LDFLAGS) \
	-pthread

dnsdist_LDADD = \
	$(LUA_LIBS) \
	$(LIBEDIT_LIBS) \
	$(RT_LIBS) \
	$(YAHTTP_LIBS) \
	$(LIBSODIUM_LIBS) \
	$(FSTRM_LIBS) \
	$(SYSTEMD_LIBS) \
	$(NET_SNMP_LIBS) \
	$(LIBCAP_LIBS) \
	$(IPCRYPT_LIBS)

testrunner_LDFLAGS = \
	$(AM_LDFLAGS) \
	$(PROGRAM_LDFLAGS) \
	$(BOOST_UNIT_TEST_FRAMEWORK_LDFLAGS) \
	-pthread

testrunner_LDADD = \
	$(BOOST_UNIT_TEST_FRAMEWORK_LIBS) \
	$(FSTRM_LIBS) \
	$(LIBSODIUM_LIBS) \
	$(LUA_LIBS) \
	$(RT_LIBS) \
	$(LIBCAP_LIBS)

if HAVE_CDB
dnsdist_LDADD += $(CDB_LDFLAGS) $(CDB_LIBS)
testrunner_LDADD += $(CDB_LDFLAGS) $(CDB_LIBS)
dnsdist_SOURCES += cdb.cc cdb.hh
testrunner_SOURCES += cdb.cc cdb.hh
endif

if HAVE_RE2
dnsdist_LDADD += $(RE2_LIBS)
endif

if HAVE_LIBSSL
dnsdist_LDADD += $(LIBSSL_LIBS)
endif

if HAVE_LIBCRYPTO
dnsdist_LDADD += $(LIBCRYPTO_LDFLAGS) $(LIBCRYPTO_LIBS)
testrunner_LDADD += $(LIBCRYPTO_LDFLAGS) $(LIBCRYPTO_LIBS)
dnsdist_SOURCES += ipcipher.cc ipcipher.hh
endif

if HAVE_LMDB
dnsdist_LDADD += $(LMDB_LDFLAGS) $(LMDB_LIBS)
testrunner_LDADD += $(LMDB_LDFLAGS) $(LMDB_LIBS)
dnsdist_SOURCES += ext/lmdb-safe/lmdb-safe.cc ext/lmdb-safe/lmdb-safe.hh
testrunner_SOURCES += ext/lmdb-safe/lmdb-safe.cc ext/lmdb-safe/lmdb-safe.hh
endif

if HAVE_DNS_OVER_TLS
if HAVE_GNUTLS
dnsdist_LDADD += -lgnutls
endif
endif

if HAVE_DNS_OVER_HTTPS

if HAVE_LIBH2OEVLOOP
dnsdist_LDADD += $(LIBH2OEVLOOP_LIBS)
endif

endif

if HAVE_NGHTTP2
dnsdist_LDADD += $(NGHTTP2_LDFLAGS) $(NGHTTP2_LIBS)
testrunner_LDADD += $(NGHTTP2_LDFLAGS) $(NGHTTP2_LIBS)
endif

if !HAVE_LUA_HPP
BUILT_SOURCES += lua.hpp
nodist_dnsdist_SOURCES = lua.hpp
endif

CLEANFILES += lua.hpp

if HAVE_FREEBSD
dnsdist_SOURCES += kqueuemplexer.cc
testrunner_SOURCES += kqueuemplexer.cc
endif

if HAVE_OPENBSD
dnsdist_SOURCES += kqueuemplexer.cc
testrunner_SOURCES += kqueuemplexer.cc
endif

if HAVE_LINUX
dnsdist_SOURCES += epollmplexer.cc
testrunner_SOURCES += epollmplexer.cc
endif

if HAVE_SOLARIS
dnsdist_SOURCES += \
	devpollmplexer.cc \
	portsmplexer.cc
testrunner_SOURCES += \
	devpollmplexer.cc \
	portsmplexer.cc
endif

MANPAGES=dnsdist.1

dist_man_MANS=$(MANPAGES)

if HAVE_VENV
if !HAVE_MANPAGES
$(MANPAGES): %: docs/manpages/%.rst .venv
	$(AM_V_GEN).venv/bin/python -msphinx -b man docs . $<
endif # if !HAVE_MANPAGES

.venv: docs/requirements.txt
	$(PYTHON) -m venv .venv
	.venv/bin/pip install -U pip setuptools setuptools-git wheel
	.venv/bin/pip install -r $<

latex/dnsdist.pdf: docs/** .venv
	.venv/bin/python -msphinx -M latexpdf docs .

dnsdist.pdf: latex/dnsdist.pdf
	mv $< $@

html-docs.tar.bz2: html-docs
	tar cjf $@ $<

html-docs: docs/** .venv
	.venv/bin/python -msphinx -b html docs html-docs

all-docs: html-docs html-docs.tar.bz2 dnsdist.pdf

upload-docs: all-docs
	rsync -crv --delete --no-p --chmod=g=rwX --exclude '*~' ./html-docs/ web1.powerdns.com:/srv/www/dnsdist.org
	rsync -crv --no-p --chmod=g=rwX --exclude '*~' ./html-docs.tar.bz2 web1.powerdns.com:/srv/www/dnsdist.org
	rsync -crv --no-p --chmod=g=rwX --exclude '*~' ./dnsdist.pdf web1.powerdns.com:/srv/www/dnsdist.org

else # if HAVE_VENV
$(MANPAGES):
	@echo "You need Python 3 and the 'venv' module to generate the manpages"
	exit 1

html-docs: %: docs/manpages/%.rst .venv
	@echo "You need Python 3 and the 'venv' module to generate the HTML docs"
	exit 1

dnsdist.pdf:
	@echo "You need Python 3 and the 'venv' module to generate the PDF"
	exit 1
endif

if HAVE_SYSTEMD
dnsdist.service: dnsdist.service.in
	$(AM_V_GEN)sed -e 's![@]bindir[@]!$(bindir)!' -e 's![@]service_user[@]!$(service_user)!' -e 's![@]service_group[@]!$(service_group)!' < $< > $@
if !HAVE_SYSTEMD_LOCK_PERSONALITY
	$(AM_V_GEN)perl -ni -e 'print unless /^LockPersonality/' $@
endif
if !HAVE_SYSTEMD_PRIVATE_DEVICES
	$(AM_V_GEN)perl -ni -e 'print unless /^PrivateDevices/' $@
endif
if !HAVE_SYSTEMD_PRIVATE_TMP
	$(AM_V_GEN)perl -ni -e 'print unless /^PrivateTmp/' $@
endif
if !HAVE_SYSTEMD_PRIVATE_USERS
	$(AM_V_GEN)perl -ni -e 'print unless /^PrivateUsers/' $@
endif
if !HAVE_SYSTEMD_PROTECT_CLOCK
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectClock/' $@
endif
if !HAVE_SYSTEMD_PROTECT_CONTROL_GROUPS
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectControlGroups/' $@
endif
if !HAVE_SYSTEMD_PROTECT_HOME
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectHome/' $@
endif
if !HAVE_SYSTEMD_PROTECT_HOSTNAME
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectHostname/' $@
endif
if !HAVE_SYSTEMD_PROTECT_KERNEL_LOGS
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectKernelLogs/' $@
endif
if !HAVE_SYSTEMD_PROTECT_KERNEL_MODULES
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectKernelModules/' $@
endif
if !HAVE_SYSTEMD_PROTECT_KERNEL_TUNABLES
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectKernelTunables/' $@
endif
if !HAVE_SYSTEMD_PROTECT_SYSTEM
	$(AM_V_GEN)perl -ni -e 'print unless /^ProtectSystem/' $@
endif
if !HAVE_SYSTEMD_RESTRICT_ADDRESS_FAMILIES
	$(AM_V_GEN)perl -ni -e 'print unless /^RestrictAddressFamilies/' $@
endif
if !HAVE_SYSTEMD_RESTRICT_NAMESPACES
	$(AM_V_GEN)perl -ni -e 'print unless /^RestrictNamespaces/' $@
endif
if !HAVE_SYSTEMD_RESTRICT_REALTIME
	$(AM_V_GEN)perl -ni -e 'print unless /^RestrictRealtime/' $@
endif
if !HAVE_SYSTEMD_RESTRICT_SUIDSGID
	$(AM_V_GEN)perl -ni -e 'print unless /^RestrictSUIDSGID/' $@
endif
if !HAVE_SYSTEMD_SYSTEM_CALL_ARCHITECTURES
	$(AM_V_GEN)perl -ni -e 'print unless /^SystemCallArchitectures/' $@
endif
if !HAVE_SYSTEMD_SYSTEM_CALL_FILTER
	$(AM_V_GEN)perl -ni -e 'print unless /^SystemCallFilter/' $@
endif

dnsdist@.service: dnsdist.service
	$(AM_V_GEN)sed -e 's!/dnsdist !&--config $(sysconfdir)/dnsdist-%i.conf !' \
	  -e 's!RuntimeDirectory=.*!&-%i!' \
	  -e 's!SyslogIdentifier=.*!&-%i!' \
	  < $< >$@

systemdsystemunitdir = $(SYSTEMD_DIR)

systemdsystemunit_DATA = \
	dnsdist.service \
	dnsdist@.service

CLEANFILES += \
	dnsdist.service \
	dnsdist@.service
endif

if !HAVE_LUA_HPP
include lua_hpp.mk
endif
