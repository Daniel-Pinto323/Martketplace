Marketplace app

This application is heavily inspired by Facebook Marketplace but with the distinction that it will help me shop commercially via popular retail stores.

Key features:
- UI entrypoint
- retail store data scraping with frequent (live?) updates
- algorithmic feed recommendations for turning shopping into doomscrolling
- search to find specific items
- filter on shopping category/item/stores

Suplementary features:
- user login + user data
- show sales

PLAN

Goal
- Build a lightweight web app that lets a user scroll through listings from many stores in one continuous feed, like a personalized shopping feed instead of one store at a time.
- Keep the first version simple, fast, and reliable rather than trying to build a full marketplace platform immediately.

Core product idea
- A single page app with a vertical feed of products.
- Each product card shows: image, title, price, store name, category, and a link to the source listing.
- Users can scroll endlessly through a recommendation-heavy feed that combines items from multiple stores.
- The app supports basic filtering by category, store, and keyword search.

Implementation plan
1. Define MVP scope
   - Support 3-5 known retail stores.
   - Pull product data from a small set of public store pages or APIs.
   - Show a single feed of products sorted by relevance and recency.
   - Add search, category filters, and store toggles.
   - Keep the first version to a single-user local prototype without full login or payments.

2. Choose a simple stack
   - Backend: Python with Flask or FastAPI.
   - Frontend: lightweight HTML/CSS/JavaScript, or a small React app if the UI becomes more complex.
   - Database: SQLite for local MVP, with room to move to Postgres later.
   - Scheduler: a background job or cron process to refresh product data on a timed interval.

3. Build the data ingestion layer
   - Create a scraper or adapter for each store.
   - Normalize each store's product data into the same schema: id, title, price, category, store, image_url, product_url, updated_at.
   - Store all normalized items in a database table.
   - Deduplicate products by stable identifiers and update changed listings instead of adding duplicates.
   - Add retry and error handling so one broken site does not stop the feed.

4. Build the backend API
   - Add endpoints for:
     - listing the latest products
     - filtering by search term, category, or store
     - fetching a single product by id
     - optional recommendations based on category, price range, or recent activity
   - Keep the API simple and JSON-based for easy frontend integration.

5. Build the frontend experience
   - Create a home page that loads the product feed and renders a scrollable list of cards.
   - Each card should show enough info to browse quickly without opening the item page.
   - Add filter controls at the top or side panel.
   - Add a loading state, empty state, and error state.
   - Optimize for fast vertical scroll and lightweight rendering so the experience feels like endless browsing.

6. Add recommendation logic
   - Start with a basic rule-based recommender: sort by category match, descending freshness, and store popularity.
   - Later expand with simple heuristics such as price range preference, recently viewed items, or category weighting.
   - Keep the algorithm transparent and understandable rather than building a heavy ML system early.

7. Add persistence and freshness
   - Schedule periodic scraping jobs to refresh data every few minutes or hours depending on the store.
   - Track last_updated timestamps.
   - Remove stale listings after a defined grace period if they disappear from a store.

8. Create a usable MVP workflow
   - Step 1: set up the project skeleton and database.
   - Step 2: scrape and normalize one store.
   - Step 3: render a basic feed in the browser.
   - Step 4: add a second store and merge results.
   - Step 5: implement filtering and search.
   - Step 6: polish UI and test with a few real listings.

9. Future improvements
   - Add user accounts and saved searches.
   - Add price tracking and alerts.
   - Add AI-assisted shopping summaries or trend detection.
   - Add store-specific inventory status and shipping info.
   - Move from SQLite to a production database and deploy with Docker or a cloud host.

Success criteria for the first version
- A user can open the app and scroll through items from multiple stores in one continuous feed.
- Search and store filters work reliably.
- Data refreshes automatically without manual intervention.
- The app stays lightweight enough to run locally as a prototype.

This plan keeps the initial build focused on product discovery and endless browsing, rather than building a full ecommerce system before confirming the core experience works.

STARTING THE PROJECT

I chose FastAPI for the first version because it is Python-first, easy to read for beginners, and gives us a clean API for a product feed without the extra setup cost of a big frontend framework.

Why FastAPI instead of Flask?
- FastAPI is built around Python type hints and modern web conventions.
- It gives auto-generated API docs at /docs, which is helpful while learning.
- It works well with a simple HTML/JavaScript frontend that is lightweight and easy to debug.
- It keeps the backend and the feed logic straightforward for a project like this.

Project structure for this scaffold
- app/main.py: the FastAPI app and routes
- app/data.py: sample product data to simulate a multi-store feed
- app/templates/index.html: the page layout for the browser UI
- app/static/styles.css: styling
- app/static/app.js: simple front-end logic to load and filter products
- requirements.txt: Python dependencies

Local setup
1. Open a terminal in the project root.
2. Create a virtual environment:
   python -m venv .venv
3. Activate it:
   - Windows PowerShell: .\.venv\Scripts\Activate.ps1
   - Windows Command Prompt: .\.venv\Scripts\activate.bat
4. Install dependencies:
   python -m pip install -r requirements.txt
5. Start the app:
   uvicorn app.main:app --reload
6. Open the app in a browser:
   http://127.0.0.1:8000
7. Visit the API docs at:
   http://127.0.0.1:8000/docs

What the app is doing right now
- The backend exposes a /api/products endpoint.
- The frontend requests that endpoint and renders product cards in a scroll-friendly layout.
- The mock data includes several products from different stores so you can see the multi-store feed concept working.

How the Python pieces fit together
- app/main.py defines routes such as / and /api/products.
- A route is just a Python function that responds to a URL.
- FastAPI reads the URL, runs the function, and sends back JSON or HTML.
- Templates in app/templates let us render HTML with data from Python.
- The browser then uses JavaScript to fetch JSON and update the page without needing a full React app.

Next steps after this scaffold
1. Replace the mock data with real scraped catalog data from a small set of stores.
2. Add a SQLite database and store normalized product records.
3. Add filters and sorting logic in the backend.
4. Add a scheduled refresh job to update inventory regularly.
5. Add more search and recommendation features once the core feed works.

This is intentionally a simple MVP so you can learn the stack without getting trapped in frontend complexity before proving the shopping-feed concept works.



