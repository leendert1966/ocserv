AUTOMAKE_OPTIONS = foreign
EXTRA_DIST = LICENSE README.md CONTRIBUTING.md

SUBDIRS = gl
SUBDIRS += src doc tests

ACLOCAL_AMFLAGS = -I m4 -I gl/m4

@CODE_COVERAGE_RULES@

local-code-coverage-output: code-coverage-capture
	cat $(CODE_COVERAGE_OUTPUT_DIRECTORY)/index.html|grep headerCovTableEntryLo|head -1|sed 's/^.*>\([0-9]\+\.[0-9]\+\s*%\)<.*$$/ coverage lines: \1/'

gl:
	gnulib-tool --add-import --lgpl=2

clang:
	make clean
	scan-build ./configure
	rm -rf scan.tmp
	scan-build -o scan.tmp make

ctags:
	find . -type f -name "*.[ch]*" | xargs @CTAGS@

cscope:
	@CSCOPE@ -b -R

cref: ctags cscope

ChangeLog:
	git log --pretty > ChangeLog
.PHONY: ChangeLog

files-update:

files-compare:
	@echo "*****************************************************************"
	@echo "If the following step fails, run files-update"
	@echo "*****************************************************************"

dist-hook: files-compare ChangeLog
	mv ChangeLog $(distdir)
	test -f doc/ocserv.8 && test -f doc/ocpasswd.8 && test -f doc/occtl.8

.PHONY: files-update files-compare
