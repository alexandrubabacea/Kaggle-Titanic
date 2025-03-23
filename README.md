# Proiect Kaggle-Titanic

## Cerinta1

Folosesc interquartile range pentru a identifica si elimina outlier-ele. Outlierii sunt cei cu valori mai mici decat Q1 - 1.5IQR sau mai mare decat Q3 + 1.5IQR, unde Q1 si Q3 sunt primul si al treilea percentile, iar IQR este diferenta dintre Q3 si Q1. Valorile care sunt în afara intervalului Q1 - 1.5IQR si Q3 + 1.5IQR sunt eliminate.  

## Cerinta2

Calculez Z-score pentru fiecare observatie si elimin valorile care au un Z-score absolut mai mare decât un anumit prag. Z-score este numarul de deviatii standard fata de media setului de date. In cazul lui *Age*, valorile cu un Z-score mai mare de 1.9 sunt considerate outlier-e. Pentru *Fare* se elimina valorile cu Z-score mai mare ca 0.7. Pentru aceste praguri rezultatele sunt asemanatoare cu cele ale functiei de la cerinta 1.  

## Cerintele 3 si 4

### Analiza si prelucrarea setului de date de antrenament

<p align="center">
  <img src="Image1.png" width=300>
</p>

- Pentru toate datele in afara de *Age* (714 din 891), *Cabin* (204 din 891) si *Embarked* (889 din 891) setul de date are toate coloanele complete.  
- Pentru *Age* completez valorile lipsa cu media valorilor existente. Media este 29.69911764705882.  
- Din coloana *Cabin* se poate extrage puntea pe care se afla pasagerul, iar apoi elimin coloana *Cabin*, deoarece are prea multe valori lipsa si nu mai este utila. Coloana cu puntile se va numi *Deck*.  
- Coloana de indici *(PassengerId)* nu aduce informatii utile, iar din coloana *Ticket* informatiile sunt greu de extras, asa ca le voi elimina pe amandoua.  
- Coloana *SibSp* reprezinta numarul de frati/soti de la bordul Titanicului, iar *Parch* reprezinta numarul de parinti/copii de la bordul Titanicului. Astfel, are sens sa combinam cele 2 coloane intr-o coloana *Relatives*.

<img src="Image2.png" align="right" height="70">

- La *Embarked* pot sa completez cu valoarea cea mai intalnita (S - Southampton), deoarece sunt doar 2 valori lipsa.

<br clear="all">

<p align="center">
  <img src="Image3.png" width=500>
</p>


Exista o corelatie intre sexul persoanei si supravietuire, legata de varsta. Au sanse mai mari de supravietuire femeile intre **19 si 35 de ani**. Au sanse mai mici de supravietuire barbatii intre **19 si 38 de ani**. In consecinta voi grupa varstele pe clustere (voi discretiza datele).

<p align="center">
  <img src="Image4.png" width=300>
</p>

In coloana *Fare* se pot observa outlieri evidenti care vor fi eliminati si, ca la varsta, voi discretiza rezultatul impartind datele pe clustere.

<p align="center">
  <img src="Image5.png" width=300>
</p>

Din aceste grafice se poate observa importanta coloanei *Pclass* in predictii, in legatura cu varsta. Se observa ca cei la clasa a 3-a au cele mai mici sanse de supravietuire, pe cand cei de la clasa 1 au sansele cele mai mari de supravietuire. Este utila o coloana care sa combine valorile din *Pclass* si *Age*.
Modific coloana *Name* astfel incat sa pastrez in ea doar titlul persoanelor/formula de adresare. Fac asta cu ajutorul expresiilor regulate.

<p align="center">
  <img src="Image6.png" height=230>
</p>

  - Grupez **Mlle (Mademoiselle)** cu **Ms (Miss)** si cu **Ms** (toate inseamna domnisoara).  
  - Grupez **Mme (Madame)** cu **Mrs** (ambele inseamna doamna).  
  - Toate celelate valori care apar de mai putin de 10 ori sunt convertite in stringul **'Rare'**.

<p align="center">
  <img src="Image7.png" height=100>
</p>

Din coloana *Cabin* pot extrage puntea persoanei si pot crea coloana *Deck*, deoarece pe coloana *Cabin* este numarul puntii urmat de numarul cabinei. Deci, pot extarge puntea cu o expresie regulata. Inainte, totusi, trebuie sa completez in *Cabin* valorile lipsa. Pe pozitile goale pun N, urmat de 0 (N0) pentru a functiona expresia regulata si a ramane in coloana *Deck* doar valoarea N. Dupa
aceasta pot sterge coloana *Cabin*. Coloana *Deck* va arata astfel:

<p align="center">
  <img src="Image8.png" height=170>
</p>

<p align="center">
  <img src="Image9.png" width=300>

  <img src="Image10.png" width=300>
</p>

Asa cum am spus si mai sus, formez coloana *Relatives* prin adunarea coloanelor *SibSp* si *Parch*. Persoanele cu 3 rude au cele mai multe sanse de supravietuire, dar cele mai multe persoanele nu au rude.

<p align="center">
  <img src="Image11.png" width=300>
</p>

Pentru a filtra coloana *Fare* (elimin outlierii) am folosit functia *z_score* si am eliminat valorile care sunt mai mici sau egale cu 0.7 . Am obtinut aceasta valoare constatand ca pentru ea se obtin rezultate foarte apropiat de cele ale functiei iqr si am pastrat-o deoarece ofera rezultate bune. Au ramas valori pentru pretul croazierei mai mici decat 70-75. Intradevar, si din graficul initial se observa ca valorile cele mai frecvente sunt concentrate sub 100.

<p align="center">
  <img src="Image12.png" width=500>
</p>

Pentru a filtra coloana *Age* (elimin outlierii) am folosit functia *z_score* si am eliminat valorile care sunt mai mici sau egale cu 1.9 . Ca la *Fare*, am dedus aceasta valoare prin asemanarea cu iqr si am pastart-o, oferind rezultate bune. Din grafice se observa ca au ramas persoanele sub 55 de ani si, intradevar, se poate observa si din graficele initiale ca marea majoritate a persoanelor are sub 60 de ani.

Convertesc coloanele *Sex, Embarked, Name si Deck* din valori categorice in valori numerice.

Impart varstele (*Age*) si preturile pentru croaziera (*Fare*) in 6 categorii. Daca as incerca o distributie pe intervale egale ar fi prea multe valori in unele intervale, iar in altele prea putine. Astfel, pentru fiecare din cele 2 coloane mentionate folosesc functia qcut din pandas pentru a impartii datele in mod cat de cat egal in intervale. Obs! Intervalele au mai fost ajustate fata de cele returnate de *qcut*, pentru a obtine o acuratete cat mai buna la final. Label-urile pentru fiecare interval de varsta / de pret sunt distribuite astfel:

<p align="center">
  <img src="Image13.png" height=180>
</p>

Creez coloana suplimentara *Age_Class* care combina valorile dintre clasa si varsta. Aceasta se dovedeste ca va avea o importanta semnificativa in antrenarea modelului.
Normalizez datele folosind *StandardScaler*.

<p align="center">
  <img src="Image14.png" width=300>
</p>

### Analiza si prelucrarea setului de date de test

Aleg una din cele 2 variante in functie de parametrul *scop* pe care il citesc.

1. Pentru cazul in care testez local, doresc impartirea setului din *train.csv* in 2 parti: *80% antrenament si 20% test*.  
1. Pentru cazul in care doresc sa prezic pentru setul de date din *test.csv*, voi face asupra setului de date de test aceleasi operatii pe care le-am facut si asupra setului de antrenament, cu cateva mentiuni:  
   - Coloana *Embarked* este completa (nu mai trebuie completata).  
   - Lipsesc valori din coloanele *Fare, Age si Cabin*.  
   - Bineinteles, pentru test *nu trebuie eliminati outlieri*.

<p align="center">
  <img src="Image15.png" width=300>
</p>

### Antrenare cu RandomForestClassifier

- Setez parametrul **random_state** pentru a face tot timpul aceeasi predictie.  
- Am ales sa il setez cu *3* dupa ce am testat toate valorile pana la 100.  

#### Rezultate

<p align="center">
  <img src="Image16.png" width=300>
</p>

Rezultat de pe **Kaggle**:

<p align="center">
  <img src="Image17.png" width=600>
</p>

Am preluat cateva idei legate de prelucrarea datelor de la urmatoarea adresa:  
[Predicting the survival of Titanic passengers](https://towardsdatascience.com/predicting-the-survival-of-titanic-passengers-30870ccc7e8)  
