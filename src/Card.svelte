<script>
  let chipImage =
    "https://pngimage.net/wp-content/uploads/2018/05/chip-card-png-4-203x200.png";
  export let card;
</script>

<style>
  .card-design {
    background-color: #ccb374;
    border-radius: 5px;
    width: 400px;
    height: 220px;
  }
  img {
    margin: 20px 0px 15px 30px;
  }
  .card-number {
    font-size: 14pt;
  }
  .card-owner {
    font-size: 12pt;
  }
  .card-date {
    font-size: 9pt;
  }
  .card-logo {
    text-align: right;
    margin: 20px 0px 0px -40px;
  }
</style>

<div class="card card-design" style="background-color:{card.color}">
  <div class="row">
    <div class="col-2">
      <img src={chipImage} alt="chip" width="80" />
    </div>
    {#if card.logo !== ''}
      <div class="col-10 card-logo">
        <img src={card.logo} alt="logo" width="70" />
      </div>
    {/if}
  </div>
  <div class="row">
    <div class="col-10 offset-1">
      <span class="card-number">{card.number}</span>
    </div>
    <div class="col-10 offset-1">
      <span class="card-owner">{card.owner}</span>
    </div>
    <div class="col-10 offset-1">
      {#if card.month.length !== 0}
        <span class="card-date">{card.month}/{card.year}</span>
      {/if}
    </div>
  </div>
</div>
