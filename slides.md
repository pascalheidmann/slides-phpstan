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

<v-clicks>

- Return types
```php
function foo(): MyClass {}
```

- Type hints
```php
function bar(string $myString) {}
```

- Doc block
```php
/**
* @param string $param
 */
function baz($param) {}
```

\+ stubs & phpstan extensions

</v-clicks>

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

## Fixed type array
```java
// array of 5 strings
myStringArray = new String[5];

// ...

function foo(String[] stringArray) {}

foo(myStringArray);
```

## Generic arrays
```java
function foo<T>(T[] genericArray) {}

foo<String>(myStringArray);
```

## But: PHP doesn't have generics or even typed arrays 

---
layout: default
---


---
layout: end

transition: fade-out
---
