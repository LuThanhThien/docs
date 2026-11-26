# Tesla FSD v15 — research conversation, November 26, 2026

This is a condensed continuation of the [earlier conversation](tesla-fsd-v15-conversation.md). The selected user excerpts are from the chat. The assistant entries summarize the discussion. Thien considers the high-level and business context sufficient and wants to start the inference-engineering research on November 27. The final presentation structure is still open.

## User

> for this phase we will discuss about what are new v15 features are, compared the improvement wrt v14, what are those meaningful for customers.

## Assistant

Start with what FSD (Supervised) does and who watches the road. The car can steer, brake and turn. The driver monitors the road and takes over when needed. In the four- or five-stage AI automation illustration, mark **Human supervises** as the current customer experience. Unsupervised operation is Tesla’s stated direction. The illustration explains the human role during use. [Tesla FSD overview](https://www.tesla.com/support/fsd) · [Tesla Q1 2026 update, p. 8](https://assets-ir.tesla.com/tesla-contents/IR/TSLA-Q1-2026-Update.pdf#page=8)

Thien prefers Tesla-hosted documentation first, descriptive reading links, and checkboxes for progress. The [research checklist](https://bos-semi.atlassian.net/wiki/x/DQBQLQ) has the reading order. The [source analysis](https://bos-semi.atlassian.net/wiki/x/UgBHLQ) collects notes from the individual sources. The [details analysis](https://bos-semi.atlassian.net/wiki/spaces/~712020787c4a9996e44f6c9a67bab1fcdb48ac/pages/759890001/Tesla+FSD+v15+-+Details+Analysis#High-level-context) brings the comparisons together.

## User

> question is is this metric mile driven before collision an official legant metric for a car safety or it's tesla designed metric?

## Assistant

Crashes per mile is an established road-safety measure. Tesla expresses its reciprocal as **miles driven per collision**. For a given driving mode and period, divide fleet miles by the number of qualifying collisions. Calculate the figure separately for major and minor collisions. This describes a fleet average. [FHWA crash-rate methodology](https://highways.dot.gov/safety/speed-management/speed-management-eprimer-rural-transition-zones-and-town-centers/3) · [Tesla Vehicle Safety Report](https://www.tesla.com/fsd/safety)

Tesla publishes its counting rules. Major collisions involve a non-reversible restraint deployment. Minor collisions meet the change-in-velocity threshold of at least 8 km/h within 150 ms without such a deployment. Tesla attributes a collision to FSD if it was active at any point in the preceding five seconds. Rates cover a rolling 12 months. For the customer question “How often do collisions occur with FSD engaged compared with manual driving?”, use **manually driven Teslas equipped with active safety features** as the comparison group. Label the chart’s period, collision category and comparison group. Tesla also publishes an estimated U.S. average and describes collecting telemetry internationally. Each comparison needs its own geographic scope. [Tesla methodology and baselines](https://www.tesla.com/fsd/safety)

## User

> so how can actually i compare befre and after v15 safety actually 25x as their report?

## Assistant

The v15.0.1 release notes claim a **25× improvement in critical-intervention rate**. Phil Duan identifies v14 as the baseline. The sources reviewed leave the precise v14 build, critical-event labeling rule, counts and exposure by version unspecified. If the denominator is FSD-engaged miles, divide the v14 events-per-mile rate by the corresponding v15 rate under comparable conditions. A driver can intervene and avoid a crash. The rolling fleet collision report therefore cannot reproduce this version-specific intervention claim. [v15.0.1 release notes, mirrored](https://www.notateslaapp.com/software-updates/version/2026.38.6.11/release-notes) · [Phil Duan’s comparison, archived](https://elonmuskarchive.org/posts/2124280942034661544)

## User

> it's hard to actually know compared to v15 what changes make the improvement

## Assistant

| Area | What v14 already offered | What Tesla describes for v15 |
|---|---|---|
| Perception and interventions | v14.2.1 used higher-resolution features. v14.3 improved low-visibility perception and changed reinforcement learning for rare cases. | Tesla links the 25× claim to an upgraded vision encoder shared across FSD, Robotaxi and Optimus and trained on millions of videos. The sources give no experiment isolating the encoder’s contribution. |
| Display and compute | Drivers could see the driving visualization and use it to adjust settings. | Reality View generates an overhead view with upcoming turns. Tesla reports lower total compute for the visualization and gives no percentage. The separate **up to 20%** figure refers to v14.3 inference latency. |
| Parking | v14.2.1 let drivers choose curbside, driveway or another arrival option and remembered preferences by destination. | FSD uses personal parking history to select a location and displays multiple options on the map. |
| Road hazards | FSD adjusted its path around debris such as tires and branches. | Tesla reports improved detection of chains, small animals and potholes, plus improved avoidance of deep standing water. The notes give no per-hazard success rates. |

The earlier v14.2.1 build is a useful customer-feature baseline. Tesla’s Q1 update adds context about engineering work already shipped in v14.3. [v14.2.1 release notes, mirrored](https://www.notateslaapp.com/software-updates/version/2025.38.9.6/release-notes) · [Tesla Q1 2026 update, p. 8](https://assets-ir.tesla.com/tesla-contents/IR/TSLA-Q1-2026-Update.pdf#page=8) · [v15.0.1 release notes, mirrored](https://www.notateslaapp.com/software-updates/version/2026.38.6.11/release-notes)

## User

> ok good, i'm good to move to technical tommorrow

## Assistant

**Start the next session with one question. How does v15 use the same camera features to produce driving actions and Reality View?**

- [ ] Sketch the v15 data flow. Cameras feed the Universal Vision Encoder. Its features go to a roughly 3B-parameter driving model that produces actions and a separate roughly 1B-parameter generator that produces the display. For the v14 comparison, label what is known about driving and the display. The sources reviewed do not establish v14’s exact internal wiring. [Musk’s in-car architecture description, mirrored](https://teapawt.net/elonmusk/status/2124458557525397780)
- [ ] Trace one display update. Identify the previous generated frame, the added noise, the conditioning features and the next eight-frame chunk. Use the 18 Hz encoder and 36 fps display figures to distinguish playback cadence from the age of the camera information on screen. Keep the larger off-car world model for synthetic training data separate from the smaller in-car generator. [Musk’s generation and scheduling description, mirrored](https://teapawt.net/elonmusk/status/2124458557525397780)
- [ ] Read Ashok Elluswamy’s discussion of camera-derived 3D Gaussians as background on interpretability. It belongs in the technical reading. Tesla’s exact v15 renderer cannot be inferred from that example alone. Use papers and experiments to explain mechanisms, and attribute shipped features to release-specific sources. [Elluswamy’s account, mirrored](https://nitter.gq/aelluswamy/article/1981644831790379245) · [Technical reading and experiment checklist](https://bos-semi.atlassian.net/wiki/x/DQBQLQ)
