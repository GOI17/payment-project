<script>
  import { createEventDispatcher } from "svelte";

  let months = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12];

  let years = [2019, 2018, 2017, 2016, 2015, 2014];

  let addedCard = false;

  const checkmarkImage =
    "https://cdn3.iconfinder.com/data/icons/flat-actions-icons-9/792/Tick_Mark_Dark-512.png";

  const americanExpressImage =
    "https://cdn3.iconfinder.com/data/icons/payment-method-1/64/_American_Express-512.png";
  const masterCardImage =
    "https://upload.wikimedia.org/wikipedia/commons/thumb/2/2a/Mastercard-logo.svg/1280px-Mastercard-logo.svg.png";
  const visaImage =
    "https://seeklogo.com/images/V/VISA-logo-62D5B26FE1-seeklogo.com.png";

  export let card;

  let errors = [];

  const dispatch = createEventDispatcher();

  // data handlers
  const cardNumber = () => {
    formatCardNumber(card.number);
    dispatch("cardnumber", card.number);
    checkCardType();
  };
  const cardOwner = () => dispatch("cardowner", card.owner);
  const cardMonth = () => dispatch("cardmonth", card.month);
  const cardYear = () => dispatch("cardyear", card.year);
  const cardCvv = () => dispatch("cardcvv", card.cvv);
  const addCard = e => {
    e.preventDefault();
    if (formValidate()) return (addedCard = true);
  };
  //   validators
  const isNumber = event => {
    let value = String.fromCharCode(event.which);
    if (!/[0-9]/.test(value)) return event.preventDefault();
  };
  const isText = event => {
    let value = String.fromCharCode(event.which);
    if (!/^[a-zA-Z ]+$/.test(value)) return event.preventDefault();
  };
  const formatCardNumber = cardNumber => {
    if (card.number.length === 4) card.number = card.number + "-";
    if (card.number.length === 9) card.number = card.number + "-";
    if (card.number.length === 14) card.number = card.number + "-";
  };
  const formValidate = () => {
    errors = [];
    if (card.number.length < 18) {
      let error = {
        field: "number",
        message: "Debes ingresar un numero valido."
      };
      errors = [...errors, error];
    }
    if (card.owner.length === 0) {
      let error = {
        field: "owner",
        message: "Debes ingresar un nombre valido."
      };
      errors = [...errors, error];
    }
    if (card.month.length === 0 || card.year.length === 0) {
      let error = {
        field: "date",
        message: "Debes ingresar una fecha valida."
      };
      errors = [...errors, error];
    }
    if (card.cvv.length === 0) {
      let error = {
        field: "cvv",
        message: "Debes ingresar un cvv valido."
      };
      errors = [...errors, error];
    }
    return errors.length === 0 ? true : false;
  };
  const checkCardType = () => {
    card.number === "4"
      ? (card.logo = visaImage)((card.color = "#73b9fe"))
      : card.number === "51" ||
        card.number === "52" ||
        card.number === "53" ||
        card.number === "54" ||
        card.number === "55"
      ? (card.logo = masterCardImage)((card.color = "#ccb374"))
      : card.number === "3"
      ? (card.logo = americanExpressImage)((card.color = "rgb(152, 152, 152)"))
      : card.number.length === 0
      ? (card.logo = "")((card.color = "#ccb374"))
      : "";
  };
</script>

<style>
  input,
  select {
    display: block;
    width: 100%;
    height: calc(1.5em + 0.75rem + 2px);
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  }
  input:focus,
  select:focus {
    color: #495057;
    background-color: #fff;
    border-color: #80bdff;
    outline: 0;
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
  }
  select {
    width: 40%;
    margin-right: 5px;
  }
  label {
    font-size: 14pt;
  }
  .add-card {
    margin: 10px;
    background-color: rgb(37, 151, 75);
    color: #fff;
    font-size: 14pt;
    border: 0px;
    height: calc(1.5em + 0.75rem + 2px);
    width: 100%;
    border-radius: 5px;
  }
  .add-card:focus {
    border-color: #80bdff;
    outline: 0;
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
  }
  .add-card:hover {
    border: 0px;
    background-color: rgb(57, 168, 94);
  }
  .error-alert {
    background-color: #f8d7da;
    border-color: #f8d7da;
    border-radius: 5px;
    width: 100%;
    padding: 0.75rem 1.25rem;
    margin-top: 0.25rem;
    margin-bottom: 0.25rem;
    color: #721c24;
  }
  .added-card {
    color: #57ae49;
    font-size: 14pt;
  }
</style>

{#if addedCard}
  <div class="row">
    <div class="col-6 mx-auto">
      <img src={checkmarkImage} alt="cardAccepted" width="80" />
    </div>
  </div>
  <div class="row">
    <span class="added-card">El metodo de pago se agrego exitosamente</span>
  </div>
{:else}
  <form on:submit={addCard}>
    <!-- Card number -->
    <div class="row">
      <label for="card-number">Numero de tarjeta</label>
    </div>
    <div class="row">
      <input
        name="card-number"
        placeholder="XXXX - XXXX - XXXX - XXXX"
        minlength="0"
        maxlength="19"
        bind:value={card.number}
        on:input={cardNumber}
        on:keypress={isNumber}
        on:click={cardNumber}
        type="text" />
    </div>
    {#each errors as error}
      {#if error.field === 'number'}
        <div class="row">
          <div class="error-alert">
            <span>{error.message}</span>
          </div>
        </div>
      {/if}
    {/each}
    <!-- / Card number -->
    <!-- Card owner -->
    <div class="row">
      <label for="card-owner">Nombre del tarjetahabitante</label>
    </div>
    <div class="row">
      <input
        name="card-owner"
        placeholder="John Smith"
        bind:value={card.owner}
        on:input={cardOwner}
        on:keypress={isText}
        type="text" />
    </div>
    {#each errors as error}
      {#if error.field === 'owner'}
        <div class="row">
          <div class="error-alert">
            <span>{error.message}</span>
          </div>
        </div>
      {/if}
    {/each}
    <!-- / Card owner -->
    <!-- Card date -->
    <div class="row">
      <div class="col-8">
        <div class="row">
          <label for="card-date">Fecha de vencimiento</label>
        </div>
        <div class="row">
          <select
            name="card-month"
            bind:value={card.month}
            on:change={cardMonth}>
            <option value="0" hidden>MM</option>
            {#each months as month}
              <option value={month}>{month}</option>
            {/each}
          </select>
          <select name="card-year" bind:value={card.year} on:change={cardYear}>
            <option value="0" hidden>YYYY</option>
            {#each years as year}
              <option value={year}>{year}</option>
            {/each}
          </select>
          <!-- <div class="col-6">
        </div> -->
        </div>
      </div>
      <div class="col-4">
        <div class="row">
          <label for="card-cvv">Codigo CVV</label>
        </div>
        <div class="row">
          <input
            name="card-cvv"
            placeholder="XXX"
            bind:value={card.cvv}
            on:input={cardCvv}
            on:keypress={isNumber}
            minlength="0"
            maxlength="3"
            type="text" />
        </div>
      </div>
    </div>
    {#each errors as error}
      {#if error.field === 'date' || error.field === 'cvv'}
        <div class="row">
          <div class="error-alert">
            <span>{error.message}</span>
          </div>
        </div>
      {/if}
    {/each}
    <!-- / Card date -->
    <!-- Card submit -->
    <div class="row">
      <button type="submit" class="add-card">Agregar metodo de pago</button>
    </div>
    <!-- / Card submit -->
  </form>
{/if}
