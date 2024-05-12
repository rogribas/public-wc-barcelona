import requests
import json

def fetch_wc_data():
    url = "https://opendata-ajuntament.barcelona.cat/data/api/3/action/datastore_search?resource_id=d4333031-e42c-4d1a-9ad6-6254359b8f37&limit=200"
    response = requests.get(url)
    if response.status_code == 200:
        return response.json()
    else:
        print("Error fetching data:", response.status_code)
        return None

def generate_html_file(wc_data):
    html_content = """
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Lavabos públics de Barcelona</title>
        <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
        <style>
            body, html {
                height: 100%;
                margin: 0;
                overflow: hidden;
            }

            #map {
                height: calc(100% - 40px);
            }

            #title {
                text-align: center;
                padding: 10px;
                background-color: #f2f2f2;
                margin: 0;
            }
        </style>
    </head>
    <body>
        <h1 id="title" style="font-family: Helvetica, Arial, sans-serif">Lavabos públics de Barcelona</h1>
        <div id="map"></div>

        <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
        <script>
            // Initialize Leaflet map
            var map = L.map('map').setView([41.3851, 2.1734], 13);

            // Add OpenStreetMap tile layer
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(map);

            // Hardcoded WC data
            var wcData = """ + json.dumps(wc_data) + """;

            // Process each WC record
            wcData.result.records.forEach(record => {
                // Extract latitude and longitude
                var lat = parseFloat(record.geo_epgs_4326_lat);
                var lon = parseFloat(record.geo_epgs_4326_lon);

                // Add marker to the map
                L.marker([lat, lon]).addTo(map)
                    .bindPopup('<b>' + record.name + '</b><br>' + record.addresses_main_address);
            });
        </script>
    </body>
    </html>
    """

    with open("public_wc_map.html", "w") as file:
        file.write(html_content)

def main():
    wc_data = fetch_wc_data()
    if wc_data:
        generate_html_file(wc_data)

if __name__ == "__main__":
    main()
