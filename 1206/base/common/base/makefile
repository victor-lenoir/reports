### Makefile --- Generic Toplevel Makefile for LRDE's technical reports

## Copyright (C) 2002, 2004 LRDE

## SVN: $Id: makefile 430 2004-09-13 09:15:21Z didier $

## Author:        Didier Verna <didier@lrde.epita.fr>
## Maintainer:    Didier Verna <didier@lrde.epita.fr>
## Created:       Tue Jun 18 16:50:48 2002 under XEmacs 21.5 (beta 5)
## Last Revision: Wed Sep  8 16:29:07 2004


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

REPORT := $(shell basename `pwd`)

EXCLUDE := \*~ .auto\* .svn\* base\*

LINKS := COPYING COPYING.DOC README techrep.cls

all: check_links
	$(MAKE) gen TARGET=all

check_links:
	for i in $(LINKS) ; do                      \
          test -e $$i || ln -s base/common/base/$$i ; \
        done

## Shortcuts to specific commands ===========================================

ps: check_links
	$(MAKE) format_gen TARGET=ps FORMAT=ps

dvi: check_links
	$(MAKE) format_gen TARGET=dvi FORMAT=ps

pdf: check_links
	$(MAKE) format_gen TARGET=pdf FORMAT=pdf

psview: check_links
	$(MAKE) format_gen TARGET=psview FORMAT=ps

dviview: check_links
	$(MAKE) format_gen TARGET=dviview FORMAT=ps

pdfview: check_links
	$(MAKE) format_gen TARGET=pdfview FORMAT=pdf


## Distribution =============================================================

dist: check_links
	-rm -f $(REPORT).tar $(REPORT).tar.*
	echo > ../__exclude
	for i in $(EXCLUDE) ; do     \
          echo $(REPORT)/$$i >> ../__exclude ; \
	done
	$(MAKE) gen TARGET=exclude
	cd ..                                                      \
          && tar chvf $(REPORT).tar --exclude-from=__exclude $(REPORT) \
          && mv $(REPORT).tar $(REPORT)                                \
	  && rm -f __exclude
	bzip2 $(REPORT).tar


## Dispatch to the PS and PDF subdirs =======================================

gen:
	$(MAKE) format_gen TARGET=$(TARGET) FORMAT=ps
	$(MAKE) format_gen TARGET=$(TARGET) FORMAT=pdf

format_gen:
	test -d $(FORMAT) || ( mkdir $(FORMAT) && chmod 755 $(FORMAT) ) ; \
	( cd $(FORMAT) ;                                                  \
          test -e Makefile                                                \
            || ln -s ../base/common/base/$(FORMAT)/makefile Makefile ;    \
            $(MAKE) $(TARGET) )

.DEFAULT:
	$(MAKE) gen TARGET=$@

.PHONY: all check_links \
        ps dvi pdf psview dviview pdfview \
        dist gen format_gen

### Makefile ends here
