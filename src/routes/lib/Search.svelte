<script lang="ts">
    let inputValue = "";
    let albumName = "";
    let albumArtist = "";
    let albumImage = "";
    let albumUrl = "";
    let albumListeners = "";
    let albumPlaycount = "";

    async function search() {
      /* Song Info */
      let url = `https://ws.audioscrobbler.com/2.0/?method=album.search&album=${inputValue}&api_key=${import.meta.env.VITE_APP_LASTFM_KEY}&format=json`;
      let res = await fetch(url);
      let data = await res.json();
      albumName = data.results.albummatches.album[0].name;
      albumArtist = data.results.albummatches.album[0].artist;
      albumImage = data.results.albummatches.album[0].image[1]["#text"];
      albumUrl = data.results.albummatches.album[0].url;

      /* Album Info */
      let albumInfoUrl = `http://ws.audioscrobbler.com/2.0/?method=album.getinfo&api_key=${import.meta.env.VITE_APP_LASTFM_KEY}&artist=${albumArtist}&album=${albumName}&format=json`;
      let albumInfoRes = await fetch(albumInfoUrl);
      let albumInfoData = await albumInfoRes.json();
      albumListeners = albumInfoData.album.listeners;
      albumPlaycount = albumInfoData.album.playcount;
    }
</script>

<div>
    <h2 class="text-4xl font-bold flex justify-center mt-40">SoundSafari</h2>
    <p class="transition-all duration-300 md:text-lg lg:text-lg text-white/60 flex justify-center mt-2">Get information about a music you want.</p>
    <div class="flex justify-center">
        <input bind:value={inputValue} class="transition-all duration-300 w-80 md:w-[30rem] lg:w-[30rem] p-2 px-5 mt-5 rounded-full bg-[#1d1f23] text-white/60" type="text" placeholder="Enter a music name!">
        {#if !inputValue}
        <button disabled class="transition-all duration-300 absolute mt-20 bg-[#1d1f23]/50 p-2 px-10 rounded text-sm text-white/20"><i class="fa-solid fa-magnifying-glass"></i> Search</button>
        {:else}
        <button on:click={search} class="transition-all duration-300 absolute mt-20 bg-[#1d1f23] p-2 px-10 rounded text-sm text-white/60 hover:text-white/100"><i class="fa-solid fa-magnifying-glass"></i> Search</button>
        {/if}
    </div>

    {#if albumName}
    <div>
      <h2 class="flex justify-center mt-40 text-xl font-bold">Album Info</h2>

      <div class="transition-all duration-300 relative ml-auto mr-auto left-0 right-0 mt-5 w-80 md:w-[35rem] lg:w-[35rem] h-44 md:h-40 lg:h-40 bg-[#1d1f23] rounded">
        <div class="absolute">
          <img src="{albumImage}" class="w-28 p-4 rounded-l" alt="albumImg">
        </div>
        <div class="relative">
          <h2 class="relative p-4 text-xs md:text-base lg:text-base mx-24 w-60 md:w-96 lg:w-96">{albumName}</h2>
          <p class="relative p-4 mx-24 text-xs -mt-6 w-40">{albumArtist}</p>
        </div>

        <ul class="block text-xs -mt-6 p-4 mx-[5.2rem] text-white/50">
          <li class="inline-block mx-2">
            <p class="relative"><i class="fa-solid fa-user mx-1"></i> {albumListeners} <span class="invisible md:visible lg:visible mx-0.5">listeners</span></p>
          </li>
          <li class="inline-block mx-2">
            <p class="relative -mt-3 md:mt-0 lg:mt-0"><i class="fa-solid fa-play mx-1"></i> {albumPlaycount} <span class="invisible md:visible lg:visible mx-0.5">playcount</span></p>
          </li>
        </ul>
        <a href="{albumUrl}" class="absolute py-2 px-8 ml-44 md:ml-[27rem] lg:ml-[27rem] text-xs top-[8.5rem] md:top-28 lg:top-28 bg-[#0B0D10] rounded-full"><i class="fa-solid fa-headphones"></i> Listen</a>
      </div>
    </div>
    {:else}
    <div class="flex justify-center mt-40">
      <h2 class="text-xl font-bold">No album found!</h2>
    </div>
    {/if}
</div>