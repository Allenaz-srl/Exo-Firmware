---
description: 'Ci sono tre tipologie di componenti hardware elettroni nel dispositivo:'
---

# Hardware schematics

1. Raspberry Pi 4 B
   * Ubuntu Server v.24.04
   * Componente software: Front\_End e Back\_End&#x20;
   * Linguaggio:Node JS
2. Arduino Nano ESP32
   * Componente software: Master e Driver
   * Linguaggio:C
3. Componenti che contengono firmware di terze parti
   * Sensori e azionamenti

```mermaid
flowchart RL
subgraph sg4[third party]
SENSORS
MOOTOR[[MOTOR_CONTROLLER]]
end
subgraph sg3b[Arduino Nano ESP32]
DRIVER1[DRIVER]
end
subgraph sg3a[Arduino Nano ESP32]
DRIVER2[DRIVER]
end
subgraph sg2[Arduino Nano ESP32]
MASTER
end
subgraph sg1[Raspberry PI 4 B]
Front_End <-->  Back_End 
end

sg1 -.->sg1_note["Responsabile di HMI,\n log e funzioni alto livello"]
sg2 -.->sg2_note["Responsabile del coordinamento dei Driver"]
sg3a -.->sg3a_note["Responsabile della lettura dei sensori\n e del controllo dei motori \n per gli assi 1 e 2"]
sg3b -.->sg3b_note["Responsabile della lettura dei sensori\n e del controllo dei motori \n per gli assi 3 e 4"]

```
