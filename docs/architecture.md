# 🏗️ Brickshop Simulator - Technical Architecture

> **Status:** This is a target architecture draft for planning purposes.  
> The First Playable prototype will implement a simplified subset of these systems.

---

## 🎯 Design Principles

- **Data-driven** - Sets are ScriptableObjects, not hardcoded
- **Modular** - Each manager does one job
- **Simple first** - Build basic version, add complexity later
- **Easy to extend** - New content shouldn't need code changes

---

## 📦 Project Structure

```
Assets/
├── Scripts/
│   ├── Core/              # GameManager, day/night cycle
│   ├── Economy/           # Money, prices, transactions
│   ├── Inventory/         # Set storage, shelves
│   ├── Data/              # ScriptableObject definitions
│   └── UI/                # Menus, displays
├── ScriptableObjects/     # Actual set data files
│   └── Sets/
├── Scenes/
│   └── Shop_Prototype.unity
└── Prefabs/
    ├── UI/
    └── Shop/
```

---

## 🧩 Core Systems

### 1. Game Management
**GameManager** - Controls game flow (start day, end day, game state)

```csharp
public class GameManager : MonoBehaviour
{
    public static GameManager Instance; // Easy access from anywhere
    public int currentDay = 1;
    
    public void StartDay() 
    {
        // TODO: Open shop, reset counters
    }
    
    public void EndDay() 
    {
        // TODO: Show summary, advance day
    }
}
```

---

### 2. Economy System
**MoneyManager** - Tracks player balance, handles buy/sell transactions

```csharp
public class MoneyManager : MonoBehaviour
{
    private float balance = 1000f; // Starting money
    
    public bool TrySpend(float amount) 
    {
        // Check if affordable, deduct money, return success
    }
    
    public void AddMoney(float amount) 
    {
        // Add money from sales
    }
}
```

**PriceCalculator** *Planned for V1.0*  
Calculates prices based on condition (A/B/C) and Out-of-Print status.

---

### 3. Data Layer (ScriptableObjects)

**SetData** - Defines what a brick set is

```csharp
[CreateAssetMenu(menuName = "Brickshop/Set")]
public class SetData : ScriptableObject
{
    public string setName;        // "Fire Station"
    public string setID;          // "CITY-001"
    public int basePrice;         // 50
    public Sprite boxArt;         // Image for UI
    public SetTheme theme;        // City, Space, Castle...
}

public enum SetTheme { City, Space, Castle, Trains, Flowers }
```

**Why ScriptableObjects?**  
You can create new sets in Unity without writing code. Just right-click → Create → Brickshop → Set.

---

### 4. Inventory System

**InventoryManager** - Keeps track of all sets the player owns

```csharp
public class InventoryManager : MonoBehaviour
{
    private List<SetInstance> ownedSets = new List<SetInstance>();
    
    public void AddSet(SetData data, SetCondition condition) 
    {
        // TODO: Create new SetInstance, add to list
    }
    
    public void RemoveSet(SetInstance set) 
    {
        // TODO: Remove from list when sold
    }
}
```

**SetInstance** - A specific copy of a set the player owns

```csharp
public class SetInstance
{
    public SetData data;           // Which set is this?
    public SetCondition condition; // A, B, or C grade
    public int purchaseDay;        // When did player buy it?
    public float purchasePrice;    // How much did it cost?
}

public enum SetCondition { A, B, C } // Perfect, Good, Damaged
```

---

### 5. Shop Layout

**ShelfController** - Manages one shelf in the shop

```csharp
public class ShelfController : MonoBehaviour
{
    public int maxSlots = 6; // How many sets fit
    private List<SetInstance> placedSets = new List<SetInstance>();
    
    public bool TryPlaceSet(SetInstance set) 
    {
        // TODO: Check if space available, add set
    }
}
```

---

## 🎯 What's Actually in the Prototype?

### First Playable (v0.1) - Basic Systems Only

**What WILL be built:**
- GameManager (basic day counter, start/end buttons)
- MoneyManager (track balance, simple +/- money)
- InventoryManager (simple list of owned sets)
- SetData (ScriptableObjects for 3-5 test sets)
- ShelfController (basic placement system)
- Minimal UI (balance display, day counter)

**What comes LATER (v1.0+):**
- TimeManager (real-time day cycle)
- PriceCalculator (condition & OOP modifiers)
- TransactionLog (purchase history)
- Customer AI
- Save/Load system

---

## 🛠️ Technical Decisions

### Manager Pattern
Each manager handles one responsibility:
- MoneyManager = money only
- InventoryManager = sets only
- GameManager = game flow only

Makes debugging easier and keeps code organized.

### Singletons
Used for managers so any script can access them easily:
```csharp
MoneyManager.Instance.GetBalance();
```

**Trade-off:** Simple for small projects, but can cause tight coupling. Good enough for prototype, might refactor later.

### ScriptableObjects vs. Code
Sets are data files, not hardcoded classes. This means:
- Add new sets without coding
- Balance prices in Unity Inspector
- Designers can create content
- Easy to mod later

---

## 🔄 Example: Buying a Set

1. Player clicks "Buy" button in UI
2. UI asks MoneyManager: "Can we afford 50 coins?"
3. MoneyManager checks balance → Yes/No
4. **If yes:**
   - MoneyManager removes 50 coins
   - InventoryManager creates new SetInstance
   - UI updates to show new balance
5. **If no:**
   - UI shows "Not enough money"

No system directly touches another's data - everything goes through public methods.

---

## 🚀 How to Add Features Later

**New set theme?**
1. Add value to `SetTheme` enum
2. Create ScriptableObject files
3. Done - no code changes

**New box design styles?**
1. Add `BoxStyle` field to SetData
2. Update UI to display it
3. Existing sets still work

**Repair system?**
1. Create RepairManager
2. Add methods that change `SetCondition`
3. MoneyManager/Inventory already support it

---

*Last Updated: November 2025*
