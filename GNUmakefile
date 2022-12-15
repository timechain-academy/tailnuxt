# PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/X11/bin
SHELL									:= /bin/bash

PWD										?= pwd_unknown

PREFIX									:=
export PREFIX:=

#space:=
#space+=

# CURRENT_PATH := $(subst $(lastword $(notdir $(MAKEFILE_LIST))),,$(subst $(space),\$(space),$(shell realpath '$(strip $(MAKEFILE_LIST))')))
# export CURRENT_PATH

THIS_DIR=$(dir $(abspath $(firstword $(MAKEFILE_LIST))))
export THIS_DIR

TIME									:= $(shell date +%s)
export TIME

# PROJECT_NAME defaults to name of the current directory.
ifeq ($(project),)
PROJECT_NAME							:= $(notdir $(PWD))
else
PROJECT_NAME							:= $(project)
endif
export PROJECT_NAME

ifeq ($(NODE_VERSION),)
#NODE_VERSION									:= $(shell node --version)
NODE_VERSION									:= 14
else
NODE_VERSION									:= $(NODE_VERSION)
endif
export NODE_VERSION

ifeq ($(force),true)
FORCE									:= --force
endif
export FORCE

#GIT CONFIG
GIT_USER_NAME							:= $(shell git config user.name)
export GIT_USER_NAME
GIT_USER_EMAIL							:= $(shell git config user.email)
export GIT_USER_EMAIL
GIT_SERVER								:= https://github.com
export GIT_SERVER
GIT_PROFILE								:= $(shell git config user.name)
export GIT_PROFILE
GIT_BRANCH								:= $(shell git rev-parse --abbrev-ref HEAD)
export GIT_BRANCH
GIT_HASH								:= $(shell git rev-parse --short HEAD)
export GIT_HASH
GIT_PREVIOUS_HASH						:= $(shell git rev-parse --short HEAD^1)
export GIT_PREVIOUS_HASH
GIT_REPO_ORIGIN							:= $(shell git remote get-url origin)
export GIT_REPO_ORIGIN
GIT_REPO_NAME							:= $(PROJECT_NAME)
export GIT_REPO_NAME
GIT_REPO_PATH							:= $(HOME)/$(GIT_REPO_NAME)
export GIT_REPO_PATH

HOMEBREW_BREW_GIT_REMOTE=$(strip $(THIS_DIR))brew# put your Git mirror of Homebrew/brew here
export HOMEBREW_BREW_GIT_REMOTE

HOMEBREW_CORE_GIT_REMOTE=$(strip $(THIS_DIR))homebrew-core# put your Git mirror of Homebrew/homebrew-core here
export HOMEBREW_CORE_GIT_REMOTE

BREW                                    := $(shell which brew)
export BREW
# BREW_PREFIX                             := $(shell brew --prefix)
export BREW_PREFIX
# BREW_CELLAR                             := $(shell brew --cellar)
export BREW_CELLAR
HOMEBREW_NO_ENV_HINTS                   :=false
export HOMEBREW_NO_ENV_HINTS


##make	:	command			description
.ONESHELL:
.PHONY:-
.PHONY:	init
.PHONY:	help
.PHONY:	report
.PHONY:	brew
.SILENT:
##	:

-: help
	@echo "source ~/.bashrc"
	@echo "source ~/.bash_profile"

##	:	init
init:
#	@["$(shell $(SHELL))" == "/bin/zsh"] && zsh --emulate sh
	@cd ./scripts && ./initialize

.PHONY:install
##	:	install - ./scripts npm install
install:
	@npm install
	@cd ./scripts && npm install

.PHONY: start
##	:	start
start:
	@cd ./app && npm start

##	:	help
help:
	@echo ''
	@sed -n 's/^##ARGS//p' ${MAKEFILE_LIST} | column -t -s ':' |  sed -e 's/^/ /'
	# @sed -n 's/^.PHONY//p' ${MAKEFILE_LIST} | column -t -s ':' |  sed -e 's/^/ /'
	@sed -n 's/^##//p' ${MAKEFILE_LIST} | column -t -s ':' |  sed -e 's/^/ /'
	@echo ""
	@echo ""
	@echo ""
	@echo "Useful Commands:"
	@echo ""
	@echo "gpg --output public.pgp --armor --export FINGERPRINT"

##	:	report			environment args
report:
	@echo ''
	@echo ' TIME=${TIME}	'
	@echo ' CURRENT_PATH=${CURRENT_PATH}	'
	@echo ' THIS_DIR=${THIS_DIR}	'
	@echo ' PROJECT_NAME=${PROJECT_NAME}	'
	@echo ' NODE_VERSION=${NODE_VERSION}	'
	@echo ' GIT_USER_NAME=${GIT_USER_NAME}	'
	@echo ' GIT_USER_EMAIL=${GIT_USER_EMAIL}	'
	@echo ' GIT_SERVER=${GIT_SERVER}	'
	@echo ' GIT_PROFILE=${GIT_PROFILE}	'
	@echo ' GIT_BRANCH=${GIT_BRANCH}	'
	@echo ' GIT_HASH=${GIT_HASH}	'
	@echo ' GIT_PREVIOUS_HASH=${GIT_PREVIOUS_HASH}	'
	@echo ' GIT_REPO_ORIGIN=${GIT_REPO_ORIGIN}	'
	@echo ' GIT_REPO_NAME=${GIT_REPO_NAME}	'
	@echo ' GIT_REPO_PATH=${GIT_REPO_PATH}	'
	@echo ' BREW=${BREW}	'
	@echo ' HOMEBREW_BREW_GIT_REMOTE=${HOMEBREW_BREW_GIT_REMOTE}	'
	@echo ' HOMEBREW_CORE_REMOTE=${HOMEBREW_CORE_GIT_REMOTE}	'
	@echo ' BREW_PREFIX=${BREW_PREFIX}	'
	@echo ' BREW_CELLAR=${BREW_CELLAR}	'
	@echo ' HOMEBREW_NO_ENV_HINTS=${HOMEBREW_NO_ENV_HINTS}	'

#.PHONY:
#phony:
#	@sed -n 's/^.PHONY//p' ${MAKEFILE_LIST} | column -t -s ':' |  sed -e 's/^/ /'


.PHONY: command
##	:	command		 	command sequence
command:
	@echo "command sequence here..."

.PHONY: nvm
.ONESHELL:
##	:	nvm		 	install node version manager
nvm:
	@curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash || git pull -C $(HOME)/.nvm
	@echo "-------- Restart your terminal!!!! ---------"

.PHONY: all
##	:	all			execute installer scripts
all:- nvm init 

.PHONY: docs readme index
index: docs
readme: docs
docs:
	@echo 'docs'
	bash -c "if pgrep MacDown; then pkill MacDown; fi"
	bash -c "make help > $(PWD)/sources/COMMANDS.md"
	bash -c 'cat $(PWD)/sources/HEADER.md                >  $(PWD)/README.md'
	bash -c 'cat $(PWD)/sources/COMMANDS.md              >> $(PWD)/README.md'
	bash -c 'cat $(PWD)/sources/FOOTER.md                >> $(PWD)/README.md'
	#bash -c "if hash open 2>/dev/null; then open README.md; fi || echo failed to open README.md"
	#brew install pandoc
	bash -c "if hash pandoc 2>/dev/null; then echo; fi || brew install pandoc"
	#bash -c 'pandoc -s README.md -o index.html  --metadata title="$(GH_USER_SPECIAL_REPO)" '
	bash -c 'pandoc -s README.md -o index.html'
	#bash -c "if hash open 2>/dev/null; then open README.md; fi || echo failed to open README.md"
	git add --ignore-errors sources/*.md
	git add --ignore-errors *.md
	#git ls-files -co --exclude-standard | grep '\.md/$\' | xargs git

.PHONY: submodule submodules
submodule: submodules
submodules:
	git submodule update --init --recursive
	git submodule foreach 'git fetch origin; git checkout $$(git rev-parse --abbrev-ref HEAD); git reset --hard origin/$$(git rev-parse --abbrev-ref HEAD); git submodule update --recursive; git clean -dfx'

.PHONY: node
node:
	$(MAKE) -f node.mk

clean:
	rm -rf ./node_modules/
	rm -rf ./scripts/node_modules/
clean-usr-local-libs:
	rm -rf /usr/local/libs/node_modules/
clean-nvm:
	rm -rf $(HOME)/.nvm
	$(MAKE) nvm
clean-npm:
	rm -rf $(HOME)/.npm
clean-all: clean clean-nvm clean-npm clean-usr-local-libs

-include node.mk
# vim: set noexpandtab:
# vim: set setfiletype make
all: install build generate

postinstall:
	npm run postinstall
build:
	npm run build
dev:
	npm run dev
generate:
	npm run generate
preview:
	npm run preview
