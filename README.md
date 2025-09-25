# Kids Clothing Recommendation Card 👕🧤🧥

Ett Home Assistant-kort som hjälper dig att välja rätt ytterkläder för barnen baserat på väderdata.  
Kortet visar både **ikoner** (egna PNG-bilder) och **text** för att göra det visuellt och enkelt för barn (och vuxna!) att förstå.  

---

## ✨ Funktioner
- Dynamisk rekommendation av kläder baserat på:
  - Temperatur 🌡️
  - Vädertyp (regn, snö, sol) ☀️🌧️❄️
  - Vindstyrka 💨
  - UV-index 🕶️
  - Årstid 🍂❄️🌸☀️
- Egen uppsättning barnvänliga ikoner för kläder (mössor, jackor, skor m.m.).
- Anpassningsbar för dina egna sensorer.
- Kompakt design (max ca 180px högt).
- Extra text som påminner: *"Men kika gärna prognosen för resten av dagen också"*.

---

## 📸 Exempel
Så här kan kortet se ut i din dashboard:

*(lägg in en screenshot här från din HA när kortet körs)*

---

## ⚙️ Installation via HACS
1. Gå till **HACS → Integrations → Custom Repositories**.  
2. Lägg till:  
   `https://github.com/tkabt06/kids-clothing-recommendation-card`  
   som **Dashboard**.  
3. Installera kortet via HACS.  
4. Lägg till i din Lovelace `ui-lovelace.yaml` eller via UI.  

---

## 📂 Ikoner
Alla ikoner ligger i `/www/pics/clothes/` och kan bytas ut mot egna PNG-filer.  
De nuvarande inkluderar t.ex.: vinterjacka, skaljacka, regnjacka, mössor, vantar, halsduk, sneakers, stövlar och kängor.

---

## 🔧 Konfiguration

### Steg 1 – inkludera template-filen
I din `configuration.yaml` (eller i en separat templates-fil om du har det) lägger du till:

```yaml
button_card_templates: !include clothes_card.yaml
```

### Steg 2 – använd kortet i Lovelace
När du sedan vill lägga till kortet i din dashboard skriver du:

```yaml
type: custom:button-card
template: clothes_card
variables:
  weather_entity: weather.home
  temp_entity: sensor.outdoor_temperature
  uv_entity: sensor.uv_index
  wind_entity: sensor.wind_speed
  season_entity: sensor.season
```

👉 Byt ut sensorerna (weather.home, sensor.outdoor_temperature, osv) mot de entiteter du själv har i ditt Home Assistant.

---

## 🌍 Årstids-sensor
Kortet använder även en sensor för att veta vilken årstid det är, eftersom det påverkar vilka kläder som rekommenderas.

### Alternativ 1: Home Assistant "Season"-integration
Home Assistant har en inbyggd integration för årstider. Lägg till den via:  
**Inställningar → Enheter & tjänster → Lägg till integration → Season**

Den skapar en sensor som heter `sensor.season` och returnerar värdena:
- `spring` (vår)
- `summer` (sommar)
- `autumn` (höst)
- `winter` (vinter)

### Alternativ 2: Egen template-sensor
Vill du ha svenska namn eller mer kontroll kan du skapa en egen sensor i `configuration.yaml`:

```yaml
template:
  - sensor:
      - name: "Årstid"
        state: >
          {% set month = now().month %}
          {% if month in [12,1,2] %} vinter
          {% elif month in [3,4,5] %} vår
          {% elif month in [6,7,8] %} sommar
          {% else %} höst
          {% endif %}
```
---
