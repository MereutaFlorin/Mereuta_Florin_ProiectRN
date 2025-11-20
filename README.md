# 📘 README – Etapa 3: Analiza și Pregătirea Setului de Date pentru Rețele Neuronale



**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:** Mereuta Florin Ciprian  
**Data:** 20.11.2025


## Introducere

Acest document descrie activitățile realizate în **Etapa 3**, în care se analizează și se preprocesează setul de date necesar proiectului „Predictie a Bolilor Cronice bazata pe un esantion". Scopul etapei este pregătirea corectă a datelor pentru instruirea modelului RN, respectând bunele practici privind calitatea, consistența și reproductibilitatea datelor.

---

##  1. Structura Repository-ului Github (versiunea Etapei 3)

```
project-name/
├── README.md
├── docs/
│   └── datasets/          # descriere seturi de date, surse, diagrame
├── data/
│   ├── raw/               # date brute
│   ├── processed/         # date curățate și transformate
│   ├── train/             # set de instruire
│   ├── validation/        # set de validare
│   └── test/              # set de testare
├── src/
│   ├── preprocessing/     # funcții pentru preprocesare
│   ├── data_acquisition/  # generare / achiziție date (dacă există)
│   └── neural_network/    # implementarea RN (în etapa următoare)
├── config/                # fișiere de configurare
└── requirements.txt       # dependențe Python (dacă aplicabil)

```

---

##  2. Descrierea Setului de Date

Date pentru predictia diabetului:       
Nivelul de glucoza din sange-principalul indicator al diabetului;
Tensiunea arteriala-hipertensiunea este frecvent asociată cu diabetul;
Nivelul de insulina-arată funcționarea metabolismului și rezistența la insulină;
Varsta pacientului-riscul de diabet crește odată cu înaintarea în vârstă;
Indicele de masa corporala-obezitatea este un factor de risc major.

Date pentru predictia bolilor de inima:
Varsta– riscul cardiac crește odată cu vârsta;
Tensiune arteriala de repaus– valori mari pot indica probleme cardiovasculare;
Nivelul de colesterol– colesterolul ridicat crește riscul de boală coronariană;
Durere la efort– semn potențial al anginei sau al ischemiei cardiace;
Rezultat ECG-evidențiază eventuale anomalii ale activității electrice a inimii;
Ritm cardiac maxim– poate indica modul în care inima răspunde la stres și efort.

Date pentru predictia hipertensiunii arteriale:
Varsta– tensiunea arterială tinde să crească odată cu vârsta;
Indice masa corporala– excesul de greutate este un factor major de risc;
Colesterol total– valori ridicate pot fi asociate cu probleme cardiovasculare;
Glicemie– nivelul crescut al glucozei poate indica tulburări metabolice care influențează tensiunea;
Ore activitati fizice/saptamana– activitatea scăzută crește riscul de hipertensiune;
Numar bauturi alcoolice/saptamana– consumul excesiv de alcool favorizează creșterea tensiunii arteriale.



### 2.1 Sursa datelor

* **Origine:** dataset public
* **Modul de achiziție:** ☐ Fișier extern
* **Perioada / condițiile colectării:** [Ex: Noiembrie 2024 - Ianuarie 2025, condiții experimentale specifice]

### 2.2 Caracteristicile dataset-ului

* **Număr total de observații:** [Ex: 15,000]
* **Număr de caracteristici (features):** [Ex: 12]
* **Tipuri de date:** ☐ Categoriale 
* **Format fișiere:** ☐ TXT 