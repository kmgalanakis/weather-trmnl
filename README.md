# Weather for Terminus

This repo holds a [Terminus](https://github.com/usetrmnl/terminus) extension that shows the weather on an e-ink screen.
It reads its data from [Open-Meteo](https://open-meteo.com/), which is free and needs no API key.
I built it for a jailbroken Kindle Paperwhite, but it works on any model Terminus can render for.

The two files use Terminus's own export format, so Terminus can import them directly.

| File | Contents |
| --- | --- |
| `configuration.yml` | The extension settings, its form fields, the default data and the exchange. |
| `template.html.liquid` | The Liquid template. |

## What the screen shows

- The current temperature, a weather icon, the condition and the "feels like" temperature.
- Cards for today and tomorrow with the high, low, rain chance and maximum UV.
- Eight detail stats: humidity, wind, rain today, pressure with its trend, UV now, air quality, sunrise or sunset, and the moon phase.
- A 5-day outlook.
- An hourly chart of temperature, with bars for the chance of rain.

When the air quality request fails, the air quality stat shows cloud cover and visibility instead.

The layout adapts to the screen shape. In portrait the template stacks the sections and shows fewer hours in the chart when they do not fit.

## How it works

Terminus fetches the data through one exchange with two addresses:

1. The Open-Meteo forecast API gives current conditions, hourly values for the chart and 7 days of daily values.
2. The Open-Meteo air quality API gives the European AQI, PM10, PM2.5 and dust.

Both requests use `timezone=auto`, so all times are local to the location you set.
A script at the bottom of the template draws the icons, the stats and the chart in the browser that renders the screen.

## Set it up

1. Zip the two files. The zip must hold them at its top level, not inside a folder:

   ```bash
   zip extension.zip configuration.yml template.html.liquid
   ```

2. In Terminus, open **Extensions**, click **Upload**, and attach `extension.zip`. Terminus creates the extension with its fields, data and exchange.
3. Open the new extension. Under **Data**, set `location`, `latitude` and `longitude` for your place. The defaults are for Heraklion, Crete.
4. Under **Build Matrix**, choose **Models** and select your device's model. The export format does not include this setting.
5. Save the extension, then click **Build**. Terminus renders a screen for each selected model.
6. Open your device's playlist and add the new screen.

## Settings

| Field | Default | Meaning |
| --- | --- | --- |
| `location` | `Heraklion` | Name shown in the title bar. |
| `latitude` | `35.3387` | Latitude of the place. |
| `longitude` | `25.1442` | Longitude of the place. |
| `temperature_unit` | `celsius` | `celsius` or `fahrenheit`. |
| `wind_speed_unit` | `kmh` | `kmh`, `mph`, `ms` or `kn`. |
| `precipitation_unit` | `mm` | `mm` or `inch`. |
| `forecast_headings` | `relative` | `relative` shows Today and Tomorrow. `dates` shows weekday names. |
| `clock` | `24h` | `24h` or `12h`. |
| `locale` | `en-GB` | Language for day and month names, such as `el-GR`. |
| `hours` | `12` | Number of hours in the chart. The template shows fewer when they do not fit. |

## Keep the copies in sync

Terminus stores the live copy of the extension.
When you change the extension in Terminus, click its **Download** link on the **Extensions** page and unzip the export over these two files.
The export saves the template with Windows line endings. Convert it to Unix line endings before you commit:

```bash
tr -d '\r' < template.html.liquid > t && mv t template.html.liquid
```

## License

MIT. See [LICENSE](LICENSE).
