# Placa de Iluminação Weiss

Placa de acionamento de iluminação desenvolvida em **KiCad**, responsável por controlar dois LEDs com o objetivo de iluminar a parte interna do weiss através de uma chave (push-button), a partir de uma alimentação externa de **3.3V**. O projeto foi desenhado para ser simples e robusto, priorizando facilidade de montagem (todos os componentes em THT) e baixo custo.

## Visão geral do circuito

O circuito é alimentado externamente via um conector **JST XH de 2 vias**, que entrega +3.3V e GND à placa. Essa alimentação passa por um **push-button (SW1)**, que atua como chave liga/desliga geral do circuito. A partir do botão, a alimentação se divide em **dois ramos em paralelo**, cada um composto por um resistor limitador de corrente em série com um LED de alta potência:

![Layout da PCB](img/pcb_layout.jpeg)


Ou seja: quando o botão é pressionado (ou travado, dependendo do tipo de chave utilizada), o circuito fecha e os dois LEDs acendem simultaneamente. Cada LED possui seu próprio resistor, de forma que o brilho e a corrente de cada um são definidos de forma independente — isso evita que uma eventual diferença entre os dois LEDs (tensão direta, tolerância) afete o outro ramo, como aconteceria se estivessem em paralelo direto sob um único resistor.

Foram incluídas **PWR_FLAGs** no esquemático em +3.3V e GND. Elas não fazem parte do circuito fisicamente: servem apenas para informar ao ERC (Electrical Rules Checker) do KiCad que esses nós de alimentação são supridos por uma fonte externa (o JST), evitando falsos positivos de "net não alimentada".

## Componentes

| Referência | Componente | Footprint / Símbolo | Função |
|---|---|---|---|
| **JST1** | Conector JST XH 2 vias | `Connector_JST:JST_XH_B2B-XH-A_1x02_P2.50mm_Vertical` | Entrada de alimentação externa (+3.3V e GND) |
| **SW1** | Push-button | `Button_Switch_SMD:SW_SPST_B3S-1000` (footprint na placa: `SW_SPST_Omron_B3F-40xx-CRD`) | Chave de acionamento (liga/desliga) do circuito |
| **R1** | Resistor 15Ω | `Resistor_THT:R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal` | Limitador de corrente do LED U1 |
| **R2** | Resistor 15Ω | `Resistor_THT:R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal` | Limitador de corrente do LED U2 |
| **U1** | LED de alta potência | `LED_THT:LED_D5.0mm` | Iluminação (ramo 1) |
| **U2** | LED de alta potência | `LED_THT:LED_D5.0mm` | Iluminação (ramo 2) |

> Os símbolos `JST_basic`, `Push_button_4_pin` e `Led_High` são customizados e estão definidos na biblioteca local `CRD_KICAD.kicad_sym`, criada para padronizar os componentes usados nos projetos da Cactus Rockets.

## Funcionamento

O funcionamento da placa é puramente analógico/passivo — não há microcontrolador ou lógica embarcada:

1. A alimentação de +3.3V chega pelo conector **JST1**.
2. O **SW1** atua como interruptor: enquanto estiver fechado, a corrente flui para os dois ramos de LED.
3. Cada ramo (R + LED) limita a corrente que passa pelo respectivo LED, garantindo que ele opere dentro da faixa segura especificada pelo fabricante, evitando queima por excesso de corrente.
4. O retorno de ambos os ramos é feito para o **GND** comum da placa, que fecha o circuito de volta à fonte externa.

Por não haver controle ativo (PWM, microcontrolador, etc.), o brilho dos LEDs é fixo e definido unicamente pelo valor dos resistores e pela tensão de alimentação — não há dimerização nem controle remoto nesta versão da placa.

## Cálculo dos resistores limitadores

O valor do resistor limitador é definido pela equação pela lei de ohm:

```
R = (V_fonte - V_LED) / I_LED
```

Com `V_fonte = 3.3V` e `R = 15Ω`, a corrente em cada ramo depende da tensão direta (V_f) do LED utilizado. Por exemplo:

- Para um LED com V_f ≈ 2.0V → I ≈ (3.3 − 2.0) / 15 ≈ **87 mA**
- Para um LED com V_f ≈ 3.0V → I ≈ (3.3 − 3.0) / 15 ≈ **20 mA**

## Placa (PCB)

- Placa de **1.6mm** de espessura (FR4 padrão), com contorno definido em `Edge.Cuts`.
- Todos os componentes são **THT** (through-hole), facilitando a montagem manual e a soldagem.
- Footprints utilizados:
  - `CRD_Footprints:SW_SPST_Omron_B3F-40xx-CRD` (SW1)
  - `LED_THT:LED_D5.0mm` / `CRD_Footprints:LED_D5.0mm-CRD` (U1, U2)
  - `Resistor_THT:R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal` (R1, R2)
  - `Connector_JST:JST_XH_B2B-XH-A_1x02_P2.50mm_Vertical` (JST1)

## Arquivos do projeto

| Arquivo | Descrição |
|---|---|
| `Placa_de_Iluminação_Weiss.kicad_sch` | Esquemático elétrico da placa |
| `Placa_de_Iluminação_Weiss.kicad_pcb` | Layout da placa (PCB) |
| `Placa_de_Iluminação_Weiss.kicad_pro` | Arquivo de projeto do KiCad |
| `CRD_KICAD.kicad_sym` | Biblioteca de símbolos customizados da Cactus Rockets |
| `fp-lib-table` | Tabela de bibliotecas de footprints usadas no projeto |

