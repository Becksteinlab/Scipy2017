NAME := manuscript
TEX  := $(NAME).tex
PDF  := $(NAME).pdf

# comment for proper debugging
NONSTOP := 1

PDFLATEX := pdflatex 
BIBTEX   := bibtex

# bibtex breaks long urls and inserts a % which does show up in the
# url; $$ to quote the $ in Make
BIB_FIXURL := perl -wpi~ -e 's(%$$)()' 

# macros for $(call macro,arg1,...)
ifdef NONSTOP
run_pdflatex = $(PDFLATEX) '\nonstopmode\input{$1}'
warn_NONSTOP = { echo "*** WARNING: Errors while running in NONSTOP mode"; \
                 egrep -i 'error|warning' $*.log; true; }
else
run_pdflatex = $(PDFLATEX) $1
warn_NONSTOP := { echo "*** ERROR while running LaTeX"; false; }
endif

ifndef SUBMISSION
SUBMISSION := 0
endif

ifeq ($(SUBMISSION),0)
# generate bbl from file
%.pdf : %.tex
	$(call run_pdflatex,$<)  ;  \
	$(BIBTEX) $*;            \
	$(BIB_FIXURL) $*.bbl  ;  \
	$(call run_pdflatex,$<)  ;  \
	$(BIBTEX) $*;            \
	$(BIB_FIXURL) $*.bbl  ;  \
	$(call run_pdflatex,$<)  ;  \
	$(call run_pdflatex,$<)  \
	|| $(warn_NONSTOP)
else
%.pdf : %.tex
	$(call run_pdflatex,$<)  ;  \
	$(call run_pdflatex,$<)  ;  \
	$(call run_pdflatex,$<)  \
	|| $(warn_NONSTOP)
endif

%.bbl : %.tex  # actually it wants .aux
	$(BIBTEX) $*;            \
	$(BIB_FIXURL) $*.bbl


.phony: all clean see

all: $(PDF)

clean:
	rm *.aux *.log *.pdf *.blg *.bbl

see: $(PDF)
	open $^
