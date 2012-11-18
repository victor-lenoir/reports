share_dir = .
include make/share.mk
include make/tex.mk

QUIET = @
papersdir = /lrde/doc/lrde/papers
papers_DATA = lrde.pdf lrde-private.pdf README-lrde-bib.txt

dloaddir = /lrde/dload/papers
dload_DATA = lrde.pdf bib/lrde.bib


CLEANFILES = lrde.pdf lrde-private.pdf contents.html contents.txt list.bib \
list.pdf diffs.patch contents.bib contents_bib.html

all: check lrde.pdf

## ------- ##
## Check.  ##
## ------- ##

# Our own *.bib that should obey our rules.
BIBS =							\
  lrde.bib csi.bib olena.bib tiger.bib			\
  gp.bib lang.bib mc.bib parallel.bib rr.bib sip.bib

# We don't use the dependencies here so that all the checks are run.
check:
	$(share_bin_dir)/bibtex-check all

check-perms:
	$(share_bin_dir)/bibtex-check perms

check-csi:
	$(share_bin_dir)/bibtex-check csi

## Normalization.
bibneat = $(share_bin_dir)/bibneat
.PHONY: neat
neat:
	for b in $(BIBS); do \
	  $(bibneat) $(share_bib_dir)/$$b; \
	done; \
	for b in $(BIBS); do \
	  $(share_bin_dir)/bibtex-check $(share_bib_dir)/$$b; \
	done;

## ------------------------------- ##
## lrde.pdf and lrde-private.pdf.  ##
## ------------------------------- ##

%.txt: $(share_bib_dir)/%.bib
	rm -f $@.tmp
	$(QUIET)bibtex2html -q $<
	$(QUIET)html2text -nobs $*.html | \
	  sed -e 's/.*\[ bib .*//;/^==*$$/{s/.*//;q;}' >$@.tmp
	chmod a-w $@.tmp
	mv -f $@.tmp $@

# Create a LaTeX document to force the BibTeX check on lrde.bib.
make_bib = $(share_bin_dir)/make-bib.pl
%.tex: $(share_bib_dir)/%.bib
	perl $(make_bib) $(share_bib_dir)/$*.bib >$*.tmp
	chmod a-w $*.tmp
	mv -f $*.tmp $*.tex

lrde.tex:  $(make_bib)

%-private.tex: $(share_bib_dir)/%.bib
	$(make_bib) --with-submitted $(share_bib_dir)/$*.bib >$*-private.tmp
	chmod a-w $*-private.tmp
	mv -f $*-private.tmp $*-private.tex

lrde-private.tex:  $(make_bib)

view: lrde.pdf
	xpdf lrde.pdf


## ------------------ ##
## Wrappers for svn.  ##
## ------------------ ##

diff:
	bin/svndiff

up svnupdate:
	svn up

install: svnupdate $(papers_DATA) $(dload_DATA)
## The "doc" group rights should allow this.
	install -m 664 $(dload_DATA) $(dloaddir)
	install -m 664 $(papers_DATA) $(papersdir)

ci checkin: diff check
	bin/svnci
	$(MAKE) $(MAKEFLAGS) install


## ----------------------- ##
## Updating from masters.  ##
## ----------------------- ##

.PHONY: update update-bib update-styles

update: update-bib
update-bib:
	bin/update-bib

# Sources.
GASTEX_STYLES = http://www.lsv.ens-cachan.fr/~gastin/gastex
FR_BIB_STYLES = http://www.lsv.ens-cachan.fr/~markey/BibTeX/bst

update: update-styles
update-styles:
	wget $(GASTEX_STYLES)/gastex.sty -O styles/gastex.sty
	wget $(GASTEX_STYLES)/gastex.pro -O styles/gastex.pro
	wget http://igm.univ-mlv.fr/~lombardy/Vaucanson-G/vaucanson-g.tgz
	tar zxvf vaucanson-g.tgz
	rm vaucanson-g/VCManual.ps.gz
	mv vaucanson-g/* styles/
	rm -rf vaucanson-g vaucanson-g.tgz
	wget $(FR_BIB_STYLES)/abbrv-fr.bst -O styles/abbrv-fr.bst
	wget $(FR_BIB_STYLES)/abbrvnat-fr.bst -O styles/abbrvnat-fr.bst
	wget $(FR_BIB_STYLES)/alpha-fr.bst -O styles/alpha-fr.bst
	wget $(FR_BIB_STYLES)/apalike-fr.bst -O styles/apalike-fr.bst
	wget $(FR_BIB_STYLES)/ieeetr-fr.bst -O styles/ieeetr-fr.bst
	wget $(FR_BIB_STYLES)/plain-fr.bst -O styles/plain-fr.bst
	wget $(FR_BIB_STYLES)/plainnat-fr.bst -O styles/plainnat-fr.bst
	wget $(FR_BIB_STYLES)/siam-fr.bst -O styles/siam-fr.bst
	wget $(FR_BIB_STYLES)/unsrt-fr.bst -O styles/unsrt-fr.bst
	wget $(FR_BIB_STYLES)/unsrtnat-fr.bst -O styles/unsrtnat-fr.bst
