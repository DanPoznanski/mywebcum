












## list

variant1:
```yaml
list_one:
  - foo 
  - bar
```
or variant2:
```yaml
list_one_other: [foo, bar]
```

## Dictionary

variant1:
```yaml 
map_one:
  foo: one
  bar: two
```
or variant2:
```yaml
map_one_another:{foo: one, bar: two}
```

## Strings

variants:
```yaml
string_one: Hello World
string_two: «Hello World»
string_three: 'Hello World'

## bool

variant:
```yaml
bool_one: yes
bool_two: no
bool_three: True
bool_four: true
bool_five: false
bool_string: «yes»
bool_dig_yes: 1
bool_dig_no: 0
```

## Text

```yaml
long_text: >
  support for reading
  and 
  writing
```
or
```yaml
long_text: | # or \
  support reading and writing
```

## environments
```yaml
foo: &default_setting
  env: dev
  db: 
    host: localhost
  db_name: test
  port: 5432 
```

```yaml
dev: *default_settings
stage: *default_settings
```

```yaml
stage:
  <<: *default_settings
  env: stage
  app:
    port: 8080
```

## Multiple documents

```yaml
---
foo: hello world
---
foo: hello world2
```
-*/--