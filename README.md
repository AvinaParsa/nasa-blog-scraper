# Web Scraping Pipeline for NASA Blog Articles

A scraping pipeline that collects articles from NASA's science blog, extracting structured metadata for each post and exporting it to CSV.

## Overview

The pipeline works in two steps:

1. Scrape the blog archive page to get a list of article links, titles, summaries, and thumbnail images.
2. Visit each article's page individually to collect the full detail: image caption, body text, author, publish date, and category tags.

Everything is collected into a single Pandas dataframe and exported to CSV, and both the article thumbnails and author avatars are downloaded locally.

## Scope

This run collects the 10 articles listed on the first load of the blog archive page. It doesn't yet follow the archive's pagination/infinite scroll to pull additional pages — the per-article scraper works on any post URL, so extending this to more pages is a matter of adding that pagination step, not rewriting the pipeline.

## Output

The CSV output (`web_crawling_nasa.csv`) has one row per article, with these columns:

| Column | Description |
|---|---|
| title | Article title |
| summary | Short summary from the archive listing |
| img_src | Thumbnail image URL |
| caption | Image caption from the article page |
| description | Full article body, as a list of paragraphs |
| auther_name | Author name |
| auther_img | Author avatar URL |
| date | Publish date |
| category | Category tags |
| link | Article URL |

## Tech Stack

- **Requests** — HTTP requests
- **BeautifulSoup** — HTML parsing and CSS selector-based extraction
- **Pandas** — structuring the scraped data
- **tqdm** — progress tracking during the per-article scraping loop

## Project Structure

```
.
├── notebook/
│   └── nasa_blog_scraper.ipynb
├── requirements.txt
└── README.md
```

## How to Run

1. Install the dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Open `notebook/nasa_blog_scraper.ipynb` and run it top to bottom.
3. Output CSV and downloaded images are written to an `output/` folder created automatically by the notebook.

## Notes

- CSS selectors are matched to NASA's current page layout and would need updating if the site's HTML structure changes.
- Author extraction assumes one author per post.
- Network requests aren't rate-limited beyond the retry-on-failure logic, so running this against a much larger number of pages would benefit from adding delays between requests.
