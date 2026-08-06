# Záloha na účet cez Google — čo treba nastaviť

Hra to má hotové. Chýbajú len dva údaje, ktoré si vygeneruješ raz
a vpíšeš do `index.html`. Kým tam nie sú, funkcia je vypnutá a hra
beží presne ako doteraz.

Celé to beží v prehliadači — **nepotrebuješ vlastný server**. Prihlásenie
aj úložisko zabezpečí Supabase, ktorý má na tento rozsah bezplatnú úroveň.

---

## 1 · Projekt v Supabase

1. Založ si účet na **supabase.com** a vytvor nový projekt
2. V **Project Settings → API** si odlož dve hodnoty:
   - **Project URL** (napr. `https://abcdefgh.supabase.co`)
   - **anon public** kľúč

Ten kľúč je verejný a patrí do stránky — nie je to heslo. Prístup
k dátam stráži pravidlo, ktoré nastavíš v treťom kroku.

---

## 2 · Prihlásenie cez Google

1. V Google Cloud Console vytvor **OAuth client ID** typu *Web application*
2. Do **Authorized redirect URIs** vlož adresu, ktorú ti ukáže Supabase
   v **Authentication → Providers → Google**
3. Client ID a Client Secret vlož do toho istého miesta v Supabase
   a poskytovateľa zapni

Hra si od Googlu pýta len e-mailovú adresu — žiadny prístup k pošte,
diskom ani kontaktom. Tester preto neuvidí žiadne strašidelné hlásenie
o overení aplikácie.

**Dôležité:** v Supabase v **Authentication → URL Configuration** pridaj
adresu svojej hry do **Redirect URLs** (napr. `https://provizia.vercel.app`).
Bez toho sa prihlásenie vráti na zlé miesto.

---

## 3 · Tabuľka na zálohy

V Supabase otvor **SQL Editor** a spusti toto:

```sql
create table if not exists savy (
  id uuid primary key references auth.users(id) on delete cascade,
  data text not null,
  meno text,
  uroven int,
  mesiac int,
  den int,
  jednotky int,
  verzia text,
  kedy timestamptz default now()
);

alter table savy enable row level security;

create policy "kazdy vidi len svoju zalohu"
  on savy for all
  using (auth.uid() = id)
  with check (auth.uid() = id);
```

Posledná časť je podstatná: zaručuje, že **každý hráč vidí a mení len
svoj vlastný záznam**. Cudziu zálohu si nikto nestiahne ani neprepíše.

---

## 4 · Vpíš údaje do hry

V `index.html` nájdi tento blok (je hneď na začiatku skriptu):

```js
const CLOUD={
 url:"",
 anon:"",
 tabulka:"savy"
};
```

Vyplň `url` a `anon` hodnotami z prvého kroku. Nič iné nemeň.
Nahraj súbor a hotovo.

---

## Ako to potom vyzerá pre hráča

V **Nastaveniach → ☁️ Záloha na účet**:

- **Prihlásiť sa cez Google** — presmeruje, prihlási, vráti späť do hry
- **⬆︎ Nahrať tento postup na účet**
- **⬇︎ Stiahnuť postup z účtu**

Hra vždy ukáže, čo je na účte — meno, úroveň, mesiac, jednotky aj kedy
to bolo uložené — a **upozorní, ktorá pozícia je novšia**. Sťahovanie
prepíše rozohranú hru, takže si ho hráč musí potvrdiť na osobitnej
obrazovke, kde vidí obe pozície vedľa seba.

---

## Čo som mohol otestovať a čo nie

Otestované je všetko, čo nepotrebuje sieť: čítanie tokenu, spracovanie
návratu z prihlásenia, odhlásenie, porovnanie, ktorá pozícia je novšia,
zostavenie zálohy aj správanie, keď funkcia nie je nastavená —
tam sa hra nesmie zaseknúť a musí ponúknuť prenosový kód. To je 13 testov.

**Samotné spojenie so serverom otestované nie je** — v prostredí, kde
hru staviam, nie je sieť. Prvé prihlásenie a prvé nahratie si preto
vyskúšaj sám a daj vedieť, ak sa niečo zasekne.

---

## Prenosový kód zostáva

Záloha na účet ho nenahrádza, len dopĺňa. Kto sa prihlásiť nechce,
prenesie si postup kódom ako doteraz.
