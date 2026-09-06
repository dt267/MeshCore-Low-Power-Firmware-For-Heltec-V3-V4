# Companion Radio — Display & Button Guide

How to use the display and button on your Companion Radio node. Applies to all supported hardware — OLED (Heltec V3, V4), color TFT (Heltec T096), and e-ink (Heltec E213, Wireless Paper, E290).

---

## Button basics

One button does everything:

| Press | Action |
|---|---|
| **Single click** | Next page / next item / scroll down |
| **Double click** | Previous page / exit sub-level |
| **Triple click** | Previous item in most lists and sub-levels — falls back to double click's action on screens with no separate "previous" step |
| **Long press** | Open message preview (Home) · Enter sub-level (Quick Send / Settings / Radio) · Open menu (Recent page) · Toggle GPS (GPS page) |

> **Heltec E213 and E290 — second button (GPIO21):** Sends the same signal as triple-clicking the main button — previous item in a list or sub-level, or double click's action on screens with no separate "previous" step. The Wireless Paper has a single button only.

---

## Home screen pages

Single click cycles forward; double click and triple click both cycle backward.

| Page | What it shows |
|---|---|
| **Home** | Current time (large), date, connection status, battery |
| **Quick Send** | Send a preset message over LoRa |
| **Contacts** | List of known contacts for direct messaging |
| **Saved Locations** | Preview of saved map locations |
| **Recent** | Most recently heard nodes over LoRa, or found via Node Discovery |
| **Radio** | LoRa radio parameters and noise floor |
| **GPS** | GPS status and coordinates *(if GPS hardware present)* |
| **Sensors** | Battery history chart, plus attached environment sensor readings *(if any)* |
| **Settings** | Device settings |

---

## Home page

**OLED / T096:**
```
┌──────────────────────────────┐
│ MyNodeName        5✉ [=]    │
│         · • · · · · ·        │
│                              │
│           14:32              │
│                              │
│         14 Apr 2026          │
│        < Connected >         │
└──────────────────────────────┘
```

**E-ink (E213 / Wireless Paper / E290):**
```
┌──────────────────────────────────────────────────────────┐
│ MyNodeName                                  5✉  [===]   │
│                · · · • · · · ·                           │
│                                                          │
│                       14:32                              │
│                                                          │
│                                                          │
│ Pin: 477370                             14 Apr 2026      │
└──────────────────────────────────────────────────────────┘
```

- Node name at top-left; battery icon at top-right
- **Battery icon**: shows charge percentage as a fill bar in normal use.
  - **Charging**: replaced by a lightning-bolt glyph whenever the device detects external power — the percentage isn't meaningful mid-charge. *(V3, Wireless Paper, T096 only — other boards don't have charger detection yet.)*
  - **USB connected**: replaced by a USB connector glyph whenever Connection Mode is set to USB and a live USB connection is detected — takes priority over the charging glyph if both apply at once. *(Heltec V4, E213, E290, T096 only — V3 and Wireless Paper's USB chip has no way to detect this.)* On V4 / E213 / E290 this means the cable is plugged into a powered host (PC); on T096 it means an app has actually opened the connection — just plugging into a PC without opening anything won't show it.
- **Header**: when messages are stored in memory, shows count + envelope icon (`5✉`) where the clock used to be; empty when count is zero
- **GPS-on icon**: a small circle-with-center-dot glyph appears just left of the battery icon whenever GPS is turned on — an easy thing to forget about and leave draining the battery. *(Only on the Home page for OLED, since header space is tight there; shown on every page on T096 and e-ink, which have more header room.)*
- Page indicator dots below the header (filled dot = current page)
- **Large clock** (`HH:MM`) always centered — shows current local time once synced; shows uptime (`00:00` counting up) before first sync
- **OLED / T096**: date on the middle-bottom row, connection status at the very bottom
- **E-ink**: connection status / pairing pin at the bottom-left, date at the bottom-right
- **Long press** → open message preview

Connection status values:
- `< BLE Connected >` — app connected via BLE
- `< USB >` — USB transport mode active
- `< WiFi Connected >` — app connected via WiFi TCP
- `IP:192.168.1.x:5000` — WiFi mode, waiting for TCP connection (shows IP once router connection is up)
- `WiFi: connecting...` — WiFi mode, not yet connected to router
- `BLE: OFF` — Bluetooth disabled
- `Pin: 123456` — waiting for BLE pairing *(BLE mode only)*

> **Time and date** show local time once the timezone offset is set. Default is UTC. Configure once via TerminalCLI: `set tz.offset 7` (for UTC+7). Time is synced from the app on connect, or from GPS if hardware is present.

---

## Quick Send page

Send a preset message to a channel without typing. Targets the Public channel by default; the target channel can be changed via **Qk.Ch.** in Settings — see [Quick Send Channel screen](#quick-send-channel-screen).

```
┌──────────────────────────────┐
│ QUICK SEND        14:32 [=]  │
│         · · • · · · ·        │
│ I'm OK                       │
│ On my way                    │
│ I need help                  │
│ GPS:10.7769,106.7009         │
└──────────────────────────────┘
```

- 3 presets visible at a time (4 if no GPS); scrolls as you cycle
- Bottom line shows current GPS status:
  - `GPS:lat,lon` — valid fix, coordinates will be attached when sending
  - `?GPS:lat,lon` — no fix / using last known coords from app
  - `GPS: Private` — GPS Privacy mode is ON; coordinates will **not** be attached
- **Long press** to enter sub-level (active item highlights)

**In sub-level:**

| Press | Action |
|---|---|
| Single click | Next preset |
| Double click | Exit sub-level |
| Triple click | Previous preset |
| Long press | Send highlighted preset → alert "Sent!" or "Send failed" |

After sending, the display shows how many nodes have relayed your message (e.g. `Heard 3 Repeats`). The count updates in real time as each relay is heard.

GPS coordinates are appended automatically when sending (e.g. `I'm OK @10.7769,106.7009`), unless **GPS Privacy** is enabled in Settings.

**Customize presets and target channel via [TerminalCLI](Companion_TerminalCLI_Commands.md):**
```
get quick                        list all presets
set quick.0 Arrived at camp      change preset at index 0
set quick.reset                  restore all defaults
get quick.channel                show current target channel
set quick.channel MyGroup        change target channel
```

---

## Quick Send Channel screen

Opened from **Qk.Ch.** in Settings. Lists every configured channel so the Quick Send target can be picked directly, without stepping through the others.

```
┌──────────────────────────────┐
│ QK.CHANNEL 2/12              │
│──────────────────────────────│
│   Public                   * │
│ > Base Camp                  │
│   #SOS                       │
│   Valley Net                 │
└──────────────────────────────┘
```

- The header counts your position in the list (`2/12`)
- `*` marks the channel currently in use; the arrow starts there when the screen opens
- The list scrolls once there are more channels than fit on screen

| Press | Action |
|---|---|
| Single click | Move arrow to next channel |
| Double click | Return to home, target unchanged |
| Triple click | Move arrow to previous channel |
| Long press | Set highlighted channel as the Quick Send target |

---

## Contacts page

```
┌──────────────────────────────┐
│ CONTACTS          14:32 [=]  │
│         · · · • · · ·        │
│ Alien                        │
│ Big Boy                      │
│                              │
│                              │
└──────────────────────────────┘
```

Shows how many chat contacts are known. **Long press** to open the contact list.

---

## Contact List screen

```
┌──────────────────────────────┐
│ CONTACTS  4                  │
│──────────────────────────────│
│ > Alien                      │
│   Big Boy                    │
│   [R] Base Camp Room         │
│                              │
└──────────────────────────────┘
```

Chat nodes and room servers are listed. Room servers are tagged `[R]`. Repeaters and sensors are excluded.

| Press | Action |
|---|---|
| Single click | Move arrow to next contact |
| Double click | Return to home |
| Triple click | Move arrow to previous contact |
| Long press | Open action menu for selected contact |

---

## Contact Action screen

After selecting a contact and long pressing, an action menu appears:

```
┌──────────────────────────────┐
│ Alien                        │
│──────────────────────────────│
│ > Send message               │
│   Request telemetry          │
│                              │
│                              │
└──────────────────────────────┘
```

| Press | Action |
|---|---|
| Single click | Move arrow to next item |
| Double click | Return to contact list |
| Triple click | Move arrow to previous item |
| Long press | Execute selected item |

- **Send message** — opens Quick Reply screen to send a direct message
- **Request telemetry** — sends a telemetry request and opens the Telemetry Result screen

---

## Telemetry Result screen

Displays battery voltage and GPS coordinates returned by the contact.

**While waiting:**
```
┌──────────────────────────────┐
│ Alien                        │
│──────────────────────────────│
│ Requesting...                │
│                              │
│                              │
│                              │
└──────────────────────────────┘
```

**On success:**
```
┌──────────────────────────────┐
│ Alien                        │
│──────────────────────────────│
│ VBAT: 3.82V                  │
│ GPS:                         │
│ 10.7769  106.7009            │
│ > Trace GPS                  │
└──────────────────────────────┘
```

**On timeout:**
```
┌──────────────────────────────┐
│ Alien                        │
│──────────────────────────────│
│ No response                  │
│                              │
│                              │
│                              │
└──────────────────────────────┘
```

| State | Press | Action |
|---|---|---|
| Waiting | Double click | Cancel request, return to contact list |
| Result | Long press | Open GPS Trace screen *(only shown if GPS data present)* |
| Result / Timeout | Any other button | Return to contact list |

GPS coordinates follow the **Pos. Format** selected in Settings (DD, UTM, or MGRS). `> Trace GPS` is only shown when GPS data is present in the response.

---

## Reading messages

Long press on the **Home** page opens message preview. Messages are shown in three levels:

### Level 2 — New messages

When there are new messages, you go straight to a linear view of all unread messages across all senders and channels.

```
┌──────────────────────────────┐
│ (2) Alien:         1/3  42s  │
│──────────────────────────────│
│ I need help                  │
│ can you come?                │
│                              │
│                              │
│                              │
└──────────────────────────────┘
```

- **Top row**: who sent it (left), message number + age (right)
- **`▲`** top-right: more text above — double click to scroll up
- **`▼`** bottom-right: more text below — single click to scroll down

| Press | Action |
|---|---|
| Single click | Scroll down one page (5 lines); at end of message → next new message |
| Double click | Scroll up one page; at top → previous new message |
| At last new message, single click | Return to home (all new messages seen) |
| Long press | Open popup menu |

### Level 0 — Group list

After reading all new messages (or double click at the start), you land on the group list — a summary of all senders and channels.

```
┌──────────────────────────────┐
│ MSGS  *3 new                 │
│──────────────────────────────│
│ > Alien                    5 │
│   [G] Public               2 │
│   [R] Dev                  1 │
│   [G] #SOS                 8 │
└──────────────────────────────┘
```

- Groups sorted by most recently active; number on the right = total messages
- `>` marks the selected group; `*N new` shown when unread messages exist
- `[G]` prefix = group/channel; `[R]` prefix = room server; no prefix = direct contact

| Press | Action |
|---|---|
| Single click | Move arrow to next group |
| Double click | Return to home |
| Long press | Open selected group (Level 1) |

### Level 1 — Group detail

Shows all messages from one sender or channel, including older history.

```
┌──────────────────────────────┐
│ (2) Alien:         1/5  3h   │
│──────────────────────────────│
│ where are you?               │
│                              │
│                              │
│                           ▼  │
│                              │
└──────────────────────────────┘
```

| Press | Action |
|---|---|
| Single click | Scroll down / next older message |
| Double click | Scroll up / next newer message; at newest → group list |
| Long press | Open popup menu |

---

## Popup menu (while reading a message)

Long press any message to open the popup:

```
┌──────────────────────────────┐
│ (2) Alien:         1/5  3h   │
│──────────────────────────────│
│ I need help @10.77,106.70    │
│ ┌──────────────────────┐     │
│ │ > Reply              │     │
│ │   Save location      │     │
│ │   Delete             │     │
│ │   Delete all         │     │
│ │   Home               │     │
│ └──────────────────────┘     │
└──────────────────────────────┘
```

| Press | Action |
|---|---|
| Single click | Move to next item |
| Triple click | Move to previous item |
| Long press | Confirm selected item |
| Double click | Dismiss menu (cancel) |

- **Reply** — opens Quick Reply screen; sends back to the same contact (DM), the same channel, or the room server (`[R]`) — room replies are posted to the whole room and visible to all subscribers
- **Save location** — only shown if the message contains GPS coordinates
- **Delete** — removes the message on screen
- **Delete all** — removes every message from that sender or channel; only shown in **Level 1**, where the messages on screen really are one conversation, and only when there is more than one of them
- **Home** — go home immediately

Reply and Save location depend on the message, but Delete and Home are always there, so a long press always opens the menu.

### Confirming a delete

Both delete items ask first, starting on **No** — a deleted message cannot be brought back:

```
┌──────────────────────────────┐
│ (2) Alien:         1/5  3h   │
│──────────────────────────────│
│ I need help @10.77,106.70    │
│ ┌──────────────────────┐     │
│ │ Delete 5 msgs?       │     │
│ │ > No                 │     │
│ │   Yes                │     │
│ └──────────────────────┘     │
└──────────────────────────────┘
```

Single click moves between **No** and **Yes**, long press confirms, double click cancels.

> Deleting takes the message off the node for good. If a phone app has not synced it yet, it never will — delete what you have already read, not what you are still waiting to sync.

---

## Quick Reply screen

Choose a preset to send without typing. The preset list is the same one used by the Quick Send page — customize it once and it works everywhere. Used for both direct messages and channel replies.

```
┌──────────────────────────────┐
│ To: Alien                    │
│──────────────────────────────│
│ > I'm on my way              │
│   Meet at base               │
│   Need supplies              │
│                              │
└──────────────────────────────┘
```

The header shows who the message will go to (`To: Alien` for a DM, `To: [Public]` for a channel reply).

| Press | Action |
|---|---|
| Single click | Next preset |
| Double click | Cancel, return without sending |
| Triple click | Previous preset |
| Long press | Send selected preset |

After sending, an alert confirms success or failure and the screen returns to the message preview.

---

## Saving a GPS location

When viewing a message that contains GPS coordinates, long press opens the popup and select **Save location**:

```
┌──────────────────────────────┐
│ Save to slot:                │
│──────────────────────────────│
│ > 1. [empty]                 │
│   2. Base camp               │
│   3. [empty]                 │
│   4. [empty]                 │
└──────────────────────────────┘
```

| Press | Action |
|---|---|
| Single click | Move to next slot |
| Triple click | Move to previous slot |
| Long press | Save to selected slot |
| Double click | Cancel |

Saved locations persist in flash memory — they survive reboot.

> **Tip for SOS situations**: save the location immediately. Even if the message is later overwritten by new incoming messages, your saved location remains accessible from the **Saved Locs** page.

---

## Saved locations

The **SAVED LOCS** page shows your saved GPS points.

```
┌──────────────────────────────┐
│ SAVED LOCS         ●         │
│──────────────────────────────│
│ Alien: I need help           │
│ Big Boy: Heading home        │
│                              │
│                              │
└──────────────────────────────┘
```

Long press the **SAVED LOCS** page to open the full list:

```
┌──────────────────────────────┐
│ SAVED  2/10                  │
│──────────────────────────────│
│ > Alien: I need help         │
│   Big Boy: Heading home      │
│                              │
│                              │
└──────────────────────────────┘
```

Each entry shows **sender name + message snippet** so you can tell entries apart.

| Press | Action |
|---|---|
| Single click | Move highlight to next entry |
| Triple click | Move highlight to previous entry |
| Long press | Open GPS trace screen for highlighted entry |
| Double click | Return to home |

---

## GPS Trace screen

Shows live distance and bearing from your current position to a saved location.

```
┌──────────────────────────────┐
│ Alien: I need help     14:32 │
│──────────────────────────────│
│      10.7769  106.7009       │
│                              │
│            1.2km             │
│                              │
│          247°  WSW           │
└──────────────────────────────┘
```

- **Top line**: location label + current time
- **Coordinates**: the saved GPS point (always shown; format follows **Pos. Format** in Settings)
- **Distance**: straight-line to the target — requires your own GPS fix
- **Bearing / cardinal direction**: direction to head toward the target

If your node has no GPS fix, only the coordinates are shown.

| Press | Action |
|---|---|
| Any button | Return to Saved Locations list |

---

## Recent page

Shows the most recently heard nodes — real adverts, and repeaters found by **Node Discovery**.

```
┌──────────────────────────────┐
│ RECENT            14:32 [==] │
│         · · · · • · ·        │
│ Alien                    42s │
│ Base Camp                 5m │
│ Repeater-1          3.5>-2.0 │
│ a1b2c3d4            1.0>-5.5 │
└──────────────────────────────┘
```

A row from a real advert shows time since last heard. A row from Node Discovery shows SNR both ways instead — your signal as the repeater heard it, then its signal as you heard it. A repeater that answers but isn't a saved contact shows its public key prefix instead of a name.

**Long press** opens a menu (active item highlights):

- **Send Zero Hop Advert** — broadcast your own advert → alert "Advert sent!" or "Advert failed..". Handy when two people standing together want to pick each other up straight away.
- **Node Discovery** — probe for nearby repeaters → alert "Discovering...". Answers appear on this page as they arrive.

**In menu:**

| Press | Action |
|---|---|
| Single click | Next item |
| Double click | Exit menu |
| Triple click | Previous item |
| Long press | Activate item |

Node Discovery only updates this list — it never adds a contact, even with **Auto-add Contacts** on in Settings.

---

## Radio page

LoRa radio parameters. Long press opens a sub-level holding the two settings this page displays.

```
┌──────────────────────────────┐
│ RADIO             14:32 [==] │
│         · · · · · • ·        │
│ FQ: 915.000          SF: 10  │
│ BW: 250.00            CR: 5  │
│ TX: 20dBm          RxG: OFF  │
│ NF: -95            RxD: OFF  │
└──────────────────────────────┘
```

| Field | Meaning |
|---|---|
| FQ | Frequency in MHz |
| SF | Spreading factor |
| BW | Bandwidth in kHz |
| CR | Coding rate |
| TX | Transmit power in dBm |
| RxG | RX Gain mode: `OFF` / `ON` |
| NF | Noise floor — ambient noise level in dBm |
| RxD | RX duty cycle: `OFF` / `ON` |

- **Long press** to enter sub-level (active item highlights)

```
┌──────────────────────────────┐
│ RADIO             14:32 [==] │
│         · · · · · • ·        │
│ RxGain                    ON │
│ RxDuty                   OFF │
└──────────────────────────────┘
```

**In sub-level:**

| Press | Action |
|---|---|
| Single click | Next item |
| Double click | Exit sub-level |
| Triple click | Previous item |
| Long press | Activate item |

| Item | Action |
|---|---|
| **RxGain** | Toggle RxGain: OFF → ON |
| **RxDuty** | Toggle RX duty cycle on/off — sleeps the receiver to save 2-3 mA. Only works at SF5-SF8. |

Both items stay in [Settings](#settings-page) as well; the sub-level just puts them next to the `RxG` and `RxD` readings they change.

---

## GPS page *(optional)*

Only shown if GPS hardware is present. On E213, V3, and E290's free-GPIO GPS header, the module needs to be connected and powered at boot/reset so the firmware can confirm it's present.

**GPS on:**
```
┌──────────────────────────────┐
│ GPS               14:32 [==] │
│         · · · · · · •        │
│ gps on                   fix │
│ sat / hdop           8 / 1.2 │
│ pos         10.7769 106.7009 │
│ alt                      12m │
└──────────────────────────────┘
```

**GPS off:**
```
┌──────────────────────────────┐
│ GPS               14:32 [==] │
│         · · · · · · •        │
│ gps off                      │
│ intv                  always │
│ mode             GPS+BDS+GLO │
└──────────────────────────────┘
```
*(mode line: Heltec V4 / T096 / E213 / V3 / E290 only)*

- **Long press** — toggle GPS module on/off
- `gps on` / `gps off` — current software state
- `fix` / `no fix` — whether a valid position is available
- `sat / hdop` — number of satellites used in the fix, and current HDOP (`--` if not yet available); see `gps.minsat` / `gps.hdop` in [TerminalCLI Commands](Companion_TerminalCLI_Commands.md) to adjust how many satellites / how accurate a fix needs to be before GPS reports it as valid
- `intv` — GPS update interval: `always` if GPS stays on continuously (`gps.interval 0`), otherwise interval in seconds; shown only when GPS is off
- `mode` — active GNSS constellation (Heltec V4 / T096 / E213 / V3 / E290 only; see `gps.mode` in [TerminalCLI Commands](Companion_TerminalCLI_Commands.md))
- `pos` — current position (DD by default; UTM or MGRS if selected via **Pos. Format** in Settings)
- `alt` — altitude

---

## Sensors page

Two views in one page — **Battery** is always first; long press cycles through any attached environment sensors after that.

**Battery — 14-day history:**
```
┌──────────────────────────────┐
│ BATTERY 14D             3.82V │
│         · · · · · · •        │
│ ▂▃▄▅▆▇█▇▆▅▄▃▂▁          │
└──────────────────────────────┘
```

- 14-day rolling battery-percentage history, one bar per day, scrolling in as the days go
- Dashed lines mark the 100% and 50% levels for a quick read on trend
- Header top-right shows precise Vbat voltage (the battery icon elsewhere only shows a rough %)

**Attached sensor readings** *(only shown if an environment sensor is detected)*:
```
┌──────────────────────────────┐
│ SENSOR                BME280 │
│         · · · · · · •        │
│ 24.3°C          61%          │
│ 1013hPa         42m          │
└──────────────────────────────┘
```

- Header top-right shows the sensor's name (e.g. `BME280`)
- 2-column grid, up to 6 fields — just the value and its unit (`°C`, `%`, `hPa`, `m`, `V`, `A`, `W`, `kOhm`...) identifies each reading
- One page per detected sensor. With only one attached, the title just reads `SENSOR`; with more than one, pages are numbered by the order they were detected (`SENSOR 1`, `SENSOR 2`...) rather than the raw LPP channel number

| Press | Action |
|---|---|
| Long press | Cycle to next sensor (wraps back to Battery) |

---

## Settings page

```
┌──────────────────────────────┐
│ SETTINGS          14:32 [==] │
│         · · · · · · •        │
| Connection Mode          BLE |
│ BLE                       ON │
│ Repeat                   OFF │
│ RxGain                    ON │
└──────────────────────────────┘
```

- **Long press** to enter sub-level (active item highlights)

**In sub-level:**

| Press | Action |
|---|---|
| Single click | Next item (scrolls if more than 4) |
| Double click | Exit sub-level |
| Triple click | Previous item |
| Long press | Activate item |

| Item | Action |
|---|---|
| **BLE** | Toggle Bluetooth on/off; shows `ON, Connected` when app is connected. *Hidden when Connection Mode is USB or WiFi.* |
| **Connection Mode** | Opens a mode selection screen. Navigate to `BLE`, `USB`, or `WiFi` and confirm — the node reboots into the chosen mode. The current active mode is marked `*`. |
| **Repeat** | Toggle packet repeat on/off |
| **RxGain** | Toggle RxGain: OFF → ON |
| **RxDuty** | Toggle RX duty cycle on/off — sleeps the receiver to save 2-3 mA. Only works at SF5-SF8. |
| **Brightness** | Cycle backlight intensity: `25` → `50` → `75` → `100` *(Heltec T096 only)* |
| **Units** | Toggles between Metric and Imperial |
| **Pos. Format** | Cycle GPS coordinate display: `DD` → `UTM` → `MGRS` |
| **GPS Privacy** | Toggle ON to hide GPS coords from Quick Send messages |
| **Qk.Ch.** | Opens a [channel selection screen](#quick-send-channel-screen) — pick which channel Quick Send targets. The row shows the current channel name, or `(none)` if no channel is configured. Long channel names are truncated with `...` to fit. |
| **Message Persist** | Toggle ON to persist unsynced messages to flash so they survive a crash, reboot, or dead battery before the app syncs them. Off by default (RAM only). Turning off immediately clears any already-persisted copy from flash. |
| **Screen Off** | Cycle screen timeout: `15s` → `3min` → `Never` |
| **Flip Screen** | Rotate display 180° |
| **Font Weight** | Cycle font: `Thin` → `Bold`. *(E-ink displays only — E213, Wireless Paper, E290)* |
| **Message Font** | Cycle message body size: `Normal` → `Large`. Applies to all displays (OLED, TFT, E-ink). |
| **Start OTA** | OTA update mode. On ESP32: connect to WiFi `MeshCore-OTA`, go to `192.168.4.1`. The page includes a firmware upload section and a **WiFi credentials form** to set the SSID/password for WiFi mode and optionally switch to it immediately. On nRF52 (T096 / RAK4631): enters BLE DFU mode — use the **nRF Device Firmware Update** app to upload the `.zip` package. |
| **Shutdown** | Power off the device |
| **About** | Show device name, firmware version and build date |

---

## Message origin labels

| Label | Meaning |
|---|---|
| `(0) Alien:` | Message from Alien, arrived directly (0 hops) |
| `(2) Alien:` | Message from Alien, relayed through 2 nodes |
| `(D) Alien:` | Message routed directly to this node via a known path |
| `[G] Public` | Public channel broadcast |
| `[G] #SOS` | #SOS channel broadcast |
| `[G] MyGroup` | Private channel "MyGroup" |
| `[R] DevRoom` | Message pushed from room server "DevRoom" |

For direct messages, the hop count in parentheses tells you how many nodes relayed the message — useful for gauging link quality. Channel and room server messages omit the hop count since it varies per relay and doesn't reflect your own connectivity.

The `#` prefix on a channel name indicates a public hashtag channel; channels without `#` may be private groups.

