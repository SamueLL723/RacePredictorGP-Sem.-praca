# RacePredictorGP

**Meno a priezvisko študenta:** Samuel Tomáš Izrael  
**Študijná skupina:** [doplniť]

## Stručný opis projektu

RacePredictorGP je webová aplikácia určená fanúšikom motocyklových pretekov MotoGP, ktorí si chcú vyskúšať predpovedanie výsledkov jednotlivých pretekov a porovnávať svoje úspechy s ostatnými používateľmi. Aplikácia poskytne prehľad nadchádzajúcich podujatí, umožní vybrať jazdcov na prvé tri miesta a po skončení pretekov vyhodnotí úspešnosť predpovedí.

Hlavným cieľom projektu je vytvoriť jednoduchú tipovaciu súťaž. Registrovaní používatelia budú získavať body za presne uhádnuté umiestnenia jazdcov a súťažiť v rebríčku za sezónu. Pri šprinte bude bodovanie za správne tipované 1., 2. a 3. miesto postupne 12, 9 a 7 bodov; pri hlavných pretekoch 25, 20 a 16 bodov. V rozšírenej verzii bude dostupný aj týždenný rebríček a jednoduchý obchod s produktmi.

## Role v projekte

- **Návštevník (neprihlásený používateľ):** Môže si prezerať verejné informácie o aplikácii, kalendár podujatí a rebríček. Môže sa zaregistrovať alebo prihlásiť, ale nemôže odosielať predpovede.
- **Registrovaný používateľ:** Okrem verejných informácií môže vytvárať a upravovať svoje predpovede do uzávierky, prezerať ich výsledky a svoj profil. V rozšírenej verzii môže používať obchod a nákupný košík.
- **Administrátor:** Spravuje údaje o jazdcoch a podujatiach, nastavuje termíny uzávierok, zadáva oficiálne výsledky a stará sa o obsah aplikácie. V rozšírenej verzii spravuje aj produkty obchodu.

## Prípady použitia podľa rolí

### Návštevník

- Návštevník si zobrazí úvodnú stránku a informácie o bodovaní.
- Návštevník si prezrie plánované podujatia a verejný rebríček.
- Návštevník si vytvorí účet alebo sa prihlási do aplikácie.

### Registrovaný používateľ

- Používateľ si vyberie nadchádzajúci šprint alebo hlavné preteky.
- Používateľ vyberie troch rôznych jazdcov a priradí ich na 1., 2. a 3. miesto.
- Používateľ odošle predpoveď a pred uzávierkou ju môže upraviť.
- Používateľ si prezrie svoje predpovede, získané body a umiestnenie v rebríčku.
- Používateľ si zobrazí a upraví základné údaje svojho profilu.
- **Rozšírenie:** Používateľ si prezrie produkty a pridá ich do nákupného košíka.

### Administrátor

- Administrátor pridá, upraví alebo odstráni jazdca.
- Administrátor vytvorí alebo upraví podujatie a nastaví uzávierku predpovedí.
- Administrátor zadá výsledky pretekov, podľa ktorých sa vyhodnotia predpovede.
- **Rozšírenie:** Administrátor pridá, upraví alebo odstráni produkt v obchode.

## Plánované entity

- **Používateľ (`users`):** Konto používateľa. Hlavné atribúty: `id`, používateľské meno, e-mail, hash hesla, rola, dátum registrácie. Súvisí s predpoveďami a prípadne s košíkom.
- **Jazdec (`riders`):** Údaje o jazdcovi MotoGP. Atribúty: `id`, meno, štartové číslo, tím, prípadne krajina. Jazdec sa môže objaviť vo viacerých predpovediach a výsledkoch.
- **Podujatie (`events`):** Šprint alebo hlavné preteky. Atribúty: `id`, názov, okruh, dátum a čas, typ podujatia (šprint/preteky), uzávierka predpovedí, stav. Má viacero používateľských predpovedí.
- **Predpoveď (`predictions`):** Tip konkrétneho používateľa na konkrétne podujatie. Atribúty: `id`, `user_id`, `event_id`, dátum vytvorenia a poslednej úpravy. Obsahuje tri tipované pozície.
- **Položka predpovede (`prediction_items`):** Jeden tipovaný jazdec na jednej pozícii. Atribúty: `id`, `prediction_id`, `rider_id`, tipovaná pozícia (1–3). Jedna predpoveď obsahuje presne tri položky.
- **Výsledok (`results`):** Skutočné umiestnenie jazdca v podujatí potrebné na výpočet bodov. Atribúty: `id`, `event_id`, `rider_id`, výsledná pozícia.
- **Produkt (`products`, rozšírenie):** Produkt v plánovanom obchode. Atribúty: `id`, názov, popis, cena, dostupné množstvo, obrázok.
- **Položka košíka (`cart_items`, rozšírenie):** Produkt, ktorý si používateľ pridal do košíka. Atribúty: `id`, `user_id`, `product_id`, množstvo.

Rebríček nie je nutné viesť ako samostatnú entitu: môže sa vypočítavať zo získaných bodov za predpovede podľa oficiálnych výsledkov.

## Vzťahy medzi entitami

- **Používateľ – Predpoveď (1:N):** Jeden používateľ môže vytvoriť viac predpovedí, najviac jednu na každé podujatie.
- **Podujatie – Predpoveď (1:N):** Na jedno podujatie môže tipovať viac používateľov.
- **Predpoveď – Položka predpovede (1:N):** Každá predpoveď má presne tri položky s pozíciami 1, 2 a 3.
- **Jazdec – Položka predpovede (1:N):** Ten istý jazdec sa môže nachádzať v rôznych predpovediach. V rámci jednej predpovede sa však nesmie opakovať.
- **Podujatie – Výsledok (1:N):** Jedno podujatie má viac výsledných umiestnení jazdcov.
- **Jazdec – Výsledok (1:N):** Jeden jazdec má výsledky z viacerých podujatí.
- **Používateľ – Produkt (M:N, rozšírenie):** Používatelia môžu mať v košíku viac produktov a produkt môže byť v košíkoch viacerých používateľov; vzťah realizuje entita Položka košíka.

## Hlavné stránky aplikácie

1. **Domovská stránka (Home):** Predstavenie aplikácie, pravidlá bodovania a prehľad najbližších podujatí.
2. **Prihlásenie (Login):** Formulár na prihlásenie existujúceho používateľa.
3. **Registrácia (Register):** Vytvorenie nového používateľského účtu.
4. **Predpovede (Predictions):** Výber podujatia a zadávanie alebo úprava troch tipovaných umiestnení pred uzávierkou.
5. **Rebríček (Leaderboard):** Poradie používateľov podľa získaných bodov v aktuálnej sezóne.
6. **Profil (Profile):** Informácie o používateľovi, jeho predpovede a dosiahnuté výsledky.
7. **Administrácia (Admin):** Správa jazdcov, podujatí, uzávierok a oficiálnych výsledkov.
8. **Obchod (Shop, rozšírenie):** Ponuka produktov a jednoduchý nákupný košík.

## Rozdelenie funkcionality

### Základné funkcie

- Registrácia, prihlásenie, odhlásenie a rozlíšenie rolí používateľa a administrátora.
- Zobrazenie a administrátorská správa jazdcov a podujatí (pridanie, zobrazenie, úprava, odstránenie).
- Vytváranie a úprava predpovedí pre šprinty a hlavné preteky, s kontrolou troch rôznych jazdcov a uzávierky.
- Zadávanie oficiálnych výsledkov administrátorom.
- Výpočet bodov iba za presne uhádnuté pozície: šprint 12/9/7 bodov, hlavné preteky 25/20/16 bodov.
- Zobrazenie sezónneho rebríčka a histórie predpovedí používateľa.
- Základný profil používateľa a responzívne používateľské rozhranie.

### Rozširujúce funkcie

- Týždenný rebríček a zobrazenie troch najúspešnejších tipujúcich za daný týždeň.
- Jednoduchý obchod s produktmi ukladanými v databáze a nákupným košíkom (bez integrácie online platby).
- Administrátorská správa produktov.
- Rozšírené štatistiky používateľa (úspešnosť tipovania, história bodov, porovnanie s ostatnými).
- Ďalšie vizuálne a používateľské vylepšenia podľa časových možností.
