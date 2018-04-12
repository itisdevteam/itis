.NOTPARALLEL :

SOURCES_PATH ?= $(BASEDIR)/sources
BASE_CACHE ?= $(BASEDIR)/built
SDK_PATH ?= $(BASEDIR)/SDKs
NO_QT ?=
NO_WALLET ?=
NO_UPNP ?=
FALLBACK_DOWNLOAD_PATH ?= https://bitcoincore.org/depends-sources

BUILD = $(shell ./config.guess)
HOST ?= $(BUILD)
PATCHES_PATH = $(BASEDIR)/patches
BASEDIR = $(CURDIR)
HASH_LENGTH:=11
DOWNLOAD_CONNECT_TIMEOUT:=10
DOWNLOAD_RETRIES:=3
HOST_ID_SALT ?= salt
BUILD_ID_SALT ?= salt

host:=$(BUILD)
ifneq ($(HOST),)
host:=$(HOST)
host_toolchain:=$(HOST)-
endif

ifneq ($(DEBUG),)
release_type=debug
else
release_type=release
endif

base_build_dir=$(BASEDIR)/work/build
base_staging_dir=$(BASEDIR)/work/staging
base_download_dir=$(BASEDIR)/work/download
canonical_host:=$(shell ./config.sub $(HOST))
build:=$(shell ./config.sub $(BUILD))

build_arch =$(firstword $(subst -, ,$(build)))
build_vendor=$(word 2,$(subst -, ,$(build)))
full_build_os:=$(subst $(build_arch)-$(build_vendor)-,,$(build))
build_os:=$(findstring linux,$(full_build_os))
build_os+=$(findstring darwin,$(full_build_os))
build_os:=$(strip $(build_os))
ifeq ($(build_os),)
build_os=$(full_build_os)
endif

host_arch=$(firstword $(subst -, ,$(canonical_host)))
host_vendor=$(word 2,$(subst -, ,$(canonical_host)))
full_host_os:=$(subst $(host_arch)-$(host_vendor)-,,$(canonical_host))
host_os:=$(findstring linux,$(full_host_os))
host_os+=$(findstring darwin,$(full_host_os))
host_os+=$(findstring mingw32,$(full_host_os))
host_os:=$(strip $(host_os))
ifeq ($(host_os),)
host_os=$(full_host_os)
endif

$(host_arch)_$(host_os)_prefix=$(BASEDIR)/$(host)
$(host_arch)_$(host_os)_host=$(host)
host_prefix=$($(host_arch)_$(host_os)_prefix)
build_prefix=$(host_prefix)/native
build_host=$(build)

AT_$(V):=
AT_:=@
AT:=$(AT_$(V))

all: install

include hosts/$(host_os).mk
include hosts/default.mk
include builders/$(build_os).mk
include builders/default.mk
include packages/packages.mk

build_id_string:=$(BUILD_ID_SALT)
build_id_string+=$(shell $(build_CC) --version 2>/dev/null)
build_id_string+=$(shell $(build_AR) --version 2>/dev/null)
build_id_string+=$(shell $(build_CXX) --version 2>/dev/null)
build_id_string+=$(shell $(build_RANLIB) --version 2>/dev/null)
build_id_string+=$(shell $(build_STRIP) --version 2>/dev/null)

$(host_arch)_$(host_os)_id_string:=$(HOST_ID_SALT)
$(host_arch)_$(host_os)_id_string+=$(shell $(host_CC) --version 2>/dev/null)
$(host_arch)_$(host_os)_id_string+=$(shell $(host_AR) --version 2>/dev/null)
$(host_arch)_$(host_os)_id_string+=$(shell $(host_CXX) --version 2>/dev/null)
$