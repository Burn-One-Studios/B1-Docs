#### `UpdateSkill`

- **Purpose:** Modifies a given skill by a specified amount.
- **Arguments:**  
   `skill` - `string` - The name of the skill to modify.  
   `amount` - `int` - The amount to modify the skill by.
- **Usage:**
```lua
exports["B1-skillz"]:UpdateSkill("Stamina", 2)  -- Adds 2% to Stamina.
```