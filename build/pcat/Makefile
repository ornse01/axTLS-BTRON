#
#	@(#)Makefile (libaxtls)
#		GNU make 用
#
#	make の方法
#	・make
#		リリース用の正式版を作成
#	・make mode=debug
#		デバッグ用に作成
#	・make clean
#		make で作成した全てのファイルを削除
#	・make install
#		規定の場所にインストール (実機用のみ)
#

# バージョン
version = 0x0100

# リリース用をデフォルトにする
#mode =

# マルチタスク対応ライブラリを使用
USE_MT = 1

# C++ アプリケーションライブラリを使用
USE_CPP = 1

# ソースの依存関係ファイル (自動生成)
DEPS = Dependencies
DEPENDENCIES_OUTPUT := $(DEPS)

# アプリケーション標準ルール
#include ../../etc/makerules
include $(BD)/lib/etc/makerules

# ターゲット
ifeq ($(MACHINE), sun4)
  btron = no
else
  btron = yes
endif

# ----------------------------------------------------------------------------
# 作成対象
TARGET = libaxtls.a

# ソースファイルのサーチパス
S = ../..
VPATH = $(S)/ssl $(S)/crypto
SRC = 

# ヘッダファイルのディレクトリ追加
HEADER := $(S)/config $(S)/ssl $(S)/crypto $(HEADER)

DATABOX_HEADER := $(S)

# ソースファイル (crypto)
SRC += 	aes.c \
	bigint.c \
	crypto_misc.c \
	hmac.c \
	md5.c \
	rc4.c \
	rsa.c \
	sha1.c \
	sha256.c \
	sha384.c \
	sha512.c

# ソースファイル (ssl)
SRC += 	asn1.c \
	gen_cert.c \
	loader.c \
	openssl.c \
	os_port.c \
	p12.c \
	tls1.c \
	tls1_svr.c \
	tls1_clnt.c \
	x509.c

EXT = .out
CFLAGS += $(CFLAGS_WARING) -Wno-uninitialized
DATABOX_HEADER += $(BD)/include
#LOADLIBES += -lstlport

OBJ = $(addsuffix .o, $(basename $(SRC)))

WC_SRC = $(filter %.C, $(SRC))
WC_HEAD = tcsliteral.H

#ifneq ($(mode), debug)
#  CFLAGS += -W -Wall
#endif

ifeq ($(MACHINE), sun4)
  ifeq ($(mode), debug)
    CFLAGS += -Wno-unused
  endif
  LDLIBS = -lg -llang -ldbg
  LDOBJS =
endif

# ----------------------------------------------------------------------------
.PHONY: all clean install

all: $(TARGET)

$(TARGET): $(OBJ)
	$(AR) r $(TARGET) $(OBJ)

clean:
	$(RM) $(TARGET) $(OBJ)
	$(RM) $(WC_SRC:%.C=%.c) $(DBSRC)
	$(RM) $(DEPS)

install: $(TARGET:%=$(LIB_INSTALLDIR)/%)
	@mkdir -p -m 755 $(BD)/include/axTLS
	install -m 644 $(S)/crypto/*.h $(BD)/include/axTLS
	install -m 644 $(S)/ssl/*.h $(BD)/include/axTLS
	-rm $(BD)/include/axTLS/cert.h
	-rm $(BD)/include/axTLS/private_key.h
	-rm $(BD)/include/axTLS/os_port.h
	install -m 644 $(S)/config/config.h $(BD)/include/axTLS

# ソースの依存関係

$(WC_SRC:%.C=%.c):

ifdef DEPENDENCIES_OUTPUT
$(DEPS):	; touch $(DEPS)
else
$(DEPS): $(SRC)	; $(MAKEDEPS) $@ $?
endif

include $(DEPS)
