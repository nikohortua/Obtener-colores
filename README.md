<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Mezcla de Colores</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
            background-color: #f4f4f4;
        }
        .color-box {
            width: 120px;
            height: 120px;
            margin: 10px auto;
            border-radius: 50%;
            border: 2px solid #000;
            box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.2);
        }
        .inputs {
            margin-top: 10px;
        }
        #colorPicker {
            border-radius: 50%;
            width: 50px;
            height: 50px;
            border: none;
            cursor: pointer;
        }
        button {
            padding: 10px 15px;
            font-size: 16px;
            border: none;
            background-color: #007bff;
            color: white;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            background-color: #0056b3;
        }
        .search-box {
            margin-top: 20px;
        }
        input[type="text"] {
            padding: 8px;
            width: 200px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h2>Calculadora de Mezcla de Colores</h2>
    <input type="color" id="colorPicker" value="#ff0000" onchange="actualizarColor()">
    <div class="color-box" id="colorDisplay"></div>
    <p id="codigoColor">Código: #ff0000</p>
    
    <div class="inputs">
        <label for="cantidad">Cantidad total de pintura (ml): </label>
        <input type="number" id="cantidad" value="100" min="1">
        <button onclick="calcularProporcion()">Calcular</button>
    </div>
    
    <h3>Proporciones:</h3>
    <p id="proporciones"></p>
    
    <div class="search-box">
        <h3>Buscar color de personaje</h3>
        <input type="text" id="searchColor" placeholder="Ejemplo: pelo de Morty">
        <button onclick="buscarColor()">Buscar</button>
        <p id="resultadoColor"></p>
    </div>
    
    <script>
        const coloresPersonajes = {
            "pelo de Morty": "#b08040",
            "piel de Homer Simpson": "#ffcc00",
            "pelo de Goku": "#ff9900",
            "traje de Spiderman": "#d00000",
            "chaqueta de Rick": "#a0a0a0"
        };

        function actualizarColor() {
            let color = document.getElementById("colorPicker").value;
            document.getElementById("colorDisplay").style.backgroundColor = color;
            document.getElementById("codigoColor").innerText = "Código: " + color;
        }

        function calcularProporcion() {
            let cantidad = parseFloat(document.getElementById("cantidad").value);
            let color = document.getElementById("colorPicker").value;
            
            let r = parseInt(color.substr(1,2), 16) / 255;
            let g = parseInt(color.substr(3,2), 16) / 255;
            let b = parseInt(color.substr(5,2), 16) / 255;
            
            let amarillo = Math.max(0, Math.min(1, g - Math.min(r, b)));
            let rojo = Math.max(0, Math.min(1, r - b));
            let azul = Math.max(0, Math.min(1, b - r));
            let negro = Math.min(r, g, b);
            let blanco = 1 - Math.max(rojo, amarillo, azul, negro);
            
            let total = rojo + azul + amarillo + blanco + negro;
            let proporcionRojo = (rojo / total) * cantidad;
            let proporcionAzul = (azul / total) * cantidad;
            let proporcionAmarillo = (amarillo / total) * cantidad;
            let proporcionBlanco = (blanco / total) * cantidad;
            let proporcionNegro = (negro / total) * cantidad;
            
            document.getElementById("proporciones").innerHTML =
                `Rojo: ${proporcionRojo.toFixed(2)} ml<br>
                Azul: ${proporcionAzul.toFixed(2)} ml<br>
                Amarillo: ${proporcionAmarillo.toFixed(2)} ml<br>
                Blanco: ${proporcionBlanco.toFixed(2)} ml<br>
                Negro: ${proporcionNegro.toFixed(2)} ml`;
        }

        function buscarColor() {
            let consulta = document.getElementById("searchColor").value.toLowerCase();
            let resultado = coloresPersonajes[consulta];
            if (resultado) {
                document.getElementById("resultadoColor").innerHTML = `Código de color: ${resultado}`;
                document.getElementById("colorPicker").value = resultado;
                actualizarColor();
            } else {
                document.getElementById("resultadoColor").innerHTML = "No encontrado";
            }
        }
    </script>
</body>
</html>
