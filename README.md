# Gewerbepark Lonsee – Website

Statische Website für **gewerbepark-lonsee.de** (Gewerbepark 1, 2 und 3 in Lonsee).
Reines HTML, CSS und JavaScript – kein Build-Schritt, keine Abhängigkeiten, keine externen Schriften oder Tracker.

## Inhalt

| Datei / Ordner | Zweck |
|---|---|
| `index.html` … `kontakt.html` | Die Seiten (Startseite, Standorte, Gewerbepark 1–3, Mieter mit „Wer sitzt wo?“, Aktuelles/Chronik, Lage, Kontakt) |
| `impressum.html`, `datenschutz.html` | Impressum und Datenschutzerklärung |
| `assets/css/style.css` | Gesamtes Styling (Farben oben als Variablen) |
| `assets/js/main.js` | Menü, Mieter-Filter, Mieter-Finder, Formulare, „Adresse kopieren“ |
| `assets/img/`, `assets/logos/`, `assets/fonts/` | Bilder (WebP + JPG/PNG), Mieter-Logos, Schrift (TeX Gyre Heros, selbst gehostet) |
| `functions/api/kontakt.js` | Logik für Kontaktformular und Flächen-Alarm (E-Mail-Versand) |
| `worker/index.js`, `wrangler.jsonc`, `.assetsignore` | Einrichtung als Cloudflare Worker |
| `_headers` | Sicherheits- und Cache-Header für Cloudflare |
| `404.html`, `sitemap.xml`, `robots.txt`, Favicons | Standard-Dateien |

## Lokal ansehen

```bash
python3 -m http.server 8000
# dann http://localhost:8000 öffnen
```
(Das Formular funktioniert lokal nicht – es braucht die Cloudflare Function.)

## Veröffentlichen (Cloudflare Worker)

1. Cloudflare → Workers & Pages → **Create** → Import a repository → `peterscheel/gewerbepark-lonsee`. Der Worker heißt **gewerbepark-lonsee** (wie in `wrangler.jsonc`), Root directory `/`, kein Build-Befehl.
2. Settings → Variables and Secrets → **Secret** `RESEND_API_KEY` anlegen. `CONTACT_TO` und `CONTACT_FROM` stehen bereits in `wrangler.jsonc`.
3. Settings → Domains & Routes → Custom Domain `gewerbepark-lonsee.de` und `www.gewerbepark-lonsee.de`.
4. Jeder Push auf `main` wird automatisch veröffentlicht.

## Kontaktformular (Resend)

- Empfänger: `gerrit.korner@zaunteam.de`, Absender: `kontakt@gewerbepark-lonsee.de`
- In Resend die Domain `gewerbepark-lonsee.de` hinzufügen und die angezeigten DNS-Einträge (DKIM, SPF/MX für `send`) in Cloudflare eintragen.

## Caching

CSS und JS werden mit `?v=JJJJMMTTHHMM` eingebunden. Nach jeder Änderung die Version in allen HTML-Dateien erhöhen.
