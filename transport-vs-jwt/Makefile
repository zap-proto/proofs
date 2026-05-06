PROOF = proof
REFS  = ../../papers/transport-vs-jwt/refs.bib

LATEXMK ?= latexmk
PDFLATEX ?= pdflatex
BIBER ?= biber

LATEXMK_FLAGS = -pdf -interaction=nonstopmode -halt-on-error -file-line-error

.PHONY: all clean distclean check

all: $(PROOF).pdf

$(PROOF).pdf: $(PROOF).tex $(REFS)
	@if command -v $(LATEXMK) >/dev/null 2>&1; then \
		$(LATEXMK) $(LATEXMK_FLAGS) $(PROOF).tex; \
	else \
		$(PDFLATEX) -interaction=nonstopmode $(PROOF).tex; \
		$(BIBER) $(PROOF); \
		$(PDFLATEX) -interaction=nonstopmode $(PROOF).tex; \
		$(PDFLATEX) -interaction=nonstopmode $(PROOF).tex; \
	fi

# Brace-balance smoke test for environments where pdflatex is unavailable.
check:
	@awk 'BEGIN{o=0;c=0} {for(i=1;i<=length($$0);i++){ch=substr($$0,i,1); if(ch=="{")o++; else if(ch=="}")c++}} END{print "open:",o,"close:",c; if(o!=c){exit 1}}' $(PROOF).tex
	@grep -c '\\begin{theorem}'  $(PROOF).tex | xargs -I{} echo "theorems:    {}"
	@grep -c '\\begin{lemma}'    $(PROOF).tex | xargs -I{} echo "lemmas:      {}"
	@grep -c '\\begin{corollary}' $(PROOF).tex | xargs -I{} echo "corollaries: {}"
	@grep -c '\\begin{proof}'    $(PROOF).tex | xargs -I{} echo "proofs:      {}"

clean:
	rm -f *.aux *.bbl *.bcf *.blg *.fdb_latexmk *.fls *.log *.out *.run.xml *.toc *.synctex.gz

distclean: clean
	rm -f $(PROOF).pdf
