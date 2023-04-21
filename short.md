---
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

aspectRatio: '1/1'
canvasWidth: 500

layout: center
---

# 8 Gründe für PHPStan in deinem Projekt

---
layout: center
---

## 1. PHP ist eine interpretierte Sprache 🤔

Programmcode wird zeilenweise ausgeführt

→ Fehler werden erst beim Ausführen erkannt

---
layout: center
---

## 2. PHPStan analysiert den Code statisch lange vor der Ausführung ⏰

Direkt beim Entwickeln kann PHPStan bereits sagen, ob Code-Pfade Fehler werfen werden.

---
layout: center
---

## 3. PHPStan analysiert die komplette Codebasis 🔎

Keine Datei wird vergessen

---
layout: center
---

## 4. PHPStan versteht die Codebasis 💡

Typo im Klassennamen, nicht vorhandene Funktion, potenziell nicht definierte Variable?

Kein Problem: PHPStan erkennt die möglichen Daten-Typen und ihre Eigenschaften 😎

---
layout: center
---

## 5. PHPStan lässt sich super in die Build-Pipeline einbauen 🏗️

Reports im JUnit, Gitlab CI, teamcity oder auch JSON Format

---
layout: center
---

## 6. Verschiedene Level für unkomplizierten Einstieg ⚙️

Niedriges Level = weniger Checks = sanfter Einstieg

---
layout: center
---

## 7. Baseline mit aktuellem Stand der Codequalität 🤫

Aktueller Stand wird in der Baseline festgehalten und nur Regressionen reported

---
layout: center
---

## 8. Erweiterbarkeit ➕

Es gibt viele Erweiterungen, die auch Sonderfälle für PHPStan verständlich machen

---
layout: center
---

# Fazit 🚀

PHPStan hilft dir
- Fehler zu erkennen bevor sie beim Kunden auftreten
- Kompatibilität von deinem Code und neuen PHP & Library Versionen zu prüfen
- Regressionen zu vermeiden
