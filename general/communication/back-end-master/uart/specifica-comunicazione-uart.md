# Specifica comunicazione UART

## 📘 Introduzione

La comunicazione avviene tramite una serie di registri identificati da ID numerici. Ogni registro rappresenta un parametro definito, un formato e una direzione di comunicazione univoca.

I registri sono classificati in base al tipo di dato trasportato, secondo i seguenti intervalli:

| Intervallo     | Tipo di dato     | Descrizione         |
| -------------- | ---------------- | ------------------- |
| 1 < ID < 9     | String           | stringhe            |
| 10 < ID < 49   | uint\_8          | unsigned int 1 byte |
| 50 < ID < 199  | int\_16          | intero 2 byte       |
| 200 < ID < 399 | float            | float               |
| 400 < ID < 499 | boolean          | bool                |
| 500 < ID < 599 | long int (int32) | long int (4 byte)   |

È anche possibile raggruppare i registri in base alla loro funzione logica come segue:

<table><thead><tr><th width="211">Gruppo</th><th>From->To</th><th width="387">Descrizione</th></tr></thead><tbody><tr><td>Parametri operativi</td><td>HMI->Master</td><td>-10 Working mode<br>-11 Test cycle<br>-50-54 Posizioni angolari da raggiungere<br>-55-59 Posizioni cartesiane da raggiungere<br>-85-89 Velocità massima del singolo movimento</td></tr><tr><td>Informazioni all'avvio</td><td>Master->HMI</td><td>-1 Numero di serie<br>-2-3 Numero di assi<br>-7-8 Versione del software</td></tr><tr><td>Informazioni durante il funzionamento</td><td>Master->HMI</td><td>-100 Percentuale di carica della batteria</td></tr><tr><td>Settaggi all'avvio</td><td>Master->HMI</td><td>-160-164 Pos min per ogni asse<br>-170-174 Pos max per ogni asse<br>Per ogni sensore di posizione:<br>-140-144 Zero<br>-130-134 Risoluzione<br>-200-204 Corsa<br>-400-404 Direzione</td></tr><tr><td>Settaggi da interfaccia</td><td>HMI->Master</td><td><p>-70-74 Limite di velocità utente<br>-85-89 Limite di velocità avanzato</p><p>-90-93 Regolazioni movimento assistito<br>-101-111 Limiti di posizione (ROM)</p></td></tr></tbody></table>



***

#### 🔹 Parametri operativi: Working Mode

<table><thead><tr><th width="88">Registro</th><th width="521">Descrizione</th><th>Direzione</th><th data-hidden>Tipo</th></tr></thead><tbody><tr><td>10</td><td>Modalità di lavoro da attivare</td><td>HMI → Master</td><td>int</td></tr></tbody></table>

#### 🔹 Parametri operativi: Test Cycle

<table><thead><tr><th width="88">Registro</th><th width="521">Descrizione</th><th>Direzione</th><th data-hidden>Tipo</th></tr></thead><tbody><tr><td>11</td><td>Numero del ciclo di test</td><td>HMI → Master</td><td>int</td></tr></tbody></table>

#### 🔹 Parametri operativi: Posizioni Angolari da raggiungere

#### Con questi registri vengono passati i valori delle posizioni che ogni asse deve raggiungere nel working mode TARGET POSITION. Può essere passata anche solo una posizione. Il dato è in bit e non in deg.

<table><thead><tr><th width="118">Registro</th><th width="496">Descrizione</th><th>Direzione</th></tr></thead><tbody><tr><td>50–>Asse 1<br>51–>Asse 2<br>52–>Asse 3<br>53–>Asse 4<br>54–>Asse 5</td><td>Posizione target per ciascun asse </td><td>HMI → Master</td></tr></tbody></table>

#### 🔹 Parametri operativi: Posizioni Cartesiane da raggiungere

Con questi registri vengono passati i valori delle posizioni cartesiane dell'estremità dell'esoschetro da raggiungere. La conversione dello spazio degli assi viene fatta internamente. che ogni asse deve raggiungere nel working mode TARGET POSITION. Può essere passata anche solo una posizione. Il dato è in bit e non in deg.\
Il ciclo prevede uno start me



| Registro                                      | Tipo  | Descrizione                       | Direzione    |
| --------------------------------------------- | ----- | --------------------------------- | ------------ |
| 55                                            | int   |                                   | HMI → Master |
| <p>56–>Asse 1<br>57–>Asse 2<br>58–>Asse 3</p> | int   | Posizione cartesiana target (XYZ) | HMI → Master |
| <p>208–>G3<br>209->G5</p>                     | float |                                   |              |
| 59                                            | int   |                                   | Master→ HMI  |

#### 🔹 Velocità Massima Movimento

| Registro | Tipo | Descrizione               | Direzione    |
| -------- | ---- | ------------------------- | ------------ |
| 59       | int  | Velocità massima per asse | HMI → Master |

***

### 🔍 Informazioni all’Avvio

Registri fondamentali per l’identificazione del sistema al momento dell’accensione. Il Master comunica informazioni strutturali e di sistema all’HMI.

#### 🔹 Numero di Assi

| Registro | Tipo   | Descrizione                | Direzione    |
| -------- | ------ | -------------------------- | ------------ |
| 3        | String | Numero di assi disponibili | Master → HMI |

#### 🔹 Numero di Serie

| Registro | Tipo   | Descrizione                    | Direzione    |
| -------- | ------ | ------------------------------ | ------------ |
| 1        | String | Codice seriale del dispositivo | Master → HMI |

#### 🔹 Versione Software

| Registro | Tipo   | Descrizione                  | Direzione    |
| -------- | ------ | ---------------------------- | ------------ |
| 8        | String | Versione software del Master | Master → HMI |

***

### 🛠️ Settaggi all’Avvio

Contiene registri relativi ai parametri statici per ogni asse, come posizioni min/max e configurazioni dei sensori.

#### 🔹 Posizione Min/Max

| Registro | Tipo | Descrizione                           | Direzione    |
| -------- | ---- | ------------------------------------- | ------------ |
| 100–110  | int  | Posizioni min e max ROM per ogni asse | Master → HMI |

#### 🔹 Zero del Sensore

| Registro | Tipo | Descrizione                             | Direzione    |
| -------- | ---- | --------------------------------------- | ------------ |
| 120–124  | int  | Valore di zero per sensore di posizione | Master → HMI |

#### 🔹 Risoluzione

| Registro | Tipo | Descrizione                  | Direzione    |
| -------- | ---- | ---------------------------- | ------------ |
| 130–134  | int  | Risoluzione encoder per asse | Master → HMI |

#### 🔹 Corsa

| Registro | Tipo | Descrizione             | Direzione    |
| -------- | ---- | ----------------------- | ------------ |
| 140–144  | int  | Lunghezza corsa sensore | Master → HMI |

#### 🔹 Direzione

| Registro | Tipo    | Descrizione                            | Direzione    |
| -------- | ------- | -------------------------------------- | ------------ |
| 400–404  | boolean | Direzione movimento (normal/invertita) | Master → HMI |

***

### 🧩 Settaggi da Interfaccia

Parametri configurabili dall’utente tramite HMI.

#### 🔹 Limite Velocità Utente

| Registro | Tipo | Descrizione                     | Direzione    |
| -------- | ---- | ------------------------------- | ------------ |
| 150      | int  | Velocità max definita da utente | HMI → Master |

#### 🔹 Velocità Avanzata

| Registro | Tipo | Descrizione              | Direzione    |
| -------- | ---- | ------------------------ | ------------ |
| 151      | int  | Limite velocità avanzato | HMI → Master |

#### 🔹 Limiti di Posizione (ROM)

## 🧭 Comandi Base (String)

I registri in questo gruppo (ID tra 1 e 9) contengono comandi o informazioni generali, inviati come stringhe. Sono utilizzati per la comunicazione iniziale, versioni software o scambi simbolici tra HMI e Master.

| ID | Descrizione                                    | Tipo   | Asse | Direzione  |
| -- | ---------------------------------------------- | ------ | ---- | ---------- |
| 1  | Serial Number (ask and response with 1)        | String |      | HMI/Master |
| 2  | 0 (ask for n. axes)                            | String |      | HMI        |
| 3  | Number of Axis                                 | String |      | Master     |
| 4  | Direct transaction of data (front-back-Master) | String |      | HMI        |
| 5  | 0 - left; 1 - right (BTN true = click)         | String |      | HMI        |

***

## 🗂️ Gruppi di Registri

Questa tabella riassume i principali gruppi di registri, ognuno caratterizzato da un intervallo di ID, un tipo di dato, e una funzione coerente all’interno del protocollo.

| Range ID | Nome Gruppo            | Tipo Dato | Descrizione                                    | Direzione    |
| -------- | ---------------------- | --------- | ---------------------------------------------- | ------------ |
| 1–9      | Comandi Base           | String    | Identificativi, versioni, comandi testuali     | HMI ↔ Master |
| 10–49    | Parametri Operativi    | int       | Modalità operative, flag, opzioni              | HMI → Master |
| 50–54    | Posizioni Target       | int       | Posizione desiderata per ogni asse             | HMI → Master |
| 200–204  | Esercizi               | float     | Dati analogici per esercizi asse per asse      | HMI ↔ Master |
| 400–404  | Sensori Booleani       | boolean   | Stato on/off di sensori digitali per ogni asse | Master → HMI |
| 600–604  | Sensori ad Alta Risol. | long int  | Posizioni o misure con elevata precisione      | Master → HMI |
