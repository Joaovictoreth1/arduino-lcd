
#include <SPI.h>
#include <Adafruit_PN532.h>
#include <LiquidCrystal.h>

// --- CONFIGURAÇÃO DO MÓDULO NFC (Modo SPI) ---
// Pinos exatos onde ligou a placa vermelha
#define PN532_SCK  (13)
#define PN532_MISO (12)
#define PN532_MOSI (11)
#define PN532_SS   (10)

Adafruit_PN532 nfc(PN532_SCK, PN532_MISO, PN532_MOSI, PN532_SS);

// --- CONFIGURAÇÃO DO DISPLAY LCD ---
// Pinos Analógicos do Arduino ligados ao LCD (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(A0, A1, A2, A3, A4, A5);

void setup(void) {
  // Atraso para dar tempo à tela LCD e ao leitor NFC estabilizarem a energia
  delay(2000); 

  // Inicializa o ecrã LCD (16 colunas e 2 linhas)
  lcd.begin(16, 2);
  lcd.clear();
  
  // Mensagens de inicialização
  lcd.setCursor(0, 0); 
  lcd.print("Iniciando...");
  delay(1500);

  lcd.setCursor(0, 1); 
  lcd.print("Display Conectado");
  delay(2000);

  // Inicia o leitor NFC
  nfc.begin();
  uint32_t versiondata = nfc.getFirmwareVersion();
  
  // Tratamento de erro caso o NFC não esteja ligado corretamente
  if (!versiondata) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("ERRO CRITICO:");
    lcd.setCursor(0, 1);
    lcd.print("Leitor NFC Falhou");
    while (1); // Para a execução do código aqui
  }
  
  // Configura a placa NFC para ler cartões
  nfc.SAMConfig();
  
  // Interface de aguardar cartão
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Sistema Pronto");
  lcd.setCursor(0, 1);
  lcd.print("Aproxime a TAG");
}

void loop(void) {
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;

  // Busca passiva pela tag num intervalo de 1 segundo
  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength, 1000);

  // Se um cartão for detetado (success > 0)
  if (success) {
    
    // Ação: Mostrar mensagem de sucesso no LCD
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("TAG ENCONTRADA!");
    
    lcd.setCursor(0, 1);
    lcd.print("Acesso Permitido");
    
    // Mantém a mensagem de sucesso na tela por 3 segundos
    delay(3000); 
    
    // Retorna ao menu de espera
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Sistema Pronto");
    lcd.setCursor(0, 1);
    lcd.print("Aproxime a TAG");
  }
}
