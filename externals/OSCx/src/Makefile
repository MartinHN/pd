#
# Copyright (C) 2004 Hans-Christoph Steiner
#
#    This program is free software; you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation; either version 2 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with this program; if not, write to the Free Software
#    Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.


EXT=o
LIBS = -lc -lm
LIBOSC = ../libOSC/libOSC.a

CFLAGS = -g -arch i386 -O2 -DUNIX -Wall -Wimplicit -Wunused -Wmissing-prototypes -O3  -I../libOSC -I../../pd/src -I../../../pd/src -I../src
INCLUDES = -I../libOSC -I../../pd/src -I../../../pd/src -I../src
LDFLAGS =  -arch i386 -bundle -bundle_loader ../../../pd/bin/pd -flat_namespace -undefined suppress 

prefix=/usr/local

SOURCES = OSC-pattern-match.c OSC.c dumpOSC.c htmsocket.c OSCroute.c sendOSC.c OSC-system-dependent.c
TARGETS = $(SOURCES:.c=.o)
EXTS=sendOSC.pd_darwin dumpOSC.pd_darwin OSCroute.pd_darwin

# ----------------------- autoconf -----------------------

all: $(EXTS)

lib: OSC.pd_darwin

dumpOSC.pd_darwin: dumpOSC.o
	$(CC) $(LDFLAGS) -o dumpOSC.pd_darwin $? $(LIBS)

sendOSC.pd_darwin: sendOSC.o htmsocket.o OSC-system-dependent.o
	$(CC) $(LDFLAGS) -o sendOSC.pd_darwin $? $(LIBS) $(LIBOSC)

OSCroute.pd_darwin: OSCroute.o OSC-pattern-match.o OSC-system-dependent.o
	$(CC) $(LDFLAGS) -o OSCroute.pd_darwin $? $(LIBS)

OSC.pd_darwin: OSC.o sendOSC.o dumpOSC.o OSCroute.o htmsocket.o OSC-pattern-match.o OSC-system-dependent.o
	$(CC) $(LDFLAGS) -o OSC.pd_darwin $? $(LIBS) $(LIBOSC)

$(TARGETS): %.o : %.c
	$(CC) $(CFLAGS) $(INCLUDES) -c -o $*.o $*.c

# ----------------------------------------------------------

install-doc:
	@test -d $(prefix)/lib/pd/doc/5.reference || mkdir -p $(prefix)/lib/pd/doc/5.reference
	cp -r ../doc/* $(prefix)/lib/pd/doc/5.reference/

install: install-doc
	@test -d $(prefix)/lib/pd/extra || mkdir -p $(prefix)/lib/pd/extra
	install -m644 *.pd_darwin $(prefix)/lib/pd/extra

clean:
	rm -rf *.$(EXT) *.pd_darwin
