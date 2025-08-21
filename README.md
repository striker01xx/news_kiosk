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
$url = "https://gnews.io/api/v4/search?q=AI+OR+technology&lang=en&max=5&apikey=YOUR_API_KEY";
$json = file_get_contents($url);
$data = json_decode($json, true);
```

---

### Parameters

- `q` – keywords or Boolean operators (e.g., `"AI OR technology"`)  
- `lang` – language code (e.g., `en`)  
- `max` – maximum number of articles (default: 10)  
- `apikey` – your personal GNews API key  

---

### Example JSON Response

```json
{
  "articles": [
    {
      "title": "AI breakthrough announced",
      "url": "https://example.com/article1",
      "publishedAt": "2025-08-20T10:00:00Z",
      "source": "Example News"
    }
  ]
}
```

---

### Customization

- To change keywords → modify `q` in the respective parser script.  
- To change article count → adjust `max`.  
- To change language → update `lang`.  
- To use your own API key → replace `apikey`.  

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
- Access to the [GNews API](https://gnews.io/) (API key required)  

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
