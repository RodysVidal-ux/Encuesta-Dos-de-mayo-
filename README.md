<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Encuesta de Comida</title>

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
    width:320px;
    text-align:center;
}

h4{
    color:#bdbdbd;
    margin-top:-10px;
    margin-bottom:15px;
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

#admin{
    display:none;
    margin-top:20px;
    background:#111;
    padding:10px;
    border-radius:10px;
    text-align:left;
}

#mensaje{
    margin-top:15px;
    color:#00ff99;
    font-weight:bold;
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

    <button onclick="votar('Locro de papa')">Locro de papa</button>

    <button onclick="votar('Horneado')">Horneado</button>

    <button onclick="votar('Llapingachos')">Llapingachos</button>

    <button onclick="votar('Fritada')">Fritada</button>

    <button onclick="votar('Humitas')">Humitas</button>

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

<script>

let votos = {
    "Locro de papa":0,
    "Horneado":0,
    "Llapingachos":0,
    "Fritada":0,
    "Humitas":0
};

function votar(comida){

    // Verifica si ya votó
    if(localStorage.getItem("yaVoto")){

        document.getElementById("mensaje").innerText =
        "Ya realizaste tu votación.";

        return;
    }

    votos[comida]++;

    actualizar();

    // Guarda que ya votó
    localStorage.setItem("yaVoto", "si");

    document.getElementById("mensaje").innerText =
    "Gracias por tu votación.";
}

function actualizar(){

    let total = 0;

    for(let comida in votos){
        total += votos[comida];
    }

    document.getElementById("total").innerText = total;

    document.getElementById("locro").innerText =
        porcentaje("Locro de papa", total);

    document.getElementById("horneado").innerText =
        porcentaje("Horneado", total);

    document.getElementById("llapingachos").innerText =
        porcentaje("Llapingachos", total);

    document.getElementById("fritada").innerText =
        porcentaje("Fritada", total);

    document.getElementById("humitas").innerText =
        porcentaje("Humitas", total);
}

function porcentaje(comida, total){

    if(total === 0){
        return "0%";
    }

    return ((votos[comida] / total) * 100).toFixed(1) + "%";
}

function abrirAdmin(){

    let clave = prompt("Ingrese contraseña admin");

    if(clave === "1234"){
        document.getElementById("admin").style.display = "block";
    }else{
        alert("Contraseña incorrecta");
    }
}

</script>

</body>
</html>