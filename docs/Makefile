# Makefile for building CORD docs site, guide.opencord.org
# Building docs requires the following tools:
#  - Gitbook toolchain: https://toolchain.gitbook.com/setup.html
#  - NPM (for Gitbook and Swagger)
#  - Python (for build glossary script)
#  - linkchecker (for test target) http://wummel.github.io/linkchecker/

default: serve

# use bash for pushd/popd, and to fail if commands within  a pipe fail
SHELL = bash -o pipefail

GENERATED_DOCS = build_glossary.md

serve: setup
	gitbook serve --port 4040

build: setup
	gitbook build

setup: clean platform-install cord-tester profiles xos xos-gui xos-tosca swagger $(GENERATED_DOCS)
	gitbook init
	gitbook install

test: linkcheck lint

linkcheck: build
	linkchecker --check-extern -a _book/

platform-install:
	ln -s ../platform-install/docs platform-install

cord-tester:
	ln -s ../../test/cord-tester/docs cord-tester

profiles:
	mkdir -p profiles
	ln -s ../../../orchestration/profiles/rcord/docs profiles/rcord
	ln -s ../../../orchestration/profiles/ecord/docs profiles/ecord
	ln -s ../../../orchestration/profiles/mcord/docs profiles/mcord

lint:
	@echo "markdownlint(mdl) version: `mdl --version`"
	@echo "rule descriptions: https://github.com/markdownlint/markdownlint/blob/master/docs/RULES.md"
	@echo "style config:"
	@echo "---"
	@cat mdlstyle.rb
	@echo "---"
	mdl -s mdlstyle.rb `find -L . ! -path "./partials/*" ! -path "./_book/*" ! -path "./node_modules/*" ! -path "./cord-tester/modules/*" -name "*.md"`

xos:
	ln -s ../../orchestration/xos/docs xos

xos-gui:
	ln -s ../../orchestration/xos-gui/docs xos-gui

xos-tosca:
	ln -s ../../orchestration/xos-tosca/docs xos-tosca

build_glossary.md: scripts/descriptions.md scripts/defaults.md.j2 scripts/defaultsdoc.py scripts/markedyaml.py
	python scripts/defaultsdoc.py -o build_glossary.md

swagger: xos
	pushd ../../orchestration/xos/docs/; make swagger_docs; popd;

clean:
	rm -rf $(GENERATED_DOCS)
	rm -rf _book
	rm -rf node_modules
	rm -rf platform-install test profiles xos xos-gui xos-tosca

