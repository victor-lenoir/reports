### Makefile --- Generic pdf/ Makefile for LRDE's technical reports

## Copyright (C) 2002, 2003, 2004 LRDE

## SVN: $Id: makefile 2249 2011-06-08 09:54:28Z senta $

## Author:        Didier Verna <didier@lrde.epita.fr>
## Maintainer:    Didier Verna <didier@lrde.epita.fr>
## Created:       Tue Jun 18 17:27:20 2002 under XEmacs 21.5 (beta 5)
## Last Revision: Wed Sep  8 16:31:55 2004


## This Makefile is free software; you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation; either version 2 of the License, or
## (at your option) any later version.

## This Makefile is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.

## You should have received a copy of the GNU General Public License
## along with this program; if not, write to the Free Software
## Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.


### Commentary:

## Contents management by FCM version 0.1-b2.


### Code:

## You might want to twiddle this:
PDFVIEW := xpdf
GRAPHICS_DIR := ..
TEXI2DVI := texi2dvi

# put this here in case the inc files contain targets.
all: pdf

# User redefinitions can go there
-include ../makefile.inc
-include makefile.inc

REPORT := $(shell basename `cd .. && pwd`)

# Text files
TEX_FILES := $(shell find .. -name '*'.tex)
BIB_FILES := $(shell find .. -name '*'.bib)

# Graphics
FIG_FILES := $(wildcard $(GRAPHICS_DIR)/*.fig)
DIA_FILES := $(wildcard $(GRAPHICS_DIR)/*.dia)
SVG_FILES := $(wildcard $(GRAPHICS_DIR)/*.svg)
JPG_FILES := $(wildcard $(GRAPHICS_DIR)/*.jpg)
EPS_FILES := $(wildcard $(GRAPHICS_DIR)/*.eps)
PDF_FILES := $(wildcard $(GRAPHICS_DIR)/*.pdf)
PNG_FILES := $(wildcard $(GRAPHICS_DIR)/*.png)

# Generated graphics
GEN_PDF_FILES := $(patsubst %.fig, %.pdf, $(notdir $(FIG_FILES))) \
	         $(patsubst %.dia, %.pdf, $(notdir $(DIA_FILES))) \
	         $(patsubst %.svg, %.pdf, $(notdir $(SVG_FILES))) \
	         $(patsubst %.eps, %.pdf, $(notdir $(EPS_FILES)))

GEN_PNG_FILES := $(patsubst %.jpg, %.png, $(notdir $(JPG_FILES)))

EXCLUDE := .auto\* .svn\*
CLEAN_FILES := *.aux *.toc *.log *.bbl *blg *.lot *.out *~
DISTCLEAN_FILES := $(GEN_PDF_FILES) $(GEN_PNG_FILES) $(REPORT).pdf

LINKS := logo_lrde.png

DEPS := ../techrep.cls $(LINKS) $(TEX_FILES) $(BIB_FILES) \
        $(PDF_FILES) $(GEN_PDF_FILES) $(PNG_FILES) $(GEN_PNG_FILES)

# Flags
TEXI2DVI_FLAGS := --batch --pdf -I ..:$(GRAPHICS_DIR):$(TEXINPUTS) -~

pdf: $(REPORT).pdf

pdfview: $(REPORT).pdf
	$(PDFVIEW) $<

# WARNING: TEXPSHEADER is needed for older versions of pdftex.
$(REPORT).pdf: $(DEPS)
	TEXPSHEADERS=..:$(GRAPHICS_DIR):${TEXPSHEADERS} \
        BIBINPUTS=..:$(BIBINPUTS) \
	$(TEXI2DVI) $(TEXI2DVI_FLAGS) ../$(REPORT).tex

clean:
	-rm $(CLEAN_FILES)

distclean: clean
	-rm $(DISTCLEAN_FILES)

exclude:
	for i in $(EXCLUDE) $(CLEAN_FILES) $(DISTCLEAN_FILES) ; do \
	  echo $(REPORT)/pdf/$$i >> ../../__exclude ;    \
	done

$(LINKS):
	ln -s ../base/common/base/pdf/$@

%.pdf: $(GRAPHICS_DIR)/%.fig
	fig2dev -L pdf -p dummy -c $< $@

%.pdf: $(GRAPHICS_DIR)/%.dia
	dia --nosplash --export=__tmp.eps $<
	epstopdf __tmp.eps
	mv __tmp.pdf $@
	rm __tmp.eps

%.pdf: $(GRAPHICS_DIR)/%.eps
	epstopdf --outfile=$@ $<

%.pdf: $(GRAPHICS_DIR)/%.svg
	inkscape $< --export-pdf=$@

%.png: $(GRAPHICS_DIR)/%.jpg
	convert $< $@

.PHONY: all pdf pdfview clean distclean exclude
.SUFFIXES: .tex .fig .dia .svg .jpg .pdf .png

### Makefile ends here
