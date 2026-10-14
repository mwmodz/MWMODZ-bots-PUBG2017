# PUBG_Bots v44 — every `[Bots]` setting

All settings live in the `[Bots]` section of `settings.ini`, next to the DLLs. Every key is optional.

**How values are read**
- **Missing or empty key:** the code default is used.
- **Number that won't parse:** the code default is used.
- **On/off settings:** `1`, `true`, `yes` or `on` mean on. Anything else means off.
- **Distances:** in **cm** (100 = 1 m, 100000 = 1 km).
- **Times:** in seconds.
- **Shares and chances:** written as 0–1 (0.25 = 25%).

**Applying changes**
- Typing `reload` in the server console re-reads `[Bots]` while the server runs, and the new values
  apply to existing bots too.
- `PawnClass` and `ControllerClass` only affect bots spawned after the reload.
- A few others only matter at spawn or match start (spawn box, `Count`, `RegisterTeams`…). Those
  take effect on the next spawn or the next match.

**Columns:** *Default* is the value in the code. *Live* is the value in `config/settings.ini`
right now. "—" means the key isn't in the file, so the default is used.

---

## Core / server behaviour

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `Enabled` | on/off | 1 | 1 | Master switch for the whole mod. |
| `BotType` | `ai` / `player` | ai | ai | `ai` = AIController bots. `player` uses CreatePlayer, which **crashes this build**, so don't use it. |
| `RegisterTeams` | on/off | 1 | 1 | Gives each bot its own PUBG team, so the game treats it as a player: it rides the plane, jumps and counts for kills/placement. |
| `BotsInPlayerCount` | on/off | 1 | 1 | JOINED, "X left" and alive-teams numbers count humans + bots. The game itself only counts real logins. |
| `CountAsJoined` | on/off | 1 | — | Bumps the lobby JOINED counter per bot. Only used when `BotsInPlayerCount=0`. |
| `KeepMatchWhileBotsAlive` | on/off | 1 | 1 | Stops the game from ending the match while bots are alive and at least one human is alive. |
| `NativeEndGuard` | on/off | 1 | 1 | Hooks the game's own match-end code, so a solo player + bots match can't auto-finish. |
| `EndWhenPlayersDead` | seconds, 0 = off | 5 | 5 | Once the last real player dies or leaves, the match ends after this long. Bots alone don't keep it going. |
| `ExitAfterMatch` | seconds, 0 = off | 5 | 5 | After the match ends, the server process closes itself after this long. The watchdog then restarts it cleanly, with no crash box. |
| `HoldLobbyWhenEmpty` | on/off | 1 | 1 | The lobby countdown stays at full while no real player is connected, so an empty server never plays a match on its own. |
| `Hotkeys` | on/off | 1 | 1 | Server-window hotkeys: F5 = 1 bot near a player, F6 = spawn `Count`, F7 = remove all bots, F8 = freeze/unfreeze the AI. |
| `VerboseLog` | on/off | 0 | 1 | Logs every arm, move and water step to PUBG_Bots_LOG.txt. Useful for debugging, but the log gets big. |
| `CombatLog` | on/off | 1 | 1 | Logs which bot spotted or hit a player, and why a nearby bot isn't shooting. |
| `ZoneWatch` | on/off | 1 | 1 | Logs the blue zone timing and server stalls. Also makes the game state replicate faster while the gas moves. |
| `FixKillFeedCount` | on/off | 1 | 1 | The kill feed's "X left" counts players + bots, so a kill doesn't say "MATCH FINISHED" early. |

## Spawning

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `Count` | whole number | 20 | 75 | How many bots match start, F6 and the `bots` command spawn. |
| `MaxBots` | whole number | 99 | 99 | Hard cap on bots alive at once. |
| `SpawnOnMatchStart` | on/off | 1 | 1 | Spawns `Count` bots automatically. In the lobby they join so they can board the plane. |
| `SpawnPerTick` | whole number ≥1 | 2 | 2 | Bots spawned per AI tick (10 ticks/s). Lower = smoother server while spawning. |
| `PawnClass` | `random` / `auto` / class name | random | random | Character body. `random` = female/male like real players. `auto` = the class a connected player uses. Or give an exact class name. |
| `ControllerClass` | `AIController` / `TslAIController` / `TslBotAIController` | AIController | AIController | AI controller class. AIController is the safe one. |
| `Weapon` | comma list, or empty | (empty) | (empty) | Guns bots get, matched against part of the gun class name, e.g. `AK47,M16,SCAR`. Empty = random non-pistol gun. Type `weapons` in the console to list them. |
| `NamePrefix` | text | Bot_ | Bot_ | Bot names are prefix + number (`Bot_12`). |
| `SpawnMinX` / `SpawnMaxX` | cm | 150000 / 550000 | same | Random spawn box on the X axis. The server uses the same box for players. |
| `SpawnMinY` / `SpawnMaxY` | cm | 150000 / 500000 | same | Random spawn box on the Y axis. |
| `MinSpawnZ` | cm | -1000000 | same | Ground lower than this is never used for a spawn. Raise it if bots spawn on the sea floor. |
| `SeaLevelZ` | cm, or empty | (learn) | (learn) | Water level. Empty = learned the first time a bot lands in water, and the log prints it. Set it to skip the learning step. |

## Plane & landing

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `EjectMin` / `EjectMax` | seconds | 15 / 75 | 10 / 50 | Each bot jumps a random time between these after boarding. Max is forced to be ≥ Min. |
| `SuperheroDrop` | on/off | 1 | 1 | Bots only jump over land, then land safely: no freefall, no chute, no fall damage. |
| `JumpSpread` | seconds | 90 | 30 | Once the plane is over land, each bot whose timer is up jumps within this window, so they don't all drop at once. |
| `DropSpread` | cm | 150000 | 250000 | A bot lands up to this far from the point under the plane. |
| `VisibleFall` | seconds | 5 | 12 | How long players can see a bot falling after it jumps, before it turns invisible for the simulated drop. |
| `DropTimeMin` / `DropTimeMax` | seconds | 45 / 100 | 45 / 100 | Jump-to-ground time, like a player's skydive + chute. The bot sits invisible and untouchable on its landing spot until then. It has no gun until it's down. |
| `LandingGrace` | seconds | 25 | 25 | After landing, a bot waits this long before shooting. |
| `PlaneKickTime` | seconds, 0 = off | 150 | 0 | Seconds after match start when everyone still in the plane is thrown out. It waits up to 60 s more for land under the plane. Not needed with PUBG_Match, which ejects everyone near the far coast. |
| `FixPlaneAltitude` | on/off | 1 | 1 | Lifts any plane flight-path point below `PlaneMinZ` up to `PlaneZ`, so the plane doesn't fly in the water. |
| `PlaneMinZ` / `PlaneZ` | cm | 100000 / 150000 | same | Threshold and target height for `FixPlaneAltitude` (1000 m / 1500 m). |
| `UnarmedInLobby` | on/off | 1 | 1 | Bots get no gun in the lobby or plane. They're armed once the match starts. |

## Brain: senses, movement, roaming

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `ThinkHz` | thinks per second | 10 | 10 | How often the bot AI runs. |
| `AiLod` | on/off | 1 | 1 | Bots far from every player think less often (600 m+: 3×/s, 1.5 km+: 1×/s). Fighting bots always think at full rate. A big CPU saver with many bots. |
| `ViewDistance` | cm | 30000 | 35000 | Normal sight range. |
| `AlertDistance` | cm | 50000 | 45000 | Sight range for about 6 s after being shot or hearing gunfire. The larger of this and ViewDistance is used. |
| `FovDegrees` | 10–360 | 130 | 130 | Field of view for spotting. Targets closer than 4 m, and alerted bots, ignore it. |
| `HearingDistance` | cm, 0 = off | 30000 | 30000 | Bots this close to a player's gunfire come to check it out. |
| `MaxResponders` | whole number | 2 | 1 | How many of the closest bots respond to one burst of gunfire. |
| `RoamRadius` | cm | 12000 | 12000 | How far a bot wanders per roam pick. Normal bots stay within about 1.5× this of their home area. |
| `AcceptRadius` | cm | 120 | 120 | How close a bot must get to a move target before it counts as "arrived". |
| `Sprint` | on/off | 1 | 1 | Bots may sprint, e.g. zone runs and hunters closing in. |
| `AvoidObstacles` | on/off | 1 | 1 | Probes ahead while walking: steers around walls, hops small obstacles, steers away from water. |
| `UseNavMesh` | on/off | 1 | 1 | Uses a navmesh if one exists. This map has none, so bots move in straight lines whatever this is set to. 0 forces straight lines. |
| `FollowZone` | on/off | 1 | 1 | Bots head into the white circle when they're outside it, or near its edge. |
| `ZoneLeg` | cm | 30000 | 30000 | How far a bot runs per leg when heading into the zone. |
| `LateZoneShare` | 0–1 | 0.4 | 0.4 | Share of bots that wait in the blue to ambush players coming in, then run in when the gas gets close. Higher = more bots die to the zone. |
| `LateZoneMargin` | cm | 25000 | 25000 | How close the gas gets before those late bots start running in. Bigger = they leave earlier and survive more. |

## Hunting players

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `HuntPlayers` | on/off | 1 | 1 | Lets roaming bots drift toward players at all. Hunters also need this. |
| `HuntDistance` | cm | 200000 | 200000 | Only players within this range pull drifting bots in. |
| `HuntChance` | 0–1 | 0.6 | 0.9 | Share of a **hunter's** roam picks that head toward its player. |
| `DriftChance` | 0–1 | 0 | 0 | Share of a **normal** bot's roam picks that drift toward a nearby player. 0 = they stay around their own area. |
| `HuntersPerPlayer` | whole number, 0 = off | 3 | 1 | Bots per living player that always know where that player is and track them down. |
| `HunterRefillDelay` | seconds | 150 | 90 | A dead hunter is replaced this long after it dies. |
| `HunterPickRange` | cm | 150000 | 150000 | Hunters are picked from bots already within this range of the player. Bots 300 m+ away are preferred, so they walk in. |
| `HunterReplan` | seconds | 3 | 3 | How often a hunter re-reads where its player is. |
| `HunterStandoff` | cm | 1500 | 1500 | Hunters aim for a spot this far from the player, each from its own side (flanking). It's doubled when they're still far away. |
| `HunterCatchUp` | on/off | 0 | 1 | A hunter that is far from its player, or keeps walking into water, gets teleported onto land near the player. At most once every 45 s per hunter, and never outside the zone. |
| `HunterCatchUpDist` | cm | 150000 | 60000 | Farther than this from its player = teleport. |
| `HunterCatchUpMin` / `HunterCatchUpMax` | cm | 25000 / 45000 | 15000 / 30000 | The teleport lands it between these distances from the player. |
| `HunterCatchUpReplacements` | on/off | 0 | 1 | Replacement hunters may also teleport. 0 = only the first wave can, and later hunters walk in. |

## Who fights whom

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `FightEachOther` | on/off | 1 | 1 | Bots also fight other bots, after `BotPeaceTime`. |
| `BotPeaceTime` | seconds | 600 | 600 | Seconds after match start before bots fight **each other**. They always shoot players. |
| `CloseFightDist` | cm | 4000 | 4000 | Bots this close to each other may fight even during the peace time. |
| `MaxBotsPerPlayer` | whole number, 0 = no limit | 1 | 3 | Most bots fighting one player at the same time. 1 = once a bot engages you, no other bot joins in. A bot you shoot can always fight back. |
| `ClaimHold` | seconds | 20 | 20 | With `MaxBotsPerPlayer=1`: a player stays "taken" by the bot fighting them for this long after it last saw them. |
| `FairFights` | on/off | 1 | 0 | Until a player shoots a bot: only one bot shoots a given player at a time (3 s turns), only out in the open (never indoors), single taps. Full bursts once it's been hit. |
| `PlayerLandingGrace` | seconds | 10 | 10 | Bots never shoot players in the air, and wait this long after a player lands. |
| `MaxFireDistance` | cm | 20000 | 30000 | Bots only shoot players closer than this. Farther away, they close in first. |
| `ReactionTime` | seconds | 0.6 | 0.45 | Delay between first spotting a target and the first shot. |

## Shooting

Bots don't really aim. They roll their own hits and apply the damage directly (`OwnHitRoll`). There are two hit systems, picked by `ScheduledHits`:

- **`ScheduledHits=1` (in use):** fights are scripted in "hit windows". Every burst misses until
  burst `HitBurstMin..Max`. Then 2–3 hits land, then a gap of `WindowGapMin..Max` missed bursts,
  then the next window. Far or moving targets can make a window fail.
- **`ScheduledHits=0` (old system):** every shot rolls a hit chance from `Accuracy`,
  `AccuracyStart`, `AccuracyRampTime`, `HitFalloffDist`, `MovingTargetFactor`, `WarningShots` and
  `MaxHitsPerBurst`. Those keys do **nothing** while `ScheduledHits=1`.

**Rules both systems follow:**
- The first burst of a fight always misses.
- A body part the bot can't see can't be hit, so a bot never hits a target that's fully behind cover.
- `HitCooldown` applies.
- Headshots only happen if `HeadshotChance` > 0.

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `OwnHitRoll` | on/off | 1 | 1 | Bots roll their own hits and apply damage. This is needed because 2017 PUBG lets the shooter's client report hits, and bots have no client. |
| `ScheduledHits` | on/off | 1 | 1 | 1 = hit windows (above). 0 = old per-shot chance system. |
| `HitBurstMin` / `HitBurstMax` | burst number | 6 / 7 | 2 / 3 | Every burst before this one misses. The first window lands on a random burst in this range, 2 bursts later if the target is far away. |
| `HitsPerWindowMin` / `HitsPerWindowMax` | whole number | 1 / 2 | 2 / 3 | Hits in each window. They're spread over the burst(s), with about a 45% chance per shot. |
| `WindowGapMin` / `WindowGapMax` | bursts | 3 / 5 | 2 / 3 | Missed bursts between windows. |
| `LongRangeDist` | cm | 10000 | 10000 | Past this: shorter bursts, longer pauses (up to 3× at 3× the distance), and windows more likely to fail (about 50% at 250 m with the default). |
| `ReturnFire` | on/off | 1 | 1 | When a player hits a bot, the bot gets one early hit window instead of waiting for `HitBurstMin`. |
| `ReturnFireHits` | whole number | 1 | 2 | Hits in that early window. Then the normal gap applies. |
| `ReturnFireBurstMin` / `ReturnFireBurstMax` | burst offset | 1 / 2 | 1 / 2 | Which burst after being hit may land it (1 = the very next one). |
| `EvadeTime` | seconds | 4 | 4 | After a player hits it, the bot dodges for this long, side-stepping every 0.5–1 s. |
| `EvadeCrouch` | on/off | 1 | 1 | Crouch-spam while dodging. |
| `HitCooldown` | seconds | 2.5 | 1.5 | Minimum time between two hits from the same bot on a player, a breather to react. It doesn't apply bot vs bot. |
| `Damage` | HP per hit | 14 | 20 | Damage per body hit. It goes through the game's normal damage, so vests and helmets should reduce it. With no armour, 100 HP at 20 per hit = 5 hits. |
| `HeadshotChance` | 0–1 | 0.08 | 0 | Chance a hit goes to the head, if the head is visible. 0 = bots never hit heads. If only the head is visible, it's a miss. |
| `HeadshotMultiplier` | multiplier | 1.0 | 1.0 | Extra damage multiplier on head hits. The game's own head multiplier applies on top. |
| `DamageTypeClass` | class name, or empty | (empty) | (empty) | Optional UDamageType class for bot hits. Empty = engine default. The log says whether it was found. |
| `BurstMin` / `BurstMax` | seconds | 0.25 / 0.8 | same | Length of each burst. Max is forced to be ≥ Min. |
| `BurstPauseMin` / `BurstPauseMax` | seconds | 0.8 / 2.0 | same | Pause between bursts. It's longer at long range, and 1.5–3.5 s while `FairFights` is single-tapping. |
| `InfiniteAmmo` | on/off | 1 | 1 | Refills the bot's clip when it drops below 2. |
| `ShowBotShots` | on/off | 1 | 1 | Players see and hear bot gunfire: muzzle flash, sound, tracers. |

**Old system only (`ScheduledHits=0`)**

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `Accuracy` | 0–1 | 0.18 | 0.12 | Hit chance per shot at point blank, once fully dialed in. |
| `AccuracyStart` | 0–1 | 0.03 | 0.2 | Hit chance when a fight starts. It ramps to `Accuracy`. |
| `AccuracyRampTime` | seconds | 10 | 15 | Seconds of shooting the same target to go from start to full accuracy. |
| `HitFalloffDist` | cm | 6000 | 4000 | Hit chance halves at this range: at 4000 it's 1/2 at 40 m, ~1/7 at 100 m. |
| `MovingTargetFactor` | multiplier | 0.5 | 0.5 | Hit chance × this when the target is running or driving. In the scheduled system, a fixed 0.6 is used on the window chance instead. |
| `WarningShots` | shots | 5 | 4 | First shots of every fight that always miss. |
| `MaxHitsPerBurst` | whole number | 1 | 2 | Never more hits than this in one burst. |

## Looks & lobby

| Key | Values | Default | Live | What it does |
|---|---|---|---|---|
| `DressBots` | on/off | 1 | 1 | Random clothes for bots: shirt, pants and shoes, plus sometimes a jacket, hat, gloves, glasses or mask. |
| `DressPlayers` | on/off | 1 | 1 | Random clothes for players too, only in empty slots. |
| `BotUpdateAppearance` | on/off | 1 | 1 | Runs the game's own appearance rebuild on a bot after dressing it. Fixes headless, naked or red-ghost bots. |
| `BotBodyFix` | on/off | 1 | 1 | Copies face, hair and skin values from a connected player of the same gender into bots. Fixes clothes with no body inside. |
| `BotGender` | `match` / `random` / `female` / `male` | match | match | `match` = only spawn genders a connected player has. This is needed for `BotBodyFix`. |
| `BodyServerRebuild` | on/off | 0 | 0 | Experimental: also rebuild the fixed look on the server. Leave it at 0 unless bots lose their heads after looting. |
| `CopyAppearance` | on/off | 0 | 0 | Old workaround that copies a real player's whole look onto bots. Superseded; leave it at 0. |
| `MarkAsBot` | on/off | 0 | 0 | Flags bots as bots in their player state. Clients then draw them as test bots (red ghosts / headless), so keep it at 0. |
| `LobbyAntics` | on/off | 1 | 1 | In the lobby, bots walk circles, bunny-hop, crouch-spam, spin and sprint instead of standing still. |
| `LobbyPunch` | on/off | 1 | 1 | Lobby antics include walking up to someone and throwing punches. |


