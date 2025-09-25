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

<img width="499" height="256" alt="image" src="https://github.com/user-attachments/assets/128ff37c-732f-4492-8a2b-935189826e8c" />


---

## ☕ Stöd projektet
Om du gillar det här projektet och vill stötta utvecklingen kan du göra det här:

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?logo=buy-me-a-coffee&logoColor=black&style=for-the-badge)](https://buymeacoffee.com/karlssontka)  
[![PayPal](https://img.shields.io/badge/PayPal-00457C?logo=paypal&logoColor=white&style=for-the-badge)](https://paypal.me/tobiaskarlsson307)

---

## ⚙️ Installation (manuell)

### Steg 1 – Ladda ner filer
1. Ladda ner `clothes_card.yaml` och lägg den i din `/config/`-mapp.  
2. Ladda ner hela mappen `www/pics/clothes/` och lägg den i `/config/www/pics/clothes/` i ditt Home Assistant.

### Steg 2 – Lägg till i configuration.yaml
Öppna din `configuration.yaml` och lägg till följande rad (om du inte redan använder `button_card_templates`):
```yaml
button_card_templates: !include clothes_card.yaml
```

Starta sedan om Home Assistant.

### Steg 3 – Lägg till kortet i Lovelace
När Home Assistant startat igen kan du lägga till kortet i din dashboard med:

```yaml
type: custom:button-card
template: clothes_card
```

---

## 🔧 Anpassa sensorer
I början av `clothes_card.yaml` finns en sektion markerad med kommentarer:

```yaml
variables:
  # 👇 HIT SKA DU TITTA 👇
  # Här anger du dina egna sensorer.
  # Byt ut exemplen nedan mot det du har i ditt system:
  # - weather_entity: ditt weather-entity (t.ex. weather.smhi_home)
  # - temp_entity: din temperatursensor (t.ex. sensor.utomhus_temp)
  # - uv_entity: din UV-sensor (t.ex. sensor.uv_index)
  # - wind_entity: din vind-sensor (t.ex. sensor.wind_speed)
  # - season_entity: din årstid-sensor (om du har en)
  weather_entity: weather.home
  temp_entity: sensor.outdoor_temperature
  uv_entity: sensor.uv_index
  wind_entity: sensor.wind_speed
  season_entity: sensor.season
```

Byt ut dessa till dina egna entiteter.

---

## 📂 Ikoner
Alla ikoner ligger i `/www/pics/clothes/` och kan bytas ut mot egna PNG-filer.  
De nuvarande inkluderar t.ex.: vinterjacka, skaljacka, regnjacka, mössor, vantar, halsduk, sneakers, stövlar och kängor.

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

## 🚀 Kom igång snabbt
Så här installerar du kortet på enklast möjliga sätt:

1. Kopiera `clothes_card.yaml` till `/config/`.  
2. Kopiera hela mappen `www/pics/clothes/` till `/config/www/pics/clothes/`.  
3. Lägg till i din `configuration.yaml`:  
   ```yaml
   button_card_templates: !include clothes_card.yaml
4. Starta om Home Assistant så att ändringen laddas in.
5. Lägg till kortet i din dashboard via: 
  ```yaml
  type: custom:button-card
  template: clothes_card
  ```
---

## ✅ Klart!
Nu ska du ha ett kort i din dashboard som dynamiskt rekommenderar kläder för barnen – både i text och bild.
