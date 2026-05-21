# HVAC Peak Electrical Usage Optimizer

A lightweight, single-file web application that helps facility managers, engineers, and energy teams **stagger HVAC unit startups** to minimize peak electrical demand.

By intelligently grouping units into "waves" and scheduling their startup sequences, you can significantly reduce the simultaneous load on your electrical system — lowering demand charges, improving power quality, and supporting demand-response goals.

> **Try it instantly:** [https://michaelbonilla.github.io/HVAC-Peak-Electrical-Usage-Optimizer/](https://michaelbonilla.github.io/HVAC-Peak-Electrical-Usage-Optimizer/)

## ✨ Features

- **Smart Wave Scheduling** — Automatically groups units into staggered startup waves
- **Peak Demand Reduction** — Ensures no wave exceeds the electrical draw of the single largest unit
- **10% Safety Buffer** — Automatically adds a conservative buffer to all ramp times
- **Interactive Demand Profile Chart** — Visual stepped chart showing instantaneous kW load over time
- **Detailed Results Table** — See exact start/end times, buffers, and wave assignments
- **Multiple Schedules** — Save and switch between different building configurations (e.g., "Main Building", "Lab Wing")
- **CSV Import / Export** — Easily edit data in Excel or Google Sheets and import/export
- **Sample Data** — One-click loader with realistic 12-unit example (chillers, AHUs, pumps, VAVs)
- **Dark / Light Theme** — Respects system preference with manual toggle
- **Fully Client-Side** — Everything runs in the browser with `localStorage` persistence
- **Responsive Design** — Works great on desktop and tablet

## How It Works

1. **Define Units**
   - Enter each HVAC unit’s name, **Target Ready Time** (when it must be fully operational), **Ramp time** (minutes to reach full operation), and **Demand** (kW during startup).

2. **Optimization Engine**
   - Calculates the latest possible start time for each unit (`target - ramp × 1.1`)
   - Sorts units by required start time
   - Uses **greedy wave packing**: Units are grouped so that the total demand of any wave never exceeds `P` (the kW of the single largest unit)
   - Performs **backward scheduling** from the final wave to ensure earlier waves finish in time
   - Computes instantaneous power profile and identifies the true peak demand

3. **Results**
   - Visual wave cards showing start time, total kW, and unit count
   - Interactive Chart.js demand profile (stepped line)
   - Color-coded buffer column (green = ahead of schedule, red = target missed)

## Getting Started

### 🚀 Live Demo (Recommended)

Visit the hosted version here:  
**[https://michaelbonilla.github.io/HVAC-Peak-Electrical-Usage-Optimizer/](https://michaelbonilla.github.io/HVAC-Peak-Electrical-Usage-Optimizer/)**

No installation or setup needed — just open the link and start optimizing.

### Run Locally

1. Download or clone this repository
2. Open `index.html` directly in any modern browser (Chrome, Firefox, Edge, Safari)
3. Click **Load Sample** to explore with realistic data
4. Modify values and click **Calculate Optimal Schedule**

That's it. No build tools, no dependencies, no server required.

## Usage Guide

### Adding Units

| Field                  | Description                                      | Example     |
|------------------------|--------------------------------------------------|-------------|
| **Unit Name**          | Descriptive identifier                           | `Chiller-A` |
| **Target Ready Time**  | Time the unit must be fully operational          | `07:45`     |
| **Ramp (min)**         | Duration required to reach full operation        | `45`        |
| **Demand (kW)**        | Electrical load during the ramp-up period        | `85.0`      |

> **Tip**: A 10% safety buffer is automatically applied to every ramp time.

### Interpreting Results

- **Waves**: Groups of units that start together. The system keeps wave demand ≤ largest single unit.
- **Peak Demand**: Highest simultaneous kW load across the entire schedule.
- **Buffer (min)**: How much earlier (positive/green) or later (negative/red) a unit finishes compared to its target.
- **Demand Profile Chart**: Shows exactly when and how much power is being drawn.

## Algorithm Summary

The optimizer uses a practical, production-friendly approach:

1. Compute required start time for every unit
2. Sort by required start (earliest first), breaking ties by longest ramp
3. Pack units into waves using a **first-fit decreasing** style where wave capacity = max unit kW
4. Schedule waves backward from the final wave
5. Sweep events to build an accurate instantaneous demand curve

This balances computational simplicity with excellent real-world peak reduction.

## Technologies

- **Pure HTML + CSS + JavaScript** (no frameworks)
- [Chart.js](https://www.chartjs.org/) v4 (for the demand profile)
- `localStorage` for persistence
- Responsive CSS with system theme detection

## Persistence & Data

- All schedules are automatically saved in your browser's `localStorage`
- You can create multiple named schedules
- CSV format: `name,target,ramp_minutes,kW`
- Data never leaves your computer

## Roadmap / Ideas

- [ ] Add "Previous Day" indicator for overnight starts
- [ ] Export PDF report
- [ ] Support for different wave capacity rules
- [ ] Time-of-use rate integration
- [ ] Mobile PWA support

## Contributing

Pull requests are welcome! This project is intentionally kept as a **single self-contained HTML file** to remain extremely easy to use and deploy.

When contributing, please:
- Keep changes minimal and focused
- Maintain the existing code style and dark theme variables
- Test with both light and dark modes

## License

This project is released under the **MIT License**.

## Support

If this tool has saved you time or money on demand charges, consider buying me a pizza! 🍕

[![Buy Me a Pizza](https://img.shields.io/badge/Buy%20Me%20a%20Pizza-ffdd00?style=for-the-badge&logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/michaelbonilla)

---

**Made for facility engineers, energy managers, and anyone trying to tame HVAC startup peaks.**

Open the [live demo](https://michaelbonilla.github.io/HVAC-Peak-Electrical-Usage-Optimizer/), load the sample, and see your peak demand drop. ⚡

If you find it useful, consider starring the repo, sharing it with your facilities team, or [buying me a pizza](https://buymeacoffee.com/michaelbonilla)!
