<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Problema de Diseño de Tuberías</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    <style>
        :root{--bg:#f6f9fc;--card:#ffffff;--accent:#0b6fab}
        body{font-family:Inter, system-ui, Arial, sans-serif;background:var(--bg);color:#0b2240;margin:24px}
        .card{background:var(--card);border-radius:12px;box-shadow:0 6px 18px rgba(10,20,40,0.08);padding:20px;max-width:820px;margin:0 auto}
        h1{font-size:20px;margin:0 0 12px}
        p{margin:6px 0}
        .line{margin:10px 0}
        button{background:var(--accent);color:white;border:0;padding:10px 12px;border-radius:8px;cursor:pointer;margin-top:15px}
        .resultado{font-weight:bold;color:var(--accent);margin-top:10px}
    </style>
</head>
<body>
    <div class="card">
        <h1>Diseño de sistema de fluidos</h1>
        <p><strong>Problema:</strong> Se está diseñando un sistema de distribución de fluidos bombeados para suministrar <strong>400 gal/min</strong> de agua a un sistema de enfriamiento en una planta de generación de energía. Utilice la figura de selección de tamaño de tubería para hacer una selección inicial de diámetros de tubería cédula 40 para las líneas de succión y descarga implementadas en este sistema. Después calcule la velocidad de flujo media real para cada tubería.</p>

        <div id="solution"></div>
        <button id="nextBtn">Mostrar siguiente paso</button>
    </div>

    <script>
        const pasos = [
            {tipo:"intro", texto:"1) Identificar los datos del problema:"},
            {tipo:"eq", linea:0, partes:["Q = 400 \\, gal/min"]},
            {tipo:"eq", linea:1, partes:["\\text{Tubería de succión: } 4 \\, \\text{in cédula 40} \\, \\Rightarrow \\, A_s = 0.08840 \\, ft^2"]},
            {tipo:"eq", linea:2, partes:["\\text{Tubería de descarga: } 3 \\, \\text{in cédula 40} \\, \\Rightarrow \\, A_d = 0.05132 \\, ft^2"]},

            {tipo:"intro", texto:"2) Convertir el flujo de volumen (Q) de gal/min a ft³/s:"},
            {tipo:"eq", linea:3, partes:["Q = 400 \\, gal/min \\times \\frac{1 \\, ft^3}{7.48 \\, gal} \\times \\frac{1 \\, min}{60 \\, s}"]},
            {tipo:"add", linea:3, texto:"= 0.891 \\, ft^3/s"},

            {tipo:"intro", texto:"3) Calcular la velocidad de flujo en la tubería de succión (v_s):"},
            {tipo:"eq", linea:4, partes:["v_s = \\frac{Q}{A_s} = \\frac{0.891 \\, ft^3/s}{0.08840 \\, ft^2}"]},
            {tipo:"add", linea:4, texto:"= 10.08 \\, ft/s"},

            {tipo:"intro", texto:"4) Calcular la velocidad de flujo en la tubería de descarga (v_d):"},
            {tipo:"eq", linea:5, partes:["v_d = \\frac{Q}{A_d} = \\frac{0.891 \\, ft^3/s}{0.05132 \\, ft^2}"]},
            {tipo:"add", linea:5, texto:"= 17.36 \\, ft/s"},
        ];

        const contenedor = document.getElementById('solution');
        const lineas = [];
        let pasoActual = 0;

        document.getElementById('nextBtn').addEventListener('click', () => {
            if(pasoActual >= pasos.length){
                const resultado1 = document.createElement('p');
                resultado1.className = 'resultado';
                resultado1.innerHTML = "Velocidad de succión: <strong>10.08 ft/s</strong>";
                contenedor.appendChild(resultado1);

                const resultado2 = document.createElement('p');
                resultado2.className = 'resultado';
                resultado2.innerHTML = "Velocidad de descarga: <strong>17.36 ft/s</strong>";
                contenedor.appendChild(resultado2);
                
                const comentario = document.createElement('p');
                comentario.className = 'line';
                comentario.innerHTML = "Aunque estos tamaños de tuberías y velocidades deben ser aceptables en el servicio normal, hay situaciones en las que se desean velocidades más bajas para limitar las pérdidas de energía en el sistema.";
                contenedor.appendChild(comentario);

                document.getElementById('nextBtn').style.display = 'none';
                return;
            }
            const paso = pasos[pasoActual];

            if(paso.tipo === "intro"){
                const p = document.createElement('p');
                p.className = 'line';
                p.textContent = paso.texto;
                contenedor.appendChild(p);
            } else if(paso.tipo === "eq"){
                const p = document.createElement('p');
                p.className = 'line eq';
                p.dataset.linea = paso.linea;
                p.innerHTML = `\\[ ${paso.partes.join(' ')} \\]`;
                contenedor.appendChild(p);
                lineas[paso.linea] = {elemento: p, partes:[...paso.partes]};
            } else if(paso.tipo === "add"){
                const linea = lineas[paso.linea];
                if (linea) {
                    linea.partes.push(paso.texto);
                    linea.elemento.innerHTML = `\\[ ${linea.partes.join(' ')} \\]`;
                }
            }

            MathJax.typesetPromise();
            pasoActual++;
        });
    </script>
</body>
</html>
