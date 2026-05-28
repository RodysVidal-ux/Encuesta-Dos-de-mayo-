<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Encuesta Online</title>

<style>
body{
    font-family: Arial;
    background:#1e1e1e;
    color:white;
    display:flex;
    justify-content:center;
    align-items:center;
    height:100vh;
}

.box{
    background:#2b2b2b;
    padding:25px;
    border-radius:15px;
    width:330px;
    text-align:center;
}

h4{
    color:#bdbdbd;
    margin-top:-10px;
    font-weight:normal;
}

button{
    width:100%;
    padding:12px;
    margin-top:10px;
    border:none;
    border-radius:10px;
    cursor:pointer;
    font-size:16px;
    background:#444;
    color:white;
}

button:hover{
    background:#666;
}

#mensaje{
    margin-top:15px;
    color:#00ff99;
    font-weight:bold;
}

#admin{
    display:none;
    margin-top:20px;
    background:#111;
    padding:10px;
    border-radius:10px;
    text-align:left;
}
</style>
</head>

<body>

<div class="box">

    <h2>Bienvenido/a</h2>

    <h4>Encuesta realizada por el 2 BGU "A"</h4>

    <p>
        Encuesta de Platos típicos de la sierra
        elije el que más te guste.
    </p>

    <button onclick="votar('locro')">Locro de papa</button>

    <button onclick="votar('horneado')">Horneado</button>

    <button onclick="votar('llapingachos')">Llapingachos</button>

    <button onclick="votar('fritada')">Fritada</button>

    <button onclick="votar('humitas')">Humitas</button>

    <div id="mensaje"></div>

    <button onclick="abrirAdmin()">Panel Admin</button>

    <div id="admin">

        <h3>Resultados</h3>

        <p>Locro de papa: <span id="locro">0%</span></p>

        <p>Horneado: <span id="horneado">0%</span></p>

        <p>Llapingachos: <span id="llapingachos">0%</span></p>

        <p>Fritada: <span id="fritada">0%</span></p>

        <p>Humitas: <span id="humitas">0%</span></p>

        <p>Total votos: <span id="total">0</span></p>

    </div>

</div>

<script type="module">

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";

import {
getFirestore,
doc,
getDoc,
setDoc,
updateDoc,
increment
} from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";


const firebaseConfig = {

  apiKey: "AIzaSyDQCpdCLhKb7YPLfUcAdos8UL5NzKdGlFM",

  authDomain: "encuesta-2bgu.firebaseapp.com",

  projectId: "encuesta-2bgu",

  storageBucket: "encuesta-2bgu.firebasestorage.app",

  messagingSenderId: "888710509133",

  appId: "1:888710509133:web:1de7a62b47fd659586e204",

  measurementId: "G-W3D6PHCGD5"

};

const app = initializeApp(firebaseConfig);

const db = getFirestore(app);

const encuestaRef = doc(db, "encuesta", "votos");


async function iniciar(){

    const snap = await getDoc(encuestaRef);

    if(!snap.exists()){

        await setDoc(encuestaRef, {

            locro:0,
            horneado:0,
            llapingachos:0,
            fritada:0,
            humitas:0

        });
    }

    cargarResultados();
}

iniciar();


window.votar = async function(tipo){

    if(localStorage.getItem("yaVoto")){

        document.getElementById("mensaje").innerText =
        "Ya realizaste tu votación.";

        return;
    }

    await updateDoc(encuestaRef, {

        [tipo]: increment(1)

    });

    localStorage.setItem("yaVoto", "si");

    document.getElementById("mensaje").innerText =
    "Gracias por tu votación.";

    cargarResultados();
}


async function cargarResultados(){

    const snap = await getDoc(encuestaRef);

    const datos = snap.data();

    let total =
        datos.locro +
        datos.horneado +
        datos.llapingachos +
        datos.fritada +
        datos.humitas;

    document.getElementById("total").innerText = total;

    document.getElementById("locro").innerText =
        porcentaje(datos.locro,total);

    document.getElementById("horneado").innerText =
        porcentaje(datos.horneado,total);

    document.getElementById("llapingachos").innerText =
        porcentaje(datos.llapingachos,total);

    document.getElementById("fritada").innerText =
        porcentaje(datos.fritada,total);

    document.getElementById("humitas").innerText =
        porcentaje(datos.humitas,total);
}

function porcentaje(valor,total){

    if(total === 0){
        return "0%";
    }

    return ((valor / total) * 100).toFixed(1) + "%";
}


window.abrirAdmin = function(){

    let clave = prompt("Ingrese contraseña admin");

    if(clave === "1234"){

        document.getElementById("admin").style.display = "block";

        cargarResultados();

    }else{

        alert("Contraseña incorrecta");
    }
}

</script>

</body>
</html>