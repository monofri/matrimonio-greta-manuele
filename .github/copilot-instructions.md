# Istruzioni per Copilot - Matrimonio Greta & Manuele

## Panoramica del Progetto
Questo è un **sito web statico per un matrimonio** (22 Maggio 2026). Stack: HTML5 + CSS3 vanilla + JavaScript puro. No framework, no build tool.

## Architettura

### File Structure
- **index.html** — Unico file HTML; contiene tutta la struttura e JavaScript inline
- **styles_new.css** — Unico file CSS; design system con colori salvia, animate fondos fiorali
- **matrimonio-greta-manuele.ics** — Evento calendario (.ics) scaricabile
- **immagini/** — Cartella risorse (SVG animati, foto, webp)

### Sezioni Principali (HTML)
1. **Navbar sticky** — Menu mobile-first con hamburger checkbox-toggle (no jQuery)
2. **Hero section** — Titolo, countdown interattivo, CTA buttons
3. **Main sections** — Card layout per: Cerimonia, Rinfresco, Programma (timeline), RSVP, Lista Nozze, Noi
4. **Footer** — Testo centro con stile navbar

## Design System

### Colori (CSS Custom Properties)
```css
--ink: #2a2a2a        /* Testo principale */
--olive: #6b705c      /* Verde salvia - accenti, heading */
--sage: #a5a58d       /* Verde chiaro - borders */
--ivory: #faf7f2      /* Sfondo cremoso */
--card: rgba(255,255,255,.85)  /* Card semi-trasparente */
```

### Tipografia
- **Headings** — "Playfair Display" serif (elegante, matrimoniale)
- **Body** — "Inter" sans-serif (leggibile, moderno)
- Importati via Google Fonts

### Responsive
- **Breakpoint mobile** — 768px (hamburger menu attivato)
- **Extra-small** — 520px (timeline più compatta)
- **Altro** — Padding/margin fluidi con clamp() per fluidità

## Pattern Chiave

### 1. Menu Mobile (Checkbox Hack)
```html
<input type="checkbox" id="menu-toggle" />
<label for="menu-toggle" class="hamburger">...</label>
<ul class="menu">...</ul>

<script>
  document.querySelectorAll('.menu a').forEach(link => {
    link.addEventListener('click', () => {
      document.getElementById('menu-toggle').checked = false;  // Chiudi dopo click
    });
  });
</script>
```
**Non usare librerie** — CSS-only toggle con JavaScript plain per chiudere il menu.

### 2. Countdown Timer
- Target: `2026-05-22T15:00:00+02:00` (CEST)
- Aggiorna ogni 1000ms, mostra giorni/ore/min/sec
- Quando < 0, mostra "È il grande giorno! 🎉"
- **Modifica target date qui:** [index.html](index.html#L256)

### 3. Design Romantico (Layering)
```css
body::before  /* SVG pattern floreale */
body::after   /* Radial gradient overlay cremoso */
.hero::before /* Velo semi-trasparente + blur */
```
Crea profondità senza pesare il DOM. Keep it simple — no Three.js.

### 4. Animazioni CSS
- **Button hover** — `translateY(-2px)` + fade
- **Hero SVG** — `floatFaster 14s` infinite (movement lento e romantico)
- **Foto cerchio** — Scale 1.03 on hover
- Tutte le transizioni `.18s ease` per coerenza

### 5. Timeline Programma
- Griglia 3-colonne: `tempo | pallino | contenuto`
- Linea verticale CSS `:before` su `.timeline`
- Mobile (< 520px) — colonne ridotte per reattività

## Developer Workflow

### Modifiche Comuni
- **Cambiar data/ora matrimonio** — Aggiorna:
  - `index.html` → script countdown (riga ~256)
  - `matrimonio-greta-manuele.ics` → DTSTART/DTEND
  - Testo nelle sezioni "Cerimonia"
  
- **Aggiungere/rimuovere sezioni** — Mantieni pattern `<section id="..." class="card">`; CSS applica automaticamente

- **Foto** — Usa `.foto-cerchio` per crop circolare con border sage + shadow

- **Link esterni** — Sempre con `target="_blank" rel="noopener noreferrer"` (sicurezza)

### Testing
- **Desktop** — Chrome/Safari full-width
- **Mobile** — Resize browser a 375px, test hamburger menu
- **Countdown** — Apri browser console, verifichi che non errori JavaScript

### Deployment
- Nessun build step — Serve direttamente gli `.html`, `.css`, `.js`, immagini
- GitHub Pages: commit su `main`, attiva Pages da settings
- File `.ics` scaricabile — Aggiungi al calendario

## Convenzioni di Codice

### HTML
- Usa ID per link interni (`#cerimonia`, `#rsvp`)
- Emoji in `<h2>` come parte dello stile (non troppo)
- `aria-label` su hamburger, countdown, SVG decorativi `aria-hidden="true"`

### CSS
- **Custom properties** sopra (`:root`)
- **Selettori semplici** — no nesting, no preprocessor
- **Ombreggiature coerenti** — `var(--shadow)` vs `--shadow-soft`
- **z-index strategici** — navbar 999, hero content z:1
- **Backdrop-filter blur** — Per vetro opaco (navbar, hero, card)
- No hardcoded colori — Usa variabili CSS

### JavaScript
- **Plain vanilla** — no jQuery, no framework
- Funzioni corte e leggibili
- `const`/`let`, non `var`
- Commenti su logica non-ovvia (es: countdown math)

## Troubleshooting

| Problema | Soluzione |
|----------|-----------|
| Countdown non aggiorna | Verifica console per errori; check target date syntax |
| Menu non si chiude su mobile | Ensure script loop chiude checkbox all `.menu a` click |
| Sfondo fiorale non appare | Verifica path `immagini/floral-pattern.svg` esiste e SVG è valido |
| Foto sfocate | Usa `.foto-cerchio` class; assicura immagine è alta risoluzione |
| Navbar flickering | Check z-index 999, backdrop-filter non conflitti con altre regole |

## File Chiave da Conoscere
- [index.html](index.html) — Struttura HTML + JavaScript countdown
- [styles_new.css](styles_new.css) — Tutto lo stile (no separazione)
- [immagini/](immagini/) — Risorse visive
- [matrimonio-greta-manuele.ics](matrimonio-greta-manuele.ics) — Evento calendario

---
**Per agenti AI:** Focus su mantenere semplicità vanilla, non aggiungere dipendenze. Testa countdown e responsive prima di commit.
