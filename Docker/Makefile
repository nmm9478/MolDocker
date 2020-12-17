OS = $(patsubst CYGWIN_NT%,CYGWIN_NT,$(shell uname -s))

# MAC:
ifeq ($(OS),Darwin)
CHIMERAX_APP ?= /Applications/ChimeraX.app
CHIMERAX_EXE = $(CHIMERAX_APP)/Contents/bin/ChimeraX
endif

# WINDOWS:
ifeq ($(OS),CYGWIN_NT)
CHIMERAX_APP ?= "c:/Program Files/ChimeraX.app"
CHIMERAX_EXE = $(CHIMERAX_APP)/bin/ChimeraX.exe
endif

# LINUX:
ifeq ($(OS),Linux)
#CHIMERAX_EXE = $(shell which chimerax)
endif

RUN = $(CHIMERAX_EXE) --nogui --exit --cmd

PYSRCS = $(wildcard src/*.py)
CSRCS = $(wildcard src/*.cpp)
SRCS = $(PYSRCS) $(CSRCS)

# If you want to install into ChimeraX.app, add "user false"
# to the "devel build" and "devel install" commands.
# By default, we install for just the current user.

install:	bundle_info.xml $(SRCS) generated_files
	$(RUN) "devel install . exit true"

wheel:	bundle_info.xml $(SRCS) generated_files
	$(RUN) "devel build . exit true"

test::
	python3 -m nose test

test-qt::
	env RMF_DISABLE_QT_MOCK=1 python3 -m nose test

test-chimerax::
	$(CHIMERAX_EXE) --exit --nogui test/run-with-chimerax.py test/test_*.py

debug:
	$(CHIMERAX_EXE) --debug

clean:	clean_generated_files
	if [ -x $(CHIMERAX_EXE) ]; then \
		$(RUN) "devel clean . exit true" ; \
	else \
		rm -rf build dist *.egg-info src/__pycache__ ; \
	fi

pylint:
	$(CHIMERAX_EXE) -m flake8 $(filter %.py, $(SRCS))

# Modify targets below if you have files that need
# to be generated before building the bundle and/or
# removed when cleaning the bundle

generated_files:
	# Generate files

clean_generated_files:
	# Remove generated files