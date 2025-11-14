# Question

## 🔥 The Ultimate OOP Challenge: Real-Time Collaborative Whiteboard

Bhai, ye problem tumhe sochne par majboor kar degi. Isme OOP ki saari power use karni padegi.

***

### 📋 **Problem Statement:**

Tumhe ek **collaborative whiteboard application** banana hai jisme:

#### **Core Features:**

1. **Multiple Users** - Real-time mein kai log ek saath draw kar sakte hain
2. **Different Tools** - Pen, Eraser, Shapes (Rectangle, Circle, Line), Text, Highlighter
3. **Layers System** - Photoshop jaisa - elements ko layers mein organize karo
4. **Undo/Redo** - Har user ki apni undo/redo history
5. **Permissions** - Admin, Editor, Viewer - alag-alag access levels
6. **Export** - Different formats mein export (PNG, SVG, JSON)
7. **Version History** - Snapshots save karo, purane versions ko restore karo
8. **Collaborative Cursors** - Har user ka cursor color alag dikhna chahiye
9. **Comments/Annotations** - Kisi bhi element pe comment kar sakte ho
10. **Tool Presets** - Users apne favorite tool settings save kar sakte hain

#### **Advanced Requirements:**

* **Performance:** 1000+ elements smoothly render hone chahiye
* **Memory Management:** Purane actions ko smart tarike se cleanup karo
* **Conflict Resolution:** Do users ek hi element edit kar rahe ho toh kya hoga?
* **State Management:** Poora application state serialize/deserialize ho sake
* **Event System:** Proper event bubbling/propagation (jaise DOM)
* **Plugin Architecture:** Baad mein nayi tools add kar sako bina core code change kiye

***

### 🤔 **Tumhe Sochna Hai:**

#### **Design Decisions:**

1. **Class Hierarchy kaisa hoga?**
   * Tool classes kaise design karoge?
   * Shape inheritance kaise structure karoge?
   * User roles ko kaise implement karoge?
2. **Patterns kaun se use karoge?**
   * Command Pattern (undo/redo ke liye)?
   * Observer Pattern (real-time updates ke liye)?
   * Factory Pattern (tool creation ke liye)?
   * Strategy Pattern (different export formats ke liye)?
   * Composite Pattern (layers/groups ke liye)?
3. **State Management:**
   * Canvas ka state kaise manage karoge?
   * History ko efficiently kaise store karoge?
   * Memory leaks se kaise bachoge?
4. **Performance Optimization:**
   * Kaunse elements re-render honge aur kaunse nahi?
   * Collision detection kaise optimize karoge?
   * Large canvas pe zoom in/out kaise handle karoge?
5. **Collaboration Logic:**
   * Optimistic updates vs Server sync?
   * Race conditions kaise handle karoge?
   * Lock mechanism chahiye kya elements pe?

***

### 🎯 **Main Challenge Areas:**

#### **1. Tool System Design**

Har tool ka apna behavior hai but common interface chahiye. Kaise design karoge?

#### **2. Element Hierarchy**

Shapes, Text, Images - sab ko unified way mein kaise handle karoge? Groups aur nested elements ka kya?

#### **3. History Management**

Undo karne pe kya hoga? Redo? Memory usage? 100 actions ka history kaise efficiently store karoge?

#### **4. Permission System**

Admin sab kar sakta hai, Editor draw kar sakta hai, Viewer sirf dekh sakta hai. Ye logic har jagah duplicate nahi hona chahiye.

#### **5. Event System**

Element click hua, drag hua, resize hua - ye sab events kaise propagate honge? Parent-child relationships?

#### **6. Serialization**

Poori canvas ko JSON mein convert karo aur wapas restore karo - sab kuch (tools state, history, layers) ke saath!

***

### 💀 **Specific Tricky Scenarios:**

1. **User A ne ek rectangle draw kiya, User B use delete kar raha hai, same time User C use resize kar raha hai** - Kaise handle karoge?
2. **10,000 elements hai canvas pe, mouse move pe hover effects dikhane hain** - Performance kaise maintain karoge?
3. **User ne 50 undo kiya, phir nayi action ki, toh redo history ka kya hoga?**
4. **Grouped elements ko move karte time sab children bhi move hone chahiye** - Ye relationship kaise maintain karoge?
5. **Different tools different cursors show karte hain, keyboard shortcuts bhi change hote hain** - Ye state kaise manage karoge?
6. **Export karte time SVG chahiye, PNG chahiye ya custom JSON format** - Ek hi codebase se teeno kaise generate karoge?

***

### 🚀 **Bonus Challenges:**

* **Smart Guides:** Jab element drag karo toh alignment guides automatically dikhe
* **Snapping:** Elements ek dusre se align ho jaye automatically
* **Z-index Management:** Elements ko front/back lana
* **Selection Tool:** Multiple elements select karke ek saath move karna
* **Copy/Paste:** With full state preservation
* **Templates:** Pre-made designs ko load karna
* **Real-time Sync:** WebSocket se multiple users ka coordination

***
