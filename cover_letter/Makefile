MAIN = cover_letter
LATEXENGINE = pdflatex
BUILDDIR = build
PDFDIR = pdf

$(MAIN): initialize
	$(LATEXENGINE) --shell-escape --output-directory=$(BUILDDIR) $(MAIN).tex
	@mv $(BUILDDIR)/$(MAIN).pdf $(PDFDIR)
	@osascript -e 'tell application "Preview" to close document "$(MAIN).pdf"' || echo 'Nothing to close'
	@open $(PDFDIR)/$(MAIN).pdf

initialize:
	@mkdir -p $(BUILDDIR)
	@mkdir -p $(PDFDIR)

clean:
	@rm -rf $(BUILDDIR)
	@rm -rf $(PDFDIR)

.PHONY : initialize clean
