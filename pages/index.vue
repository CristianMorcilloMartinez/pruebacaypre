<template>
<v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
        <v-card>
            <v-card-title class="headline">Breve prueba técnica - Saludos tributos</v-card-title>
            <hr class="my-3">
            <v-card-text>
                <p>La siguiente prueba nos permitirá valorar la experiencia y la capacidad de los candidatos para desenvolverse con las tecnologías usadas en la empresa.</p>
                <p><b>Requisitos: </b>Usar Visual Studio Code como IDE de programación, no modificar este ejemplo, todo lo que se complete debe ser añadido sin eliminar estas líneas de código. Los resultados se me harán llegar como máximo hasta el lunes 18 de Julio.</p>
                <p>Se trata de una prueba sencilla y asequible con una duración no mayor a una hora. Las tareas <b>mínimas</b> a realizar son las siguientes:</p>
                <ul>
                    <li>Descargar y clonar el repositorio desde la <a href="https://github.com/sitokat/pruebateccaypre" target="new">siguiente dirección de Git.</a></li>
                    <li>Al final del documento encontrareis una tabla con una serie de datos. <b>Sin modificar el código proporcionado en el repositorio y a continuación de la tabla,</b> se deben mostrar los datos como si se trataran de fichas de producto. El resultado debe ser parecido al de la <a href="https://prueba.caypre.es/img/captura1.jpg" target="new">siguiente imagen</a>, podéis ser tan creativos como querais.</li>
                </ul>
                <br>
                <p>Podéis hacerme llegar el resultado de la prueba de la manera que considereis oportuno, teniendo en cuenta que debo ver el código y poner ejecutarlo en mi máquina. Podéis usar el método que considereis oportuno para resolver la prueba. </p>
                <p>Los apartados extra <b>son totalmente opcionales</b> y no son necesarios para completar la prueba, pero se valorará positivamente el ser capaz de completar <b>cualquiera</b> de los apartados, mientras mas apartados se completen, mejor valorado será el candidato. De igual forma se valorará la <b>creatividad</b> mostrada al resolver el problema.</p>
                <ul>
                    <li><b>Extra:</b> Subir el resultado a cualquier servidor web y proporcionar el enlace donde poder visualizarlo.</li>
                    <li><b>Extra:</b> Añadir campos extra a las tarjetas de producto, como por ejemplo una valoración con estrellas. Podéis añadir tantos como consideréis oportunos.</li>
                    <li><b>Extra:</b> Añadir paginación a los "productos" mostrados.</li>
                    <li><b>Extra:</b> La paginación debe retornar un parametro en la URL y debe ser visitable de forma independiente. Ejemplo: (https://megahogar.es/informatica/monitor-pc/?page=3)</li>
                    <li><b>Extra:</b> Obtener los datos a mostrar desde una base de datos. Para ello se deberá crear la base de datos con los campos correspondientes, la cadena de conexión y el backend, en este caso para que pueda ser ejecutada en mi equipo será necesario hacerme llegar un dump de la BD para poder replicarla.</li>
                    <li><b>Extra:</b> Subir el resultado a un nuevo repositorio público con vuestro usuario de Git, manteniendo el código original como rama principal y vuestro trabajo como una nueva rama.</li>
                    <li><b>Extra:</b> Reducir al máximo las líneas de código usado.</li>
                    <li><b>Extra:</b> Trabajar la vista móvil.</li>
                    <li><b>Extra:</b> Añadir un botón de "Comprar" y que el producto se añada al carrito y pueda ser visualizado desde la página de carrito.</li>
                    <li><b>Super Extra Overkill:</b> Implementar un middleware con gestión de roles de usuario y que únicamente el rol administrador sea capaz de entrar a la interfaz de administración de la página. Además, desde el admin se podrán añadir nuevos productos que se mostraran en la HOME. Mantener la sesión del usuario mediante una cookie y destruirla al cerrar sesión. <i>Para resolver esta parte será necesario realizar varios de los puntos anteriores y demostrará un importante dominio del framework.</i></li>
                </ul>
                <div class="text-xs-right">
                    <em><small>&mdash; Andrés H.</small></em>
                </div>
                <hr class="my-3">
                <p>Os dejo a continuación un par de enlaces de interés.</p>
                <a href="https://es.vuejs.org/" target="_blank">Documentación sobre Vue</a>
                <br>
                <a href="https://nuxtjs.org/" target="_blank">Documentación sobre NUXT</a>
                <br>
                <a href="https://vuetifyjs.com" target="_blank">Documentación sobre Vuetify</a>
            </v-card-text>
        </v-card>
        <br>
        <v-card>
            <v-card-title class="headline">Datos a transformar en fichas de producto</v-card-title>
            <v-data-table :headers="headers" :items="articulos" :items-per-page="5" class="elevation-1"></v-data-table>
        </v-card>
        <br>

    </v-flex>

    <!-- CÓDIGO -->

    <paginate name="articulos" :list="articulos" :per="3" tag="v-container">
        <v-layout row>
            <v-flex v-for="item in paginated('articulos')" md4 sm6>
                <v-card class="mx-5 my-5 py-5 text-center">
                    <img width="200px" v-bind:src="item.imagen_url">
                    <h5 class="title my-5">{{item.nombre}}</h5>
                    <div class="price my-5 ">
                        <h1 class="font-weight-bold" style="color:blue">{{item.precio}}€</h1>
                    </div>
                    <div class="font-weight-bold" style="color:green">Disponible</div>
                    <v-btn @click="anyadirCarrito(item)" class="my-5" color="#E65100">
                        Añadir al carrito
                    </v-btn>
                </v-card>
            </v-flex>
        </v-layout>
    </paginate>

    <paginate-links class="d-flex" for="articulos" :simple="{
    prev: 'Anterior',
    next: 'Siguiente'
  }"></paginate-links>

    <v-btn @click="borrarCarrito()" class="my-5" color="#E65100">
        Borrar carrito
    </v-btn>

    <v-container>
        <h2>Carrito de la compra</h2>
        <v-layout row>

            <v-flex v-for="item in carrito" md4 sm6>
                <v-card class="mx-5 my-5 py-5 text-center">
                    <img width="200px" v-bind:src="item.imagen_url">
                    <h5 class="title my-5">{{item.nombre}}</h5>
                    <div class="price my-5 ">
                        <h1 class="font-weight-bold" style="color:blue">{{item.precio}}€</h1>
                    </div>
                    <div class="font-weight-bold" style="color:orange">En el carrito</div>
                </v-card>
            </v-flex>

        </v-layout>

    </v-container>

    <style>
        .paginate-links li {
            margin-right: 30px;
            list-style: none;
            border: 1px solid black;
            border-radius: 50px;
            padding: 10px;
            background-color:#90CAF9;
            transition:0.4s;
        }
        .paginate-links li:hover{
            background-color:#81C784;
            transition:0.4s;
        }
    </style>

    <!-- FIN CÓDIGO -->

</v-layout>
</template>

<script>
import Vue from 'vue'
var VuePaginate = require('vue-paginate')
Vue.use(VuePaginate)

export default {

    data() {
        return {
            headers: [{
                    text: 'Nombre',
                    align: 'start',
                    sortable: false,
                    value: 'nombre'
                },
                {
                    text: 'Descripción Corta',
                    value: 'desc_corta'
                },
                {
                    text: 'Precio',
                    value: 'precio'
                },
                {
                    text: 'URL de la Imagen',
                    value: 'imagen_url'
                }
            ],
            paginate: ['articulos'],
            articulos: [{
                    nombre: 'Monitor Xiaomi MI 23,8" Full HD Desktop 1C LED IPS1',
                    desc_corta: 'Con su ángulo de visión de 178º, puedes lograr un rango de visión superior al de otros modelos, puedes tener las imágenes más espectaculares y maravillosas frente a tus ojos.',
                    precio: 150,
                    imagen_url: 'https://dist.megahogar.es/fotos/BHR4510GL_1.jpg'
                },
                {
                    nombre: 'Monitor LED 23.6" AOC 24B1H Negro',
                    desc_corta: 'Aspecto elegante y amplios ángulos de visión. Equipado con un panel VA, el 24B1H ofrece una asombrosa relación de contraste estático de 3000: 1 con negros oscuros profundos. ',
                    precio: 159,
                    imagen_url: 'https://dist.megahogar.es/fotos/24B1H_1.jpg'
                },
                {
                    nombre: 'Monitor Xiaomi MI 23,8" Full HD Desktop 1C LED IPS',
                    desc_corta: 'Con su ángulo de visión de 178º, puedes lograr un rango de visión superior al de otros modelos, puedes tener las imágenes más espectaculares y maravillosas frente a tus ojos.',
                    precio: 150,
                    imagen_url: 'https://dist.megahogar.es/fotos/BHR4510GL_1.jpg'
                },
                {
                    nombre: 'Monitor LED 23.6" AOC 24B1H Negro',
                    desc_corta: 'Aspecto elegante y amplios ángulos de visión. Equipado con un panel VA, el 24B1H ofrece una asombrosa relación de contraste estático de 3000: 1 con negros oscuros profundos. ',
                    precio: 159,
                    imagen_url: 'https://dist.megahogar.es/fotos/24B1H_1.jpg'
                },
                {
                    nombre: 'Monitor Xiaomi MI 23,8" Full HD Desktop 1C LED IPS',
                    desc_corta: 'Con su ángulo de visión de 178º, puedes lograr un rango de visión superior al de otros modelos, puedes tener las imágenes más espectaculares y maravillosas frente a tus ojos.',
                    precio: 150,
                    imagen_url: 'https://dist.megahogar.es/fotos/BHR4510GL_1.jpg'
                },
                {
                    nombre: 'Monitor LED 23.6" AOC 24B1H Negro',
                    desc_corta: 'Aspecto elegante y amplios ángulos de visión. Equipado con un panel VA, el 24B1H ofrece una asombrosa relación de contraste estático de 3000: 1 con negros oscuros profundos. ',
                    precio: 159,
                    imagen_url: 'https://dist.megahogar.es/fotos/24B1H_1.jpg'
                },
                {
                    nombre: 'Monitor Xiaomi MI 23,8" Full HD Desktop 1C LED IPS',
                    desc_corta: 'Con su ángulo de visión de 178º, puedes lograr un rango de visión superior al de otros modelos, puedes tener las imágenes más espectaculares y maravillosas frente a tus ojos.',
                    precio: 150,
                    imagen_url: 'https://dist.megahogar.es/fotos/BHR4510GL_1.jpg'
                },
                {
                    nombre: 'Monitor LED 23.6" AOC 24B1H Negro',
                    desc_corta: 'Aspecto elegante y amplios ángulos de visión. Equipado con un panel VA, el 24B1H ofrece una asombrosa relación de contraste estático de 3000: 1 con negros oscuros profundos. ',
                    precio: 159,
                    imagen_url: 'https://dist.megahogar.es/fotos/24B1H_1.jpg'
                },
                {
                    nombre: 'Monitor Xiaomi MI 23,8" Full HD Desktop 1C LED IPS',
                    desc_corta: 'Con su ángulo de visión de 178º, puedes lograr un rango de visión superior al de otros modelos, puedes tener las imágenes más espectaculares y maravillosas frente a tus ojos.',
                    precio: 150,
                    imagen_url: 'https://dist.megahogar.es/fotos/BHR4510GL_1.jpg'
                },
                {
                    nombre: 'Monitor LED 23.6" AOC 24B1H Negro',
                    desc_corta: 'Aspecto elegante y amplios ángulos de visión. Equipado con un panel VA, el 24B1H ofrece una asombrosa relación de contraste estático de 3000: 1 con negros oscuros profundos. ',
                    precio: 159,
                    imagen_url: 'https://dist.megahogar.es/fotos/24B1H_1.jpg'
                }
            ],
            carrito: [],
            productos2: [],
        }
    },
    mounted() {
        if (localStorage.carrito) {
            this.carrito = JSON.parse(localStorage.getItem("carrito"));
        }
    },
    methods: {
        anyadirCarrito(item) {
            this.carrito.push(item);
            localStorage.setItem("carrito", JSON.stringify(this.carrito));
        },
        borrarCarrito() {
            localStorage.removeItem("carrito");
            location.reload();
        },

    }

}
</script>
