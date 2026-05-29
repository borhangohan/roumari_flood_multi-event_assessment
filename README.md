## **BEYOND THE GLOBAL BASELINE: SAR FLOOD MAPPING, GFM BENCHMARKING, AND CROP EXPOSURE IN A BANGLADESH CHARLAND FLOODPLAIN** 

A. S. M Borhanul Islam[a, ] ¹, Sumaiya Tabassum[a] , Md Niamal Hafiz[a] , Syed Ahanaf Nakibur Rahman[a] , Md Asif Bin Khaled[a, b, 1,] * 

a Independent University, Bangladesh (IUB), Dhaka, Bangladesh 

b Center for Computational & Data Sciences (CCDS), Independent University, Bangladesh, Dhaka, Bangladesh 

1 These authors contributed equally to this work 

* Corresponding author 

Address: Independent University, Bangladesh Phone: +8801676076329 

Email addresses: iborhan36@gmail.com (Islam, A.S.M Borhanul), ridhisumaiya@gmail.com (Tabassum, Sumaiya), niamalhafiz10@gmail.com(Hafiz, Md Niamal), sanr2001@gmail.com (Rahman, Syed Ahanaf Nakibur), mdasifbinkhaled@iub.edu.bd (Bin Khaled, Md Asif) 

## **Beyond the global baseline: SAR flood mapping, GFM benchmarking, and crop exposure across four monsoon events in a Bangladesh charland floodplain** 

**Abstract:** Charland floodplains in Bangladesh experience severe annual monsoon flooding, yet how  well  global  operational  flood  mapping  services  perform  in  these  vegetated,  sandy environments remains largely untested. This study aimed to benchmark five Synthetic Aperture Radar (SAR)-based flood detection methods against the Copernicus Global Flood Monitoring (GFM) service across four monsoon events (2019–2024) in Roumari Upazila, Kurigram — one of Bangladesh's most chronically flood-exposed charland environments. Sentinel-1 GRD imagery, Sentinel-2, and Landsat-8 optical composites were accessed through Google Earth Engine (GEE).  Five  methods  were  implemented:  VH  and  VV  change  detection,  multi-polarisation frequency fusion, Otsu adaptive thresholding, and a two-stage Random Forest (RF) combining Sen1Floods11 transfer learning with local calibration, validated under dual temporal windows. RF methods led in every event, with Stage 1 RF reaching restricted-window Cohen's Kappa (κ) of up to 0.660 against GFM's κ of 0.490. That gap — 0.17 to 0.19 kappa units — held across all four events regardless of flood magnitude, pointing to a structural ceiling in GFM's VV-only polarisation design. A phenology-aware analysis shows the 2019 event was the most agronomically critical, striking at T. Aman (transplanted monsoon rice) planting onset while Aus (pre-monsoon rice) harvest  was  simultaneously  underway.  These  findings  support  improved  flood  exposure assessment and agricultural early warning systems in South Asian monsoon environments. 

**Keywords:** agricultural  vulnerability,  Bangladesh,  deltaic  floodplain,  Google  Earth  Engine, monsoon flooding, phenology, Random Forest, Sentinel-1 

## **1. Introduction** 

Roumari Upazila covers approximately 200 km² in Kurigram district, Rangpur Division, northern Bangladesh (Fig. 1). It lies within the active Brahmaputra-Jamuna floodplain, one of the world's most morphodynamically active river systems, characterised by flat topography (slopes <5°), seasonally active distributary channels, and charlands subject to continuous lateral migration and bank erosion driven by high Himalayan sediment loads. The Brahmaputra-Jamuna carries approximately 800 million tonnes of sediment annually, producing rapid char formation and destruction cycles that distinguish this environment from more stable deltaic floodplains further south. Agricultural land constitutes approximately 60.5% of the upazila (12,117 ha), dominated by rain-fed T. Aman rice and jute; permanent and seasonal waterbodies (1,885 ha) are excluded from all flood products using the JRC Global Surface Water mask (Pekel et al., 2016). 

Monitoring this from space is harder than it sounds. Cloud cover during peak flood months exceeds 70% (Uddin et al. 2019), which blinds optical sensors at precisely the wrong time. Synthetic Aperture Radar (SAR) bypasses this: microwave pulses pass through cloud cover day and night, making it the only workable option for real-time flood mapping during monsoon (Portalés-Julià et al. 2025). 

Sentinel-1 acquires SAR data in two co-polarisation channels. VV (vertical-vertical) responds strongly to open water. VH (vertical-horizontal) penetrates crop canopies and picks up scattering from flooded fields beneath vegetation, especially relevant in rice-growing areas like Roumari (Tsyganskaya  et  al.  2018).  Threshold-based  detection  has  been  applied  widely  across 

Bangladesh (Uddin et al. 2019; Aziz et al. 2022; Singha et al. 2020; Pandey et al. 2022), though on sandy charlands, Otsu adaptive thresholding can misread smooth dry surfaces as water (Islam and Meng 2022). Random Forest classifiers handle this better by combining multiple features simultaneously; Toma et al. (2024) found consistent RF gains over threshold methods in vegetated floodplains. Where local training data are thin, the Sen1Floods11 dataset (Bonafilia et al. 2020) provides pre-trained classifiers, and ensemble combinations can push accuracy further still (Kar et al. 2024). 

The Copernicus Global Flood Monitoring (GFM) service (Wagner et al. 2024) is the dominant operational system globally, processing Sentinel-1 at scale and delivering maps within five hours of acquisition. GFM runs three algorithms through a majority ensemble and applies a global exclusion mask covering around 70% of land area. It uses VV polarisation only, a deliberate choice that favours pipeline consistency and false-alarm suppression worldwide, accepting lower accuracy in vegetated environments as the trade-off. The developers say so explicitly (Wagner et al. 2024). Independent evaluations put numbers on that gap: Risling et al. (2024) found a related JRC model correctly identified only around 40% of flooded pixels in global tests; Misra et al. (2025) showed a locally trained deep learning model outperformed GFM by a substantial F1 margin. Detection method choice also compounds over time, and a decade-long SAR archive documented roughly 71% more flood-affected area than optical products (Chapman et al. 2015). 

This study does not dispute GFM's operational value at planetary scale. Wagner et al. (2026) established that GFM's uniform exclusion masks and VV-only processing experience significant limitations in morphodynamically complex landscapes, and explicitly identified the need for sitespecific evaluations to quantify these limitations. This study addresses that gap directly: when GFM's global design constraints meet one of South Asia's most flood-vulnerable charland floodplains, does one size still fit all? The specific objectives are: (1) to map flood extent across four monsoon events using five SAR-based detection methods; (2) to compare method accuracy under dual temporal validation windows; (3) to benchmark all methods against GFM; (4) to characterise inter-event variability in flood extent and method agreement; and (5) to assess cropland flood exposure by phenological stage using the local crop calendar. 

## **1.1 Study area** 

Roumari Upazila covers roughly 200 km² in Kurigram district, Rangpur Division (Fig. 1). The terrain is flat, with slopes below 5°, cut through by distributary channels and morphodynamically unstable  charlands  typical  of  the  active  Brahmaputra-Jamuna  floodplain.  Agricultural  land accounts for about 60.5% of the upazila (12,117 ha), mostly rain-fed T. Aman rice and jute; permanent and seasonal waterbodies (1,885 ha) are excluded from all flood products using the JRC Global Surface Water mask (Pekel et al. 2016). The four study events span a range of drivers and timings: the 2019 and 2020 floods were driven by Brahmaputra-Jamuna discharge peaks, the 2023 event reflected elevated late-monsoon river levels, and the 2024 flood arrived early in the season as a flash event. This range is what a single-event study cannot offer. 

**Figure 1: Study area map** 

## **1.2 Site selection and flood event windows** 

Roumari Upazila was identified through a three-stage selection protocol. Stage 1 drew on Bangladesh Water Development Board (BWDB) Flood Forecasting and Warning Centre (FFWC) vulnerability maps to identify the 40 highest flood-exposure hydrological stations nationally. Stage 2 cross-referenced these against the published literature, keeping only sites with fewer than three to five existing SAR or optical flood mapping studies. Stage 3 compiled FFWC station records for two indicators: Danger Level Margin (DLM), defined as the difference between the prevailing water level and the officially declared danger threshold, and Historical Severity Margin (HSM), defined as the difference between the recorded highest water level and the danger level. A low DLM indicates that the station regularly approaches or exceeds its danger threshold, while a high HSM indicates that historically severe floods have substantially exceeded it. Together these two indicators identify stations that are both chronically vulnerable and historically prone to extreme events; the full candidate matrix appears in Table 1. Boalmari station in Roumari had the lowest DLM (2.3 m) and direct Brahmaputra-Jamuna exposure, making it the most chronically floodvulnerable understudied candidate in the set. Fig. 2 plots the DLM distribution across all shortlisted stations and places Boalmari's position in that range. Flood event windows were defined  from  Sentinel-1  acquisition  dates  within  GFM  that showed  the  greatest  sustained inundation extent, so all methods were evaluated over a consistent temporal footprint. 

**Table 1. FFWC hydrological indicator matrix for shortlisted upazila candidates. Roumari selected on minimum DLM and direct Brahmaputra exposure.** 

|**Station**|**Present WL**<br>**(mMSL)**|**Danger**<br>**Level**<br>**(mMSL)**|**RHWL**<br>**(mMSL)**|**RHWL Date**|**DLM (m)**|**HSM (m)**|**Selection**|
|---|---|---|---|---|---|---|---|
|Singra|9.91|12.2|12.84|30 Sept 2020|2.29|0.64|—|
|**Boalmari**|**21.6**|**23.9**|**24.57**|**25 Aug 1998**|**2.3**|**0.67**|**✓**<br>**SELECTE**<br>**D**|
|Kalmakanda|3.27|6.55|12.6|30 May2006|3.28|6.05|—|
|Narsingdi|1.25|5.25|6.21|25 July2004|4.00|0.96|—|
|Thakurgaon|45.76|49.95|50.87|12 Aug2017|4.19|0.92|—|
|Nakuagaon|16.98|21.95|25.00|15 Aug2011|4.97|3.05|—|
|Chapai-|14.24|20.55|22.39|09 Sept 1998|6.31|1.84|—|



|Nawabganj||||||||
|---|---|---|---|---|---|---|---|
|Rohanpur|14.43|21.55|23.65|10 Sept 1998|7.12|2.10|—|
|Hatboalia|6.32|14.05|14.67|16 Sept 1984|7.73|0.62|—|



− Note: WL = Water Level; DLM = Danger Level Margin (danger level present water level); HSM = Historical Severity − Margin (RHWL danger level). FFWC station data compiled from Bangladesh Water Development Board records. Smaller DLM indicates higher chronic flood vulnerability. 

**Figure 2. Danger level margin (m) at selected hydrological stations across Bangladesh** 

**Figure 3. Distribution of Sentinel-1 SAR baseline and flood scenes across flood event years** 

## **2. Data** 

All data were accessed and processed within Google Earth Engine (GEE). Table 2 lists the datasets used across all four events; event-specific acquisition windows and Otsu threshold values are in Table 3. 

## **Table 2. Datasets used across all four flood events.** 

|**Dataset**|**Source**|**Period**|**Resolution**|**Role**|
|---|---|---|---|---|
|Sentinel-1 GRD<br>(IW, ASC)|ESA/GEE|Mar–Apr (baseline); flood<br>windowper event|10 m|Primary flood signal|
|Sentinel-2 SR<br>Harmonized|ESA/GEE|Extended Jun–Aug/Sep;<br>restricted = flood window|10–20 m|Optical reference|
|Landsat-8 OLI/TIRS|USGS/GEE|Same as S2|30 m|Optical reference|
|MODIS MOD09GA|NASA/GEE|Extended window per<br>event|500 m|Synoptic check|
|JRC Global Surface<br>Water v1.4|JRC/GEE|1984–2020|30 m|Permanent water / river mask|
|SRTM DEM|USGS/GEE|~2000|30 m|Slope & TWI|
|ESA WorldCover<br>v200|ESA/GEE|2021|10 m|Cropland mask|
|GFM (Copernicus<br>EMS)|JRC/GFM|Per-event operational|30 m|Operational benchmark|
|Sen1Floods11<br>(India subset)|Bonafilia et al.<br>2020|Historical (68 chips)|10 m|Stage 1 RF training|



**Table 3. Event-specific acquisition windows, scene counts, and adaptive threshold values.** 

|**Event**|**Flood window**|**Ext. opt.**<br>**window**|**S1 flood**<br>**scenes**|**Otsu VH**<br>**(dB)**|**Otsu VH abs**<br>**(dB)**|**Otsu VV (dB)**|
|---|---|---|---|---|---|---|
|2019|19 Jul–02 Aug<br>2019|15 Jun–25 Aug<br>2019|3|4.60<br>−|21.40<br>−|2.60<br>−|
|2020|21 Jun–27 Jul<br>2020|01 Jun–15 Aug<br>2020|4|3.50<br>−|20.10<br>−|3.00<br>−|
|2023|05 Aug–29 Aug<br>2023|15 Jul–15 Sep<br>2023|2|4.20<br>−|20.20<br>−|2.70<br>−|
|2024|22 Jun–16 Jul<br>2024|01 Jun–31 Jul<br>2024|2|5.50<br>−|22.60<br>−|2.30<br>−|



## **2.1 Sentinel-1 SAR** 

Sentinel-1 is a C-band (5.405 GHz) SAR constellation operated by the European Space Agency (ESA) under the Copernicus programme. Data were acquired in Interferometric Wide (IW) swath mode at 10 m Ground Range Detected (GRD) resolution across two co-polarisation channels: VV and VH. Prior to the December 2021 failure of Sentinel-1B, the constellation maintained a 6-day repeat cycle over Bangladesh. After that failure, only Sentinel-1A remained operational, cutting the revisit interval to 12 days. This directly affected the 2023 and 2024 events, where only two SAR flood scenes were available, against three and four in 2019 and 2020 respectively (Table 3). Fig. 3 shows baseline and flood scene distributions across all four events, with the drop in temporal density after the Sentinel-1B failure clearly visible. 

All imagery was accessed in ascending-pass geometry via the GEE data catalogue. Preprocessing applied by ESA prior to GEE ingestion includes thermal noise removal, radiometric calibration, and terrain correction using the Shuttle Radar Topography Mission Digital Elevation Model (SRTM DEM). A pre-monsoon baseline composite was constructed from scenes acquired 

between 1 March and 30 April of each year (4 to 5 scenes), and per-scene VH and VV backscatter change (dB) was then calculated against this baseline. 

Figs 4a and 4b show Sentinel-1 VV and VH backscatter for baseline and flood periods across all four  events.  The  backscatter  depression  between  baseline  and  flood  scenes  is  apparent throughout: dark returns in flood scenes correspond to standing water and shallow inundation over the active char surface, while brighter returns indicate dry or vegetated land. VH panels show a more spatially heterogeneous depression pattern than VV, which follows from VH's greater sensitivity to volume scattering beneath crop canopies. In 2019 and 2020 the spatial extent of backscatter depression is visibly larger than in 2023 and 2024, reflecting both higher flood magnitudes and the greater number of available SAR scenes in the earlier events. 

**Figure 4a. Sentinel-1 SAR VH backscatter during baseline and flood (2019, 2020, 2023, 2024)** 

**Figure 4b. Sentinel-1 SAR VV backscatter during baseline and flood periods (2019, 2020, 2023,** 

**2024)** 

## **2.2 Optical reference and ancillary data** 

Optical composites for validation were built from Sentinel-2 Surface Reflectance Harmonized (10 to 20 m) and Landsat-8 OLI/TIRS (30 m) imagery (Fig. 7). Water pixels were identified using the Normalised Difference Water Index (NDWI; Green/NIR > 0.3) and Modified NDWI (MNDWI; Green/SWIR > 0.3), with per-scene cloud masking via the Sentinel-2 Scene Classification Layer (SCL) and Landsat-8 QA_PIXEL band. Two composite windows were constructed per event: an extended window of roughly 10 weeks centred on the flood peak, and a restricted window coinciding with SAR acquisition dates. MODIS MOD09GA (500 m) provided a synoptic crosscheck. 

The JRC Global Surface Water v1.4 product (Pekel et al., 2016) masked permanent and seasonal waterbodies from all flood products. Slope and Topographic Wetness Index (TWI) layers were extracted from the SRTM DEM (30 m). The ESA WorldCover v200 product (10 m, 2021) defined the cropland mask for agricultural exposure analysis. The GFM product, accessed via the JRC GFM portal at 30 m resolution, is the operational benchmark throughout. Sen1Floods11 training data (Bonafilia et al., 2020), specifically the India subset of 68 chips, provided 65,902 labelled SAR flood pixels for Stage 1 RF training. 

## **3. Methods** 

The methodological framework is summarised in Fig. 5. Four stages make up the pipeline: Sentinel-1 SAR pre-processing and baseline construction, flood detection using five methods, 

post-processing and binary map generation, and accuracy assessment against dual-window optical references. All four stages were applied consistently across each flood event. 

**Figure 5. Overview of the methodological framework for SAR-based flood detection,** 

**classification, and validation across four flood events** 

## **3.1 Flood mapping methods** 

Five flood detection methods were implemented per event. A sixth layer, the VV/VH backscatter ratio change, was computed as a negative control: when both VV and VH backscatter decrease simultaneously over open water, the ratio stays flat and suppresses rather than enhances the flood signal. This layer was included to confirm that failure mode empirically. 

Figs 6a and 6b show Sentinel-1 VV and VH backscatter for baseline and flood periods across all four  events.  The  circular  outlines  in  the  baseline  panels  mark  charland  boundaries  with characteristically smooth backscatter that sharpens into darker, more uniform returns during flooding. The transition is most pronounced in 2019 and 2020, where flood extent covers a larger fraction of the study area. In 2023 and 2024 the flood signal is spatially more constrained, consistent with lower scene counts following the Sentinel-1B failure. VH flood panels show darker and  more  spatially  consistent  returns  over  central  char  zones  than  VV  panels,  a  pattern attributable to cross-polarisation sensitivity to water beneath early-stage rice canopies. 

**Figure 6a. Sentinel-1 SAR backscatter comparison for 2019 and 2020 showing VV baseline (a), VV** 

**flood (b), VH baseline (c), and VH flood (d) scenes over the study area.** 

**Figure 6b. Sentinel-1 SAR backscatter comparison for 2023 and 2024 showing VV baseline (a), VV** 

**flood (b), VH baseline (c), and VH flood (d) scenes over the study area.** 

VH change detection computes the difference in VH backscatter between each flood scene and the pre-monsoon baseline, applies an Otsu adaptive threshold to the mean VH change image, and retains the maximum flood extent across all available scenes. 

VV change detection follows the same procedure using VV backscatter. VV responds more strongly to open water on rougher surfaces and beneath early-stage vegetation, so it picks up flooding that VH alone may miss. 

Multi-polarisation frequency fusion (MultiPol) combines binary flood detections from both VV and VH channels into a single observation stack. Pixels flagged as flooded by one or both polarisation channels in at least half of all observations are retained. This reduces false detections from individual noisy scenes while drawing on dual-polarisation evidence. 

Otsu thresholding applies the Otsu criterion directly to the scene-mean VH backscatter change histogram, producing a single binary flood map per event without a baseline comparison. 

Stage 1 RF uses a Random Forest classifier trained on 65,902 flood-labelled pixels from 68 Sentinel-1 image chips in India from the Sen1Floods11 dataset (Bonafilia et al., 2020), applied in probability mode to produce a continuous flood likelihood layer (s11_prob) for each event. 

Stage 2 RF trains a second Random Forest using flood labels derived from the Otsu result for that event, with eleven input features: VH backscatter, VV backscatter, VH change, VV change, VH/VV ratio, slope, topographic wetness index, pre-flood NDVI, NDWI, MNDWI, and s11_prob. Including s11_prob lets Stage 2 draw on generalised flood knowledge from India while adapting to local conditions. Training samples were drawn from spatial zones independent of validation points to prevent data leakage. 

A majority-vote ensemble of all five primary methods was computed as a combination benchmark. 

## **3.2 Post-processing** 

All binary flood maps were passed through the same post-processing sequence. This comprised morphological smoothing, slope masking above 5°, texture masking of urban surfaces, exclusion of dense vegetation where pre-flood NDVI exceeded 0.4, masking of permanent and seasonal river channels using the JRC Global Surface Water dataset (Pekel et al., 2016), and removal of flood patches smaller than 9 pixels (900 m²). A probability threshold of 0.5 was applied to both RF outputs. 

## **3.3 Accuracy assessment** 

Accuracy was assessed against optical water masks (Fig. 7) under two temporal windows per event. The extended window spans roughly ten weeks centred on the flood peak. The restricted window matches SAR acquisition dates; this places all methods on the same temporal footing as GFM and is the more direct of the two comparisons. Water pixels were − identified using the Normalised Difference Water Index (NDWI = (Green NIR) / (Green + − NIR) > 0.3) and Modified NDWI (MNDWI = (Green  SWIR) / (Green + SWIR) > 0.3). Stratified random sampling generated 200 validation points per event, drawn equally from flooded and non-flooded classes. Producer's accuracy, user's accuracy, and balanced accuracy are 

reported with 95% confidence intervals. F1-score and Cohen's Kappa (κ) are reported with 95% confidence intervals derived by resampling the validation point set 1,000 times. 

**Figure 7. Distribution of Sentinel-2 (extended and restricted) and Landsat-8 optical reference** 

## **scenes across flood event years (2019–2024).** 

## **3.4 Phenology-aware agricultural exposure** 

Agricultural flood exposure was quantified by intersecting each event's flood maps with the ESA WorldCover v200 cropland mask in GEE, giving inundated cropland area per method and event. Results were then interpreted against the local crop calendar for Kurigram District, covering T. Aman rice, Aus rice, Boro rice, wheat, and mustard (Miah, 2022; Shariot-Ullah, 2016; Khanam and Biswas, 2022; Islam and Sikder, 2011; Madhu et al., 2018; Sikder, 2009). Flood timing relative to phenological stage shapes agronomic impact, with early-season inundation generally more damaging than flooding that occurs at or after harvest. 

The AI tool Claude (Anthropic) assisted with manuscript preparation to improve clarity and readability. All content was reviewed and edited by the authors, who take full responsibility for the published work. 

## **4. Results** 

## **4.1 Flood extent** 

Mapped flood extents for all methods across all four events are in Table 4 and Fig. 8. In 2019 and 2020, SAR methods mapped between 83% and 110% of GFM extent, a range consistent with broad spatial agreement. In 2023 and 2024, SAR methods substantially overestimated GFM, with ratios ranging from 1.28 to 1.84. The most plausible explanation is GFM's global exclusion masks suppressing detections in charland zones, where sandy substrates and shallow water produce 

similar backscatter signatures. The VV/VH ratio method failed systematically across all four events, returning extents between 344 and 4,115 ha, and is excluded from further analysis. 

**Figure 8. Flood extent (hectares) derived from multiple SAR-based different methods and GFM** 

**across four flood events (2019–2024).** 

**Table 4. Mapped flood extents (ha) and ratio to GFM per method and event. S2+L8 optical extents** 

**shown as reference.** 

|**Method**|**2019 (ha)**|**vs GFM**|**2020**<br>**(ha)**|**vs GFM**|**2023**<br>**(ha)**|**vs GFM**|**2024 (ha)**|**vs GFM**|
|---|---|---|---|---|---|---|---|---|
|VH Change|5,265|0.83|5,639|0.91|3,640|1.32|4,688|1.38|
|VV Change|5,860|0.93|6,758|1.10|3,955|1.43|5,553|1.63|
|MultiPol|5,040|0.80|5,762|0.93|3,796|1.38|4,659|1.37|
|Otsu|5,489|0.87|6,601|1.07|4,172|1.51|4,687|1.38|
|Stage 1 RF|5,858|0.93|6,748|1.09|5,071|1.84|5,942|1.74|
|Stage 2 RF|5,284|0.84|6,332|1.03|4,450|1.61|4,374|1.28|
|Ensemble|5,349|0.85|6,194|1.00|4,065|1.47|4,688|1.38|
|**GFM**<br>**(benchmark)**|**6,306**|**1.00**|**6,170**|**1.00**|**2,758**|**1.00**|**3,409**|**1.00**|



## **4.2 Accuracy — extended and restricted validation** 

Accuracy metrics for all methods across the four events are presented in Table 5, Fig. 9 (Balanced Accuracy), and Fig. 10 (F1-score). RF methods outperformed all threshold approaches and GFM in every event. Stage 2 RF led in 2019 (κ = 0.680) while Stage 1 RF led in 2020, 2023, and 2024, with kappa values of 0.600, 0.760, and 0.640 respectively. VV Change ranked second in 2023 (κ = 0.660) and outperformed VH Change across all four events. MultiPol tracked closely with VH Change and Otsu throughout. The Ensemble did not outperform the best individual method in any event; majority voting appears to dilute the RF signal when weaker threshold methods dominate the vote. GFM was the weakest performer overall. Its worst result came in 2019 (κ = 0.040) under the extended window, driven by temporal mismatch between the broad 10-week optical composite and GFM's narrow acquisition footprint rather than a purely spatial failure. 

The 2019 event showed the widest spread between methods under the extended window, with GFM's κ of 0.040 reflecting temporal mismatch between the broad 10-week optical composite and GFM's narrow acquisition footprint rather than genuine spatial failure — the restricted window recovers GFM to κ = 0.490 for the same event. Stage 2 RF led in 2019 (κ = 0.680), likely because the stronger backscatter contrast during this high-magnitude riverine flood allowed the locally calibrated classifier to better separate flooded agricultural fields from surrounding dry char surfaces. In 2020, Stage 1 RF led (κ = 0.600) with Stage 2 RF tracking closely (κ = 0.590), suggesting the generalised Sen1Floods11 prior was sufficiently informative for this event and local calibration added marginal rather than substantial improvement. The 2023 event produced the highest absolute kappa values across all methods — Stage 1 RF reaching κ = 0.760 — consistent with favourable backscatter contrast during the late-monsoon flood and a higher proportion of clearly inundated agricultural pixels in the validation set. In 2024, all methods showed slightly reduced kappa values compared to 2023 despite similar flood extents, most likely reflecting the early-monsoon timing when pre-flood NDVI values are lower and the distinction between flooded and non-flooded bare char surfaces is less pronounced in both SAR and optical references. 

The restricted window places all methods on the same temporal footing as GFM. Table 6 summarises kappa values for the best method, VV Change, MultiPol, GFM, and the Ensemble across both windows and all four events; Fig. 11 shows corresponding crop damage extents. RF methods led under the restricted validation in every event. The kappa advantage over GFM held between 0.17 and 0.19 units across events of varying magnitude and timing: 2019 (Δκ = +0.170), 2020 (Δκ = +0.190), 2023 (Δκ = +0.180), and 2024 (Δκ = +0.210). VV Change also outperformed GFM under the restricted window in every event, a finding that suggests a single additional polarisation channel, with no training data, is sufficient to improve on the operational baseline. 

**Figure 9. Balanced Accuracy (%) Comparison of different Methods by Year (2019–2024)** 

**Table 5. Cohen's Kappa (κ) for all methods across four flood events (extended optical validation, n** 

|**Event**|**VH**|**VV**|**MultiPol**|**Otsu**|**St1 RF**|**St2 RF**|**Ensemble**|**GFM**|
|---|---|---|---|---|---|---|---|---|
|**2019**|0.500|0.580|0.450|0.510|0.640|**0.680**|0.520|0.040|
|**2020**|0.390|0.550|0.410|0.430|**0.600**|0.590|0.420|0.250|
|**2023**|0.560|0.660|0.630|0.620|**0.760**|0.750|0.620|0.300|
|**2024**|0.500|0.550|0.480|0.500|**0.640**|0.590|0.500|0.410|



Note: Bold = best performing method per event. Full accuracy metrics (BA, PA, UA, F1) available in Supplementary Table S1. 

**Figure 10. F1 Score (%) Performance of Different Methods Across Years (2019–2024)** 

## **4.3 Spatial comparison with GFM** 

Agreement areas and GFM capture rates for each SAR method are in Table 7 and Fig. 12. Stage 1 RF achieved the highest GFM capture rates in every event, from 56.8% in 2019 to 73.2% in 2024. VV Change captured 1 to 2 percentage points more of GFM extent than VH Change across all events. Fig. 12 shows the spatial breakdown into three agreement categories: Agree, Ouronly, and GFM-only. The Agree fraction is consistently the largest component. The Our-only fraction is visibly larger in 2023 and 2024, which aligns with the SAR overestimation of GFM observed in those events. GFM-only pixels fell outside permanent water body classifications in 34% to 64% of cases, with a JRC-wet fraction of only 1% to 5%. These figures point to genuinely shallow agricultural inundation sitting below the sensitivity threshold of all polarisation-based approaches tested here. 

**Figure 11. Crop damage extent (hectares) by different methods and GFM across four flood events (2019–** 

## **2024).** 

**Table 6. Cross-event kappa summary: best method, VV Change, MultiPol, GFM, and Ensemble on extended** 

**and restricted validation sets.** 

|**Method /**<br>**Event**|**2019**<br>**Ext**|**2019**<br>**Res**|**2020**<br>**Ext**|**2020**<br>**Res**|**2023**<br>**Ext**|**2023**<br>**Res**|**2024**<br>**Ext**|**2024**<br>**Res**|**Method /**<br>**Event**|
|---|---|---|---|---|---|---|---|---|---|
|Best method<br>(kappa)|St2 RF<br>0.680|St1 RF<br>0.660|St1 RF<br>0.600|St1 RF<br>0.630|St1 RF<br>0.760|St2 RF<br>0.500|St2 RF<br>0.640|St1 RF<br>0.540|Best<br>method<br>(kappa)|
|GFM (kappa)|0.040|0.490|0.250|0.440|0.300|0.320|0.410|0.330|GFM<br>(kappa)|
|Delta-kappa<br>(best - GFM)|+0.640|+0.170|+0.350|+0.190|+0.460|+0.180|+0.230|+0.210|Delta-<br>kappa<br>(best -<br>GFM)|



## **4.4 Phenology-aware agricultural exposure** 

Cropland-flooded areas per method and event are shown in Table 7 and Fig. 13. Total cropland in the study area is 12,117 ha. GFM produced the highest cropland exposure estimate in 2019 (5,971 ha; 49.3%), exceeding all SAR methods, yet severely underestimated cropland exposure in 2023 (2,452 ha; 20.2%) and 2024 (3,137 ha; 25.9%) relative to all local methods. The reversal coincides with the halving of Sentinel-1 temporal density after the Sentinel-1B failure in December 2021. Fig. 13 shows comparative cropland flood maps for GFM and VV Change across all four events. In 2019, GFM-only pixels are distributed across the central and southern char zones, in 

line with GFM's higher cropland exposure estimate. In 2023 and 2024, VV-only pixels dominated the  northern  agricultural  belt,  where  VV  Change  produced  substantially  higher  exposure estimates than GFM. The both-agree fraction shrinks visibly between the 2019 to 2020 and 2023 to 2024 panels, consistent with reduced spatial agreement in the later events. 

Cross-referencing flood event windows against the crop calendar in Table 8 reveals substantial differences in agronomic severity. The 2019 event (19 Jul to 02 Aug) hit at T. Aman transplanting onset while Aus rice harvest was simultaneously underway, the most vulnerable timing of the four. The 2020 event (21 Jun to 27 Jul) produced the largest absolute cropland inundation across most SAR methods but fell before T. Aman transplanting began, with losses concentrated in Aus harvest and land preparation. The 2023 event (05 Aug to 29 Aug) overlapped with later transplanting stages, when established seedlings tolerate inundation better. The 2024 event (22 Jun to 16 Jul) concluded before transplanting started and had no direct impact on standing T. Aman crops. None of the four events overlapped with standing Rabi season crops. Flooded cropland area alone, then, is not sufficient to judge agronomic impact; phenological stage at flood timing matters just as much. 

**Table 7. Spatial comparison: agreement areas (ha) and GFM capture rates across all four events.** 

|**Method**|**2019**<br>**Agre**<br>**e**|**2019**<br>**GFM-**<br>**cap**|**2020**<br>**Agree**|**2020**<br>**GFM-cap**|**2023**<br>**Agre**<br>**e**|**2023**<br>**GFM-cap**|**2024**<br>**Agree**|**2024 GFM-**<br>**cap**|**Non-JRC**<br>**avg**|
|---|---|---|---|---|---|---|---|---|---|
|VH Change|3,403|53.8<br>%|3,651|59.1%|1,451|52.9%|2,304|67.6%|48%|
|VV Change|3,567|56.4<br>%|3,930|63.6%|1,538|56.0%|2,431|71.3%|51%|
|MultiPol|3,325|52.6<br>%|3,712|60.1%|1,519|55.3%|2,336|68.5%|49%|
|Otsu|3,475|55.0<br>%|3,875|62.8%|1,543|56.2%|2,304|67.6%|49%|
|Stage 1 RF|3,591|56.8<br>%|3,929|63.6%|1,674|61.0%|2,496|73.2%|49%|
|Stage 2 RF|3,399|53.8<br>%|3,850|62.3%|1,623|59.1%|2,273|66.6%|49%|
|Ensemble|3,438|54.4<br>%|3,814|61.8%|1,535|55.9%|2,304|67.6%|48%|
|Note: GFM capture = agreement area / GFM total area. Non-JRC avg = GFM-only pixels outside JRC-classified water areas,||||||||||



averaged across events. VV/VH Ratio excluded due to systematic failure. 

**Figure 12. Comparison of flood-mapped area (ha) by detection method across agreement categories (Agree,** 

**Our-only, and GFM-only)** 

**Table 8. Cropland-flooded area (ha) and percentage of total cropland (12,117 ha) per method and event.** 

|**Method**|**2019 Crop**<br>**ha**|**%**|**2020 Crop**<br>**ha**|**%**|**2023 Crop**<br>**ha**|**%**|**2024 Crop**<br>**ha**|**%**|
|---|---|---|---|---|---|---|---|---|
|VH Change|4,730|39.0%|5,224|43.1%|3,184|26.3%|4,216|34.8%|
|VV Change|5,108|42.2%|5,873|48.5%|3,383|27.9%|4,947|40.8%|
|MultiPol|4,570|37.7%|5,224|43.1%|3,304|27.3%|4,272|35.3%|
|Otsu|4,899|40.4%|5,822|48.0%|3,630|30.0%|4,216|34.8%|
|Stage 1 RF|5,117|42.2%|5,884|48.6%|4,287|35.4%|5,204|43.0%|
|Stage 2 RF|4,662|38.5%|5,567|45.9%|3,784|31.2%|3,861|31.9%|
|Ensemble|4,784|39.5%|5,544|45.8%|3,538|29.2%|4,216|34.8%|
|GFM<br>(benchmark)|5,971|49.3%|5,799|47.9%|2,452|20.2%|3,137|25.9%|



**Table 9. Crop phenology calendar for Kurigram District and surrounding area. Flood event windows: 2019 (19** 

**Jul-02 Aug), 2020 (21 Jun-27 Jul), 2023 (05 Aug-29 Aug), 2024 (22 Jun-16 Jul).** 

|**Crop Category**|**Crop / Variety**|**Phenological Stage**|**Estimated Dates**|**Reference**|
|---|---|---|---|---|
|Rice (Boro Season)|General Boro Rice|Sowing / Planting|December -<br>February|Miah (2022);<br>Shariot-Ullah<br>(2016)|
||General Boro Rice|Vegetative / Tillering|January - March|Miah (2022);<br>Shariot-Ullah<br>(2016)|
||General Boro Rice|Reproductive (PI,<br>Heading, Flowering)|March - April|Miah (2022);<br>Shariot-Ullah<br>(2016)|
||General Boro Rice|Maturity / Harvesting|April - May|Miah (2022);|



|||||Shariot-Ullah<br>(2016)|
|---|---|---|---|---|
||BRRI dhan81/89|Seedling Establishment|Mid-Nov - Late Dec|Khanam &<br>Biswas (2022);<br>Miah (2022)|
||BRRI dhan81/89|Tiller Formation|Late Dec - Early<br>Feb|Khanam &<br>Biswas (2022);<br>Miah (2022)|
||BRRI dhan81/89|Panicle Initiation (PI)|February - Early<br>March|Khanam &<br>Biswas (2022);<br>Miah (2022)|
||BRRI dhan81/89|50% Flowering|Late March - Early<br>April|Khanam &<br>Biswas (2022);<br>Miah (2022)|
||BRRI dhan81/89|Maturity / Harvesting|Mid-April - May|Khanam &<br>Biswas (2022);<br>Miah (2022)|
|Rice (T. Aman<br>Season)|T. Aman Rice|Sowing / Transplanting|20 July - 30 August|Shariot-Ullah<br>(2016)|
||T. Aman Rice|Reproductive (PI,<br>Booting, Flowering)|September -<br>October|Shariot-Ullah<br>(2016)|
||T. Aman Rice|Maturity / Harvesting|October -<br>December|Shariot-Ullah<br>(2016)|
||Fine Rice (e.g.,<br>Rajshahi Swarna)|Seedling Establishment|Late July - Early<br>August|Islam & Sikder<br>(2011)|
||Fine Rice|Initial Tillering|Mid-August|Islam & Sikder<br>(2011)|
||Fine Rice|Booting to Anthesis|Late Sep - Mid-Oct|Islam & Sikder<br>(2011)|
||Fine Rice|Maturity / Harvesting|Late Oct - Early<br>Nov|Islam & Sikder<br>(2011)|
|Rice (Aus Season)|Aus Rice|Sowing / Planting|March - May|Shariot-Ullah<br>(2016)|
||Aus Rice|Maturity / Harvesting|July - August|Shariot-Ullah<br>(2016)|
|Wheat|BARI Gom / Shatabdi|Sowing|15 Nov - 15 Dec|Miah (2022);<br>Madhu et al.<br>(2018); Sikder<br>(2009)|
||BARI Gom / Shatabdi|Crown Root Initiation<br>(CRI)|December|Miah (2022);<br>Madhu et al.<br>(2018); Sikder<br>(2009)|
||BARI Gom / Shatabdi|Heading / Anthesis|February|Miah (2022);<br>Madhu et al.<br>(2018); Sikder<br>(2009)|
||BARI Gom / Shatabdi|Maturity / Harvesting|March - April|Miah (2022);<br>Madhu et al.<br>(2018); Sikder|



|||||(2009)|
|---|---|---|---|---|
|Cash Crops|Mustard|Sowing / Planting|November|Miah (2022)|
||Mustard|Maturity / Harvesting|January - February|Miah (2022)|
||Mango|Sowing / Planting|July|Miah (2022)|
||Watermelon|Harvesting|April - May (Rabi<br>season)|Miah (2022)|



**Figure 13. Comparative cropland flood maps derived from GFM and VV for four flood events - (a) 2019, (b)** 

**2020, (c) 2023, (d) 2024.** 

## **5. Discussion** 

## **5.1 Method performance and inter-event variability** 

The most important structural result is not any single accuracy value but the consistency of method  rankings  across  all  four  events.  RF  methods  outperformed  all  threshold-based approaches and GFM in every event under both validation windows, despite considerable 

variation in flood magnitude, optical data availability, and SAR scene count between years. Rankings that hold under both the extended and restricted windows cannot be attributed to favourable acquisition timing; they reflect genuine differences in method capability. 

VV Change ranked above VH Change in all four events. The SAR flood mapping literature typically  treats  VH  as  the  default  single-polarisation  approach  for  vegetated  floodplains (Tsyganskaya et al., 2018), so this result warrants scrutiny. In this charland environment, where rice crops are at early growth stages during peak flood months, VV appears to respond more reliably to the mix of open water, shallow inundation, and bare soil that characterises active char surfaces. MultiPol fell between VH Change and Otsu, suggesting its 50% persistence threshold discards genuine detections that single-polarisation thresholding retains. The Ensemble did not outperform the best individual method in any event; majority voting dilutes the RF signal when weaker threshold methods dominate the vote. 

The consistent VV superiority over VH Change across all four events is worth examining in detail. The SAR flood mapping literature typically treats VH as the default for vegetated floodplains on the grounds that VH better penetrates crop canopies and detects double-bounce scattering over flooded rice fields (Tsyganskaya et al., 2018). The finding here suggests a more nuanced picture: during peak flood months in Roumari, rice crops are at early growth stages — seedlings or recently transplanted shoots — when canopy height and leaf area are still limited. Under these conditions, VV responds more reliably to the mix of open water, shallow inundation over bare sandy char, and very early-stage crop cover that characterises the flooded surface. As crops mature through the season, VH's canopy-penetrating advantage would be expected to increase — a seasonally dynamic relationship that static single-polarisation products like GFM cannot capture. The Ensemble's consistent failure to outperform the best individual method is also instructive. With five methods in the vote, the three threshold-based methods form a majority bloc that can outvote the two RF methods even when the RF methods are substantially more accurate. A precision-weighted ensemble assigning higher vote weight to RF outputs would likely perform better, but evaluating weighted ensemble designs was beyond the scope of this study. 

Stable method rankings coexisted with substantial inter-event variability in flood extents and SAR to GFM spatial agreement. SAR methods broadly matched GFM in 2019 and 2020, with ratios between 0.80 and 1.10, but overestimated it substantially in 2023 and 2024, where ratios ran from 1.28 to 1.84. Two factors likely contributed. GFM's global exclusion masks may be more aggressively applied in charland environments where sandy substrates generate backscatter signatures similar to shallow water. The reduction in Sentinel-1 temporal density following the December 2021 Sentinel-1B failure also gave GFM fewer acquisition opportunities to confirm flood extent before event windows closed in 2023 and 2024. VV Change showed the highest overestimation ratios in those events. This is consistent with VV's known sensitivity to high soil moisture on bare char surfaces, where the gain in flood recall comes at the cost of more false positives. 

## **5.2 GFM performance and the operational accuracy gap** 

The kappa gap between the best RF method and GFM held at 0.17–0.19 units under the restricted validation  across  all  four  events.  This  stability  —  across  events  of  different  magnitude, hydrological driver, and phenological context — is the core finding of this paper. Wagner et al. (2026),  in  their  comprehensive  evaluation  of  GFM's  first  operational  years,  explicitly acknowledged that the service's uniform exclusion masks and VV-only polarisation design experience significant limitations in morphodynamically complex landscapes, and identified site- 

specific evaluations as a necessary next step. The results presented here provide exactly that evidence for Bangladesh charland floodplains. 

The structural root cause of the accuracy gap is GFM's exclusive reliance on VV polarisation: in Bangladesh's rice-growing floodplains, inundation beneath a developing crop canopy attenuates VV backscatter, reducing the depression signal that VV change detection depends on. VH's greater sensitivity to double-bounce returns from vertical vegetation elements over water captures this condition more reliably (Tsyganskaya et al., 2018). Wagner et al. (2026) specifically called for VH channel integration and L-band adoption as future directions for GFM — VV Change and MultiPol in this study both outperformed GFM without requiring any training data, confirming that the VH addition alone is sufficient to close a substantial portion of the accuracy gap in this environment. 

GFM's  design  choices  are  appropriate  for  its  global  emergency  mandate  —  conservative exclusion masks and VV-only polarisation deliberately prioritise reliability at planetary scale (Wagner et al., 2026). This study does not dispute that mandate. Rather, it quantifies the operational  cost  of  those  choices  in  one  of  South  Asia's  most  flood-exposed  charland environments, providing the site-specific empirical evidence that Wagner et al. (2026) identified as necessary to understand where and why GFM's global design constraints become a source of detection error. 

## **5.3 Phenology-aware agricultural exposure** 

Agronomic vulnerability does not scale with flooded areas. The 2019 event was the most damaging in crop terms: it hit at the onset of T. Aman transplanting, when freshly transplanted seedlings are at their most sensitive to submergence, while Aus rice harvest was already underway. Two crops at peak vulnerability at the same time is something none of the other events produced. The 2020 event caused the largest raw cropland inundation but arrived before T. Aman transplanting, mostly disrupting Aus harvest and field preparation. The 2023 and 2024 events came during less critical crop windows. GFM's exposure estimates shifted between the two periods in opposite directions: overestimates in 2019 and 2020, then substantial underestimates in 2023 and 2024. That pattern fits with reduced Sentinel-1 temporal coverage following the Sentinel-1B failure. A global product that systematically underestimates cropland exposure in two of the four most recent flood years would provide misleading inputs to damage assessment and early warning systems — precisely the kind of site-specific consequence that Wagner et al. (2026) identified as requiring dedicated local evaluation. 

## **5.4 Limitations and future directions** 

Three limitations bear noting. Optical validation references are affected by monsoon cloud cover, which  introduces  temporal  latency  between  SAR  acquisition  and  the  nearest  cloud-free composite; the restricted window reduces this problem but does not remove it. The study also compares an operational global product against a locally optimised dual-polarisation workflow. That is a comparison of unequal inputs, and it reflects real-world operational constraints rather than a controlled experiment. Finally, the static ESA WorldCover 2021 cropland mask does not account for annual riverbank erosion in the morphodynamically active charland environment, adding marginal uncertainty to absolute exposure hectares. 

On the future work side, the most pressing question is whether Sentinel-1C's restored repeat cycle closes GFM's post-failure accuracy gap. Separately, integrating flood depth, duration, and crop-stage response functions would allow exposure estimates to feed into yield-loss projections 

rather than stopping at inundation area. Applying the same pipeline to the additional high-risk charland stations in Table 1 would then test whether local RF calibration holds up across different sites. 

## **6. Conclusions** 

This study benchmarked five SAR-based flood detection methods against the Copernicus GFM service across four monsoon flood events in Roumari Upazila, Kurigram, Bangladesh (2019, 2020, 2023, 2024). All events were processed through an identical pipeline and evaluated using a dual-window validation strategy. Flood extent results were then linked to a phenology-aware agricultural exposure analysis against the local crop calendar. 

Four principal findings emerged. Method rankings were consistent across all four events despite substantial variability between them, which indicates that performance differences reflect genuine methodological characteristics rather than event-specific conditions. VV Change matched or exceeded VH Change in every event and ranked second only to RF methods in 2023 (kappa = 0.660); MultiPol performed comparably to VH Change throughout. Both results confirm that using more than one polarisation improves flood detection in this rice-growing charland environment, with no requirement for labelled training data. GFM showed a stable accuracy deficit of 0.17 to 0.19 kappa units relative to the best local RF methods across all four events. This gap traces back to GFM's exclusive reliance on VV polarisation and its conservative global exclusion masks. Those design choices suit GFM's global emergency mandate; this study quantifies what they cost in one of South Asia's most chronically flood-exposed charland environments. The 2019 event was the most agronomically critical: it struck at the onset of T. Aman transplanting while Aus rice harvest was simultaneously underway. The 2020 event generated the largest absolute cropland inundation but at a less vulnerable phenological stage. The 2023 and 2024 events produced lower exposure at less critical crop windows. 

GFM's cropland exposure estimates reversed between the two sub-periods, diverging from all local SAR methods in 2023 to 2024 in a pattern consistent with halved Sentinel-1 temporal density following the December 2021 Sentinel-1B failure. Flooded cropland area alone, then, is an insufficient indicator of agronomic impact, and the choice of operational product materially affects exposure estimates in ways that compound as sensor availability changes. 

A locally adapted dual-polarisation workflow implemented entirely within Google Earth Engine, using publicly available data and no field-collected labels, consistently outperformed the current global operational baseline across all four events in this environment. 

## **Data Availability Statement** 

All Sentinel-1, Sentinel-2, Landsat-8, MODIS, JRC, SRTM, and ESA WorldCover data were accessed through the Google Earth Engine public data catalogue. The GFM product was obtained from the Copernicus Emergency Management Service. The Sen1Floods11 dataset is publicly available at https://github.com/cloudtostreet/Sen1Floods11. Flood maps and validation datasets for all events are archived at the following GitHub link: https://anonymous.4open.science/r/roumari_flood_multi-event_assessment-308C 

## **Acknowledgements** 

The authors gratefully acknowledge the European Space Agency (ESA) for Sentinel-1 and Sentinel-2 data, USGS for Landsat-8 data, NASA for MODIS data, the Joint Research Centre 

(JRC) for the Global Surface Water dataset and GFM product, and Google for Earth Engine computational resources. This research received no external funding. 

## **References** 

Uddin K, Matin MA, Meyer FJ (2019) Operational flood mapping using multi-temporal Sentinel-1 SAR images: a case study from Bangladesh. Remote Sensing 11(13): 1581. https://doi.org/10.1016/j.rse.2025.114882 

Portalés-Julià E, Mateo-García G, Gómez-Chova L (2025) Understanding flood detection models across Sentinel-1 and Sentinel-2 modalities and benchmark datasets. Remote Sensing of Environment 328: 114882. https://doi.org/10.1016/j.rse.2025.114882 

Aziz MA, Moniruzzaman M, Tripathi A, Hossain MI, Ahmed S, Rahaman KR, Ahmed R (2022) Delineating flood zones upon employing synthetic aperture data for the 2020 flood in - Bangladesh. Earth Systems and Environment 6(3): 733–743. https://doi.org/10.1007/s41748 022-00295-0 

Singha M, Dong J, Sarmah S, You N, Zhou Y, Zhang G, Xiao X (2020) Identifying floods and flood-affected paddy rice fields in Bangladesh based on Sentinel-1 imagery and Google Earth Engine. ISPRS Journal of Photogrammetry and Remote Sensing 166: 278–293. https://doi.org/10.1016/j.isprsjprs.2020.06.011 

Pandey AC, Kaushik K, Parida BR (2022) Google Earth Engine for large-scale flood mapping using SAR data and impact assessment on agriculture and population of Ganga-Brahmaputra basin. Sustainability 14(7): 4210. https://doi.org/10.3390/su14074210 

Islam MT, Meng Q (2022) An exploratory study of Sentinel-1 SAR for rapid urban flood mapping on Google Earth Engine. International Journal of Applied Earth Observation and Geoinformation 113: 103002. https://doi.org/10.1016/j.jag.2022.103002 

Toma A, andric I, Mihai BA (2024) Flooded area detection and mapping from Sentinel-1 Ș imagery: complementary approaches and comparative performance evaluation. European Journal of Remote Sensing 57(1): 2414004. https://doi.org/10.1080/22797254.2024.2414004 

Bonafilia D, Tellman B, Anderson T, Issenberg E (2020) Sen1Floods11: A georeferenced dataset to train and test deep learning flood algorithms for Sentinel-1. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops, 210–211. https://doi.org/10.1109/CVPRW50498.2020.00113 

Kar B, Schumann GJP, Mendoza MT, Bausch D, Wang J, Sharma P, Glasscoe MT (2024) Assessing a model-of-models approach for global flood forecasting and alerting. IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing 17: 9641–9650. https://doi.org/10.1109/JSTARS.2024.3382089 

Wagner W, Bauer-Marschallinger B, Roth F, Raiger-Stachl T, Reimer C, McCormick N, Salamon P (2026) The fully-automatic Sentinel-1 Global Flood Monitoring service: scientific challenges and future directions. Remote Sensing of Environment 333: 115108. https://doi.org/10.1016/j.rse.2025.115108 

Risling A, Lindersson S, Brandimarte L (2024) A comparison of global flood models using Sentinel-1 and a change detection approach. Natural Hazards 120(12): 11133–11152. https://doi.org/10.1007/s11069-024-06538-9 

Misra A, White K, Nsutezo SF, Straka W, Lavista J (2025) Mapping global floods with 10 years of satellite radar data. Nature Communications 16(1): 5762. https://doi.org/10.1038/s41467-02561063-6 

Chapman B, McDonald K, Shimada M, Rosenqvist A, Schroeder R, Hess L (2015) Mapping regional inundation with spaceborne L-Band SAR. Remote Sensing 7(5): 5440–5470. https://doi.org/10.3390/rs70505440 

Pekel JF, Cottam A, Gorelick N, Belward AS (2016) High-resolution mapping of global surface water and its long-term changes. Nature 540: 418–422. https://doi.org/10.1038/nature20584 

Miah MRA (2022) Study on agricultural cropping pattern change in a water stress area of Northwest region of Bangladesh: a case study in Sapahar Upazilla under Naogaon district. PhD Thesis, BRAC University, Dhaka, Bangladesh. 

Shariot-Ullah M (2016) Climate change and the Boro rice phenology in Rajshahi: scenarios of future crop water demand for Boro rice due to climate change in Rajshahi, north-western district of Bangladesh. MSc Thesis, Wageningen University, Wageningen, Netherlands. https://doi.org/10.13140/RG.2.2.29138.48326/2 

Khanam M, Biswas A (2022) Determination of growth stages of some rice varieties as affected by sowing time. Bangladesh Rice Journal 26(2): 41–49. https://doi.org/10.3329/brj.v26i2.66733 

Islam MR, Sikder S (2011) Phenology and degree days of rice cultivars under organic culture. Bangladesh Journal of Botany 40(2): 149–153. https://doi.org/10.3329/bjb.v40i2.9770 

Madhu U, Begum M, Salam A, Sarkar SK (2018) Influence of sowing date on the growth and yield performance of wheat (Triticum aestivum L.) varieties. Archives of Agriculture and Environmental Science 3(1): 89–94.https://doi.org/10.26832/24566632.2018.0301014 

Sikder S (2009) Accumulated heat unit and phenology of wheat cultivars as influenced by late sowing heat stress condition. Journal of Agriculture & Rural Development, 59–64. https://doi.org/10.3329/jard.v7i1.4422 

Tsyganskaya V, Martinis S, Marzahn P, Ludwig R (2018) Detection of temporary flooded vegetation using Sentinel-1 time series data. Remote Sensing 10(8): 1286. https://doi.org/10.3390/rs10081286 

