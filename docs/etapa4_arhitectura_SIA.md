# 📘 README – Etapa 4: Arhitectura Completă a Aplicației SIA bazată pe Rețele Neuronale

**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:** Mereuta Florin Ciprian  
**Link Repository GitHub** https://github.com/MereutaFlorin/Mereuta_Florin_ProiectRN
**Data:** 11/02/2026 

```
## Scopul Etapei 4

Această etapă corespunde punctului **5. Dezvoltarea arhitecturii aplicației software bazată pe RN** din lista de 9 etape - slide 2 **RN Specificatii proiect.pdf**.

**Trebuie să livrați un SCHELET COMPLET și FUNCȚIONAL al întregului Sistem cu Inteligență Artificială (SIA). In acest stadiu modelul RN este doar definit și compilat (fără antrenare serioasă).**

### IMPORTANT - Ce înseamnă "schelet funcțional":

 **CE TREBUIE SĂ FUNCȚIONEZE:**
- Toate modulele pornesc fără erori
- Pipeline-ul complet rulează end-to-end (de la date → până la output UI)
- Modelul RN este definit și compilat (arhitectura există)
- Web Service/UI primește input și returnează output

 **CE NU E NECESAR ÎN ETAPA 4:**
- Model RN antrenat cu performanță bună
- Hiperparametri optimizați
- Acuratețe mare pe test set
- Web Service/UI cu funcționalități avansate

**Scopul anti-plagiat:** Nu puteți copia un notebook + model pre-antrenat de pe internet, pentru că modelul vostru este NEANTRENAT în această etapă. Demonstrați că înțelegeți arhitectura și că ați construit sistemul de la zero.

---

##  Livrabile Obligatorii

### 1. Tabelul Nevoie Reală → Soluție SIA → Modul Software (max ½ pagină)
Completați in acest readme tabelul următor cu **minimum 2-3 rânduri** care leagă nevoia identificată în Etapa 1-2 cu modulele software pe care le construiți (metrici măsurabile obligatoriu):

| **Nevoie reală concretă** | **Cum o rezolvă SIA-ul vostru** | **Modul software responsabil** |
|---------------------------|--------------------------------|--------------------------------|
| Triajul rapid al pacienților în UPU pentru boli cronice | Analiză instantanee a 7 parametri biometrici → diagnostic prezumtiv în < 1 ms | Main_SIA.vi + RN_Inference.vi |
| Evitarea diagnosticării greșite a pacienților sportivi | Logică de compensare (inhibiție) bazată pe orele de sport → reducere 100% alarme false | RN_Inference.vi (Logică Sport) |
| Detecția diabetului asimptomatic la vârstnici | Corelare neliniară Glucoză + Vârstă + BMI → Recall > 90% pentru clasa Diabet | RN_Inference.vi (MLP) |

**Instrucțiuni:**
- Fiți concreti (nu vagi): "detectare fisuri sudură" ✓, "îmbunătățire proces" ✗
- Specificați metrici măsurabile: "< 2 secunde", "> 95% acuratețe", "reducere 20%"
- Legați fiecare nevoie de modulele software pe care le dezvoltați

---

### 2. Contribuția Voastră Originală la Setul de Date – MINIM 40% din Totalul Observațiilor Finale

**Regula generală:** Din totalul de **N observații finale** în `data/processed/`, **minimum 40%** trebuie să fie **contribuția voastră originală**.

#### Cum se calculează 40%:

**Exemplu 3 - Dataset complet original:**

```

Etapa 3-4: Generați toate datele (simulare, senzori proprii, etichetare manuală - varianta recomandata)
→ 100% original ✓ (depășește cu mult 40% - FOARTE BINE!)

```

#### Tipuri de contribuții acceptate (exemple din inginerie):

Alegeți UNA sau MAI MULTE dintre variantele de mai jos și **demonstrați clar în repository**:

| **Tip contribuție** | **Exemple concrete din inginerie** | **Dovada minimă cerută** |
|---------------------|-------------------------------------|--------------------------|
| **Date generate prin simulare fizică** | • Simulare pacienți virtuali cu distribuții gaussiene și reguli medicale | Cod LabVIEW funcțional (`Generator_Dataset_Raw.vi`) + CSV generat + justificare praguri ADA/WHO |

#### Declarație obligatorie în README:

Scrieți clar în acest README (Secțiunea 2):

```markdown
### Contribuția originală la setul de date:

**Total observații finale:** 1000 (după Etapa 3 + Etapa 4)
**Observații originale:** 1000 (100%)

**Tipul contribuției:**
[x] Date generate prin simulare fizică  
[ ] Date achiziționate cu senzori proprii  
[ ] Etichetare/adnotare manuală  
[ ] Date sintetice prin metode avansate  

**Descriere detaliată:**
Am dezvoltat un generator de date sintetice în LabVIEW (`Generator_Dataset_Raw.vi`) care simulează profilul medical al a 1000 de pacienți. Generatorul utilizează funcții de distribuție gaussiană (`Gaussian White Noise`) pentru a crea variabilitate naturală în parametri (ex: Glucoză, Tensiune).

Datele sunt etichetate automat pe baza unor reguli medicale stricte implementate în cod (Sistem Expert):
- **Diabet:** Dacă Glucoza > 140 mg/dL.
- **Hipertensiune:** Dacă Tensiunea > 140 mmHg SAU BMI > 30 (corelație obezitate-tensiune).
- **Boli de Inimă:** Dacă (Colesterol > 240 ȘI Vârsta > 50) SAU Tensiunea > 160.

Această metodă asigură un dataset curat, echilibrat și plauzibil medical, relevant pentru antrenarea rețelei neuronale în absența datelor reale de la pacienți (GDPR).

**Locația codului:** `src/data_acquisition/Generator_Dataset_Raw.vi`
**Locația datelor:** `data/generated/dataset.csv`

**Dovezi:**
- Setup experimental: `docs/screenshots/etapa3_data_gen.png`
- Tabel statistici: `data/generated/dataset.csv` (primele rânduri)

```

---

### 3. Diagrama State Machine a Întregului Sistem (OBLIGATORIE)

**Cerințe:**

* **Minimum 4-6 stări clare** cu tranziții între ele
* **Formate acceptate:** PNG/SVG, pptx, draw.io
* **Locație:** `docs/state_machine.png`
* **Legendă obligatorie:** 1-2 paragrafe în acest README: "De ce ați ales acest State Machine pentru nevoia voastră?"

**Stări tipice pentru un SIA:**

```
IDLE → ACQUIRE → PREPROCESS → INFERENCE → DISPLAY
  ↑__________________________________________|

```

**Legendă obligatorie (scrieți în README):**

```markdown
### Justificarea State Machine-ului ales:

Am ales arhitectura **State Machine (Mașină cu Stări Finite)** pentru că proiectul nostru necesită procesarea secvențială și repetitivă a datelor unui pacient, cu posibilitatea de a reveni oricând la starea de așteptare pentru un nou consult.

Stările principale sunt:
1. **IDLE:** Starea de repaus, așteaptă ca utilizatorul să introducă date sau să apese "Run".
2. **ACQUIRE:** Citește valorile de la cele 7 slidere de pe interfața grafică (Glucoză, Tensiune, etc.).
3. **PREPROCESS:** Normalizează datele brute (împarte la 300) pentru a le aduce în intervalul [0, 1] compatibil cu rețeaua neuronală.
4. **INFERENCE:** Execută modelul neuronal (MLP) care calculează suma ponderată a intrărilor și aplică logica de inhibiție (Sport).
5. **DISPLAY:** Actualizează termometrele de risc (Verde/Galben/Roșu) pe baza rezultatului inferenței.

Tranzițiile critice sunt:
- **IDLE → ACQUIRE:** Când utilizatorul apasă butonul de execuție sau modifică o valoare.
- **DISPLAY → IDLE:** După afișarea rezultatului, sistemul revine automat în așteptare.

Starea **ERROR** este implicită în LabVIEW (Error Cluster), gestionând cazurile în care fișierele de configurare lipsesc sau valorile de intrare sunt aberante (ex: negative).

```

---

### 4. Scheletul Complet al celor 3 Module Cerute la Curs (slide 7)

Toate cele 3 module trebuie să **pornească și să ruleze fără erori** la predare. Nu trebuie să fie perfecte, dar trebuie să demonstreze că înțelegeți arhitectura.

| **Modul** | **Python (exemple tehnologii)** | **LabVIEW** | **Cerință minimă funcțională (la predare)** |
| --- | --- | --- | --- |
| **1. Data Logging / Acquisition** | `src/data_acquisition/` | `Generator_Dataset_Raw.vi` | **MUST:** Produce CSV cu datele voastre (inclusiv cele 40% originale). Cod rulează fără erori și generează minimum 100 samples demonstrative. |
| **2. Neural Network Module** | `src/neural_network/` | `RN_Inference.vi` | **MUST:** Modelul RN definit, compilat, poate fi încărcat. **NOT required:** Model antrenat cu performanță bună (poate avea weights random/inițializați). |
| **3. Web Service / UI** | Streamlit, Gradio, FastAPI | `Main_SIA.vi` | **MUST:** Primește input de la user și afișează un output. **NOT required:** UI frumos, funcționalități avansate. |

#### Detalii per modul:

#### **Modul 1: Data Logging / Acquisition**

**Funcționalități obligatorii:**

* [x] Cod rulează fără erori: `Generator_Dataset_Raw.vi`
* [x] Generează CSV în format compatibil cu preprocesarea din Etapa 3 (`dataset.csv`)
* [x] Include minimum 40% date originale în dataset-ul final (100% în cazul nostru)
* [x] Documentație în cod: ce date generează, cu ce parametri (comentarii în Block Diagram)

#### **Modul 2: Neural Network Module**

**Funcționalități obligatorii:**

* [x] Arhitectură RN definită și compilată fără erori (`RN_Inference.vi`)
* [x] Model poate fi salvat și reîncărcat (SubVI)
* [x] Include justificare pentru arhitectura aleasă (MLP simplu pentru transparență)
* [x] **NU trebuie antrenat** cu performanță bună (weights pot fi random sau inițiale)

#### **Modul 3: Web Service / UI**

**Funcționalități MINIME obligatorii:**

* [x] Propunere Interfață ce primește input de la user (Slidere pe Front Panel)
* [x] Includeți un screenshot demonstrativ în `docs/screenshots/ui_demo.png`

**Ce NU e necesar în Etapa 4:**

* UI frumos/profesionist cu grafică avansată
* Funcționalități multiple (istorice, comparații, statistici)
* Predicții corecte (modelul e neantrenat, e normal să fie incorect)
* Deployment în cloud sau server de producție

**Scop:** Prima demonstrație că pipeline-ul end-to-end funcționează: input user → preprocess → model → output.

## Structura Repository-ului la Finalul Etapei 4 (OBLIGATORIE)

**Verificare consistență cu Etapa 3:**

```
proiect-rn-SIA-LabVIEW/
├── data/
│   ├── raw/
│   ├── processed/
│   ├── generated/  # Date originale (dataset.csv)
│   ├── train/      # X_train.csv, y_train.csv
│   ├── validation/
│   └── test/       # X_test.csv, y_test.csv
├── src/
│   ├── data_acquisition/
│   │   ├── Generator_Dataset_Raw.vi
│   │   └── Aleator.vi
│   ├── preprocessing/  # (Integrat în Main_SIA - State: Preprocess)
│   ├── neural_network/
│   │   └── RN_Inference.vi
│   └── app/  # UI schelet
│       └── Main_SIA.vi
├── docs/
│   ├── state_machine.png           #(Diagrama Bloc din Main_SIA)
│   ├── screenshots/
│   │   ├── ui_demo.png
│   │   ├── inference_optimized.png
│   │   └── etapa3_data_gen.png
│   └── 
├── models/  # Untrained model (reprezentat de structura internă RN_Inference)
├── config/
├── README.md
├── README_Etapa3.md              # (etapa 3.md)
├── README_Etapa4_Arhitectura_SIA.md              # ← acest fișier completat (în rădăcină)
└── requirements.txt  # (LabVIEW Runtime 2020+)

```

**Diferențe față de Etapa 3:**

* Adăugat `data/generated/` pentru contribuția dvs originală
* Adăugat `src/data_acquisition/` - MODUL 1
* Adăugat `src/neural_network/` - MODUL 2
* Adăugat `src/app/` - MODUL 3
* Adăugat `models/` pentru model neantrenat
* Adăugat `docs/state_machine.png` - OBLIGATORIU
* Adăugat `docs/screenshots/` pentru demonstrație UI

---

## Checklist Final – Bifați Totul Înainte de Predare

### Documentație și Structură

* [x] Tabelul Nevoie → Soluție → Modul complet (minimum 2 rânduri cu exemple concrete completate in README_Etapa4_Arhitectura_SIA.md)
* [x] Declarație contribuție 40% date originale completată în README_Etapa4_Arhitectura_SIA.md
* [x] Cod generare/achiziție date funcțional și documentat (`Generator_Dataset_Raw.vi`)
* [x] Dovezi contribuție originală: grafice + log + statistici în `docs/` (`etapa3_data_gen.png`)
* [x] Diagrama State Machine creată și salvată în `docs/state_machine.png`
* [x] Legendă State Machine scrisă în README_Etapa4_Arhitectura_SIA.md (minimum 1-2 paragrafe cu justificare)
* [x] Repository structurat conform modelului de mai sus (verificat consistență cu Etapa 3)

### Modul 1: Data Logging / Acquisition

* [x] Cod rulează fără erori (`Generator_Dataset_Raw.vi`)
* [x] Produce minimum 40% date originale din dataset-ul final (100%)
* [x] CSV generat în format compatibil cu preprocesarea din Etapa 3 (`dataset.csv`)
* [x] Documentație în `src/data_acquisition/README.md` cu:
* [x] Metodă de generare/achiziție explicată (Simulare Gaussiană)
* [x] Parametri folosiți (Praguri ADA/WHO)
* [x] Justificare relevanță date pentru problema voastră (Triaj boli cronice)


* [x] Fișiere în `data/generated/` conform structurii

### Modul 2: Neural Network

* [x] Arhitectură RN definită și documentată în cod (docstring detaliat) - versiunea inițială (`RN_Inference.vi`)
* [x] README în `src/neural_network/` cu detalii arhitectură curentă

### Modul 3: Web Service / UI

* [x] Propunere Interfață ce pornește fără erori (comanda de lansare testată)
* [x] Screenshot demonstrativ în `docs/screenshots/ui_demo.png`
* [x] README în `src/app/` cu instrucțiuni lansare (comenzi exacte)

---

**Predarea se face prin commit pe GitHub cu mesajul:** `"Etapa 4 completă - Arhitectură SIA funcțională"`



```

```