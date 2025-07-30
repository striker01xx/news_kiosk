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

tbd: Ordnerstruktur

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

## Parser and API Usage

The application uses the [GNews API](https://gnews.io/) to retrieve current news data. Each parser uses a `POST` call to fetch and write 5 unique articles per category to a `.json` file.

**Example request inside `tech_parser.php`:**

```php
$url = "https://gnews.io/api/v4/search?q=AI+OR+technology&lang=en&max=5&apikey=YOUR_API_KEY";
