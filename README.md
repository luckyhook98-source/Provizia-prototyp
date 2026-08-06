# PROVÍZIA — čo nahrať na Vercel

Verzia **0.9.0**, zostavenie **2026-08-06-71ea62**

Do Vercelu ide **celý tento priečinok**. Sú v ňom štyri súbory a všetky
tam patria:

| súbor | načo je |
|---|---|
| `index.html` | celá hra — jeden súbor, žiadny build, žiadne závislosti |
| `vercel.json` | zabráni tomu, aby testerom zostala v prehliadači stará verzia |
| `manifest.json` | umožní pridať hru na plochu telefónu ako aplikáciu |
| `icon.svg` | ikona, ktorá sa pri tom zobrazí |
| `ZALOHA-NA-UCET.md` | návod, ako zapnúť prihlásenie cez Google (nenahráva sa) |

---

## Rýchlo, na jedno ukázanie

1. Choď na **vercel.com/drop**
2. Pretiahni tam **celý priečinok**
3. Vyber tím, pomenuj projekt, stlač **Deploy**

Máš živý odkaz. Pozor: každé ďalšie pretiahnutie vytvorí **nový projekt
s novým odkazom**.

---

## Na testovanie — odkaz zostane navždy rovnaký

Toto odporúčam, keďže vydávaš verziu za verziou.

1. Založ repozitár na GitHube a nahraj doň tieto štyri súbory
2. Na Verceli daj **Add New → Project → Import Git Repository**
3. Vyber repozitár, nič nenastavuj, stlač **Deploy**

Pri každej ďalšej verzii len prepíšeš `index.html` a pushneš. Nasadí sa
samo, **odkaz sa nikdy nezmení** a v paneli máš históriu, z ktorej sa
dá jedným klikom vrátiť späť.

---

## Prečo tam je `vercel.json`

Hovorí prehliadaču, aby sa pri každom otvorení spýtal servera, či nie je
novšia verzia, namiesto toho, aby použil odloženú kópiu. Bez toho by
testeri videli starú hru aj po nasadení novej — presne ten problém,
čo si mal.

**Overenie ostáva v hre.** V Nastaveniach je zostavenie
(`2026-08-06-71ea62`). Keď si nie ste istí, porovnajte tento reťazec.
A v tých istých Nastaveniach je **Overiť kód od kolegu** — vložíš jeho
prenosový kód a hra povie, či beží na staršom súbore.

---

## Čo dostanú testeri navyše

Vďaka `manifest.json` si môžu hru **pridať na plochu telefónu**
(v Safari cez Zdieľať → Pridať na plochu, v Chrome cez ponuku).
Otvorí sa potom na celú obrazovku bez panela prehliadača, ako appka.

---

## Po nasadení novej verzie

Nič nemusíš písať do správy. Komu sa načíta rozohraná hra zo staršej
verzie, tomu hra sama ukáže obrazovku **Čo je nové** so zoznamom zmien.

---

## Iné hostingy

Rovnaký priečinok funguje aj na **Netlify**, **Cloudflare Pages** alebo
**GitHub Pages**. Súbor `vercel.json` je len pre Vercel — Netlify aj
Cloudflare čítajú namiesto neho súbor `_headers` s obsahom:

```
/*
  Cache-Control: public, max-age=0, must-revalidate
```

---

## Ako je to s uloženými hrami

Na webe sa postup ukladá **do prehliadača testera** — zostane mu tam aj
po zatvorení stránky a po reštarte telefónu. Platí to pre každú adresu
zvlášť: uložená hra z prostredia Claude sa na Vercel sama neprenesie.

**Kto už niekde hrá a nechce začínať odznova:**

1. V starej hre otvorí **Nastavenia → Vytvoriť prenosový kód** a skopíruje ho
2. Otvorí novú adresu a hneď na úvodnej obrazovke dá
   **Načítať z prenosového kódu**
3. Vloží kód — hra pokračuje presne tam, kde skončil

Postavu vytvárať nemusí, je v kóde. V Nastaveniach vždy vidí, kam sa mu
hra ukladá.

---

## Vlastná doména

Dá sa pripojiť na ktoromkoľvek z nich. Má zmysel až vtedy, keď hru
budeš ukazovať mimo okruh testerov.
