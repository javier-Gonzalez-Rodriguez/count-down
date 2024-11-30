# count-down Component

Este repositorio contiene un componente de reloj (count-down) que permite inertar un reloj de cuenta atás o si asi lo desea realizar una cuenta de tiempo hasta un limite dado en el tiempo.

# Características

- Dado un tiempo en segundos el componente muestra un reloj de cuenta regresiva 
- Dado un tiempo máximo al componente este muestra un reloj de cuenta progresiva que se detendrá al llegar al limite fijado
- Diseño responsive compatible con dispositivos móviles.

## Instalación

1. Clona el repositorio.
2. Añade el componente vue a tu punto de montaje vue para integrar el elemento.

## Parametros

### time

- Tiempo en segundos, establece el tiempo de la cuenta atrás en el reloj.
---

### clock-type

- Establece como va a funcionar el reloj puede ser de dos tipos
 1. ***down***: Establece el reloj para realizar la cuenta atrás (es el valor por defecto).
 2. ***up***: Establece el reloj con un contador progresivo.

---

### identifier(opcional)

- Identificador que es mandado por los eventos 'on-finish'(cuando clock-type == 'down') o 'stop-time'(cuando clock-type == 'up').

---
### stop-time

- JSON que contiene la especificación del momento en el que se debe detener el reloj cuando clock-type == 'up', en la estructura siguiente 
  se muestra como es la estructura del JSON donde el reloj se parará cuando el reloj llege a 1 minuto.

´´´
    stop-time : {
                    days: 0,
                    hours: 0,
                    minutes: 1,
                    seconds: 0
                }
´´´

---
### background-color

- Establece el color de fondo de las cajas de los elementos del reloj.

---
### text-color

- Establece el color del texto de los valores del reloj.

---