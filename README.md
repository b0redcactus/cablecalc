# cablecalc – Földkábel terhelhetőség

---

## Tartalomjegyzék

- [Funkciók](#funkciók)
- [Telepítés és futtatás](#telepítés-és-futtatás)
- [Használat](#használat)
- [Bemenetek és logika](#bemenetek-és-logika)
- [Adatszerkezetek](#adatszerkezetek)
- [Hibakezelés](#hibakezelés)
- [Bővítés / karbantartás](#bővítés--karbantartás)
- [Ismert korlátok](#ismert-korlátok)

---

## Funkciók

- **MSZ 13207:2020** táblák alapján számolt **f1** és **f2** korrekciós tényezők.
- Paraméterek: *kábel típusa*, *talajhőmérséklet*, *talaj fajlagos hőellenállás (ρ)*, *terhelési tényező*, *elrendezés*, *rendszerek száma*.
- **Védőcső opció**: ha *Igen*, a végeredmény további **0,85** szorzót kap.
- Azonnali visszajelzés: külön megjelenik az **f1**, **f2** és a **végső A**.
- **Minimalista, reszponzív** felhasználói felület.
- 100% **offline** – nincs külső függőség.
- (Ha beépítve) **Dark/Light mód** váltó.

---

## Telepítés és futtatás

1. Klónozd vagy töltsd le a repót.
2. Nyisd meg az `index.html` fájlt bármely modern böngészőben.

    git clone <REPO_URL>
    cd <REPO_MAPPÁJA>
    dupla katt az index.html-re, vagy:
    python -m http.server 5500
    és nyisd meg: http://localhost:5500

Nem szükséges build vagy szerver; egy sima fájlmegnyitás is elegendő.

---

## Használat

1. **Alap**: add meg az *alapterhelhetőséget (A)* amperben.
2. **f1**:
   - Kábel típusa: `XLPE90`, `PVC70`, `PE70`, `PAPER65`, `PAPER60`
   - Talaj hőmérséklete: `5`, `10`, `15`, `20`, `25` °C
   - Talaj fajlagos hőellenállása ρ: `0.7`, `1.0`, `1.5`, `2.5`
   - Terhelési tényező (LF): `0.50`, `0.60`, `0.70`, `0.85`, `1.00`
3. **f2**:
   - Elrendezés: `triangle7`, `triangle25`, `flat7`, `3core`
   - Kábel típusa: `XLPE`, `PE`, `PVC`, `Paper`
   - Kábelrendszerek száma: `1`–`10`
   - Talaj fajlagos hőellenállása ρ: `0.7`, `1.0`, `1.5`, `2.5`
   - Terhelési tényező (LF): `0.5`, `0.6`, `0.7`, `0.85`, `1.0`
4. **Védőcső**: válaszd ki, hogy *van-e védőcső* (`Igen/Nem`).
5. Kattints a **Számítás** gombra → megjelenik az **f1**, **f2** és a **végső A**.

---

## Bemenetek és logika

- **f1** a **12.a–12.b** táblák alapján: a kiválasztott *talajhőmérséklet*, *ρ* és *LF* szerint.
- **f2** a **13.a–16.b** táblák alapján: *elrendezés*, *kábel-típus*, *rendszám*, *ρ* és *LF* szerint.
- **Védőcső**: ha *Igen*, a végeredmény további `0.85` szorzót kap.

> A kód **nem** cserél át automatikusan másik ρ-értékre vagy LF-re, ha egy kombináció hiányzik (pl. 25 °C és ρ = 0,7 nincs definiálva az adott táblában). Ilyenkor **„Nincs adat”** és **„Nem számolható az adott adatokkal”** jelenik meg.

---

## Adatszerkezetek

### `f1Data`
- 1. szint: kábel-típus (`XLPE90`, `PVC70`, `PE70`, `PAPER65`, `PAPER60`)
- 2. szint: talajhőmérséklet kulcsok (`5`, `10`, `15`, `20`, `25`)
- 3. szint: ρ kulcsok (stringek: `"0.7"`, `"1.0"`, `"1.5"`, `"2.5"`)
- 4. szint: terhelési tényező kulcsok (stringek: `"0.50"`, `"0.60"`, `"0.70"`, `"0.85"`, `"1.00"`)
- Érték: szám (korrekciós tényező)

### `f2Data`
- 1. szint: elrendezés (`triangle7`, `triangle25`, `flat7`, `3core`)
- 2. szint: kábel-típus (`XLPE`, `PE`, `PVC`, `Paper`)
- 3. szint: rendszám (string: `"1"`, `"2"`, …, `"10"`)
- 4. szint: ρ kulcsok (string: `"0.7"`, `"1.0"`, `"1.5"`, `"2.5"`)
- 5. szint: terhelési tényező kulcsok (string: `"0.5"`, `"0.6"`, `"0.7"`, `"0.85"`, `"1.0"`)
- Érték: szám (korrekciós tényező)

---

## Hibakezelés

- Ha valamelyik adat **hiányzik**, a felület:
  - az adott f-értéknél **„Nincs adat”**-ot mutat,
  - és a végső eredménynél **„Nem számolható az adott adatokkal”** jelenik meg.
- Az elérési logika **nem** kever át másik ρ-ra vagy LF-re, ha a pontos kombináció hiányzik.

---

## Bővítés / karbantartás

- **Adatok frissítése**: az `f1Data` / `f2Data` objektumokban.
- **Új opciók**:
  - Elrendezés bővítése: új top-szintű kulcs az `f2Data`-ban + a UI-ba `<option>`.
  - Új kábel-típus: `f1Data` és/vagy `f2Data` új al-kulcs + UI `<option>`.
- **Védőcső logika**: a `duct` mező (Igen/Nem) alapján a végeredmény `× 0.85` vagy `× 1`.
- **Stílus**: a `<style>` blokkban; a „Számítás” gomb középre igazításához külön `.actions` osztály használható (`display:flex; justify-content:center; margin-top:12px;`).

---

## Ismert korlátok

- A kalkulátor a jelenleg bevitt táblázat-pontokkal dolgozik; ha egy kombináció **nincs a szabványban** vagy még **nincs felvive**, a rendszer nem számol.
- A szabványban szereplő értékek **kerekítési** és **köztes állapot** sajátosságai miatt a felhasználó felelőssége az eredmény **ellenőrzése és műszaki jóváhagyása**.

---


### Kapcsolat / hozzájárulás

- Hibát találtál vagy bővítenéd a táblázatokat?  
  Nyiss egy **issue**-t, írj **pull request**-et, vagy jelezd részletesen a hiányzó kombinációt (elrendezés, kábel-típus, rendszerek száma, ρ, LF, hőmérséklet).
