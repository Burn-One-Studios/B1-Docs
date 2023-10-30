## B1-skillz Resource Guide

### Installation Steps

1. **Download and Place the Resource:**  
   Download the `B1-skillz` resource and move it to your resource folder.

2. **Folder Naming:**  
   Ensure the folder is named `B1-skillz`. Remove any `-qb` or `-esx` suffix if present.

3. **Database Configuration:**  
   Import the accompanying SQL file into your server's database.

4. **Server Configuration:**  
   If you have added it to a folder that is already being ensured then you have completed the process otherwise, add `start B1-skillz` to your `server.cfg` file.

!!! warning
    Make sure you have properly followed step 2. If you do not, the resource will not work.
---

### Function Documentation

#### `UpdateSkill(skill, amount)`

- **Purpose:** Modifies a given skill by a specified amount.
- **Arguments:**  
   `skill` - `string` - The name of the skill to modify.  
   `amount` - `int` - The amount to modify the skill by.
- **Usage:**
```lua
exports["B1-skillz"]:UpdateSkill("Stamina", 2)  -- Adds 2% to Stamina.
```
#### `CheckSkill(skill, val)`

- **Purpose:** Checks if a skill meets or exceeds a certain value.
- **Arguments:**  
   `skill` - `string` - The name of the skill to check.  
   `val` - `int` - The value to check against.  
- **Usage:**
```lua
exports["B1-skillz"]:CheckSkill("Lung Capacity", 50, function(hasskill)
    if hasskill then
        print("Lung Capacity 50 or over")
    else
        print("Lung Capacity lower than 50")
    end
end)
```
!!! info
    This function is asynchronous, so it requires a callback function to be passed in. The callback function will be called with a boolean value indicating whether the skill meets or exceeds the given value. This function is useful for checking if a player has a certain skill level before allowing them to perform an action.

#### `GetCurrentSkill(skill)`

- **Purpose:** Retrieves the current value of a given skill.
- **Arguments:**
   `skill` - `string` - The name of the skill to retrieve.
- **Usage:**
```lua
CreateThread(function()
    local shootingskill = exports["B1-skillz"]:GetCurrentSkill("Shooting")
    ESX.DumpTable(shootingskill) -- Prints to server console.
    print(shootingskill.Current)  -- Prints to client console.
end)
```

| Key          | Type    | Description                   |
| ------------ | ------- | ----------------------------- |
| Current      | int     | Current Value of the skill    |
| RemoveAmount | int     | RemoveAmount for that skill   |
| Stat         | string  | Name of the skill             |
| icon         | string  | FontAwesome icon name         |


---

### Resource Preview

#### **Skills Menu:**

![Preview of skills menu](https://raw.githubusercontent.com/Kingsage311/Kingsage311/main/assets/skillmenuprev.png)
