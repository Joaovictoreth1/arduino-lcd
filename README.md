# Leitor NFC com Arduino e LCD

Este exercício demonstra a leitura de tags NFC ISO 14443A com um Arduino UNO, um PN532 e um LCD 16×2, sem monitor serial. O sketch detecta tags compatíveis, mas não autentica usuários nem controla acesso.

## Compilar e executar

Com o Arduino CLI 1.6.0 instalado, execute na raiz do repositório:

```sh
arduino-cli core update-index
arduino-cli core install arduino:avr@1.8.8
arduino-cli lib install --no-deps "Adafruit BusIO@1.17.4" "Adafruit PN532@1.3.4" "LiquidCrystal@1.0.7"
arduino-cli compile --fqbn arduino:avr:uno leitor_nfc
```

Conecte o UNO e use `arduino-cli board list` para identificar a porta. Substitua `/dev/ttyACM0` pela porta correta:

```sh
arduino-cli upload --fqbn arduino:avr:uno --port /dev/ttyACM0 leitor_nfc
```

**Exemplo:** sem tag, o LCD mostra `Sistema Pronto` / `Aproxime a TAG`. Ao aproximar uma tag compatível, mostra `TAG ENCONTRADA!` / `Leitura OK` por três segundos. Se o PN532 não responder, mostra `NFC Falhou`; se a configuração falhar, mostra `Erro Config NFC`. A CI compila o sketch, mas não testa a fiação ou a leitura física.

## 💡 O "Pulo do Gato" deste Projeto

Quando tentamos ligar o leitor NFC (que usa comunicação muito rápida) e o ecrã LCD (que precisa de muitos fios) nos pinos normais do Arduino, a confusão de cabos é enorme e os conflitos são comuns.

**A solução que adotei:** Mudei todos os 6 fios de dados do ecrã LCD lá para o outro lado da placa, para as portas **Analógicas (do A0 ao A5)**. Assim, os pinos digitais ficaram livres para o leitor NFC respirar. Funcionou na perfeição!

---

## 🔌 Como eu liguei tudo (Esquema Físico)

Se quiseres recriar este projeto, esta é a "receita do bolo" exata que usei para que não falhasse:

### 1. O Leitor PN532 (Configurado para SPI)
*Nota importante:* Confirme as posições SPI e a tensão de alimentação e dos sinais no manual do seu módulo. A tabela pressupõe um módulo compatível com 5 V; outros módulos podem exigir 3,3 V e conversão de nível. No módulo usado no projeto, SPI corresponde à **Chave 1 OFF** e à **Chave 2 ON**.

| Fio do Leitor | Pino no Arduino |
| :--- | :--- |
| **VCC** | 5V |
| **GND** | GND |
| **SCK** | Pino 13 |
| **MISO** | Pino 12 |
| **MOSI** | Pino 11 |
| **SS / SDA** | Pino 10 |

### 2. O Ecrã LCD 16x2 (O verdadeiro desafio)

Este ecrã precisa de muita atenção. Qualquer fio frouxo e ele não mostra nada. 

| Pino do Ecrã LCD | Onde ligar | Detalhes |
| :--- | :--- | :--- |
| **1 (VSS)** | **GND** | O terra básico do ecrã. |
| **2 (VDD)** | **5V** | A energia para a placa. |
| **3 (V0)**  | **Potenciómetro** | Liga no pino do meio de um potenciómetro para regular a força das letras! |
| **4 (RS)**  | **A0** (Analógico) | |
| **5 (RW)**  | ⚠️ **GND** | *Super Crítico:* Se não ligares este pino ao GND (Terra), o ecrã nunca vai aceitar texto do Arduino. |
| **6 (E)**   | **A1** (Analógico) | |
| *(7 ao 10)* | *Vazios* | Deixa estes quatro de fora, não precisamos deles. |
| **11 (D4)** | **A2** (Analógico) | |
| **12 (D5)** | **A3** (Analógico) | |
| **13 (D6)** | **A4** (Analógico) | |
| **14 (D7)** | **A5** (Analógico) | |
| **15 (A)**  | **5V** | Liga a luz de fundo. |
| **16 (K)**  | **GND** | O terra da luz de fundo. |

---

## 🛠️ Dicas de quem já bateu cabeça com isto

Se montaste tudo igual a mim, ligaste o Arduino à corrente e **o ecrã ficou azul, mas as letras não apareceram**, tenta isto antes de desesperar:

1. **Roda o Potenciómetro:** Às vezes as letras estão lá, mas a cor está tão clara que não se veem. Dá um jeitinho na roda.
2. **Só vês quadrados pretos? Verifica o Pino 5 (RW):** Volta a confirmar se ele está mesmo cravado na linha do negativo (GND) da breadboard.
3. **O teste da solda:** A placa do teu LCD veio separada daquela barrinha de pinos pretos? Se só encaixaste e não soldaste com estanho (ferro de soldar), a eletricidade não passa. Tens mesmo de os soldar.

Espero que este repositório te ajude! Sente-te livre para usar o código e modificar à vontade.
