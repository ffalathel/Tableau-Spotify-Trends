This project is a fully interactive Spotify Recap Dashboard, built in Tableau using your personal Spotify listening history. Inspired by Spotify Wrapped, this version provides clickable visual analytics, including top artists, top songs, time-of-day behavior, and the ability to play tracks via embedded Spotify.



 🚀 Features

- 🎨 Artist Photo Selection  
  Clickable circular artist photos dynamically filter the entire dashboard

- 📈 Top Songs by Selected Artist  
  Always shows the top 10 songs you personally played, even if they’re not globally popular

- ⏰ Listening Habits by Hour & Month  
  View average listening activity by hour and month

- 📊 Average Songs per Day/Month  
  Respects artist filters — recalculates KPIs when artists are selected

- ▶️ Spotify Embedded Player  
  Click a track to open and play it directly in Tableau via the Spotify Web Player



 📁 Data Format

Spotify's JSON listening history files like:
json
{
  "endTime": "2024-04-09 03:02",
  "artistName": "The Backseat Lovers",
  "trackName": "Pool House",
  "msPlayed": 200206
}
`

Merged into a single CSV using Python or Excel.

 🔧 Filtering Rules:

* Only plays over 30 seconds (`msPlayed >= 30000`) are counted
* Duplicate plays within the same hour are deduplicated in visualizations using `COUNTD`



 📊 Tableau Architecture

 📄 Sheets

* `Top Artists` — photo-based shape chart
* `Top Songs by Artist` — dynamic rank via `RANK_DENSE()` per filtered artist
* `Songs by Month` — timeline trend
* `Songs by Hour` — shows average plays per hour using `WINDOW_AVG()`
* `Avg Songs per Day/Month` — also powered by `WINDOW_AVG()` for dynamic filters

 🌐 Web Integration

* `spotifyTrackURL` is joined to each song to enable “Play” feature
* Dashboard includes a Web Page object + URL action to open tracks



 🧱 Setup Instructions

1. 📦 Export your Spotify streaming history
   From: [https://www.spotify.com/account/privacy](https://www.spotify.com/account/privacy)

2. 🛠 Merge JSONs into CSV using Python:

   python
   import json, glob
   import pandas as pd

   files = glob.glob("StreamingHistory_music_*.json")
   data = []
   for file in files:
       with open(file) as f:
           data.extend(json.load(f))

   df = pd.DataFrame(data)
   df.to_csv("spotify_streaming.csv", index=False)
   

3. 📸 Add artist photos to:

   
   Documents > My Tableau Repository > Shapes > SpotifyArtists
   

4. 🎵 Build `spotifyTrackURL` mapping for each top song (CSV join)

5. 🧠 Open Tableau:

   * Import CSV
   * Build all sheets as described
   * Create filter actions:

     * Click artist → filter charts and KPIs
     * Click track → show breakdown + play music



 🧪 Known Limitations

* Requires manual mapping of songs to Spotify track URLs
* Limited to static historical data (not real-time)



 📜 License

MIT License — use it, remix it, share it.

