.PHONY: black-check
black-check:
	uv run black --check src tests

.PHONY: black
black:
	uv run black src tests

.PHONY: flake8
flake8:
	uv run flake8 src tests

.PHONY: isort-check
isort-check:
	uv run isort --check-only src tests

.PHONY: isort
isort:
	uv run isort src tests

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
	$(MAKE) black
	$(MAKE) isort
	$(MAKE) mdformat

.PHONY: lint
lint:
	$(MAKE) black-check
	$(MAKE) isort-check
	$(MAKE) mdformat-check
	$(MAKE) flake8
	$(MAKE) mypy

.PHONY: test-all
test-all:
	$(MAKE) lint
	$(MAKE) test