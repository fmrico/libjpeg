#
#  Automake Makefile for the JPEG library
#
#  This file is written by Bob Friesenhahn, bfriesen@simple.dallas.tx.us
#

# Don't require all the GNU mandated files
AUTOMAKE_OPTIONS = 1.9 dist-zip dist-bzip2 foreign 

# Sources to build library
LIBSOURCES = jcapimin.c jcapistd.c jccoefct.c jccolor.c jcdctmgr.c jchuff.c \
        jcinit.c jcmainct.c jcmarker.c jcmaster.c jcomapi.c jcparam.c \
        jcphuff.c jcprepct.c jcsample.c jctrans.c jdapimin.c jdapistd.c \
        jdatadst.c jdatasrc.c jdcoefct.c jdcolor.c jddctmgr.c jdhuff.c \
        jdinput.c jdmainct.c jdmarker.c jdmaster.c jdmerge.c jdphuff.c \
        jdpostct.c jdsample.c jdtrans.c jerror.c jfdctflt.c jfdctfst.c \
        jfdctint.c jidctflt.c jidctfst.c jidctint.c jidctred.c jquant1.c \
        jquant2.c jutils.c jmemmgr.c @MEMORYMGR@.c

# System dependent sources
SYSDEPSOURCES = jmemansi.c jmemname.c jmemnobs.c jmemdos.c jmemmac.c

# Headers which are installed to support the library
INSTINCLUDES  = jerror.h jmorecfg.h jpeglib.h

# Headers which are not installed
OTHERINCLUDES = cderror.h cdjpeg.h jchuff.h jdct.h jdhuff.h jinclude.h \
        jmemsys.h jpegint.h jversion.h transupp.h

# Manual pages (Automake uses 'MANS' for itself)
DISTMANS= cjpeg.1 djpeg.1 jpegtran.1 rdjpgcom.1 wrjpgcom.1

# Other documentation files
DOCS= README install.doc usage.doc wizard.doc example.c libjpeg.doc \
	structure.doc coderules.doc filelist.doc change.log

# Makefiles for various systems
MKFILES= configure makefile.cfg makefile.ansi makefile.unix makefile.bcc \
        makefile.mc6 makefile.dj makefile.wat makefile.vc makelib.ds \
        makeapps.ds makeproj.mac makcjpeg.st makdjpeg.st makljpeg.st \
        maktjpeg.st makefile.manx makefile.sas makefile.mms makefile.vms \
        makvms.opt

# Configuration files
CONFIGFILES= jconfig.cfg jconfig.bcc jconfig.h.in jconfig.mc6 jconfig.dj \
	jconfig.wat jconfig.vc jconfig.mac jconfig.st jconfig.manx \
	jconfig.sas jconfig.vms

# Miscellaneous support files
OTHERFILES= README.1st jconfig.doc ckconfig.c ansi2knr.c ansi2knr.1 \
	jmemdosa.asm

# Test support files
TESTFILES= testorig.jpg testimg.ppm testimg.bmp testimg.jpg testprog.jpg \
	testimgp.jpg

# libtool libraries to build
lib_LTLIBRARIES = libjpeg.la

# Library sources for libjpeg.la
libjpeg_la_SOURCES = $(LIBSOURCES)

# LDFLAGS for libjpeg.la
libjpeg_la_LDFLAGS = -no-undefined \
	-version-info $(JPEG_LIB_VERSION)

# Conditionally-build library sources for libjpeg.la
EXTRA_libjpeg_la_SOURCES = $(SYSDEPSOURCES)

# Executables to build
bin_PROGRAMS = cjpeg djpeg rdjpgcom wrjpgcom jpegtran

# Executable sources & libs
cjpeg_SOURCES    = cjpeg.c rdppm.c rdgif.c rdtarga.c rdrle.c rdbmp.c rdswitch.c cdjpeg.c
cjpeg_LDADD      = libjpeg.la $(PNGLIBS)
djpeg_SOURCES    = djpeg.c wrppm.c wrgif.c wrtarga.c wrrle.c wrbmp.c rdcolmap.c cdjpeg.c 
djpeg_LDADD      = libjpeg.la $(PNGLIBS)
jpegtran_SOURCES = jpegtran.c rdswitch.c cdjpeg.c transupp.c
jpegtran_LDADD   = libjpeg.la
rdjpgcom_SOURCES = rdjpgcom.c
wrjpgcom_SOURCES = wrjpgcom.c

# Manual pages to install
man_MANS = $(DISTMANS)

# Headers to install
include_HEADERS = $(INSTINCLUDES)

# Other distributed headers
noinst_HEADERS = $(OTHERINCLUDES)

# Other distributed files
EXTRA_DIST =  $(DOCS) $(DISTMANS) $(MKFILES) $(CONFIGFILES) $(SYSDEPSOURCES) \
        $(OTHERFILES) $(TESTFILES)

# Files to be cleaned
CLEANFILES = testout.ppm testout.bmp testout.jpg testoutp.ppm testoutp.jpg testoutt.jpg

# Install jconfig_api.h
install-data-local:
	$(mkinstalldirs) $(DESTDIR)$(includedir)
	$(INSTALL_HEADER) jconfig_api.h $(DESTDIR)$(includedir)/jconfig.h

# Uninstall jconfig_api.h
uninstall-local:
	rm -f $(DESTDIR)$(includedir)/jconfig.h

# Non-Automake subdirectories to distribute
DISTDIRS = m4 config
dist-hook:
	( \
	  builddir=`pwd` ; \
	  cd $(srcdir) && \
	  ( \
	    for dir in $(DISTDIRS) ; do \
	      find $$dir -depth -print | egrep -v '(~$$)|(/CVS)|(/\.#)|(/\.deps)' \
	        | cpio -pdum $$builddir/$(distdir) 2> /dev/null ; \
	    done \
	  ) \
	)

# Run tests
test: check-local
check-local:
	$(RM) testout*
	./djpeg -dct int -ppm -outfile testout.ppm  $(srcdir)/testorig.jpg
	./djpeg -dct int -bmp -colors 256 -outfile testout.bmp  $(srcdir)/testorig.jpg
	./cjpeg -dct int -outfile testout.jpg  $(srcdir)/testimg.ppm
	./djpeg -dct int -ppm -outfile testoutp.ppm $(srcdir)/testprog.jpg
	./cjpeg -dct int -progressive -opt -outfile testoutp.jpg $(srcdir)/testimg.ppm
	./jpegtran -outfile testoutt.jpg $(srcdir)/testprog.jpg
	cmp $(srcdir)/testimg.ppm testout.ppm
	cmp $(srcdir)/testimg.bmp testout.bmp
	cmp $(srcdir)/testimg.jpg testout.jpg
	cmp $(srcdir)/testimg.ppm testoutp.ppm
	cmp $(srcdir)/testimgp.jpg testoutp.jpg
	cmp $(srcdir)/testorig.jpg testoutt.jpg

