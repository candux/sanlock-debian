TARGET1 = fence_sanlockd
TARGET2 = fence_sanlock

SOURCE1 = fence_sanlockd.c
SOURCE2 = fence_sanlock.in

CFLAGS += -D_GNU_SOURCE -g \
	-Wall \
	-Wformat \
	-Wformat-security \
	-Wnested-externs \
	-Wpointer-arith \
	-Wextra -Wshadow \
	-Wcast-align \
	-Wwrite-strings \
	-Waggregate-return \
	-Wstrict-prototypes \
	-Winline \
	-Wredundant-decls \
	-Wno-sign-compare \
	-Wp,-D_FORTIFY_SOURCE=2 \
	-fexceptions \
	-fasynchronous-unwind-tables \
	-fdiagnostics-show-option

VER=$(shell cat ../VERSION)
CFLAGS += -DVERSION=\"$(VER)\" -I../src -I../wdmd

LDFLAGS = -lrt -laio -lblkid -lsanlock -lwdmd

all: $(TARGET1) $(TARGET2)

$(TARGET1): $(SOURCE1)
	$(CC) $(CFLAGS) $(LDFLAGS) $(SOURCE1) -o $@ -L. -L../src -L../wdmd

$(TARGET2): $(SOURCE2)
	cat $(SOURCE2) | sed \
		-e 's#@VERSION@#${VER}#g' \
	> $(TARGET2)
	chmod 755 $(TARGET2)

clean:
	rm -f *.o *.so *.so.* $(TARGET1) $(TARGET2)

INSTALL=$(shell which install)

DESTDIR=
BINDIR=/usr/sbin
LIBDIR=/usr/lib64
HEADIR=/usr/include
MANDIR=/usr/share/man

.PHONY: install
install: all
	$(INSTALL) -d $(DESTDIR)/$(BINDIR)
	$(INSTALL) -d $(DESTDIR)/$(MANDIR)/man8
	$(INSTALL) -c -m 755 $(TARGET1) $(TARGET2) $(DESTDIR)/$(BINDIR)
	$(INSTALL) -m 644 fence_sanlock.8 $(DESTDIR)/$(MANDIR)/man8/
	$(INSTALL) -m 644 fence_sanlockd.8 $(DESTDIR)/$(MANDIR)/man8/
