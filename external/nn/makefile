SHELL = /bin/sh

prefix = /usr/local
exec_prefix = ${prefix}
bindir = ${exec_prefix}/bin
libdir = /usr/local/lib
includedir = ${prefix}/include

INSTALLDIRS =\
$(bindir)\
$(libdir)\
$(includedir)

DISTDIR = ~/pub_web

INSTALL = /usr/bin/install -c
INSTALL_PROGRAM = ${INSTALL}
INSTALL_DATA = ${INSTALL} -m 644

CC = gcc
CFLAGS = -g -O2 -Wall -pedantic
CFLAGS_TRIANGLE = -g -O2 -Wall -pedantic -w -ffloat-store
CFLAGS_VULNERABLE = -fno-force-mem -ffloat-store
LDFLAGS = 

AR = ar
ARFLAGS = cru

MLIB = -lm

LIBSRC =\
delaunay.c\
hash.c\
istack.c\
lpi.c\
minell.c\
nnai.c\
nnpi.c\
nncommon.c\
nncommon-vulnerable.c\
preader.c

UTILSRC=\
nnbathy.c

HEADERS=\
delaunay.h\
hash.h\
istack.h\
minell.h\
nan.h\
nn.h\
nn_internal.h\
preader.h\
version.h

PROGRAMS =\
minell\
nnbathy

TESTS=\
nnai_test\
nnphi_test\
ht_test

LIBOBJS = $(LIBSRC:.c=.o)

all: libnn.a $(PROGRAMS)

minell: minell.c
	$(CC) -o minell minell.c -DME_STANDALONE $(CFLAGS) -I. $(LDFLAGS) $(MLIB)

nnbathy: libnn.a nnbathy.c
	$(CC) -o nnbathy nnbathy.c $(CFLAGS) -I. $(LDFLAGS) libnn.a $(MLIB)

standalone: override LDFLAGS+=-static
standalone: $(PROGRAMS)
	strip $(PROGRAMS)

libnn.a: triangle.o $(LIBOBJS)
	$(AR) $(ARFLAGS) libnn.a $(LIBOBJS) triangle.o
	chmod go+r libnn.a

nncommon-vulnerable.o: override CFLAGS+=$(CFLAGS_VULNERABLE)

triangle.o: triangle.c
	$(CC) -c -DTRILIBRARY $(CFLAGS_TRIANGLE) -I. triangle.c

tests: libnn.a $(TESTS)

nnai_test:
	$(CC) -o nnai_test nnai.c -DNNAI_TEST $(CFLAGS) -I. $(MLIB) libnn.a

nnphi_test:
	$(CC) -o nnphi_test nnpi.c -DNNPHI_TEST $(CFLAGS) -I. $(MLIB) libnn.a

ht_test:
	$(CC) -o ht_test hash.c -DHT_TEST $(CFLAGS) -I. $(MLIB)

.c.o:
	$(CC) $(CFLAGS) -c $*.c

installdirs:
	$(SHELL) mkinstalldirs $(INSTALLDIRS)

install: all installdirs
	for i in libnn.a; do \
	   $(INSTALL_DATA) $$i $(libdir)/$$i; \
	done

	for i in nn.h; do \
	   $(INSTALL_DATA) $$i $(includedir); \
	done

	for i in $(PROGRAMS); do \
	   fname=`basename $$i`; \
	   $(INSTALL_PROGRAM) $$i $(bindir); \
	done

clean:
	rm -f *.o libnn.a $(PROGRAMS) $(TESTS) *~ core

configclean:
	rm -f config.h makefile config.cache config.status config.log

ex1clean:
	cd examples/1; make clean;

ex2clean:
	cd examples/2; make clean;

ex3clean:
	cd examples/3; make clean;

ex4clean:
	cd examples/4; make clean;

ex5clean:
	cd examples/5; make clean;

distclean: clean configclean ex1clean ex2clean ex3clean ex4clean ex5clean

indent:
	indent -T FILE -T NN_ALGORITHM -T point -T delaunay -T lpi -T nnpi -T nnhpi -T indexedpoint -T nnai -T ht_bucket -T hashtable -T istack -T triangle -T triangle_neighbours -T circle -T nn_weights -T lweights -T minell $(LIBSRC) $(UTILSRC) $(HEADERS)
	rm -f *~

dist:
	WDIR=`pwd`;\
	DIR=`basename $$WDIR`;\
	cp -r $$WDIR /tmp;\
	cd /tmp/$$DIR/examples/1;\
	make clean;\
	rm -f  example.pdf;\
	cd /tmp/$$DIR/examples/2;\
	make clean;\
	rm -f  example.pdf;\
	cd /tmp/$$DIR/examples/3;\
	make clean;\
	cd /tmp/$$DIR;\
	rm -rf `find /tmp/$$DIR -name CVS`;\
	if [ -e makefile ] ; then\
		make distclean;\
	fi;\
	/tmp/"$$DIR"/configure;\
	make standalone;\
	chmod a+r+x $(PROGRAMS);\
	mv -f $(PROGRAMS) $(DISTDIR);\
	make distclean;\
	cd $(DISTDIR);\
	mv -f $$DIR".tar.gz" $$DIR".tar.gz.prev";\
	mv -f $$DIR"-noexamples.tar.gz" $$DIR"-noexamples.tar.gz.prev";\
	gzip -f $(PROGRAMS);\
	cd /tmp;\
	tar czvf $$DIR".tar.gz" $$DIR;\
	chmod a+r $$DIR".tar.gz";\
	mv -f $$DIR".tar.gz" $(DISTDIR)/$$DIR".tar.gz";\
	tar czvf $$DIR"-noexamples.tar.gz" --exclude=$$DIR/examples $$DIR;\
	chmod a+r $$DIR"-noexamples.tar.gz";\
	mv -f $$DIR"-noexamples.tar.gz" $(DISTDIR)/$$DIR"-noexamples.tar.gz";\
	rm -rf $$DIR
