# Moon Analog Finder

NASA Space Apps Challenge 2026, "Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars".

Pick a spot at the lunar south pole, say what you need to test, and get the Earth sites that best stand in for it, with a score per factor and a clear statement of where the analogy breaks.

## Findings
See `docs/FINDINGS.md` for what the search found and three characterised candidate sites (East Greenland, the south-east Pamir on the Afghanistan and Tajikistan border, and Tarso Tousside in the Tibesti massif), with what matches, where each analogy breaks and what is not established.

## Use the app
Open the hosted link, or locally:

```
cd app
python -m http.server 8766
```

Then open http://localhost:8766. No keys or accounts are needed.

## How it works
1. Describe the lunar spot: a 10 km window measured for slope spread, slope classes (10 and 20 degrees), roughness, sunlight, permanent shadow and temperature.
2. Describe every Earth candidate the same way, on the same 80 m grid, using the same code. Only bare, snow-free land is kept.
3. Score each factor from 0 to 1 as the fraction that is the same, the definition used in NASA's lunar simulant Figure of Merit (Schrader et al. 2009).
4. Combine the factors with weights set by the test purpose: rover driving, shadow work and crater access, or power and darkness survival.
5. Rank 236,193 Earth windows in the browser. Gravity, vacuum and radiation are always shown as not matchable.

Weights and cut-offs are set by our team, not by NASA. See `docs/DECISIONS.md` for the reason behind each choice and `docs/BENCHMARK.md` for how the method is checked.

## Data
| Body | Data | Source |
|---|---|---|
| Moon | Elevation and slope, 80 m, 80 to 90 S | NASA LRO LOLA (PGDA product 90, Barker et al. 2023) |
| Moon | Average sunlight, permanent shadow | NASA LRO LOLA (PGDA products 69 and 90, Mazarico et al.) |
| Moon | Surface temperature, ice stability | NASA LRO Diviner Polar Resource Products (PDS Geosciences Node) |
| Earth | Elevation, 90 m | Copernicus DEM GLO-90 (ESA), open data on AWS |
| Earth | Vegetation index, land surface temperature, 2024 | NASA MODIS MOD13C2 and MOD11C3 (LP DAAC) |
| Earth | Map imagery | NASA GIBS Blue Marble |
| Earth | 3D display terrain | Terrain Tiles on AWS Open Data (display only) |

## Limits
- Measured skill is weak to modest: known lunar analog sites beat random bare land 61 to 65% of the time (50% is chance). The tool is a terrain screen, not a validated analog finder. See `docs/BENCHMARK.md`.
- The Earth library covers bare, snow-free land between 55 S and 84 N, plus four ice-free Antarctic areas added by hand.
- Lunar region centres are approximate.
- Windows are 10 km, so features smaller than a few km (for example Meteor Crater) are not resolved.
- No Earth site reproduces lunar gravity, vacuum or radiation.

## Rebuild the data
The raw data (about 5 GB) is not in this repository. Scripts in `scripts/` download it and `pipeline/` processes it. Copy `.env.example` to `.env` and add a NASA Earthdata token and Modal tokens.

## Use of AI
This project was built with the help of Claude (Anthropic) for research, planning and code. All scores come from the data and code in this repository, not from an AI model.
