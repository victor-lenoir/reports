### Makefile --- Generic ps/ Makefile for LRDE's technical reports

## Copyright (C) 2002, 2003, 2004 LRDE.

## SVN: $Id: makefile 2133 2009-10-16 15:43:54Z ordy $

## Author:        Didier Verna <didier@lrde.epita.fr>
## Maintainer:    Didier Verna <didier@lrde.epita.fr>
## Created:       Tue Jun 18 17:00:01 2002 under XEmacs 21.5 (beta 5)
## Last Revision: Wed Sep  8 16:32:17 2004


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

## You might want to twiddle these:
DVIVIEW := xdvi
PSVIEW  := gv
GRAPHICS_DIR := ..
TEXI2DVI := texi2dvi

# put this here in case the inc files contain targets.
all: ps

# User options can go there
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
GEN_EPS_FILES := $(patsubst %.fig, %.eps, $(notdir $(FIG_FILES))) \
	         $(patsubst %.dia, %.eps, $(notdir $(DIA_FILES))) \
	         $(patsubst %.svg, %.eps, $(notdir $(SVG_FILES))) \
	         $(patsubst %.jpg, %.eps, $(notdir $(JPG_FILES))) \
	         $(patsubst %.png, %.eps, $(notdir $(PNG_FILES))) \
	         $(patsubst %.pdf, %.eps, $(notdir $(PDF_FILES)))

EXCLUDE := .auto\* .svn\*
CLEAN_FILES := *.aux *.toc *.log *.bbl *blg *.lot *.out *~
DISTCLEAN_FILES := $(GEN_EPS_FILES) $(REPORT).ps $(REPORT).dvi

LINKS := logo_lrde.eps

DEPS := ../techrep.cls $(LINKS) $(TEX_FILES) $(BIB_FILES) \
        $(EPS_FILES) $(GEN_EPS_FILES)

# Flags
TEXI2DVI_FLAGS := --batch -I ..:$(GRAPHICS_DIR):$(TEXINPUTS) -~

ps: $(REPORT).ps

dvi: $(REPORT).dvi

psview: $(REPORT).ps
	$(PSVIEW) $<

dviview: $(REPORT).dvi
	$(DVIVIEW) $<

$(REPORT).ps: $(REPORT).dvi

$(REPORT).dvi: $(DEPS)
	BIBINPUTS=..:$(BIBINPUTS) $(TEXI2DVI) $(TEXI2DVI_FLAGS) ../$(REPORT).tex

$(LINKS):
	ln -s ../base/common/base/ps/$@

%.ps: %.dvi
	dvips -o $@ $<

clean:
	-rm $(CLEAN_FILES)

distclean: clean
	-rm $(DISTCLEAN_FILES)

exclude:
	for i in $(EXCLUDE) $(CLEAN_FILES) $(DISTCLEAN_FILES) ; do \
	  echo $(REPORT)/ps/$$i >> ../../__exclude ;    \
	done

%.eps: $(GRAPHICS_DIR)/%.fig
	fig2dev -L eps -p dummy -c $< $@

%.eps: $(GRAPHICS_DIR)/%.dia
	dia --nosplash --export=$@ $<

%.eps: $(GRAPHICS_DIR)/%.svg
	convert $< $@

%.eps: $(GRAPHICS_DIR)/%.jpg
	convert $< $@

%.eps: $(GRAPHICS_DIR)/%.pdf
	convert $< $@

%.eps: $(GRAPHICS_DIR)/%.png
	convert $< $@

.PHONY: all ps dvi psview dviview clean distclean exclude
.SUFFIXES: .tex .fig .dia .svg .jpg .eps .dvi .ps

### Makefile ends here
