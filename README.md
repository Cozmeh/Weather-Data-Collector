# Weather Data Collector

This Python script collects weather data automatically using GitHub Actions. It runs on a schedule of 5:30 GMT, collects the latest weather information from the Open-Meteo API, and pushes the changes to the repository for easy tracking and analysis.

## Features

- Automatic weather data collection
- Scheduled execution using GitHub Actions
- Data collected includes hourly temperature readings
- Commit and push data to the repository

## Prerequisites

Before running this script, make sure you have the following:

- Python 3 installed on your machine
- A GitHub account
- Access to the repository where you want to store the weather data

## Usage

1. Clone the repository to your local machine:

   ```bash
   https://github.com/Cozmeh/WeatherCollector.git
   ```

2. Navigate to the project directory:

   ```bash
   cd WeatherCollector
   ```

3. Install the required Python packages:

   ```bash
   pip install requests
   ```

4. Open the `collector.py` script in a text editor.

5. Modify the latitude and longitude values in the API call to specify the location for which you want to collect weather data. For example:

   ```python
   rawData = requests.get('https://api.open-meteo.com/v1/forecast?latitude=12.9719&longitude=77.5937&hourly=temperature_2m&forecast_days=1')
   ```

   Replace `latitude` and `longitude` with the coordinates of your desired location.

6. Save the changes to the script.

7. Commit the script to the repository:

   ```bash
   git add collector.py
   git commit -m "Add Weather Data Collector script"
   git push
   ```

8. Go to your GitHub repository and navigate to the "Actions" tab.

9. Click on "Set up a workflow yourself" and create a new workflow file, such as `.github/workflows/weather-data-collector.yml`.

10. Paste the following code into the workflow file:

    ```yaml
    name: Weather Data Collector

    on:
      schedule:
        - cron: '30 5 * * *' # Schedule to run at 5:30 GMT (replace with your desired time)

    jobs:
      collect-weather-data:
        runs-on: ubuntu-latest

        steps:
          - name: Checkout repository
            uses: actions/checkout@v2

          - name: Set up Python
            uses: actions/setup-python@v2
            with:
              python-version: '3.x'

          - name: Install dependencies
            run: |
              pip install requests

          - name: Run Weather Data Collector script
            run: python collector.py

          - name: Commit and push changes
            run: |
              git config user.email "yourEmail@mail.com"
              git config user.name "AnyName"
              git add -A
              git commit -m "Updated Today's Weather"
              git push
    ```

    Ensure that the `cron` expression in the `on.schedule` section matches your desired schedule. The example above runs at 5:30 GMT. ie 11:00 IST

11. Commit and push the workflow file to the repository

The script will now run automatically according to the specified schedule, collect weather data from the [Open-Meteo API](https://open-meteo.com/en/docs), and commit and push the changes to the repository. The collected data will be stored in the `HourlyTemperature.csv` file as the weather dataset.
### Note
* Currently the script collects the weather data of Bengaluru
* Use [Open-Meteo](https://open-meteo.com/en/docs) to get your desired location's weather
