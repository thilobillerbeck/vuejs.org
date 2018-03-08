---
title: Template-Syntax
type: guide
order: 4
---

Vue.js verwendet eine HTML-basierte Template-Syntax, mit der du das gerenderte DOM deklarativ an eine zugrundeliegende Vue-Instanz binden kannst. Vue.js-Templates sind gültiges HTML, das mit allen spezifikationskonformen Browsern und HTML-Parsern gelesen werden kann.

Intern kompiliert Vue die Templates zu Renderfunktionen für virtuelles DOM. Gepaart mit dem Reaktivitätssystem ist Vue bei Änderungen des Applikationszustands in der Lage, auf intelligente Art und Weise die kleinstmögliche Anzahl an Komponenten zu ermitteln, die neu gerendert werden müssen, und nur die minimal notwendigen Manipulationen am virtuellen DOM vorzunehmen.

Wenn du mit den Konzepten von virtuellem DOM vertraut bist und es vorziehst, die vollen Möglichkeiten von rohem JavaScript auszuschöpfen, kannst Du anstelle von Templates auch [direkt Renderfunktionen schreiben](render-function.html), optional mit JSX-Unterstützung.

## Interpolationen

### Text

Die einfachste Form von Datenbindung ist Textinterpolation mittels der sogenannten "Mustache"-Syntax (d.h. doppelte geschweifte Klammern):

``` html
<span>Nachricht: {{ msg }}</span>
```

Das Mustache-Tag wird ersetzt mit dem Wert der `msg`-Eigenschaft des zugehörigen Datenobjekts. Ausserdem wird es stets aktualisiert, wenn sich der Wert der `msg`-Eigenschaft des Datenobjekts verändert.

Du kannst mit der [`v-once`-Direktive](../api/#v-once) auch einmalige Interpolationen ausführen, bei denen keine Aktualisierung mehr stattfindet, wenn das Datenobjekt ändert. Behalte dabei aber im Auge, dass sich dies auf sämtliche Datenbindungen im selben Knoten auswirken wird:

``` html
<span v-once>Diese Nachricht wird nie mehr ändern: {{ msg }}</span>
```

### Rohes HTML

Bei der oben gezeigten Mustache-Syntax wird der Wert aus dem Datenobjekt als reiner Text interpretiert und nicht als HTMTL. Um stattdessen HTML auszugeben, musst du die `v-html`-Direktive verwenden:

``` html
<p>Mit Mustaches: {{ rawHtml }}</p>
<p>Mit der v-html-Direktive: <span v-html="rawHtml"></span></p>
```

{% raw %}
<div id="example1" class="demo">
  <p>Mit Mustaches: {{ rawHtml }}</p>
  <p>Mit der v-html-Direktive: <span v-html="rawHtml"></span></p>
</div>
<script>
new Vue({
  el: '#example1',
  data: function () {
  	return {
  	  rawHtml: '<span style="color: red">Dieser Text sollte rot sein.</span>'
  	}
  }
})
</script>
{% endraw %}

Der Inhalt von `span` wird mit dem Wert der `rawHtml`-Eigenschaft ersetzt und anschließend als rohes HTML interpretiert &mdash; allfällige Datenbindungen werden ignoriert. Beachte, dass Du `v-html` nicht zur Kombination partieller Templates verwenden kannst, da Vue keine zeichenkettenbasierte Template-Engine ist. Wir empfehlen stattdessen Komponenten als wiederverwendbaren Baustein für die UI-Komposition.

<p class="tip">Es kann sehr gefährlich sein, auf deiner Website beliebiges HTML dynamisch zu rendern, da dies leicht zu [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting)-Schwachstellen führen kann. Verwende HTML-Interpolation nur für vertrauenswürdige Inhalte, und **nie** für vom Benutzer bereitgestellte Inhalte.</p>

### Attribute

Innerhalb von HTML-Attributen können Mustaches nicht verwendet werden. Benutze stattdessen eine [`v-bind`-Direktive](../api/#v-bind):

``` html
<div v-bind:id="dynamicId"></div>
```

Im Fall von Booleschen Attributen, deren bloße Präsenz den Wert `true` impliziert, funktioniert `v-bind` etwas anders. Im folgenden Beispiel:

``` html
<button v-bind:disabled="isButtonDisabled">Schaltfläche</button>
```

Wenn `isButtonDisabled` den Wert `null`, `undefined` oder `false` hat, wird das `disabled`-Attribut im gerenderten `<button>`-Element überhaupt nicht vorhanden sein.

### Verwendung von JavaScript-Ausdrücken

Bis hierhin haben wir in unseren Templates nur Datenbindungen an einfache Eigenschaften (Objektschlüssel) betrachtet. Tatsächlich unterstützt Vue.js aber beliebige JavaScript-Ausdrücke in Datenbindungen:

``` html
{{ number + 1 }}

{{ ok ? 'JA' : 'NEIN' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

Diese Ausdrücke werden im Datenkontext der zugehörigen Vue-Instanz als JavaScript ausgewertet. Jede Datenbindung ist jedoch auf **einen einzigen Ausdruck** beschränkt, weshalb das folgende **NICHT** funktionieren wird:

``` html
<!-- Dies ist eine Anweisung und kein Ausdruck: -->
{{ var a = 1 }}

<!-- Programmflusssteuerung funktioniert auch nicht; verwende ternäre Ausdrücke: -->
{{ if (ok) { return message } }}
```

<p class="tip">Template-Ausdrücke werden in einer "Sandbox" ausgewertet und haben nur Zugriff auf jene globalen Symbole, die auf einer Whitelist stehen, darunter etwa `Math` und `Date`. Du solltest nicht versuchen, in Template-Ausdrücken auf benutzerdefinierte globale Variablen zuzugreifen.</p>

## Direktiven

Direktiven sind spezielle Attribute mit einem `v-`-Präfix. Es wird erwartet, dass ihre Werte jeweils **ein einziger JavaScript-Ausdruck** sind (mit Ausnahme von `v-for`, worüber wir später sprechen werden). Die Aufgabe einer Direktive ist es, reaktiv einen Seiteneffekt auf das DOM auszuüben, wenn der Wert ihres Ausdrucks ändert. Betrachten wir erneut das Beispiel aus der Einleitung:

``` html
<p v-if="seen">Jetzt siehst du mich</p>
```

Diese `v-if`-Direktive würde das `<p>`-Element je nach "Truthiness" des Werts von `seen` einfügen oder entfernen.

### Argumente

Einige Direktiven haben ein "Argument", welches hinter einem Doppelpunkt auf den Direktivennamen folgt. Zum Beispiel wird die `v-bind`-Direktive dazu verwendet, ein HTML-Attribut reaktiv zu aktualisieren:

``` html
<a v-bind:href="url"> ... </a>
```

Im obigen Beispiel weiss die `v-bind`-Direktive aufgrund des Arguments `href`, dass das `href`-Attribut des Elements an den Wert des Ausdrucks `url` gebunden werden soll.

Ein weiteres Beispiel dafür ist die `v-on`-Direktive, welche DOM-Ereignisse behandelt:

``` html
<a v-on:click="doSomething"> ... </a>
```

Hier ist das Argument der Name des Ereignisses, das behandelt werden soll: `click`. Über Ereignisbehandlung werden wir ebenfalls noch genauer sprechen.

### Modifizierer

Modifizierer sind spezielle, durch einen Punkt eingeleitete Suffixe, welche darauf hinweisen, dass eine Direktive auf eine bestimmte spezielle Art gebunden werden soll. Beispielsweise sagt ein `.prevent`-Modifizierer der `v-on`-Direktive, dass für ausgelöste Ereignisse ein Aufruf von `event.preventDefault()` erfolgen soll:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

Du wirst später noch weitere Beispiele von Modifizierern [für `v-on`](events.html#Ereignismodifizierer) und [für `v-model`](forms.html#Modifizierer) sehen, wenn wir diese Features näher betrachten.

## Kurzformen

Das `v-`-Präfix dient als visuelles Erkennungsmerkmal für Vue-spezifische Attribute in deinen Templates. Das ist zwar hilfreich, wenn du Vue.js verwendest, um dynamisches Verhalten zu bestehendem Markup hinzuzufügen; aber auf Dauer fühlt es sich für ein paar oft verwendete Direktiven langatmig an. Des weiteren wird ein einfach zu erkennendes `v-`-Präfix weniger wichtig, wenn du eine [SPA](https://en.wikipedia.org/wiki/Single-page_application) baust, bei der Vue.js alle Templates handhabt. Deshalb kennt Vue.js für zwei der am häufigsten verwendeten Direktiven, `v-bind` and `v-on`, Kurzformen:

### `v-bind`-Kurzform

``` html
<!-- Vollständige Syntax -->
<a v-bind:href="url"> ... </a>

<!-- Kurzform -->
<a :href="url"> ... </a>
```

### `v-on`-Kurzform

``` html
<!-- Vollständige Syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- Kurzform -->
<a @click="doSomething"> ... </a>
```

Zwar sehen sie etwas anders aus als normales HTML, aber `:` und `@` sind Zeichen, die in Attributnamen erlaubt sind, und alle von Vue.js unterstützten Browser können sie korrekt verarbeiten. Ausserdem erscheinen sie letztendlich nicht mehr im gerenderten Markup. Die Kurzformen sind völlig optional, aber wahrscheinlich wirst du sie schätzen lernen, wenn du später noch mehr über ihre Verwendung erfährst.
