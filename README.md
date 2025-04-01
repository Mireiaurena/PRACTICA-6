# PRACTICA-6
# **Práctica 6: Uso del Bus SPI**
# **Práctica 6.1: Manipulación de Archivos en una Tarjeta SD**
## **1. Contexto y Objetivos**
En esta práctica se analizará el funcionamiento del bus de comunicación SPI (Serial Peripheral Interface), continuando con el estudio de protocolos digitales. A diferencia de la práctica anterior, donde se trabajó con I2C, esta vez se abordará la gestión de archivos en una tarjeta SD y la lectura de identificadores RFID mediante SPI.

Los objetivos de esta actividad son:
- Leer y escribir datos en una tarjeta SD utilizando SPI.
- Configurar un lector RFID para extraer el UID de una tarjeta.

## **2. Desarrollo de la Práctica**

El siguiente código permite acceder a un archivo de texto almacenado en una tarjeta SD y mostrar su contenido en la consola:

```c++
 #include <SPI.h> 
#include <SD.h> 
File myFile; 
void setup() 
{ 
  Serial.begin(9600); 
  Serial.print("Iniciando SD ..."); 
  if (!SD.begin(4)) { 
    Serial.println("No se pudo inicializar"); 
    return; 
  }
Serial.println("inicializacion exitosa"); 
  myFile = SD.open("archivo.txt");//abrimos  el archivo  
  if (myFile) { 
    Serial.println("archivo.txt:"); 
    while (myFile.available()) { 
Serial.write(myFile.read()); 
    } 
    myFile.close(); //cerramos el archivo 
  } else { 
    Serial.println("Error al abrir el archivo"); 
  } 
} 
void loop() 
{ 
}
```

### **Descripción del Código**
El programa establece comunicación con la tarjeta SD y busca un archivo de texto denominado `archivo.txt`. Si la SD se encuentra operativa, el archivo se abre, su contenido se lee y se muestra en la consola serie. Para evitar problemas, el archivo se cierra al finalizar la lectura.

### **Ejemplo de Salida Esperada:**
```
Iniciando SD... inicializacion existosa
archivo.txt:
Ejemplo de Salida Esperada
```
# **Práctica 6.2: Lectura de Etiquetas RFID**

## **1. Objetivos**
En esta práctica se explorará el funcionamiento del bus de comunicación SPI, con un enfoque en la lectura de etiquetas RFID mediante NFC.

Los objetivos de esta práctica son:
- Comprender el uso del protocolo SPI en la comunicación con dispositivos periféricos.
- Leer datos desde un lector RFID utilizando NFC.

## **2. Materiales Necesarios**
- ESP32-S3
- Módulo lector RFID
- Dispositivo con capacidad NFC

## **3. Desarrollo de la Práctica**

### **Código Utilizado**
```c++
#include <Arduino.h>
#include <SPI.h>
#include <MFRC522.h>
#define RST_PIN 9 //Pin 9 para el reset del RC522
#define SS_PIN 10 //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522
void setup() {
    Serial.begin(115200); //Iniciamos la comunicación serial
    SPI.begin(36, 37, 35); //Iniciamos el Bus SPI
    mfrc522.PCD_Init(); // Iniciamos el MFRC522
    Serial.println("Lectura del UID");
}
void loop() {
    // Revisamos si hay nuevas tarjetas presentes
    if ( mfrc522.PICC_IsNewCardPresent()) 
    { 
        //Seleccionamos una tarjeta
        if ( mfrc522.PICC_ReadCardSerial()) 
        {
        // Enviamos serialemente su UID
        Serial.print("Card UID:");
        for (byte i = 0; i < mfrc522.uid.size; i++) {
            Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0"
            : " ");
            Serial.print(mfrc522.uid.uidByte[i], HEX); 
        } 
        Serial.println();
        // Terminamos la lectura de la tarjeta actual
        mfrc522.PICC_HaltA(); 
        } 
    } 
}
```

### **Descripción del Código**
Este programa implementa la lectura de etiquetas RFID mediante un módulo MFRC522. Su propósito es detectar tarjetas RFID cercanas y extraer su UID.

El código utiliza la librería `MFRC522` para la comunicación con el lector RFID a través de SPI. Se definen los pines necesarios para la conexión con el módulo, y en la función `setup()` se inicializan tanto la comunicación serie como el bus SPI.

En el `loop()`, el programa revisa si hay una nueva tarjeta en el rango del lector. Si se detecta, se obtiene su identificador único (UID) y se muestra en el monitor serie. Luego, la tarjeta se pone en estado de espera para permitir nuevas lecturas.

## **4. Salida Esperada en el Monitor Serie**
```
Lectura del UID
Card UID: 08 E4 3E AD
Card UID: 08 DB 76 CB
```

## **5. Conclusión**
Esta práctica permite la detección y lectura de etiquetas RFID utilizando un módulo MFRC522 junto con un ESP32-S3. Este sistema proporciona una forma sencilla y eficiente de interactuar con dispositivos RFID en diversos proyectos electrónicos.



