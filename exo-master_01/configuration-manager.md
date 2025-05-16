# Configuration Manager

## Requirements

1. Il componente deve essere una libreria statica accessibile in sola lettura da tutti gli altri componenti.
2. Il componente deve leggere i dati di configurazione una sola volta all'accensione.
3. I dati devono essere letti da più file .json salvati dentro una scheda microSD.
4. I dati possono essere singoli o array.
5. I gruppi di dati da leggere sono i seguenti:
   1. configurazioni comuni ad una serie o variante
      1. cfg\_main: dati di configurazione generali del dispositivo
      2. cfg\_hardware: configurazioni relative al cablaggio e all'hardware (elettrico e meccanico) installato
      3. cfg\_collision: struttura di dati per gestire il collision manager
   2. configurazioni che variano in ogni dispositivo
      1. cfg\_serial\_number: numero di serie del dispositivo
      2. cfg\_sensor\_offset: configurazioni relative agli zeri dei sensori
   3. configurazioni utili in fase di sviluppo e debug, disattivate in produzione
      1. cfg\_compiler: per utilizzo in debug

## Test list

1. TBD
