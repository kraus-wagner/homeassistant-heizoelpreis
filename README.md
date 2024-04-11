# homeassistant-heizoelpreis

Add this to your configuration.yaml:

rest:
  - resource: https://www.heizoel24.de/DailyPriceXml.ashx?zipCode=XXXXX&litre=3000&unloadingpoints=1
    headers:
      user_agent: "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0"
    scan_interval: 300
    sensor:
      - name: "heizoelpreis"
        value_template: "{{ value_json['result']['deliveries']['delivery']['price'][0]['#text'] | regex_replace(find=',' , replace='.') | float }}"
        unit_of_measurement: "EUR"
        state_class: "measurement"
        device_class: "monetary"
        unique_id: "heizoelpreis"

Replace XXXXX with your german zip code / Postleitzahl
Replace 3000 with your amount of heating oil. Minimum is 500 i believe.
Scan interval is 300 seconds or 5 minutes. You can change it eg to 3600 for once an hour.
You can add a chart on your dashboard (i use apex-chart-card for long term), sensor is sensor.heizoelpreis
