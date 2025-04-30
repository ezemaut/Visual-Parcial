<script type="module">
  import * as d3 from "d3";

  // Seeded PRNG: Mulberry32
  function mulberry32(seed) {
    return function() {
      let t = seed += 0x6D2B79F5;
      t = Math.imul(t ^ (t >>> 15), t | 1);
      t ^= t + Math.imul(t ^ (t >>> 7), t | 61);
      return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
    }
  }

  // Seeded RNG instance
  const rng = mulberry32(17); // You can change this seed for a different layout

  const grid_size = 6;
  const grid_offset = 0;

  let players = ["Messi", "Maradona"];
  let PlayerAndGoals = {};

  // Data: manually defined
  PlayerAndGoals = {
    'Messi': [24, 33, 42, 54, 63, 71, 77, 87, 92, 98],
    'Maradona': [23, 35, 45, 56, 50, 43, 38, 32, 28, 25]
  };


  let size_var = [];
  
  for (let jugador of players) {
    size_var = size_var.concat(PlayerAndGoals[jugador]);
  }
  
  let messi = [];
  let marado = [];

  for (let col = 1 + grid_offset; col <= grid_size - grid_offset; col++) {
    for (let row = 1 + grid_offset; row <= grid_size - grid_offset; row++) {
      if ((col === grid_offset + 1 || col === grid_size) ||
          (row === grid_offset + 1 || row === grid_size)) {
        messi.push({ col, row });
      } else {
        marado.push({ col, row });
      }
    }
  }

  let posJugadores = [messi, marado];
  let positions = [];
  let i = 0;

  for (let jugador of players) {
    let nums = PlayerAndGoals[jugador];
    for (let n of nums) {
      let pos;
      if (posJugadores[i].length > 0) {
        pos = posJugadores[i].splice(Math.floor(rng() * posJugadores[i].length), 1)[0];
      } else {
        pos = {
          col: Math.floor(rng() * (grid_size - grid_offset)) + 1,
          row: Math.floor(rng() * (grid_size - grid_offset)) + 1
        };
      }

      positions.push({
        value: n,
        col: pos.col,
        row: pos.row,
        juga: jugador,
        offsetLeft: rng() * 75,
        offsetTop: rng() * 5
      });
    }
    i++;
  }

  const sizeScale = d3.scaleLinear()
    .domain([d3.min(size_var), d3.max(size_var)])
    .range([40, 70]);

</script>

<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap" rel="stylesheet">
<main id="container">
  <div class='title-box'>
    <h1 class="title">Titanes del gol: Messi y Maradona en la cancha</h1>
  </div>

  <h2 class='Subtitulo'>¿Dónde hacen magia? Mapa de goles en sus mejores años</h2>

  <div class="chart">
    <img src="./images/Arco.svg" alt="arco" class="goal-image" />
    <div class="cardinal-container">
      {#each positions as { value, col, row, juga, offsetLeft, offsetTop }}
      <div class="circle {juga}"
        style="
          width: {sizeScale(value)}px; 
          height: {sizeScale(value)}px;
          font-size: {sizeScale(value) / 2}px;
          grid-column: {col}; 
          grid-row: {row};
          position: relative;
          left: {offsetLeft}px; 
          top: {offsetTop}px;
        ">
        {value}
      </div>
      {/each}
    </div>
  </div>

  <div class='ref-box'>
    <h3 class='referencias'>
      <span class="item"><span class="circle Messi"></span> Messi</span>  
      <span class="item"><span class="circle Maradona"></span> Maradona</span>  
    </h3>
  </div>

  <div class="descripcion-box"> 
    <p> Entre los 18 y los 24 años, Lionel Messi y Diego Maradona vivieron sus etapas más explosivas frente al arco. A partir de datos oficiales de
    ligas, copas nacionales e internacionales, se trazó un mapa detallado de los goles que convirtieron durante sus respectivos primes.</p> 
    <p>El gráfico revela un contraste marcado en sus estilos de definición. <strong>Messi</strong> concentra sus goles en las esquinas del arco,
    evitando sistemáticamente el centro. Su tendencia a buscar los ángulos no solo habla de precisión, sino también de una lectura quirúrgica del 
    juego. Además, se destaca por anotar desde una mayor variedad de posiciones y alcanzar un volumen goleador superior.</p> 
    <p><strong>Maradona</strong>, en cambio, muestra una preferencia por definir en zonas más centrales del arco. Esta preferencia puede explicarse
    por su explosividad, potencia de remate y su capacidad de decidir en fracciones de segundo dentro del área. Este patrón refleja un estilo más 
    directo, más visceral, y profundamente efectivo en el contexto del fútbol de su época.</p> 
  </div>


   <section class="charts-section">

    <!-- PRIMERA FILA: Gráfico a la izquierda, texto a la derecha -->
    <div class="chart-row-double">
      <div class="chart-box-half">
        <div class="flourish-embed flourish-chart" data-src="visualisation/22921930">
          <script src="https://public.flourish.studio/resources/embed.js"></script>
        </div>
      </div>
      <div class="text-container">
        <h3 class="final-title"> La juventud goleadora de dos grandes leyendas del fútbol</h3>
        <p class="chart-description">
          En su etapa más explosiva, tanto Messi como Maradona dejaron huella como goleadores natos.
          Este gráfico de barras muestra que, entre los 18 y 24 años, Messi convirtió más goles que Maradona, 
          marcando una diferencia que, aunque no aplastante, resulta significativa considerando que ambos atravesaban 
          sus años de mayor vitalidad.
          La cifra anticipa un rasgo que se mantendrá a lo largo de sus carreras: Messi como un finalizador constante, y
           Maradona como un ejecutor preciso y oportuno.
        </p>
      </div>  
    </div>
  
  <!-- SEGUNDA FILA: -->
  <div class="chart-row-double row-reverse">
    <div class="text-container">
      <h3 class="final-title">Territorios del Gol: <br> ¿Dónde mandan Messi y Maradona?</h3>
    <div class="chart-box-half">
      <div class="flourish-embed flourish-pictogram" data-src="visualisation/22923841">
        <script src=src="https://public.flourish.studio/resources/embed.js"></script>
      </div>
      <p><strong>Pocos</strong>: Menos de 30 goles<br>
        <strong>Medio</strong>: Entre 31 y 50 goles<br>
        <strong>Muchos</strong>: Mas de 50 goles</p>
    </div>
      <p class="text-container">
        Este gráfico muestra la concentración de goles según zonas de los goles de cada uno. <strong>Messi</strong> destaca por una clara <em>preferencia repetida</em>: 
        el 78% de sus goles se concentran en unas pocas áreas específicas, lo que indica una <strong>consistencia quirúrgica</strong> en su estilo de finalización. 
        <br>
        En contraste, <strong>Maradona</strong> reparte sus goles de forma mucho más equilibrada: un 33% en cada categoría de zonas (pocos, medios y muchos), 
        lo que sugiere una <strong>mayor variedad</strong> en sus definiciones y una capacidad de adaptarse a diferentes situaciones sin depender de una sola zona del arco.
      </p>

  </div>
</div>


<h3 class="final-title">Puntería Letal: ¿Dónde apuntaban los genios?</h3>
<h4 class="chart-subtitle">Este gráfico revela la brújula interna de Messi y Maradona al momento de definir</h4>
<div class="chart-box-3">
  <div class="flourish-embed flourish-chart" data-src="visualisation/22690208" style="height: 450px;">
    <script src="https://public.flourish.studio/resources/embed.js"></script>
  </div>
</div>
<p class="chart-description">
  Podemos observar que <strong>Messi</strong> muestra una clara inclinación por rematar hacia la parte superior del arco, especialmente al centro, donde concentra la mayor cantidad de definiciones. 
  Las zonas derecha e izquierda también son protagonistas, pero en menor medida. Las finalizaciones bajas son más escasas, lo que sugiere una preferencia por tiros elevados, difíciles de alcanzar 
  para cualquier arquero. <br>
  <strong>Maradona</strong>, en contraste, presenta una distribución más pareja. Si bien hay una leve concentración hacia la izquierda, su estilo revela adaptabilidad:
   podía convertir desde cualquier ángulo sin depender de una única zona. Su versatilidad frente al arco era parte de su magia.
</p>


  </section>


  <footer class="footer">
    <div class="footer-content">
      <a href="https://github.com/Sofiik1/Visual" target="_blank"> <i class="fab fa-github"></i> Repositorio en GitHub </a>
      |
      <a href="https://www.linkedin.com/in/sofia-kutter" target="_blank">  <i class="fab fa-linkedin"></i> Sofía Kutter </a>
      |
      <a href="https://www.linkedin.com/in/ezequiel-mautner-031333360/" target="_blank">  <i class="fab fa-linkedin"></i> Ezequiel Mautner</a>
    </div>
  </footer>
  
</main>

<style>
  .cardinal-container {
    position:absolute;
    align-items:center;
    top: 25px;
    left: 25px;
    width: 94%;
    height: 95%;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    grid-template-rows: repeat(6, 1fr);
    margin-bottom: 20px;
  }


</style>


 
