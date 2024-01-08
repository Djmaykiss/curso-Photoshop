<template>
    <div class="container">
        <section id="intro">
            <!-- Sección de introducción con un video o imagen -->
            <div>
                <h2>Bienvenido al Curso de Photoshop Online</h2>
                <!-- Puedes usar un video o una imagen para la introducción -->
<iframe width="1350" height="480" src="https://www.youtube.com/embed/4-VMuLs99Ss" title="Curso Photoshop - Capitulo 0, Introducción para Principiantes" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>            </div>
        </section>

        <section id="cursos">
            <!-- Lista de todos los cursos -->
            <h2>Tus Cursos</h2>
            <ul>
                <li><a href="/./Cursos/Curso 1.php" >Lección 1: Introducción </a></li>
                <li><a href="/./Cursos/Curso 2.php" >Lección 2: Herramientas Básicas de Photoshop</a></li>
                <li><a href="/./Cursos/Curso 3.php" >Lección 3: Capas y Máscaras</a></li>
                <li><a href="/./Cursos/Curso 4.php" >Lección 4: Filtros y Efectos</a></li>
                <li><a href="/./Cursos/Curso 5.php" >Lección 5: Técnicas Avanzadas de Retoque</a></li>
                <li><a href="/./Cursos/Curso 6.php" >Lección 6: Creación de Gráficos y Diseño Visual</a></li>
                <li><a href="/./Cursos/Curso 7.php" >Lección 7: Trabajo con Texturas y Tipografías</a></li>
                <li><a href="/./Cursos/Curso 8.php" >Lección 8: Principios de Diseño de Interfaces</a></li>
                <li><a href="/./Cursos/Curso 9.php" >Lección 9: #</a></li>
                <li><a href="/./Cursos/Curso 10.php" >Lección 10: #</a></li>
                <!-- Agrega más lecciones según sea necesario -->
            </ul>
        </section>

        <section id="progreso">
            <!-- Sección para mostrar el progreso del alumno -->
            <h2>Tu Progreso</h2>
            <h3>Compleción: 100%</h3> <!-- Ajusta este valor según el progreso real -->
            <progress value="100" max="100"></progress>
        </section>
    </div>
</template>

<script>
export default {
    name:"dashboard",
    data(){
        return {
            user:this.$store.state.auth.user
        }
    }
}
</script>
<style>

section {
    width: 100%;
    margin: 40px 0;
}

iframe {
    width: 100%;
    height: 300px;
}

h2 {
    color: #fff;
    margin-bottom: 10px;
}
h3 {
    color: #fff;
    margin-bottom: 10px;
    font-size: small;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    margin-bottom: 10px;
}

li a {
    color: black;
    display: block;
    padding: 10px;
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    transition: background-color 0.3s;
}

li a:hover {
    background-color: #f5f5f5;
    color: red;
}

progress {
    width: 100%;
}
</style>
