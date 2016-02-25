# Docker Jekyll

A Docker image for Jekyll, tested with minimal mistakes theme

## Docker compose

```yaml
web:
    image: ypereirareis/jekyll:3.1.2-minimal-mistakes
    working_dir: /app
    user: bob
    volumes:
        - .:/app
    ports:
        - "4000:4000"
```

## Makefile

```shell

step=--------------------------------
project=Blog
projectCompose=blog-USERNAME
compose = docker-compose -p $(projectCompose)

install: remove build jkbuild jkserve

bundle:
	@echo "$(step) Bundler $(step)"
	@$(compose) run --rm web bash -ci '\
                bundle install --path vendor/bundle && \
                    bundle check && \
                    bundle update'

jkbuild:
	@echo "$(step) Jekyll build $(step)"
	@$(compose) run --rm web bash -ci '\
		bundle exec jekyll build'

jkserve:
	@echo "$(step) Jekyll Serve $(step)"
	@$(compose) up -d web

start: stop jkbuild jkserve

stop:
	@echo "$(step) Stopping $(project) $(step)"
	@$(compose) stop

state:
	@echo "$(step) Etat $(project) $(step)"
	@$(compose) ps

remove: stop
	@echo "$(step) Remove $(project) $(step)"
	@$(compose) rm --force

bash:
	@echo "$(step) Bash $(project) $(step)"
	@$(compose) run --rm web bash

```
