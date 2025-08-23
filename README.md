# news_kiosk
**Make News Sexy Again**

# Interactive News Kiosk

## Table of Contents
- [Project Description](#project-description)
- [Project Architecture](#project-architecture)
- [API Documentation](#api-documentation)
- [QR Navigation](#qr-navigation)
- [Installation](#installation)
- [Server Management](#server-management)
- [Testing and Validation](#testing-and-validation)
- [Tech Stack](#tech-stack)
- [License](#license)

---

## Project Description

This project powers a QR-driven interactive news kiosk for public display environments.  
Users can access current news topics (Sports, Economy, Science, Tech, Entertainment) via a large screen and scan QR codes to vote for their favorite articles or navigate to related content.

### Key Features
- Touchless interaction via QR codes  
- Real-time news fetched from [GNews API](https://gnews.io/)  
- Voting mechanism with persistent result storage  
- Start page highlighting top-voted articles  

---

## Project Architecture

```plaintext
public_html/
├── waitqr/                 # QR-Callback-Service
│   ├── initiate.php        # Starts QR-Callback
│   ├── callback.php        # QR-Scan Callback Handler
│   ├── callback.url        # Callback-URL File
│
├── qrcodes/                # QR-Codes
│   ├── qr_sports.png
│   ├── qr_science.png
│   ├── qr_tech.png
│   ├── qr_end.png
│   ├── qr_landing.png
│   ├── qr_economic.png
│
├── jsonpasser/             # Parsers for content. They talk to the API when called via POST fromt the CPEE. Then they write the content to the respective JSON files.
│   ├── tech_parser.php
│   ├── vote.php            # gets sent an article ID and a theme, then writes the article in the voted_articles.json
│   ├── dadjoke_parser.php
│   ├── economic_parser.php
│   ├── sports_parser.php
│   ├── science_parser.php
│   ├── .index.php.swp
│   ├── data/               # JSON-Data Sources
│   │   ├── tech_data.json
│   │   ├── dadjoke.json
│   │   ├── economic_data.json
│   │   ├── sports_data.json
│   │   ├── science_data.json
│   │   ├── voted_articles.json
│
├── display/                # Pages
│   ├── start.html          # Start page for the main process
│   ├── main.html           # Main Page where recommended articles can be seen and you can navigate to the 
│   ├── navigation.html     # Navigation bar
│
├── categories/             # Category pages and their respective navigation bar
│   ├── science.html
│   ├── science_navigation.html
│   ├── sports.html
│   ├── sports_navigation.html
│   ├── tech.html
│   ├── tech_navigation.html
│   ├── economic.html
│   ├── economic_navigation.html
│   ├── style.css           # Stylesheet
```

---

## API Documentation

The system fetches articles from the [GNews API](https://gnews.io/)  
and stores them in category-specific JSON files. Each parser script (`*_parser.php`) handles one category.

### Endpoints and Scripts

| Parser Script        | Query Parameters                         | Output JSON             |
|----------------------|------------------------------------------|-------------------------|
| `tech_parser.php`    | `q=AI OR technology&lang=en&max=5`       | `data/tech_data.json`   |
| `economic_parser.php`| `q=economy OR finance&lang=en&max=5`     | `data/economic_data.json`|
| `sports_parser.php`  | `q=sports&lang=en&max=5`                 | `data/sports_data.json` |
| `science_parser.php` | `q=science OR research&lang=en&max=5`    | `data/science_data.json`|
| `dadjoke_parser.php` | `q=dad+joke&lang=en&max=5`               | `data/dadjoke.json`     |
| `vote.php`           | POST request with article ID             | `data/voted_articles.json`|

---

### Example API Call (from `tech_parser.php`)

```php
$url = "https://gnews.io/api/v4/search?q=AI+OR+technology&lang=en&max=5&apikey=92b57dabb4b148dacaafc52214785cb2";
$json = file_get_contents($url);
$data = json_decode($json, true);
```

---

### Parameters

- `q` – keywords or Boolean operators (e.g., `"AI OR technology"`)  
- `lang` – language code (e.g., `en`)  
- `max` – maximum number of articles (default: 10)  
- `apikey` – the personal gnewsAPI key, which is: 92b57dabb4b148dacaafc52214785cb2

---

### Example JSON Response

```json
{
      "id": "bbe34feeabae428b9679dd3d183a05c9",
      "title": "Fast & Furious: Arcade Edition release date revealed",
      "description": "The game will be released on PS5, Xbox Series X|S and Nintendo Switch.",
      "content": "Open Extended Reactions\nIn a press release, developer Cradle Games and publisher GameMill Entertainment revealed that Fast & Furious: Arcade Edition will be released on PS5, Xbox Series X|S and Nintendo Switch on Oct. 24, 2025.\nAs the name suggests, ... [1133 chars]",
      "url": "https://www.espn.ph/gaming/story/_/id/46038583/fast-furious-arcade-edition-release-date",
      "image": "https://a3.espncdn.com/combiner/i?img=%2Fphoto%2F2025%2F0821%2Fr1534781_1296x729_16%2D9.png",
      "publishedAt": "2025-08-21T17:19:39Z",
      "lang": "en",
      "source": {
        "id": "a38b4683030b8963222bbd436bbe8ed5",
        "name": "ESPN Philippines",
        "url": "https://www.espn.ph",
        "country": "ph"
      }
    }
```

---

### Customization

- To change keywords → modify `q` in the respective parser script.  
- To change article count → adjust `max`.  
- To change language → update `lang`.  
- To use another API key → replace `apikey`.

---

## QR Navigation

Each screen is designed for QR-driven interaction. Every article card includes:

- QR Code linking to the original article which gets created by using the api.qrserver.com API to create them dynamically
- QR Code to vote for the article. The voting Link gets created dynamically aswell with the same API and contains the article ID gets passed to the vote.php which then stores the json of the article in the voted_articles.json file
- Footer QR Codes for navigating to other categories or ending the session. The used links within the qr Codes are:
```Navigation Links
https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=tech
https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=landing
https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=sports
https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=economy
https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=science
https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=end
```


### Example Footer Navigation

- Start Page → `display/start.html`  
- Landing Page → `display/main.html`  
- Tech → `categories/tech.html`  
- Sports → `categories/sports.html`  
- Economy → `categories/economic.html`  
- Science → `categories/science.html`  

---

## Installation

### Prerequisites

- Web server with PHP support  
- Access to the [GNews API](https://gnews.io/)

### Setup Steps

1. Clone or copy the repository into your web server’s public folder.  
2. Update API keys and query parameters in each parser script under `jsonpasser/`.  
3. Ensure the `jsonpasser/data/` folder is writable by the web server (to save `.json` files).  
4. Access the system via your browser:  
   `http://<your-server>/public_html/display/start.html`  

---


## Testing and Validation

- Verify JSON files are updated after parser execution.  
- Check QR codes lead to correct article links and navigation paths.  
- Confirm voting updates `voted_articles.json`.  

---

## Extendability
1. Adding new topic (same API): (1) To add a new topic, a new .php parser for the topic needs to be created in the jsonpasser/ folder and a new .json file to store the data in the jsonpasser/data/ folder. (2) A parser from the other topics can be copied, the API call should be changed the path, where the result gets stored needs to be changed. (3) Then in the cageories/ folder, a new html page needs to be created. Also, here another topic page can be copied and the path where the data comes from needs to be changed aswell as the headline. (4) A new QR code needs to be generated with the URL "https://lehre.bpm.in.tum.de/~go56sew/waitqr/callback.php?navigate=X", where X is the new topic. (5) In the process engine, a new subprocess needs to be created with the naming of show_X.xml. The process model consists of 3 tasks, where the first one sends a POST request to the parser to update the data in the backend, while the other two are responsible for displaying the page and the navigation bar.
2. Adding new topic (other API): (1) + (3) + (4) + (5) remain the same, (2) changes, that you need to insert a new API + the key and not just change the keywords.

## Tech Stack

- PHP (parser and QR callback handling)  
- HTML/CSS/JavaScript (frontend display)  
- JSON (article storage, votes)  
- [GNews API](https://gnews.io/) (news source)  

---

## License

This project is licensed for educational use only.  
Commercial use requires explicit permission.  
See [LICENSE](LICENSE) for full terms.
