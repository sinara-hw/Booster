# Booster / Booster-HL Specification and Characterization

> Specification and characterization note based on the Booster wiki and selected issue history.
> Intended as a living technical reference for the project.

## 1. Scope

This document combines:
- product-level information from the public Booster wiki,
- measurement and behavior data from selected public issues.

It distinguishes between two variants:
- **Booster** — optimized for high efficiency,
- **Booster-HL** — optimized for high linearity.

Each measurement is linked directly to the source issue or issue comment where it was reported.

## 2. Product Overview

Booster is an **eight-channel RF power amplifier** providing **several watts of RF between 40 MHz and 500 MHz** in a **2U high 19-inch chassis**.

According to the wiki, Booster is optimized for:
- low cost,
- low power consumption,
- good RF performance,
- full interlocking and logging via an Ethernet-based remote interface.

Source:
- Wiki home: https://github.com/sinara-hw/Booster/wiki

## 3. Variants

The wiki states that Booster is offered in two variants:

### 3.1 Booster
- optimized for **high efficiency**

### 3.2 Booster-HL
- optimized for **high linearity**
- wiki statement: it offers the **second harmonic at -40 dBc level**

Source:
- Wiki home: https://github.com/sinara-hw/Booster/wiki

## 4. Platform Features

From the wiki:

- Ethernet interface for monitoring and configuration
- User-configurable interlocks protecting both Booster and connected loads
- Per-channel remote monitoring of:
 - input power
 - output forward power
 - output reverse power
 - temperature
 - current
 - voltage
- Remote shutdown of individual channels via Ethernet
- Open source hardware and firmware
- Modular design with individually field-replaceable channels, e.g. for changing frequency band

Source:
- Wiki home: https://github.com/sinara-hw/Booster/wiki

## 5. Interlocks and Protection

The wiki describes the following interlocks:

### 5.1 Output forward power
- implemented in hardware
- response time: **a few microseconds**
- user-adjustable threshold up to **39 dBm**
- primarily intended to protect sensitive loads such as AOMs from excessive RF power

### 5.2 Output reverse (reflected) power
- fixed threshold: **30 dBm**
- primarily intended to protect Booster itself from damage

### 5.3 Current consumption
- monitors internal **30 V** and **6 V** supplies
- includes hardware fold-back protection on the **30 V** PA supply

### 5.4 Temperature
- software protection disables a channel when temperature reaches **60 C**
- thermal switch cuts power to the entire chassis at **80 C**

### 5.5 Interlock behavior
- when any interlock trips, input RF is disconnected using an internal RF switch
- in an error condition, the amplifier power stage is also shut down
- interlocks can be cleared by:
 - front-panel **INTERLOCK CLEAR** button,
 - remote programming interface,
 - power cycling
- error conditions require power cycling

Source:
- Wiki home: https://github.com/sinara-hw/Booster/wiki

## 6. Diagnostics and User Interface

From the wiki:

- input, output forward, and output reverse powers can be queried remotely
- channel temperature and fan speed can be queried remotely
- reported power values clip to the lowest accurately measurable value at very low power
- the onboard power readings are useful operational diagnostics, but should **not** be treated as test-and-measurement-grade RF instrumentation
- high VSWR can lead to **dB-level errors** in forward power readings

### Front panel switches
- `INTERLOCK CLEAR`
- `STANDBY`

### Installation notes from the wiki
- by default, all channels are disabled at power-up
- MAC address can be obtained via the `macconfig` command in the VCP interface
- minimum fan level can be configured via `fanlevel`
- wiki guidance suggests checking typical idle current draws:
 - **29 V** should be approximately **50 mA** with no RF
 - **6 V** should be approximately **250 mA**

Source:
- Wiki home: https://github.com/sinara-hw/Booster/wiki

## 7. RF Input

The RF input is protected using **two TVS diodes SZNUP1301ML3T1G**.

Key notes for the input stage:
- each TVS diode will continuously conduct approximately **215 mA**
- after exceeding an input power of approximately **11 mW**, the input behavior becomes **reflective rather than absorptive**
- in that regime the input can tolerate **much more than 25 dBm**
- under normal operation, the input is typically driven at **0 dBm**

This is a design and usage note. Final guaranteed limits should be cross-checked against the schematic and dedicated input-protection calculations.

## 8. Characterization Data from Public Issues

This section summarizes selected issues that are useful as source material for characterization.

---

## 8.1 Booster-HL characterization

### 8.1.1 HL spectrum at 150 MHz, 30 dBm
Issue source:
- [Issue #385 — Booster HL testing](https://github.com/sinara-hw/Booster/issues/385)

Image:
![Booster-HL spectrum at 30 dBm](https://user-images.githubusercontent.com/4325054/122688259-9626b080-d21b-11eb-9adc-ca13a9a81f50.png)

Use:
- representative spectrum plot for Booster-HL

Status:
- typical characterization plot

### 8.1.2 HL spectrum at 150 MHz, 33 dBm
Issue source:
- [Issue #385 — Booster HL testing](https://github.com/sinara-hw/Booster/issues/385)

Image:
![Booster-HL spectrum at 33 dBm](https://user-images.githubusercontent.com/4325054/122688277-af2f6180-d21b-11eb-8e75-ebcc311e09bf.png)

Use:
- representative spectrum plot for Booster-HL

Status:
- typical characterization plot

### 8.1.3 HL spectrum at 150 MHz, 34 dBm
Issue source:
- [Issue #385 — Booster HL testing](https://github.com/sinara-hw/Booster/issues/385)

Image:
![Booster-HL spectrum at 34 dBm](https://user-images.githubusercontent.com/4325054/122688287-b5254280-d21b-11eb-833f-8d1a378a8bf6.png)

Use:
- representative spectrum plot for Booster-HL

Status:
- typical characterization plot

### 8.1.4 HL S21, 40–800 MHz
Issue source:
- [Issue #385 — Booster HL testing](https://github.com/sinara-hw/Booster/issues/385)

Image:
![Booster-HL S21](https://user-images.githubusercontent.com/4325054/122688289-ba828d00-d21b-11eb-9add-194e99c44d13.png)

Use:
- representative small-signal gain plot for Booster-HL

Status:
- typical characterization plot

### 8.1.5 HL S11, 40–800 MHz
Issue source:
- [Issue #385 — Booster HL testing](https://github.com/sinara-hw/Booster/issues/385)

Image:
![Booster-HL S11](https://user-images.githubusercontent.com/4325054/122688299-c4a48b80-d21b-11eb-80c7-579ad8ec0c3a.png)

Use:
- representative input return-loss plot for Booster-HL

Status:
- typical characterization plot

---

## 8.2 Booster standard variant characterization and caveats

### 8.2.1 Prototype small-signal S21
Issue source:
- [Issue #35 — Booster testing take II](https://github.com/sinara-hw/Booster/issues/35)

Image:
![Booster prototype S21](https://user-images.githubusercontent.com/4325054/34476758-7807ed66-ef8e-11e7-9202-d5f1556f6ee0.png)

Notes pulled from the issue:
- small-signal S21 measurement with `Pin = -20 dBm`
- gain flatness looked good on most channels
- one channel showed poorer matching
- gain was reported as too high for that prototype
- the issue explicitly states this prototype result should not be considered acceptable as a production result without retuning
- issue [#335](https://github.com/sinara-hw/Booster/issues/335) later adds useful context from Thomas Harty that a practical target of around **30 dB** gain, or perhaps **35 dB absolute maximum**, would likely be sufficient in real use, especially with Urukul-driven systems

Use:
- prototype characterization only

Status:
- not a final production specification

### 8.2.2 Crosstalk measurement
Issue source:
- [Issue #35 — Booster testing take II](https://github.com/sinara-hw/Booster/issues/35)
- [Issue comment — Crosstalk measurement](https://github.com/sinara-hw/Booster/issues/35#issuecomment-408823704)

Image:
![Booster crosstalk measurement](https://user-images.githubusercontent.com/21218399/43393241-446f51fc-93ee-11e8-8a2d-8452fff3b465.png)

Notes from the linked source comment and associated public search snippets:
- no strong channel-to-channel dependence of crosstalk was reported
- below about **200 MHz**, the author notes difficulty measuring it because crosstalk is reported as **< 100 dB on most channels**
- the linked plot is the primary source for crosstalk / channel-isolation characterization in this document

Use:
- typical characterization reference for channel-to-channel isolation / crosstalk
- strong candidate for a dedicated `Crosstalk / Channel Isolation` subsection in the final repo document

Status:
- characterization data from issue history/comment

### 8.2.3 Phase-noise notes
Issue source:
- [Issue #95 — measure phase noise](https://github.com/sinara-hw/Booster/issues/95)
- [Issue #381 — Harmonic Distortion](https://github.com/sinara-hw/Booster/issues/381)

Notes from the public issue history:
- Issue **#95** exists specifically as the phase-noise tracking / documentation issue
- in **#381**, Thomas Harty explicitly states that **the phase noise from Booster is incredibly low**
- the same issue also notes that Booster has been used for **high-fidelity quantum gates**, which is a strong practical endorsement of RF quality

Interpretation:
- these are strong public qualitative statements about practical RF quality
- however, the currently extracted public content still does **not** include a directly embedded phase-noise plot in this document

Use:
- qualitative support for a `Low phase noise / demonstrated in precision AMO experiments` statement
- placeholder section awaiting direct phase-noise plots from Harty's measurements if they are recovered from issue comments or other public attachments

Status:
- qualitative public evidence with the correct dedicated source issue linked, but still missing a hard plot-based phase-noise figure in this document

### 8.2.4 Stability / bistability diagnostics
Issue source:
- [Issue #258 — booster bistability](https://github.com/sinara-hw/Booster/issues/258)
- [Issue #181 — v1.4 rc1](https://github.com/sinara-hw/Booster/issues/181)

Image:
![Booster bistability diagnostics](https://user-images.githubusercontent.com/21218399/62223171-67e02380-b3ac-11e9-8731-efa2d8b254a4.png)

Notes from the issue history:
- in **#258**, Thomas Harty reports observing a small bistability on Booster's power detectors
- he reports operation at constant **power, frequency, and phase** from Urukul while logging Booster diagnostics through the VCP interface
- the observed change is approximately **0.3 dB**, with a simultaneous change in noise
- the only correlation he reports spotting in diagnostics is that the **5 V MP voltage** changes slightly at the same time
- in **#181**, Harty also states that after testing, Booster **v1.3** seemed to work really excellently apart from outstanding software issues

Interpretation:
- #258 is best treated as a characterization/debug note for diagnostic stability or detector bistability, not as evidence of output-spectrum instability by itself
- it remains a useful source because it documents a concrete stability-related observation together with a diagnostic screenshot

Use:
- characterization note for diagnostic stability / bistability behavior
- candidate material for a `Diagnostics stability` or `Known behaviors` subsection rather than a headline datasheet claim

Status:
- useful stability-related figure and observation; not a standalone general RF-output stability limit without additional external-power-meter correlation

### 8.2.5 Harmonic content caveat
Issue source:
- [Issue #380 — High harmonic content](https://github.com/sinara-hw/Booster/issues/380)

Images:
![Booster harmonic content](https://user-images.githubusercontent.com/4325054/106255857-d8bf0080-621a-11eb-8702-75608fa523a1.png)

![Comparison amplifier harmonic content](https://user-images.githubusercontent.com/4325054/106255979-00ae6400-621b-11eb-8024-094555d8cd58.png)

Notes pulled from the issue:
- reported for a **150 MHz** signal
- issue states that Booster showed high harmonic content in that test
- the issue explicitly states: **"The Booster was optimized for efficiency, not for linearity."**

Use:
- important caveat for the standard variant
- useful for explaining the Standard vs HL split

Status:
- reported caveat

### 8.2.6 Product-positioning discussion
Issue source:
- [Issue #381 — Harmonic Distortion](https://github.com/sinara-hw/Booster/issues/381)

Use:
- public discussion that helps justify wording such as:
 - standard Booster prioritizes efficiency and lower dissipation,
 - Booster-HL exists for better linearity

Status:
- positioning source rather than a direct measurement plot

### 8.2.7 AM keying distortion
Issue source:
- [Issue #368 — AM keying distortion](https://github.com/sinara-hw/Booster/issues/368)

Image:
![AM keying distortion](https://user-images.githubusercontent.com/12980362/91681708-4f4b3b80-eb49-11ea-85cb-6cf0edc3dff1.png)

Notes pulled from the issue:
- approximately **0.6 dB AM distortion**
- reported at **33 dBm**
- observed during full AM keying
- reported on two time scales
- reported as less severe at lower powers

Use:
- dynamic behavior caveat
- candidate for application note or characterization section

Status:
- reported caveat

### 8.2.8 Step response ringing in AOM drive chain
Issue source:
- [Issue #384 — Step Response Ringing](https://github.com/sinara-hw/Booster/issues/384)

Images:

AOM optical pulse:
![AOM optical pulse](https://user-images.githubusercontent.com/12980362/122259815-cd016d00-ce8f-11eb-908e-303fc83db12e.png)

RF pulse out of Urukul:
![RF pulse out of Urukul](https://user-images.githubusercontent.com/12980362/122257006-d2a98380-ce8c-11eb-9d99-0d905dd55fe2.png)

RF pulse out of Booster:
![RF pulse out of Booster](https://user-images.githubusercontent.com/12980362/122257441-3e8bec00-ce8d-11eb-878b-a0c51ae605b8.png)

Spectrum with visible second harmonic:
![Booster spectrum with second harmonic](https://user-images.githubusercontent.com/12980362/122258221-089b3780-ce8e-11eb-99b7-678649bc7645.png)

Notes pulled from the issue:
- reported in an `Urukul -> Booster -> AOM` chain at **80 MHz**
- ringing observed in the optical pulse
- ringing and asymmetry observed at Booster RF output
- issue text mentions a second harmonic around **-9 dBc** in that setup
- swapping to a Mini-Circuits amplifier reportedly removed the observed ringing in that user setup

Use:
- application-level transient caveat
- useful as a characterization note rather than a headline spec

Status:
- reported caveat

## 9. What this document is safe to claim now

Based on the current public wiki and the selected public issues, this document can state:

- Booster is an 8-channel RF power amplifier platform covering **40–500 MHz**
- Booster is available in:
 - a high-efficiency version,
 - a high-linearity version
- Booster includes Ethernet monitoring/configuration, interlocks, and per-channel telemetry
- the wiki states that Booster-HL offers second harmonic at about **-40 dBc**
- public issue data for Booster-HL cover:
 - spectra at **150 MHz**
 - output levels of **30 dBm**, **33 dBm**, and **34 dBm**
 - **S21** and **S11** from **40 MHz to 800 MHz**
- public issue data also document important standard-version caveats and characterization related to:
 - gain versus frequency for prototype hardware
 - crosstalk / channel-to-channel isolation
 - harmonic content
 - AM keying distortion
 - ringing in some AOM use cases
- public issue historys also provide qualitative support for **very low phase noise** and strong practical stability, but this document still lacks the underlying hard plots that would be needed for a formal datasheet claim
- the RF input protection note in this document states the use of two **SZNUP1301ML3T1G** TVS diodes and a typical operating level of **0 dBm**; final guaranteed limits should still be cross-checked against the primary design sources

## 10. What should still be validated before turning this into a formal datasheet

Before promoting this into a formal datasheet, the following items should be validated:

- guaranteed output power by frequency band
- flatness / gain tolerances across channels
- harmonic content limits for production hardware
- startup / pulsed / AM transient behavior
- measurement conditions behind the wiki claim for Booster-HL second harmonic
- direct phase-noise plots and long-term stability plots from Harty testing
- direct crosstalk figures from the linked issue comment, if they are to be embedded locally rather than linked
- environmental limits, cooling assumptions, and warm-up conditions
- connectorization, power input, mechanical dimensions, and exact revision scope

## 11. Source Index

### Wiki
- Home: https://github.com/sinara-hw/Booster/wiki

### Additional source / reference links
- [Issue #35 comment — Crosstalk measurement](https://github.com/sinara-hw/Booster/issues/35#issuecomment-408823704)

### Issues used for measurement and characterization
- [#95 — measure phase noise](https://github.com/sinara-hw/Booster/issues/95)
- [#35 — Booster testing take II](https://github.com/sinara-hw/Booster/issues/35)
- [#368 — AM keying distortion](https://github.com/sinara-hw/Booster/issues/368)
- [#380 — High harmonic content](https://github.com/sinara-hw/Booster/issues/380)
- [#381 — Harmonic Distortion](https://github.com/sinara-hw/Booster/issues/381)
- [#384 — Step Response Ringing](https://github.com/sinara-hw/Booster/issues/384)
- [#385 — Booster HL testing](https://github.com/sinara-hw/Booster/issues/385)
- [#335 — scrap PHA-1](https://github.com/sinara-hw/Booster/issues/335)
- [#181 — v1.4 rc1](https://github.com/sinara-hw/Booster/issues/181)
