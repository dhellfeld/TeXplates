# LaTeX Thesis Makefile
# D. Hellfeld 1.20.18

# TODO
# - test references in appendices
# - comment/document
#

MAIN = thesis

LATEXENGINE = pdflatex
BIBTEXENGINE = biber

FMATTER = title approval copyright abstract dedication indexing acknowledgements
FMATTERDIR = FrontMatter

CHAPTERS = ch1 ch2
CHAPTERDIR = Chapters

APPENDICES = appendixA appendixB
APPENDICESDIR = Appendices

OUTLINE = outline
OUTLINEDIR = Outline

BUILDDIR = _build

G := $(shell tput -Txterm setaf 2)
Y := $(shell tput -Txterm setaf 3)
W := $(shell tput -Txterm setaf 7)

.PHONY: initialize $(FMATTER) $(CHAPTERS) $(APPENDICES) $(OUTLINE) all clean help

$(MAIN): initialize
	$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(MAIN).tex
	$(BIBTEXENGINE) $(BUILDDIR)/$(MAIN)
	$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(MAIN).tex
	@open $(BUILDDIR)/$(MAIN).pdf

$(FMATTER): %: initialize
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(FMATTERDIR)/$*/$*}\input{$(MAIN)}"
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(FMATTERDIR)/$*/$(BUILDDIR)
	@open $(BUILDDIR)/$(FMATTERDIR)/$*/$(BUILDDIR)/$*.pdf

$(CHAPTERS): %: % initialize
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(CHAPTERDIR)/$*/$*}\input{$(MAIN)}"
	$(BIBTEXENGINE) $(BUILDDIR)/$*
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(CHAPTERDIR)/$*/$*}\input{$(MAIN)}"
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(CHAPTERDIR)/$*/$(BUILDDIR)
	@open $(BUILDDIR)/$(CHAPTERDIR)/$*/$(BUILDDIR)/$*.pdf

$(APPENDICES): %: % initialize
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(APPENDICESDIR)/$*/$*}\input{$(MAIN)}"
	$(BIBTEXENGINE) $(BUILDDIR)/$*
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(APPENDICESDIR)/$*/$*}\input{$(MAIN)}"
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(APPENDICESDIR)/$*/$(BUILDDIR)
	@open $(BUILDDIR)/$(APPENDICESDIR)/$*/$(BUILDDIR)/$*.pdf

$(OUTLINE): %: initialize
	$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(OUTLINEDIR)/$*.tex
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(OUTLINEDIR)
	@open $(BUILDDIR)/$(OUTLINEDIR)/$@.pdf

all: initialize $(FMATTER) $(CHAPTERS) $(APPENDICES) $(MAIN)

initialize:
	@mkdir -p $(BUILDDIR)
	@mkdir -p $(foreach section,  $(FMATTER),    $(BUILDDIR)/$(FMATTERDIR)/$(section)/$(BUILDDIR))
	@mkdir -p $(foreach chapter,  $(CHAPTERS),   $(BUILDDIR)/$(CHAPTERDIR)/$(chapter)/$(BUILDDIR))
	@mkdir -p $(foreach appendix, $(APPENDICES), $(BUILDDIR)/$(APPENDICESDIR)/$(appendix)/$(BUILDDIR))
	@mkdir -p $(foreach section,  $(OUTLINE),    $(BUILDDIR)/$(OUTLINEDIR))

clean:  
	@echo "Cleaning up..."
	@rm -rf $(BUILDDIR)

help:
	@echo ''
	@echo '${W}Usage:'
	@echo '  ${Y}make ${G}<target>'
	@echo ''
	@echo '${W}Targets:'
	@echo '  ${Y}thesis           ${G} - Build the entire document'
	@echo '  ${Y}initialize       ${G} - Build and dislpay individual sections and entire document'
	@echo '  ${Y}title            ${G} - Build and display title page'
	@echo '  ${Y}abstract         ${G} - Build and display the abstract'
	@echo '  ${Y}acknowledgements ${G} - Build and display the acknowledgements'
	@echo '  ${Y}outline          ${G} - Build and display the outline'
	@echo '  ${Y}copyright        ${G} - Build and display the copyright'
	@echo '  ${Y}dedication       ${G} - Build and display the dedication'
	@echo '  ${Y}approval         ${G} - Build and display the approval'
	@echo '  ${Y}<chapter>        ${G} - Build and display individual chapters'
	@echo '  ${Y}<appendix>       ${G} - Build and display individual appendices'
	@echo '  ${Y}all              ${G} - Build and display all sections and the entire document'
	@echo '  ${Y}clean            ${G} - Remove all build directories'
	@echo '  ${Y}help             ${G} - Display this help'
	@echo ''
