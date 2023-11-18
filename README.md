<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Menu Calculator and Form Submission</title>
  <script src="https://code.jquery.com/jquery-3.4.1.js" integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU=" crossorigin="anonymous"></script>
  <style>
    body, h2, form, label, p, button, select, input {
      font-size: 8;
      margin-right: 10px; /* Add a margin for spacing */
    }

    label {
		display: block;
		margin-bottom: 5px;
	}

    body, h2, form {
      text-align: center;
    }

    p {
      text-align: center;
      margin: 0; /* Remove default margin for <p> */
    }

    button {
      margin-top: 10px;
    }
	body, h2 {
	font-weight: bold;
	}
	body {
	background-color: grey;
	}
  </style>
  <script>
    function calculateTotals() {
  let total = 0;

  // Calculate total from selected items
  const menuItems = document.querySelectorAll('.menu-item:checked');
  menuItems.forEach(item => {
    const price = parseFloat(item.dataset.price);
    const quantity = parseInt(item.nextElementSibling.value);

    if (!isNaN(price) && !isNaN(quantity) && quantity > 0) {
      // Exclude "Mystery Box" from discounts
      if (item.classList.contains('exclude-discount')) {
        total += price * quantity;
      } else {
        total += price * quantity * (1 - ($("#discount").val() / 100));
      }
    }
  });

  // Change commission rate from 5% to 10%
  const commission = total * 0.10;

  document.getElementById('total').innerText = total.toFixed(2);
  document.getElementById('commission').innerText = commission.toFixed(2);
}

    function SubForm() {
  // Check if the employee name is provided
  const employeeName = $("#employeeName").val();
  if (employeeName.trim() === "") {
    alert("Employee Name is required!");
    return;
  }

  // Get selected menu items and quantities
  const orderedItems = [];
  const menuItems = document.querySelectorAll('.menu-item:checked');
  menuItems.forEach(item => {
    const itemName = item.parentNode.textContent.trim();
    const price = parseFloat(item.dataset.price);
    const quantity = parseInt(item.nextElementSibling.value);

    if (!isNaN(price) && !isNaN(quantity) && quantity > 0) {
      orderedItems.push({
        name: itemName,
        price: price,
        quantity: quantity
      });
    }
  });

  // Calculate total and commission
  const total = parseFloat($("#total").text());
  const commission = parseFloat($("#commission").text());
  const discount = parseFloat($("#discount").val());

  // Prepare data for API submission
  const formData = {
    "Employee Name": employeeName,
    "Total": total.toFixed(2),
    "Commission": commission.toFixed(2),
    "Items Ordered": JSON.stringify(orderedItems),
    "Discount Applied": discount
  };

  // Form Submission Logic for Spreadsheet
  $.ajax({
    url: "https://api.apispreadsheets.com/data/5S6sd3ULAqiqAmBZ/",
    type: "post",
    data: formData,
    headers: {
      accessKey: "a5dd457651e43fbb8e24c645829b7c1f",
      secretKey: "d310a5b9fef212f045462e9be7a09743",
      "Content-Type": "application/x-www-form-urlencoded",
    },
    success: function () {
      alert("Form Data Submitted to Spreadsheet and Discord :)");
      resetForm();
    },
    error: function () {
      alert("There was an error :(");
    }
  });

  // Prepare data for Discord webhook
  const discordData = {
    username: "Vangelico Employee Recipts",
    content: `New order submitted by ${employeeName}`,
    embeds: [{
      title: "Order Details",
      fields: [
        { name: "Employee Name", value: employeeName, inline: true },
        { name: "Total", value: `$${total.toFixed(2)}`, inline: true },
        { name: "Commission", value: `$${commission.toFixed(2)}`, inline: true },
        { name: "Discount Applied", value: `${discount}%`, inline: true },
        { name: "Items Ordered", value: orderedItems.map(item => `${item.quantity}x ${item.name}`).join('\n') }
      ],
      color: 0x00ff00 // You can customize the color
    }]
  };

  // Form Submission Logic for Discord webhook
  $.ajax({
    url: "https://discord.com/api/webhooks/1154112405585023046/zG7g42xUk39GGS-Uk7UrWJBwpqc1jtnM6Ig2QUPdtf8huGz-8nxX8lOY64GMtbZeBUx8", // Replace with your Discord webhook URL
    type: "post",
    contentType: "application/json",
    data: JSON.stringify(discordData),
    success: function () {
      // Do nothing specific for Discord success
    },
    error: function () {
      console.error("Error sending data to Discord :(");
    }
  });

  // Reset checkboxes and quantity inputs
  $('.menu-item').prop('checked', false);
  $('.quantity').val(1);

  // Reset totals
  document.getElementById('total').innerText = '';
  document.getElementById('commission').innerText = '';
  // Reset discount dropdown to default
  $("#discount").val("0");
}

    function resetForm() {
      // Reset checkboxes and quantity inputs
      $('.menu-item').prop('checked', false);
      $('.quantity').val(1);

      // Reset totals
      document.getElementById('total').innerText = '';
      document.getElementById('commission').innerText = '';
      // Reset discount dropdown to default
      $("#discount").val("0");
    }
  </script>
</head>
<body>

  <h3>Gems</h3>
    <label>
      <input type="checkbox" class="menu-item" data-price="150"> Opal - 150$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="300"> Citrine - 300$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="300"> Amethyst - 300$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1000"> Sapphire  - 350$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="3000"> Ruby  - 450$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="5000"> Emerald - 600$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1000"> Diamond - 1100$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="4000"> Gold  - 300$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="7000"> Silver  - 125$
      <input type="number" class="quantity" value="1" min="1">
    </label>

	
	<h3>Materials</h3>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Iron  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Heavy Glue  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Rubber  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Plastic  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Copper Wire  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
	<label>
      <input type="checkbox" class="menu-item" data-price="1"> Electronic Parts  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Glue  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
	<label>
      <input type="checkbox" class="menu-item" data-price="1"> Salavaged parts  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
    <label>
      <input type="checkbox" class="menu-item" data-price="1"> Car Polish(1-2 days) - $1000
      <input type="number" class="quantity" value="1" min="1">
    </label>
	<label>
      <input type="checkbox" class="menu-item" data-price="1"> Scrap Metal  - 1$
      <input type="number" class="quantity" value="1" min="1">
    </label>
	
	
	
	
	
	
	
	<div style="margin-bottom: 30px;"></div>
	
	<label for="discount">Select Discount:</label>
    <select id="discount" onchange="calculateTotals()">
      <option value="0">No Discount</option>
      
    </select>
	
	<div style="margin-bottom: 30px;"></div>
	


    <label for="employeeName">Employee Name:</label>
    <input type="text" id="employeeName" required>
	
	<div style="margin-bottom: 30px;"></div>
	
	

    <p>Total: $<span id="total"></span></p>
    <p>Commission (10%): $<span id="commission"></span></p>
	
	<div style="margin-bottom: 30px;"></div>

    <button type="button" onclick="calculateTotals()">Calculate</button>
    <button type="button" onclick="SubForm()">Submit</button>
    <button type="button" onclick="resetForm()">Reset</button>
  </form>

</body>
</html>
