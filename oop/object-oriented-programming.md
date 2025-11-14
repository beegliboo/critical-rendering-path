# Object oriented programming

## Code Ki Asli Kahaani: OOP Ko Samajhne Ka Naya Nazariya

Bhai, main samajhta hoon teri problem. Tutorials dekhte ho, sab samajh aata hai, but jab khud code likhne baithte ho toh dimag blank. Kyunki koi tumhe _sochna_ nahi sikhata, sirf _syntax_ sikhaate hain.

Chalo, aaj main tumhe OOP ka **asli maqsad** dikhata hoon - architecture ki tarah, real duniya ki tarah.

***

### 🏗️ **Pehle Ye Samjho: Problem Kya Hai?**

Imagine karo tum ek **restaurant** chalate ho.

#### **Without OOP (Spaghetti Code):**

```javascript
// Sab kuch ek jagah - chaos!
let waiter1Name = "Rahul";
let waiter1Orders = [];
let waiter1Tips = 0;

let waiter2Name = "Priya";
let waiter2Orders = [];
let waiter2Tips = 0;

let chef1Name = "Singh";
let chef1Dishes = [];

// Order lena
function takeOrderWaiter1(dish) {
    waiter1Orders.push(dish);
    chef1Dishes.push(dish);
}

// Agar 50 waiters ho? 1000 lines of variables? 😱
```

Ye dekh ke hi darr lagta hai na? Isko **maintain** kaise karoge? Bug kahan hai ye kaise pata chalega?

***

### 🎯 **Class Ka Real Magic: Blueprint Banao**

Class ek **blueprint** hai - jaise architectural drawing. Ek baar bana liya, usse unlimited copies ban sakti hain.

#### **Real Duniya Ki Tarah Socho:**

```javascript
// BLUEPRINT: Har waiter mein kya hona chahiye?
class Waiter {
    constructor(name) {
        this.name = name;          // Identity
        this.orders = [];          // Responsibility
        this.tips = 0;             // State/Data
        this.isAvailable = true;   // Status
    }
    
    // BEHAVIOR: Waiter kya kar sakta hai?
    takeOrder(tableNumber, dish) {
        console.log(`${this.name}: Ji sir, ${dish} note kar liya`);
        
        this.orders.push({
            table: tableNumber,
            dish: dish,
            time: new Date()
        });
        
        return this; // Method chaining ke liye (aage dekhenge)
    }
    
    serveDish(tableNumber) {
        console.log(`${this.name}: Table ${tableNumber}, aapka order!`);
        this.orders = this.orders.filter(o => o.table !== tableNumber);
        return this;
    }
    
    receiveTip(amount) {
        this.tips += amount;
        console.log(`${this.name}: Dhanyavaad! Total tips: ₹${this.tips}`);
        return this;
    }
    
    // Helper method
    showMyOrders() {
        console.log(`${this.name} ke paas ${this.orders.length} orders pending hain`);
        return this.orders;
    }
}
```

#### **Ab Use Karo (Instantiation):**

```javascript
// Blueprint se real waiters banao
let rahul = new Waiter("Rahul");
let priya = new Waiter("Priya");
let amit = new Waiter("Amit");

// Har ek independent hai, apna kaam karta hai
rahul.takeOrder(5, "Butter Chicken")
     .takeOrder(7, "Dal Makhani")
     .receiveTip(50);

priya.takeOrder(3, "Paneer Tikka")
     .serveDish(3)
     .receiveTip(100);

console.log(rahul.tips);  // 50
console.log(priya.tips);  // 100
// Dono alag hain! 🎉
```

***

### 🧠 **Encapsulation: Apna Data Protect Karo**

Real life mein koi tumhare wallet mein seedha hath nahi daal sakta na? OOP mein bhi waisa hi.

```javascript
class BankAccount {
    // Private field (# se - ES2022)
    #balance = 0;
    #pin;
    
    constructor(ownerName, initialDeposit, pin) {
        this.ownerName = ownerName;
        this.#balance = initialDeposit;
        this.#pin = pin;
        this.transactionHistory = [];
    }
    
    // Public method - controlled access
    deposit(amount, enteredPin) {
        if (!this.#verifyPin(enteredPin)) {
            console.log("❌ Galat PIN!");
            return false;
        }
        
        if (amount <= 0) {
            console.log("❌ Amount positive hona chahiye");
            return false;
        }
        
        this.#balance += amount;
        this.#addTransaction('deposit', amount);
        console.log(`✅ ₹${amount} deposit ho gaya. Balance: ₹${this.#balance}`);
        return true;
    }
    
    withdraw(amount, enteredPin) {
        if (!this.#verifyPin(enteredPin)) {
            console.log("❌ Galat PIN!");
            return false;
        }
        
        if (amount > this.#balance) {
            console.log("❌ Itne paise nahi hain bhai!");
            return false;
        }
        
        this.#balance -= amount;
        this.#addTransaction('withdraw', amount);
        console.log(`✅ ₹${amount} nikaal liye. Remaining: ₹${this.#balance}`);
        return true;
    }
    
    // Getter - read-only access
    getBalance(enteredPin) {
        if (!this.#verifyPin(enteredPin)) {
            console.log("❌ PIN check karo pehle");
            return null;
        }
        return this.#balance;
    }
    
    // Private helper methods
    #verifyPin(enteredPin) {
        return enteredPin === this.#pin;
    }
    
    #addTransaction(type, amount) {
        this.transactionHistory.push({
            type,
            amount,
            date: new Date(),
            balanceAfter: this.#balance
        });
    }
}

// Use karo
let myAccount = new BankAccount("Arjun", 5000, "1234");

myAccount.deposit(2000, "1234");     // ✅ Works
myAccount.withdraw(1000, "1234");    // ✅ Works
myAccount.withdraw(1000, "5678");    // ❌ Galat PIN!

// Direct access nahi ho sakta - PROTECTED!
console.log(myAccount.#balance);     // ❌ SyntaxError
console.log(myAccount.getBalance("1234")); // ✅ This works
```

**Ye hai encapsulation ka fayda:** Koi bhi tumhara balance directly change nahi kar sakta. Sab kuch controlled way se hota hai.

***

### 🧬 **Inheritance: Code Reuse Ka Jaadu**

Agar tumhe different types ke employees chahiye, har baar sab kuch dobara likhoge?

```javascript
// PARENT CLASS - Common properties sab employees mein
class Employee {
    constructor(name, salary, id) {
        this.name = name;
        this.salary = salary;
        this.id = id;
        this.isPresent = false;
    }
    
    markAttendance() {
        this.isPresent = true;
        console.log(`${this.name} ne attendance laga di`);
    }
    
    calculatePay() {
        return this.salary;
    }
    
    showDetails() {
        return `${this.name} (ID: ${this.id}) - Salary: ₹${this.salary}`;
    }
}

// CHILD CLASS 1 - Chef ko extra skills chahiye
class Chef extends Employee {
    constructor(name, salary, id, specialty) {
        super(name, salary, id); // Parent ka constructor call karo
        this.specialty = specialty;
        this.dishesCooked = 0;
    }
    
    // Chef ki apni special ability
    cookDish(dishName) {
        this.dishesCooked++;
        console.log(`${this.name} ne ${dishName} bana diya! (${this.specialty} mein expert)`);
    }
    
    // Parent method ko OVERRIDE kar sakte ho
    calculatePay() {
        // Base salary + bonus per dish
        let bonus = this.dishesCooked * 50;
        return this.salary + bonus;
    }
}

// CHILD CLASS 2 - Manager ke alag powers
class Manager extends Employee {
    constructor(name, salary, id, department) {
        super(name, salary, id);
        this.department = department;
        this.teamMembers = [];
    }
    
    addTeamMember(employee) {
        this.teamMembers.push(employee);
        console.log(`${employee.name} ab ${this.name} ke team mein hai`);
    }
    
    conductMeeting() {
        console.log(`${this.name}: Meeting shuru karte hain...`);
        this.teamMembers.forEach(member => {
            console.log(`  - ${member.name} present?`, member.isPresent ? "✅" : "❌");
        });
    }
    
    calculatePay() {
        // Manager ka salary + team bonus
        let teamBonus = this.teamMembers.length * 500;
        return this.salary + teamBonus;
    }
}

// Ab use karo
let chef1 = new Chef("Ramu Kaka", 30000, "C001", "North Indian");
let manager1 = new Manager("Sharma Ji", 50000, "M001", "Kitchen");

chef1.markAttendance();           // Parent class ka method
chef1.cookDish("Butter Chicken"); // Apna method
chef1.cookDish("Dal Makhani");

manager1.markAttendance();
manager1.addTeamMember(chef1);
manager1.conductMeeting();

console.log(`${chef1.name} ki salary: ₹${chef1.calculatePay()}`);     // 30100
console.log(`${manager1.name} ki salary: ₹${manager1.calculatePay()}`); // 50500
```

**Dekha?** Chef aur Manager dono `markAttendance()` use kar sakte hain bina dobara code likhe. But dono apne hisaab se `calculatePay()` override kar sakte hain!

***

### 🎭 **Polymorphism: Ek Interface, Alag Behaviors**

Greek word hai - "poly" = many, "morph" = forms. Matlab ek hi cheez ke kai roop.

```javascript
// Base class
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    makeSound() {
        console.log(`${this.name} kuch awaaz kar raha hai...`);
    }
    
    move() {
        console.log(`${this.name} chal raha hai`);
    }
}

class Dog extends Animal {
    makeSound() {
        console.log(`${this.name}: Bhow bhow! 🐕`);
    }
    
    move() {
        console.log(`${this.name} daud raha hai`);
    }
}

class Cat extends Animal {
    makeSound() {
        console.log(`${this.name}: Meow meow! 🐈`);
    }
    
    move() {
        console.log(`${this.name} chup-chap chal rahi hai`);
    }
}

class Bird extends Animal {
    makeSound() {
        console.log(`${this.name}: Chirp chirp! 🐦`);
    }
    
    move() {
        console.log(`${this.name} ud rahi hai`);
    }
}

// Ab magic dekho - POLYMORPHISM
let animals = [
    new Dog("Tommy"),
    new Cat("Billu"),
    new Bird("Mitthu"),
    new Dog("Sheru")
];

// Same method call, but different behavior! 🎯
console.log("=== Sab awaaz karo ===");
animals.forEach(animal => {
    animal.makeSound(); // Polymorphism in action!
});

console.log("\n=== Sab chalo ===");
animals.forEach(animal => {
    animal.move();
});
```

**Output:**

```
=== Sab awaaz karo ===
Tommy: Bhow bhow! 🐕
Billu: Meow meow! 🐈
Mitthu: Chirp chirp! 🐦
Sheru: Bhow bhow! 🐕

=== Sab chalo ===
Tommy daud raha hai
Billu chup-chap chal rahi hai
Mitthu ud rahi hai
Sheru daud raha hai
```

**Ye power hai polymorphism ki:** Tum same method call karte ho, but har object apne hisaab se behave karta hai!

***

### 🏭 **Real-World Example: E-Commerce System**

Ab sab concepts ek saath implement karte hain:

```javascript
// 1. Base Product Class
class Product {
    static productCount = 0; // Static property - shared by all
    
    constructor(name, price, stock) {
        this.id = ++Product.productCount;
        this.name = name;
        this.price = price;
        this.stock = stock;
        this.reviews = [];
    }
    
    addReview(rating, comment) {
        this.reviews.push({ rating, comment, date: new Date() });
    }
    
    getAverageRating() {
        if (this.reviews.length === 0) return 0;
        let total = this.reviews.reduce((sum, r) => sum + r.rating, 0);
        return (total / this.reviews.length).toFixed(1);
    }
    
    updateStock(quantity) {
        this.stock += quantity;
        console.log(`${this.name} ka stock update: ${this.stock} items`);
    }
    
    isAvailable() {
        return this.stock > 0;
    }
    
    // Ye method child classes override karengi
    calculateFinalPrice() {
        return this.price;
    }
    
    display() {
        console.log(`
📦 ${this.name}
   Price: ₹${this.calculateFinalPrice()}
   Stock: ${this.stock}
   Rating: ${this.getAverageRating()}⭐
        `);
    }
}

// 2. Electronics - Extra warranty feature
class Electronics extends Product {
    constructor(name, price, stock, warrantyYears) {
        super(name, price, stock);
        this.warrantyYears = warrantyYears;
        this.brand = "";
    }
    
    calculateFinalPrice() {
        // Electronics pe 18% GST
        return Math.round(this.price * 1.18);
    }
    
    display() {
        super.display(); // Parent ka display call karo
        console.log(`   Warranty: ${this.warrantyYears} years`);
    }
}

// 3. Clothing - Size options
class Clothing extends Product {
    constructor(name, price, stock, sizes) {
        super(name, price, stock);
        this.sizes = sizes; // ['S', 'M', 'L', 'XL']
        this.material = "";
    }
    
    calculateFinalPrice() {
        // Clothing pe 12% GST
        return Math.round(this.price * 1.12);
    }
    
    isSizeAvailable(size) {
        return this.sizes.includes(size) && this.isAvailable();
    }
}

// 4. Shopping Cart
class ShoppingCart {
    constructor(customerName) {
        this.customerName = customerName;
        this.items = [];
    }
    
    addItem(product, quantity = 1) {
        if (!product.isAvailable()) {
            console.log(`❌ ${product.name} stock mein nahi hai!`);
            return false;
        }
        
        if (quantity > product.stock) {
            console.log(`❌ Sirf ${product.stock} items available hain`);
            return false;
        }
        
        // Check if already in cart
        let existing = this.items.find(item => item.product.id === product.id);
        
        if (existing) {
            existing.quantity += quantity;
        } else {
            this.items.push({ product, quantity });
        }
        
        console.log(`✅ ${product.name} x${quantity} cart mein add ho gaya`);
        return true;
    }
    
    removeItem(productId) {
        this.items = this.items.filter(item => item.product.id !== productId);
        console.log(`🗑️ Item removed`);
    }
    
    getTotalPrice() {
        return this.items.reduce((total, item) => {
            return total + (item.product.calculateFinalPrice() * item.quantity);
        }, 0);
    }
    
    checkout() {
        if (this.items.length === 0) {
            console.log("❌ Cart khali hai!");
            return false;
        }
        
        console.log(`\n🛒 ${this.customerName} ka Bill:`);
        console.log("=".repeat(40));
        
        this.items.forEach(item => {
            let price = item.product.calculateFinalPrice();
            console.log(`${item.product.name} x${item.quantity} = ₹${price * item.quantity}`);
            
            // Stock reduce karo
            item.product.updateStock(-item.quantity);
        });
        
        console.log("=".repeat(40));
        console.log(`Total: ₹${this.getTotalPrice()}`);
        console.log("\n✅ Order placed successfully!");
        
        // Cart empty karo
        this.items = [];
        return true;
    }
    
    viewCart() {
        console.log(`\n🛒 ${this.customerName} ki Cart:`);
        if (this.items.length === 0) {
            console.log("Cart khali hai");
            return;
        }
        
        this.items.forEach(item => {
            console.log(`- ${item.product.name} x${item.quantity} @ ₹${item.product.calculateFinalPrice()}`);
        });
        console.log(`Total: ₹${this.getTotalPrice()}`);
    }
}

// 5. Ab actually use karo!
console.log("🏪 Welcome to HamaaraDukaan.com\n");

// Products banao
let laptop = new Electronics("Dell Laptop", 45000, 5, 2);
laptop.brand = "Dell";
laptop.addReview(5, "Bahut badhiya laptop hai!");
laptop.addReview(4, "Accha hai but thoda mehenga");

let phone = new Electronics("iPhone 13", 60000, 3, 1);
phone.addReview(5, "Best phone ever!");

let tshirt = new Clothing("Nike T-Shirt", 1200, 10, ['S', 'M', 'L', 'XL']);
tshirt.material = "Cotton";
tshirt.addReview(4, "Comfortable hai");

// Products display karo
laptop.display();
phone.display();
tshirt.display();

// Customer shopping kare
let customer1 = new ShoppingCart("Rohit");

customer1.addItem(laptop, 1);
customer1.addItem(tshirt, 2);
customer1.addItem(phone, 1);

customer1.viewCart();
customer1.checkout();

// Stock check karo
console.log(`\n📊 Updated Stock:`);
console.log(`Laptop: ${laptop.stock} units`);
console.log(`T-Shirt: ${tshirt.stock} units`);
console.log(`Phone: ${phone.stock} units`);
```

***

### 🎯 **Ab Samjhe OOP Ka Real Power?**

#### **Benefits jo tune dekhe:**

1. **Organization** - Har cheez apni jagah pe hai
2. **Reusability** - Ek baar likh diya, baar baar use karo (inheritance)
3. **Scalability** - 10 products ho ya 10000, code same rahega
4. **Maintainability** - Bug fix karna easy ho gaya
5. **Real-World Mapping** - Code real duniya ki tarah sochta hai

#### **Key Takeaways:**

* **Class** = Blueprint (नक्शा)
* **Object** = Real instance (असली चीज़)
* **Encapsulation** = Data ko protect karo
* **Inheritance** = Parent se features lo
* **Polymorphism** = Same interface, different behavior
* **`this`** = Current object ko refer karta hai
* **`super`** = Parent class ko access karne ke liye

***

### 💡 **Practice Karo - Ye Banao:**

1. **Library Management System** - Books, Members, Borrowing
2. **Game Character System** - Warrior, Mage, Archer (inheritance + polymorphism)
3. **Social Media** - User, Post, Comment (encapsulation)
4. **Food Delivery** - Restaurant, Order, DeliveryPerson

Bhai, ab jaake code likho. Pehle kaam karta hua banao, baad mein perfect karna. **Architecture pehle, optimization baad mein!**

Koi doubt ho toh poochh lena. All the best! 🚀
