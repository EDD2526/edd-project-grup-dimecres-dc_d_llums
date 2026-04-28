# Projecte Llums Cotxe

> **Autors:** Marcel Lleonart, Eric Ruiz, Josep Leyva
> **Versió:** 1.1
> **Eina de disseny:** KiCad 9.0 o superior

---

##  Objectiu

Desenvolupar un **sistema de control de llums per a un automòbil a escala de projecte**, gestionat mitjançant un microcontrolador. El sistema integra els principals subsistemes d'il·luminació d'un vehicle i permet el seu control manual i automàtic.

### Funcions principals

* Llums diürnes (**DRL**)
* Intermitents esquerre i dret
* Llums de creuament
* Llums de carretera
* Activació automàtica segons la llum ambiental

Cada canal incorpora:

* Etapa de potència amb MOSFET
* Resistència de gate i pull-down
* Control directe des del microcontrolador mitjançant GPIO

---

##  Diagrama de blocs

<p align="center">
  <img width="1097" height="613" alt="image" src="https://github.com/user-attachments/assets/7cd6f459-77a8-4090-bef5-ecb2085d7b2a" />
</p>

---

##  Descripció funcional dels blocs

###  Microcontrolador – PIC18F26Q83

Coordina tots els subsistemes de llum, processa les entrades dels sensors i gestiona les comunicacions externes. Disposa d'interfícies **CAN**, **I2C** i **USART** per a expansió, diagnosi i depuració.

###  Driver de llums diürnes (DRL)

Controla l'encesa automàtica de les llums diürnes mitjançant un MOSFET **AO3400A**. L'activació es realitza segons la lectura del sensor de llum ambiental.

###  Driver d'intermitents

Gestiona els intermitents esquerre i dret. El parpelleig es genera per programari utilitzant temporitzadors interns del microcontrolador.

###  Driver de llums de creuament

Permet el control ON/OFF de les llums de creuament amb protecció elèctrica individual.

###  Driver de llums de carretera

Controla les llums de carretera amb protecció mitjançant fusible. Compatible amb càrregues LED o halògenes de baixa potència.

### Sensor de llum ambiental

Mesura la il·luminació exterior i permet l'activació automàtica de les llums diürnes o altres funcions programades.

---

##  Especificacions tècniques

| Paràmetre              | Valor                                               |
| ---------------------- | --------------------------------------------------- |
| Alimentació principal  | 12 Vcc                                              |
| Alimentació lògica     | 5 V regulats                                        |
| Microcontrolador       | PIC18F26Q83-I/SS                                    |
| Comunicacions          | CAN, I2C, USART                                     |
| MOSFET de potència     | AO3400A (canal N, logic-level)                      |
| Nivell de control GPIO | 3.3 V / 5 V compatible                              |
| Protecció              | Fusible per canal + resistència de gate + pull-down |

---

##  Assignació de GPIO

| Senyal            | Funció                         |
| ----------------- | ------------------------------ |
| `DRL_CTRL`        | Control de llums diürnes       |
| `TURN_LEFT_CTRL`  | Intermitent esquerre           |
| `TURN_RIGHT_CTRL` | Intermitent dret               |
| `LOW_BEAM_CTRL`   | Llums de creuament             |
| `HI_BEAM_CTRL`    | Llums de carretera             |
| `LIGHT_INT`       | Interrupció del sensor de llum |

---

##  Funcionalitats implementades

* Activació automàtica de les DRL segons la llum ambiental
* Parpelleig d'intermitents a **1 Hz**
* Control independent de llums de creuament i carretera
* Protecció individual de cada sortida
* Interfície de depuració per USART
* Arquitectura preparada per integració en xarxa CAN

---

##  Llista de materials (BOM)

| Component                     | Valor / Model              | Encapsulat         | Quantitat |
| ----------------------------- | -------------------------- | ------------------ | --------: |
| Microcontrolador              | PIC18F26Q83-I/SS           | SOIC-28            |         1 |
| MOSFET canal N                | AO3400A                    | SOT-23             |         8 |
| Regulador lineal              | LM1117-5.0                 | TO-252             |         1 |
| Resonador / Cristall          | 8 MHz                      | HC-49 o equivalent |         1 |
| Inductor                      | 33 µH                      | Radial             |         1 |
| Díodes de protecció           | Rectificador               | DO-201AD           |         9 |
| Condensadors de desacoblament | 100 nF                     | 0805               |         7 |
| Condensadors                  | 10 µF                      | 0805               |         3 |
| Condensador electrolític      | 680 µF                     | Radial             |         1 |
| Condensadors                  | 33 pF                      | 0805               |         2 |
| Condensador                   | 4.7 nF                     | 0805               |         1 |
| Resistències                  | 100 Ω                      | 0805               |         8 |
| Resistències                  | 10 kΩ                      | 0805               |  Diverses |
| Borns de connexió             | 2 vies, pas 5.08 mm        | Terminal Block     |        11 |
| Connector ICSP                | Programació PIC            | 2x03, 2.54 mm      |         1 |
| Interruptors SPDT             | Selector                   | Through-hole       |         5 |
| Polsador                      | Reset / funció             | 6 mm               |         1 |
| Sensor de llum                | TSL2561 (o equivalent I2C) | SMD                |         1 |

> **Nota:** La quantitat exacta d'alguns components passius pot variar segons la revisió del disseny i les opcions de muntatge.

---

##  Programació i configuració

### Perifèrics utilitzats

* GPIO digitals per al control de sortides
* Temporitzadors interns per al parpelleig
* I2C per al sensor de llum
* USART per a depuració i diagnosi
* CAN per a futures ampliacions

### Seqüència de funcionament

1. Inicialització del microcontrolador i perifèrics.
2. Lectura del sensor de llum ambiental.
3. Activació automàtica de DRL si escau.
4. Gestió d'intermitents amb temporització periòdica.
5. Control independent de llums de creuament i carretera.
6. Supervisió contínua de les entrades i estats del sistema.

---

##  Proteccions incorporades

* Fusible independent per a cada sortida de llum
* Resistència sèrie a la gate dels MOSFET
* Resistència pull-down per evitar activacions espúries
* Filtrat de l'alimentació mitjançant condensadors de desacoblament
* Protecció davant transitoris de commutació

---

##  Aplicacions

Aquest projecte és ideal per a:

* Maquetes i prototips d'automoció
* Sistemes d'il·luminació intel·ligents
* Pràctiques de microcontroladors i electrònica de potència
* Desenvolupament de mòduls electrònics per automoció

---

##  Estat del projecte

**Versió actual:** 1.5

* ✅ Esquemàtic completat
* 🔄 PCB en desenvolupament i validació
* 💻 Firmware base en implementació
* ✔️ BOM inicial verificada

---

## 📄 Llicència

Aquest projecte ha estat desenvolupat amb finalitats educatives i acadèmiques.

---
