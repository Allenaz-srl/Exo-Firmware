# Battery manager

## Requirements

1. Il componente deve essere una libreria che viene instanziata all'accensione.
2. Deve misurare la tensione della batteria con una media sufficientemente lunga da avere un valore stabile e affidabile.
   1.
3. Deve ricavarne un livello di carica da 0% a 100% in base ai seguenti parametri caricati nei file di configurazione:
   1.
4. Deve comunicare questo livello di carica all'HMI.
5. Deve generare i seguenti messaggi:
   1. warning di tensione della batteria troppo alta (in base ai parametri definiti nei file di configurazione che dipendono dal tipo di batteria installato, tipicamente se superiore al 110% della massima tensione della batteria).
   2. errore di batteria scarica



## Test list

1. TBD
