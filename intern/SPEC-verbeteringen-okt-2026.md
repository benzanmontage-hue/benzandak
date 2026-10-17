# SPEC — benzandak.nl verbeterronde (okt 2026)

Status: v2 — vastgesteld na team-review (SEO, toegankelijkheid, UX/copy, claims/juridisch, techniek)
Repo: benzanmontage-hue/benzandak · statische site op Netlify · branch `verbeteringen-okt-2026`

## Uitgangspunten
- Geen feiten verzinnen. Wat alleen de eigenaar kan bevestigen staat onder "Open vragen" en wordt NIET als placeholder live gezet; onbevestigde claims worden afgezwakt of weggehaald.
- Huisstijl (groen/goud/oranje, logo, Inter, u-vorm) blijft. Oranje wordt alleen donkerder waar contrast dat vereist.
- Elke eis heeft een acceptatiecriterium (AC) dat met grep, code-inspectie of een browser-test te controleren is.

## A. Kritiek

R1 Bellen = bellen
- AC: elke link met tekst "Bel…" of met het telefoonnummer als linktekst heeft `href="tel:+31657079276"` en geen `target`. `wa.me` alleen bij links die WhatsApp noemen of het WhatsApp-icoon zijn.
- AC: mobiele balk = Bellen (tel:) · WhatsApp · Offerte.

R2 ISDE-subsidie correct (bron RVO, peildatum 2026)
- AC: geen "subsidie tot 30%" / "30% van de kosten" op welke pagina dan ook.
- AC: tekst noemt €16,25/m² (1 maatregel), €32,50/m² (2+), +€5/m² biobased, min 20 / max 200 m², voorwaarden (eigen woning, uitgevoerd door bedrijf, Rd ≥ 3,5) en "onder voorbehoud", met link naar rvo.nl.
- AC: calculator: slider 20–200 m², subsidie = m² × 16,25 (afgerond); 60 m² → €975; 200 m² → €3.250.
- AC: geen hard-coded startwaarden in HTML die afwijken van wat het script berekent; de terugverdientijd-tegel is weg (kosten/besparing niet onderbouwd).
- AC: calculator-knop belooft geen e-mail ("Neem deze berekening mee in mijn aanvraag").

R3 Energiebesparing niet absoluut
- AC: geen "Bespaar tot 30%" in title, meta, og, H1 of diensttekst. Besparing wordt beschreven als indicatie met bron (Milieu Centraal) of zonder percentage.

R4 Reviews, scores en keurmerken eerlijk
- AC: geen `aggregateRating` of `review` in JSON-LD.
- AC: geen "4.9", "42 reviews" of score zonder bronlink op de site.
- AC: geen "Gecertificeerd" zonder naam van certificaat; FAQ-vraag gaat alleen over verzekering.
- AC: geen "100% Tevredenheidsgarantie" zonder voorwaarden.
- AC: review-formulier: geen voorgeselecteerde 5 sterren; tekst klopt met wat het formulier vraagt.

R5 Onderhoudscontract consistent en rechtsgeldig
- AC: pakketinhoud Basis/Comfort/Premium is woordelijk gelijk op index.html en onderhoudscontract.html.
- AC: geen onbegrensd "Reparaties inbegrepen", geen "24/7", geen "nooit meer zorgen".
- AC: opzegtekst: "Na het eerste jaar maandelijks opzegbaar." + 14 dagen herroepingsrecht bij online afsluiten genoemd op onderhoudscontract.html.
- AC: "Welk onderhoud" (niet "Welke"); losse prijzen gelijk aan index (schoonmaken €4–8/m², kleine reparatie vanaf €150).

R6 Cookies & privacy (AVG/Telecomwet)
- AC: zonder keuze of na "Weigeren": geen request naar connect.facebook.net / facebook.com / google.com/maps. Geen `<noscript>` pixel.
- AC: Meta Pixel laadt alleen na "Accepteren" én als een echte ID is ingevuld.
- AC: Google Maps is een klik-om-te-laden placeholder (of laadt pas na accepteren).
- AC: footer-link "Cookie-instellingen" heropent de banner; localStorage-toegang in try/catch.
- AC: privacy.html noemt Formspree (VS), Meta, Google (Maps/Fonts), Netlify, grondslagen per doel, publicatie van reviews (naam+woonplaats), recht op klacht bij de AP, datum "oktober 2026".

R7 Interne tools niet indexeren
- AC: offerte-generator.html en inspectie-rapport.html hebben `<meta name="robots" content="noindex, nofollow">` en een `X-Robots-Tag: noindex` header in netlify.toml; niet in sitemap.

R8 Geen soft-404's, geen privé-bestanden publiek
- AC: catch-all `/* → /index.html 200` is weg; `404.html` bestaat (huisstijl, links naar home/bellen).
- AC: abonnement-contract.txt is niet meer bereikbaar (verplaatst naar `intern/`, dat met status 404 wordt afgeschermd).

R9 Formulieren werken zonder risico op leadverlies
- AC: hoofdformulier heeft `action="https://formspree.io/f/xgoqnzqn" method="POST"` als fallback; geen persoonsgegevens in de URL.
- AC: onderhoudscontract.html verstuurt via Formspree (fetch), toont "Bedankt" alleen bij succes, inline foutmelding met telefoonnummer, geen alert(), geen mailto.
- AC: elke form heeft een Nederlandse succes- én foutmelding (`role="status"` / `role="alert"`) met het telefoonnummer in de fout.

## B. SEO

R10 Head: title ≤ 60 tekens met "Dakdekker Arnhem"; meta description ≤ 155 zonder emoji/30%/4.9; één og:description; geen meta keywords; og:url = canonical (met slash); hreflang weg.
R11 JSON-LD: `RoofingContractor`, `@id` met slash, geen `sameAs` naar wa.me, geen SearchAction.
R12 Koppen: precies één H1 per pagina; homepage-H1 bevat "Dakdekker" en "Arnhem"; geen overgeslagen niveaus (process/footer); geen emoji in koppen.
R13 Nieuwe pagina's met unieke, bruikbare inhoud (geen verzonnen projecten/reviews/cijfers), elk met canonical, meta, H1, interne links, contact-CTA en opgenomen in sitemap:
- `/daklekkage/`, `/dakisolatie/`, `/plat-dak/`
- Footer-dienstlinks en de dienstkaarten verwijzen ernaar.
- Geen dunne stadspagina's (Nijmegen alleen na input eigenaar).
R14 Afbeeldingen: beschrijvende alt (of `alt=""` waar decoratief/dubbel); `width`/`height`; lazy buiten de hero; strip/service-afbeeldingen geschaald (≤ 2× weergavebreedte, ≤ 150 KB); og:image 1200×630 met kloppende tags.
R15 Ongebruikte afbeeldingen/logo's uit de repo (behalve wat og/favicon gebruikt).

## C. Conversie & UX
R16 Hero: H1 met Dakdekker + Arnhem, lekkage en isolatie beide zichtbaar; knoppen: "Bel 06 57 07 92 76" (tel:) + "Gratis offerte"; WhatsApp-link.
R17 Eén consistente belofte: reactie binnen 24 uur; bij lekkage zo mogelijk binnen 24 uur ter plaatse. Inspectie: "gratis bij een offerteaanvraag, losse inspectie €95". Reparatie €150–500 overal.
R18 Dienst-dropdown bevat alle diensten + "Lekkage / spoed" + "Onderhoudscontract"; niet verplicht. Dienstknoppen en pakketknoppen vullen de dienst vooraf in; "Lekkage / spoed" toont direct een bel-link.
R19 Telefoon-placeholder is niet het eigen nummer; "Geen verkoopgesprek" weg; prijsnotitie "Indicatieve prijzen; de exacte prijs staat in uw offerte."
R20 onderhoudscontract.html: header met link naar home + bel-link; titel-case/emoji opgeschoond.

## D. Toegankelijkheid (WCAG 2.1 AA)
R21 Contrast: tekst ≥ 4,5:1 — witte tekst niet meer op #d4832b (oranje knoppen → #a85d14 of donkerder), oranje tekst op licht → #a85d14, grijs → #6b6b6b, wit op groen ≥ .7 alpha.
R22 Focus: globale `:focus-visible` outline; geen `outline:none` zonder vervanging; invoerranden ≥ 3:1.
R23 Bediening: sterren = echte radio's in fieldset/legend, toetsenbord; FAQ/menu/review-toggle met `aria-expanded`; mobiel menu dicht = niet focusbaar, Escape sluit; labels met `for` op alle velden; `autocomplete` op persoonsvelden; pakketkeuze = radio's.
R24 Structuur: skip-link + `<main id="main">`; logo linkt naar `/`; decoratieve svg/emoji `aria-hidden`.
R25 Beweging: hero-video niet bij reduced-motion of op mobiel (poster), met pauzeknop op desktop; geen oneindige animaties bij reduced-motion; calculator-uitkomst via `aria-live` (één keer).
R26 Vaste elementen bedekken niets: body-padding onder de mobiele balk; cookiebanner bedekt de balk niet; geen zwevende WhatsApp-knop < 769px.

## E. Techniek
R27 Geen JS-fouten; alle interne links/anchors bestaan; dode CSS (.fade-in, .portfolio-*, .review-google-badge) weg.
R28 netlify.toml: security headers (HSTS, nosniff, Referrer-Policy, Permissions-Policy, X-Frame-Options), cache op /images/*.

## Buiten scope (bewust)
- Volledige herindeling van de homepage-secties en een "Wie komt er langs"-blok: wachten op input eigenaar (naam, foto).
- Fonts self-hosten (geen toegang tot pakketregister vanuit deze omgeving); Google Fonts blijft en staat in de privacyverklaring.
- CSP: alleen als Report-Only.

## Open vragen voor eigenaar
1. Welk certificaat (VCA?) en welke verzekering (bedrijfsaansprakelijkheid) heeft u?
2. Waar komen "4.9 / 42 reviews" vandaan? Zijn de 4 getoonde reviews echte klanten met toestemming? Link naar Google Bedrijfsprofiel?
3. "8+ jaar": uw vakervaring of de leeftijd van het bedrijf (KVK lijkt 2023)? Klopt "100+ daken"?
4. Zijn de prijzen incl. of excl. btw?
5. Inspectie: gratis bij offerte, losse inspectie €95 — klopt dat?
6. Wat valt onder "kleine reparaties" in het contract (uren/materiaal)? Inhoud pakketten definitief?
7. Garantie: hoeveel jaar op nieuw dak/reparatie, en wat betekent de tevredenheidsgarantie?
8. Gebruikt u de Meta Pixel echt? Zo niet: helemaal weghalen.
9. Is het zonnepanelen-abonnement (abonnement-contract.txt) een actief product?
10. Echte projectfoto's (met plaats) en een foto/naam van uzelf voor een "Over ons"-blok?
11. Wilt u een pagina "Dakdekker Nijmegen" (alleen met echt werk daar)?
12. Klopt de review-werkwijze op de site (controle of u bij de schrijver gewerkt heeft, positieve en kritische reviews gelijk behandeld)?
13. Bewaartermijn in de privacyverklaring: aanvragen zonder opdracht na 1 jaar verwijderen — akkoord?
14. Isoleert u hellende daken ook aan de binnenkant (zoals op /dakisolatie/ staat)?

## Verificatie (onafhankelijke controle, 17 okt 2026)
25 van 28 eisen PASS bij eerste controle; R2 (onder voorbehoud), R15 (ongebruikte afbeelding) en R21 (contrast interne tools, c-price) zijn daarna opgelost, plus: dienstkaarten 1 kolom op mobiel, focus in mobiel menu, label videoknop, dode CSS.
Niet lokaal te testen: Netlify-404/headers, geslaagde Formspree-verzending, video-afspelen, iOS/Safari.
