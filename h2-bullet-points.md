# OWASP: OWASP Top 10: Broken Access Control

## Yleisesti
- Broken Access Control tarkoittaa tilanteita, joissa käyttäjä voi tehdä asioita, joihin hänellä ei pitäisi olla oikeutta
- 94 % testatuista sovelluksista sisälsi liittyviä ongelmia
- Esiintymistiheys ~3,81 %

## Yleisiä CWE-kategorioita: (Common Weakness Enumeratons = Yleisten Heikkouksien Luetteloita)
- CWE‑200: luvattomalle paljastettu tieto
- CWE‑201: arkaluonteisen tiedon lisääminen lähetettyyn dataan
- CWE‑352: Cross-Site Request Forgery (CSRF)

## Yleisiä ongelmia:
- Liian väljät oikeudet eli Deny by default -periaate puuttuu
- URL-, parametri- tai API‑muokkauksella voi ohittaa pääsynvalvonnan
- IDOR: toisen käyttäjän tietojen katselu tai muokkaus ID:tä vaihtamalla
- Väärin toteutetut POST/PUT/DELETE‑valvonnat
- Oikeuksien nostaminen (privilege escalation)
- Tokenien, cookieiden tai piilokenttien manipulointi
- Väärät CORS‑asetukset
- Force browsing – pääsy sivuihin ilman tunnuksia

## Ehkäisy
- Tee kaikki käyttöoikeustarkistukset palvelinpuolella
- Käytä “deny by default” -mallia
- Yhtenäinen access control koko palvelussa
- Toteuta tietuekohtaiset oikeudet
- Poista hakemistolistaukset ja varmistustiedostot
- Hälytä toistuvista virheellisistä kirjautumisyrityksistä
- Rate limiting API‑kutsuille
- Sessioiden asianmukainen invalidointi
- Testaa käyttöoikeuslogiikka (myös automaattisesti)

Lähde: OWASO.org. 2021. A01:2021 – Broken Access Control. Luettavissa: https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/index.html. Luettu: 19.1.2026.

# Find Hidden Web Directories - Fuzz URLs with ffuf
- Verkkopalvelimmilla voi olla piilotettuja tai linkittämättömiä hakemistoja(/secret, /.svn/admin.)
- Niiden etsiminen käsin on hidasta, kannattaa käyttää automaattista työkalua
- ffuf on on nopea ja joustava web-fuzzeri, jonka tarkoitus on kokeilla automaattisesti monia URL‑polkuja
- ffufilla voi fuzzata myös muita asioita kuin hakemistoja
- Käyttö vaatii lailliset oikeudet ja luvat testausta varten
- Harjoittelu kannattaa tehdä paikallisesti ja ilman internetiä (to avoid leaking packets to the Internet)

Lähde: Karvinen, T. 2023. Find Hidden Web Directories - Fuzz URLs with ffuf. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/. Luettu: 19.1.2026.

# PortSwigger
## Access control
- Määrittää, mitä käyttäjä saa tehdä ja mihin tietoihin pääsee käsiksi
- Rikkoutuu, kun käyttäjä voi suorittaa muita toimintoja ilman oikeuksia

## Tyypit
- Vertikaalinen: käyttäjä saa korkeamman roolin toiminnot (esim. admin-oikeudet)
- Horisontaalinen: käyttäjä pääsee toisen käyttäjän tietoihin (IDOR)
- Kontekstisidonnainen: toimintoja tehdään väärässä järjestyksessä (ohitetaan prosessin vaiheet)

## Haavoittovuudet
- Suojaamattomat admin‑URL:t
- Käyttöoikeus talletettu käyttäjän hallittavaan paikkaan (cookie)
- HTTP‑metodien tai header‑kenttien avulla ohitettavat suojaukset
- URL‑muotoilun erot (polut, isot kirjaimet) kiertävät säännöt

## Ehkäisy
- Deny by default
- Keskitetty access control
- Älä koskaan luota URL:iin, parametreihin, cookien arvoihin
- Suojaa kaikki vaiheet
- Testaa ja auditoi säännöllisesti

Lähde: PortSwigger. s.a. Access control vulnerabilities and privilege escalation. Luettavissa: https://portswigger.net/web-security/access-control. Luettu: 22.1.2026.

# Raportin kirjoittaminen

## Toistettavuus
- Raportin täytyy olla niin tarkka, että toinen opiskelija voi toistaa työn ja saada samat tulokset
- Kirjaa myös virheet ja odottamattomat tulokset
- Kuvaile ympäristösi: aika, paikka, laitteisto, OS, sovellusten versiot jne.

## Täsmällisyys
- Kirjoita kaikki tarkat komennot, valinnat ja toimenpiteet
- Merkitse ajankohdat
- Kerro, mikä onnistui ja mikä ei. Kerro myös jatkotoimet
- Käytä imperfektiä (“suoritin”, “asensin”)

## Helppolukuisuus
- Käytä väliotsikoita, kappalejakoa sekä selkeää kieltä, lisää myös kuvia
- Lisää tiivistelmä ja loppukappale tarpeen mukaan

## Viittaukset
- Mainitse aina tiedon alkuperä
- Lisää selkeät lähteet ja tarvittaessa lisenssitekstit

## Mokia
- Sepittäminen: ei saa keksiä tuloksia tai testejä, se on vilppiä.
- Plagiointi: ei saa kopioida tekstiä tai kuvia ilman lähdemerkintää
- Kotitehtävien tulee olla omaa työtä
- Luvaton kopiointi rikkoo tekijänoikeuksia

Lähde: Karvinen, T. 2006. Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/. Luettu: 22.1.2026.

Lähteet: 

Karvinen, T. 2023. Find Hidden Web Directories - Fuzz URLs with ffuf. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/. Luettu: 19.1.2026.

Karvinen, T. 2006. Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/. Luettu: 22.1.2026.

OWASO.org. 2021. A01:2021 – Broken Access Control. Luettavissa: https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/index.html. Luettu: 19.1.2026.

PortSwigger. s.a. Access control vulnerabilities and privilege escalation. Luettavissa: https://portswigger.net/web-security/access-control. Luettu: 22.1.2026.
