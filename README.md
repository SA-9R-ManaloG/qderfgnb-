<!DOCTYPE html>
<html>
<head>
    <title>1st Quarter Project | Toy Store</title>
    <!-- These lines load PyScript so Python can run directly in the browser -->
    <link rel="stylesheet" href="https://pyscript.net/latest/pyscript.css" />
    <script defer src="https://pyscript.net/latest/pyscript.js"></script>
    
    <style>
        /* Simple CSS to make it look neat but like a student project */
        body { font-family: Arial, sans-serif; background-color: #f4f4f4; margin: 0; padding: 20px; }
        header { background: #333; color: white; padding: 15px; text-align: center; }
        nav a { color: white; margin: 0 15px; text-decoration: none; }
        
        .container { max-width: 600px; margin: 20px auto; background: white; padding: 20px; border-radius: 5px; border: 1px solid #ddd; }
        .section { margin-bottom: 30px; padding-bottom: 20px; border-bottom: 2px solid #eee; }
        
        input, select, button { padding: 10px; margin: 5px 0; width: 100%; box-sizing: border-box; }
        button { background-color: #28a745; color: white; border: none; cursor: pointer; border-radius: 4px; font-size: 16px; }
        button:hover { background-color: #218838; }
        
        .toy-row { display: flex; justify-content: space-between; padding: 10px; border-bottom: 1px solid #eee; align-items: center; }
        .toy-row input { width: auto; margin-right: 10px; }
        
        .footer { text-align: center; margin-top: 20px; color: #777; font-size: 12px; }
    </style>
</head>
<body>

    <header>
        <h2>1st Quarter Project | Toy Store</h2>
        <nav>
            <a href="#">Home</a>
            <a href="#">SKU Generator</a>
            <a href="#">Receipt Generator</a>
        </nav>
    </header>

    <div class="container">
        
        <!-- SKU GENERATOR SECTION -->
        <div class="section">
            <h3>SKU Generator</h3>
            <label>Category:</label>
            <select id="category">
                <option value="GIRLS">Girls Toys</option>
                <option value="BOYS">Boys Toys</option>
                <option value="GAMES">Board Games</option>
            </select>
            
            <label>Product Name:</label>
            <input type="text" id="product_name" placeholder="e.g. Teddy">
            
            <label>Stock Quantity:</label>
            <input type="number" id="quantity" placeholder="e.g. 50">
            
            <!-- This button runs the Python function make_sku -->
            <button py-click="make_sku">Generate SKU</button>
            
            <div id="sku_box" style="margin-top: 15px; color: gray; text-align: center;">
                Your generated SKU will appear here.
            </div>
        </div>

        <!-- RECEIPT / MENU SECTION -->
        <div class="section" style="border-bottom: none;">
            <h3>Toy Store Menu</h3>
            <p>Select the toys you want to buy:</p>
            
            <!-- Checkboxes for the toys. The value is the price in Peso -->
            <div class="toy-row">
                <label><input type="checkbox" id="item1" value="199"> Teddy Bear</label>
                <span>₱199</span>
            </div>
            <div class="toy-row">
                <label><input type="checkbox" id="item2" value="299"> Robot Transformer</label>
                <span>₱299</span>
            </div>
            <div class="toy-row">
                <label><input type="checkbox" id="item3" value="150"> Race Car</label>
                <span>₱150</span>
            </div>
            <div class="toy-row">
                <label><input type="checkbox" id="item4" value="450"> Doll House</label>
                <span>₱450</span>
            </div>
            <div class="toy-row">
                <label><input type="checkbox" id="item5" value="250"> Building Blocks</label>
                <span>₱250</span>
            </div>

            <br>
            <!-- This button runs the Python function make_receipt -->
            <button py-click="make_receipt">Create Order & Receipt</button>
            
            <div id="receipt_area" style="margin-top: 20px; border: 1px dashed #ccc; padding: 15px; color: gray; text-align: center;">
                Your order summary will appear here.<br>Select your items and click "Create Order"
            </div>
        </div>

    </div>

    <div class="footer">
        SKU & Receipt Generator - 1st Quarter Project
    </div>

    <!-- INLINE PYTHON CODE -->
    <py-script>
        # 1st Quarter Project - Toy Store
        # This script handles the SKU generation and Receipt calculation using PyScript

        from pyscript import document

        # Function to generate the unique SKU code
        def make_sku(event):
            # Clear the previous result first
            document.getElementById('sku_box').innerHTML = ""
            
            # Get the values from the HTML inputs
            cat = document.getElementById('category').value
            p_name = document.getElementById('product_name').value
            qty = document.getElementById('quantity').value
            
            # Check if the user actually typed something
            if p_name == "" or qty == "":
                document.getElementById('sku_box').innerHTML = "<span style='color:red;'>Please fill in all fields!</span>"
                return

            # Create the code: First 3 letters of category + first 3 letters of name + quantity
            final_sku = cat[:3].upper() + "-" + p_name[:3].upper() + "-" + str(qty)
            
            # Display the result on the webpage
            document.getElementById('sku_box').innerHTML = f"<h3 style='color:blue;'>Generated SKU: {final_sku}</h3>"


        # Function to calculate the total and make the receipt
        def make_receipt(event):
            # Get the toy elements from the HTML by their IDs
            toy1 = document.getElementById("item1")
            toy2 = document.getElementById("item2")
            toy3 = document.getElementById("item3")
            toy4 = document.getElementById("item4")
            toy5 = document.getElementById("item5")
            
            # Calculate subtotal
            # In Python, True is 1 and False is 0, so multiplying by .checked works perfectly
            subtotal = (float(toy1.value) * toy1.checked + 
                        float(toy2.value) * toy2.checked + 
                        float(toy3.value) * toy3.checked + 
                        float(toy4.value) * toy4.checked + 
                        float(toy5.value) * toy5.checked)
                        
            # Calculate 12% Tax
            tax_rate = 0.12 
            tax = subtotal * tax_rate
            total_price = subtotal + tax
            
            # Check if they bought anything
            if subtotal == 0:
                document.getElementById("receipt_area").innerHTML = "<span style='color:red;'>Please select at least one toy!</span>"
                return

            # Create the HTML for the receipt
            receipt_html = f"""
                <h3 style='text-align:center;'>--- Official Receipt ---</h3>
                <p>Subtotal: {subtotal:.2f}</p>
                <p>Tax (12%): ₱{tax:.2f}</p>
                <h3 style='color:green; text-align:center;'>Total: {total_price:.2f}</h3>
            """
            
            # Put the receipt into the div on the webpage
            document.getElementById("receipt_area").innerHTML = receipt_html
    </py-script>

</body>
</html>
