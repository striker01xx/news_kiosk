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

- QR Code linking to the original article  
- QR Code to vote for the article  
- Footer QR Codes for navigating to other categories or ending the session  

### Example Footer Navigation

- Landing Page → `display/start.html`  
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

## Server Management

### Run Locally

Start your PHP server inside the project folder:

```bash
php -S localhost:8000 -t public_html/
```

Access at: `http://localhost:8000/display/start.html`

### Deployment

- Copy project files to your hosting provider (e.g., via `scp` or `rsync`).  
- Ensure file permissions for `/jsonpasser/data/`.  

---

## Testing and Validation

- Verify JSON files are updated after parser execution.  
- Check QR codes lead to correct article links and navigation paths.  
- Confirm voting updates `voted_articles.json`.  

---

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
