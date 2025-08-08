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

<table><thead><tr><th width="211">Gruppo</th><th>From->To</th><th width="387">Descrizione</th></tr></thead><tbody><tr><td>Parametri operativi</td><td>HMI->Master</td><td>-10 Working mode<br>-11 Test cycle<br>-50-54 Posizioni angolari da raggiungere<br>-80-84 Velocità massima del singolo movimento<br>-101-104 e 210-211 Posizioni cartesiane da raggiungere<br>-109 Attiva e disattiva limiti di posizione (ROM)</td></tr><tr><td>Informazioni all'avvio</td><td>Master->HMI</td><td>-1 Numero di serie<br>-2 Numero di assi<br>-7 Versione del software</td></tr><tr><td>Informazioni durante il funzionamento</td><td>Master->HMI</td><td>-100 Percentuale di carica della batteria</td></tr><tr><td>Settaggi all'avvio</td><td>Master->HMI</td><td><p>-160-164 Pos min per ogni asse<br>-170-174 Pos max per ogni asse<br>Per ogni sensore di posizione:<br>-130-134 Risoluzione</p><p>-140-144 Zero<br>-200-204 Corsa<br>-400-404 Direzione</p></td></tr><tr><td>Settaggi da interfaccia</td><td>HMI->Master</td><td><p>-60-64 Limite di velocità avanzato</p><p>-70-74 Limite di velocità utente</p><p>-90-93 Regolazioni movimento assistito<br>-110-114 Pos min ROM per ogni asse<br>-120-124 Pos max ROM per ogni asse</p></td></tr></tbody></table>



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



| Registro                          | Tipo  | Descrizione                                                            | Direzione    |
| --------------------------------- | ----- | ---------------------------------------------------------------------- | ------------ |
| 101                               | int   | 0 - START MSG; 1 - Go without check; 2 - Go with check; 3 - check only | HMI → Master |
| <p>102–>X<br>103–>Y<br>104–>Z</p> | int   | Posizione cartesiana target (XYZ)                                      | HMI → Master |
| <p>210–>G3<br>211->G5</p>         | float |                                                                        |              |
| 101                               | int   | 0 - no error; 1 - pos not reachable                                    | Master→ HMI  |

#### 🔹  Parametri operativi: Velocità Massima Movimento

| Registro                                                                  | Tipo | Descrizione                                                                 | Direzione    |
| ------------------------------------------------------------------------- | ---- | --------------------------------------------------------------------------- | ------------ |
| <p>80–>Asse 1<br>81–>Asse 2<br>82–>Asse 3<br>83–>Asse 4<br>84–>Asse 5</p> | int  | Velocità massima per asse da applicare all'interno della sessione corrente. | HMI → Master |

***

### 🔍 Informazioni all’Avvio

Registri fondamentali per l’identificazione del sistema al momento dell’accensione. Il Master comunica informazioni strutturali e di sistema all’HMI. Lo stesso registro è usato dall'HMI per chiedere il dato. Il Master lo invia unicamente come risposta alla richiesta.

#### 🔹 Numero di Serie

| Registro | Tipo   | Descrizione                    | Direzione      |
| -------- | ------ | ------------------------------ | -------------- |
| 1        | String | Codice seriale del dispositivo | Master <-> HMI |

#### 🔹 Numero di Assi

| Registro | Tipo   | Descrizione                | Direzione      |
| -------- | ------ | -------------------------- | -------------- |
| 2        | String | Numero di assi disponibili | Master <-> HMI |

#### 🔹 Versione Software

| Registro | Tipo   | Descrizione                  | Direzione      |
| -------- | ------ | ---------------------------- | -------------- |
| 7        | String | Versione software del Master | Master <-> HMI |

***

### Informazioni durante il funzionamento

Contiene registri relativi ai parametri statici per ogni asse, come posizioni min/max e configurazioni dei sensori.

| Registro | Tipo | Descrizione                          | Direzione    |
| -------- | ---- | ------------------------------------ | ------------ |
| 100      | int  | Percentuale di carica della batteria | Master → HMI |

#### 🔹 Zero del Sensore

### 🛠️ Settaggi all’Avvio



Contiene registri relativi ai parametri statici per ogni asse, come posizioni min/max e configurazioni dei sensori.

#### 🔹 Posizione Min ASSE nel file di configurazione (pos\_min nel file cfg)

<table><thead><tr><th width="258.111083984375">Registro</th><th>Tipo</th><th>Descrizione</th><th>Direzione</th></tr></thead><tbody><tr><td>160–>Asse 1<br>161–>Asse 2<br>162–>Asse 3<br>163–>Asse 4<br>164–>Asse 5</td><td>int</td><td>Posizioni min per ogni asse</td><td>Master → HMI</td></tr></tbody></table>

#### 🔹 Posizione Min ASSE nel file di configurazione (pos\_max nel file cfg)

<table><thead><tr><th width="258.111083984375">Registro</th><th>Tipo</th><th>Descrizione</th><th>Direzione</th></tr></thead><tbody><tr><td>170–>Asse 1<br>171–>Asse 2<br>172–>Asse 3<br>173–>Asse 4<br>174–>Asse 5</td><td>int</td><td>Posizioni max per ogni asse</td><td>Master → HMI</td></tr></tbody></table>

#### 🔹 Risoluzione del sensore di posizione (position\_sensor\_resolution nel file cfg)

| Registro                                                                       | Tipo | Descrizione                                                                                                                                             | Direzione    |
| ------------------------------------------------------------------------------ | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| <p>130–>Asse 1<br>131–>Asse 2<br>132–>Asse 3<br>133–>Asse 4<br>134–>Asse 5</p> | int  | Risoluzione trasduttore di posizione per asse. è abbinata alla corsa. Le 2 insieme determinano le costanti per la conversione da bit a gradi angolari.  | Master → HMI |

#### 🔹 Zero del sensore di posizione (position\_sensor\_offset\_kinematics nel file cfg)

| Registro                                                                       | Tipo | Descrizione                                                                                | Direzione    |
| ------------------------------------------------------------------------------ | ---- | ------------------------------------------------------------------------------------------ | ------------ |
| <p>140–>Asse 1<br>141–>Asse 2<br>142–>Asse 3<br>143–>Asse 4<br>144–>Asse 5</p> | int  | Valore di zero trasduttore di posizione per asse per la traformazione bit-> gradi angolari | Master → HMI |

#### 🔹 Corsa del sensore di posizione (position\_sensor\_stroke nel file cfg)

| Registro                                                                       | Tipo  | Descrizione                                                                                                                                                 | Direzione    |
| ------------------------------------------------------------------------------ | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| <p>200–>Asse 1<br>201–>Asse 2<br>202–>Asse 3<br>203–>Asse 4<br>204–>Asse 5</p> | float | Corsa del trasduttore di posizione per asse. è abbinata alla risoluzione. Le 2 insieme determinano le costanti per la conversione da bit a gradi angolari.  | Master → HMI |

#### 🔹 Direzione del sensore di posizione (position\_sensor\_dir\_inverted\_kinematics nel file cfg)

| Registro                                                                       | Tipo    | Descrizione                                                                             | Direzione    |
| ------------------------------------------------------------------------------ | ------- | --------------------------------------------------------------------------------------- | ------------ |
| <p>400–>Asse 1<br>401–>Asse 2<br>402–>Asse 3<br>403–>Asse 4<br>404–>Asse 5</p> | boolean | Direzione movimento (normal/invertita). Si usa nella traformazione bit-> gradi angolari | Master → HMI |

***

### 🧩 Settaggi da Interfaccia

Parametri configurabili dall’utente tramite HMI.

#### 🔹 Range of motion limits(ROM): posizione Min/Max e enable

<table><thead><tr><th width="258.111083984375">Registro</th><th>Tipo</th><th>Descrizione</th><th>Direzione</th></tr></thead><tbody><tr><td>109-> ROM enable</td><td>int</td><td>0 – disable ROM limits; 1 – enable ROM limits</td><td>HMI → Master</td></tr><tr><td>110–> ROM min Asse 1<br>111–> ROM min Asse 2<br>112–> ROM min Asse 3<br>113–> ROM min Asse 4<br>114–> ROM min Asse 5</td><td>int</td><td>Posizioni min e max ROM per ogni asse</td><td>HMI → Master</td></tr><tr><td><p></p><p>120–> ROM max Asse 1</p><p>121–> ROM max Asse 2</p><p>122–> ROM max Asse 2</p><p>123–> ROM max Asse 4</p><p>124–> ROM max Asse 5</p></td><td>int</td><td>Posizioni min e max ROM per ogni asse</td><td>HMI → Master</td></tr></tbody></table>

#### 🔹 Limite Velocità Avanzata&#x20;

| Registro                                                                  | Tipo | Descrizione                                                                                          | Direzione    |
| ------------------------------------------------------------------------- | ---- | ---------------------------------------------------------------------------------------------------- | ------------ |
| <p>60–>Asse 1<br>61–>Asse 2<br>62–>Asse 3<br>63–>Asse 4<br>64–>Asse 5</p> | int  | Percentuale di velocità massima per asse da applicare sempre. Accessibile solo dall'utente avanzato. | HMI → Master |

***

#### 🔹 Limite Velocità Utente

| Registro                                                                  | Tipo | Descrizione                                                                                                           | Direzione    |
| ------------------------------------------------------------------------- | ---- | --------------------------------------------------------------------------------------------------------------------- | ------------ |
| <p>70–>Asse 1<br>71–>Asse 2<br>72–>Asse 3<br>73–>Asse 4<br>74–>Asse 5</p> | int  | Percentuale di velocità massima per asse da applicare sempre. Va in cascata  a quella avanzata. Accessibile da tutti. | HMI → Master |

#### 🔹 Regolazioni del movimento assistito (compliante)

<table><thead><tr><th width="308.11114501953125">Registro</th><th width="72.6666259765625">Tipo</th><th width="229.6666259765625">Descrizione</th><th>Direzione</th></tr></thead><tbody><tr><td>90–>Lunghezza braccio<br></td><td>int</td><td>Dato in mm relativo alla regolazione della lunghezza del braccio.</td><td>HMI → Master</td></tr><tr><td>91–>Percentuale peso braccio<br></td><td>int</td><td>Percentuale di peso da usare nella funzione torque predictor.</td><td>HMI → Master</td></tr><tr><td>92–>Lunghezza avanbraccio<br></td><td>int</td><td>Dato in mm relativo alla regolazione della lunghezza del avanbraccio.</td><td>HMI → Master</td></tr><tr><td>93–>Percentuale peso avanbraccio</td><td>int</td><td>Percentuale di peso da usare nella funzione torque predictor.</td><td>HMI → Master</td></tr></tbody></table>
