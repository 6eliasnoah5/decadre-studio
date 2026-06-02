# Décadre Studio — Website

Deploy-ready static site for **decadre.studio**. Two pages, zero dependencies
beyond Google Fonts.

```
site/
├── index.html         ← landing page
├── impressum.html     ← legal subpage
├── README.md          ← this file
├── videos/            ← place your video files here
└── images/            ← place your image files + favicons here
```

---

## Videos austauschen

**Pfad:** `videos/`
**Format:** MP4, H.264, max 5 MB pro Datei, 1920×1080 oder kleiner
**Poster:** JPG, gleiche Dimensionen wie das Video (für den ersten Frame)

Die Hover-Logik ist im JS bereits eingebaut — beim Mouseover spielt das
Video, beim Verlassen pausiert es. Du musst nur die Dateien an die
richtige Stelle legen. Die `<video>`-Tags sind schon korrekt mit `muted`,
`loop`, `playsinline` und `poster`-Attribut konfiguriert.

**Slot-Übersicht** — erwartete Dateinamen:

| Slot           | Video             | Poster             | Verwendung                  |
| -------------- | ----------------- | ------------------ | --------------------------- |
| Hero           | `hero.mp4`        | `hero.jpg`         | Fullscreen Hintergrund      |
| Work — slot 01 | `work-01.mp4`     | `work-01.jpg`      | Groß links oben (8 col)     |
| Work — slot 02 | `work-02.mp4`     | `work-02.jpg`      | Klein rechts oben (4 col)   |
| Work — slot 03 | `work-03.mp4`     | `work-03.jpg`      | Klein links unten (5 col)   |
| Work — slot 04 | `work-04.mp4`     | `work-04.jpg`      | Groß rechts unten (7 col)   |

Falls eine Datei fehlt: der Slot zeigt einfach den schwarzen Hintergrund
mit dem `[ video — slot NN ]`-Label. Nichts bricht.

Im HTML findest du jeden Slot über einen Kommentar in der Form:

```html
<!-- VIDEO HIER: src="videos/work-01.mp4", poster="videos/work-01.jpg" -->
```

---

## Texte ändern

Suche im `index.html` nach `<!-- TEXT HIER` — jede Stelle, an der du
eigenen Text einsetzen sollst, ist so markiert. Aktuell stehen überall
`[ platzhalter ]` als Lückenfüller — niemals Lorem Ipsum.

**Übersicht aller Platzhalter:**

| Sektion              | Was hin muss                                      |
| -------------------- | ------------------------------------------------- |
| `<meta description>` | SEO-Beschreibung, 1–2 Sätze                       |
| `og:description`     | Social-Share-Beschreibung (kann gleich sein)      |
| Hero — `.hero__title` | 3 Zeilen Display-Text (z.B. drei Schlagworte)    |
| Hero — `aria-label`   | Kurze Beschreibung des Hero-Videos               |
| [ 01 ] Work — Titel  | Display-Headline für die Work-Sektion             |
| Work-Slots — `.slot__title` | Projekt-Name pro Slot                       |
| Work-Slots — `.slot__meta`  | Kategorie · Jahr (z.B. "brand film · twentytwentyfive") |
| [ 02 ] Services      | je 1 kurze Beschreibung pro Service-Zeile         |
| [ 03 ] About — Titel | Display-Headline                                  |
| [ 03 ] About — Body  | 2–3 Absätze in Kleinbuchstaben                   |
| [ 04 ] Contact       | E-Mail-Adresse in `<a href="mailto:…">`           |
| `impressum.html`     | TMG-Angaben, vertretungsberechtigt, USt-ID, etc.  |

**Stil-Regeln** beim Schreiben (siehe auch `../README.md` im Projekt-Root):

- alles kleinbuchstaben außer großen Display-Headlines
- niemals "we craft / we believe / we are"
- jahre wenn passend ausgeschrieben: `twentytwentysix`
- aktionen in eckigen klammern: `[ → contact ]`
- captions / meta in JetBrains Mono klein

---

## Design anpassen

Alle gestaltungs-relevanten Werte stehen am Anfang von `<style>` im
`:root`-Block. Tausch dort einen Wert aus, und alles passt sich an.

```css
:root {
  /* colors */
  --color-bg: #F4F2EE;     /* warmes Offwhite, jede Fläche             */
  --color-fg: #111111;     /* tiefes Schwarz, jeder Text + jede Marke  */

  /* fonts */
  --font-display: "Inter Tight", …  /* Display-Headlines                 */
  --font-body:    "Inter", …        /* Fließtext                         */
  --font-mono:    "JetBrains Mono", … /* Klammern, Nav, Meta, Captions  */

  /* layout */
  --max-width:             1440px;  /* max. Container-Breite             */
  --gutter:                24px;    /* Grid-Spaltenabstand               */
  --side-padding-desktop:  32px;    /* Seitenränder Desktop              */
  --side-padding-mobile:   20px;    /* Seitenränder Mobile (≤768px)      */
  --section-spacing:       160px;   /* vertikaler Abstand pro Sektion    */
}
```

**Häufige Anpassungen:**

- **Weniger Abstand zwischen Sektionen** → `--section-spacing: 120px`
  (Default ist `160px`; mobile fällt automatisch auf `96px` zurück.)
- **Breitere Seite** → `--max-width: 1600px`
- **Andere Schrift** → `--font-display` und `--font-body` tauschen.
  Vergiss nicht, im `<link>` oben den Google-Fonts-URL anzupassen.

**Was du nicht ändern solltest, ohne Rücksprache:**

- Die Farb-Regel: das System nutzt **nur** `--color-bg` und `--color-fg`.
  Kein Akzentton, kein Grau-Mittelton — Abstufungen entstehen ausschließlich
  über `opacity` auf `--color-fg` (`--color-fg-60`, `--color-fg-40`,
  `--color-fg-15` sind Alpha-Derivate, keine eigenen Farben).
- Die Bracket-Manier: `[ work ]`, `[ → contact ]`, `[ 01 ]` — durchgehend.
- Vertikales Rhythmus-Minimum: Sektionen min 160px Whitespace auseinander.

---

## Deploy auf Vercel in 3 Schritten

### Schritt 1 — GitHub-Repo

1. Auf [github.com](https://github.com) neues, leeres Repo anlegen
   (z.B. `decadre-site`, privat oder öffentlich).
2. Inhalt des `site/`-Ordners (`index.html`, `impressum.html`, `README.md`,
   `videos/`, `images/`) als Root des Repos hochladen.
   Per CLI: `git init && git add . && git commit -m "init" && git remote add origin … && git push -u origin main`.

### Schritt 2 — Vercel-Import

1. Auf [vercel.com](https://vercel.com) einloggen.
2. **New Project** → das GitHub-Repo auswählen und importieren.
3. Framework Preset: **Other**. Root Directory: `./`. Keine Build-Settings,
   kein Output-Directory. **Deploy** klicken.
4. Nach ~30 Sekunden ist die Seite unter `decadre-site.vercel.app` live.

### Schritt 3 — Custom Domain

1. Vercel → Project → **Settings** → **Domains** → eigene Domain eintragen
   (z.B. `decadre.studio`).
2. Beim Domain-Registrar (z.B. namecheap, ionos, hetzner) im DNS einen
   CNAME- oder A-Record auf Vercels Werte setzen — Vercel zeigt sie an,
   typisch `cname.vercel-dns.com` für www und `76.76.21.21` für apex.
3. **DNS-Propagation abwarten** — meist 10–60 Minuten, max. 24h.
   Sobald Vercel ein grünes ✓ zeigt, ist HTTPS automatisch aktiv.

---

## Lokal anschauen

Einfach `index.html` im Browser öffnen — oder einen lokalen Server starten:

```bash
cd site && python3 -m http.server 8000
# → http://localhost:8000
```

Letzteres ist nötig, sobald du echte Video-Dateien im `videos/`-Ordner hast
(manche Browser blockieren `<video src="…">` über `file://`).
