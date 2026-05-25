# US-Japan Floodplain Management Workshop Log

Date logged: 2026-05-17

Purpose: ASFPM 2026 workshop preparation. This note captures recurring comparison points between the U.S. floodplain management system and Japan's water-disaster risk management system.

## Core Framing

The important comparison is not simply "both countries have flood maps." The stronger workshop point is:

> The U.S. links flood mapping, insurance access, mortgage rules, and local floodplain regulation through NFIP. Japan has detailed inundation maps and hazard maps, but they are more strongly tied to evacuation, risk communication, disaster prevention planning, and selective land-use/building controls.

This difference matters because participants may assume that a hazard map is a universal instrument. In practice, the same-looking map can sit inside very different institutional systems.

## U.S. Terms

### FEMA

FEMA is the Federal Emergency Management Agency. It handles federal disaster response and recovery support, and it also manages major flood-risk programs such as NFIP and federal flood maps.

### NFIP

NFIP is the National Flood Insurance Program. It is a federal flood insurance system, but it is also a land-use governance mechanism.

Key institutional point:

- A local community joins NFIP.
- The community adopts and enforces minimum floodplain management regulations.
- Residents and businesses in that community become eligible to purchase NFIP flood insurance.

So NFIP is not only an individual insurance product. It is a bargain between federal insurance availability and local floodplain regulation.

### SFHA

SFHA means Special Flood Hazard Area. It is the high-risk flood zone shown on FEMA flood maps, generally associated with the 1% annual chance flood.

The "1% annual chance" can sound small, but over the life of a building or mortgage it becomes substantial:

- 30 years: about 26% chance of at least one event
- 50 years: about 39%
- 70 years: about 51%

Calculation: `1 - 0.99^years`

### BFE

BFE means Base Flood Elevation. It is the modeled water-surface elevation for the base flood. Many floodplain construction requirements use BFE as a reference point, for example requiring the lowest floor to be elevated to or above BFE.

## What SFHA Maps Mainly Represent

SFHA is not a complete map of every way water can damage a building. It is primarily a regulatory and insurance-oriented flood hazard zone.

Main flood mechanisms reflected:

- Riverine flooding: rivers, streams, and channels overflow after rainfall, snowmelt, or upstream runoff.
- Coastal and lakefront flooding: storm surge, waves, coastal water levels, and Great Lakes-type shoreline hazards.
- Shallow flooding or ponding in some mapped areas.

Important distinction:

Heavy rain is often the trigger, but the real analytical question is the pathway by which water reaches the property.

- River overflow: rain enters the watershed, concentrates in a river, and the river overtops or breaches.
- Coastal/lake flooding: water comes from the coast or lake through surge, waves, or wind setup.
- Pluvial or urban drainage flooding: rain falls locally, drainage capacity is exceeded, and water ponds or flows through streets and buildings.

SFHA/FIRM products are generally stronger for modeled riverine and coastal sources than for local drainage failures, small unmapped catchments, rapidly changing urban runoff, or future climate-driven rainfall extremes.

## NFIP Claims Outside SFHA

The dashboard statement says approximately 25% of NFIP claims come from outside SFHA.

Interpretation:

- This does not mean SFHA is useless.
- It means SFHA boundaries do not capture all practical flood risk.
- "Outside SFHA" does not mean "safe."
- Because flood insurance is often optional outside SFHA, NFIP claims undercount real damage outside the mapped high-risk zone.

More precise wording:

> About 25% of NFIP claim counts originate outside SFHA. Because uninsured flood damage, self-paid repairs, and non-covered losses are not captured in NFIP claims, actual physical damage and economic loss outside SFHA may be larger than the claim data suggest.

Use this when discussing the gap between regulatory flood zones and lived flood risk.

## Japan Comparison

Japan has flood inundation assumption maps and municipal hazard maps, but the institutional architecture is different from the U.S. NFIP/SFHA system.

### Similarities

Japan also maps expected inundation areas and depths. The Ministry of Land, Infrastructure, Transport and Tourism and prefectures publish flood inundation assumption area maps for designated rivers. Municipalities use these to create hazard maps with evacuation information.

Japan's maps can include:

- Flood inundation from river overflow
- Inland water flooding
- Storm surge
- Tsunami
- Sediment disaster risk, depending on map type and local context

### Japan: How National Land Numerical Information Flood Inundation Data Is Made

Important terminology:

> "National Land Numerical Information flood inundation data" is not the original hydraulic model itself. It is a GIS distribution product derived from flood inundation assumption maps prepared by river managers.

Basic production chain:

- River managers, such as MLIT or prefectural governors, prepare flood inundation assumption maps under the Flood Fighting Act framework.
- The underlying maps are based on assumed rainfall scenarios and flood/inundation analysis: river water levels rise, overtopping or levee breach points are assumed, and inundation depth, extent, flow, and duration are simulated across the floodplain.
- National Land Numerical Information then takes the river-manager products, including GIS data, numerical map data, and in some cases map-image data, and converts/organizes them into polygon data by inundation-depth rank.
- Current flood inundation data is organized by river and category, including planned-scale rainfall, assumed maximum-scale rainfall, inundation duration, and house-collapse risk zones caused by flood flow or bank erosion.
- Output formats include GML and shapefiles, with data prepared by regional bureau or prefecture.

Practical interpretation:

> The data is best understood as "scenario-based expected inundation, packaged for GIS use," not as a record of actual past flood damage.

Do not confuse three related but different products:

- Flood inundation assumption area data: pre-disaster scenario data based on assumed rainfall and hydraulic/inundation analysis.
- Flood hazard maps: municipal maps that add evacuation sites, warning transmission, and local emergency information on top of inundation assumptions.
- Actual or estimated inundation maps after an event: post-disaster products based on aerial imagery, satellite imagery, field information, elevation data, or reported damage.

### US-Japan Mapping Data Comparison

| Question | U.S. FEMA/FIRM | Japan National Land Numerical Information / Inundation Maps |
|---|---|---|
| Core unit | Regulatory flood zones such as SFHA, AE/A, X, floodway | Inundation extent, depth rank, duration, and house-collapse risk |
| Main analytical basis | Hydrology/hydraulics, terrain, flood frequency, coastal/riverine modeling | Assumed rainfall scenarios, river/inundation simulations, terrain, breach/overtopping assumptions |
| Public GIS product | FIRM/NFHL-style regulatory map products | GIS packaging of river-manager inundation assumption maps |
| Main output | Zone boundary, BFE, floodway, insurance/regulatory status | Depth polygons, duration, maximum envelope, house-collapse risk zones |
| Institutional use | Insurance, mortgage compliance, floodplain regulation, risk communication | Evacuation, hazard maps, disaster planning, basin policy, selective land-use controls |
| Insurance link | Direct: SFHA can trigger mandatory purchase for federally backed mortgages | Indirect: hazard/inundation maps inform awareness; private water-damage pricing has risk classes but no SFHA-like public mandate |

Workshop wording:

> A U.S. FEMA map draws a legal-insurance boundary; Japan's National Land Numerical Information flood data packages expected inundation depth and related hazard layers for GIS and hazard-map use. Both rely on flood modeling, but the institutional consequences of the map are different.

### Differences

The U.S. system strongly links:

- FEMA flood maps
- SFHA designation
- NFIP insurance availability
- Mandatory purchase requirements in some mortgage contexts
- Local floodplain management standards

Japan does not have the same nationwide structure where a municipality adopts floodplain regulations in exchange for residents' access to a federal flood insurance product.

Japan's system is more centered on:

- Risk communication
- Evacuation planning
- Disaster prevention and local hazard maps
- River basin management and "river basin disaster resilience and sustainability by all" / ryuuiki-chisui
- Selective land-use and building regulation in especially high-risk areas
- Private insurance and mutual-aid products, rather than an NFIP-equivalent national flood insurance program

## US-Japan Insurance Comparison

Short answer for workshop use:

> Japan does not have a U.S.-style SFHA-based mandatory purchase rule. Flood or water-damage coverage is mainly handled through private fire insurance or mutual-aid products, and households choose the coverage package. However, Japan is no longer simply "no risk differentiation": fire insurance reference rates now include regional water-disaster risk classes.

### United States

In the U.S., the insurance question is institutionally tied to the FEMA map.

- SFHA designation can trigger a mandatory flood insurance purchase requirement when a property has a federally backed mortgage.
- NFIP availability depends on local community participation and enforcement of minimum floodplain management rules.
- The map boundary has direct consequences for mortgage compliance, insurance obligations, and local development regulation.

### Japan

Japan does not have an NFIP-equivalent national flood insurance program or a nationwide rule that says "if a home is inside this inundation zone, the owner must buy public flood insurance."

Important nuance:

- Water-damage coverage is usually part of, or attached to, private fire insurance or mutual-aid products.
- Whether to include water-damage coverage is generally a household/product choice, though lenders or individual contracts may require broader property insurance.
- Japan's hazard maps and inundation maps primarily support risk communication, evacuation planning, and local disaster prevention, not a direct federal-style insurance mandate.
- Since the 2023 fire insurance reference-rate revision, water-disaster risk has been classified into five regional classes in the reference-rate framework: class 1 is the lowest risk and class 5 is the highest.
- These Japanese "water-disaster rating classes" are pricing/risk classes, not the same as FEMA flood zones. They include river flooding, inland flooding, and sediment-disaster risk, so they do not necessarily match municipal flood hazard maps.
- Reference rates are not mandatory retail premiums. Insurers can differ in how they design and price products.

Key comparison:

| Question | United States | Japan |
|---|---|---|
| Does the flood map trigger a national/public flood insurance mandate? | Yes, in SFHA for federally backed mortgages | No equivalent national rule |
| Main residential flood insurance structure | NFIP plus private market | Private fire insurance / mutual aid with water-damage coverage |
| Risk zone affects insurance? | SFHA affects mandatory purchase and NFIP/regulatory treatment | Water-disaster risk classes can affect premiums, but not as a public mandatory-purchase boundary |
| Main map function | Regulation, insurance, mortgage compliance, risk communication | Evacuation, risk communication, disaster prevention, selective land-use controls |

Workshop wording:

> Japan is not "no insurance risk classification." It is "no SFHA-like mandatory public flood insurance trigger." The Japanese system increasingly reflects water-disaster risk in private insurance pricing, while the U.S. system uses the FEMA map as a stronger legal and mortgage-compliance boundary.

## Japan: Regulatory Tools That Are Similar But Not Identical

Japan does have tools that can restrict or guide development in high-risk areas.

### Disaster Hazard Areas

Under the Building Standards Act framework, local governments can designate disaster hazard areas by ordinance and restrict residential construction or impose building rules in areas with serious risk from tsunami, storm surge, flooding, and similar hazards.

### Inundation Damage Prevention Areas

Under the Specific Urban River Inundation Countermeasures framework, prefectural governors can designate areas where flood or rainwater inundation could seriously harm life or body. In those areas, certain development and building activities can be subject to prior permission. Requirements can include checking whether living floors are above expected water levels or whether the building has adequate structural safety against flood action.

### River Basin Policy

Japan's "ryuuiki-chisui" approach emphasizes that flood risk is managed not only by river works but by the whole basin: rivers, sewers, land use, storage, infiltration, municipalities, private actors, and residents.

This is a useful comparison point with U.S. NFIP because Japan's institutional emphasis is more basin-wide and planning-oriented, while the U.S. NFIP link is more insurance-and-regulation-oriented.

## Building Importance and Flood Design Levels

Workshop 2 includes ASCE 24-24, which is useful because it shows a more explicit U.S. building-code approach to matching flood design level to building importance.

### United States: ASCE 24-24 / Flood Design Class

ASCE 24-24 uses Flood Design Class, or FDC, to classify buildings by use and consequence of failure:

- FDC 1: low-risk buildings such as agricultural storage and temporary structures.
- FDC 2: ordinary buildings such as typical residential and commercial buildings.
- FDC 3: high-risk buildings such as schools, fire stations, and large assembly facilities.
- FDC 4: essential facilities such as hospitals, emergency operations centers, and critical infrastructure.

The design flood elevation becomes more demanding as the building becomes more important:

- FDC 1: tied to the 1% annual chance flood / 100-year flood.
- FDC 2: tied to the 0.2% annual chance flood / 500-year flood.
- FDC 3: tied to a 750-year mean recurrence interval flood / about 0.13% annual exceedance probability.
- FDC 4: tied to a 1,000-year mean recurrence interval flood / about 0.1% annual exceedance probability.

Practical interpretation:

> ASCE 24-24 makes the design question less binary than "inside or outside SFHA." It asks what the building does during and after a disaster, and then raises the flood design level for buildings whose failure would have larger public consequences.

Important code-status nuance:

- ASCE 24-24 is a technical standard.
- It becomes enforceable where it is referenced by building codes or adopted by a jurisdiction.
- It was not referenced in the 2024 International Building Code / International Residential Code.
- It is expected to enter the 2027 I-Code cycle; until adopted locally, it is best understood as an emerging best-practice reference rather than a universal legal requirement.

### Japan: Similar Logic, More Dispersed Institutions

Japan also has the idea that important facilities need stronger flood, tsunami, or water-disaster protection, but it is not organized like ASCE 24-24 with one national FDC table and explicit 100-year / 750-year / 1,000-year design levels for ordinary building regulation.

Instead, the logic appears across several systems:

- Government buildings and disaster-response bases: MLIT's Government Buildings Department has revised performance expectations for government facilities that conduct disaster emergency response activities, including performance against maximum-assumed rainfall and water disasters.
- Disaster-resilient government facility guidelines: national and local public facilities are treated as important because they support administrative continuity and emergency response.
- Facilities for people requiring special consideration: under the Flood Fighting Act / hazard-map system, underground malls, social welfare facilities, schools, medical facilities, and other facilities listed in municipal disaster management plans must prepare evacuation or inundation-prevention plans and conduct drills.
- Building electrical equipment guidance: for apartments, offices, hospitals, and similar buildings, guidance encourages locating receiving equipment, distribution equipment, emergency generators, and other critical equipment above expected inundation levels or in watertight areas.
- Tsunami and seismic-plus-tsunami standards for government facilities: some government-facility standards explicitly classify required performance by the function of the agency or facility.

Key comparison:

| Question | United States / ASCE 24-24 | Japan |
|---|---|---|
| Main classification | Flood Design Class 1-4 | Government disaster bases, facilities for people requiring special consideration, hospitals, schools, underground spaces, important equipment |
| Design-level expression | FDC-linked flood elevations such as 100-year, 750-year, 1,000-year | Maximum-assumed rainfall/inundation, hazard maps, facility continuity, evacuation/inundation-prevention planning |
| Main policy vehicle | Building code / referenced technical standard | Government facility standards, Water Disaster / Flood Fighting Act planning duties, building equipment guidance, local disaster plans |
| Core concept | More important buildings get more demanding flood design levels | More important or vulnerable-use facilities need stronger function-continuity, evacuation, and equipment-protection measures |

Workshop wording:

> Japan has a similar policy instinct: hospitals, government disaster bases, schools, welfare facilities, underground spaces, and critical equipment should not be treated like ordinary low-consequence buildings. But Japan expresses this through facility-specific standards, disaster-management planning, and equipment guidance, while ASCE 24-24 expresses it more directly as a building-code design classification with higher flood elevations for higher-consequence buildings.

## Early Warning Systems and Action Triggers

Workshop 2 treats Early Warning Systems, or EWS, as part of nonstructural mitigation. The key point is that EWS is not just an alert message. It is the chain from observation and forecast to decision, warning dissemination, and action.

Flood EWS can include:

- Rain gauges, radar rainfall, river gauges, tide gauges, soil moisture, and storm surge forecasts.
- Hydrology and hydraulic forecasts.
- Alert dissemination through phones, radio, sirens, web dashboards, or local emergency channels.
- Evacuation orders, road closures, pump operation, facility shutdown, and deployable flood barrier installation.

### United States

The U.S. has strong warning infrastructure, but response authority is decentralized.

Core pieces include:

- NOAA / National Weather Service flash flood warnings, flood warnings, and watches.
- USGS real-time water-level and flow monitoring.
- FEMA Integrated Public Alert and Warning System, or IPAWS.
- Wireless Emergency Alerts, Emergency Alert System, and NOAA Weather Radio.
- State, county, tribal, and local emergency management.
- Local flood warning and response plans.
- NFIP Community Rating System credits for warning and response activities.

Practical interpretation:

> The U.S. warning architecture is technically strong, but evacuation and response are local-government functions. The national system can transmit warnings, but the conversion from warning to action depends heavily on local emergency managers, county sheriffs, municipal capacity, road networks, public trust, and privately maintained O&M plans.

This matters for Workshop 2 because deployable barriers only work if warning lead time exceeds deployment time. Urban pluvial flooding and flash flooding can leave only minutes, so the practical question is not just "was there a warning?" but "could the required action actually be completed before water arrived?"

### Japan

Japan has a relatively explicit warning-to-evacuation framework.

Core pieces include:

- Japan Meteorological Agency heavy rain warnings, flood warnings, storm surge warnings, emergency warnings, and Kikikuru risk maps.
- MLIT and prefectural river information, including designated river flood forecasts and flood danger levels.
- Municipal evacuation information: elderly people evacuation, evacuation instruction, and emergency safety measures.
- Five alert levels that connect meteorological and hydrological information to recommended protective actions.
- Emergency alert mail, J-Alert, municipal disaster radio, websites, and local disaster management systems.
- Evacuation and inundation-prevention planning requirements for certain underground malls, social welfare facilities, schools, medical facilities, and other facilities requiring special consideration.

Practical interpretation:

> Japan is comparatively strong at linking meteorological and river information to resident evacuation behavior through alert levels and municipal evacuation information.

However, the connection between warnings and building-level nonstructural measures is more uneven:

- Public evacuation behavior is institutionalized.
- Facility-level planning exists for underground spaces and facilities for vulnerable users.
- Some buildings and companies have flood barrier, pump, or equipment-protection procedures.
- But a nationwide standard like "ANSI/FM 2510 deployable barriers + written O&M plan + annual deployment drills" is not generally embedded for ordinary buildings or homes.

Key comparison:

| Question | United States | Japan |
|---|---|---|
| Warning infrastructure | NOAA/NWS, USGS, IPAWS, WEA, NOAA Weather Radio | JMA, MLIT/prefectures, Kikikuru, emergency alert mail, J-Alert, municipal systems |
| Link to evacuation behavior | Often local and decentralized | More explicitly tied to five alert levels and municipal evacuation information |
| Link to flood insurance / regulation | Can be credited in CRS; interacts with NFIP community incentives | Mostly evacuation, disaster management, and facility planning; not an insurance trigger |
| Link to building-level barriers | ASCE 24-24 and ANSI/FM 2510 emphasize O&M plans and annual drills for deployable barriers | Exists in some facilities, but not a broad national standard for ordinary buildings |
| Main weakness | Local implementation capacity and very short warning lead time for flash/pluvial floods | Warning-to-action gaps, evacuation behavior limits, and weaker integration with property-level floodproofing |

Workshop wording:

> Japan is strong at linking meteorological and river information to evacuation behavior. The U.S. also has robust alert infrastructure, but response is more decentralized. The distinctive issue in Workshop 2 is not warning alone; it is whether warnings can reliably trigger building-level nonstructural actions, such as deployable barrier installation, before fast-onset pluvial flooding arrives.

## Workshop Discussion Questions

Useful questions to ask or prepare for:

- If 25% of NFIP claims occur outside SFHA, should floodplain management rely less on a binary inside/outside regulatory line?
- How should flood maps communicate risk when the legally important boundary is sharper than the physical risk gradient?
- What kinds of flood risk are underrepresented in SFHA/FIRM products: urban drainage, pluvial flooding, small streams, old terrain data, climate-change rainfall, or compound flooding?
- In Japan, hazard maps are widely used for evacuation, while water-disaster risk classes can affect private insurance pricing. What would change if hazard maps themselves became stronger insurance or mortgage-compliance triggers?
- In the U.S., NFIP creates a federal-local bargain. What is gained and lost when insurance access depends on local floodplain management compliance?
- Could Japan's basin-wide approach offer lessons for U.S. urban pluvial flooding? Could the U.S. insurance-regulation link offer lessons for Japan's land-use risk governance?
- ASCE 24-24 raises flood design expectations for schools, fire stations, hospitals, EOCs, and critical infrastructure. Would Japan benefit from a more unified FDC-like building classification, or is its facility-specific approach more flexible?
- Japan links warnings to evacuation behavior through alert levels and municipal evacuation information. What would it take to link warnings more directly to property-level floodproofing actions, such as deploying barriers, closing flood gates, or moving critical equipment?
- In the U.S., can deployable barriers be treated as reliable mitigation if warning lead time for urban pluvial flooding is often shorter than realistic deployment time?

## Sources To Revisit

- ASCE 24-24: https://www.asce.org/publications-and-news/codes-and-standards/asce-sei-24-24
- ASFPM, ASCE 24-24 to be included in 2027 I-Codes: https://www.floods.org/news-views/flood-mitigation/asce-24-24-to-be-included-in-the-2027-edition-of-the-international-building-codes/
- FEMA Integrated Public Alert and Warning System: https://www.fema.gov/sq/emergency-managers/practitioners/integrated-public-alert-warning-system
- FEMA Community Rating System: https://www.fema.gov/vi/floodplain-management/community-rating-system
- NOAA / NWS Turn Around Don't Drown: https://www.weather.gov/safety/flood-turn-around-dont-drown
- JMA, weather disaster information and alert levels: https://www.jma.go.jp/jma/kishou/know/bosai/alertlevel.html
- FEMA flood maps: https://www.fema.gov/flood-maps
- FEMA flood insurance: https://www.fema.gov/flood-insurance
- FEMA NFIP participation: https://www.fema.gov/es/node/500081
- MLIT National Land Numerical Information, flood inundation assumption area data: https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-A31a-2024.html
- MLIT flood inundation assumption area map manual: https://www.mlit.go.jp/river/shishin_guideline/pdf/manual_kouzuishinsui_1710.pdf
- MLIT flood inundation assumption maps and hazard maps: https://www.mlit.go.jp/river/bousai/main/saigai/tisiki/syozaiti/
- MLIT specific urban river / inundation damage prevention areas: https://www.mlit.go.jp/river/kasen/tokuteitoshikasen/index.html
- MLIT disaster hazard area system: https://www.mlit.go.jp/jutakukentiku/build/jutakukentiku_house_tk_000144.html
- MLIT sewerage and inland water flooding measures: https://www.mlit.go.jp/mizukokudo/sewerage/crd_sewerage_tk_000117.html
- MLIT, review of inundation performance for disaster-base government buildings: https://www.mlit.go.jp/report/press/eizen04_hh_000025.html
- MLIT, disaster-resilient government facility guidelines: https://www.mlit.go.jp/gobuild/gobuild_tk2_000056.html
- MLIT, flood inundation assumption maps and facility planning duties: https://www.mlit.go.jp/river/bousai/main/saigai/tisiki/syozaiti/
- MLIT, building electrical equipment inundation countermeasures: https://www.mlit.go.jp/jutakukentiku/build/jutakukentiku_house_tk_000132.html
- General Insurance Rating Organization of Japan, fire insurance reference rates: https://www.giroj.or.jp/ratemaking/fire/
- General Insurance Rating Organization of Japan, water-disaster rating class search: https://www.giroj.or.jp/ratemaking/fire/touchi/
- General Insurance Association of Japan, insurance for wind/water/snow disasters: https://www.sonpo.or.jp/insurance/shizen/index.html
- Financial Services Agency, 2024 Insurance Monitoring Report: https://www.fsa.go.jp/news/r6/hoken/20240703/02.pdf
