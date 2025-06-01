<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Laboratorio JavaScript y AJAX</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f4f6f8;
      margin: 0;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #1a1a1a;
    }

    h2 {
      color: #2e5c8a;
      border-bottom: 2px solid #ddd;
      padding-bottom: 4px;
    }

    input, button {
      padding: 8px;
      margin: 5px 0;
      font-size: 1rem;
    }

    button {
      background-color: #2e5c8a;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }

    button:hover {
      background-color: #1d3f66;
    }

    .section {
      background: white;
      padding: 20px;
      margin-bottom: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    #contenido, #cabeceras {
      background-color: #f1f1f1;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      white-space: pre-wrap;
    }
  </style>
</head>
<body>

  <h1>Laboratorio de Desarrollo en Red</h1>

  <div class="section">
    <h2>1. Palíndromo</h2>
    <input type="text" id="palindromoInput" placeholder="Escribe una palabra" />
    <button onclick="verificarPalindromo()">Verificar</button>
    <p id="palindromoResultado"></p>
  </div>

  <div class="section">
    <h2>2. Comparar Números</h2>
    <input type="number" id="num1" placeholder="Número 1" />
    <input type="number" id="num2" placeholder="Número 2" />
    <button onclick="compararNumeros()">Comparar</button>
    <p id="compararResultado"></p>
  </div>

  <div class="section">
    <h2>3. Vocales presentes</h2>
    <input type="text" id="frase1" placeholder="Escribe una frase" />
    <button onclick="vocalesPresentes()">Detectar vocales</button>
    <p id="vocalesResultado"></p>
  </div>

  <div class="section">
    <h2>4. Contar Vocales</h2>
    <input type="text" id="frase2" placeholder="Escribe una frase" />
    <button onclick="contarVocales()">Contar vocales</button>
    <p id="conteoResultado"></p>
  </div>

  <div class="section">
    <h2>5. AJAX: Mostrar contenido remoto</h2>
    <input type="text" id="urlInput" value="" style="width: 60%;" />
    <button onclick="realizarPeticion()">Mostrar Contenido</button>

    <h3>Contenido:</h3>
    <div id="contenido"></div>

    <h3>Estado de la petición:</h3>
    <p id="estado"></p>

    <h3>Cabeceras HTTP:</h3>
    <pre id="cabeceras"></pre>

    <h3>Código de estado:</h3>
    <p id="codigoEstado"></p>
  </div>

  <script>
    function verificarPalindromo() {
      const palabra = document.getElementById('palindromoInput').value.toLowerCase().replace(/\s/g, '');
      const reverso = palabra.split('').reverse().join('');
      const resultado = (palabra === reverso) ? 'Es un palíndromo' : 'No es un palíndromo';
      document.getElementById('palindromoResultado').innerText = resultado;
    }

    function compararNumeros() {
      const a = parseFloat(document.getElementById('num1').value);
      const b = parseFloat(document.getElementById('num2').value);
      let resultado = '';
      if (a === b) resultado = 'Ambos números son iguales';
      else resultado = (a > b) ? `${a} es mayor` : `${b} es mayor`;
      document.getElementById('compararResultado').innerText = resultado;
    }

    function vocalesPresentes() {
      const texto = document.getElementById('frase1').value.toLowerCase();
      const vocales = ['a', 'e', 'i', 'o', 'u'];
      const presentes = vocales.filter(v => texto.includes(v));
      document.getElementById('vocalesResultado').innerText = `Vocales presentes: ${presentes.join(', ')}`;
    }

    function contarVocales() {
      const texto = document.getElementById('frase2').value.toLowerCase();
      const contador = { a: 0, e: 0, i: 0, o: 0, u: 0 };
      for (let letra of texto) {
        if (contador.hasOwnProperty(letra)) contador[letra]++;
      }
      const resultado = Object.entries(contador).map(([v, c]) => `${v.toUpperCase()}: ${c}`).join(', ');
      document.getElementById('conteoResultado').innerText = resultado;
    }

    window.onload = () => {
      document.getElementById('urlInput').value = window.location.href;
    };

    function realizarPeticion() {
      const url = document.getElementById('urlInput').value;
      const xhr = new XMLHttpRequest();

      xhr.open('GET', url, true);

      xhr.onreadystatechange = function () {
        const estados = {
          0: 'No iniciada',
          1: 'Conexión establecida',
          2: 'Petición recibida',
          3: 'Procesando',
          4: 'Completada',
        };
        document.getElementById('estado').innerText = estados[xhr.readyState] || 'Desconocido';

        if (xhr.readyState === 4) {
          document.getElementById('contenido').innerText = xhr.responseText;
          document.getElementById('cabeceras').innerText = xhr.getAllResponseHeaders();
          document.getElementById('codigoEstado').innerText = `${xhr.status} ${xhr.statusText}`;
        }
      };

      xhr.send();
    }
  </script>
</body>
</html>
