# AVD Experience Tool — GitHub Pages

Deze versie is volledig statisch en kan rechtstreeks op GitHub Pages worden gepubliceerd. Er is geen backend, buildstap, `npm install` of API-sleutel nodig.

## Bestandsstructuur

```text
index.html
assets/
  app.js
  measurement.mjs
  regions.mjs
  style.css
  avd.css
tests/
  measurement.test.mjs
  regions.test.mjs
README.md
LICENSE
.nojekyll
```

## Publiceren op GitHub Pages

1. Maak een publieke of (indien jouw GitHub-abonnement dat ondersteunt) private GitHub-repository aan.
2. Upload **de inhoud van deze ZIP** naar de **root** van de repository, zodat `index.html` in de hoofdmap staat en niet in een extra submap.
3. Ga in GitHub naar **Settings → Pages**.
4. Kies bij **Build and deployment → Source** voor **Deploy from a branch**.
5. Selecteer de branch **main** en de map **/(root)**, en klik op **Save**.
6. Open na publicatie het door GitHub Pages weergegeven adres, meestal `https://<gebruikersnaam>.github.io/<repositorynaam>/`.

De CSS en modules gebruiken relatieve paden (`./assets/...`). Dit werkt ook wanneer GitHub Pages de website onder een repositorynaam publiceert. De lege `.nojekyll` schakelt verwerking door Jekyll uit.

## Lokaal testen

Open `index.html` niet rechtstreeks via `file://` omdat JavaScript-modules normaal via HTTP(S) moeten worden geladen. Start bijvoorbeeld in de hoofdmap een lokale webserver:

```bash
python -m http.server 8000
```

Open vervolgens `http://localhost:8000`.

Voor de meegeleverde automatische tests (Node.js vereist):

```bash
node --test tests/*.test.mjs
```

## Werking

- De meting begint automatisch bij het openen van de pagina.
- Per regio wordt één opwarmverzoek verstuurd gevolgd door vijf metingen; de tabel toont de mediane gemeten HTTPS-responstijd.
- **Restart lookup** annuleert de huidige ronde en begint opnieuw.
- **Export** downloadt de beschikbare resultaten als JSON.
- Verborgen browsertabs pauzeren de meting. Samples rond zichtbaarheidsovergangen worden opnieuw gedaan.
- Regio's zonder geverifieerd testendpoint krijgen geen verzonnen meetwaarde.

De regio-indeling staat in `assets/regions.mjs`. De opgegeven regionale gateway-endpoints zijn afkomstig uit de oorspronkelijke bron en werden volgens die bron op 28 oktober 2026 geverifieerd; Microsoft kan deze endpoints wijzigen.

### Meetbeperkingen

De meting is een HTTPS-responstijd vanuit de browser, **geen echte RDP- of ICMP-RTT** naar een sessiehost. Browserbeveiliging, proxy, DNS, TLS of VPN kunnen het resultaat beïnvloeden. Door `no-cors` kan de browser de HTTP-status van een ontvangen opaque response niet uitlezen; een response bevestigt daarom niet dat de dienst gezond is.

Alle metingen gebeuren client-side. Meetresultaten blijven in de browser tot de gebruiker ze eventueel exporteert. De externe gatewayservers zien het IP-adres waarvan de verzoeken komen.

**Externe middelen:** het logo en de achtergrondafbeelding worden nog steeds geladen vanaf de publieke Azure Blob Storage van de oorspronkelijke website. Die afbeeldingen zijn daarom niet in `assets/` opgeslagen en vereisen toegang tot die externe host.

## Licentie

MIT — zie [LICENSE](./LICENSE). Oorspronkelijke tool en vormgeving door Justin Verstijnen.
