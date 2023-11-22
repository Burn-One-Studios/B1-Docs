- **Purpose:** Retrieves the current value of a given skill.
- **Arguments:**
   `skill` - `string` - The name of the skill to retrieve.
- **Returns:** `table`

| Key          | Type    | Description                   |
| ------------ | ------- | ----------------------------- |
| Current      | int     | Current Value of the skill    |
| RemoveAmount | int     | RemoveAmount for that skill   |
| Stat         | string  | Name of the skill             |
| icon         | string  | FontAwesome icon name         |

- **Usage:**
```lua
CreateThread(function()
    local shootingskill = exports["B1-skillz"]:GetCurrentSkill("Shooting")
    ESX.DumpTable(shootingskill) -- Prints to server console.
    print(shootingskill.Current)  -- Prints to client console.
end)
```
