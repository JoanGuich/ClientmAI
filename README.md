### INSTRUCCIONS FUNCIONAMENT

### Commands d'ús per git


[ABANS] Sempre fer quan comencem a treballar

```bash
git pull
```
Això bàsicament fa que actualitzi el teu repositori local amb el que hi ha penjat a github



[DESRPÉS] Quan hagis fet canvis/creat nous fitxers, has de seguir els següents passos:

Per veure quins documents estan disponibles (creats, modificats, eliminats,...) has de fer:

```bash
git status
```


¡¡¡NO PUJAR MAI TOTA UNA CARPETA DE COP. PENJAR DOCUMENTS UN PER UN!!!

```bash
git add <nom_del_fitxer>
```

Una vegada hagis afegit tots els fitxers que volies, després:
```bash
git commit -m <missatge_descriptiu_del_que_puges>
```

Després d'haver fet commit, ja ho pugem al github:
```bash
git push
```

¡¡¡A VEGADES, PEL PRE-COMMIT, EL PUSH NO FUNCIONA. HAURÀS DE REPETIR AQUESTS PASSSOS DESPRÉS:!!!

Veuràs que alguns documents dels que havies fet "git add", tornen a estar disponibles (es veuen fent "git status") per penjar. Aleshores, has de tornar a fer:

```bash
git add <nom_del_fitxer>
```
```bash
git commit -m <missatge_descriptiu_del_que_puges>
```
```bash
git push

________________________


