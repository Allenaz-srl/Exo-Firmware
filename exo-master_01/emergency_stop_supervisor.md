---
description: >-
  Questo componente ha lo scopo di monitorare le posizioni e le velocità degli
  assi e "premere il fungo" qualora riconosca un comportamento anomalo.
---

# Emergency\_stop\_supervisor

## Requirements

1. Il componente deve essere una libreria che viene instanziata dal main.
2. Deve essere periodicamente lanciata all'interno di other\_loop del main.
3. Il costruttore deve ricevere i seguenti parametri:
   1. pin di MOTOR POWER ENABLE
4. Durante il funzionamento deve ricevere i dati di posizione e velocità aggiornati con frequenza pari al loop main.
5. Effettua per ogni asse i seguenti controlli sulla posizione:

```mermaid

flowchart TD
    A((Start)) --> B{actual_position > drive_min_position}
    B -->|YES| C
    B -->|NO| errore1[ERRORE1]
    errore1-->C{actual_position < drive_max_position}
    C -->|YES| D
    C -->|NO| errore2[ERRORE2]
    errore2-->D{limited ROM active}
    D -->|YES| E{actual_position > ROM_min_position}
    E -->|YES| F
    E -->|NO| errore3[ERRORE3]
    errore3-->F{actual_position < ROM_max_position}
    F -->|YES| G
    F -->|NO| errore4[ERRORE4]
    errore4-->G{limited ROM active}
    G-->|NO| fine((End))
    D -->|NO| fine
```



6. Effettua per ogni asse i seguenti controlli sulla velocità:

```mermaid
flowchart TD
    A((Start)) --> B{actual_frequency>max_frequency            OR             actual_frequency<-max_frequency}
    B -->|YES| errore5[ERRORE5]
    errore5 --> C
    B -->|NO| C[controllo velocità  oltre i limiti per fermarsi: lo stesso fatto dal drive]
    C -->D[Switch su WORKING_MODE e controllare se frequenze sono corrette con il relativo working mode]
    D-->fine((End))
```

## Test list

1. TBD
