# 👋 Olá! Bem-vindo ao meu Sistema de Controlo de Acessos com Arduino

Este é um projeto que desenvolvi para criar um leitor de cartões RFID/NFC "stand-alone" — ou seja, que funciona sozinho, dando o feedback diretamente numa tela LCD, sem precisar de estar sempre ligado à janela do Monitor Serial no ecrã do computador.

Foi um desafio muito interessante (especialmente domar os fios do ecrã LCD!), mas o resultado final é um sistema super estável. Utilizei um Arduino UNO, o famoso leitor NFC PN532 e o clássico ecrã LCD 16x2.

## 💡 O "Pulo do Gato" deste Projeto

Quando tentamos ligar o leitor NFC (que usa comunicação muito rápida) e o ecrã LCD (que precisa de muitos fios) nos pinos normais do Arduino, a confusão de cabos é enorme e os conflitos são comuns.

**A solução que adotei:** Mudei todos os 6 fios de dados do ecrã LCD lá para o outro lado da placa, para as portas **Analógicas (do A0 ao A5)**. Assim, os pinos digitais ficaram livres para o leitor NFC respirar. Funcionou na perfeição!

---

## 🔌 Como eu liguei tudo (Esquema Físico)

Se quiseres recriar este projeto, esta é a "receita do bolo" exata que usei para que não falhasse:

### 1. O Leitor PN532 (Configurado para SPI)
*Nota importante:* No meio da placa vermelha do leitor há um quadradinho com botõezinhos brancos. Para o código funcionar, tive de colocar a **Chave 1 para cima (OFF)** e a **Chave 2 para baixo (ON)**.

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
