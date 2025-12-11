# AURA Laundry Perfumes – GA4 & Looker Studio Analytics MVP

Reálný mini-projekt datové analytiky pro začínající e-shop s parfémovaným gelem na praní (Shopify).  
Cíl: **bezplatně** a **co nejjednodušeji** nasadit měření (GA4) a postavit první dashboard v Looker Studiu – včetně jednoduché *funnel analýzy*.

---

## 1. Kontext projektu

- Klient: malý e-shop s jedním produktem (AURA – parfémovaný gel na praní).
- Platforma: **Shopify**, jazyk webu CZ.
- Startovní situace:
  - žádná GA4 property
  - žádný přehled o chování uživatelů
  - cílem je 1000 prodaných kusů v prvním období
- Můj cíl (junior / trainee data analyst):
  - získat **reálnou zkušenost** s implementací GA4 na e-shopu,
  - naučit se pracovat s eventy z GA4 v **Looker Studiu**,
  - připravit jednoduchý, ale použitelný **MVP reporting pro majitele e-shopu**.

---

## 2. Tech stack

- **Google Analytics 4 (GA4)** – webová property + datový stream pro e-shop.
- **Shopify** – implementace měření přes oficiální Google / GA4 integraci.
- **Google Tag Assistant & DebugView** – ladění a validace eventů.
- **Looker Studio** – vizualizace dat, výpočet metrik, funnel grafy.

> Pozn.: GTM byl připravený, ale finální e-commerce eventy (`purchase` atd.) jdou přes oficiální Shopify → GA4 integraci. Pro MVP analytics to je nejjednodušší a nejstabilnější varianta.

---

## 3. GA4 eventy a datový model

Základ tvoří standardní GA4 webová implementace:

- automaticky měřené eventy: `page_view`, `session_start`, `scroll`, `click`, …
- e-commerce eventy z Shopify:
  - `add_to_cart`
  - `begin_checkout`
  - `add_payment_info`
  - `purchase`

V GA4 je `purchase` nastaven jako **klíčová událost (Key event)**.

### Kontrola implementace

1. **Tag Assistant** – ověřeno, že se na všech důležitých stránkách načítá GA4 tag.
2. **DebugView v GA4** – kontrola, že:
   - `purchase` se spouští na děkovací stránce,
   - přichází měna, hodnota objednávky a položky,
   - eventy mají správnou posloupnost (`add_to_cart` → `begin_checkout` → `add_payment_info` → `purchase`).

---

## 4. Dashboard v Looker Studiu

Dashboard je rozdělený do dvou hlavních stránek:

### 4.1 Stránka 1 – Přehled MVP

**KPI Scorecards:**

- **Počet objednávek**  
  – metrika: Key event `purchase`.
- **Prodáno kusů**  
  – počet zakoupených položek (GA4 e-commerce metrika).
- **Celkové tržby**  
  – obrat z `purchase` eventů.
- **Konverzní poměr (MVP)**  
  – vlastní metrika v Looker Studiu, např.:

  Konverzní poměr = keyEvents:purchase / sessions

### 4.2.Globální funnel (horní graf)

**Typ grafu:** svislý pruhový graf  
**Cíl:** ukázat, kolik návštěvníků projde jednotlivými kroky nákupního procesu.

**Použité pole `Funnel step` (vypočtená dimenze):**

CASE
  WHEN eventName = "add_to_cart" THEN "1 - Add to cart"
  WHEN eventName = "begin_checkout" THEN "2 - Begin checkout"
  WHEN eventName = "add_payment_info" THEN "3 - Add payment info"
  WHEN eventName = "purchase" THEN "4 - Purchase"
END

- export GA4 dat do **BigQuery** a pokročilejší analýzy v SQL,
- RFM segmentace zákazníků,
- market basket / cross-sell analýzy (po rozšíření sortimentu),
- A/B testy produktové stránky a checkoutu,
- propojení s Google Ads daty (ROAS dashboard).
