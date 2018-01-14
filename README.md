# Uniform Development Environment API

## Why?

Because we just want to code, not figure out how to setup the development environment!

The goal is to provide a uniform way to build, and run stuff regardless of what language it's written in, or what dependencies it has, or what tools it requires.

More specifically:

> allow a new developer to quickly start developing and skip the nitty gritty of setting up a development environment

However, this API does not attempt to be an opaque layer - in time, you might need to run `yarn`, `mvn`, `rake` or whatever "manually".

## Why `make`?

Because it's ubiquitous - it's installed on most systems.

Because it provides a single entry point with "sub commands" syntax with minimal effort.

Because you are free to implement this API any way you want (e.g. `make dev.build` might call a Ruby script).

## The API

```
make dev.setup
```

Sets up the environment. This is meant to be something the developer runs once and forgets about. It's a good place to install dependencies.

```
make dev.start
```

Start any dependencies that the application might need. For example, if the application needs a database, start it here.

This may also start the application itself, or at least hint the user how to do that.

```
make dev.stop
```

Revert `make dev.start`.

```
make dev.teardown
```

Revert `make dev.setup`.

### Extensions

If it makes sense to you, you can also define more targets like:

```
make dev.db.migrate      # Run database migrations
make ci.run              # Run the CI pipeline
```

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
	echo 'All set. Now run the application in your IDE'

dev.stop:
	docker-compose stop

dev.teardown:
	docker-compose down
```

Running a Node.JS project with dockerized dependencies:
```
dev.setup:
	docker-compose build
	yarn

dev.start:
	docker-compose up -d
	yarn start

dev.stop:
	yarn stop
	docker-compose stop

dev.teardown:
	docker-compose down
```
