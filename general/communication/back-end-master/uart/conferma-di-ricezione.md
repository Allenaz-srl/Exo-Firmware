# Conferma di ricezione

Tutti i messaggi che non sono risposta ad una richiesta devono inviare una conferma di ricezione inviando il seguente formato:\
\
\#address\_ricevuto:0%CRC\*

Lo zero deve essere inviato con il formato corretto del gruppo dell'address ( int, float, bool, int32, etc).
