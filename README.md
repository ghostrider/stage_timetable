# Light Night Stage Clock

A single-page stage timetable for the Light Night at Gaswerkhalle. It shows a large clock and two independent lanes, **DJ** and **VJ**. Each lane shows the current slot ("Jetzt") with its remaining time and a progress bar, the next slot ("Als Nächstes"), and a short list of the slots after that.

The page is a single `index.html` with no build step, served via GitHub Pages. It is written to run on old devices (Safari 12 / iOS 12, e.g. a first-generation iPad Air) as well as modern, new devices. Old devices because many people have some older devices somewhere in a drawer and I want to give these old devices some use. 

## Usage

- **Programm**: opens the editor. Enter one slot per line:

  ```
  DJ 22:00-23:30 Name
  VJ 22:00-23:00 Name
  ```

  `.` also works as the time separator (`22.00`), and an en dash works in place of the hyphen. Empty lines are ignored. **Beispiel laden** puts the built-in sample programme into the editor. Click **Speichern** to save it.
- The schedule is stored in the browser's `localStorage` on **this device only**. Each device needs its own copy.
- **Demo**: switches to generated demo slots around the current time, useful for checking the layout at any hour. Opening the page with `#demo` in the URL also starts demo mode.
- **Vollbild**: fullscreen, where the browser supports it. On iOS, use "Zum Home-Bildschirm" instead and set Auto-Lock to "Never".

## Day roll-over (important)

The page counts one **night** as running from **12:00 noon to 12:00 noon the next day**:

- Slot times **before 12:00** count as *after midnight* of the same night. So `23:00-01:00` and `01:00-03:00` follow straight on from the evening slots.
- The current time follows the same rule. Between 00:00 and 11:59 the page assumes it is still the night that started the evening before.

This means that **in the morning (before 12:00) the page treats the night as finished**. Both lanes show "Ende" and "Kein weiterer Slot", even when a programme is loaded for the coming evening. At **12:00** it switches over, and the upcoming evening's slots appear (e.g. "Umbau / Pause" with the first slot as "Als Nächstes").

To check a programme before noon, use the **Demo** mode or wait until 12:00.

The roll-over hour is set in two places in `index.html`: `toMin()` inside `parse()` (`h < 12`) and `nightSeconds()` (`d.getHours() < 12`). If you change it, change both to the same hour. Slots must not start or end after that hour following midnight.
