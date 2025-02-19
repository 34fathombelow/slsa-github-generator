SHELL := /bin/bash
ACTION_NAME = $(shell basename "$$(pwd)")

.PHONY: help
help: ## Shows all targets and help from the Makefile (this message).
	@echo "$(ACTION_NAME) Makefile"
	@echo "Usage: make [COMMAND]"
	@echo ""
	@grep --no-filename -E '^([/a-z.A-Z0-9_%-]+:.*?|)##' $(MAKEFILE_LIST) | \
		awk 'BEGIN {FS = "(:.*?|)## ?"}; { \
			if (length($$1) > 0) { \
				printf "  \033[36m%-20s\033[0m %s\n", $$1, $$2; \
			} else { \
				if (length($$2) > 0) { \
					printf "%s\n", $$2; \
				} \
			} \
		}'

node_modules/.installed: package.json package-lock.json
	npm ci
	touch node_modules/.installed

.PHONY: action
action: node_modules/.installed ## Builds the action.
	npm run build

.PHONY: package
package: action ## Builds the distribution package.
	npm run package

.PHONY: clean
clean:
	rm -rf dist lib node_modules

## Testing
#####################################################################

.PHONY: unit-test
unit-test: node_modules/.installed ## Runs all unit tests.
	npm run test

.PHONY: lint
lint: node_modules/.installed ## Runs eslint.
	npm run format-check
	npm run lint
