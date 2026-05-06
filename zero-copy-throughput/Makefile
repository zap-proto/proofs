PAPER=proof
LATEX=pdflatex
LATEXFLAGS=-interaction=nonstopmode -halt-on-error

.PHONY: all clean view

all: $(PAPER).pdf

$(PAPER).pdf: $(PAPER).tex
	$(LATEX) $(LATEXFLAGS) $(PAPER).tex
	$(LATEX) $(LATEXFLAGS) $(PAPER).tex

clean:
	rm -f *.aux *.log *.out *.toc *.synctex.gz $(PAPER).pdf

view: $(PAPER).pdf
	open $(PAPER).pdf
