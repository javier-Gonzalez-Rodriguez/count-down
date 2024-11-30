<template>
    <div id="custom-count-down">
        <div class="countdown-header">
            <span>{{customHeaderText.dias}}</span>
            <span>{{customHeaderText.horas}}</span>
            <span>{{customHeaderText.minutos}}</span>
            <span>{{customHeaderText.segundos}}</span>
        </div>
        <div class="countdown-container">
            <div class="contenedor" :style="customStyles">
                <div class="up days">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.days.actualNumber }}</div>
                </div>
                <div class="down" :style="customStyles">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.days.actualNumber }}</div>
                </div>
                <div class="fixedNmber" :style="{'color': customStyles.color}">
                    {{ clock.days.nextNumber }}
                </div>
            </div>
            :
            <div class="contenedor" :style="customStyles">
                <div class="up hours">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.hours.actualNumber }}</div>
                </div>
                <div class="down">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.hours.actualNumber }}</div>
                </div>
                <div class="fixedNmber" :style="{'color': customStyles.color}">
                    {{ clock.hours.nextNumber }}
                </div>
            </div>
            :
            <div class="contenedor" :style="customStyles">
                <div class="up minutes">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.minutes.actualNumber }}</div>
                </div>
                <div class="down">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.minutes.actualNumber }}</div>
                </div>
                <div class="fixedNmber" :style="{'color': customStyles.color}">
                    {{ clock.minutes.nextNumber }}
                </div>
            </div>
            :
            <div class="contenedor" :style="customStyles">
                <div class="up seconds">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.seconds.actualNumber }}</div>
                </div>
                <div class="down">
                    <div class="shadow"></div>
                    <div class="inn" :style="customStyles">{{ clock.seconds.actualNumber }}</div>
                </div>
                <div class="fixedNmber" :style="{'color': customStyles.color}">
                    {{ clock.seconds.nextNumber }}
                </div>
            </div>            
        </div>
    </div>
</template>

<script>
    export default {
        name: 'custom-count-down',
        props: {
            time: {
                type: Number,
                required: true,
            },
            clockType: {
                type: String,
                default: 'down',
            },
            identifier: {
                type: String,
                default: 'countdown',
            },
            stopTime: {
                type: JSON,
                default: {
                    days: -1,
                    hours: 0,
                    minutes: 0,
                    seconds: 0
                }
            },
            backgroundColor: {
                type: String,
                default: '#333',
            },
            textColor: {
                type: String,
                default: '#ccc',
            },
            customHeaderText: {
                type: JSON,
                default: {
                    dias: 'D',
                    horas: 'H',
                    minutos: 'M',
                    segundos: 'S',
                }
            }
        },
        data(){
            return {
                fragmentTime: {
                    days: 0,
                    hours: 0,
                    minutes: 0,
                    seconds: 0
                },
                clock: {
                    days: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 0,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                    hours: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 0,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                    minutes: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 0,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                    seconds: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 60,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                },
                intervalo: null,
                customStyles: {
                    'background-color': '#333',
                    color: '#ccc'
                },
            };
        },
        created(){

            this.customStyles['background-color'] = this.backgroundColor;
            this.customStyles.color = this.textColor;

            if(this.clockType == 'down'){
                this.clockTypeDown();
            }else{
                this.clockTypeUp();
            }
        },
        mounted() {
            this.clock.seconds.element = this.$el.querySelector('.seconds');
            this.clock.minutes.element = this.$el.querySelector('.minutes');
            this.clock.hours.element = this.$el.querySelector('.hours');
            this.clock.days.element = this.$el.querySelector('.days');

            if(this.clockType == 'down'){
                this.iniciarCuentaAtrasSegundos();
            }else{
                this.iniciarCuentaSegundos();
            }
        },
        methods: {
            /**
             * prepara los datos para la cuenta regresiva
             */
            clockTypeDown(){
                //SE CALCULAN  LOS DIAS, HORAS , MINUTOS Y SEGUNDOS DE FORMA INDIVIDUAL
                let remainder = this.time;

                this.fragmentTime.days = Math.floor(remainder / 86400); // Total de días
                remainder = remainder % 86400;
                
                this.fragmentTime.hours = Math.floor(remainder / 3600); // Total de horas restantes
                remainder = remainder % 3600;

                this.fragmentTime.minutes = Math.floor(remainder / 60); // Total de minutos restantes
                
                this.fragmentTime.seconds = remainder % 60; // Segundos restantes

                //SE CALCULA EL SIGUIENTE NUMERO Y SE ESTABLECE EL NUMERO ACTUAL DE CADA CONTADOR
                var aux = 0;

                this.clock.days.actualNumber = this.fragmentTime.days;
                aux = this.fragmentTime.days - 1;
                this.clock.days.nextNumber = aux < 0 ? 364 : aux;
                this.clock.days.currentAngle = 0;

                this.clock.hours.actualNumber = this.fragmentTime.hours;
                aux = this.fragmentTime.hours - 1;
                this.clock.hours.nextNumber = aux < 0 ? 24 : aux;
                this.clock.hours.currentAngle = 0;

                this.clock.minutes.actualNumber = this.fragmentTime.minutes;
                aux = this.fragmentTime.minutes - 1;
                this.clock.minutes.nextNumber = aux < 0 ? 59 : aux;
                this.clock.minutes.currentAngle = 0;

                this.clock.seconds.actualNumber = this.fragmentTime.seconds;
                aux = this.fragmentTime.seconds - 1;
                this.clock.seconds.nextNumber = aux < 0 ? 59 : aux;
                this.clock.seconds.currentAngle = 0;

                //SE CALCULA CUALES CONTADORES DEBEN DESACTIVARSE POR NO PODER CONTINUAR CON LA CUENTA ATRÁS
                if(this.fragmentTime.seconds == 0 && this.fragmentTime.minutes == 0 && this.fragmentTime.hours == 0 && this.fragmentTime.days == 0 ){
                    this.clock.seconds.isActive = false;
                }

                if(this.fragmentTime.minutes == 0 && this.fragmentTime.hours == 0 && this.fragmentTime.days == 0 ){
                    this.clock.minutes.isActive = false;
                }

                if(this.fragmentTime.hours == 0 && this.fragmentTime.days == 0){
                    this.clock.hours.isActive = false;
                }

                if(this.fragmentTime.days == 0){
                    this.clock.days.isActive = false;
                }
            },
            /**
             * inicia la animacion y cuenta regresiva para los dias cuando es requerido
             */
            daysDecrease(){
                if(this.clock.days.isActive){
                    const intervalo = setInterval(() => {
                        this.clock.days.currentAngle += 1;

                        // Aplica la transformación de rotación al elemento
                        this.clock.days.element.style.transform = `rotateX(${-this.clock.days.currentAngle}deg)`;

                        if(this.clock.days.currentAngle == 80){
                            this.clock.days.actualNumber = this.clock.days.nextNumber;
                        }else if(this.clock.days.currentAngle == 90){
                            if(!this.clock.days.hasReached50){
                                this.clock.days.hasReached50 = true;
                                this.clock.days.nextNumber--;
                                if(this.clock.days.nextNumber < 0){
                                    this.clock.days.nextNumber = 364;
                                }
                            }
                        } 

                        if (this.clock.days.currentAngle > 90) {
                            this.clock.days.currentAngle = 0;
                            this.clock.days.element.style.transform = `rotateX(${-this.clock.days.currentAngle}deg)`;
                            this.clock.days.hasReached50 = false;
                            //
                            clearInterval(intervalo);
                        }
                    }, 11.11);
                }
            },
            /**
             * inicia la animacion y cuenta regresiva para las horas cuando es requerido
             */
            hoursDecrease(){
                if(this.clock.hours.isActive){
                    const intervalo = setInterval(() => {
                        this.clock.hours.currentAngle += 1;
                        // Aplica la transformación de rotación al elemento
                        this.clock.hours.element.style.transform = `rotateX(${-this.clock.hours.currentAngle}deg)`;

                        if (this.clock.hours.currentAngle == 1) {
                            if(this.clock.hours.nextNumber > this.clock.hours.actualNumber){
                                this.daysDecrease();
                            }
                        }else if(this.clock.hours.currentAngle == 80){
                            this.clock.hours.actualNumber = this.clock.hours.nextNumber;
                        }else if(this.clock.hours.currentAngle == 90){
                            if(!this.clock.hours.hasReached50){
                                this.clock.hours.hasReached50 = true;
                                this.clock.hours.nextNumber--;
                                if(this.clock.hours.nextNumber < 0){
                                    this.clock.hours.nextNumber = 23;
                                }
                            }
                        } 

                        if (this.clock.hours.currentAngle > 90) {
                            this.clock.hours.currentAngle = 0;
                            this.clock.hours.element.style.transform = `rotateX(${-this.clock.hours.currentAngle}deg)`;
                            this.clock.hours.hasReached50 = false;
                            //
                            clearInterval(intervalo);
                        }
                    }, 11.11);
                }
            },
            /**
             * inicia la animacion y cuenta regresiva para los minutos cuando es requerido
             */
            minutesDecrease() {
                //si la cuenta regresiva no puede continuar se desactiva la cuenta regresiva
                if(this.clock.minutes.isActive){
                    const intervalo = setInterval(() => {
                        this.clock.minutes.currentAngle += 1;

                        // Aplica la transformación de rotación al elemento
                        this.clock.minutes.element.style.transform = `rotateX(${-this.clock.minutes.currentAngle}deg)`;
                        
                        // Detecta si se ha llegado al 50% (es decir, -90 grados)
                        if (this.clock.minutes.currentAngle == 1) {
                            if(this.clock.minutes.nextNumber > this.clock.minutes.actualNumber){
                                this.hoursDecrease();
                            }
                        }else if(this.clock.minutes.currentAngle == 80){
                            this.clock.minutes.actualNumber = this.clock.minutes.nextNumber;
                        }else if(this.clock.minutes.currentAngle == 90){
                            if(!this.clock.minutes.hasReached50){
                                this.clock.minutes.hasReached50 = true;
                                this.clock.minutes.nextNumber--;
                                if(this.clock.minutes.nextNumber < 0){
                                    this.clock.minutes.nextNumber = 59;
                                }
                            }
                            
                        }

                        if (this.clock.minutes.currentAngle > 90) {
                            this.clock.minutes.currentAngle = 0;
                            this.clock.minutes.element.style.transform = `rotateX(${-this.clock.minutes.currentAngle}deg)`;
                            this.clock.minutes.hasReached50 = false;
                            //
                            clearInterval(intervalo);
                        }
                        
                    }, 11,11);
                }
            },
            /**
             * Es el metodo encargado de iniciar la cuenta atras de los segundos ejecutando en cascada la cuenta regresiva de los minutos, horas y dias 
             * si es necesario
             */
            iniciarCuentaAtrasSegundos(){
                //si la cuenta atras no puede continuar se desactiva la cuenta regresiva
                if(this.clock.seconds.isActive){
                    this.intervalo = setInterval(() => {
                        // Incrementa el ángulo actual
                        this.clock.seconds.currentAngle += 1;

                        // Si el ángulo es mayor a 180, reinícialo para que la animación sea continua
                        if (this.clock.seconds.currentAngle > 90) {
                            this.clock.seconds.currentAngle = 0;
                        }

                        
                        // Aplica la transformación de rotación al elemento
                        this.clock.seconds.element.style.transform = `rotateX(${-this.clock.seconds.currentAngle}deg)`;
                        
                        if(this.clock.seconds.currentAngle == 1){
                            if(this.clock.seconds.nextNumber > this.clock.seconds.actualNumber){
                                this.minutesDecrease();
                            }
                        }else if (this.clock.seconds.currentAngle == 80) {// Detecta si se ha llegado al 50% (es decir, -90 grados)
                            this.clock.seconds.actualNumber = this.clock.seconds.nextNumber;
                        }else if(this.clock.seconds.currentAngle == 90){
                            this.clock.seconds.nextNumber--;
                            if(this.clock.seconds.nextNumber < 0){
                                if(this.clock.minutes.actualNumber == 0 && this.clock.hours.actualNumber == 0 && this.clock.days.actualNumber == 0){
                                    this.clock.isActive = false;
                                    this.clock.seconds.element.style.transform = `rotateX(0deg)`;
                                    clearInterval(this.intervalo);
                                    this.intervalo = null;
                                    this.$emit('onFinish', this.identifier);
                                }
                                this.clock.seconds.nextNumber = 59;
                            }
                            
                        }
                        
                    }, 11,11); //para calcular el tiempo del intervalo 1000/(grados a recorrer en este caso 90) = 11.11...
                }
            },
            /**
             * establece los datos iniciales para la cuenta de tiempo
             */
            clockTypeUp(){
                this.fragmentTime = {
                    days: 0,
                    hours: 0,
                    minutes: 0,
                    seconds: 0
                };

                this.clock = {
                    days: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 1,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                    hours: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 1,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                    minutes: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 1,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                    seconds: {
                        element: null,
                        actualNumber: 0,
                        nextNumber: 1,
                        hasReached50: false,
                        currentAngle: 0,
                        isActive: true,
                    },
                };
            },
            /**
             * activa la animacion de la caja de dias y aumenta el tiempo para los dias
             */
            daysIncrease(){
                const intervalo = setInterval(() => {
                    this.clock.days.currentAngle += 1;

                    // Aplica la transformación de rotación al elemento
                    this.clock.days.element.style.transform = `rotateX(${-this.clock.days.currentAngle}deg)`;

                    if(this.clock.days.currentAngle == 80){
                        this.clock.days.actualNumber = this.clock.days.nextNumber;
                    }else if(this.clock.days.currentAngle == 90){
                        if(!this.clock.days.hasReached50){
                            this.clock.days.hasReached50 = true;
                            this.clock.days.nextNumber++;
                        }
                    } 

                    if (this.clock.days.currentAngle > 90) {
                        this.clock.days.currentAngle = 0;
                        this.clock.days.element.style.transform = `rotateX(${-this.clock.days.currentAngle}deg)`;
                        this.clock.days.hasReached50 = false;
                        //
                        clearInterval(intervalo);
                    }
                }, 11.11);
            },
            /**
             * activa la animacion de la caja de horas y aumenta el tiempo para las horas
             */
            hoursIncrease(){
                const intervalo = setInterval(() => {
                    this.clock.hours.currentAngle += 1;
                    // Aplica la transformación de rotación al elemento
                    this.clock.hours.element.style.transform = `rotateX(${-this.clock.hours.currentAngle}deg)`;

                    if (this.clock.hours.currentAngle == 1) {
                        if(this.clock.hours.actualNumber == 23){
                            this.daysIncrease();
                        }
                    }else if(this.clock.hours.currentAngle == 80){
                        this.clock.hours.actualNumber = this.clock.hours.nextNumber;
                    }else if(this.clock.hours.currentAngle == 90){
                        if(!this.clock.hours.hasReached50){
                            this.clock.hours.hasReached50 = true;
                            this.clock.hours.nextNumber++;
                            if(this.clock.hours.nextNumber == 24){
                                this.clock.hours.nextNumber = 0;
                            }
                        }
                    } 

                    if (this.clock.hours.currentAngle > 90) {
                        this.clock.hours.currentAngle = 0;
                        this.clock.hours.element.style.transform = `rotateX(${-this.clock.hours.currentAngle}deg)`;
                        this.clock.hours.hasReached50 = false;
                        //
                        clearInterval(intervalo);
                    }
                }, 11.11);
                
            },
            /**
             * activa la animacion de la caja de minutos y aumenta el tiempo para los minutos
             */
            minutesIncrease(){
                const intervalo = setInterval(() => {
                    this.clock.minutes.currentAngle += 1;

                    // Aplica la transformación de rotación al elemento
                    this.clock.minutes.element.style.transform = `rotateX(${-this.clock.minutes.currentAngle}deg)`;
                    
                    // Detecta si se ha llegado al 50% (es decir, -90 grados)
                    if (this.clock.minutes.currentAngle == 1) {
                        if(this.clock.minutes.actualNumber == 59){
                            this.hoursIncrease();
                        }
                    }else if(this.clock.minutes.currentAngle == 80){
                        this.clock.minutes.actualNumber = this.clock.minutes.nextNumber;
                    }else if(this.clock.minutes.currentAngle == 90){
                        if(!this.clock.minutes.hasReached50){
                            this.clock.minutes.hasReached50 = true;
                            this.clock.minutes.nextNumber++;
                            if(this.clock.minutes.nextNumber == 60){
                                this.clock.minutes.nextNumber = 0;
                            }
                        }
                        
                    }

                    if (this.clock.minutes.currentAngle > 90) {
                        this.clock.minutes.currentAngle = 0;
                        this.clock.minutes.element.style.transform = `rotateX(${-this.clock.minutes.currentAngle}deg)`;
                        this.clock.minutes.hasReached50 = false;
                        //
                        clearInterval(intervalo);
                    }
                    
                }, 11,11);
            },
            /**
             * activa la cuenta del tiempo aumentando los segundos y ejecutando en cascada las demás funciones de incremento
             */
            iniciarCuentaSegundos(){
                 //si la cuenta atras no puede continuar se desactiva la cuenta regresiva
                this.intervalo = setInterval(() => {
                    // Incrementa el ángulo actual
                    this.clock.seconds.currentAngle += 1;

                    // Si el ángulo es mayor a 180, reinícialo para que la animación sea continua
                    if (this.clock.seconds.currentAngle > 90) {
                        this.clock.seconds.currentAngle = 0;
                    }

                    
                    // Aplica la transformación de rotación al elemento
                    this.clock.seconds.element.style.transform = `rotateX(${-this.clock.seconds.currentAngle}deg)`;
                    
                    if(this.clock.seconds.currentAngle == 1){
                        if(this.clock.seconds.actualNumber == 59){
                            this.minutesIncrease();
                        }
                    }else if (this.clock.seconds.currentAngle == 80) {// Detecta si se ha llegado al 50% (es decir, -90 grados)
                        this.clock.seconds.actualNumber = this.clock.seconds.nextNumber;
                    }else if(this.clock.seconds.currentAngle == 90){
                        this.clock.seconds.nextNumber++;
                        if(this.clock.seconds.nextNumber == 60){
                            this.clock.seconds.nextNumber = 0;
                        }
                    }

                    if(this.stopTime.seconds == this.clock.seconds.actualNumber &&
                        this.stopTime.minutes == this.clock.minutes.actualNumber &&
                        this.stopTime.hours == this.clock.hours.actualNumber &&
                        this.stopTime.days == this.clock.days.actualNumber
                    ){
                        clearInterval(this.intervalo);
                        this.$emit('timeStoped', this.identifier);
                        this.intervalo = null;
                    }
                    
                }, 11,11); //para calcular el tiempo del intervalo 1000/(grados a recorrer en este caso 90) = 11.11...
            },
        },
        watch: {
            //si se actualiza el valor de la variable "time" se resetea el contador al nuevo tiempo
            time(nuevoValor) {
                if(this.clockType == 'down'){
                    if(this.intervalo != null){
                        clearInterval(this.intervalo);
                        this.intervalo = null;
                    }
                    
                    nextTick(() => {
                        this.clockTypeDown();
                        this.iniciarCuentaAtrasSegundos();
                    });

                }
            },
            stopTime(nuevoValor) {
                if(this.clockType == 'up'){
                    if(this.intervalo != null){
                        clearInterval(this.intervalo);
                        this.intervalo = null;
                    }

                    nextTick(() => {
                        this.clockTypeUp();
                        this.iniciarCuentaSegundos();
                    });
                }
            },
            textColor(nuevoValor) {
                this.customStyles.color = nuevoValor;
            },
            backgroundColor(nuevoValor) {
                this.customStyles['background-color'] = nuevoValor;
            }
        }
    }
</script>

<style scoped>
    .countdown-container {
        display: flex;
        flex-direction: row;   
        max-width: 230px;
        font-size: larger;
        font-family: fantasy;
        font-weight: bold;
        align-items: center;
    }
    /* Estilo del contenedor principal */

    .contenedor {
        width: 30px;
        height: 50px;
        position: relative;
        perspective: 800px; /* Añade perspectiva para el efecto 3D */
        margin: 7px auto;
        background-color: #333;
        border-radius: 6px;
        font-family: fantasy;
    }

    /* Estilo para la mitad superior */
    .up {
        transform-origin: 50% 100%; /* El punto de rotación está en el borde inferior */
        top: 0;
        z-index: 2;
        position: absolute;
        left: 0;
        width: 100%;
        height: 50%;
        /*font-size: 80px;*/
        overflow: hidden;
        outline: 1px solid transparent;
        
    }

    .up .shadow {
        position: absolute;
        width: 100%;
        height: 100%;
        z-index: 2;
        /*background: rgba(0, 0, 0, 0.2); /* Sombra ligeramente oscura */
    }

    .up .inn {
        top: 0;
        position: absolute;
        left: 0;
        z-index: 1;
        width: 100%;
        height: 200%;
        color: #ccc;
        text-shadow: 0 1px 2px #000;
        text-align: center;
        background-color: #333;
        border-radius: 6px;
        font-size: 17px;
        display:flex;    
        align-items: center;
        justify-content: center;
        font-family: fantasy;
    }

    /* Estilo para la mitad inferior */
    .down {
        transform-origin: 50% 0;
        bottom: 0;
        z-index: 1;
        position: absolute;
        left: 0;
        width: 100%;
        height: 50%;
        /*font-size: 80px;*/
        overflow: hidden;
        outline: 1px solid transparent;
        border-bottom-left-radius: 6px;
        border-bottom-right-radius: 6px;
    }

    .down .shadow {
        position: absolute;
        width: 100%;
        height: 100%;
        z-index: 2;
        /*background: rgba(0, 0, 0, 0.2); /* Sombra ligeramente oscura */
    }

    .down .inn {
        bottom: 0;
        position: absolute;
        left: 0;
        z-index: 1;
        width: 100%;
        height: 200%;
        color: #ccc;
        text-shadow: 0 1px 2px #000;
        text-align: center;
        background-color: #333;
        border-radius: 6px;
        font-size: 17px;
        display:flex;    
        align-items: center;
        justify-content: center;
        font-family: fantasy;
        
    }

    .fixedNmber{
        height: 100%;
        width: 100%;
        font-size: 17px;
        position: absolute;
        color: #ccc;
        text-shadow: 0 1px 2px #000;
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: fantasy;
    }

    .countdown-header {
        font-size: 10px;
        max-width: 230px;
        flex-direction: row;
        display: flex;
    }

    .countdown-header span {
        width: 100%;
        display: flex;
        font-weight: bold;
        justify-content: center;
    }
</style>