# CRC for UART

Il CRC usato è un singolo byte.\
Il CRC per i messaggi UART è calcolato senza considerare i byte relativi all'inizio e alla fine del messaggio (# e \*) e al CRC stesso. Per semplicità i separatori interni ( : o ; o | o altri) sono considerati.\
La formula per calcolarlo è la seguente:

```
uint8_t return_crc8(const uint8_t *data, int starting_index, int ending_index)
{
  uint8_t crc = 0x00;
  const uint8_t poly = 0x07;

  for (size_t i = starting_index; i <= ending_index; i++) {
    crc ^= data[i];
    for (uint8_t bit = 0; bit < 8; bit++) {
      if (crc & 0x80) {
        crc = (uint8_t)((crc << 1) ^ poly);
      } else {
        crc <<= 1;
      }
    }
  }

  return crc;
}
```
