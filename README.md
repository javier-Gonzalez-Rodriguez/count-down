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

- Establece como va a funcionar el reloj puede ser de dos tipos:
 1. ***down***: Establece el reloj para realizar la cuenta atrás (es el valor por defecto).
 2. ***up***: Establece el reloj con un contador progresivo.

---

### identifier(opcional)

- Identificador que es mandado por los eventos 'on-finish'(cuando clock-type == 'down') o 'stop-time'(cuando clock-type == 'up').

---
### stop-time

- JSON que contiene la especificación del momento en el que se debe detener el reloj cuando clock-type == 'up', en la estructura siguiente 
  se muestra como es la estructura del JSON donde el reloj se parará cuando el reloj llege a 1 minuto.

```
    stop-time : {
                    days: 0,
                    hours: 0,
                    minutes: 1,
                    seconds: 0
                }
```

---
### background-color(opcional)

- Establece el color de fondo de las cajas de los elementos del reloj.

---
### text-color(opcional)

- Establece el color del texto de los valores del reloj.

---
### custom-header-text(opcional)

- JSON que establece el texto del header del reloj, su estructura es la siguiente.

```
    customHeaderText: {
                    dias: 'D',
                    horas: 'H',
                    minutos: 'M',
                    segundos: 'S',
                }
            
```

## Eventos

### @on-finish

- Evento lanzado cuando un reloj de cuenta regresiva finaliza, el evento manda el parametro ***identifier***

### @time-stoped

- Evento lanzado cuando un reloj de cuenta progresiva finaliza, el evento manda el parametro ***identifier***

## Configuración

### Cuenta regresiva

1. Para configurar el componente como cuenta regresiva el único parametro obligatorio es el parametro ***time***.
2. A continuación se muestra como quedaría el componete con una configuración básica estableciendo el reloj en cuenta regresiva con 4 minutos de tiempo.

```
<count-down :time="240"></count-down>
```
3. Si se desea capturar el momento en el que el reloj llega a 0 se debe usar el evento ***@on-finish***

```
<count-down :time="240" :clock-type="'down'" @on-finish="eventoFinalizacion"></count-down>
```
4. Si existen varios compentes en la misma página y se necesita saber cual de los componentes ha finalizado se puede usar un identificador que se mandará como parametro en la funcion ***@on-finish***

```
<count-down :time="240" :clock-type="'down'" @on-finish="eventoFinalizacion" :identifier="'countdown customizado'"></count-down>
```
---

### Cuenta progresiva

1. Para configurar el componente como cuenta progresiva el único parametro obligatorio es el parametro ***clock-type***.
2. A continuación se muestra como quedaría el componete con una configuración básica.

```
<count-down :clock-type="'up'"></count-down>
```

3. Si se dedea que el tiempo se detenga en un momento dado es necesario usar ***stop-time***

```
<count-down :stop-time="timeToStopVar" clock-type="'up'"></count-down>
```

4. Si existen varios compentes en la misma página y se necesita saber cual de los componentes ha finalizado se puede usar un identificador que se mandará como parametro en la funcion ***@time-stoped***

```
<count-down :stop-time="timeToStopVar" :clock-type="'up'" @time-stoped="eventoFinalizacion" :identifier="'countdown customizado'"></count-down>
```
---

## Cómo funciona

```
    Reloj 1
    <count-down :time="240" clockType="down" @on-finish="eventoFinalizacion" :identifier="'countdown customizado'"></count-down>

    Reloj 2
    <count-down :text-color="color1" :background-color="color2"  :clock-type="'up'" @time-stoped="eventoFinalizacion" :identifier="'countdown customizado'"></count-down>
```

![Interfaz principal](./images/reloj.gif)  

