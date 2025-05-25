.PHONY: mdformat
mdformat:
	uv run mdformat *.md

.PHONY: mdformat-check
mdformat-check:
	uv run mdformat --check *.md

.PHONY: mypy
mypy:
	uv run mypy src

.PHONY: test
test:
	uv run pytest tests --cov=src --cov-report term-missing --durations 5

.PHONY: format
format:
	uv run ruff format src
	$(MAKE) mdformat

.PHONY: lint
lint:
	uv run ruff check src
	$(MAKE) mdformat-check

.PHONY: test-all
test-all:
	$(MAKE) lint
	$(MAKE) test