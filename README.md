﻿# ⚔️ SpacePvP-API

![Java 17](https://img.shields.io/badge/Java-17%2B-orange?style=flat)
[![JitPack](https://jitpack.io/v/Klaidi4YT/SpacePvP-API.svg?style=flat)](https://jitpack.io/#Klaidi4YT/SpacePvP-API)
[![SpigotMC](https://img.shields.io/badge/SpigotMC-Resource-orange?style=flat&logo=spigotmc)](https://www.spigotmc.org/resources/spacepvp.124417/)

**SpacePvP-API** is the comprehensive developer interface for the **SpacePvP** plugin.
It allows you to deeply integrate PvP mechanics, manipulate player statistics, manage queues, control arena states, and listen to custom game events.

## 📝 Requirements

* **Java 17** or newer
* **SpacePvP** plugin installed and running on the server

---

## 📦 Installation

This library is hosted on [JitPack](https://jitpack.io).

### Step 1. Add the JitPack repository

**Maven** (`pom.xml`):
```xml
<repositories>
    <repository>
        <id>jitpack.io</id>
        <url>https://jitpack.io</url>
    </repository>
</repositories>
```

**Gradle Groovy** (`build.gradle`):
```groovy
repositories {
    mavenCentral()
    maven { url 'https://jitpack.io' }
}
```

**Gradle Kotlin** (`build.gradle.kts`):
```kotlin
repositories {
    mavenCentral()
    maven { url = uri("https://jitpack.io") }
}
```

---

### Step 2. Add the dependency

Replace `Tag` with the latest release version (e.g. `1.0.0`).

**Maven** (`pom.xml`):
```xml
<dependency>
    <groupId>com.github.klaidi4dev</groupId>
    <artifactId>SpacePvP-API</artifactId>
    <version>Tag</version>
    <scope>provided</scope>
</dependency>
```

**Gradle Groovy** (`build.gradle`):
```groovy
dependencies {
    compileOnly 'com.github.klaidi4dev:SpacePvP-API:Tag'
}
```

**Gradle Kotlin** (`build.gradle.kts`):
```kotlin
dependencies {
    compileOnly("com.github.klaidi4dev:SpacePvP-API:Tag")
}
```

---

## ⚡ API Initialization

Before using any methods, you must obtain the instance of `SpacePvPProvider`.
**Always** check if the API is registered to avoid errors if the core plugin is missing.

```java
import dev.ua.klaidi4_.spacepvpapi.SpacePvPAPI;
import dev.ua.klaidi4_.spacepvpapi.SpacePvPProvider;

public class MyPlugin extends JavaPlugin {

    private SpacePvPProvider api;

    @Override
    public void onEnable() {
        if (!SpacePvPAPI.isRegistered()) {
            getLogger().severe("SpacePvP not found! Disabling plugin...");
            getServer().getPluginManager().disablePlugin(this);
            return;
        }

        // Initialize the API provider
        this.api = SpacePvPAPI.get();
        getLogger().info("Hooked into SpacePvP successfully!");
    }
}
```

---
## 📊 StatsManager (Statistics)

Methods for retrieving and modifying player statistics.
**Access:** `api.getStatsManager()`

| Return Type | Method | Description |
| :--- | :--- | :--- |
| `int` | `getWins(UUID uuid)` | Gets the total wins of a player. |
| `int` | `getLosses(UUID uuid)` | Gets the total losses of a player. |
| `int` | `getCurrentWinStreak(UUID uuid)` | Gets the current active win streak. |
| `double` | `getKDRadio(UUID uuid)` | Gets the Kill/Death ratio. |
| `int` | `getPoints(UUID uuid)` | Gets the current points (Elo). |
| `void` | `addPoints(UUID uuid, int amount)` | Adds points to a player (use negative to subtract). |
| `void` | `removePoints(UUID uuid, int amount)` | Removes points from a player. |
| `void` | `setPoints(UUID uuid, int amount)` | Sets the exact amount of points. |
| `void` | `setWins(UUID uuid, int amount)` | Sets the exact amount of wins. |
| `void` | `setLosses(UUID uuid, int amount)` | Sets the exact amount of losses. |
| `void` | `setCurrentWinStreak(UUID uuid, int amount)` | Sets the win streak manually. |
| `void` | `resetPoints(UUID uuid)` | Resets points to the default value. |

### Example Usage:
```java
// Add points
api.getStatsManager().addPoints(player.getUniqueId(), 25);
```

---

## ⚔️ DuelManager (Default 1vs1)

Methods for the Default 1vs1 queue and fight logic.
**Access:** `api.getDuelManager()`

| Return Type | Method | Description |
| :--- | :--- | :--- |
| `boolean` | `isInDefaultQueue(UUID uuid)` | Checks if player is in the Default Queue. |
| `boolean` | `isInDefaultCountdown(UUID uuid)` | Checks if the countdown is running. |
| `boolean` | `isInDefaultFight(UUID uuid)` | Checks if the fight is active. |
| `UUID` | `getDefaultOpponent(UUID uuid)` | Gets the opponent's UUID (returns `null` if none). |

---

## 🏟️ ArenaManager (Arena 1vs1)

Methods for Arena fights, queues, and **configuration/setup** data.
**Access:** `api.getArenaManager()`

### Logic & State
| Return Type | Method | Description |
| :--- | :--- | :--- |
| `boolean` | `hasFreeArena()` | Checks if there is at least one arena with status READY. |
| `boolean` | `isInArenaQueue(UUID uuid)` | Checks if player is in the Arena Queue. |
| `boolean` | `isInArenaCountdown(UUID uuid)` | Checks if the arena countdown is running. |
| `boolean` | `isInArenaFight(UUID uuid)` | Checks if the arena fight is active. |
| `String` | `getCurrentArenaName(UUID uuid)` | Gets the name of the current arena. |
| `UUID` | `getArenaOpponent(UUID uuid)` | Gets the opponent in the arena (if applicable). |

### Configuration & Data
| Return Type | Method | Description |
| :--- | :--- | :--- |
| `List<String>` | `getArenaNames()` | Returns a list of all arena names. |
| `boolean` | `isArenaReady(String name)` | Checks if an arena is setup (Status READY). |
| `boolean` | `isArenaConfirmed(String name)` | Checks if the setup is confirmed. |
| `ApiArenaStatus` | `getArenaStatus(String name)` | Gets current status (READY, BUSY, OPENING, REGEN). |
| `Location` | `getArenaLobby()` | Gets the global Arena Lobby location. |
| `Location` | `getArenaPos1(String name)` | Gets the first corner of the arena region. |
| `Location` | `getArenaPos2(String name)` | Gets the second corner of the arena region. |
| `Location` | `getArenaCenter(String name)` | Gets the center location of the arena. |
| `Location` | `getArenaPlayer1Spawn(String name)` | Gets spawn point for Player 1. |
| `Location` | `getArenaPlayer2Spawn(String name)` | Gets spawn point for Player 2. |
| `String` | `getArenaSchematic(String name)` | Gets the schematic file name. |

### Example Usage:
```java
if (api.getArenaManager().isArenaReady("SpaceStation")) {
    Location center = api.getArenaManager().getArenaCenter("SpaceStation");
    player.teleport(center);
}
```

---

## 🚪 CabinManager (Physical Rooms)

Methods for physical Cabins logic and **configuration** data.
**Access:** `api.getCabinManager()`

### Logic & State
| Return Type | Method | Description |
| :--- | :--- | :--- |
| `boolean` | `isInCabin(UUID uuid)` | Checks if player is physically inside a cabin region. |
| `boolean` | `isInCabinFight(UUID uuid)` | Checks if player is fighting inside a cabin. |
| `String` | `getCabinName(UUID uuid)` | Gets the name of the cabin. |
| `UUID` | `getCabinOpponent(UUID uuid)` | Gets the opponent inside the cabin. |

### Configuration & Data
| Return Type | Method | Description |
| :--- | :--- | :--- |
| `List<String>` | `getCabinNames()` | Returns a list of all cabin names. |
| `boolean` | `isCabinReady(String name)` | Checks if a cabin is setup (Status READY). |
| `boolean` | `isCabinConfirmed(String name)` | Checks if the setup is confirmed. |
| `ApiCabinStatus` | `getCabinStatus(String name)` | Gets current status (READY, BUSY, OPENING, REGEN). |
| `Location` | `getCabinsLobby()` | Gets the global Cabins Lobby location. |
| `Location` | `getCabinPos1(String name)` | Gets the first corner of the cabin region. |
| `Location` | `getCabinPos2(String name)` | Gets the second corner of the cabin region. |
| `Location` | `getCabinCenter(String name)` | Gets the center/teleport location. |
| `List<Location>` | `getCabinDoorLocations(String name)` | Gets a list of all door block locations. |

---

## ⚔️ GladiatorEventManager (Gladiator Events)

Methods for managing the Gladiator free-for-all events.
**Access:** `api.getGladiatorEventManager()`

| Return Type | Method | Description |
| :--- | :--- | :--- |
| `boolean` | `isEventActive()` | Checks if the Gladiator event is currently active. |
| `String` | `getActiveArenaName()` | Gets the name of the arena currently hosting the event (returns `null` if not active). |
| `ApiGladiatorsStatus` | `getEventStatus()` | Gets the current status of the active Gladiator event (e.g., WAITING, INGAME). |
| `List<UUID>` | `getPlayersInEvent()` | Gets a list of UUIDs of players currently participating in the event. |
| `boolean` | `isPlayerInEvent(UUID uuid)` | Checks if a specific player is in the Gladiator event. |
| `boolean` | `startEvent()` | Forces the Gladiator event to start. Returns false if already active or no arenas are available. |
| `void` | `stopEvent(String reason)` | Forces the Gladiator event to stop. You can provide an optional broadcast message. |
| `boolean` | `joinEvent(Player player)` | Adds a player to the Gladiator event. Returns false if event is full or not in WAITING status. |
| `boolean` | `leaveEvent(Player player)` | Removes a player from the Gladiator event. |

---

## 🎒 KitManager

Methods for accessing Kit contents.
**Access:** `api.getKitManager()`

| Return Type | Method | Description |
| :--- | :--- | :--- |
| `List<String>` | `getKitNames()` | Returns a list of all kit names. |
| `boolean` | `isKitExists(String name)` | Checks if a kit exists. |
| `ItemStack[]` | `getKitContents(String name)` | Gets the main inventory items of a kit. |
| `ItemStack[]` | `getKitArmor(String name)` | Gets the armor items of a kit. |

---

## 🎮 Global Game Control

General methods available directly in the provider.

| Return Type | Method | Description |
| :--- | :--- | :--- |
| `boolean` | `isArmorMode(UUID uuid)` | Checks if the player is in any queue (Default or Arena) that requires Armor. |
| `boolean` | `endFight(Player p, ApiGameEndReason r)` | Forcefully ends a fight for a player in any mode. |
| `long` | `getFightDuration(UUID uuid)` | Gets the duration of current fight in seconds. |
| `void` | `onFightStart(UUID u, Consumer<String>)` | Executes code when a fight starts. |
| `void` | `onFightEnd(UUID u, Consumer<ApiGameEndReason>)` | Executes code when a fight ends. |
| `void` | `forceTeleport(Player player, Location location)` | Teleports a player while bypassing internal SpacePvP restrictions. Cannot be cancelled by events. |
| `boolean` | `startDefaultMatch(Player player1, Player player2)` | Starts a duel in Default mode (Random Location). Returns false if players are busy or no locations available. |
| `boolean` | `startDefaultMatch(Player player1, Player player2, GameSettings settings)` | Starts a duel in Default mode with custom GameSettings. |
| `boolean` | `startArenaMatch(Player player1, Player player2)` | Starts a duel in Arena mode on a random free arena. Returns false if players are busy or no arenas available. |
| `boolean` | `startArenaMatch(Player player1, Player player2, GameSettings settings)` | Starts a duel in Arena mode with custom GameSettings. |
| `boolean` | `startArenaMatch(Player player1, Player player2, String arenaName)` | Starts a duel in Arena mode on a specific arena. Returns false if arena is busy, does not exist, or players are busy. |
| `boolean` | `startArenaMatch(Player player1, Player player2, String arenaName, GameSettings settings)` | Starts a duel in a specific Arena with custom GameSettings. |

### Example Usage:
```java
SpacePvPProvider api = SpacePvPAPI.get();
UUID uuid = player.getUniqueId();

// Force end a game by Admin
api.endFight(player, ApiGameEndReason.AdminForce);

// Callback on start
api.onFightStart(player.getUniqueId(), (arenaName) -> {
    player.sendTitle("FIGHT!", "Map: " + arenaName, 10, 20, 10);
});

// Force teleport a player
api.forceTeleport(player, new Location(player.getWorld(), 100, 64, 100));

// Start a default match
boolean defaultStarted = api.startDefaultMatch(player1, player2);
if (defaultStarted) player.sendMessage("Default duel started!");

// Start a random arena match
boolean arenaStarted = api.startArenaMatch(player1, player2);
if (arenaStarted) player.sendMessage("Arena duel started!");

// Start a specific arena match
boolean specificArena = api.startArenaMatch(player1, player2, "SpaceStation");
if (specificArena) player.sendMessage("Arena duel in SpaceStation started!");
```
### Example Usage with GameSettings:
```java
GameSettings settings = GameSettings.builder()
.setCountdown(5)
.setUseKit(true)
.setKitName("pvp")
.setUpdateStats(true)
.setClearInventory(true)
.setUseEffects(true)
.addEffect("SPEED", 200, 1)
.setRegenArena(true)
.setVictoryMessages(true)
.build();


boolean started = api.startDefaultMatch(player1, player2, settings);
if (started) player1.sendMessage("Custom duel started!");
```
---

## 🔔 Events

The API fires standard Bukkit events. All primary game and queue events are now **Cancellable**, allowing third-party plugins to intercept and stop SpacePvP actions.

| Event Class | Description | Cancellable |
| :--- | :--- | :---: |
| `PvPGameStartEvent` | Fired when a match begins (after countdown). Cancelling prevents teleportation and fight start. | ✅ |
| `PvPGameEndEvent` | Fired when a match finishes. *Note: Cancelling `DEATH` or `QUIT` reasons may break internal arena states.* | ✅ |
| `ArenaQueueJoinEvent` | Player joins Arena queue. | ✅ |
| `ArenaQueueLeaveEvent` | Player leaves Arena queue. | ✅ |
| `DefaultQueueJoinEvent` | Player joins 1vs1 queue. | ✅ |
| `DefaultQueueLeaveEvent` | Player leaves 1vs1 queue. | ✅ |
| `GladiatorEventStartEvent` | Fired when the Gladiator Event attempts to start its registration phase. | ✅ |
| `GladiatorJoinEvent` | Fired when a player attempts to join a waiting Gladiator Event. | ✅ |
| `GladiatorLeaveEvent` | Fired when a player attempts to leave a Gladiator Event before it starts. | ✅ |

### Listener Example:
```java
import dev.ua._klaidi4_.spacepvpapi.events.*;

@EventHandler
public void onDuelStart(PvPGameStartEvent event) {
    Player p1 = event.getPlayer1();
    Player p2 = event.getPlayer2();
    String arena = event.getArenaName();
    
    Bukkit.broadcastMessage("⚔️ Duel started: " + p1.getName() + " vs " + p2.getName() + " on " + arena);
}

@EventHandler
public void onGladiatorStart(GladiatorEventStartEvent event) {
    // Prevent the Gladiator event from starting automatically
    event.setCancelled(true);
    Bukkit.getLogger().info("Gladiator event auto-start prevented by custom plugin!");
}
```




