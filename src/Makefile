# This is a makefile that builds the KMR morphological parser
# *****************************************************************
# This is a dummy file. KMR refers to the name of the language, kmr to the 
# ISO code of the language, for use in file names.

# This is a preliminary file that builds a KMR parser based upon
# twol and not twolc.

# Here we build the final generator , an inverted transducer of the analyzer.
# It is dependent upon kmr.save


XFST = xfst
LEXC = lexc
TWOLC = twolc

ifeq (victorio.uit.no, $(shell hostname))
XFST = /opt/sami/xerox/c-fsm/ix86-linux2.6-gcc3.4/bin/fst -utf8
LEXC = /opt/sami/xerox/bin/lexc -utf8
TWOLC = /opt/sami/xerox/bin/twolc -utf8
endif

scripts=$(GTHOME)/gt/script

LEX = kmr-lex.txt \
	noun-kmr-lex.txt \
	noun-kmr-morph.txt \
	adj-kmr-lex.txt \
	adj-kmr-morph.txt \
	verb-kmr-lex.txt \
	verb-kmr-morph.txt \
	adv-kmr-lex.txt \
	pron-kmr-lex.txt \
	punct-kmr-lex.txt \
	pp-kmr-lex.txt \
	cc-kmr-lex.txt \
	cs-kmr-lex.txt \
	interj-kmr-lex.txt 

#	numeral-kmr-lex.txt \
#	abbr-kmr-lex.txt \
#	acro-kmr-lex.txt \
#	propernoun-kmr-lex.txt \
#	propernoun-kmr-morph.txt \
#	det-kmr-lex.txt

all: ikmr.fst



ikmr.fst: ../bin/ikmr.fst
../bin/ikmr.fst: ../bin/kmr.save ../bin/tok.fst ../bin/inituppercase.fst ../bin/kmr.fst
	@echo
	@echo "*** Building the inverse ikmr.fst ***"
	@echo
	@printf "load $< \n\
	invert net \n\
	save stack $@ \n\
	quit \n" > ../../tmp/ikmr-fst-script
	$(XFST) < ../../tmp/ikmr-fst-script
	@rm -f ../../tmp/ikmr-fst-script


# This goal is to build the final analyser. It depends on all the files.

kmr.fst: ../bin/kmr.fst
../bin/kmr.fst: ../bin/kmr.save ../bin/inituppercase.fst 
	@echo
	@echo "*** Building kmr.fst ***" ;
	@echo
	@printf "read regex [[@\"../bin/kmr.save\"] .o. \
	[@\"../bin/inituppercase.fst\"]] ; \n\
	save stack ../bin/kmr.fst \n\
	quit \n" > ../../tmp/kmr-fst-script
	$(XFST) < ../../tmp/kmr-fst-script
	@rm -f ../../tmp/kmr-fst-script


# The second goal is to build the inituppercase.fst file
# This goal depends on inituppercase.regex

inituppercase.fst: ../bin/inituppercase.fst
../bin/inituppercase.fst: inituppercase.regex
	@echo
	@echo "*** Building inituppercase.fst ***" ;
	@echo
	@printf "read regex < inituppercase.regex \n\
	save stack ../bin/inituppercase.fst \n\
	quit \n" > ../../tmp/inituppercase-script
	$(XFST) < ../../tmp/inituppercase-script
	@rm -f ../../tmp/inituppercase-script


# Another goal is to build a preprocessor.This goal depends on tok.txt

tok.fst: ../bin/tok.fst
../bin/tok.fst: tok.txt
	@echo
	@echo "*** Building the tokenizer tok.fst ***" ;
	@echo
	@printf "source tok.txt \n\
	save stack ../bin/tok.fst \n\
	quit \n" > ../../tmp/tok-script
	$(XFST) < ../../tmp/tok-script
	@rm -f ../../tmp/tok-script


# The third goal is to build kmr.save
# This goal depends on twol-kmr.bin and a bunch of lexicon files
kmr.save: ../bin/kmr.save
../bin/kmr.save: ../bin/twol-kmr.bin $(LEX)
	@echo
	@echo "*** Building kmr-lex.save ***" ;
	@echo
	printf "compile-source $(LEX)  \n\
	read-rules $< \n\
	compose-result \n\
	save-result $@ \n\
	quit \n" > ../../tmp/kmr-lex-save-script
	$(LEXC)  < ../../tmp/kmr-lex-save-script
	rm -f ../../tmp/kmr-lex-save-script



# The fourth goal is to build twol-kmr.bin
# This goal depends on twol-kmr.txt
twol-kmr.bin: ../bin/twol-kmr.bin
../bin/twol-kmr.bin: twol-kmr.txt
	@echo
	@echo "*** Building twol-kmr.bin ***" ;
	@echo
	@printf "read-grammar twol-kmr.txt \n\
	compile \n\
	save-binary ../bin/twol-kmr.bin \n\
	quit \n" > ../../tmp/twol-kmr-script
	$(TWOLC)  < ../../tmp/twol-kmr-script
	@rm -f ../../tmp/twol-kmr-script


clean:
	@rm -f ../bin/*.bin ../bin/*.fst ../bin/*.save


