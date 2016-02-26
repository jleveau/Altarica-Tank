ARCHIVE_SUJET = FD-2015-2016-M1-CC-sujet.tgz

ARCHIVE_CORRIGE = FD-2015-2016-M1-CC-corrige.tgz

TEX = pdflatex

MODEL = tank

SOURCE_ALT = Alt/$(MODEL).alt

SOURCE_SPEC = Spec/$(MODEL).spe

TARGET = sujet.pdf\
	rapport.pdf\
	$(ARCHIVE_SUJET)\
	$(ARCHIVE_CORRIGE)\

SOURCE_TEX = $(MODEL).tex\
	sujet.tex\
	rapport.tex\

SUBDIRS = Alt Spec

SUBDIRS2 = Res Controleurs Graphs

SOURCE_SUJET = $(SUBDIRS)\
	Controleurs/GNUmakefile\
	Graphs/GNUmakefile\
	Res/GNUmakefile\
	$(MODEL).tex\
	sujet.tex\
	sujet.pdf\
	TODO\
	rapport.tex\
	GNUmakefile\

SOURCE_CORRIGE = $(SUBDIRS)\
	Controleurs/GNUmakefile\
	Graphs/GNUmakefile\
	Res/GNUmakefile\
	$(MODEL).tex\
	TODO\
	ControleursOpt/GNUmakefile\
	ControleursOpt/Optimisation.alt\
	rapport.tex\
	GNUmakefile\

# pour iterer sur le nombre de pannes
PANNES = 0 1 2 3

# pour iterer sur les controleurs generes
ITERATIONS = 1 2 3 4 5

# contient le controleur original
CONTROLEURS_INIT = Ctrl CtrlVV

# La liste des controleurs que l'on souhaite optimiser
CONTROLEURS_OPT = 

all: sources $(MODEL).time $(TARGET)

clean:
	for d in $(SUBDIRS); do \
		$(MAKE) -C $$d $@ || exit 1;\
	done
	for d in $(SUBDIRS2); do \
		$(MAKE) -C $$d $@ || exit 1;\
	done
	rm -f *.dvi *.eps *.log *.aux *.toc *.bbl *.blg *.out *.snm *.nav *~ *.core

cleandir: clean
	for d in $(SUBDIRS); do \
		$(MAKE) -C $$d $@ || exit 1;\
	done
	for d in $(SUBDIRS2); do \
		$(MAKE) -C $$d $@ || exit 1;\
	done
	rm -f *.dot *.prop *.res *.results *.validate
	rm -f $(TARGET) $(SOURCE_ALT) $(SOURCE_SPEC) $(MODEL).time

sources : 
	for d in $(SUBDIRS); do \
		$(MAKE) -C $$d all || exit 1;\
	done

$(MODEL).time: $(SOURCE_ALT) $(SOURCE_SPEC)
	for c in $(CONTROLEURS_INIT); do \
		for p in $(PANNES); do \
			cat Alt/Parameters.alt > Alt/tank.alt;\
			cat Alt/Valve.alt >> Alt/tank.alt;\
			cat Alt/ValveVirtual.alt >> Alt/tank.alt;\
			cat Alt/Tank.alt >> Alt/tank.alt;\
			cat 'Alt/'$$c'.alt' >> Alt/tank.alt;\
			cat Alt/System.alt >> Alt/tank.alt;\
			sed -e 's:NbPannes:'$$p':' -e 's:NomDuControleur:'$$c':' Alt/tank.alt > Alt/test.alt;\
			sed -e 's:CtrlInitial:'$$c':' -e 's:NbPannes:'$$p':' -e 's:NumIter:1I:' -e 's:NomDuControleur:'$$c':' Spec/System.spe > Spec/test.spe;\
			arc -b Alt/test.alt Spec/test.spe 2>> $(MODEL).time;\
			for d in $(ITERATIONS); do \
				cat Alt/Parameters.alt > Alt/tank.alt;\
				cat Alt/Valve.alt >> Alt/tank.alt;\
				cat Alt/ValveVirtual.alt >> Alt/tank.alt;\
				cat Alt/Tank.alt >> Alt/tank.alt;\
				cat 'Controleurs/'$$c$$p'F'$$d'I.alt' >> Alt/tank.alt;\
				cat Alt/System.alt >> Alt/tank.alt;\
				nd=$$(expr $$d + 1);\
				sed -e 's:NbPannes:'$$p':' -e 's:NomDuControleur:'$$c$$p'F'$$d'I:' Alt/tank.alt > Alt/test.alt;\
				sed -e 's:CtrlInitial:'$$c':' -e 's:NbPannes:'$$p':' -e 's:NumIter:'$$nd'I:' -e 's:NomDuControleur:'$$c$$p'F'$$d'I:' Spec/System.spe > Spec/test.spe;\
				arc -b Alt/test.alt Spec/test.spe 2>> $(MODEL).time;\
			done;\
		done;\
	done;
	for d in $(SUBDIRS2); do \
		$(MAKE) -C $$d || exit 1;\
	done
	uname -mps > $(MODEL).time

sujet.pdf: $(SOURCE_TEX)

rapport.pdf: $(SOURCE_TEX) $(MODEL).time

$(ARCHIVE_SUJET): $(SOURCE_SUJET)
	tar -czf $(ARCHIVE_SUJET) $(SOURCE_SUJET)

$(ARCHIVE_CORRIGE): $(SOURCE_CORRIGE)
	tar -czf $(ARCHIVE_CORRIGE) $(SOURCE_CORRIGE)

.SUFFIXES: .pdf .tex

.tex.pdf:
	$(TEX) $*.tex && $(TEX) $*.tex
