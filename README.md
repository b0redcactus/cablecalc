Tartalomjegyzék

Funkciók

Telepítés és futtatás

Használat

Bemenetek és logika

Adatszerkezetek (f1Data, f2Data)

Hibakezelés

Bővítés / karbantartás

Ismert korlátok

Licenc és forrás

Funkciók

MSZ 13207:2020 táblázatokra épülő f1 és f2 korrekciós tényezők kiválasztása.

Több paraméter: kábel típusa, talajhőmérséklet, talaj fajlagos hőellenállás, terhelési tényező, elrendezés, rendszerek száma stb.

Védőcső opció: ha Igen, a szorzó 0,85.

Azonnali eredmény: f1, f2 és a végső A megjelenítése.

Minimalista felület, mobilra is jól tördelődik.

100% offline – nincs külső függőség.

Telepítés és futtatás

Klónozd vagy töltsd le a repót.

Nyisd meg az index.html fájlt böngészőben (dupla kattintás).

Nem szükséges semmilyen build vagy szerver.

Használat

Alap: Add meg az alapterhelhetőséget (A) amperben.

f1: Válaszd ki:

Kábel típusa (pl. XLPE90, PVC70, PE70, PAPER65, PAPER60),

Talaj hőmérséklete,

Talaj fajlagos hőellenállása (ρ),

Terhelési tényező (load factor).

f2: Válaszd ki:

Kábel elrendezés (pl. triangle7, triangle25, flat7, 3core),

Kábel típusa (pl. XLPE, PE, PVC, Paper),

Kábelrendszerek száma (1–10),

Talaj fajlagos hőellenállása (ρ),

Terhelési tényező.

Védőcső: Válaszd ki, hogy van-e védőcső (Igen/Nem).

Kattints a Számítás gombra → azonnal megjelenik az f1, f2 és a végső A.

Bemenetek és logika

f1 a 12.a–12.b táblázatok alapján: a kiválasztott hőmérséklet, ρ és LF szerint.

f2 a 13.a–16.b táblázatok alapján: elrendezés, kábel-típus, rendszerek száma, ρ és LF szerint.

Védőcső: ha Igen, a végeredmény további 0,85 szorzót kap.

A kód szigorúan kezeli a hiányzó táblázat-pontokat: ha egy kombináció nincs definiálva (például 25 °C-nál nincs ρ = 0,7 sor az adott táblában), akkor „Nincs adat” és „Nem számolható az adott adatokkal” jelenik meg – nem esik át automatikusan másik ρ-ra.

Adatszerkezetek (f1Data, f2Data)

f1Data:

Top-szint: kábel-típus (pl. XLPE90, PVC70…)

szint: talajhőmérséklet kulcsok (5, 10, 15, 20, 25)

szint: ρ kulcsok ("0.7", "1.0", "1.5", "2.5")

szint: terhelési tényező kulcsok ("0.50", "0.60", "0.70", "0.85", "1.00") → szám érték

f2Data:

Top-szint: elrendezés (triangle7, triangle25, flat7, 3core)

szint: kábel-típus (XLPE, PE, PVC, Paper)

szint: rendszerek száma ("1", "2", … "10")

szint: ρ kulcsok ("0.7", "1.0", "1.5", "2.5")

szint: terhelési tényező kulcsok ("0.5", "0.6", "0.7", "0.85", "1.0") → szám érték

Tipp bővítéshez: új kombinációk felvételekor pontosan ezeket a kulcsformátumokat tartsd (string vs. szám), és az LF kulcsoknál figyelj a formázásra ("0.85" vs "0.85" – következetesen).

Hibakezelés

Ha valamelyik adat hiányzik, a felület:

az adott f-értéknél „Nincs adat”-ot mutat,

és a végső eredménynél „Nem számolható az adott adatokkal” jelenik meg.

Az elérési logika nem kever át másik ρ-ra vagy LF-re, ha a pontos kombináció hiányzik (pl. 25 °C @ ρ=0,7 → „Nincs adat”, nem ρ=1,0).

Bővítés / karbantartás

Adatok frissítése: az f1Data / f2Data objektumokban.

Új opciók:

Elrendezés bővítése: új top-szintű kulcs az f2Data-ban + a UI-ba <option>.

Új kábel-típus: f1Data és/vagy f2Data új al-kulcs + UI <option>.

Védőcső logika: a duct mező (Igen/Nem) olvasása után ductFactor = 0.85 vagy 1, és ezzel szorozzuk a végeredményt.

Stílus: a <style> blokkban; a „Számítás” gomb középre igazításához külön .actions osztály használható (display:flex; justify-content:center; margin-top:12px;).

Ismert korlátok

A kalkulátor a jelenleg bevitt táblázat-pontokkal dolgozik; ha egy kombináció nincs a szabványban vagy még nincs felvive, a rendszer nem számol.

A szabványban szereplő értékek kerekítési és köztes állapot sajátosságai miatt a felhasználó felelőssége az eredmény ellenőrzése és műszaki jóváhagyása.

