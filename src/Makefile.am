# Copyright (c) 2013-2016 The Crowncoin Core developers
# Distributed under the MIT software license, see the accompanying
# file COPYING or http://www.opensource.org/licenses/mit-license.php.

AM_LDFLAGS = $(PTHREAD_CFLAGS) $(LIBTOOL_LDFLAGS) $(HARDENED_LDFLAGS)
AM_CXXFLAGS = $(HARDENED_CXXFLAGS) $(ERROR_CXXFLAGS)
AM_CPPFLAGS = $(HARDENED_CPPFLAGS)
EXTRA_LIBRARIES =

if EMBEDDED_UNIVALUE
LIBUNIVALUE = univalue/libunivalue.la

$(LIBUNIVALUE): $(wildcard univalue/lib/*) $(wildcard univalue/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)
else
LIBUNIVALUE = $(UNIVALUE_LIBS)
endif

BITCOIN_CONFIG_INCLUDES=-I$(builddir)/config
BITCOIN_INCLUDES=-I$(builddir) -I$(builddir)/obj $(BDB_CPPFLAGS) $(BOOST_CPPFLAGS) $(LEVELDB_CPPFLAGS) $(CRYPTO_CFLAGS) $(SSL_CFLAGS)

LIBCROWNCOIN_SERVER=libCrowncoin_server.a
LIBCROWNCOIN_COMMON=libCrowncoin_common.a
LIBCROWNCOIN_CLI=libCrowncoin_cli.a
LIBCROWNCOINQT=qt/libCrowncoinqt.a
if ENABLE_WALLET
LIBCROWNCOIN_WALLET=libCrowncoin_wallet.a
endif

# Make is not made aware of per-object dependencies to avoid limiting building parallelization
# But to build the less dependent modules first, we manually select their order here:
EXTRA_LIBRARIES += \
  $(LIBCROWNCOIN_COMMON) \
  $(LIBCROWNCOIN_SERVER) \
  $(LIBCROWNCOIN_CLI) \
  $(LIBCROWNCOIN_WALLET) 

bin_PROGRAMS =
noinst_LIBRARIES = 
noinst_PROGRAMS =
TESTS =
BENCHMARKS =

if BUILD_BITCOIND
  bin_PROGRAMS += Crowncoind
endif

if BUILD_BITCOIN_UTILS
  bin_PROGRAMS += Crowncoin-cli
endif

.PHONY: FORCE
# Crowncoin core #
BITCOIN_CORE_H = \
  addrman.h \
  alert.h \
  allocators.h \
  base58.h \
  bignum.h \
  rpcclient.h \
  rpcprotocol.h \
  rpcserver.h \
  bloom.h \
  checkpoints.h \
  checkpointsync.h \
  checkqueue.h \
  clientversion.h \
  compat.h \
  crypter.h \
  db.h \
  hash.h \
  init.h \
  key.h \
  kernel.h \
  keystore.h \
  leveldbwrapper.h \
  limitedmap.h \
  main.h \
  mruset.h \
  netbase.h \
  net.h \
  protocol.h \
  script.h \
  serialize.h \
  sync.h \
  threadsafety.h \
  txdb.h \
  ui_interface.h \
  uint256.h \
  util.h \
  version.h \
  walletdb.h \
  wallet.h

JSON_H = \
  json/json_spirit.h \
  json/json_spirit_error_position.h \
  json/json_spirit_reader.h \
  json/json_spirit_reader_template.h \
  json/json_spirit_stream_reader.h \
  json/json_spirit_utils.h \
  json/json_spirit_value.h \
  json/json_spirit_writer.h \
  json/json_spirit_writer_template.h 

obj/build.h: FORCE
	@$(MKDIR_P) $(abs_top_builddir)/src/obj
	@$(top_srcdir)/share/genbuild.sh $(abs_top_builddir)/src/obj/build.h \
	  $(abs_top_srcdir)
libCrowncoin_common_a-version.$(OBJEXT): obj/build.h

# server: shared between Crowncoind and Crowncoin-qt
libCrowncoin_server_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(MINIUPNPC_CPPFLAGS) $(EVENT_CFLAGS) $(EVENT_PTHREADS_CFLAGS)
libCrowncoin_server_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libCrowncoin_server_a_SOURCES = \
  addrman.cpp \
  alert.cpp \
  rpcserver.cpp \
  bloom.cpp \
  checkpoints.cpp \
  checkpointsync.cpp \
  crypter.cpp \
  init.cpp \
  kernel.cpp \
  keystore.cpp \
  leveldbwrapper.cpp \
  main.cpp \
  net.cpp \
  noui.cpp \
  rpcblockchain.cpp \
  rpcnet.cpp \
  rpcrawtransaction.cpp \
  rpcserver.cpp \
  txdb.cpp \
  $(JSON_H) \
  $(BITCOIN_CORE_H)

# wallet: shared between Crowncoind and Crowncoin-qt, but only linked
# when wallet enabled  
libCrowncoin_wallet_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libCrowncoin_wallet_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)  
libCrowncoin_wallet_a_SOURCES = \
  db.cpp \
  rpcdump.cpp \
  rpcmining.cpp \
  rpcminting.cpp \
  script.cpp \
  kernelrecord.cpp \
  wallet.cpp \
  walletdb.cpp \
  rpcwallet.cpp \
  $(BITCOIN_CORE_H)


# common: shared between Crowncoind, and Crowncoin-qt and non-server tools
libCrowncoin_common_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libCrowncoin_common_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)  
libCrowncoin_common_a_SOURCES = \
  hash.cpp \
  key.cpp \
  netbase.cpp \
  protocol.cpp \
  rpcprotocol.cpp \
  sync.cpp \
  util.cpp \
  version.cpp \
  $(BITCOIN_CORE_H)


# cli: shared between Crowncoin-cli and Crowncoin-qt
libCrowncoin_cli_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libCrowncoin_cli_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)  
libCrowncoin_cli_a_SOURCES = \
  rpcclient.cpp \
  $(BITCOIN_CORE_H)

nodist_libCrowncoin_common_a_SOURCES = $(top_srcdir)/src/obj/build.h
#

# Crowncoind binary #
Crowncoind_SOURCES = Crowncoind.cpp
Crowncoind_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
Crowncoind_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
Crowncoind_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
Crowncoind_SOURCES += Crowncoind-res.rc
endif

Crowncoind_LDADD = \
  $(LIBCROWNCOIN_SERVER) \
  $(LIBCROWNCOIN_COMMON) \
  $(LIBCROWNCOIN_WALLET) \
  $(LIBCROWNCOIN_CLI) \
  $(LIBLEVELDB) \
  $(LIBMEMENV) 

Crowncoind_LDADD += $(BOOST_LIBS) $(BDB_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(MINIUPNPC_LIBS) $(EVENT_PTHREADS_LIBS) $(EVENT_LIBS) $(ZMQ_LIBS)

# Crowncoin-cli binary #
Crowncoin_cli_SOURCES = Crowncoin-cli.cpp
Crowncoin_cli_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(EVENT_CFLAGS)
Crowncoin_cli_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
Crowncoin_cli_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
Crowncoin_cli_SOURCES += Crowncoin-cli-res.rc
endif

Crowncoin_cli_LDADD = \
  $(LIBCROWNCOIN_CLI) \
  $(LIBCROWNCOIN_COMMON)

Crowncoin_cli_LDADD += $(BOOST_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(EVENT_LIBS)


CLEANFILES = $(EXTRA_LIBRARIES)
CLEANFILES += leveldb/libleveldb.a leveldb/libmemenv.a *.gcda *.gcno
DISTCLEANFILES = obj/build.h

EXTRA_DIST = leveldb


config/Crowncoin-config.h: config/stamp-h1
	@$(MAKE) -C $(top_builddir) $(subdir)/$(@)
config/stamp-h1: $(top_srcdir)/$(subdir)/config/Crowncoin-config.h.in $(top_builddir)/config.status
	$(AM_V_at)$(MAKE) -C $(top_builddir) $(subdir)/$(@)
$(top_srcdir)/$(subdir)/config/Crowncoin-config.h.in:  $(am__configure_deps)
	$(AM_V_at)$(MAKE) -C $(top_srcdir) $(subdir)/config/Crowncoin-config.h.in

clean-local:
	-$(MAKE) -C leveldb clean
	-rm -f leveldb/*/*.gcda leveldb/*/*.gcno leveldb/helpers/memenv/*.gcda leveldb/helpers/memenv/*.gcno
	-rm -f config.h
	-rm -rf test/__pycache__

.rc.o:
	@test -f $(WINDRES)
	## FIXME: How to get the appropriate modulename_CPPFLAGS in here?
	$(AM_V_GEN) $(WINDRES) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(CPPFLAGS) -DWINDRES_PREPROC -i $< -o $@

%.pb.cc %.pb.h: %.proto
	@test -f $(PROTOC)
	$(AM_V_GEN) $(PROTOC) --cpp_out=$(@D) --proto_path=$(<D) $<

if EMBEDDED_LEVELDB
include Makefile.leveldb.include
endif

if ENABLE_QT
include Makefile.qt.include
endif
