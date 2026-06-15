# Lösung zu Blatt 05

## Überblick

In Blatt 05 bearbeite ich die Aufgaben zu ANTLR, Visitor; Äquivalenzklassen & Grenzwerte, Mocking.

## Bearbeitete Aufgaben

* Aufgabe 1.1: Syntaxhighlighting mit dem AntlrTokenCollector
* Aufgabe 1.2: Visitoren mit ANTLR und Pretty Printing
* Aufgabe 2.1: Äquivalenzklassen und Grenzwertanalyse
* Aufgabe 2.2: JUnit-Tests mit Mockito

## Repositories

Syntaxhighlighting:

`https://github.com/Exoriy/prog2-b04-syntaxhighlighting`

Cycle Chronicles:

`https://github.com/Exoriy/prog2-b05-cyclechronicles`


---

## Aufgabe 1.1: Syntaxhighlighting mit dem AntlrTokenCollector

In dieser Aufgabe habe ich die Methode `collectMatches` in der Klasse `AntlrTokenCollector` implementiert.

Zuerst wird der eingegebene Text an den generierten `MiniJavaLexer` übergeben. Der Lexer zerlegt den Text in einzelne Tokens, zum Beispiel Keywords, Strings, Characters und Kommentare.

Danach gehe ich durch den Token-Stream. Für die Tokens, die farbig dargestellt werden sollen, erstelle ich jeweils eine `HighlightRegion`. Dabei benutze ich die Start- und Endposition des Tokens sowie die passende Farbe aus `MiniJavaColours`.

Beim Endwert musste ich `getStopIndex() + 1` verwenden, weil der Endindex von ANTLR noch zum Token gehört, während das Ende einer `HighlightRegion` nicht mehr zum markierten Bereich gehört.

Annotationen werden etwas anders behandelt. Der Lexer erkennt das Zeichen `@` und den Namen der Annotation als zwei einzelne Tokens. Deshalb verbinde ich beide Teile zu einer gemeinsamen Region.

Eigene Implementierungen von `normalize` und `resolveConflicts` waren hier nicht nötig. Der Lexer liest den Text der Reihe nach und erzeugt normalerweise keine überlappenden Tokens. Beim `RegexHighlighter` konnten dagegen mehrere reguläre Ausdrücke dieselbe Textstelle erkennen. Deshalb mussten dort Konflikte extra gelöst werden.

Der `RegexHighlighter` war am Anfang einfacher zu verstehen, aber die regulären Ausdrücke und die Konflikte konnten schnell unübersichtlich werden. Für ANTLR werden zwar eine Grammatik und generierte Klassen benötigt, dafür ist die Zuordnung der Tokens im Collector klarer.

Ich habe die Implementierung mit verschiedenen Keywords, Kommentaren, Strings und einer Annotation im Editor ausprobiert. Anschließend habe ich das Projekt mit Gradle geprüft.



---

## Aufgabe 1.2: Visitoren mit ANTLR (Pretty Printing)

In dieser Aufgabe habe ich mit dem `MiniJavaLexer` und dem `MiniJavaParser` einen Parse-Tree für MiniJava-Code erstellt. Danach habe ich den vorhandenen `PrettyPrinterVisitor` ergänzt.

Ich habe die vier TODO-Methoden implementiert:

* `visitCompilationUnit`
* `visitClassBody`
* `visitBlock`
* `visitStatement`

Der Visitor geht durch den Parse-Tree und erzeugt den Quellcode neu. Klassenkörper und andere Blöcke werden nach einer öffnenden Klammer in einer neuen Zeile ausgegeben. Der Inhalt wird um eine Stufe eingerückt. Statements und Deklarationen mit einem Semikolon stehen jeweils in einer eigenen Zeile.

Beim Start des Programms wird in der Konsole gefragt, wie viele Leerzeichen für eine Einrückungsstufe benutzt werden sollen. Ich habe den Pretty Printer mit mehreren Beispielen getestet: einer einfachen Klasse, einer Klasse mit `if`, `else` und `while` sowie mit verschachtelten Blöcken.

Bei der Ausgabe fehlen die ursprünglichen Leerzeichen, Zeilenumbrüche und Kommentare. Die Whitespaces werden vom Lexer übersprungen. Kommentare liegen in einem versteckten Token-Kanal und gehören deshalb nicht zum normalen Parse-Tree, den der Visitor durchläuft. Der Visitor erzeugt stattdessen eigene Zeilenumbrüche und Einrückungen.

Die Formatierung innerhalb von Ausdrücken ist teilweise noch einfach. Das war für den Pflichtteil ausreichend, weil hauptsächlich die Struktur, die Blöcke und die Einrückung richtig dargestellt werden sollten.

Zur Prüfung habe ich folgende Befehle benutzt:

```bash
.\gradlew spotlessCheck
.\gradlew test
.\gradlew build
.\gradlew run
```


