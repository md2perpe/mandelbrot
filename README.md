# Mandelbrotzoom

En webbsida där man kan zooma in i Mandelbrotmängden. Allt finns i `index.html` – öppna filen direkt i webbläsaren, inga beroenden behövs.

## Användning

- **Scrolla** eller **nyp** (pekskärm) för att zooma runt muspekaren/fingrarna.
- **Dra** för att panorera.
- **Dubbelklicka** för att zooma in, **Skift + dubbelklick** för att zooma ut.
- Tangenter: `+` / `−` zoomar, piltangenterna panorerar, `R` återställer.
- Panelen styr antal iterationer (automatiskt eller manuellt), färgpalett, färgtäthet och färgskift.
- Aktuell position sparas i adressen, så **Kopiera länk** delar exakt den vy du tittar på. **Spara bild** laddar ner en PNG.

## Teknik

Beräkningen görs i en WebGL 2-fragmentshader. För att kunna zooma djupare än vad 32-bitars flyttal i grafikkortet klarar (ca 10⁵×) används *perturbationsteori*:

1. En referensbana Zₙ för bildens mittpunkt beräknas i JavaScript med 64-bitars precision och laddas upp som en flyttalstextur.
2. Varje pixel itererar bara sin avvikelse δ från referensbanan: δₙ₊₁ = 2Zₙδₙ + δₙ² + δc, vilket fungerar bra i 32-bitars precision.
3. *Ombasering* (när |Zₙ + δₙ| < |δₙ| eller referensbanan tar slut) undviker de "glitchar" som annars uppstår.

Det ger förstoringar på upp till ungefär 10¹² gånger. Därefter räcker inte JavaScripts dubbla precision för mittpunkten.
