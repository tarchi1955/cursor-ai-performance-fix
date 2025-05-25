# Come ho risolto i rallentamenti di Cursor AI: cache e progetto su disco esterno.

## 💬 Introduzione

Ciao a tutti,  
voglio condividere la mia esperienza con Cursor AI. Avevo gravi rallentamenti e blocchi durante l’uso, ma dopo diverse prove ho trovato una soluzione semplice ed efficace.

---

## 🚩 Il problema

- Cursor AI diventava sempre più lento dopo pochi minuti di utilizzo.
- Apriva 25–30 processi `Cursor.exe`, saturando il disco **C:** (SSD interno).
- Anche impostando `CURSOR_MAX_WORKERS=6`, il numero di subprocessi **non veniva limitato**.
- Cursor si bloccava o rallentava tanto da diventare inutilizzabile.

---

## ✅ La soluzione che ha funzionato

### 1. Ho spostato la cache di Cursor su disco esterno (F:)
- Ho preso la cartella `C:\Users\<utente>\AppData\Roaming\Cursor`
- L’ho spostata su `F:\Cursor_Cache`
- Poi ho creato un collegamento simbolico con:

```cmd
mklink /D "C:\Users\<utente>\AppData\Roaming\Cursor" "F:\Cursor_Cache"
```

> Cursor continua a scrivere nella sua cartella “standard”, ma in realtà i file vanno su F:

### 2. Ho spostato anche il progetto sul disco F:
- Tutti i file del progetto (incluso `app.py`) sono su `F:\`
- Questo evita qualsiasi scrittura pesante sul disco C:

### 3. Ho smesso di contare su `CURSOR_MAX_WORKERS`
- L’ho impostata come variabile utente e di sistema → nessun effetto
- Cursor continua ad aprire 20+ subprocessi
- Conclusione: **la variabile è ignorata nelle versioni attuali**

---

## 🧠 Modalità d'uso

Durante l’intero utilizzo del progetto, **la modalità AI di Cursor è sempre attiva**:
- L'AI assiste continuamente durante la scrittura, modifica e analisi del codice
- Cursor mantiene attivi molti subprocessi per supportare il completamento automatico, la spiegazione del codice, le refactor AI e gli agenti
- Questo comporta un **accesso costante alla cache**, alla rete e al disco

---

## 🎯 Risultato

- Cursor è **fluido e stabile**
- Nessun blocco o saturazione dell’SSD interno
- Anche dopo l’aggiornamento all’ultima versione
- I subprocessi ci sono ancora, ma **non rallentano il sistema**

---

## 🖥️ Specifiche del mio notebook

- **Sistema operativo:** Windows 10 Pro 64 bit – 22H2 (build 19045.5854)
- **Processore:** Intel Core i7-3630QM @ 2.40GHz
- **RAM:** 16 GB (15.9 GB utilizzabili)
- **Scheda grafica:** NVIDIA GeForce GT 740M (2 GB) + Intel HD Graphics 4000
- **Disco interno (C:):** SSD SATA da 1 TB
- **Disco esterno (F:):** SSD SATA da 500 GB (collegato via USB)

---

## 📦 Informazioni sul progetto

Il progetto che utilizzo con Cursor AI **non è un semplice file `.py` da poche righe**, ma una struttura reale composta da:

- Circa 25 file Python, organizzati in moduli
- Script di interfaccia web (Dash/Plotly) e gestione utente
- **Database NoSQL:** utilizzo **MongoDB**
- **Storage attivo:** directory con oltre 2.5 GB di contenuti (file JSON, binari, cache, dati serializzati)
- Più file vengono aperti contemporaneamente durante il lavoro

---

## 💡 Consigli per altri utenti

- Spostate **cache e progetto** su un SSD esterno se il vostro disco C: è sotto pressione
- Create un **symlink** per evitare problemi di percorso
- Non perdete tempo con `CURSOR_MAX_WORKERS`, almeno per ora
- (Facoltativo) Usate **Process Lasso** per dare priorità bassa a Cursor e controllare i processi

---

Spero che questa esperienza possa aiutare altri nella community. 
