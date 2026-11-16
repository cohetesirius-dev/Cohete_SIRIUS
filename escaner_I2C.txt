#include <Wire.h>

void setup() {
  Wire.begin();
  Serial.begin(9600);
  while (!Serial); // Espera a que se abra el Monitor Serie (necesario para placas como Leonardo, Micro, etc.)
  Serial.println("\nI2C Scanner");
}

void loop() {
  byte error, address;
  int nDevices = 0;

  Serial.println("Escaneando...");

  for (address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();

    if (error == 0) {
      Serial.print("Dispositivo I2C encontrado en la direccion 0x");
      if (address < 16) {
        Serial.print("0");
      }
      Serial.print(address, HEX);
      Serial.println(" !");

      nDevices++;
    } else if (error == 4) {
      Serial.print("Error desconocido en la direccion 0x");
      if (address < 16) {
        Serial.print("0");
      }
      Serial.println(address, HEX);
    }
  }

  if (nDevices == 0) {
    Serial.println("No se encontraron dispositivos I2C\n");
  } else {
    Serial.println("Escaneo completado\n");
  }

  delay(5000); // Espera 5 segundos antes de volver a escanear
}
