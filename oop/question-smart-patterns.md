# Question Smart Patterns

## 🎯 Real 1-Hour OOP Problems (Problem Statement Only)

Bhai, **sirf problem batata hoon**. Solution tum khud nikalna.

***

### 1️⃣ **Smart Cache System** ⏱️ 45-60 min

#### Problem:

Tumhare app mein API calls bahut zyada ho rahe hain. Same data baar baar fetch ho raha hai - waste of bandwidth aur slow performance.

#### Requirements:

1. **Time-based expiry** - 5 minute purana data automatically delete ho jaye
2. **Size limit** - Maximum 100 items store ho sakein
3. **LRU Eviction** - Jab cache full ho, toh sabse purana unused item nikal do
4. **Stats tracking** - Kitne hits, misses, evictions hue ye track karo
5. **Memory cleanup** - Expired items automatically clean ho jayen

#### Real-world scenario:

```
User ne product search kiya → API call
2 seconds baad wahi product search → Cache se instantly return (no API call!)
100 different searches ho gaye → 101st search pe sabse purana wala nikal do
5 minute baad → Same search pe fresh API call (expired cache)
```

#### Think about:

* Kaunse classes banoge?
* Entry ka expiry kaise track karoge?
* LRU ko kaise implement karoge? (Map? Array? Linked List?)
* Memory leaks se kaise bachoge?

***

### 2️⃣ **Retry Manager with Exponential Backoff** ⏱️ 45 min

#### Problem:

API calls kabhi-kabhi fail ho jati hain (network issues, server down). Agar ek baar fail hui toh user ko error dikha dete ho. Bad UX!

#### Requirements:

1. **Auto retry** - Failed request automatically retry karo
2. **Exponential backoff** - Pehli baar 1s wait, phir 2s, phir 4s, phir 8s...
3. **Max retries** - 5 baar try karo, uske baad give up
4. **Selective retry** - Sirf 500, 502, 503 errors pe retry karo (404 pe nahi!)
5. **Callbacks** - Har retry attempt pe user ko inform karo

#### Real-world scenario:

```
API call fails (500 error) → Wait 1 second → Retry
Still fails → Wait 2 seconds → Retry  
Still fails → Wait 4 seconds → Retry
Success! → Return data to user

Different error (404) → Don't retry, return error immediately
```

#### Think about:

* Promise handling kaise karoge?
* Recursive calls ya loops?
* Timeout kaise manage karoge?
* Different error types ka logic kahan rahega?

***

### 3️⃣ **Event Bus / PubSub System** ⏱️ 40 min

#### Problem:

Tumhare app mein multiple components hain jo ek dusre se baat karna chahte hain. But directly coupling nahi chahiye (tight coupling = bad architecture).

#### Requirements:

1. **Subscribe** - Koi bhi component kisi event ko listen kar sake
2. **Publish** - Koi bhi component event trigger kar sake
3. **Unsubscribe** - Memory leaks avoid karne ke liye cleanup
4. **Wildcard support** - `user.*` subscribe karo toh `user.login`, `user.logout` dono sun sako
5. **Once listeners** - Ek baar sun ke automatically unsubscribe

#### Real-world scenario:

```
User login hua → 'user.login' event fire
↓
- Analytics component: "Track login"
- Navbar component: "Show user name"  
- Cart component: "Load user's cart"
- Notification component: "Show welcome message"

Sab independently listen kar rahe hain!
```

#### Think about:

* Events ko store kaise karoge? (Map? Object?)
* Multiple listeners ko kaise handle karoge?
* Unsubscribe kaise efficiently karoge?
* Wildcard matching kaise implement karoge?

***

### 4️⃣ **Form Validator with Rules** ⏱️ 50 min

#### Problem:

Har form mein validation manually likhte ho. Code repeat ho raha hai. Reusable validator chahiye.

#### Requirements:

1. **Chainable rules** - `validator.required().email().minLength(8)`
2. **Custom rules** - User apne rules add kar sake
3. **Custom error messages** - Har rule ka message customize ho
4. **Async validation** - API se check karo (username available hai?)
5. **Multiple fields** - Ek form mein kai fields validate karo

#### Real-world scenario:

```
Email field:
- Required hai? ✓
- Valid email format? ✓
- Already registered? (API check) ✓

Password field:
- Required? ✓
- Min 8 characters? ✓
- At least 1 uppercase? ✓
- At least 1 number? ✓

Confirm Password:
- Matches password? ✓
```

#### Think about:

* Rules ko chain kaise karoge?
* Async validation ko sync ke saath kaise mix karoge?
* Error messages ko kaise store/display karoge?
* Field dependencies kaise handle karoge? (confirm password depends on password)

***

### 5️⃣ **State Machine for UI Flow** ⏱️ 50 min

#### Problem:

Complex UI flows mein state management mess ho jata hai. Checkout process, multi-step forms, game states - ye sab finite states hain but code spaghetti ban jata hai.

#### Requirements:

1. **Defined states** - `IDLE`, `LOADING`, `SUCCESS`, `ERROR`
2. **Valid transitions** - `IDLE` → `LOADING` allowed, but `ERROR` → `SUCCESS` not allowed
3. **Transition guards** - Conditions check karo before transition
4. **Lifecycle hooks** - State enter/exit pe actions run karo
5. **History tracking** - Previous states track karo (undo ke liye)

#### Real-world scenario:

```
E-commerce checkout:
CART → SHIPPING_INFO → PAYMENT → ORDER_PLACED

Valid: CART → SHIPPING_INFO ✓
Invalid: CART → ORDER_PLACED ✗ (steps skip nahi kar sakte)

Guards:
- SHIPPING_INFO → PAYMENT (only if address filled)
- PAYMENT → ORDER_PLACED (only if payment successful)
```

#### Think about:

* States ko kaise represent karoge? (String? Enum? Objects?)
* Transitions ka map kaise banoge?
* Guards ko kaise implement karoge?
* Illegal transitions ko kaise prevent karoge?

***

### 6️⃣ **Notification Queue System** ⏱️ 40 min

#### Problem:

App mein notifications flood ho jati hain. 10 notifications ek saath dikhaoge toh UI kharab lagega. Queue chahiye jo controlled way mein dikhaye.

#### Requirements:

1. **Priority levels** - `HIGH`, `MEDIUM`, `LOW` - high pehle dikhe
2. **Auto dismiss** - 3 seconds baad automatically gayab ho
3. **Max visible** - Screen pe max 3 notifications ek saath
4. **Queue management** - Baaki queue mein wait karein
5. **Manual dismiss** - User close button click kare toh turant next dikhe

#### Real-world scenario:

```
5 notifications aaye ek saath:
1. Error (HIGH)
2. Success (LOW)  
3. Warning (MEDIUM)
4. Info (LOW)
5. Error (HIGH)

Display order:
1st: Error (HIGH) → 3s → dismiss → next
2nd: Error (HIGH) → 3s → dismiss → next
3rd: Warning (MEDIUM) → 3s → dismiss → next
4th: Success (LOW) → ...
5th: Info (LOW)
```

#### Think about:

* Queue ko kaise implement karoge? (Array? Priority queue?)
* Timing/delays kaise handle karoge?
* Multiple notifications ko parallel kaise manage karoge?
* Priority sorting efficiently kaise karoge?

***

### 7️⃣ **Undo/Redo Manager (Command Pattern)** ⏱️ 55 min

#### Problem:

Text editor, drawing app, ya koi bhi app jisme user actions undo kar sake. Har feature ke liye alag undo logic likhna painful hai.

#### Requirements:

1. **Generic undo/redo** - Kisi bhi action ko undo/redo kar sako
2. **Command history** - Last 50 actions track karo
3. **Redo invalidation** - Naya action kiya toh redo history clear ho jaye
4. **Batch operations** - Multiple actions ko group karke ek saath undo karo
5. **Memory limit** - Purani history delete ho jaye (50 se zyada mat rakho)

#### Real-world scenario:

```
User actions:
1. Add rectangle
2. Change color to red
3. Move rectangle  
4. Delete rectangle

Undo → Rectangle wapas aa gaya (position 3)
Undo → Color change undo (position 2)
Redo → Color red ho gaya again (position 3)

Naya action: Add circle
↓
Redo history clear! (Ab "Delete rectangle" redo nahi kar sakte)
```

#### Think about:

* Commands ko kaise represent karoge?
* Execute aur undo dono kaise store karoge?
* Redo stack ko kab clear karoge?
* Memory efficiently kaise use karoge?

***

### 8️⃣ **Local Storage Manager with Schema** ⏱️ 45 min

#### Problem:

LocalStorage directly use karte ho, toh data mess ho jata hai. Key names clash karte hain, data corrupt ho jata hai, expiry nahi hoti.

#### Requirements:

1. **Namespacing** - Har app ka apna namespace ho
2. **Versioning** - Schema change ho toh old data migrate kar do
3. **Expiry** - TTL support (like cache)
4. **Type safety** - Store karte time type check karo
5. **Quota management** - LocalStorage full ho toh oldest data delete karo

#### Real-world scenario:

```
App 1: myapp.user.token
App 2: myapp.cart.items

Schema v1: user = { name, email }
Schema v2: user = { firstName, lastName, email }
↓
Migrate: name → firstName (automatic!)

Store: user token with 1 hour expiry
After 1 hour → Automatic cleanup
```

#### Think about:

* Schema changes ko kaise handle karoge?
* Migration logic kahan likha jayega?
* Quota exceed hone pe kya karoge?
* Type checking kaise karoge without TypeScript?

***

### 🎯 **Pick Karo Aur Start Karo!**

**Easy to Hard order:**

1. Event Bus (easiest)
2. Notification Queue
3. Cache System
4. Form Validator
5. LocalStorage Manager
6. Retry Manager
7. Undo/Redo (hardest)

**Pick one, set 1-hour timer, aur shuru karo!** ⏰

**Rules:**

* ❌ Solution mat dekho Google pe
* ✅ Pehle soch lo - paper pe design sketch karo
* ✅ Test cases khud likho
* ✅ Working code > Perfect code

**Jab ban jaye, mujhe batana. Main review karunga aur next level problem dunga!** 🔥

Kaunsa problem choose kiya? 👀

***
