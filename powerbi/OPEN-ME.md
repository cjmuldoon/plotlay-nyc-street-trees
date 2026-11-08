# Opening the Power BI report

`NYCStreetTrees.pbip` is a Power BI Project: the report (`NYCStreetTrees.Report`, PBIR format) and the semantic model (`NYCStreetTrees.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine. In Parallels the Mac home folder is usually shared as `\\Mac\Home`.

## Open it

1. In Windows, go to the repo's `powerbi` folder and double-click `NYCStreetTrees.pbip`.
2. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-nyc-street-trees/main/data/`). If Desktop asks how to connect, choose **Anonymous** and then **Connect**.
   - To read the files from disk instead, go to **Home, Transform data, Edit parameters** and set `DataFolder` to the full path of the repo's `data` folder, **ending with a backslash**. Then select **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** (all the sources are public open data).
3. Check the numbers (below).

If the report was updated while open, close Power BI and reopen the .pbip rather than using Apply external changes. (After Apply external changes one export drew the page at about 57% inside a much larger canvas; a full close and reopen put it right.)

## Check the numbers after Refresh

After **Refresh**, on **Trees by census** with the Census slicer on **2015** (as the report opens; it takes one census at a time):

| Card | Should read |
|---|---|
| Live street trees, census picked | 645,444 |
| Change 1995 to 2015 | 38.8% |

Pick 1995 and 2005 and the trees card reads 465,116 and 574,114. These are the trees placed in a neighbourhood; the README's citywide 652,173 also counts the few trees in parks, cemeteries and airports and any the census could not place. The borough chart starts with Queens (235,767).

**Change 1995 to 2015**: the bar chart starts with East New York (+6,270), then Great Kills (+3,374).

**East New York**: 5,289 points for 1995, 6,717 for 2005 and 15,402 for 2015.

## The model

- **Neighbourhoods**: one row per neighbourhood (`data/tables/change_1995_2015.csv`), keyed by `nta`, with the trees in each census and the change. The measures live here.
- **TreesByCensus**: one row per neighbourhood and census (`trees_by_census.csv`), related to Neighbourhoods by `nta`.
- **Census** (1995, 2005, 2015): for the census slicer. The tree measures always use one census: the one picked, or 2015 when none is.
- **Species** (`species.csv` unpivoted in Power Query to one row per species and census) and **EastNewYork** (`data/trees/east_new_york_trees.csv`, with a `tree` key made of the census and the census's own id).
- Measures: `Census shown`, `Trees`, `Trees per km2`, `Change since 1995`, `Change since 1995 pct`, `Share of trees`, `Tree count`.

## The two maps

- **Shape Map** (pages 1 and 2). The custom map, `data/geometry/neighbourhoods.topojson`, is saved inside the report (a copy that keeps only the neighbourhood code), so the neighbourhoods draw on open. The Shape Map has no base map of its own, so the file also carries the counties round the city (US Census TIGERweb, public domain) in a pale ground tone, with the harbour, the rivers and the Sound showing as the map's pale blue background. If the map shows the United States instead (an older Desktop can ignore a saved map): select it, then **Format visual**, **Visual**, **Map settings**, **Map type: Custom map**, **Add map type**, and choose `..\data\geometry\neighbourhoods.topojson`. Under **View map keys** the codes (BK82, MN17...) should be matched. If the Shape Map is not in the Visualizations pane, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- **Azure Maps** (East New York page). Each live street tree with a position in East New York's three neighbourhoods, for the census picked: Location is the tree, latitude and longitude averaged (one point per tree), the species in the tooltip. Your organisation's admin must allow Azure Maps (on by default on a personal account).

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Neighbourhoods[nta]`, Color saturation `[Trees per km2]` (page 1) or `[Change since 1995 pct]` (page 2).
- Azure Maps: Location `EastNewYork[tree]`, Latitude `EastNewYork[lat]` (Average), Longitude `EastNewYork[lng]` (Average).

Then save with **File, Save**, and if you like **File, Save as** a `.pbix`, which includes the data.
