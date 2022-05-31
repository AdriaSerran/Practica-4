# Practica-4 - SISTEMAS OPERATIVOS EN TIEMPO REAL

Aquesta pràctica ens servirà per entendre el funcionament d'un sistema operatiu en temps real. El que farem serà encendre i apagar un led constanment amb l'ajuda del codi que se'ns ha proporcionat

# Material

- Led
- ESP32
- Oscil·loscopi

# Funcionament

```c++
#include <Arduino.h>

void ledON (void * pvParameters);
void ledOFF(void * pvParameters);
int LED =2; 

SemaphoreHandle_t semaforo;
```

Al principi del codi definim els voids ON I OFF, el led i inicialitzem el que ve a ser el semàfor. Tot seguit comença el setup, aquí definirem que el nostre pin correspon a un led i que
és de sortida.

També definirem 2 xTaskCreate, els quals serviran per a escriure tambñe per la pantalla de l'ordinador l'estat del nostre led (ON/OFF)

```c++
void setup(){

Serial.begin(9600);
pinMode(LED, OUTPUT);
semaforo= xSemaphoreCreateMutex();

xTaskCreate(
    ledON,
    "LED ON",
    10000,
    NULL,
    1,
    NULL);

xTaskCreate(
    ledOFF,
    "LED OFF",
    10000,
    NULL,
    1,
    NULL);

}
```

Per últim veiem el loop i implementem els dos voids, tant el ledON com el ledOFF, aquests seran els encarregats d'encendre i apagar el led. El ledON té un delay de 3000ms
tal i com es pot observar i tambñe veiem que el led estarà en HIGH, en canvi, el led estarà en LOW quan aquest estigui apagat. el ledOFF té un delay de 1500ms 
tal i com es pot observar.

```c++
void loop(){}

void ledON (void * pvParameters){
    for(;;){
        xSemaphoreTake(semaforo, portMAX_DELAY);
        digitalWrite(LED, HIGH);
        delay(3000);
        xSemaphoreGive(semaforo);
    }
}

void ledOFF (void * pvParameters){
    for(;;){
        xSemaphoreTake(semafor, portMAX_DELAY);
        digitalWrite(LED, LOW);
        delay(1500);
        xSemaphoreGive(semaforo);
    }
}
```

COm es pot veure en ambdos vídeos, vam fer servir l'oscil·loscopi ja que estàvem tenint problemes amb els leds i conectant aquest dispositiu tambés es pot veure el canvi de HIGH a LOW.

https://user-images.githubusercontent.com/101885469/171153893-4500ea80-c06d-497c-af3d-5dc6062d43d3.mp4




En aquest vídeo simplement vam canviar el delay i en vam establir un d'inferior.

https://user-images.githubusercontent.com/101885469/171154198-499d903d-2af5-476c-8e30-b89da470bdc3.mp4

