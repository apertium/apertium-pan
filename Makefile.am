###############################################################################
## Makefile for apertium-pan
###############################################################################

RELEASE=0.1
VERSION=0.1.0
LANG1=pan
VAR1=Guru
VAR2=Arab
BASENAME=apertium-$(LANG1)

TARGETS_COMMON =			\
	$(LANG1)_$(VAR1).automorf.bin		\
	$(LANG1)_$(VAR1).automorf.att.gz	\
	$(LANG1)_$(VAR1).autogen.bin		\
	$(LANG1)_$(VAR1).autogen.att.gz		\
	$(LANG1)_$(VAR2).automorf.bin		\
	$(LANG1)_$(VAR2).automorf.att.gz	\
	$(LANG1)_$(VAR2).autogen.bin		\
	$(LANG1)_$(VAR2).autogen.att.gz		\
	$(LANG1).rlx.bin

# Use this goal for creating .deps, otherwise make -j2 will give problems:
@ap_include@

###############################################################################
## Punjabi transducer
###############################################################################

## Devanagari script (Gurmukhi)

$(LANG1)_$(VAR1).autogen.bin: $(BASENAME).$(LANG1)_$(VAR1).dix .deps/.d
	apertium-validate-dictionary $<
	lt-comp rl $< $@ $(BASENAME).$(LANG1)_$(VAR1).acx

$(LANG1)_$(VAR1).automorf.bin: $(BASENAME).$(LANG1)_$(VAR1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@ $(BASENAME).$(LANG1)_$(VAR1).acx

$(LANG1)_$(VAR1).autogen.att.gz: $(LANG1)_$(VAR1).autogen.bin
	lt-print $< | gzip -9 -c -n > $@

$(LANG1)_$(VAR1).automorf.att.gz: $(LANG1)_$(VAR1).automorf.bin
	lt-print $< | gzip -9 -c -n > $@

## Arabic script (Shahmukhi)

$(LANG1)_$(VAR2).autogen.bin: $(BASENAME).$(LANG1)_$(VAR2).dix .deps/.d
	apertium-validate-dictionary $<
	lt-comp rl $< $@ $(BASENAME).$(LANG1)_$(VAR2).acx

$(LANG1)_$(VAR2).automorf.bin: $(BASENAME).$(LANG1)_$(VAR2).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@ $(BASENAME).$(LANG1)_$(VAR2).acx

$(LANG1)_$(VAR2).autogen.att.gz: $(LANG1)_$(VAR2).autogen.bin
	lt-print $< | gzip -9 -c -n > $@

$(LANG1)_$(VAR2).automorf.att.gz: $(LANG1)_$(VAR2).automorf.bin
	lt-print $< | gzip -9 -c -n > $@

###############################################################################
## Disambiguation rules
###############################################################################

$(LANG1).rlx.bin: $(BASENAME).$(LANG1).rlx $(CGCOMP)
	$(CGCOMP) $< $@

###############################################################################
## Distribution
###############################################################################
EXTRA_DIST=$(BASENAME).$(LANG1)_$(VAR1).dix		\
		$(BASENAME).$(LANG1)_$(VAR2).dix	\
		$(BASENAME).$(LANG1).acx	\
		$(BASENAME).$(LANG1).rlx	\
		$(BASENAME).post-$(LANG1).dix	\
		$(LANG1)_$(VAR1).prob			\
		$(LANG1)_$(VAR2).prob			\
		modes.xml

###############################################################################
## Installation stuff
###############################################################################
#
#   apertium_pan_dir: This is where the compiled binaries go
#   apertium_pan_srcdir: This is where the source files go

apertium_pandir=$(prefix)/share/apertium/$(BASENAME)/
apertium_pan_srcdir=$(prefix)/share/apertium/$(BASENAME)/

apertium_pan_DATA=$(TARGETS_COMMON) $(LANG1)_$(VAR1).prob $(LANG1)_$(VAR2).prob

pkgconfigdir = $(prefix)/share/pkgconfig
pkgconfig_DATA = $(BASENAME).pc

# Modes are not installed since they're all just "debug modes". All
# modes are created by this goal.
modes/$(LANG1)-morph.mode: modes.xml
	apertium-validate-modes modes.xml
	apertium-gen-modes modes.xml

noinst_DATA=modes/$(LANG1)-morph.mode

install-data-local: install-modes
	test -d $(DESTDIR)$(apertium_pan_srcdir) || mkdir -p $(DESTDIR)$(apertium_pan_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1)_$(VAR1).dix $(DESTDIR)$(apertium_pan_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1)_$(VAR2).dix $(DESTDIR)$(apertium_pan_srcdir)
	$(INSTALL_DATA) $(BASENAME).$(LANG1).rlx $(DESTDIR)$(apertium_pan_srcdir)

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "TODO test sentence" | apertium -d . pan-morph | tee .test-mt
	@echo "TODO test analysis results" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt

