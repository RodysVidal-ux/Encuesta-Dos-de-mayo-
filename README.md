<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Encuesta Online</title>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>

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

    <button onclick="votar('Locro de papa')">
        Locro de papa
    </button>

    <button onclick="votar('Horneado')">
        Horneado
    </button>

    <button onclick="votar('Llapingachos')">
        Llapingachos
    </button>

    <button onclick="votar('Fritada')">
        Fritada
    </button>

    <button onclick="votar('Humitas')">
        Humitas
    </button>

    <div id="mensaje"></div>

    <button onclick="abrirAdmin()">
        Panel Admin
    </button>

    <div id="admin">

        <h3>Resultados</h3>

        <div id="resultados"></div>

    </div>

</div>

<script>

const SUPABASE_URL = "PEGA_TU_URL";
const SUPABASE_KEY = "PEGA_TU_KEY";

const supabase = window.supabase.createClient(
    SUPABASE_URL,
    SUPABASE_KEY
);


// VOTAR
async function votar(comida){

    if(localStorage.getItem("yaVoto")){

        document.getElementById("mensaje").innerText =
        "Ya realizaste tu votación.";

        return;
    }

    await supabase
    .from("votos")
    .insert([
        { comida: comida }
    ]);

    localStorage.setItem("yaVoto", "si");

    document.getElementById("mensaje").innerText =
    "Gracias por tu votación.";
}


// ADMIN
async function abrirAdmin(){

    let clave = prompt("Ingrese contraseña admin");

    if(clave !== "1234"){

        alert("Contraseña incorrecta");

        return;
    }

    document.getElementById("admin").style.display = "block";

    const { data } = await supabase
    .from("votos")
    .select("*");

    let total = data.length;

    let conteo = {};

    data.forEach(v => {

        conteo[v.comida] =
        (conteo[v.comida] || 0) + 1;

    });

    let html = "";

    for(let comida in conteo){

        let porcentaje =
        ((conteo[comida] / total) * 100).toFixed(1);

        html += `
        <p>
            ${comida}: ${porcentaje}% (${conteo[comida]} votos)
        </p>
        `;
    }

    html += `<p>Total votos: ${total}</p>`;

    document.getElementById("resultados").innerHTML = html;
}

</script>

</body>
</html>