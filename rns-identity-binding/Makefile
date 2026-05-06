PAPER = proof
LATEX = pdflatex -interaction=nonstopmode -halt-on-error

.PHONY: all clean check

all: $(PAPER).pdf

$(PAPER).pdf: $(PAPER).tex
	$(LATEX) $(PAPER).tex
	$(LATEX) $(PAPER).tex

check:
	$(LATEX) -draftmode $(PAPER).tex

clean:
	rm -f $(PAPER).aux $(PAPER).log $(PAPER).out $(PAPER).toc $(PAPER).pdf
