<script>
  import "../app.postcss";
</script>

<slot />

<style>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');
  :global(html),
  :global(body) {
    margin: 0;
    padding: 0;
    justify-content: center;
    align-items: center;
    background-color: #0B0D10;
    color: whitesmoke;
    font-family: 'Montserrat', sans-serif;
  }
</style>