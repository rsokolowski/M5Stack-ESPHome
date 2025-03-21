### ENV IV Unit with Temperature Humidity Air Pressure Sensor (SHT40+BMP280)

https://shop.m5stack.com/products/env-iv-unit-with-temperature-humidity-air-pressure-sensor-sht40-bmp280

<b>NOTE:</b> There seems to be approx an 0.5° C difference between the SHT40 and BMP280 temperature sensors. Adjust offsets accordingly.

<pre>
i2c:
  sda: 26
  scl: 32
  scan: true
  id: bus_1

sensor:
  - platform: sht4x
    temperature:
      name: "Temperature - SHT40"
      id: "temperature_sht40"
    humidity:
      name: "Humidity - SHT40"
      id: "humidity_sht40"
    address: 0x44
    update_interval: 10s
  - platform: bmp280_i2c
    temperature:
      name: "Temperature - BMP280"
      id: "temperature_bmp280"
      oversampling: 16x
    pressure:
      name: "Pressure BMP280"
      id: "pressure_bmp280"
    address: 0x76
    update_interval: 10s
  - platform: template
    name: "VPD"
    icon: "mdi:gauge"
    id: gr2_ace_vpd
    lambda: |-
          return (((100 - id(humidity_sht40).state) / 100.0) * (0.6108 * exp((17.27 * id(temperature_sht40).state) / (id(temperature_sht40).state + 237.3))));
    update_interval: 10s
    unit_of_measurement: kPa
    accuracy_decimals: 2
    filters:
      - filter_out: nan
</pre>
