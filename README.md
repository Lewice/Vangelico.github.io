
<html>
<head>
  <title>Menu Calculator</title>
    <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 250vh;
      text-align: center;
    }
	.total-box {
		display: flex;
		justify-content: center; /* Center horizontally */
		align-items: center; /* Center vertically */
		margin-top: 20px;
	}}
	
	 .calculate-button {
      width: 150px; /* Adjust the desired width */
      height: 50px; /* Adjust the desired height */
    }
	
	.submit-button {
      width: 150px; /* Adjust the desired width */
      height: 40px; /* Adjust the desired height */
    }
	
	.reset-button {
      width: 150px; /* Adjust the desired width */
      height: 30px; /* Adjust the desired height */
    }
    
    h1 {
      margin-bottom: 20px;
    }
    
    h2 {
      margin-top: 20px;
    }
	
	h3 {
      border: 1px solid black; /* Adjust the border style as needed */
      padding: 5px; /* Add padding to create space around the heading */
    }
    
    .menu-items {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 10px;
      margin-bottom: 10px;
    }
    
    .menu-items div {
      display: flex;
      align-items: center;
    }
    
    .total-box {
      display: flex;
      justify-content: flex-end;
      align-self: Center;
      margin-top: 20px;
    }
    
    .button-container {
      display: flex;
      gap: 10px;
      margin-top: 20px;
    }
	
	.menu-items div img {
      width: 50px; /* Adjust the desired width */
      height: 50px; /* Adjust the desired height */
      margin-left: 10px; /* Add margin as per your preference */
    }
    {
    button {
      margin-top: 20px;
	}}}}}}
  </style>
  <script>
    function calculateTotal() {
      var total = 0;
      var checkboxes = document.querySelectorAll('input[type="checkbox"]:checked');

      checkboxes.forEach(function (checkbox) {
        var quantityInput = checkbox.parentNode.querySelector('input[type="number"]');
        var quantity = parseInt(quantityInput.value);
        var price = parseFloat(checkbox.value);

        if (checkbox.value === '-5%') {
          var itemPrice = price * quantity * 0.05;
          total += price * quantity - itemPrice;
        } else if (checkbox.value === '-10%') {
          var itemPrice = price * quantity * 0.10;
          total += price * quantity - itemPrice;
        } else {
          total += price * quantity;
        }
      });

      var totalElement = document.getElementById('total');
      totalElement.textContent = total.toFixed(2);

      var discountTotalElement = document.getElementById('discount-total');
      var discount5Percent = total * 0.05;
      var discount10Percent = total * 0.10;

      discountTotalElement.innerHTML = `
        5% Commission: $${discount5Percent.toFixed(2)}<br>
        10% Commission: $${discount10Percent.toFixed(2)}
      `;
    }


    
    function submitOrder() {
  var name = document.getElementById('name').value;
  if (name.trim() === '') {
    alert('Please enter a name.');
    return;
  }

  // Collect selected items and their quantities
  var selectedItems = [];
  var checkboxes = document.querySelectorAll('input[type="checkbox"]:checked');
  checkboxes.forEach(function (checkbox) {
    var itemName = checkbox.nextElementSibling.textContent;
    var quantityInput = checkbox.parentNode.querySelector('input[type="number"]');
    var quantity = parseInt(quantityInput.value);
    var price = parseFloat(checkbox.value);
    selectedItems.push({ name: itemName, quantity: quantity, price: price });
  });

  var total = 0;
  var discount5PercentTotal = 0;
  var discount10PercentTotal = 0;

  selectedItems.forEach(function (item) {
    if (item.price < 0) {
      var discountPercentage = Math.abs(item.price);
      var itemDiscount = item.price < -5 ? item.price * item.quantity * 0.05 : item.price * item.quantity * 0.10;
      if (item.price < -5) {
        discount5PercentTotal += itemDiscount;
      } else {
        discount10PercentTotal += itemDiscount;
      }
      total += item.price * item.quantity;
    } else {
      total += item.price * item.quantity;
    }
  });

  var commission5Percent = (total * 0.05).toFixed(2);
  var commission10Percent = (total * 0.10).toFixed(2);

  alert('Order submitted!');

  var discordWebhookURL = 'https://discord.com/api/webhooks/1115717872002551860/QeP0olu8qsHp7pE0XxHqB7dTK2c9i7hqA1vX4LB8ogLAw14NBj08zLN--8K9hvHeB0hO';

  var xhr = new XMLHttpRequest();
  xhr.open('POST', discordWebhookURL, true);
  xhr.setRequestHeader('Content-Type', 'application/json');

  var message = {
    content: 'New order!',
    embeds: [{
      title: 'Order Details',
      fields: [
        {
          name: 'Name',
          value: name,
          inline: true
        },
        {
          name: 'Total',
          value: '$' + total.toFixed(2),
          inline: true
        },
        {
          name: '5% Commission',
          value: '$' + commission5Percent,
          inline: true
        },
        {
          name: '10% Commission',
          value: '$' + commission10Percent,
          inline: true
        },
        {
          name: 'Ordered Items',
          value: selectedItems.map(item => `${item.name} x${item.quantity} - $${(item.price * item.quantity).toFixed(2)}`).join('\n'),
          inline: false
        }
      ]
    }]
  };

  xhr.send(JSON.stringify(message));
}

function resetCalculator() {
  var checkboxes = document.querySelectorAll('input[type="checkbox"]');
  var quantityInputs = document.querySelectorAll('input[type="number"]');
  
  checkboxes.forEach(function(checkbox) {
    checkbox.checked = false;
  });
  
  quantityInputs.forEach(function(quantityInput) {
    quantityInput.value = 1;
  });
  
  document.getElementById('total').textContent = '0.00';
}
	function submitAndReset() {
	submitOrder();
	resetCalculator();
}

  </script>
</head>
<body>
	
<div style="margin-bottom: 25px;"></div>
 
<body style="background-color:Grey;">
	<img src="Vangelico.png" alt="Company Logo!">
  <h1>Menu Calculator</h1>
  
  <h2>Menu Items</h2>

  <div style="margin-bottom: 10px;"></div>
  
  <h3>Engine Upgrades</h3>

  <div style="margin-bottom: 10px;"></div>
  
  <div>
    <input type="checkbox" id="uwueats" value="1000$">
    <label for="Velmachoice">Opal - 150$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="3000$">
    <label for="Davechoice">Citrine - 300$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="8000$">
    <label for="Davechoice">Amethyst - 300$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="3000$">
    <label for="Davechoice">Sapphire  - 350$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="8000$">
    <label for="Davechoice">Ruby  - 450$</label>
    <input type="number" value="1" min="1">
  </div>
  <div>
    <input type="checkbox" id="Davechoice" value="3000$">
    <label for="Davechoice">Emerald - 600$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="8000$">
    <label for="Davechoice">Diamond - 1100$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="3000$">
    <label for="Davechoice">Gold  - 300$</label>
    <input type="number" value="1" min="1">
  </div>
  
  <div>
    <input type="checkbox" id="Davechoice" value="8000$">
    <label for="Davechoice">Silver  - 125$</label>
    <input type="number" value="1" min="1">
  </div>
  

  <div style="margin-bottom: 10px;"></div>
  
  

</div>

<div style="margin-bottom: 100px;"></div>

<div>
    <label for="name">Employees 's Name:</label>
    <input type="text" id="name">
  </div>
  

<div style="margin-bottom: 25px;"></div>
 
<div class="total-box">
    <span>Total: $</span>
    <span id="total">0.00</span>
  </div>

  <div class="total-box">
    <span>Commission:</span>
    <div id="discount-total"></div>
  </div>




 
  
  
  <div style="margin-bottom: 45px;"></div>
  

  <button class="calculate-button" onclick="calculateTotal()">Calculate Total</button>
  <button class="submit-button" onclick="submitAndReset()">Submit Order</button>
  <button class="reset-button" onclick="resetCalculator()">Reset</button>

  <div style="margin-bottom: 10px;"></div>
