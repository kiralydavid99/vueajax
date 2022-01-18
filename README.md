# [Király Dávid]

## Hogyan kezdj neki?

1. Jelentkezz be a GitHub szolgáltatásába a böngésződben és a VSCode szerkesztőben
2. A böngészőben forkold ezt a repository-t
3. A Settings > Pages oldalon a Source értékét állítsd gh-pages-re és mentsd el (Save)
4. Clone-ozd a saját(!) repodat a VSCode-ba
5. Írd át a Readme.md elején a [Név] részt a saját nevedre, majd commitolj és pusholj.
6. Telepítsd a függőségeket
```
  npm install
```
7. Indítsd el a fejlesztői szervert
```
  npm run serve
```

## Hogyan dolgozz?

A főbb pontokon commitolj és pushold fel a változtatásokat. Ezt érdemes minél sűrűbben megtenni, hisz így adod majd be a feladatot.

## Hogyan fejezd be?

Add be a repod linkjét. Figyelj oda, hogy az utolsó commit idejét fogjuk nézni.

# Feladat

Készíts TODO alkalmazást.

Az alkalmazás jelenítse meg a todo-kat. Egy-egy todo sorában legyenek a kövekezők:

* A feladat leírása
* Edit gomb

Az Edit gomb megnyomására a feladat leírásából beviteli mező legyen és az Edit gombból pedig egy Save gomb. A Save gomb megnyomására mentődjenek el a változások és legyen látható az új érték, valamint az Edit gomb.

Figylej a következőkre:

* Az App.vue-ban található "todos" változó értékei módosuljanak!
* Bontsd komponensekre a feladatot!
* a props értékeket soha ne módosítsd, használj eseményeket!