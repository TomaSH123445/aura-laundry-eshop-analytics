## AURA Laundry – Shopify e-shop analytics (GA4 + Looker Studio)

End-to-end analytický projekt pro začínající e-shop s parfémy do praní na Shopify.

Cílem bylo:
- nastavit spolehlivé měření v **GA4** včetně e-commerce eventů,
- postavit jednoduchý, ale businessově užitečný **dashboard v Looker Studiu**,
- umožnit majiteli sledovat cíl: **prodat 1000 ks** produktu.

---

## 🔍 Kontext

Klient: malý e-shop s jedním produktem (parfémovaný gel do praní) na platformě **Shopify**.  
Původní stav: bez GA4, bez reportingu, bez přehledu o tom, odkud přichází objednávky.

---

## 🧠 Moje role a stack

**Role:** data / web analytik – návrh měření, komunikace s vývojářem, dashboard.

**Tech stack:**

- **Google Analytics 4** – eventové a e-commerce měření
- **Shopify + Google & YouTube app** – nasazení Google tagu
- **Tag Assistant, DebugView** – debugging trackingu
- **Looker Studio** – reporting a vizualizace
- (future) **BigQuery + SQL** – pro další analýzy

---

## 🛠 Postup

### 1. Implementace měření

- založení GA4 property a web streamu,
- analýza možností měření v prostředí Shopify (GTM vs. oficiální appka),
- nasazení Google tagu přes **Google & YouTube app**,
- ověření e-commerce eventů:

  - `view_item`
  - `add_to_cart`
  - `begin_checkout`
  - `add_payment_info`
  - `purchase`

- kontrola parametrů (`transaction_id`, `value`, `currency`, `items[]`) v **DebugView**,
- ladění filtrů interního / developer trafficu, aby se testy nepletly s reálnými daty.

Více detailů: [`ga4/ga4_setup_notes.md`](ga4/ga4_setup_notes.md)

### 2. Dashboard v Looker Studiu

Hlavní stránka reportu obsahuje:

- **KPI řádek:**
  - počet objednávek,
  - počet prodaných kusů,
  - celkové tržby,
  - **AOV** (průměrná hodnota objednávky),
  - % splnění cíle **1000 ks**.

- **Návštěvnost v čase** (sessions, new users),
- **Tabulka kanálů** (Default channel group):
  - users, sessions, purchases, revenue, conversion rate,
- **Tabulka návštěv podle dne**.

Vlastní metriky:

- `AOV = Revenue / Purchases`
- `Goal_progress_1000 = Items_sold / 1000`

Popis dashboardu: [`looker/dashboard_description.md`](looker/dashboard_description.md)

---

## 📊 funnel analýza

V další iteraci plánuji:

- postavit mini funnel:
  `view_item → add_to_cart → begin_checkout → add_payment_info → purchase`,
- spočítat drop-off mezi kroky,
- rozpadnout funnel podle kanálů (Organic, Referral, Shopping, Social),
- navrhnout konkrétní UX / marketingové změny pro zvýšení konverze.

---

## 📈 Learnings

Na projektu jsem se naučil:

- prakticky propojit **GA4 + Shopify + Looker Studio** v reálném prostředí,
- debugovat měření (Tag Assistant, DebugView, filtry dat),
- přemýšlet od **business cíle (1000 ks)** k metrikám a vizualizacím,
- komunikovat zadání s vývojářem a kontrolovat implementaci na úrovni dat.

Detailní case study: [`docs/case-study.md`](docs/case-study.md)

---

## 🔮 Future work

Nápady na další rozšíření (viz [`future-work/roadmap.md`](future-work/roadmap.md)):

- export GA4 dat do **BigQuery** a pokročilejší analýzy v SQL,
- RFM segmentace zákazníků,
- market basket / cross-sell analýzy (po rozšíření sortimentu),
- A/B testy produktové stránky a checkoutu,
- propojení s Google Ads daty (ROAS dashboard).
