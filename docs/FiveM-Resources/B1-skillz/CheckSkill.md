#### `CheckSkill`

- **Purpose:** Checks if a skill meets or exceeds a certain value.
- **Arguments:**  
   `skill` - `string` - The name of the skill to check.  
   `val` - `int` - The value to check against.  
- **Returns:** `boolean` - Whether the skill meets or exceeds the given value.
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
