---
# try also 'default' to start simple
theme: check24

# https://sli.dev/custom/highlighters.html
highlighter: shiki

# show line numbers in code blocks
lineNumbers: true

# persist drawings in exports and build
drawings:
  persist: false
# page transition

transition: slide-left
# transition: fade

# use UnoCSS
css: unocss

titleTemplate: '%s'

layout: cover
---
# PHPStan

<p style="text-decoration: line-through">Pain you gotta love</p>

## How to update easily and detect errors early on

[^1]: mostly
---
layout: center
---

# About me

Hi, I'm Pascal and working as Developer @ Check24 Hamburg

- PHP fanatic since over 15 years
- Interests: clean code & architecture
- At Check24: Laminas/Mezzio + Doctrine


- <a href="https://github.com/pascalheidmann" target="_blank"><carbon-logo-github /> github.com/pascalheidmann</a>
- <a href="https://www.linkedin.com/in/pascal-heidmann/" target="_blank"><carbon-logo-linkedin />
  linkedin.com/in/pascal-heidmann</a>
- <a href="https://heidmann.io" target="_blank"><mdi-account-circle /> heidmann.io</a>

---
layout: default
---
# Situation May 2022

- Product running on PHP 7.4
  - big codebase with > 100K LoC over 15K files
- PHP 8.1 out, 8.2 in development
- Lots of breaking changes between 7.4 <-> 8.0
  - and even 8.0 <-> 8.1 🤯

---

# Breaking Changes PHP 8.0 / 8.1

- new keywords like `match`, `mixed`, `enum`, `readonly`
- (most) resources have dedicated objects as return type instead of `resource`
- string-number comparison
- return types & type hints for native functions changed
  - passing `null` to `non-null` arguments is deprecated


## Lots of things that have to be checked manually
Or can we get a little help by a friend?

---
layout: center
---

# Who has used PHPStan?

<div v-click>
Or psalm?
</div>


---
layout: default
---

# What is PHPStan

<h2 class="text-center">
    <span v-click>
    PHPStan =
    </span>
    <span v-click>
    PHP
    </span>
    <span v-click>
    + Static Analysis
    </span>
</h2>

<div class="text-center" v-click>
    <h3>Let our computer do the heavy work and test your code automatically.</h3>
    <p>No need to write tests!</p>
</div>

---

# Basic example[^1]

```php {all|all|5|7|all}
<?php declare(strict_types = 1);

class HelloWorld
{
	public function sayHello(DateTimeImutable $date): void
	{
		echo 'Hello, ' . $date->format('j. n. Y');
	}
}
```

<div v-click="1">

| Line | Error                                                                                        |
|------|----------------------------------------------------------------------------------------------|
| 5    | Parameter $date of method HelloWorld::sayHello() has invalid typehint type DateTimeImutable. |
| 7    | Call to method format() on an unknown class DateTimeImutable.                                |

</div>

[^1]: [source: try phpstan](https://phpstan.org/r/549ceaa7-c9fd-4e35-ae5c-38fd6cb3dd7d)

<!--
DateTimeImmutable
-->

---

# Basic example (solved)
```php {5}
<?php declare(strict_types = 1);

class HelloWorld
{
	public function sayHello(DateTimeImmutable $date): void
	{
		echo 'Hello, ' . $date->format('j. n. Y');
	}
}
```

<p style="background: rgb(220 252 231/.8); padding: .5rem">No errors!</p>

---

# How does this work?

- Doc block
```php
/**
* @param string $param
 */
function baz($param) {}
```

<v-click>

- Return types
```php
function foo(): MyClass {}
```

</v-click>
<v-click>

- Type hints
```php
function bar(string $myString) {}
```

</v-click>

<v-click>

\+ stubs & phpstan extensions

</v-click>

---

# Conflicting code [^1]

```php {all|3,7-9,11|all}
<?php declare(strict_types = 1);

function foo (): string {
    return 'foo';
}

/**
* @return int
 */
function bar() {
    return foo();
}
```

| Line | Error                                                |
|------|------------------------------------------------------|
| 11   | Function bar() should return int but returns string. |

[^1]: https://phpstan.org/r/b5c4f4af-6fe8-4e9c-9377-757b4d4c69a2

---
layout: center
---
# Arrays

In PHP a special vehicle
- Index based list of things (other languages: `list`, `array`)
- Associative array (other languages: `map`, `object`, `hash`)
- Combination of both... 🥳

---

# Solution: Generics

### Fixed type array
```java
// array of 5 strings
myStringArray = new String[5];

// ...

public void foo(String[] stringArray) {}

foo(myStringArray);
```

<v-click>

### Generic arrays
```java
class Foo {
    public void foo<T>(T[] genericArray) {}

    // ...
    foo<String>(myStringArray);
}
```

</v-click>

<v-click>

### But: PHP doesn't have generics or even typed arrays

</v-click>

---

# PHPStan annotations to the rescue

```php
class MyClass {
    /**
     * @var Collection<int, BarEntity>  
     */
     private Collection $bars;
     
    /**
     * @param Traversable<int, float>|null $traversable
     * @return array<string, MyInterface>
     */
    function foo(?Traversable $traversable): array {
        // ...
    }
}

```

---

### Callables[^1]

```php {1-6|8-9|11-12|all}
/**
 * @param Closure(string $foo): void $callback
 */
function foo(callable $callback): void {
    $callback('test');
}

function invalid(int $foo): void {}
function valid(string $foo): void {}

foo(invalid(...));
foo(valid(...));
```

<div v-click="3">

| Line | Error                                                                                           |
|------|-------------------------------------------------------------------------------------------------|
| 11   | Parameter #1 $callback of function foo expects Closure(string): void, Closure(int): void given. |

</div>

[^1]: https://phpstan.org/r/65fa3f86-3f82-4d4a-a391-71fcb3b4827d

---

# Alternatives for IDEs without support

### Prefixed annotations 
```php
/**
 * @param array $param
 * @phpstan-param array<class-string<MyClass>, float> $params
 */
function foo(array $params) {
    // ...
}
```

<v-click>

### Psalm
```php
/**
 * @param array $param
 * @psalm-param array<int<10, 50>, array{a: bool, b?: string}> $param
 */
function foo(array $param) {
    // ...
}
```
</v-click>

<v-click>

### PHPStorm and VSCode understand extended `@param`, `@var` etc.

</v-click>

---
layout: center
---
# Use PHPStan in your project

---
layout: default
---

## Adding to project
```bash
composer require --dev `phpstan/phpstan`
```

## Configuration

```yaml
includes:
  - phpstan-baseline.neon
parameters:
  level: max
  checkMaybeUndefinedVariables: true
  inferPrivatePropertyTypeFromConstructor: true
  fileExtensions:
    - php
  paths:
    - src
    - test
```

## Run it
```bash
php ./vendor/bin/phpstan analyse
```

---
layout: default
lineNumbers: false
---

# Having lots of errors

```bash {1-3|5-9|11|all}
> phpstan analyse --memory-limit=2048M
Note: Using configuration file phpstan.neon.dist.
 710/710 [============================] 100%

 ------ ------------------------------------------------------------------------------------------------
 Line   src\Infrastructure\src\Model\DeviceType.php
 ------ ------------------------------------------------------------------------------------------------
 29     Property VV\Expressive\PKV\Infrastructure\Model\DeviceType::$deviceType has no type specified.
 ------ ------------------------------------------------------------------------------------------------

[ERROR] Found 229 errors
```

---

# phpstan-baseline.neon

- Feature to keep track of errors

<v-clicks>

- Generate / update baseline
```bash
phpstan analyse --generate-baaseline phpstan-baseline.neon
```

- Add baseline to configuration
```yaml
includes:
  - phpstan-baseline.neon
```

<p style="background: rgb(220 252 231/.8); padding: .5rem">No errors!</p>

</v-clicks>

---

# Rule level & `mixed` [^1]

<v-clicks>

- Between 0-9 (max)
    - higher = more rules (= more errors)
- PHP 8.0 introduced `mixed` for arbitrary output
- `array` -> implicit `array<string|int, mixed>`
  - Level 8+ treats `mixed` as own type that can't be matched -> ERROR

### Start with lower rule set and solve errors first
### Or: use baseline

</v-clicks>

[^1]: https://phpstan.org/user-guide/rule-levels

---

# Bonus: integrate into CI[^1]

### Junit
```bash
phpstan analyse -c phpstan.neon --error-format=junit
```

### GitHub Actions
```bash
phpstan analyse -c phpstan.neon --error-format=github
```

### Gitlab CI
```bash
phpstan analyse -c phpstan.neon --error-format=gitlab
```

More output types: `teamcity`, `json`, `prettyJson`, `checkstyle`, `raw`

[^1]: https://phpstan.org/user-guide/output-format

---

# Bonus: Extending PHPStan
There are lots of extension already available

<v-click>

### Example: service container
```php
/** @var \Psr\Container\ContainerInterface $container */
/** @var ??? $fooService */
$fooService = $container->get(FooService::class);
```

</v-click>

<v-click>

-> [`bnf/phpstan-psr-container`](https://github.com/bnf/phpstan-psr-container)

</v-click>
<v-click>

Add to phpstan.neon:
```yaml
includes:
    - vendor/bnf/phpstan-psr-container/extension.neon
```

</v-click>

---
layout: end

transition: fade-out
---
