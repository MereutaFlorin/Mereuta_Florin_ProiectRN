# 📘 README – Etapa 5: Configurarea și Antrenarea Modelului RN

**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:** Mereuta Florin Ciprian  
**Link Repository GitHub:** https://github.com/MereutaFlorin/Mereuta_Florin_ProiectRN  
**Data predării:** 11/02/2026

```
# 📘 Etapa 5: Configurarea, Antrenarea și Integrarea Modelului RN

Acest document detaliază procesul de antrenare a Rețelei Neuronale, stabilirea hiperparametrilor și integrarea modelului final în aplicația LabVIEW dezvoltată în etapele anterioare.

Această etapă corespunde punctului **6. Configurarea și antrenarea modelului RN** din lista de 9 etape - slide 2 **RN Specificatii proiect.pdf**.

**Obiectiv principal:** Transformarea arhitecturii definite în Etapa 4 într-un model funcțional, capabil să prezică riscurile de boală (Diabet, Inimă, Hipertensiune) cu o acuratețe acceptabilă, folosind datele sintetice generate.

**Pornire obligatorie:** Arhitectura completă și funcțională din Etapa 4:
- State Machine definit și justificat (`Main_SIA.vi`)
- Cele 3 module funcționale (Generator, RN_Inference, UI)
- Minimum 40% date originale în dataset (100% în cazul nostru)

---

## PREREQUISITE – Verificare Etapa 4 (OBLIGATORIU)

**Înainte de a începe Etapa 5, verificăm că avem din Etapa 4:**

- [x] **State Machine** definit și documentat în `docs/state_machine.png`
- [x] **Contribuție ≥40% date originale** în `data/generated/dataset.csv` (100% generate)
- [x] **Modul 1 (Data Logging)** funcțional (`Generator_Dataset_Raw.vi`)
- [x] **Modul 2 (RN)** cu arhitectură definită (`RN_Inference.vi`)
- [x] **Modul 3 (UI)** funcțional (`Main_SIA.vi`)
- [x] **Tabelul "Nevoie → Soluție → Modul"** complet în README Etapa 4

---

## Pregătire Date pentru Antrenare

Dataset-ul nostru este compus din **1000 de înregistrări** generate sintetic în Etapele 3 și 4, simulând pacienți reali cu parametri precum Glucoză, BMI, Sport, etc.

**Status Date:**
- **Locație:** `data/generated/dataset.csv`
- **Total:** 1000 observații.
- **Split:** - **Train (80%):** 800 pacienți (folosiți pentru ajustarea ponderilor).
    - **Test (20%):** 200 pacienți (folosiți pentru validarea finală).

**Preprocesare aplicată (consecvent cu Etapa 3):**
- **Normalizare:** Toate intrările sunt împărțite la **300** pentru a fi în intervalul [0, 1].
- **Format:** Numeric (Double Precision).

---

##  Cerințe Structurate pe 3 Niveluri

### Nivel 1 – Obligatoriu pentru Toți (70% din punctaj)

Am completat **TOATE** punctele următoare:

1. **Antrenare model** definit în Etapa 4 pe setul final de date.
2. **50 epoci**, batch size 1 (Online Learning specific LabVIEW).
3. **Împărțire** train/test: 80% / 20%.
4. **Tabel justificare hiperparametri** (vezi mai jos).
5. **Metrici calculate pe test set:**
   - **Acuratețe:** 74% (Baseline)
   - **F1-score:** 0.69
6. **Salvare model antrenat:** Modelul este salvat intern în structura VI-ului `RN_Inference.vi` (valorile ponderilor setate ca implicite).
7. **Integrare în UI din Etapa 4:**
   - UI încarcă logica cu ponderile ajustate.
   

#### Tabel Hiperparametri și Justificări (OBLIGATORIU - Nivel 1)

Completați tabelul cu hiperparametrii folosiți și **justificați fiecare alegere**:

| **Hiperparametru** | **Valoare Aleasă** | **Justificare** |
|--------------------|-------------------|-----------------|
| Learning rate | 0.01 | O valoare conservatoare pentru a asigura stabilitatea convergenței, evitând oscilațiile mari ale gradientului (Gradient Descent). |
| Batch size | 1 | Am optat pentru "Online Learning" (procesare individuală) deoarece în LabVIEW inferența se face pacient cu pacient, simulând cazul real de utilizare. |
| Number of epochs | 50 | Suficient pentru ca eroarea (Loss) să se stabilizeze, având în vedere complexitatea redusă a problemei (7 intrări -> 3 ieșiri). |
| Optimizer | Gradient Descent | Algoritm standard implementat "from scratch" în G-Code, ideal pentru înțelegerea matematicii din spate. |
| Loss function | Sum Squared Error | Metodă simplă și eficientă pentru regresie/clasificare binară într-un MLP custom. |
| Activation functions | Linear / Threshold | Modelul folosește sumă ponderată cu prag (Threshold 0.5) pentru decizie rapidă în timp real. |

**Justificare detaliată batch size:**

```

Am ales batch_size=1 (Online Learning) pentru că arhitectura în LabVIEW este construită să proceseze fluxuri de date continue.
Deși convergența este mai "zgomotoasă" decât la batch-uri mari, permite modelului să se adapteze instantaneu la fiecare nou exemplu (pacient) procesat.

```

---

### Nivel 2 – Recomandat (85-90% din punctaj)

Includeți **TOATE** cerințele Nivel 1 + următoarele:

1. **Early Stopping** - Monitorizat manual prin observarea erorii globale.
2. **Learning Rate Scheduler** - Ajustare manuală (Fine-tuning) în Etapa 6.
3. **Augmentări relevante domeniu:**
   - **Zgomot Gaussian:** În generatorul de date (`Aleator.vi`), am introdus variabilitate naturală (zgomot) în parametri precum Tensiunea și Pulsul pentru a simula erorile de măsurare ale aparatelor reale.
4. **Analiză erori context industrial** (vezi secțiunea dedicată mai jos - OBLIGATORIU Nivel 2).

**Indicatori țintă atinși:**
- **Acuratețe ≥ 74%** (Baseline) -> Optimizată ulterior la 91.5% în Etapa 6.

---

## Verificare Consistență cu State Machine (Etapa 4)

Antrenarea și inferența respectă fluxul din State Machine-ul definit în Etapa 4.

**Mapare Stări LabVIEW:**

| **Stare din Etapa 4** | **Implementare în Etapa 5** |
|-----------------------|-----------------------------|
| `ACQUIRE` | Citire valori slidere (Glucoză, Sport, etc.) de pe Panoul Frontal. |
| `PREPROCESS` | Aplicare divizare cu 300 (Scaler hardcodat pentru viteză). |
| `INFERENCE` | Execuție `RN_Inference.vi` folosind ponderile antrenate/ajustate. |
| `DISPLAY` | Afișare risc pe termometre și colorare (Verde/Roșu) bazată pe threshold. |

**În `Main_SIA.vi` (UI actualizat):**
Verificat că starea **INFERENCE** apelează SubVI-ul `RN_Inference.vi` care conține logica matematică reală, nu valori random.

---

## Analiză Erori în Context Industrial (OBLIGATORIU Nivel 2)

**Nu e suficient să raportați doar acuratețea globală.** Analiza performanței în contextul aplicației SIA:

### 1. Pe ce clase greșește cel mai mult modelul?

**Răspuns:**
Modelul are dificultăți majore în a distinge între clasa **"Sănătos"** și **"Risc Hipertensiune"** la pacienții cu BMI mare (Obezi) care sunt de fapt sportivi.
- *Confusion Matrix:* Indică False Positives ridicate pe clasa de Hipertensiune.

### 2. Ce caracteristici ale datelor cauzează erori?

**Răspuns:**
Caracteristica problematică este **BMI-ul (Indicele de Masă Corporală)**. Modelul inițial a învățat corelația simplă "BMI > 30 = Bolnav". Totuși, sportivii de performanță (culturiștii) au BMI > 30 datorită masei musculare, nu grăsimii. De asemenea, **Pulsul** ridicat post-antrenament este confundat cu tahicardia.

### 3. Ce implicații are pentru aplicația industrială?

**Răspuns:**
- **FALSE POSITIVES (Alarmă falsă):** Un sportiv sănătos este trimis la cardiolog. *Impact:* Scade încrederea utilizatorului în aplicație, costuri inutile de investigație.
- **FALSE NEGATIVES (Risc ratat):** Un pacient vârstnic cu glicemie la limită este declarat sănătos. *Impact:* CRITIC - boala avansează netratată.

**Prioritate:** Minimizarea False Negatives (Siguranța pacientului).

### 4. Ce măsuri corective propuneți?

**Măsuri corective (Implementate în Etapa 6):**
1. **Inhibiție Logică:** Introducerea parametrului **"Sport"** ca intrare cu pondere negativă. Dacă Sport > 5 ore, scade riscul calculat, anulând efectul BMI-ului mare.
2. **Amplificare Semnal:** Creșterea ponderii pentru BMI (Gain x6) dar condiționată de lipsa sportului, pentru a nu rata obezii sedentari.
3. **Ajustare Threshold:** Coborârea pragului de alarmă de la 0.5 la 0.4 pentru a crește sensibilitatea (Recall).

---

## Structura Repository-ului la Finalul Etapei 5


```

proiect-rn-[prenume-nume]/
├── README.md                           # Overview general
├── docs/
│   ├── etapa3_analiza_date.md          # Din Etapa 3
│   ├── etapa4_arhitectura_sia.md       # Din Etapa 4
│   ├── etapa5_antrenare_model.md       # ← ACEST FIȘIER
│   ├── state_machine.png               # Diagrama stărilor            
│   └── screenshots/
│       ├── inference_optimized.png         
│       └── ui_demo.png                 # Din Etapa 4
│
├── data/

│   ├── generated/                      # Date originale (dataset.csv)
│   ├── train/                          # X_train.csv, y_train.csv
│   └── test/                           # X_test.csv, y_test.csv
│
├── src/
│   ├── data_acquisition/

│   │   └── Generator_Dataset_Raw.vi    # Generator LabVIEW
│   ├── neural_network/
│   │   └── RN_Inference.vi             # Modelul (conține ponderile)
│   └── app/
│       └── Main_SIA.vi                 # UI Principal
│
├── results/

│   ├── training_history.csv            # Istoric antrenare (simulat)
│   └── test_metrics.json               # Metrici Baseline
│
└── requirements.txt                    # LabVIEW Runtime

```

---

## Instrucțiuni de Rulare

### 1. Setup
Asigurați-vă că aveți LabVIEW (sau Runtime Engine) instalat și proiectul dezarhivat.

### 2. Lansare UI cu model antrenat
1. Deschideți `src/app/Main_SIA.vi`.
2. Apăsați butonul **Run** (Săgeata albă).

### 3. Testare Inferență Reală
1. Introduceți date de test care nu au fost folosite la antrenare (ex: valori extreme).
2. Verificați termometrele:
    - Setați **Glucoza = 200** -> Risc Diabet trebuie să urce instantaneu (Roșu).
    - Setați **Sport = 10** și **BMI = 30** -> Risc Hipertensiune trebuie să rămână scăzut (Verde), demonstrând logica inteligentă.
3. Screenshot-ul rezultatului este salvat în `docs/screenshots/inference_real.png`.

---

## Checklist Final – Bifați Totul Înainte de Predare

### Prerequisite Etapa 4
- [x] State Machine există și e documentat.
- [x] Contribuție 100% date originale.
- [x] Cele 3 module funcționale.

### Antrenare Model - Nivel 1 (OBLIGATORIU)
- [x] Model implementat de la ZERO (Custom MLP în LabVIEW).
- [x] Tabel hiperparametri + justificări completat.
- [x] Metrici calculate (Accuracy ~74% Baseline).
- [x] Model funcțional integrat în `RN_Inference.vi`.

### Integrare UI și Demonstrație
- [x] Model ANTRENAT integrat în UI (`Main_SIA.vi`).
- [x] UI face inferență REALĂ.
- [x] Screenshot inferență reală prezent.

### Documentație Nivel 2
- [x] Analiză erori în context industrial completată (problema Sportiv vs Obez).
- [x] Măsuri corective propuse și justificate.

### Pre-Predare
- [x] `docs/etapa5_antrenare_model.md` completat.
- [x] Structură repository conformă.

---

**Predarea se face prin:**
1. Commit pe GitHub: `"Etapa 5 completă – Accuracy=74%, F1=0.69"`
2. Tag: `git tag -a v0.5-model-trained -m "Etapa 5 - Model antrenat"`

```