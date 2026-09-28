# Import the necessary tools from Flask to run the website
from flask import Flask, render_template_string

# Create the Flask web application
app = Flask(__name__)

# ==============================================================================
# THIS IS THE HTML CODE FOR THE SKU GENERATOR PAGE (Home Page)
# I used basic HTML and CSS so it looks like a normal student project.
# ==============================================================================
sku_page_html = """
<!DOCTYPE html>
<html>
<head>
    <!-- Title of the webpage -->
    <title>1st Quarter Project | Toy Store SKU</title>
    <style>
        /* Basic styling to make it look neat but simple */
        body { font-family: Arial, sans-serif; background-color: #f0f0f0; margin: 0; padding: 20px; }
        header { background: #444; color: white; padding: 15px; text-align: center; }
        nav a { color: white; margin: 0 15px; text-decoration: none; font-size: 14px; }
        nav a:hover { text-decoration: underline; }
        
        /* Styling for the main white box */
        .box { max-width: 500px; margin: 20px auto; background: white; padding: 20px; border-radius: 5px; border: 1px solid #ccc; }
        
        /* Styling for the form inputs */
        input, select, button { width: 100%; padding: 10px; margin: 10px 0; box-sizing: border-box; }
        button { background-color: #4CAF50; color: white; border: none; cursor: pointer; font-size: 16px; }
        button:hover { background-color: #45a049; }
        
        .footer { text-align: center; margin-top: 30px; color: #777; font-size: 12px; }
    </style>
</head>
<body>

    <!-- Top Navigation Bar -->
    <header>
        <h2>Toy Store</h2>
        <nav>
            <a href="/">SKU Generator</a>
            <a href="/receipt">Receipt Generator</a>
        </nav>
    </header>

    <!-- Main Content Box for SKU Generation -->
    <div class="box">
        <h3>SKU Generator</h3>
        
        <!-- Dropdown menu for Category -->
        <label>Category</label>
        <select id="category">
            <option value="GIRLS">Girls Toys</option>
            <option value="BOYS">Boys Toys</option>
            <option value="GAMES">Board Games</option>
            <option value="PUZZLES">Puzzles</option>
        </select>
        
        <!-- Text box for Product Name -->
        <label>Product Name</label>
        <input type="text" id="prodName" placeholder="e.g. Teddy Bear">
        
        <!-- Number box for Stock -->
        <label>Stock Quantity</label>
        <input type="number" id="stockQty" placeholder="e.g. 50">
        
        <!-- Button that runs the JavaScript function below -->
        <button onclick="makeSKU()">Generate SKU</button>
        
        <!-- Area where the result will show up -->
        <p id="result" style="color: gray; text-align: center; margin-top: 20px;">
            Your generated SKU will appear here.
        </p>
    </div>

    <!-- Bottom Footer -->
    <div class="footer">
        SKU Generator - 1st Quarter Project
    </div>

    <!-- JavaScript to make the button actually work -->
    <script>
        function makeSKU() {
            // Get the values the user typed in
            var cat = document.getElementById('category').value;
            var name = document.getElementById('prodName').value;
            var stock = document.getElementById('stockQty').value;

            // Check if they left it blank
            if(name == "" || stock == "") {
                alert("Please fill in all the boxes!");
                return;
            }

            // Create the SKU code (First 3 letters of category + name + stock)
            var skuCode = cat.substring(0,3) + "-" + name.substring(0,3).toUpperCase() + "-" + stock;
            
            // Show the result on the screen
            document.getElementById('result').innerHTML = "<b>Your SKU is: " + skuCode + "</b>";
        }
    </script>

</body>
</html>
"""

# ==============================================================================
# THIS IS THE HTML CODE FOR THE RECEIPT / MENU PAGE
# ==============================================================================
receipt_page_html = """
<!DOCTYPE html>
<html>
<head>
    <!-- Title of the receipt page -->
    <title>Skills Test - 1st Quarter | Toy Store</title>
    <style>
        /* Same basic styling as the other page */
        body { font-family: Arial, sans-serif; background-color: #f0f0f0; margin: 0; padding: 20px; }
        header { background: #444; color: white; padding: 15px; text-align: center; }
        nav a { color: white; margin: 0 15px; text-decoration: none; font-size: 14px; }
        
        .box { max-width: 500px; margin: 20px auto; background: white; padding: 20px; border-radius: 5px; border: 1px solid #ccc; }
        
        /* Styling for the menu items list */
        .item { display: flex; justify-content: space-between; padding: 12px; border-bottom: 1px solid #eee; }
        .item:hover { background-color: #f9f9f9; }
        
        button { width: 100%; padding: 12px; background-color: #007bff; color: white; border: none; margin-top: 15px; cursor: pointer; font-size: 16px; }
        button:hover { background-color: #0056b3; }
        
        .footer { text-align: center; margin-top: 30px; color: #777; font-size: 12px; }
    </style>
</head>
<body>

    <!-- Top Navigation Bar -->
    <header>
        <h2>School Entrep Fair - Toy Store</h2>
        <nav>
            <a href="/">SKU Generator</a>
            <a href="/receipt">Receipt Generator</a>
        </nav>
    </header>

    <!-- Main Content Box for the Menu -->
    <div class="box">
        <h3>Toy Store Menu</h3>
        
        <!-- List of toys and their prices (Changed from coffee to toys) -->
        <div class="item"><span>Teddy Bear</span><span>₱199</span></div>
        <div class="item"><span>Robot Transformer</span><span>₱299</span></div>
        <div class="item"><span>Remote Control Car</span><span>₱350</span></div>
        <div class="item"><span>Princess Doll</span><span>₱250</span></div>
        <div class="item"><span>Building Blocks Set</span><span>₱450</span></div>

        <!-- Button to create order -->
        <button onclick="alert('Order created! (This is just a demo)')">Create Order</button>
        
        <!-- Placeholder text for the receipt area -->
        <p style="color: gray; text-align: center; margin-top: 20px; border: 1px dashed #ccc; padding: 15px;">
            Your order summary will appear here.<br>
            Select your items and click "Create Order"
        </p>
    </div>

    <!-- Bottom Footer -->
    <div class="footer">
        Receipt Generator - 1st Quarter Project
    </div>

</body>
</html>
"""

# ==============================================================================
# PYTHON ROUTES (This tells the website which page to show)
# ==============================================================================

# When the user goes to the home page (/) or /sku, show the SKU page
@app.route('/')
@app.route('/sku')
def home_page():
    return render_template_string(sku_page_html)

# When the user goes to /receipt, show the Receipt/Menu page
@app.route('/receipt')
def receipt_page():
    return render_template_string(receipt_page_html)

# ==============================================================================
# RUN THE WEBSITE
# ==============================================================================
if __name__ == '__main__':
    print("Starting the Toy Store website...")
    print("Open your browser and go to: http://127.0.0.1:5000")
    # debug=True means if you change the code, the website updates automatically
    app.run(debug=True)
