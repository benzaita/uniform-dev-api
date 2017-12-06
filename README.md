# Uniform Development Environment API

```
make dev.setup
make dev.build
make dev.start
make dev.stop
make dev.teardown
```

## Why?

Because we just want to code, not figure out how to setup the development environment!

The goal is to provide a uniform way to build, and run stuff regardless of what language it's written in, or what dependencies it has, or what tools it requires.

## Why `make`?

Because it's ubiquitous - it's installed on most systems.
Because it provides a single entry point with "sub commands" syntax with minimal effort.
Because you are free to implement this API any way you want (e.g. `make dev.build` might call a Ruby script).

## Examples

Running a Java backend with dockerized dependencies:

```
dev.setup:
	docker-compose build

dev.build:
	mvn package

dev.db.migrate:
	docker-compose up -d
	java $(JAVA_OPTS) db migrate $(CONFIG_FILE)

dev.start:
	docker-compose up -d
	java $(JAVA_OPTS) server $(CONFIG_FILE)

dev.stop:
	docker-compose stop

dev.teardown:
	docker-compose down
```

Running a Node.JS project with dockerized dependencies:
```
dev.setup:
	docker-compose build

dev.build:
	yarn

dev.start:
	docker-compose up -d
	yarn start

dev.stop:
	docker-compose stop
	yarn stop

dev.teardown:
	docker-compose down
```
