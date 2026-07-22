from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

# Menu data
MENU = [
    {"id": 1, "name": "Milk Tea", "price": 3.50, "category": "Tea"},
    {"id": 2, "name": "Black Tea", "price": 2.50, "category": "Tea"},
    {"id": 3, "name": "Coffee", "price": 3.00, "category": "Coffee"},
    {"id": 4, "name": "Latte Coffee", "price": 4.50, "category": "Coffee"},
    {"id": 5, "name": "Green Tea", "price": 3.00, "category": "Tea"},
    {"id": 6, "name": "Cappuccino", "price": 4.00, "category": "Coffee"},
    {"id": 7, "name": "Hot Chocolate", "price": 3.75, "category": "Beverage"},
]

@app.route('/')
def index():
    return render_template('coffee_shop.html', menu=MENU)

@app.route('/api/order', methods=['POST'])
def process_order():
    data = request.get_json()
    order_items = data.get('order', [])
    
    # Calculate total
    total = 0
    for item in order_items:
        total += item['price'] * item['quantity']
    
    # Log order
    print("\n" + "="*50)
    print("📝 NEW ORDER RECEIVED".center(50))
    print("="*50)
    for item in order_items:
        print(f"{item['name']:<20} x{item['quantity']:<5} ${(item['price'] * item['quantity']):.2f}")
    print("-"*50)
    print(f"{'TOTAL':<20} {'':<5} ${total:.2f}")
    print("="*50)
    
    return jsonify({
        "status": "success",
        "message": "Order placed successfully!",
        "total": total,
        "items": order_items
    })

if __name__ == '__main__':
    app.run(debug=True, port=5000)
