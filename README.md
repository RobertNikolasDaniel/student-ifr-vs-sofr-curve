# Treasury Futures IFR vs SOFR Curve Workstation

## Purpose

This project compares Treasury futures implied financing rates (IFR) against the SOFR funding curve across multiple delivery horizons.

The objective is to determine whether Treasury futures financing is trading rich or cheap relative to the secured funding market.

Rather than analyzing a single futures contract, this workstation constructs a small term structure using:

* Current CTD financing
* Front delivery implied financing
* Next delivery implied financing

and compares those points against:

* Spot SOFR
* SOFR at front delivery
* SOFR at next delivery

This provides a visualization of how Treasury futures financing evolves relative to the market's secured funding expectations.

---

## Inputs

### CTD Bond Inputs

| Input                 | Description                            |
| --------------------- | -------------------------------------- |
| Par Value             | Bond principal amount                  |
| Clean Price           | Bond price excluding accrued interest  |
| Coupon Rate           | Annual coupon rate                     |
| Coupon Frequency      | Coupon payments per year               |
| Days Since Coupon     | Days accrued since last coupon payment |
| Days In Coupon Period | Total days in coupon cycle             |

---

### Front Futures Inputs

| Input                   | Description                                |
| ----------------------- | ------------------------------------------ |
| Front Futures Price     | Current front-month Treasury futures price |
| Front Conversion Factor | CME conversion factor for front contract   |
| Front Days To Delivery  | Days remaining until delivery              |

---

### Next Futures Inputs

| Input                  | Description                                 |
| ---------------------- | ------------------------------------------- |
| Next Futures Price     | Current deferred Treasury futures price     |
| Next Conversion Factor | CME conversion factor for deferred contract |
| Next Days To Delivery  | Days remaining until delivery               |

---

### Funding Inputs

| Input                 | Description                                          |
| --------------------- | ---------------------------------------------------- |
| Spot SOFR             | Current secured overnight financing rate             |
| SOFR @ Front Delivery | Forward SOFR corresponding to front delivery horizon |
| SOFR @ Next Delivery  | Forward SOFR corresponding to next delivery horizon  |

---

## Intermediate Calculations

### Coupon Payment

Coupon cash flow per payment period.

Formula:

Coupon Payment = (Coupon Rate × Par) / Coupon Frequency

---

### Current Accrued Interest

Current accrued coupon on the CTD.

Formula:

Current AI = (Days Since Coupon / Days In Coupon Period) × Coupon Payment

---

### Dirty Price

Formula:

Dirty Price = Clean Price + Current AI

---

### Projected Accrued Interest

Projected accrued interest at futures delivery.

Front Delivery:

Projected AI Front = Current AI + (Front DTD / Days In Coupon Period) × Coupon Payment

Next Delivery:

Projected AI Next = Current AI + (Next DTD / Days In Coupon Period) × Coupon Payment

---

### Invoice Price

Front Contract:

Invoice Front = (Front Futures Price × Front CF) + Projected AI Front

Next Contract:

Invoice Next = (Next Futures Price × Next CF) + Projected AI Next

---

## Implied Funding Rate (IFR)

The IFR represents the annualized financing rate implied by the relationship between the CTD bond and Treasury futures contract.

### Front Delivery IFR

IFR Front = ((Invoice Front − Dirty Price) / Dirty Price) × (360 / Front DTD)

### Next Delivery IFR

IFR Next = ((Invoice Next − Dirty Price) / Dirty Price) × (360 / Next DTD)

---

## Funding Curve Construction

### IFR Curve

T = 0      → Current CTD Repo

Front Date → IFR Front

Next Date  → IFR Next

---

### SOFR Curve

T = 0      → Spot SOFR

Front Date → SOFR @ Front Delivery

Next Date  → SOFR @ Next Delivery

---

## Relative Value Calculations

### Current Spread

Current Spread = Repo − Spot SOFR

### Front Spread

Front Spread = IFR Front − SOFR Front

### Next Spread

Next Spread = IFR Next − SOFR Next

---

## Outputs

### Financing Metrics

* Current Repo
* Front IFR
* Next IFR

### Funding Benchmarks

* Spot SOFR
* Front SOFR
* Next SOFR

### Relative Value Metrics

* Current Spread
* Front Spread
* Next Spread

---

## Graphing Methodology

### Chart 1: IFR vs SOFR Curve

X-Axis

* T = 0
* Front Delivery
* Next Delivery

Series 1

* Repo
* Front IFR
* Next IFR

Series 2

* Spot SOFR
* Front SOFR
* Next SOFR

Purpose:

Visualize how Treasury futures financing evolves relative to secured funding expectations.

---

### Chart 2: IFR-SOFR Spread Curve

X-Axis

* T = 0
* Front Delivery
* Next Delivery

Series

* Current Spread
* Front Spread
* Next Spread

Purpose:

Measure Treasury futures financing richness or cheapness relative to the SOFR curve.

Positive values indicate richer financing relative to SOFR.

Negative values indicate cheaper financing relative to SOFR.

---

## Key Insight

Treasury futures do not simply reflect interest-rate expectations.

They also embed financing expectations through the delivery process.

By comparing the Treasury futures implied funding curve against the SOFR curve, traders can identify whether Treasury financing is becoming richer or cheaper through time and evaluate relative-value opportunities across futures delivery horizons.
