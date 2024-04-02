<script>
  import {onMount} from "svelte"
  import * as d3 from "d3"

  let numbers = [10,25,33,62,75,100]
  let pathLength
  let largo
  let color

  onMount(() => {
    // @ts-ignore
    // longitud total del path
    pathLength = document.querySelector("#path").getTotalLength()
  
    // longitud total del path
    let maxGap = pathLength - (pathLength * d3.min(numbers) / d3.max(numbers))
    let minGap = 0

    // console.log("total path", pathLength)
    // console.log("maximo gap", maxGap)
    // console.log("minimo gap", minGap)

    largo = function (n) {
      let scale = d3
        .scaleLinear()
        .domain(d3.extent(numbers)) // extent returns [min, max]
        .range([maxGap, minGap])
      return scale(n)
    }

    color = d3.scaleSequential(d3.interpolateRgb("#FF0000", "#48FF00"))
      .domain([d3.min(numbers), d3.max(numbers)])
  })

console.log(screen.width)
</script>

<video autoplay muted loop id="myVideo">
  <source src="../images/background-video.mp4" type="video/mp4">
</video>

<h1 class="title"> Visualización de vueltas completas </h1>
<div class="laps-container">
{#each numbers as n}
    <div class= "grid-item">
      <svg viewBox="0 0 706 376">
        <!-- Pista gris -->
        <path
          id="path"
          d = "M21.132 35.4607C39.0324 23.5699 78.3621 5.45356 113.562 16.3607C159.148 30.486 181.577 117.504 181.577 117.504C181.577 117.504 310.117 271.648 333.302 263.359C356.488 255.07 368.101 256.867 384.749 278.986C401.398 301.105 694.304 227.329 692.996 282.459C691.688 337.588 275.751 367.975 275.751 362.332C275.751 356.688 283.163 349.743 275.751 349.743C268.34 349.743 87.8709 351.758 62.1153 150.061L51.2155 147.022C15.595 77.9498 3.23163 47.3516 21.132 35.4607Z"
          fill="none"
          stroke="#fafafa"
          stroke-width="20"
        />
        <!-- Relleno pista -->
        {#if pathLength}
          <path
            id="path"
            d="M21.132 35.4607C39.0324 23.5699 78.3621 5.45356 113.562 16.3607C159.148 30.486 181.577 117.504 181.577 117.504C181.577 117.504 310.117 271.648 333.302 263.359C356.488 255.07 368.101 256.867 384.749 278.986C401.398 301.105 694.304 227.329 692.996 282.459C691.688 337.588 275.751 367.975 275.751 362.332C275.751 356.688 283.163 349.743 275.751 349.743C268.34 349.743 87.8709 351.758 62.1153 150.061L51.2155 147.022C15.595 77.9498 3.23163 47.3516 21.132 35.4607Z"
            fill="none"
            stroke= {color(n)}
            stroke-width="20"
            stroke-dasharray={pathLength}
            stroke-dashoffset={largo(n)}
          />
        {/if}
      </svg>
      <p class="number">{Math.round((50)*n/100)} laps</p>
    </div>
    {/each}
  </div>

<style>

  .title {
    position: relative;
    font-size: 2vw;
    font-family: 'Formula1-Italic';
    color: white;
    margin-top: 20px;
    margin-bottom: 40px;
    text-decoration: underline;
    text-align: center;
    z-index: 10;
  }
  .laps-container {
    position: relative;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    align-items: center;
    justify-content: center;
    z-index: 10;
  }
  .grid-item {
    justify-content: center;
    align-items: center;
  }
  .number {
    text-align: center;
    font-size: 2vw;
    font-family: 'Formula1-Italic';
    color: white;
    margin-top: 10px;
  }

  #myVideo {
  position: absolute;
  right: 0;
  bottom: 0;
  min-width: 100%;
  min-height: 100%;
  z-index: 1;
  opacity: 30%;
  }
  @media (min-width: 1300px) {
    .grid-item svg {
      width: 300px;
      height: auto;
  }}
  @media (min-width: 1600px) {
    .title {
      font-size: 2.75vw; /* Fixed font size for larger screens */
      padding-bottom: 50px;
    }

    .grid-item svg {
      width: 400px;
      height: auto;
    }
    .number {
      font-size: 40px; /* Fixed font size for larger screens */
    }
  }
</style>
