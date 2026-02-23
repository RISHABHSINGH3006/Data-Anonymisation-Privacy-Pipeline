# Data Anonymisation – Privacy Pipeline

## Overview

In this I have focused on anonymising sensitive customer data from `mobile_customers.csv` before sharing it. The objective is to protect personally identifiable information (PII) while preserving analytical utility.

The anonymisation pipeline was implemented using **Microsoft Excel**.

---

## Why Data Anonymisation Matters

Data anonymisation is essential to:

* Protect customer privacy
* Comply with data protection regulations
* Prevent **linkage attacks** (where attackers re-identify individuals by combining datasets)
* Enable safe data sharing for analytics

The approach balances **privacy protection** and **data usefulness**.

---

## Anonymisation Techniques Used

### 1. Column Removal (Redaction)

Columns that provided little analytical value and posed privacy risk were removed:

* `customer_id`
* `current_location`

**Reason:** These fields can directly identify individuals and are not required for downstream analysis.

---

### 2. Username Masking

Usernames were partially hidden.

**Excel formula used:**

```excel
=LEFT(A2,2)&"****"
```

**Effect:**

* Original: `rishi123`
* Masked: `ri****`

**Purpose:** Prevents direct identification while keeping partial structure.

---

### 3. Name Replacement (Pseudonymisation)

Real names were replaced with fake names.

**Method in Excel:**

* Created a helper column with fake names using:

```excel
="User_"&ROW()
```

(or random names from a prepared list)

**Purpose:** Removes real identity while keeping row uniqueness.

---

### 4. Email Masking

Email addresses were partially hidden.

**Excel formula:**

```excel
=LEFT(A2,2)&"****@"&RIGHT(A2,LEN(A2)-FIND("@",A2))
```

**Effect:**

* Original: `rishi@gmail.com`
* Masked: `ri****@gmail.com`

**Purpose:** Preserves domain information for analysis while hiding identity.

---

### 5. Date Noise Injection

Random noise was added to `date_registered` and `birthdate`.

**Excel formula:**

```excel
=A2 + RANDBETWEEN(-30,30)
```

**Purpose:**

* Prevents exact date matching
* Maintains approximate time distribution
* Reduces linkage attack risk

---

### 6. Age and Salary Binning (Generalisation)

#### Age Binning

**Formula example:**

```excel
=IF(A2<25,"18-24",
 IF(A2<35,"25-34",
 IF(A2<45,"35-44",
 IF(A2<55,"45-54","55+"))))
```

#### Salary Binning

**Formula example:**

```excel
=IF(A2<30000,"Low",
 IF(A2<70000,"Medium","High"))
```

**Purpose:**

* Removes precise values
* Preserves statistical distribution
* Improves k-anonymity

---

### 7. Tokenisation of Categorical Fields

The following columns were tokenised:

* `credit_card_provider`
* `credit_card_expire`
* `employer`
* `job`

**Method:**

* Created mapping table
* Used:

```excel
=VLOOKUP(A2,MappingTable,2,FALSE)
```

**Purpose:** Keeps category relationships while hiding real values.

---

### 8. Credit Card Masking

#### Credit Card Number

```excel
="XXXX-XXXX-XXXX-"&RIGHT(A2,4)
```

#### Security Code

```excel
="***"
```

**Purpose:** Removes highly sensitive financial data.

---

### 9. Address and Residence Replacement

Real location data was replaced with synthetic values.

**Example formula:**

```excel
="Address_"&ROW()
```

(or replaced via fake data list)

**Purpose:** Prevents re-identification via geographic linkage.

---

## Privacy Risk Mitigation

This pipeline specifically reduces risk of:

* Direct identification
* Quasi-identifier linkage
* Attribute disclosure
* Financial data exposure

Techniques used align with:

* Masking
* Generalisation
* Tokenisation
* Noise addition
* Suppression

---

## Conclusion

The implemented privacy pipeline successfully anonymises customer data while maintaining its analytical value. The dataset is now suitable for secure sharing and downstream data science workflows.

---
