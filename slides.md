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
# use UnoCSS
css: unocss

titleTemplate: '%s'

layout: cover
---
# PHPStan

Pain you gotta love

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
layout: center

transition: fade
---

# Who has used PHPStan?

<div v-click>
Or psalm?
</div>


[//]: # (---)
[//]: # (layout: section)
[//]: # (---)

[//]: # (# Agenda)
[//]: # ()
[//]: # (<Toc />)

---
layout: section
---

# What is PHPStan

<h2 class="text-center">
    <span v-click>
    PHP
    </span>
    <span v-click>
    + Static Analysis
    </span>
    <span v-click>
    = PHPStan
    </span>
</h2>

<div class="text-center" v-click>
    <h3>Let our computer do the heavy work and test your code automatically.</h3>
    <p>No need to write tests!</p>
</div>

---
layout: default
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
layout: default
---

# How does this work?

- Return types
```php
function foo(): MyClass {}
```

<v-click>

- Type hints
```php
function bar(string $myString) {}
```

</v-click>
<v-click>

- Doc block
```php
/**
* @param string $param
 */
function baz($param) {}
```
</v-click>
<v-click>

\+ stubs & phpstan extensions

</v-click>

---
layout: default
---
# Arrays

- In PHP a special vehicle
  - Index based list of things (other languages: `list`, `array`)
  - Associative array (other languages: `map`, `object`, `hash`)
  - Combination of both... 🥳

---
layout: default
---


---
layout: default
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
layout: default
---
# PHPStan annotations to the rescue

```php
/**
 * @return array<int, MyClass>
 */
function foo(): array {
    // ...
}
```

<v-click>

### Prefixed annotations 
```php
/**
 * @phpstan-param array<class-string<MyClass>, float> $params
 */
function foo(array $params) {
    // ...
}
```

</v-click>

<v-click>

### Psalm
```php
/**
 * @psalm-param array<int<10, 50>, array{a: bool, b?: string}> $param
 */
function foo(array $param) {
    // ...
}
```
</v-click>

---
layout: default
---

### Callables[^1]

```php {1-6|8-9|11-12|all}
/**
 * @phpstan-param Closure(string $foo): void $callback
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
layout: end

transition: fade-out
---
