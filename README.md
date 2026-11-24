# Bangle.js activity exporter

Export Bangle.js activity data as TCX files for services such as Strava.

## Supported sources

### Recorder CSV (recommended for real runs)

The Bangle.js [Recorder app](https://banglejs.com/apps/?id=recorder&readme) records GPS, heart rate, altitude, speed and other fields. The exporter converts Recorder CSV tracks to TCX, preserving GPS trackpoints and available heart-rate data.

For automatic Run/Run+ recording:

1. Install the separate **Recorder** app.
2. Open Recorder and configure GPS and any other fields you want to capture.
3. In **Run** or **Run+** settings, enable **Record Run**.
4. Start and stop the run normally. Recorder creates the track file.
5. Connect to this exporter, select the Recorder CSV and export TCX.

Run/Run+ do not independently save a GPS route. The `runplus_karvonen` file is a Run+ support module, not a saved workout.

### Health Tracking monthly RAW files (limited recovery)

The exporter can also read `health-YYYY-MM.raw` files created by Bangle.js Health Tracking (these are written by the Health Tracking app, not by Run+ itself). Each file covers a whole month, so instead of listing the raw month file as one opaque entry, the exporter decodes it and **lists each detected exercise as an individual activity** in the file picker. For HEALTH2 files, it finds records classified by the watch as **EXERCISE**, groups adjacent ten-minute samples into approximate periods, and shows one row per period (start time, duration, average heart rate). Select a single row to export just that activity as a TCX file, or the month row ("All N activities") to export every period of the month as separate TCX `<Activity>` elements with available heart-rate summaries. A downloaded `.raw` file can also be dropped onto the page manually, and is expanded the same way.

**Important limitations:** Health Tracking is a background wellness summary, not a workout recorder. Its samples are approximately ten minutes apart and do not contain GPS coordinates, a route, reliable activity distance, pace, or exact workout start/stop times. These exports are therefore non-GPS `Other` activities with zero distance; they are not reconstructed Run+ runs. Older HEALTH1 files do not contain activity classifications and cannot be segmented into exercise sessions. The export interprets timestamps in the browser's local timezone.

If you need a real running activity with a map and distance, the Recorder app must be installed and configured before the run. Historical GPS points cannot be recreated from Health Tracking data.

## Export

1. Open the page in Chrome or Edge over HTTPS.
2. Connect the Bangle.js over Bluetooth.
3. Select a Recorder CSV, an individual health activity, or a Health Tracking month file.
4. Review the preview and download the generated TCX.
5. Upload the TCX using Strava's [upload page](https://www.strava.com/upload/select).

The Strava button downloads the TCX and opens Strava's upload page. Direct Strava API upload would require OAuth and a backend, so this static page does not upload directly.
