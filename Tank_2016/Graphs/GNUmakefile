TARGET = 

all: $(TARGET)

clean:
	rm -f *.dvi *.log *.aux *.toc *.bbl *.blg *~ *.core

cleandir: clean
	rm -f $(TARGET) *.dot

.SUFFIXES: .dot .pdf

.dot.pdf: $*.dot
	dot -Tpdf $*.dot -o $*.pdf
