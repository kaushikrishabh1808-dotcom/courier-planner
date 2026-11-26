# Mini-Courier Planner

A small Python tool that plans one delivery trip: it orders the parcels, picks
what fits in the van, and works out a route through the city.

Course: Design & Analysis of Algorithms (ETCCDA302), Capstone Assignment

## How to run

You need Python 3.8 or newer. Nothing else is needed to plan a trip.

```
python run.py                  # plan a trip with the sample data (40 kg van)
python run.py --capacity 25    # try a smaller van
python run.py --bench          # timing tests + saves output/runtime_chart.png
python -m unittest discover tests    # correctness tests
```

The chart needs matplotlib (`pip install matplotlib`). Without it the timing
table still prints, only the picture is skipped.

Your own data: copy `data/parcels.json` and `data/map.json`, edit them, then
`python run.py --parcels my_parcels.json --map my_map.json`.

## What happens in a run

1. **Sort** the parcels by deadline, then priority (merge sort).
2. **Pack** the van with the best set of parcels for the weight limit (0/1 knapsack, dynamic programming).
3. **Route**: Dijkstra gives shortest distances between the depot and every stop, then nearest neighbour picks the visiting order and the van returns to the depot.

## Files

| File | What it does |
|---|---|
| `run.py` | entry point, prints every step |
| `sorting.py` | merge sort (and insertion sort for comparison) |
| `packing.py` | knapsack DP + table printing |
| `routing.py` | graph loading, Dijkstra, nearest neighbour |
| `benchmark.py` | runtime tests and chart |
| `data/` | sample parcels and city map (JSON) |
| `docs/design.md` | Phase 1 design |
| `docs/report.md` | results, complexity, optimisation ideas |
| `output/` | saved run output, runtime chart, test results |
| `tests/` | unit tests |

## Data format

Parcel: `id`, `customer` (a place on the map), `weight` (kg, whole number),
`priority` (1-5, 5 is most urgent), `deadline` (hour of the day), `value`
(how much it is worth delivering this trip).

Map: `depot` name and a list of `roads`, each `[place A, place B, km]`.
Roads work both ways.

## Screenshots

Run the commands above and take screenshots of the terminal. The text
versions of each run are already saved in `output/`.
