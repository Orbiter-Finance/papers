pdf:
	pandoc --pdf-engine /Library/TeX/texbin/pdflatex --biblio yellowpaper.bib --csl ieee.csl --mathjax --number-sections --citeproc -f markdown --filter graphviz.py -o new_era.pdf new_era.md 

.PHONY: pdf

