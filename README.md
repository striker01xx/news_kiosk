# news_kiosk
Make News Sexy Again

# Interactive News Kiosk

## Table of Contents
- [Project Description](#project-description)
- [Project Architecture](#project-architecture)
- [QR Navigation](#qr-navigation)
- [Parser and API Usage](#parser-and-api-usage)
- [Installation](#installation)
- [Server Management](#server-management)
- [Tech Stack](#tech-stack)
- [License](#license)

---

## Project Description

This project powers a QR-driven interactive news kiosk for public display environments. Users can access current news topics (e.g., Sports, Economy, Science) via a large screen and scan QR codes to vote for their favorite articles or navigate to related content.

### Key Features:
- Touchless interaction via QR codes
- Real-time news fetched from [GNews API](https://gnews.io/)
- Voting mechanism with persistent result storage
- Start page highlighting top-voted articles

---

## Project Architecture

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
├── jsonpasser/             # Parser for content
│   ├── tech_parser.php
│   ├── vote.php
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
├── display/                # Main pages
│   ├── start.html
│   ├── main.html
│   ├── navigation.html
│
├── categories/             # Category pages
│   ├── science.html
│   ├── science_navigation.html
│   ├── sports.html
│   ├── sports_navigation.html
│   ├── tech.html
│   ├── tech_navigation.html
│   ├── economic.html
│   ├── economic_navigation.html
│   ├── style.css           # Stylesheet

---

## QR Navigation

Each screen is designed for QR-driven interaction. Every article card contains:

- QR Code linking to the original article
- QR Code to vote for the article
- Footer QR Codes for navigating to other categories or ending the session

### Example Footer Navigation:

- [Landing Page](display/index.html)  
- [Tech](display/tech.html)  
- [Sports](display/sports.html)  
- [Economy](display/economy.html)  
- [Science](display/science.html)

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

php
$url = "https://gnews.io/api/v4/search?q=AI+OR+technology&lang=en&max=5&apikey=92b57dabb4b148dacaafc52214785cb2";
$json = file_get_contents($url);
$data = json_decode($json, true);

### Parameters
q – keywords or Boolean operators (e.g., "AI OR technology")
lang – language code (e.g., en)
max – maximum number of articles (default: 10)
apikey – your personal GNews API key

### Example Resonse
{
            "id": "4b0be16b14f760b8d5b9cba147becc51",
            "title": "Shinkai Launches v1.0: Onchain AI Agents Go Live with USDC & Coinbase x402",
            "description": "Georgetown, Cayman Islands, 29th July 2025, Chainwire",
            "content": "Georgetown, Cayman Islands, July 29th, 2025, Chainwire\nShinkai, the open-source, local-first platform for building and sharing autonomous AI agents, has officially released version 1.0, its first production-ready build. With support for USDC micro-pa... [4106 chars]",
            "url": "https://techstartups.com/2025/07/29/shinkai-launches-v1-0-onchain-ai-agents-go-live-with-usdc-coinbase-x402",
            "image": "https://techstartups.com/wp-content/uploads/2025/07/image_23_1753469869HjLRF0f7bT-960x640.jpg",
            "publishedAt": "2025-07-29T13:11:14Z",
            "source": {
                "id": "a3b1ce1267ce02959f0b6a7bc018cec4",
                "name": "TechStartups.com",
                "url": "https://techstartups.com"
            }
        }

### Customization
To change keywords → modify q in the respective parser script.
To change article count → adjust max.
To change language → update lang.
To use your own API key → replace apikey.
