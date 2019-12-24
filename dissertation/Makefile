MAIN = thesis

LATEXENGINE = pdflatex
BIBTEXENGINE = biber
GLOSSARYENGINE = makeglossaries
BIB = 1

FMATTERDIR = frontmatter
FMATTER := $(shell find $(FMATTERDIR) -type f -name "*.tex" -exec basename {} .tex ";")

CHAPTERDIR = chapters
CHAPTERS := $(shell find $(CHAPTERDIR) -maxdepth 1 -mindepth 1 -type d -execdir echo {} "+")

APPENDICESDIR = appendices
APPENDICES := $(shell find $(APPENDICESDIR) -maxdepth 1 -mindepth 1 -type d -execdir echo {} "+")

BUILDDIR = build
PDFDIR = pdfs

G := $(shell tput -Txterm setaf 2)
Y := $(shell tput -Txterm setaf 3)
W := $(shell tput -Txterm setaf 7)

.PHONY: initialize $(FMATTER) $(CHAPTERS) $(APPENDICES) $(OUTLINE) all clean help

$(MAIN): initialize
	$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(MAIN).tex
	$(GLOSSARYENGINE) -d $(BUILDDIR) $(MAIN)
	$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(MAIN).tex
	@if [ $(BIB) = 1 ]; then\
		$(BIBTEXENGINE) $(BUILDDIR)/$(MAIN);\
		$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(MAIN).tex;\
	fi
	@mv $(BUILDDIR)/$(MAIN).pdf $(PDFDIR)
	@osascript -e 'tell application "Preview" to close document "$(MAIN).pdf"' || echo "Nothing to close"
	@open $(PDFDIR)/$(MAIN).pdf

$(FMATTER): %: initialize
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(FMATTERDIR)/$*}\input{$(MAIN)}"
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(FMATTERDIR)/$*/$(BUILDDIR)
	@mv $(BUILDDIR)/$(FMATTERDIR)/$*/$(BUILDDIR)/$*.pdf $(PDFDIR)/$(FMATTERDIR)
	@osascript -e 'tell application "Preview" to close document "$*.pdf"' || echo "Nothing to close"
	@open $(PDFDIR)/$(FMATTERDIR)/$*.pdf

$(CHAPTERS): %: % initialize
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(CHAPTERDIR)/$*/main}\input{$(MAIN)}"
	@if [ $(BIB) = 1 ]; then\
		$(BIBTEXENGINE) $(BUILDDIR)/$*;\
		$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(CHAPTERDIR)/$*/main}\input{$(MAIN)}";\
	fi
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(CHAPTERDIR)/$*/$(BUILDDIR)
	@mv $(BUILDDIR)/$(CHAPTERDIR)/$*/$(BUILDDIR)/$*.pdf $(PDFDIR)/$(CHAPTERDIR)
	@osascript -e 'tell application "Preview" to close document "$*.pdf"' || echo "Nothing to close"
	open $(PDFDIR)/$(CHAPTERDIR)/$*.pdf

$(APPENDICES): %: % initialize
	$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(APPENDICESDIR)/$*/main}\input{$(MAIN)}"
	@if [ $(BIB) = 1 ]; then\
		$(BIBTEXENGINE) $(BUILDDIR)/$*;\
		$(LATEXENGINE) -jobname=$* --output-directory=$(BUILDDIR) "\includeonly{$(APPENDICESDIR)/$*/main}\input{$(MAIN)}";\
	fi
	@mv $(BUILDDIR)/$*.* $(BUILDDIR)/$(APPENDICESDIR)/$*/$(BUILDDIR)
	@mv $(BUILDDIR)/$(APPENDICESDIR)/$*/$(BUILDDIR)/$*.pdf $(PDFDIR)/$(APPENDICESDIR)
	@osascript -e 'tell application "Preview" to close document "$*.pdf"' || echo "Nothing to close"
	@open $(PDFDIR)/$(APPENDICESDIR)/$*.pdf

all: initialize $(FMATTER) $(CHAPTERS) $(APPENDICES) $(MAIN)

initialize:
	@mkdir -p $(BUILDDIR)
	@mkdir -p $(PDFDIR)
	@mkdir -p $(foreach section,  $(FMATTER),    $(BUILDDIR)/$(FMATTERDIR)/$(section)/$(BUILDDIR) $(PDFDIR)/$(FMATTERDIR))
	@mkdir -p $(foreach chapter,  $(CHAPTERS),   $(BUILDDIR)/$(CHAPTERDIR)/$(chapter)/$(BUILDDIR) $(PDFDIR)/$(CHAPTERDIR))
	@mkdir -p $(foreach appendix, $(APPENDICES), $(BUILDDIR)/$(APPENDICESDIR)/$(appendix)/$(BUILDDIR) $(PDFDIR)/$(APPENDICESDIR))

clean:
	@echo "Cleaning up..."
	@rm -rf $(BUILDDIR)
	@rm -rf $(PDFDIR)

help:
	@echo ''
	@echo '${W}Usage:'
	@echo '  ${Y}make ${G}<option> ${G}<target>'
	@echo ''
	@echo '${W}Targets:'
	@echo '  ${Y}thesis           ${G} - Build the entire document'
	@echo '  ${Y}initialize       ${G} - Build and dislpay individual sections and entire document'
	@echo '  ${Y}title            ${G} - Build and display title page'
	@echo '  ${Y}abstract         ${G} - Build and display the abstract'
	@echo '  ${Y}acknowledgements ${G} - Build and display the acknowledgements'
	@echo '  ${Y}copyright        ${G} - Build and display the copyright'
	@echo '  ${Y}dedication       ${G} - Build and display the dedication'
	@echo '  ${Y}approval         ${G} - Build and display the approval'
	@echo '  ${Y}<chapter>        ${G} - Build and display individual chapters'
	@echo '  ${Y}<appendix>       ${G} - Build and display individual appendices'
	@echo '  ${Y}all              ${G} - Build and display all sections and the entire document'
	@echo '  ${Y}clean            ${G} - Remove all build directories'
	@echo '  ${Y}help             ${G} - Display this help'
	@echo ''
	@echo '${W}Options:'
	@echo '  ${Y}BIB              ${G} - Compile bibliography (1 = yes, 0 = no). Default is yes.'
	@echo ''
