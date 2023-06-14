pdf:
	pandoc --pdf-engine /Library/TeX/texbin/pdflatex --biblio zkprover.bib --csl ieee.csl --mathjax --number-sections --citeproc -f markdown --filter graphviz.py -o zkprover.pdf zkprover.md 

.PHONY: pdf
