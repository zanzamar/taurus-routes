# Handover — Taurus Mountain Race 2026

**For the agent who receives the real route on Monday 12 October 2026.**
Race starts Wednesday 14 October 09:00. Updated 6 October 2026.

---

## 1. Read this first: what this work is for

**The goal was never to reconstruct the route.** It was to build a general sense
of the corridor so David could plan **weather** and **major city resupply**.
Everything between the cities is bonus.

Judge the four speculative tracks by that standard. They are not predictions to
be defended. They are a frame to hang the real route on.

---

## 2. What exists

All four legs are drawn. All four are layers in
`HTML Working/taurus-route-map.html`.

### The collection

**https://ridewithgps.com/collections/11886447?privacy_code=gZKckFMDgc5aLK1GANSsGTGzJLWchMGj**

| Leg | RideWithGPS route | Distance | Target | Ascent at 200 m | Peak |
|---|---|---|---|---|---|
| 1. Start to Checkpoint 1 | [57451027](https://ridewithgps.com/routes/57451027) | 252.5 km | 252 km | 5,921 m | 1,908 m |
| 2. Checkpoint 1 to 2 | [57451949](https://ridewithgps.com/routes/57451949) | 413.2 km | 405 km | 8,725 m | 2,255 m |
| 3. Checkpoint 2 to 3 | [57452155](https://ridewithgps.com/routes/57452155) | 451.3 km | 456 km | 5,317 m | 2,205 m |
| 4. Checkpoint 3 to finish | [57453066](https://ridewithgps.com/routes/57453066) | 344.3 km | 349 km | 9,717 m | 3,489 m |
| **Total** | | **1,461.3 km** | **1,460 km** | **29,680 m** | |

**Distance is excellent: +0.1% across the whole race.** Ascent at a 200 metre
sample is 29,680 m against the manual's 36,346 m, so **6,666 m short**. Read
section 3 before you act on that gap.

Leg 3 holds most of the deficit. It crosses the Konya and Karaman plateau, which
really is flat, but 5,317 m over 451 km is still light for this organiser.

---

## 3. Ascent is a measurement, not a fact. Read this before you compare anything

This is the most important technical lesson in the project, and it was learned
late, after two route changes were proposed to chase an error that was not in
the route.

**Total ascent is computed, not measured.** A program walks the track point by
point and adds every rise. So the answer depends entirely on how closely it
looks. Elevation data carries noise from barometric drift, satellite wobble, and
a terrain model built on a grid of about 30 metres. On flat ground, consecutive
points read 1,200, then 1,203, then 1,199, then 1,204 metres. A naive count calls
that 8 metres of climbing. It was flat.

The leg 4 file holds a point every 41 metres across 344 kilometres. At that
density, half a metre of false rise per point invents thousands of metres.

**The same leg 4 line, unchanged, measures from 8,781 m to 12,873 m.**

| Method | Leg 4 ascent |
|---|---|
| Every point, no filter (41 m spacing) | 12,873 m |
| 3 m threshold | 12,228 m |
| 10 m threshold | 10,981 m |
| 20 m threshold | 9,837 m |
| Resample to 100 m | 10,950 m |
| **Resample to 200 m** | **9,717 m** |
| Resample to 300 m | 9,239 m |
| Resample to 500 m | 8,781 m |

### The consequences

- **RideWithGPS smooths hard against its own terrain model, so it reads low.**
  Its figure is the floor of the range, not the truth. David identified this
  independently: "The RideWithGPS is probably not enough elevation in total.
  Distance looks good." He is correct.
- **Raw point-by-point counting reads high.** It is the ceiling.
- **Neither end is correct.** Smoothing deletes real short climbs together with
  the false ones. On scree and goat tracks, short steep ground is exactly what
  costs time.
- **The manual's 36,346 m came from an unknown method.** Treat it as a target,
  not as a measurement you can compare against.

### The rule

**Never compare two ascent figures that were sampled differently.**
Resample both files to 200 metres first. Then compare. The table in section 2
uses 200 metres for all four legs, so those rows are comparable with each other.

The script is `scratchpad/res200.py`. It is six lines of logic: walk the points,
emit one point per 200 metres, then sum every positive difference.

---

## 4. The cities. This is the part that matters

45 city candidates sit in the map. Each one was cross-referenced against all four
Global Positioning System files, and the confidence was adjusted to the measured
closest approach. The map hides every candidate below 50% on request.

### The anchors — high confidence, each with a live ten-day forecast

| Manual km | City | Elevation | Confidence |
|---|---|---|---|
| 0 | Antalya | sea level | 99% |
| 250 | Köprülü Canyon — Checkpoint 1 | 196 m | 95% |
| 350 | Akseki | 1,066 m | 95% |
| 505 | Karaman | 1,034 m | 85% |
| 655 | Taşkale — Checkpoint 2 | 1,359 m | 95% |
| 900 | Ereğli | 1,050 m | 92% |
| 978 | Ulukışla | 1,425 m | 85% |
| 1,111 | Demirkazık — Checkpoint 3 | 1,598 m | 95% |
| 1,292 | Çamlıyayla | ~1,100 m | 92% |
| 1,460 | Erdemli | sea level | 95% |

### The full table, after the cross-reference

| City | % | City | % | City | % |
|---|---|---|---|---|---|
| Antalya | 99 | Taşkale | 95 | Demirkazık | 95 |
| Köprülü Canyon | 95 | Akseki | 95 | Erdemli | 95 |
| Çamlıyayla | 92 | Ereğli | 92 | Kargı | 90 |
| Tarlaören | 90 | Çukurbağ | 88 | Ekomini | 85 |
| Çaltepe | 85 | Akşahap | 85 | Karaman | 85 |
| Ayrancı | 85 | Ulukışla | 85 | Karaöz | 80 |
| Madenköy | 80 | Hüyük Burun | 75 | Çiftehan | 70 |
| Kamışlı | 70 | Alihoca | 70 | Başmakçı | 65 |
| Başlar | 55 | Adabağ | 50 | Çamardı | 50 |
| Arslanköy | 20 | Çandır | 35 | Çardacık | 30 |
| Pozantı | 30 | Niğde | 25 | Derebucak | 20 |
| Bor | 20 | İbradı | 15 | Ermenek | 15 |
| Hadim | 10 | Bozkır | 10 | Sarıveliler | 10 |
| Taşkent | 10 | Karapınar | 10 | Gözne | 10 |
| Fındıkpınarı | 10 | Korkuteli | 5 | Yenişarbademli | 5 |

### What moved, and why

- **Çamardı 80 to 50.** The manual calls km 1,137 a "village shop". Çamardı is a
  district seat with a supermarket chain. The wording does not fit. Yelatan fits
  at a route factor of 1.19 with no loop at all.
- **Arslanköy 45 to 20.** No drawn line passes near it.
- **Çiftehan 50 to 70** and **Madenköy 55 to 80.** David confirmed the Çiftehan
  section from the route photographs.
- **Kargı 85 to 90** and **Tarlaören 85 to 90.** Both landed under 100 metres
  from a drawn line once the line moved south.

---

## 5. What to do when the real route arrives

Use these steps in order.

### Step 1 — decode and measure

1. Decode the file with `HTML Working/fitread.py`.
   The `fitparse` library does not build in this sandbox.
2. Report the distance in kilometres.
3. Resample the track to 200 metres.
   **Caution: do not report ascent from the raw points. The figure will be too
   high by thousands of metres.**
4. Report the ascent in metres from the resampled track.
5. Report the steepness in metres per kilometre.
6. Run the retrace test. Grid-hash at 0.0025 degrees, which is about 250 metres.
   Flag each return within 250 metres of ground covered 1.5 kilometres earlier.

### Step 2 — score the four guesses

1. Resample our four legs to 200 metres.
2. Measure the closest approach of the real route to each drawn line.
3. Measure the closest approach of the real route to each of the 45 city pins.
4. Report the score per leg and per city.
   This tells David which city calls held. That is the honest result.

### Step 3 — recompute the arrival times

This changes the plan more than the route does. The models are in
`pace-analysis.md`:

- Field moving speed: `km/h = 17.532 − 0.2296 × (m per km) − 0.00179 × (route km)`,
  weighted R² 0.742, fitted on 13 editions.
- Personal multiplier: **1.1616 ×** the field, median of 7 comparable rides.
- Stopped share: `fraction = 0.15425 + 0.0006108 × elapsed hours`.
- At the manual's own figures this gives 136.6 moving hours, 192.6 elapsed, and
  **8.03 days**.

**Warning: rerun the timing against the real ascent profile before you report any
day count. Our estimate is 6,666 m light at a 200 metre sample, which is worth
roughly 9 hours of moving time and 12 hours elapsed.**

### Step 4 — weather

The map already fetches a live ten-day forecast per city from Open-Meteo. It
flags each night at or below freezing. It marks the race window of 14 to 24
October.

Once Step 3 gives arrival days, narrow each forecast to the days David is
actually there. He asked for this and it was not built.

### Step 5 — do not rebuild the map from nothing

Read `HTML Working/CONTEXT.md` first. It records how the map is built and what
must not be broken.

---

## 6. Scree and goat tracks — confirmed by David

**Leg 4 definitely has scree fields and goat tracks. Leg 3 possibly, if the two
legs overlap into the same mountains.**

This is first-hand knowledge of how the organiser builds routes, and it explains
every routing failure in this project. The router could not carry the Bolkar
crossing because it is not a road. The same is true of the Taşkale to km 839
stretch on leg 3, and of Arslanköy to Erdemli at the finish.

### What it costs

Measured off the routed leg 4 line:

| Ground | Length | Peak |
|---|---|---|
| Bolkar crest traverse above 2,300 m | 23.9 km | 2,811 m |
| Spur beside photographs A and B | 5.2 km | 2,778 m |
| **Total high exposure** | **29.2 km** | — |

At a push speed of 3.1 km/h against 8.5 km/h ridden, **each kilometre pushed
instead of ridden costs 12.3 minutes**.

| Share that is scree | Pushed | Extra time |
|---|---|---|
| 25% | 7.3 km | +1.5 h |
| 40% | 11.7 km | +2.4 h |
| 75% | 21.9 km | +4.5 h |
| 100% | 29.2 km | +6.0 h |

### On the whole race

| Assumption | Moving | Elapsed | Days |
|---|---|---|---|
| No scree correction | 136.6 h | 192.6 h | 8.03 |
| 40% of the high ground pushed | 139.0 h | 196.0 h | 8.17 |
| 75% of the high ground pushed | 141.1 h | 199.0 h | 8.29 |

**This is leg 4 alone.** If leg 3 adjusts into the same mountains the exposure
roughly doubles, and so does the correction.

### Why this matters more than it looks

The field pace model is fitted on finishers of thirteen mountain races, so a
normal amount of hike-a-bike is **already inside the 1.1616 personal multiplier**.
Do not apply the correction twice. Apply it only to ground that is clearly worse
than a typical mountain race. Sustained scree above 2,300 m is that ground.

The correction is small in days but it lands at the worst moment. **The Bolkar
crest is the highest, coldest, most committing part of the race.** It comes after
1,200 km of accumulated fatigue. Pushing a loaded bike across scree at 2,800 m is
where a bad weather window turns into a stop. **Treat the time cost as the small
part and the exposure as the large part.**

---

## 7. Method, hard won

**The disproof hierarchy. Use it in this order.**

1. **Straight line — absolute.** A route can never be shorter than the
   great-circle distance. This killed İbradı, Ormana and Pozantı's kilometre
   permanently.
2. **Shortest mapped road — a warning only.** He uses goat tracks and unmapped
   surfaces. A track beats the mapped network; it cannot beat a straight line.
   This test wrongly killed four candidates before it was corrected.
3. **Route factor — a judgement.** `manual km ÷ great-circle km`. Legs 1 and 2
   both run at **1.88** end to end. Under about **1.15** is an implausible
   beeline; over **3.0** means a deliberate loop, so go find the terrain.

**Other rules that earned their place:**

- **Question the measurement before you change the line.** See section 3. The
  single largest wasted effort in this project was chasing a 3,325 m ascent
  discrepancy that was almost entirely a sampling artefact. Two route surgeries
  were proposed before the number was interrogated. Both were wrong.
- **A straight line samples elevation but proves nothing about passability.**
  Sampling a proposed line at 229 m found 18 of 24 segments over 25% grade,
  including +66% and −72%. David had already said it was a cliff.
- **Query Google and OpenStreetMap both, every time.** OpenStreetMap amenity
  coverage in rural Türkiye is thin and Google holds points it does not. An early
  pass judged from `place=` tags alone and got both leg 1 supermarkets wrong.
- **Scale the line before reading kilometres off it.** `k = drawn km ÷ manual km`.
  Without it, matches drift 10 to 20 km.
- **Test for out and back.** He does not ride ground twice. Grid-hash at 250 m.
  Three of four leg 2 candidates and every leg 3 southern loop failed on this.
- **Distance and ascent must land together.** Leg 2's first attempt made the
  distance on flat steppe; the ascent figure ruled it out. Flat ground that makes
  the distance is a warning, not a solution.
- **Read the slot wording.** "Village shop" against "several shops" against
  "major resupply" separates a village from a district town from a city. This
  moved Ayrancı from km 839 to km 846, and it is what demoted Çamardı.

### Three leg 4 ideas that did not survive the ground

David rejected all three from local knowledge. Each is recorded so the next agent
does not propose them again.

1. **Çamardı at km 1,137.** Assumed, not derived. The manual says "village shop".
   Çamardı is a district seat. Withdrawn. Note that `LOOP_APEX` on the map rests
   on the same assumption and is therefore also soft.
2. **A direct southern line at km 111.** It crosses a jagged cliff. Withdrawn.
3. **Holding the contour from km 180 to km 212.** The valley at km 190 is a
   confluence. The switchbacks are forced. Withdrawn.

---

## 8. Tooling

| Thing | Status |
|---|---|
| Sandbox network | Blocks Overpass, the router, Open-Meteo and the tile hosts. **All live queries go through the user's browser.** |
| Browser script timeout | 45 s. Fire a fetch, store it on `window`, return, poll on the next call. |
| Overpass | Rate limits hard; returns HTML on error, so check the body starts with `{`. Space calls 5 to 10 s. Use small bounding boxes. |
| Router profiles | `shortest` = true network minimum, which is the disproof test. `trekking` = realistic. `gravel` wanders and is useless for distance. |
| Python heredocs | An escaped apostrophe inside a string breaks them. Avoid apostrophes in patch scripts. |
| Map verification | `HTML Working/check.js` — Playwright. **It is stale: it still checks `CP4_RAW`, which was renamed to `CP4_TRACK`. Fix that name before you trust its report.** |
| Expected map state | No page errors. 45 city markers, 18 below 50%. Seven overlays. 25 resupply rows. |

---

## 9. Files

| Path | What |
|---|---|
| `HTML Working/taurus-route-map.html` | The map. Four routed layers, live weather, resupply table, collection link. |
| `HTML Working/CONTEXT.md` | How the map is built and what must not be broken. **Read before editing it.** |
| `HTML Working/fitread.py` | Decoder for the Flexible and Interoperable Data Transfer (FIT) file format. |
| `scratchpad/res200.py` | The 200 metre resampler. Use it before every ascent comparison. |
| `leg1/` to `leg4/` | Route options, GPS Exchange Format (GPX) tracks, pins and the per-leg findings. |
| `leg3/CHECKPOINT-3-PREP.md` | The method write-up in full. |
| `leg4/leg4-v2-assessment.md` | The leg 4 assessment, including the photograph matches. |
| `pace-analysis.md`, `fourteen-or-fifteen.md` | The rider models and what 8, 9 and 10 days each demand. |
| `taurus-food-resupply-research.md` | What can actually be bought in rural Turkish shops. |
| `taurus-road-surface-findings.md` | Surface and hike-a-bike calibration. |

---

## 10. Targets, so nothing drifts

**Target 9 days. 8 days is awesome amazing. 10 days is a finish. Finishing is
always the goal.** The cut-off is 255 hours: start Wednesday 14 October 09:00,
closes Saturday 24 October 24:00.

Riding with Stephen Fitzgerald.

---

## 11. One standing instruction

David is better served by **being told what is not known** than by a tidier
answer. Add a confidence and a reason to every claim. Surface contradictions
rather than smoothing them.

The most valuable results in this project were all disproofs: Pozantı's
kilometre, Korkuteli, İbradı, Çamardı's slot, and the finding that leg 2's first
255 km are forced onto the shortest road that exists.

**David corrects errors from local knowledge and from the route photographs.
Concede quickly and record the concession. He has been right every time.**
