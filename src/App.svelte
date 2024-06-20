<script>

  let city, data, lat, lon, geo
  let key = import.meta.env.VITE_KEY

  if (!navigator.geolocation) {  
    geo = 'Tu navegador no soporta Geolocalización'
  } else {
    navigator.geolocation.getCurrentPosition(async (position) => {
      lat = position.coords.latitude
      lon = position.coords.longitude
      const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${key}&lang=es&units=metric`)
      data = await res.json();   
    })
  }

  const weater = async (e) => {
    const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${key}&lang=es&units=metric`);
    data = await res.json();
    e.target.reset();
  };

</script>

<main>
  
  <form on:submit|preventDefault={weater}>
    <input type="text" bind:value={city} />
    <button type="submit">Enviar</button>
  </form>

  {#if geo != undefined}
    <p>{geo}</p>
  {/if}

 {#if data}
  <h3>{data.name}</h3>
  <p>Temperatura: <strong>{data.main.temp}</strong></p>
  <p>Clima: <strong>{data.weather[0].description}</strong></p>
  <img src={`https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`} alt="">
 {/if}

</main>
