# Projecte Llums Cotxe

> **Autors:** Marcel Lleonart, Eric Ruiz, Josep Leyva  
> **Versió:** 1.0  

---

## Objectiu

> Desenvolupar un **sistema de control de llums per a un automòbil a escala de projecte**, amb control mitjançant microcontrolador. El sistema inclou:  
> - Llums diürnes (DRL)  
> - Intermitents esquerra i dreta  
> - Llums de creuament  
> - Llums de carretera  
>  
> Cada mòdul inclou el **driver de potència** amb MOSFET, protecció amb fusible i control del microcontrolador via GPIOs. També s’implementa la **detecció de llum ambiental** per activar automàticament les llums diürnes.

---

## Diagrama de blocs
<img width="788" height="442" alt="Diagrama de blocs" src="https://github.com/user-attachments/assets/dea45409-d07e-40ad-a383-354cab6ba3d9" />

---

### Descripció / Funcionalitat de cada bloc

- **Microcontrolador PIC18F26Q83**: Coordina tots els mòduls de llums i gestiona les comunicacions amb els sensors i altres mòduls via bus CAN, I2C o USART.  
- **Driver DRL**: Controla les llums diürnes amb AO3400A com a interruptor de baixa latència. Activat automàticament pel sensor de llum.  
- **Driver Intermitents**: Controla intermitents esquerra i dreta amb AO3400A, inclou lògica de parpelleig al microcontrolador.  
- **Driver Llums de Cruce**: Control ON/OFF per a llums de creuament, amb protecció per corrent.  
- **Driver Llums de Carretera**: Control ON/OFF per a llums de carretera, amb fusible adequat segons tipus de llum (LED o halògena).  
- **Sensor de llum digital**: Detecta intensitat lumínica per activar DRL automàticament.  

---

## Requisits / Especificacions

- Alimentació: **12V** per llums, **5V regulada** per microcontrolador.  
- Microcontrolador: **PIC18F26Q83-I/SS**  
- Comunicacions: CAN, I2C, USART per depuració i control.  
- Protecció: Fusible en cada línia de llum i pull-down/serie resistències en gates de MOSFET.  
- Tipus de MOSFET: AO3400A (logic-level, canal N).  
- Senyals de control: GPIO 3.3V per microcontrolador.  

---

## Components

| Descripció | Ref | Package | Datasheet | Proveïdor | Preu | Unitats |
| --- | --- | --- | --- | --- | --- | --- |
| Microcontrolador | PIC18F26Q83-I/SS | SOIC-28 | [Datasheet](https://www.mouser.es/datasheet/2/268/PIC18F27_47_57Q83_Preliminary_Data_Sheet_40002265B-2887591.pdf) | [Mouser](https://www.mouser.es/c/?q=PIC18F27Q83-I%2FSO) | 2,17€ | 1x |
| XTAL-Ressonador | CSTCR7M99G53-R0 | SMD | [Datasheet](https://www.mouser.es/datasheet/2/281/p16e-522700.pdf) | [Mouser](https://www.mouser.es/ProductDetail/Murata-Electronics/CSTCR7M99G53-R0?qs=Zd9RUO93%2Fo7cnwzsujIkpA%3D%3D) | 0,27€ | 1x |
| MOSFET N-channel | AO3400A | SOT-23 | [Datasheet](https://www.diodes.com/assets/Datasheets/AO3400A.pdf) | [Digi-Key](https://www.digikey.com/en/products/detail/diodes-incorporated/AO3400A/1426866) | 0,55€ | 5x |
| Fusible | F1A/250V | SMD/PCB | - | Local | 0,15€ | 5x |
| Resistència 100Ω | R-100 | SMD | - | Local | 0,02€ | 10x |
| Resistència 10kΩ | R-10k | SMD | - | Local | 0,02€ | 10x |
| Lamp 12V | LED/Bombilla | - | - | Local | 1,50€ | 4x |
| Sensor de llum | TSL2561 | SMD/I2C | [Datasheet](https://www.adafruit.com/product/439) | [Adafruit](https://www.adafruit.com/) | 5,00€ | 1x |

---

### Eines

- KiCad 9.0 o superior per a esquemàtic i PCB  

### Configuració

- GPIO assignats:  
  - `DRL_CTRL` → Llums diürnes  
  - `TURN_LEFT_CTRL` → Intermitent esquerre  
  - `TURN_RIGHT_CTRL` → Intermitent dret  
  - `LOW_BEAM_CTRL` → Llums de creuament  
  - `HI_BEAM_CTRL` → Llums de carretera
  - `LIGHT_INT` → Interupcio sensor llum
- Temporitzadors interns per parpelleig dels intermitents  
- I2C per sensor de llum  
- USART per depuració  

### Funcionalitats

- Encès automàtic DRL segons sensor de llum  
- Parpelleig d’intermitents a 1 Hz  
- Control ON/OFF llums creuament i carretera  
- Protecció de corrent amb fusibles i resistències de gate  

---
