````markdown
---
title: B1-lib (FiveM)
description: A small, easy-to-use FiveM library with normalized functions for QBCore and ESX.
icon: material/library
tags:
  - FiveM
  - QBCore
  - ESX
  - Lua
---

# 🚀 B1-lib Documentation

> **A small, easy-to-use FiveM library that provides normalized functions for QBCore and ESX frameworks.**

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/your-repo/b1-lib)
[![Framework](https://img.shields.io/badge/framework-QBCore%20%7C%20ESX-green.svg)](https://github.com/your-repo/b1-lib)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](https://github.com/your-repo/b1-lib)

[View on GitHub](https://github.com/your-repo/b1-lib){ .md-button .md-button--primary }
[Skip to Quick Start](#-quick-start){ .md-button }

---

## 📦 Installation

1. **Download** the `b1-lib` folder to `resources/[lib]/b1-lib/`
2. **Add** `ensure b1-lib` to your `server.cfg`
3. **Optional dependencies:** `oxmysql`, `ox_inventory`, `ox_lib`

```lua
-- server.cfg
ensure b1-lib
ensure oxmysql
ensure ox_inventory
ensure ox_lib
````

!!! note "Optional but recommended"
\- **oxmysql**: enables the DB and ORM features
\- **ox\_inventory**: enables inventory helpers
\- **ox\_lib**: enables unified notifications / progress / skill checks

---

## ⚡ Quick Start

\=== "Method 1 · Get the entire B1 object (Recommended)"

```lua
-- Get the complete B1 object
local B1 = exports['b1-lib']:getB1Object()

-- Use any B1 function
B1.log('info', 'Hello from b1-lib!')
B1.notify('Welcome to the server!', 'success')
B1.progressBar('Loading...', 5000)
```

\=== "Method 2 · Individual exports"

```lua
-- Use specific exports
exports['b1-lib']:log('info', 'Hello from b1-lib!')
exports['b1-lib']:notify('Welcome to the server!', 'success')
exports['b1-lib']:progressBar('Loading...', 5000)
```

\=== "Method 3 · Mixed approach"

```lua
-- Get B1 object for most functions
local B1 = exports['b1-lib']:getB1Object()

-- Use individual exports for specific cases
local success = exports['b1-lib']:skillCheck('hard')
```

!!! tip "Why Method 1?"
\- ✅ Clean, readable code
\- ✅ Access to all B1 functions
\- ✅ Better performance (single export call)
\- ✅ IDE autocomplete support

---

## 🧩 Basic Usage Examples

```lua
-- Get B1 object (recommended approach)
local B1 = exports['b1-lib']:getB1Object()

-- Logging + notifications
B1.log('info', 'Hello from b1-lib!')
B1.notify('Welcome to the server!', 'success')

-- UI functions
B1.progressBar('Loading...', 5000, {
    onFinish = function()
        B1.notify('Loading complete!', 'success')
    end
})

local success = B1.skillCheck('easy')
if success then
    B1.notify('Skill check passed!', 'success')
end

-- Spawn a vehicle (client-side example below has an alt signature)
local netId = B1.spawnVehicle('adder', function(vehicle)
    print('Vehicle spawned:', vehicle)
end)

-- Simple database query (server-side only)
local players = B1.db.query('SELECT * FROM users WHERE job = ?', {'police'})

-- ORM model (server-side only)
local Users = B1.orm.define('users', 'id')
local user = Users:find(1)
user.name = 'New Name'
user:save()
```

---

## 📝 Logger *(Shared: client & server)*

Colored, boxed output with traceback support and calling resource detection.

### Functions

| Function                        | Type                                    | Description                        |
| ------------------------------- | --------------------------------------- | ---------------------------------- |
| `B1.log(level, message, opts?)` | `(string\|number, any, table?) -> void` | Log a message with specified level |
| `B1.setLogLevel(level)`         | `(string) -> void`                      | Set the minimum log level          |
| `B1.getLogLevel()`              | `() -> string`                          | Get current log level              |

### Log Levels

| Level     | Numeric | Color     | Description             |
| --------- | ------: | --------- | ----------------------- |
| `debug`   |     `1` | 🔵 Blue   | Debug information       |
| `info`    |     `2` | 🟢 Green  | General information     |
| `warning` |     `3` | 🟡 Yellow | Warnings                |
| `error`   |     `4` | 🔴 Red    | Errors (with traceback) |

#### Examples

```lua
-- String levels
B1.log('info', 'This is an info message')
B1.log('warning', 'This is a warning')
B1.log('error', 'This is an error')

-- Numeric levels (1=debug, 2=info, 3=warning, 4=error)
B1.log(2, 'This is also an info message')

-- Table logging (automatically formatted)
B1.log('info', { name = 'John', age = 30, job = 'police' })

-- With context
B1.log('info', 'Player joined', { ctx = { playerId = 123, name = 'John', source = 1 } })

-- Error with automatic traceback
B1.log('error', 'Something went wrong!')

-- Log level management
B1.setLogLevel('debug')
B1.setLogLevel('error')
local currentLevel = B1.getLogLevel() -- 'error'
```

**Output format**

```
^3[b1-lib]^7^2[INFO]^7 [my-resource] Player joined | {"playerId":123,"name":"John"}
```

---

## 🛠️ Utils *(Shared)*

Common utilities for strings, numbers, and system detection.

| Function                         | Type                          | Description               | Example                                          |
| -------------------------------- | ----------------------------- | ------------------------- | ------------------------------------------------ |
| `B1.randomStr(length?)`          | `(number?) -> string`         | Generate random string    | `B1.randomStr(10)` → `"aB3xY9mK2p"`              |
| `B1.randomInt(length?)`          | `(number?) -> number`         | Generate random integer   | `B1.randomInt(6)` → `123456`                     |
| `B1.splitStr(str, delimiter?)`   | `(string, string?) -> table`  | Split string              | `B1.splitStr('a,b,c', ',')`                      |
| `B1.sanitizeString(s, charset?)` | `(string, string?) -> string` | Keep only specified chars | `B1.sanitizeString('Hello!@#$')` → `"Hello"`     |
| `B1.removeChars(s, charset?)`    | `(string, string?) -> string` | Remove specified chars    | `B1.removeChars('Hello!@#$')` → `"Hello"`        |
| `B1.trim(s)`                     | `(string) -> string`          | Trim whitespace           | `B1.trim('  hello  ')` → `"hello"`               |
| `B1.firstToUpper(s)`             | `(string) -> string`          | Capitalize first letter   | `B1.firstToUpper('hello')` → `"Hello"`           |
| `B1.round(num, decimals?)`       | `(number, number?) -> number` | Round to decimals         | `B1.round(3.14159, 2)` → `3.14`                  |
| `B1.getCoreName()`               | `() -> string\|nil`           | Framework name            | `"qb-core"` / `"esx"`                            |
| `B1.getInventoryName()`          | `() -> string`                | Inventory system          | `"ox"` / `"qb"` / `"esx"`                        |
| `B1.getCoreObject()`             | `() -> table\|nil`            | Core object               | Framework object                                 |
| `B1.getNotificationSystem()`     | `() -> string`                | Notification system       | `"ox"` / `"zsx"` / `"qb"` / `"esx"` / `"native"` |
| `B1.getProgressBarSystem()`      | `() -> string`                | Progress bar system       | same as above                                    |
| `B1.getSkillCheckSystem()`       | `() -> string`                | Skill check system        | same as above                                    |

#### Examples

```lua
local randomStr = B1.randomStr(10)
local randomInt = B1.randomInt(6)
local parts = B1.splitStr('apple,banana,cherry', ',')

local clean = B1.sanitizeString('Hello!@#$', 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz ')
local trimmed = B1.trim('  hello world  ')
local capitalized = B1.firstToUpper('hello world')
local rounded = B1.round(3.14159, 2)

local coreName = B1.getCoreName()
local inventoryName = B1.getInventoryName()
local coreObject = B1.getCoreObject()
local notificationSystem = B1.getNotificationSystem()
local progressBarSystem = B1.getProgressBarSystem()
local skillCheckSystem = B1.getSkillCheckSystem()
```

---

## 🗄️ Database *(Server only)*

Lightweight ORM with **oxmysql** adapter. All functions are await-style and non-blocking.

!!! warning "Requires `oxmysql`"
Database functions are disabled if `oxmysql` is not available.

### Basic Queries

| Function                     | Type                             | Description                     | Example                                                               |
| ---------------------------- | -------------------------------- | ------------------------------- | --------------------------------------------------------------------- |
| `B1.db.query(sql, params?)`  | `(string, table?) -> table`      | Execute query, return rows      | `B1.db.query('SELECT * FROM users')`                                  |
| `B1.db.single(sql, params?)` | `(string, table?) -> table\|nil` | Get single row                  | `B1.db.single('SELECT * FROM users WHERE id = ?', {1})`               |
| `B1.db.scalar(sql, params?)` | `(string, table?) -> any\|nil`   | Get scalar value                | `B1.db.scalar('SELECT COUNT(*) FROM users')`                          |
| `B1.db.insert(sql, params?)` | `(string, table?) -> number`     | Insert record, return ID        | `B1.db.insert('INSERT INTO users (name) VALUES (?)', {'John'})`       |
| `B1.db.update(sql, params?)` | `(string, table?) -> number`     | Update records, return affected | `B1.db.update('UPDATE users SET name = ? WHERE id = ?', {'Jane', 1})` |

#### Examples

```lua
local players = B1.db.query('SELECT * FROM users WHERE job = ?', {'police'})
local user = B1.db.single('SELECT * FROM users WHERE id = ?', {1})
local count = B1.db.scalar('SELECT COUNT(*) FROM users')
local insertId = B1.db.insert('INSERT INTO users (name, job) VALUES (?, ?)', {'John', 'police'})
local affected = B1.db.update('UPDATE users SET job = ? WHERE id = ?', {'admin', 1})
```

### ORM Models

| Function                                | Type                         | Description         |
| --------------------------------------- | ---------------------------- | ------------------- |
| `B1.orm.define(tableName, primaryKey?)` | `(string, string?) -> Model` | Create a new model  |
| `Model:find(id)`                        | `(any) -> Row\|nil`          | Find record by ID   |
| `Model:findAll(where?, params?)`        | `(string?, table?) -> table` | Find all with WHERE |
| `Model:insert(data)`                    | `(table) -> number`          | Insert record       |
| `Model:update(id, data)`                | `(any, table) -> boolean`    | Update by ID        |
| `Model:delete(id)`                      | `(any) -> boolean`           | Delete by ID        |
| `Row:save()`                            | `() -> boolean`              | Save changes        |
| `Row:delete()`                          | `() -> boolean`              | Delete row          |

#### Examples

```lua
local Users = B1.orm.define('users', 'id')

local user = Users:find(1)
if user then
  print(user.name)
  user.name = 'New Name'
  user:save()
end

local allUsers = Users:findAll()
local police = Users:findAll('job = ?', {'police'})
local newId = Users:insert({ name = 'Jane', job = 'police', level = 5 })
local ok = Users:update(1, { job = 'admin', level = 10 })
local deleted = Users:delete(1)

local row = Users:find(1)
row.job = 'admin'
row:save()
row:delete()
```

---

## 👤 Player Functions

Normalized player management for QBCore and ESX.

### Server-side *(Server only)*

| Function                                        | Type                                           | Description           | Example                                      |
| ----------------------------------------------- | ---------------------------------------------- | --------------------- | -------------------------------------------- |
| `B1.getCorePlayer(source)`                      | `(number) -> table\|nil`                       | Core player object    | `B1.getCorePlayer(1)`                        |
| `B1.getPlayerByIdentifier(identifier)`          | `(string) -> table\|nil`                       | Online by identifier  | `B1.getPlayerByIdentifier('ABC123')`         |
| `B1.getOfflinePlayerByIdentifier(identifier)`   | `(string) -> table\|nil`                       | Offline by identifier | `B1.getOfflinePlayerByIdentifier('ABC123')`  |
| `B1.getPlayers()`                               | `() -> table`                                  | All online players    | `B1.getPlayers()`                            |
| `B1.addMoney(source, type, amount, reason?)`    | `(number, string, number, string?) -> boolean` | Add money             | `B1.addMoney(1, 'cash', 1000, 'Reward')`     |
| `B1.removeMoney(source, type, amount, reason?)` | `(number, string, number, string?) -> boolean` | Remove money          | `B1.removeMoney(1, 'bank', 500, 'Purchase')` |
| `B1.getMoney(source, type)`                     | `(number, string) -> number`                   | Get balance           | `B1.getMoney(1, 'cash')`                     |
| `B1.setMoney(source, type, amount)`             | `(number, string, number) -> boolean`          | Set balance           | `B1.setMoney(1, 'bank', 5000)`               |
| `B1.getLicences(source)`                        | `(number) -> table`                            | All licenses          | `B1.getLicences(1)`                          |
| `B1.getLicence(source, type)`                   | `(number, string) -> boolean`                  | Check license         | `B1.getLicence(1, 'driver')`                 |
| `B1.addLicence(source, type)`                   | `(number, string) -> boolean`                  | Add license           | `B1.addLicence(1, 'weapon')`                 |
| `B1.removeLicence(source, type)`                | `(number, string) -> boolean`                  | Remove license        | `B1.removeLicence(1, 'driver')`              |
| `B1.getCitizenId(source)`                       | `(number) -> string\|nil`                      | Citizen ID            | `B1.getCitizenId(1)`                         |
| `B1.getPlayerName(source)`                      | `(number) -> string`                           | Player name           | `B1.getPlayerName(1)`                        |

#### Examples

```lua
local player = B1.getCorePlayer(source)
local byId = B1.getPlayerByIdentifier('ABC123')
local offline = B1.getOfflinePlayerByIdentifier('ABC123')
local players = B1.getPlayers()

B1.addMoney(source, 'cash', 1000, 'Reward')
B1.removeMoney(source, 'bank', 500, 'Purchase')
local cash = B1.getMoney(source, 'cash')
B1.setMoney(source, 'bank', 5000)

local licenses = B1.getLicences(source)
local hasDriver = B1.getLicence(source, 'driver')
B1.addLicence(source, 'weapon')
B1.removeLicence(source, 'driver')

local citizenId = B1.getCitizenId(source)
local playerName = B1.getPlayerName(source)
```

### Client-side *(Client only)*

| Function                             | Type                                 | Description           | Example                          |
| ------------------------------------ | ------------------------------------ | --------------------- | -------------------------------- |
| `B1.getCorePlayerData()`             | `() -> table\|nil`                   | Core player data      | `B1.getCorePlayerData()`         |
| `B1.getPlayerData()`                 | `() -> table\|nil`                   | Normalized data       | `B1.getPlayerData()`             |
| `B1.getPlayerJob()`                  | `() -> table\|nil`                   | Job info              | `B1.getPlayerJob()`              |
| `B1.getPlayerGang()`                 | `() -> table\|nil`                   | Gang info             | `B1.getPlayerGang()`             |
| `B1.getPlayerMoney(type)`            | `(string) -> number`                 | Money                 | `B1.getPlayerMoney('cash')`      |
| `B1.getPlayerName()`                 | `() -> string`                       | Name                  | `B1.getPlayerName()`             |
| `B1.getCitizenId()`                  | `() -> string\|nil`                  | Citizen ID (callback) | `B1.getCitizenId()`              |
| `B1.notify(message, type?, length?)` | `(string, string?, number?) -> void` | Notify                | `B1.notify('Hello!', 'success')` |

#### Examples

```lua
local playerData = B1.getPlayerData()
local job = B1.getPlayerJob()
local gang = B1.getPlayerGang()
local cash = B1.getPlayerMoney('cash')
local bank = B1.getPlayerMoney('bank')
local name = B1.getPlayerName()
local citizenId = B1.getCitizenId()

B1.notify('Hello world!', 'success', 5000)
B1.notify('Warning message', 'warning')
B1.notify('Error occurred', 'error')
```

**Money Types**

| Type    | QBCore | ESX | Description            |
| ------- | ------ | --- | ---------------------- |
| `cash`  | ✅      | ✅   | Cash                   |
| `bank`  | ✅      | ✅   | Bank                   |
| `black` | ❌      | ✅   | Black money (ESX only) |

---

## 🚗 Vehicle Functions

Spawn, plate detection, and label retrieval.

### Server-side *(Server only)*

| Function                                         | Type                                                  | Description              | Example                                               |
| ------------------------------------------------ | ----------------------------------------------------- | ------------------------ | ----------------------------------------------------- |
| `B1.spawnVehicle(source, model, coords?, warp?)` | `(number, string, vector4?, boolean?) -> number\|nil` | Spawn vehicle for player | `B1.spawnVehicle(1, 'adder', vector4(0,0,0,0), true)` |
| `B1.getPlate(netId)`                             | `(number) -> string\|nil`                             | Get vehicle plate        | `B1.getPlate(123)`                                    |
| `B1.getVehicleLabel(model)`                      | `(string) -> string`                                  | Display name             | `B1.getVehicleLabel('adder')`                         |

#### Server-side Examples

```lua
local netId = B1.spawnVehicle(source, 'adder', vector4(0, 0, 0, 0), true)
local plate = B1.getPlate(netId)
local label = B1.getVehicleLabel('adder')
```

### Client-side *(Client only)*

| Function                                                            | Type                                                               | Description             | Example                                      |
| ------------------------------------------------------------------- | ------------------------------------------------------------------ | ----------------------- | -------------------------------------------- |
| `B1.spawnVehicle(model, cb?, coords?, isnetworked?, teleportInto?)` | `(string, function?, vector4?, boolean?, boolean?) -> number\|nil` | Spawn vehicle           | `B1.spawnVehicle('adder', function(id) end)` |
| `B1.getPlate(vehicle)`                                              | `(number\|entity) -> string\|nil`                                  | Vehicle plate           | `B1.getPlate(vehicle)`                       |
| `B1.getVehicleLabel(model)`                                         | `(string) -> string`                                               | Display name (callback) | `B1.getVehicleLabel('adder')`                |

#### Client-side Examples

```lua
local netId = B1.spawnVehicle('adder', function(vehicle)
    print('Vehicle spawned:', vehicle)
end, vector4(0, 0, 0, 0), true, true)

local plate = B1.getPlate(vehicle) -- or netId
local label = B1.getVehicleLabel('adder') -- "Truffade Adder"
```

### Vehicle Spawning Parameters

| Parameter               | Type      | Default                      | Description                     |
| ----------------------- | --------- | ---------------------------- | ------------------------------- |
| `model`                 | `string`  | —                            | Vehicle model (e.g., `adder`)   |
| `coords`                | `vector4` | `Config.VehicleDefaultSpawn` | Spawn coords (x, y, z, heading) |
| `warp` / `teleportInto` | `boolean` | `true`                       | Put player into vehicle         |
| `isnetworked`           | `boolean` | `true`                       | Should be networked             |

---

## 🎨 UI Functions *(Client only)*

Unified notifications, progress bars, and skill checks across multiple systems.

> **Important:** UI functions are client-side only because they require player interaction.
> To trigger from the server, use `TriggerClientEvent` and call the client-side functions.

### Supported Systems

| System      | Notifications | Progress Bars | Skill Checks |
| ----------- | ------------- | ------------- | ------------ |
| **ox\_lib** | ✅             | ✅             | ✅            |
| **ZSX\_UI** | ✅             | ✅             | ❌            |
| **QBCore**  | ✅             | ✅             | ❌            |
| **ESX**     | ✅             | ✅             | ❌            |
| **Native**  | ✅             | ❌             | ❌            |

> Notes:
>
> * ZSX\_UI ProgressBar supported with animation/props.
> * QBCore/ESX have no built-in skill checks → fallback to ox\_lib if available.
> * ZSX\_UI notifications return a serial for removal.

### Functions

| Function                                          | Type                                         | Description                   | Example                                             |
| ------------------------------------------------- | -------------------------------------------- | ----------------------------- | --------------------------------------------------- |
| `B1.notify(message, type?, duration?, position?)` | `(string, string?, number?, string?) -> any` | Show notification             | `B1.notify('Hello!', 'success', 5000, 'top-right')` |
| `B1.progressBar(label, duration?, options?)`      | `(string, number?, table?) -> boolean`       | Show progress bar             | `B1.progressBar('Loading...', 10000)`               |
| `B1.skillCheck(difficulty?, options?)`            | `(string?, table?) -> boolean`               | Show skill check              | `B1.skillCheck('easy', {duration=5000})`            |
| `B1.removeNotification(serial)`                   | `(any) -> void`                              | Remove notification (ZSX\_UI) | `B1.removeNotification(serial)`                     |

#### Notification Examples

```lua
-- Basic notification (client-side)
B1.notify('Hello world!', 'success')

-- Custom duration and position
B1.notify('Warning message', 'warning', 3000, 'bottom-left')

-- Different types
B1.notify('Info message', 'info')
B1.notify('Success message', 'success')
B1.notify('Warning message', 'warning')
B1.notify('Error message', 'error')

-- ZSX_UI specific (returns serial for removal)
local serial = B1.notify('Permanent notification', 'info', -1)
B1.removeNotification(serial)
```

#### Server → Client UI

```lua
-- Server-side: notify specific player
B1.notifyServer(source, 'Server notification', 'info', 5000)

-- Server-side: notify all players
B1.notifyServer(-1, 'Global notification', 'success', 3000)

-- Using different types (string or numeric)
B1.notifyServer(source, 'Success message', 'success', 5000)
B1.notifyServer(source, 'Error message', 'error', 5000)
B1.notifyServer(source, 'Warning message', 'warning', 5000)
B1.notifyServer(source, 'Info message', 'info', 5000)
B1.notifyServer(source, 'Numeric success', 2, 5000) -- 2 = success
B1.notifyServer(source, 'Numeric error', 3, 5000)   -- 3 = error
```

#### Progress Bar Examples

```lua
-- Basic
B1.progressBar('Loading...', 10000)

-- With callbacks
B1.progressBar('Processing...', 5000, {
    onFinish = function() B1.notify('Process completed!', 'success') end,
    onCancel = function() B1.notify('Process cancelled', 'warning') end
})

-- With animation and prop (ZSX / ox patterns)
B1.progressBar('Repairing vehicle...', 15000, {
    anim = {
        dict = 'mini@repair',
        clip = 'fixing_a_ped',
        blendIn = 8.0, blendOut = 8.0, duration = 15000, flag = 0, playbackRate = 1.0
    },
    prop = {
        model = 'prop_tool_wrench',
        bone = 57005,
        pos = vector3(0.1, 0.0, 0.0),
        rot = vector3(0.0, 0.0, 0.0)
    },
    disable = { car = true, move = true, combat = true, mouse = false },
    canCancel = true
})
```

#### Skill Check Examples

```lua
-- Basic skill check
local success = B1.skillCheck('easy')

-- Custom options
local success = B1.skillCheck('hard', {
    duration = 8000,
    position = 'center',
    disable = { car = true, move = true, combat = true, mouse = false }
})

if success then
    B1.notify('Skill check passed!', 'success')
else
    B1.notify('Skill check failed!', 'error')
end
```

#### System Detection

```lua
local notificationSystem = B1.getNotificationSystem() -- 'ox', 'zsx', 'qb', 'esx', 'native'
local progressBarSystem  = B1.getProgressBarSystem()
local skillCheckSystem   = B1.getSkillCheckSystem()

if notificationSystem == 'ox' then
    B1.log('info', 'Using ox_lib notifications')
end
```

### System-Specific Features

| System      | Notifications                        | Progress Bars                       | Skill Checks                  | Special                         |
| ----------- | ------------------------------------ | ----------------------------------- | ----------------------------- | ------------------------------- |
| **ox\_lib** | `exports.ox_lib:notify()`            | `exports.ox_lib:progressBar()`      | `exports.ox_lib:skillCheck()` | Full feature support            |
| **ZSX\_UI** | `exports['ZSX_UIV2']:Notification()` | `exports['ZSX_UIV2']:ProgressBar()` | ❌                             | Notification removal via serial |
| **QBCore**  | `QBCore.Functions.Notify()`          | `QBCore.Functions.Progressbar()`    | ❌                             | Framework-native                |
| **ESX**     | `ESX.ShowNotification()`             | `ESX.Progressbar()`                 | ❌                             | Framework-native                |
| **Native**  | `DrawNotification()`                 | ❌                                   | ❌                             | Basic fallback                  |

### API References

* **ZSX\_UI**: Notifications · ProgressBar
* **QBCore**: ProgressBar
* **ESX**: ProgressBar

---

## 🎒 Inventory Functions *(Server only)*

Server-side inventory management with **ox\_inventory** integration.

!!! warning "Requires `ox_inventory`"
Inventory functions are disabled if `ox_inventory` is not available.

### Functions

| Function                                             | Type                                                    | Description | Example                                         |
| ---------------------------------------------------- | ------------------------------------------------------- | ----------- | ----------------------------------------------- |
| `B1.inv.add(src, name, count?, metadata?, slot?)`    | `(number, string, number?, table?, number?) -> boolean` | Add item    | `B1.inv.add(1, 'bread', 5, {quality=100})`      |
| `B1.inv.remove(src, name, count?, metadata?, slot?)` | `(number, string, number?, table?, number?) -> boolean` | Remove item | `B1.inv.remove(1, 'bread', 2)`                  |
| `B1.inv.canCarry(src, name, count?, metadata?)`      | `(number, string, number?, table?) -> boolean`          | Can carry?  | `B1.inv.canCarry(1, 'bread', 10)`               |
| `B1.inv.get(src, name, metadata?)`                   | `(number, string, table?) -> table\|nil`                | Get item    | `B1.inv.get(1, 'bread')`                        |
| `B1.inv.search(src, searchType, items, metadata?)`   | `(number, string, table, table?) -> table`              | Search      | `B1.inv.search(1, 'slots', {'bread', 'water'})` |

#### Examples

```lua
B1.inv.add(source, 'bread', 5, {quality = 100})
B1.inv.remove(source, 'bread', 2)
local canCarry = B1.inv.canCarry(source, 'bread', 10)
local item = B1.inv.get(source, 'bread')
local items = B1.inv.search(source, 'slots', {'bread', 'water'})
```

---

## ⚙️ Configuration

Edit `config.lua` to customize behavior:

```lua
-- B1-lib Configuration
-- Core framework detection
Config.Core = 'auto' -- 'auto'|'qb-core'|'esx'

-- Notification system
Config.Notification = 'auto' -- 'auto'|'ox'|'zsx'|'esx'|'qb'|'native'

-- Progress bar system
Config.ProgressBar = 'auto' -- 'auto'|'ox'|'zsx'|'esx'|'qb'|'native'

-- Skill check system
Config.SkillCheck = 'auto' -- 'auto'|'ox'|'zsx'|'esx'|'qb'|'native'

-- Logging
Config.LogTag = '^3[b1-lib]^7'
Config.LogLevel = 'info' -- 'debug'|'info'|'warning'|'error'

-- Vehicle defaults
Config.VehicleDefaultSpawn = vector4(-42.4, -1098.3, 26.4, 70.0)

-- Notification settings
Config.NotificationDuration = 5000 -- ms
Config.NotificationPosition = 'top-right' -- 'top-left'|'top-right'|'bottom-left'|'bottom-right'|'center'

-- Progress bar settings
Config.ProgressBarDuration = 10000 -- ms
Config.ProgressBarPosition = 'bottom' -- 'top'|'bottom'|'center'

-- Skill check settings
Config.SkillCheckDuration = 5000 -- ms
Config.SkillCheckDifficulty = 'easy' -- 'easy'|'medium'|'hard'
```

### Configuration Options

| Option                        | Type      | Default                               | Description                        |
| ----------------------------- | --------- | ------------------------------------- | ---------------------------------- |
| `Config.Core`                 | `string`  | `'auto'`                              | Framework detection mode           |
| `Config.Notification`         | `string`  | `'auto'`                              | Notification system                |
| `Config.ProgressBar`          | `string`  | `'auto'`                              | Progress bar system                |
| `Config.SkillCheck`           | `string`  | `'auto'`                              | Skill check system                 |
| `Config.LogTag`               | `string`  | `'^3[b1-lib]^7'`                      | Log prefix                         |
| `Config.LogLevel`             | `string`  | `'info'`                              | Minimum log level                  |
| `Config.VehicleDefaultSpawn`  | `vector4` | `vector4(-42.4, -1098.3, 26.4, 70.0)` | Default vehicle spawn              |
| `Config.NotificationDuration` | `number`  | `5000`                                | Default notification duration (ms) |
| `Config.NotificationPosition` | `string`  | `'top-right'`                         | Notification position              |
| `Config.ProgressBarDuration`  | `number`  | `10000`                               | Default progress duration (ms)     |
| `Config.ProgressBarPosition`  | `string`  | `'bottom'`                            | Progress bar position              |
| `Config.SkillCheckDuration`   | `number`  | `5000`                                | Default skill duration (ms)        |
| `Config.SkillCheckDifficulty` | `string`  | `'easy'`                              | Default skill difficulty           |

---

## ❓ FAQ / Troubleshooting

### Missing Dependencies

| Dependency                  | Behavior                         | Solution                            |
| --------------------------- | -------------------------------- | ----------------------------------- |
| **oxmysql not found**       | DB/ORM disabled, warnings logged | Install **oxmysql**                 |
| **ox\_inventory not found** | Inventory helpers disabled       | Install **ox\_inventory**           |
| **ox\_lib not found**       | Falls back to core notifications | Install **ox\_lib** or rely on core |

### Framework Detection

* **Auto-detection** checks running `qb-core` or `esx` resources.
* **Manual override**: set `Config.Core = 'qb-core'` or `'esx'`.
* **No framework**: functions return safe defaults and warn.

### Common Issues

| Issue                        | Cause                  | Solution                                  |
| ---------------------------- | ---------------------- | ----------------------------------------- |
| Player functions not working | Framework not started  | Ensure QBCore/ESX is running              |
| Database errors              | oxmysql not configured | Check oxmysql connection                  |
| Vehicle spawning fails       | Invalid model name     | Verify vehicle model exists               |
| Notifications not showing    | ox\_lib not installed  | Use core notifications or install ox\_lib |

### Performance Notes

* ✅ DB functions are await-style (non-blocking)
* ✅ Vehicle data cached on server start
* ✅ Logger respects level settings
* ✅ Graceful fallbacks for missing dependencies

---

## 📚 Complete Examples

### Basic Resource Example

```lua
-- client.lua
-- Get B1 object (recommended approach)
local B1 = exports['b1-lib']:getB1Object()

local function onPlayerSpawn()
    local playerData = B1.getPlayerData()
    if playerData then
        B1.notify('Welcome back, ' .. playerData.charinfo.firstname, 'success')
    end
end

RegisterNetEvent('QBCore:Client:OnPlayerLoaded', onPlayerSpawn)
RegisterNetEvent('esx:playerLoaded', onPlayerSpawn)
```

```lua
-- server.lua
-- Get B1 object (recommended approach)
local B1 = exports['b1-lib']:getB1Object()

local function giveReward(source, amount)
    local success = B1.addMoney(source, 'bank', amount, 'Daily reward')
    if success then
        B1.log('info', 'Gave reward to player', { ctx = { source = source, amount = amount } })
    end
end

RegisterCommand('reward', function(source, args)
    local amount = tonumber(args[1]) or 1000
    giveReward(source, amount)
end, false)
```

### Advanced ORM Example

```lua
-- server.lua
-- Get B1 object (recommended approach)
local B1 = exports['b1-lib']:getB1Object()

local Players = B1.orm.define('players', 'id')

-- Create player record
local function createPlayer(citizenId, name, job)
    return Players:insert({
        citizenid = citizenId,
        name = name,
        job = job,
        level = 1,
        created_at = os.time()
    })
end

-- Update player level
local function levelUpPlayer(citizenId)
    local player = Players:findAll('citizenid = ?', {citizenId})[1]
    if player then
        player.level = player.level + 1
        player:save()
        B1.log('info', 'Player leveled up', { ctx = { citizenId = citizenId, level = player.level } })
    end
end
```

---

## 🎯 Summary

B1-lib provides a **clean, normalized API** that works with both QBCore and ESX frameworks, making it easy to write **framework-agnostic code**. With comprehensive logging, database ORM, and graceful fallbacks, it's the perfect library for modern FiveM development.

**Key Features**

* 🔄 **Framework Agnostic** — QBCore and ESX
* 🗄️ **Lightweight ORM** — 4DaysORM-inspired DB layer
* 📝 **Advanced Logging** — Colored output + traceback
* 🎨 **Unified UI System** — Notifications / progress / skill checks
* 🚗 **Vehicle Management** — Spawn, plate, labels
* 👤 **Player Functions** — Normalized data + money ops
* 🎒 **Inventory Integration** — ox\_inventory helpers
* ⚡ **Performance** — Await-style, non-blocking
* 🔧 **Simple Config** — `Config.[value] =` format
