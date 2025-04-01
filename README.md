# PRACTICA-6
# **Práctica 6: Uso del Bus SPI**

## **1. Contexto y Objetivos**
En esta práctica se analizará el funcionamiento del bus de comunicación SPI (Serial Peripheral Interface), continuando con el estudio de protocolos digitales. A diferencia de la práctica anterior, donde se trabajó con I2C, esta vez se abordará la gestión de archivos en una tarjeta SD y la lectura de identificadores RFID mediante SPI.

Los objetivos de esta actividad son:
- Leer y escribir datos en una tarjeta SD utilizando SPI.
- Configurar un lector RFID para extraer el UID de una tarjeta.

## **2. Desarrollo de la Práctica**

### **Ejercicio 1: Manipulación de Archivos en una Tarjeta SD**

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

