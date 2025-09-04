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
```

!!! info "Optional but recommended"
    - **oxmysql**: enables the DB and ORM features
    - **ox_inventory**: enables inventory helpers
    - **ox_lib**: enables unified notifications / progress / skill checks

---

## ⚡ Quick Start

=== "Method 1 · Get the entire B1 object (Recommended)"

```lua
-- Get the complete B1 object
local B1 = exports['b1-lib']:getB1Object()

-- Use any B1 function
B1.log('info', 'Hello from b1-lib!')
B1.notify('Welcome to the server!', 'success')
B1.progressBar('Loading...', 5000)
```

=== "Method 2 · Individual exports"

```lua
-- Use specific exports
exports['b1-lib']:log('info', 'Hello from b1-lib!')
exports['b1-lib']:notify('Welcome to the server!', 'success')
exports['b1-lib']:progressBar('Loading...', 5000)
```

=== "Method 3 · Mixed approach"

```lua
-- Get B1 object for most functions
local B1 = exports['b1-lib']:getB1Object()

-- Use individual exports for specific cases
local success = exports['b1-lib']:skillCheck('hard')
```

!!! tip "Why Method 1?"
    - ✅ Clean, readable code
    - ✅ Access to all B1 functions
    - ✅ Better performance (single export call)
    - ✅ IDE autocomplete support

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


## 📝 Logger _(Shared: client & server)_

Colored, boxed output with traceback support and calling resource detection.

### Functions

| Function | Type | Description |
|----------|------|-------------|
| `B1.log(level, message, opts?)` | `(string\|number, any, table?) -> void` | Log a message with specified level |
| `B1.setLogLevel(level)` | `(string) -> void` | Set the minimum log level |
| `B1.getLogLevel()` | `() -> string` | Get current log level |

### Log Levels

| Level | Numeric | Color | Description |
|-------|---------|-------|-------------|
| `debug` | `1` | 🔵 Blue | Debug information |
| `info` | `2` | 🟢 Green | General information |
| `warning` | `3` | 🟡 Yellow | Warning messages |
| `error` | `4` | 🔴 Red | Error messages with traceback |

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

## 🛠️ Utils _(Shared)_

Common utilities for strings, numbers, and system detection.

### Functions

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.randomStr(length?)` | `(number?) -> string` | Generate random string | `B1.randomStr(10)` → `"aB3xY9mK2p"` |
| `B1.randomInt(length?)` | `(number?) -> number` | Generate random integer | `B1.randomInt(6)` → `123456` |
| `B1.splitStr(str, delimiter?)` | `(string, string?) -> table` | Split string by delimiter | `B1.splitStr('a,b,c', ',')` → `{"a", "b", "c"}` |
| `B1.sanitizeString(s, charset?)` | `(string, string?) -> string` | Keep only specified chars | `B1.sanitizeString('Hello!@#$')` → `"Hello"` |
| `B1.removeChars(s, charset?)` | `(string, string?) -> string` | Remove specified chars | `B1.removeChars('Hello!@#$')` → `"Hello"` |
| `B1.trim(s)` | `(string) -> string` | Remove whitespace | `B1.trim('  hello  ')` → `"hello"` |
| `B1.firstToUpper(s)` | `(string) -> string` | Capitalize first letter | `B1.firstToUpper('hello')` → `"Hello"` |
| `B1.round(num, decimals?)` | `(number, number?) -> number` | Round to decimals | `B1.round(3.14159, 2)` → `3.14` |
| `B1.getCoreName()` | `() -> string\|nil` | Get framework name | `"qb-core"` or `"esx"` |
| `B1.getInventoryName()` | `() -> string` | Get inventory system | `"ox"`, `"qb"`, or `"esx"` |
| `B1.getCoreObject()` | `() -> table\|nil` | Get core object | Core framework object |
| `B1.getNotificationSystem()` | `() -> string` | Get notification system | `"ox"`, `"zsx"`, `"qb"`, `"esx"`, or `"native"` |
| `B1.getProgressBarSystem()` | `() -> string` | Get progress bar system | `"ox"`, `"zsx"`, `"qb"`, `"esx"`, or `"native"` |
| `B1.getSkillCheckSystem()` | `() -> string` | Get skill check system | `"ox"`, `"zsx"`, `"qb"`, `"esx"`, or `"native"` |

#### Examples

```lua
-- Random generation
local randomStr = B1.randomStr(10) -- "aB3xY9mK2p"
local randomInt = B1.randomInt(6)  -- 123456

-- String manipulation
local parts = B1.splitStr('apple,banana,cherry', ',') 
-- Returns: {"apple", "banana", "cherry"}

local clean = B1.sanitizeString('Hello!@#$', 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz ') 
-- Returns: "Hello "

local trimmed = B1.trim('  hello world  ') 
-- Returns: "hello world"

local capitalized = B1.firstToUpper('hello world') 
-- Returns: "Hello world"

-- Number formatting
local rounded = B1.round(3.14159, 2) 
-- Returns: 3.14

-- Framework detection
local coreName = B1.getCoreName() 
-- Returns: "qb-core" or "esx" or nil

local inventoryName = B1.getInventoryName() 
-- Returns: "ox", "qb", "esx", or "unknown"

local coreObject = B1.getCoreObject() 
-- Returns: Core framework object or nil

-- UI system detection
local notificationSystem = B1.getNotificationSystem()
-- Returns: "ox", "zsx", "qb", "esx", or "native"

local progressBarSystem = B1.getProgressBarSystem()
-- Returns: "ox", "zsx", "qb", "esx", or "native"

local skillCheckSystem = B1.getSkillCheckSystem()
-- Returns: "ox", "zsx", "qb", "esx", or "native"
```

---

## 🗄️ Database _(Server only)_

Lightweight ORM with oxmysql adapter. All functions are await-style and non-blocking.

!!! warning "Requires oxmysql"
    Database functions will be disabled if oxmysql is not available.

### Basic Queries

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.db.query(sql, params?)` | `(string, table?) -> table` | Execute query, return rows | `B1.db.query('SELECT * FROM users')` |
| `B1.db.single(sql, params?)` | `(string, table?) -> table\|nil` | Get single row | `B1.db.single('SELECT * FROM users WHERE id = ?', {1})` |
| `B1.db.scalar(sql, params?)` | `(string, table?) -> any\|nil` | Get scalar value | `B1.db.scalar('SELECT COUNT(*) FROM users')` |
| `B1.db.insert(sql, params?)` | `(string, table?) -> number` | Insert record, return ID | `B1.db.insert('INSERT INTO users (name) VALUES (?)', {'John'})` |
| `B1.db.update(sql, params?)` | `(string, table?) -> number` | Update records, return affected | `B1.db.update('UPDATE users SET name = ? WHERE id = ?', {'Jane', 1})` |

#### Examples

```lua
-- Query multiple rows
local players = B1.db.query('SELECT * FROM users WHERE job = ?', {'police'})
-- Returns: {{id=1, name="John", job="police"}, ...}

-- Get single row
local user = B1.db.single('SELECT * FROM users WHERE id = ?', {1})
-- Returns: {id=1, name="John", job="police"} or nil

-- Get scalar value
local count = B1.db.scalar('SELECT COUNT(*) FROM users')
-- Returns: 42

-- Insert record
local insertId = B1.db.insert('INSERT INTO users (name, job) VALUES (?, ?)', {'John', 'police'})
-- Returns: 123 (inserted ID)

-- Update record
local affected = B1.db.update('UPDATE users SET job = ? WHERE id = ?', {'admin', 1})
-- Returns: 1 (affected rows)
```

### ORM Models

| Function | Type | Description |
|----------|------|-------------|
| `B1.orm.define(tableName, primaryKey?)` | `(string, string?) -> Model` | Create a new model |
| `Model:find(id)` | `(any) -> Row\|nil` | Find single record by ID |
| `Model:findAll(where?, params?)` | `(string?, table?) -> table` | Find all records with optional WHERE |
| `Model:insert(data)` | `(table) -> number` | Insert new record |
| `Model:update(id, data)` | `(any, table) -> boolean` | Update record by ID |
| `Model:delete(id)` | `(any) -> boolean` | Delete record by ID |
| `Row:save()` | `() -> boolean` | Save changes to database |
| `Row:delete()` | `() -> boolean` | Delete this record |

#### Examples

```lua
-- Define a model
local Users = B1.orm.define('users', 'id')
-- Primary key defaults to 'id' if not specified

-- Find single record
local user = Users:find(1)
if user then
    print(user.name) -- Access fields directly
    user.name = 'New Name'
    user:save() -- Save changes
end

-- Find all records
local allUsers = Users:findAll()
-- Returns: {{id=1, name="John"}, {id=2, name="Jane"}}

-- Find with WHERE clause
local policeUsers = Users:findAll('job = ?', {'police'})
-- Returns: {{id=1, name="John", job="police"}}

-- Insert new record
local newId = Users:insert({
    name = 'Jane',
    job = 'police',
    level = 5
})
-- Returns: 123 (inserted ID)

-- Update record
local success = Users:update(1, {job = 'admin', level = 10})
-- Returns: true if successful

-- Delete record
local success = Users:delete(1)
-- Returns: true if successful

-- Row methods (on records returned by find/findAll)
local user = Users:find(1)
user.job = 'admin'
user:save() -- Save changes to database
user:delete() -- Delete this record from database
```

---

## 👤 Player Functions

Normalized player management functions that work with both QBCore and ESX frameworks.

### Server-side _(Server only)_

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.getCorePlayer(source)` | `(number) -> table\|nil` | Get core player object | `B1.getCorePlayer(1)` |
| `B1.getPlayerByIdentifier(identifier)` | `(string) -> table\|nil` | Get player by identifier | `B1.getPlayerByIdentifier('ABC123')` |
| `B1.getOfflinePlayerByIdentifier(identifier)` | `(string) -> table\|nil` | Get offline player | `B1.getOfflinePlayerByIdentifier('ABC123')` |
| `B1.getPlayers()` | `() -> table` | Get all online players | `B1.getPlayers()` |
| `B1.addMoney(source, type, amount, reason?)` | `(number, string, number, string?) -> boolean` | Add money to player | `B1.addMoney(1, 'cash', 1000, 'Reward')` |
| `B1.removeMoney(source, type, amount, reason?)` | `(number, string, number, string?) -> boolean` | Remove money from player | `B1.removeMoney(1, 'bank', 500, 'Purchase')` |
| `B1.getMoney(source, type)` | `(number, string) -> number` | Get player money | `B1.getMoney(1, 'cash')` |
| `B1.setMoney(source, type, amount)` | `(number, string, number) -> boolean` | Set player money | `B1.setMoney(1, 'bank', 5000)` |
| `B1.getLicences(source)` | `(number) -> table` | Get all licenses | `B1.getLicences(1)` |
| `B1.getLicence(source, type)` | `(number, string) -> boolean` | Check specific license | `B1.getLicence(1, 'driver')` |
| `B1.addLicence(source, type)` | `(number, string) -> boolean` | Add license | `B1.addLicence(1, 'weapon')` |
| `B1.removeLicence(source, type)` | `(number, string) -> boolean` | Remove license | `B1.removeLicence(1, 'driver')` |
| `B1.getCitizenId(source)` | `(number) -> string\|nil` | Get citizen ID | `B1.getCitizenId(1)` |
| `B1.getPlayerName(source)` | `(number) -> string` | Get player name | `B1.getPlayerName(1)` |

#### Examples

```lua
-- Get core player object
local player = B1.getCorePlayer(source)
-- Returns: Core player object or nil

-- Get player by identifier
local player = B1.getPlayerByIdentifier('ABC123')
-- Returns: Player object or nil

-- Get offline player
local offlinePlayer = B1.getOfflinePlayerByIdentifier('ABC123')
-- Returns: Offline player data or nil

-- Get all players
local players = B1.getPlayers()
-- Returns: {{source=1, name="John"}, {source=2, name="Jane"}}

-- Money management
B1.addMoney(source, 'cash', 1000, 'Reward')
B1.removeMoney(source, 'bank', 500, 'Purchase')
local cash = B1.getMoney(source, 'cash')
B1.setMoney(source, 'bank', 5000)

-- License management
local licenses = B1.getLicences(source)
-- Returns: {driver=true, weapon=false, ...}

local hasLicense = B1.getLicence(source, 'driver')
-- Returns: true or false

B1.addLicence(source, 'weapon')
B1.removeLicence(source, 'driver')

-- Identity
local citizenId = B1.getCitizenId(source)
-- Returns: "ABC123" or nil

local playerName = B1.getPlayerName(source)
-- Returns: "John Doe" or "Unknown"
```

### Client-side _(Client only)_

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.getCorePlayerData()` | `() -> table\|nil` | Get core player data | `B1.getCorePlayerData()` |
| `B1.getPlayerData()` | `() -> table\|nil` | Get normalized player data | `B1.getPlayerData()` |
| `B1.getPlayerJob()` | `() -> table\|nil` | Get player job info | `B1.getPlayerJob()` |
| `B1.getPlayerGang()` | `() -> table\|nil` | Get player gang info | `B1.getPlayerGang()` |
| `B1.getPlayerMoney(type)` | `(string) -> number` | Get player money | `B1.getPlayerMoney('cash')` |
| `B1.getPlayerName()` | `() -> string` | Get player name | `B1.getPlayerName()` |
| `B1.getCitizenId()` | `() -> string\|nil` | Get citizen ID (callback) | `B1.getCitizenId()` |
| `B1.notify(message, type?, length?)` | `(string, string?, number?) -> void` | Show notification | `B1.notify('Hello!', 'success')` |

#### Examples

```lua
-- Get player data
local playerData = B1.getPlayerData()
-- Returns: {
--   identifier = "ABC123",
--   job = {name="police", label="Police", grade=1, isboss=false, onduty=true},
--   money = {cash=1000, bank=5000},
--   bank = 5000,
--   metadata = {...},
--   charinfo = {firstname="John", lastname="Doe"}
-- }

-- Get specific data
local job = B1.getPlayerJob()
-- Returns: {name="police", label="Police", grade=1, isboss=false, onduty=true}

local gang = B1.getPlayerGang()
-- Returns: QB gang data or ESX job data

local cash = B1.getPlayerMoney('cash')
-- Returns: 1000

local bank = B1.getPlayerMoney('bank')
-- Returns: 5000

local name = B1.getPlayerName()
-- Returns: "John Doe"

local citizenId = B1.getCitizenId()
-- Returns: "ABC123" (via server callback)

-- Notifications
B1.notify('Hello world!', 'success', 5000)
B1.notify('Warning message', 'warning')
B1.notify('Error occurred', 'error')
```

### Money Types

| Type | QBCore | ESX | Description |
|------|--------|-----|-------------|
| `cash` | ✅ | ✅ | Cash money |
| `bank` | ✅ | ✅ | Bank account |
| `black` | ❌ | ✅ | Black money (ESX only) |

---

## 🚗 Vehicle Functions

Vehicle management functions for spawning, plate detection, and label retrieval.

### Server-side _(Server only)_

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.spawnVehicle(source, model, coords?, warp?)` | `(number, string, vector4?, boolean?) -> number\|nil` | Spawn vehicle for player | `B1.spawnVehicle(1, 'adder', vector4(0,0,0,0), true)` |
| `B1.getPlate(netId)` | `(number) -> string\|nil` | Get vehicle plate | `B1.getPlate(123)` |
| `B1.getVehicleLabel(model)` | `(string) -> string` | Get vehicle display name | `B1.getVehicleLabel('adder')` |

#### Examples

```lua
-- Spawn vehicle for player
local netId = B1.spawnVehicle(source, 'adder', vector4(0, 0, 0, 0), true)
-- Returns: 123 (network ID) or nil

-- Get vehicle plate
local plate = B1.getPlate(netId)
-- Returns: "ABC123" or nil

-- Get vehicle label
local label = B1.getVehicleLabel('adder')
-- Returns: "Truffade Adder" or "adder"
```

### Client-side _(Client only)_

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.spawnVehicle(model, cb?, coords?, isnetworked?, teleportInto?)` | `(string, function?, vector4?, boolean?, boolean?) -> number\|nil` | Spawn vehicle | `B1.spawnVehicle('adder', function(id) end)` |
| `B1.getPlate(vehicle)` | `(number\|entity) -> string\|nil` | Get vehicle plate | `B1.getPlate(vehicle)` |
| `B1.getVehicleLabel(model)` | `(string) -> string` | Get vehicle label (callback) | `B1.getVehicleLabel('adder')` |

#### Examples

```lua
-- Spawn vehicle
local netId = B1.spawnVehicle('adder', function(vehicle)
    print('Vehicle spawned:', vehicle)
end, vector4(0, 0, 0, 0), true, true)
-- Returns: 123 (network ID) or nil

-- Get vehicle plate
local plate = B1.getPlate(vehicle) -- or netId
-- Returns: "ABC123" or nil

-- Get vehicle label
local label = B1.getVehicleLabel('adder')
-- Returns: "Truffade Adder" (via server callback)
```

### Vehicle Spawning

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `model` | `string` | - | Vehicle model name (e.g., 'adder', 'zentorno') |
| `coords` | `vector4` | `Config.Vehicle.DefaultSpawn` | Spawn coordinates (x, y, z, heading) |
| `warp`/`teleportInto` | `boolean` | `true` | Whether to teleport player into vehicle |
| `isnetworked` | `boolean` | `true` | Whether vehicle should be networked |

---

## 🎨 UI Functions _(Client only)_

Client-side UI functions for notifications, progress bars, and skill checks across different systems.

!!! warning "Important"
    UI functions are client-side only as they require player interaction. To send UI elements from server to client, use `B1.notifyServer()` or `TriggerClientEvent` with the appropriate client-side function.

### Supported Systems

| System | Notifications | Progress Bars | Skill Checks |
|--------|---------------|---------------|--------------|
| **ox_lib** | ✅ | ✅ | ✅ |
| **ZSX_UI** | ✅ | ✅ | ❌ |
| **QBCore** | ✅ | ✅ | ❌ |
| **ESX** | ✅ | ✅ | ❌ |
| **Native** | ✅ | ❌ | ❌ |

!!! note "Note"
    - ZSX_UI ProgressBar now fully supported with correct API
    - QBCore and ESX don't have built-in skill check systems - fall back to ox_lib if available
    - ZSX_UI notifications use `exports['ZSX_UIV2']:Notification()` with serial return for removal

### Functions

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.notify(message, type?, duration?, position?)` | `(string, string?, number?, string?) -> any` | Show notification | `B1.notify('Hello!', 'success', 5000, 'top-right')` |
| `B1.progressBar(label, duration?, options?)` | `(string, number?, table?) -> boolean` | Show progress bar | `B1.progressBar('Loading...', 10000, {onFinish=function() end})` |
| `B1.skillCheck(difficulty?, options?)` | `(string?, table?) -> boolean` | Show skill check | `B1.skillCheck('easy', {duration=5000})` |
| `B1.removeNotification(serial)` | `(any) -> void` | Remove notification (ZSX_UI only) | `B1.removeNotification(serial)` |

#### Notification Examples

```lua
-- Basic notification (client-side only)
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
-- Remove notification later
B1.removeNotification(serial)
```

#### Server → Client UI

```lua
-- Server-side: Send notification to specific player
B1.notifyServer(source, 'Server notification', 'info', 5000)

-- Server-side: Send notification to all players
B1.notifyServer(-1, 'Global notification', 'success', 3000)

-- Server-side: Using different notification types
B1.notifyServer(source, 'Success message', 'success', 5000)
B1.notifyServer(source, 'Error message', 'error', 5000)
B1.notifyServer(source, 'Warning message', 'warning', 5000)
B1.notifyServer(source, 'Info message', 'info', 5000)

-- Server-side: Using numeric types
B1.notifyServer(source, 'Numeric success', 2, 5000) -- 2 = success
B1.notifyServer(source, 'Numeric error', 3, 5000)   -- 3 = error
```

#### Progress Bar Examples

```lua
-- Basic progress bar
B1.progressBar('Loading...', 10000)

-- With callbacks
B1.progressBar('Processing...', 5000, {
    onFinish = function()
        B1.notify('Process completed!', 'success')
    end,
    onCancel = function()
        B1.notify('Process cancelled', 'warning')
    end
})

-- With animation
B1.progressBar('Repairing vehicle...', 15000, {
    anim = {
        dict = 'mini@repair',
        clip = 'fixing_a_ped'
    },
    prop = {
        model = 'prop_tool_wrench',
        bone = 57005,
        pos = vector3(0.1, 0.0, 0.0),
        rot = vector3(0.0, 0.0, 0.0)
    },
    disable = {
        car = true,
        move = true,
        combat = true,
        mouse = false
    }
})
```

#### Skill Check Examples

```lua
-- Basic skill check
local success = B1.skillCheck('easy')

-- With custom options
local success = B1.skillCheck('hard', {
    duration = 8000,
    position = 'center',
    disable = {
        car = true,
        move = true,
        combat = true,
        mouse = false
    }
})

-- Handle result
if success then
    B1.notify('Skill check passed!', 'success')
else
    B1.notify('Skill check failed!', 'error')
end
```

#### System Detection

```lua
-- Get current systems
local notificationSystem = B1.getNotificationSystem() -- 'ox', 'zsx', 'qb', 'esx', 'native'
local progressBarSystem = B1.getProgressBarSystem()   -- 'ox', 'zsx', 'qb', 'esx', 'native'
local skillCheckSystem = B1.getSkillCheckSystem()     -- 'ox', 'zsx', 'qb', 'esx', 'native'

-- Check if specific system is available
if B1.getNotificationSystem() == 'ox' then
    B1.log('info', 'Using ox_lib notifications')
end
```

#### System-Specific Features

| System | Notifications | Progress Bars | Skill Checks | Special Features |
|--------|---------------|---------------|--------------|------------------|
| **ox_lib** | `exports.ox_lib:notify()` | `exports.ox_lib:progressBar()` | `exports.ox_lib:skillCheck()` | Full feature support |
| **ZSX_UI** | `exports['ZSX_UIV2']:Notification()` | `exports['ZSX_UIV2']:ProgressBar()` | ❌ | Notification removal with serial |
| **QBCore** | `QBCore.Functions.Notify()` | `QBCore.Functions.Progressbar()` | ❌ | Framework integration |
| **ESX** | `ESX.ShowNotification()` | `ESX.Progressbar()` | ❌ | Framework integration |
| **Native** | `DrawNotification()` | ❌ | ❌ | Basic fallback |

#### API References

- **ZSX_UI**: [Notifications](https://zsx-development.gitbook.io/docs/user-interface-v2/exports/interfaces/notifications) | [ProgressBar](https://zsx-development.gitbook.io/docs/user-interface-v2/exports/interfaces/progress-bar)
- **QBCore**: [ProgressBar](https://docs.qbcore.org/qbcore-documentation/qb-core/client-function-reference#qbcore.functions.progressbar)
- **ESX**: [ProgressBar](https://docs.esx-framework.org/en/esx_core/esx_progressbar)


---

## 🎒 Inventory Functions _(Server only)_

Server-side inventory management with ox_inventory integration.

!!! warning "Requires ox_inventory"
    Inventory functions will be disabled if ox_inventory is not available.

### Functions

| Function | Type | Description | Example |
|----------|------|-------------|---------|
| `B1.inv.add(src, name, count?, metadata?, slot?)` | `(number, string, number?, table?, number?) -> boolean` | Add item to inventory | `B1.inv.add(1, 'bread', 5, {quality=100})` |
| `B1.inv.remove(src, name, count?, metadata?, slot?)` | `(number, string, number?, table?, number?) -> boolean` | Remove item from inventory | `B1.inv.remove(1, 'bread', 2)` |
| `B1.inv.canCarry(src, name, count?, metadata?)` | `(number, string, number?, table?) -> boolean` | Check if player can carry | `B1.inv.canCarry(1, 'bread', 10)` |
| `B1.inv.get(src, name, metadata?)` | `(number, string, table?) -> table\|nil` | Get item from inventory | `B1.inv.get(1, 'bread')` |
| `B1.inv.search(src, searchType, items, metadata?)` | `(number, string, table, table?) -> table` | Search inventory | `B1.inv.search(1, 'slots', {'bread', 'water'})` |

#### Examples

```lua
-- Add item
B1.inv.add(source, 'bread', 5, {quality = 100})
-- Returns: true if successful

-- Remove item
B1.inv.remove(source, 'bread', 2)
-- Returns: true if successful

-- Check if player can carry
local canCarry = B1.inv.canCarry(source, 'bread', 10)
-- Returns: true or false

-- Get item
local item = B1.inv.get(source, 'bread')
-- Returns: {name="bread", count=5, metadata={...}} or nil

-- Search inventory
local items = B1.inv.search(source, 'slots', {'bread', 'water'})
-- Returns: {{name="bread", count=5}, {name="water", count=2}}
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
Config.NotificationDuration = 5000 -- Default notification duration in ms
Config.NotificationPosition = 'top-right' -- 'top-left'|'top-right'|'bottom-left'|'bottom-right'|'center'

-- Progress bar settings
Config.ProgressBarDuration = 10000 -- Default progress bar duration in ms
Config.ProgressBarPosition = 'bottom' -- 'top'|'bottom'|'center'

-- Skill check settings
Config.SkillCheckDuration = 5000 -- Default skill check duration in ms
Config.SkillCheckDifficulty = 'easy' -- 'easy'|'medium'|'hard'
```

#### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `Config.Core` | `string` | `'auto'` | Framework detection mode |
| `Config.Notification` | `string` | `'auto'` | Notification system |
| `Config.ProgressBar` | `string` | `'auto'` | Progress bar system |
| `Config.SkillCheck` | `string` | `'auto'` | Skill check system |
| `Config.LogTag` | `string` | `'^3[b1-lib]^7'` | Log message prefix |
| `Config.LogLevel` | `string` | `'info'` | Minimum log level |
| `Config.VehicleDefaultSpawn` | `vector4` | `vector4(-42.4, -1098.3, 26.4, 70.0)` | Default vehicle spawn location |
| `Config.NotificationDuration` | `number` | `5000` | Default notification duration (ms) |
| `Config.NotificationPosition` | `string` | `'top-right'` | Notification position |
| `Config.ProgressBarDuration` | `number` | `10000` | Default progress bar duration (ms) |
| `Config.ProgressBarPosition` | `string` | `'bottom'` | Progress bar position |
| `Config.SkillCheckDuration` | `number` | `5000` | Default skill check duration (ms) |
| `Config.SkillCheckDifficulty` | `string` | `'easy'` | Default skill check difficulty |

---

## ❓ FAQ / Troubleshooting

#### Missing Dependencies

| Dependency | Behavior | Solution |
|------------|----------|----------|
| **oxmysql not found** | Database functions disabled, warnings logged | Install oxmysql |
| **ox_inventory not found** | Inventory functions disabled, warnings logged | Install ox_inventory |
| **ox_lib not found** | Falls back to core notifications | Install ox_lib or use core notifications |

#### Framework Detection

- **Auto-detection** checks running `qb-core` or `esx` resources.
- **Manual override**: set `Config.Core = 'qb-core'` or `'esx'`.
- **No framework**: functions return safe defaults and warn.

#### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Player functions not working** | Framework not started | Ensure QBCore/ESX is running |
| **Database errors** | oxmysql not configured | Check oxmysql connection |
| **Vehicle spawning fails** | Invalid model name | Verify vehicle model exists |
| **Notifications not showing** | ox_lib not installed | Use core notifications or install ox_lib |

#### Performance Notes

- ✅ DB functions are await-style (non-blocking)
- ✅ Vehicle data cached on server start
- ✅ Logger respects level settings
- ✅ Graceful fallbacks for missing dependencies

---

## 📚 Complete Examples

#### Basic Resource Example

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

#### Advanced ORM Example

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

- 🔄 **Framework Agnostic** — QBCore and ESX
- 🗄️ **Lightweight ORM** — 4DaysORM-inspired DB layer
- 📝 **Advanced Logging** — Colored output + traceback
- 🎨 **Unified UI System** — Notifications / progress / skill checks
- 🚗 **Vehicle Management** — Spawn, plate, labels
- 👤 **Player Functions** — Normalized data + money ops
- 🎒 **Inventory Integration** — ox_inventory helpers
- ⚡ **Performance** — Await-style, non-blocking
- 🔧 **Simple Config** — `Config.[value] =` format
