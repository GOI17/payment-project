<script>
  import Card from "./Card.svelte";
  import Form from "./Form.svelte";

  let card = {
    number: "",
    owner: "",
    month: "",
    year: "",
	cvv: "",
	color: "",
	logo: ""
  };

  const cardNumber = e => (card.number = e.detail);
  const cardOwner = e => (card.owner = e.detail);
  const cardMonth = e => (card.month = e.detail);
  const cardYear = e => (card.year = e.detail);
  const cardCvv = e => (card.cvv = e.detail);
</script>

<div class="col-12 col-lg-8 mx-auto">
  <div class="row">
    <h2>Nuevo metodo de pago</h2>
  </div>
  <div class="row">
    <div class="col">
      <Card {card} />
    </div>
    <div class="col offset-0 offset-lg-1">
      <Form
        {card}
        on:cardnumber={cardNumber}
        on:cardowner={cardOwner}
        on:cardmonth={cardMonth}
        on:cardyear={cardYear}
        on:cardcvv={cardCvv} />
    </div>
  </div>
</div>
