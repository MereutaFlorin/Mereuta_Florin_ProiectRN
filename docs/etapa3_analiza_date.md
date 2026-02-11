# Etapa 3: Achiziția, Analiza și Preprocesarea Datelor

**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:** Mereuta Florin Ciprian
**Data:** 11/02/2026  

---

## 1. Descriere Generală
În această etapă, am generat un set de date sintetic (simulat) utilizând **LabVIEW** pentru a emula parametrii medicali ai pacienților și am preprocesat aceste date în **Python** pentru a fi gata de utilizare într-o Rețea Neuronală Artificială (RNA).

---

## 2. Structura Datelor (Dataset Description)

### 2.1 Sursa Datelor
* **Origine:** Generare programatică (Simulare).
* **Instrument:** LabVIEW (VI propriu: `Generator_Dataset_Raw.vi`).
* **Metodă:** S-a utilizat un generator de numere pseudo-aleatoare (`Aleator.vi`) cu limite impuse medical (Min/Max), iar etichetele (Target) au fost atribuite prin reguli logice deterministe (Formula Node).

### 2.2 Caracteristici Dataset (Raw)
* **Format:** CSV (Comma Separated Values).
* **Număr total observații (N):** 10.000 pacienți.
* **Număr atribute (Features):** 10 coloane total (7 intrări, 3 ieșiri).
* **Valori lipsă:** 0 (Setul de date este complet).

### 2.3 Dicționar de Date

| Nume Coloană | Tip | Rol | Descriere | Domeniu (Training Data) |
| :--- | :--- | :--- | :--- | :--- |
| **Glucoza** | Numeric | Intrare | Nivelul glicemiei (mg/dL) | 70 - 200 |
| **Tensiune** | Numeric | Intrare | Tensiune sistolică (mmHg) | 90 - 180 |
| **Colesterol**| Numeric | Intrare | Colesterol total (mg/dL) | 130 - 280 |
| **BMI** | Numeric | Intrare | Indice Masă Corporală | 18 - 40 |
| **Varsta** | Numeric | Intrare | Vârsta pacientului (ani) | 20 - 90 |
| **Puls** | Numeric | Intrare | Ritm cardiac (bpm) | 60 - 120 |
| **Sport** | Numeric | Intrare | Activitate fizică (ore/săpt) | 0 - 10 |
| **Target_Diabet**| Binar | Ieșire | Diagnostic Diabet (0=Nu, 1=Da)| {0, 1} |
| **Target_Inima** | Binar | Ieșire | Risc Boli Inimă (0=Nu, 1=Da) | {0, 1} |
| **Target_Hiper** | Binar | Ieșire | Diagnostic Hipertensiune (0=Nu, 1=Da)| {0, 1} |

---

## 3. Analiza Exploratorie (EDA)

S-a realizat analiza statistică în Python (Pandas/Seaborn) pentru validarea datelor generate.

* **Distribuție:** Parametrii de intrare prezintă o distribuție uniformă (dată de algoritmul Random din LabVIEW), acoperind întreg intervalul specificat.
* **Consistență:** Nu s-au identificat valori nule sau erori de formatare.
* **Corelații:** Clasele (Target) sunt derivate direct din intrări pe baza regulilor medicale implementate (ex: Glucoza > 140 => Diabet).

---

## 4. Preprocesarea Datelor

Pentru a asigura convergența rapidă a rețelei neuronale, s-au aplicat următorii pași de transformare:

### 4.1 Normalizare
S-a utilizat **Min-Max Scaling** pentru a aduce toate valorile numerice în intervalul `[0, 1]`.
* *Motiv:* Evitarea dominanței variabilelor cu valori mari (ex: Colesterol 200 vs Sport 5).

### 4.2 Împărțirea Datelor (Train / Test Split)
Setul de date a fost divizat aleatoriu în două submulțimi:
* **Set Antrenare (Training):** 8.000 pacienți (80%).
* **Set Testare (Testing):** 2.000 pacienți (20%).

---

## 5. Fișiere Generate

Fișierele rezultate în urma acestei etape sunt salvate în structura proiectului:

1.  `data/raw/dataset.csv` - Fișierul brut original (10.000 rânduri).
2.  `data/train/X_train.csv` - Date intrare antrenare (normalizate).
3.  `data/train/y_train.csv` - Etichete (rezultate) antrenare.
4.  `data/test/X_test.csv` - Date intrare testare (normalizate).
5.  `data/test/y_test.csv` - Etichete (rezultate) testare.

---

## 6. Status Etapă

- [x] Structură repository configurată
- [x] Dataset generat în LabVIEW (10.000 samples)
- [x] Dataset analizat statistic (EDA realizată)
- [x] Date normalizate (Min-Max Scaler)
- [x] Seturi train/test generate și salvate
- [x] Documentație actualizată