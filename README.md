# Week 7 – End-to-End Telegram Data Product

This repo contains a reproducible pipeline that turns raw Telegram channel data into an analytical API.

## Folder Structure

- notebooks/ – step-by-step Jupyter notebooks
    - 01_scraping.ipynb – scrape public channels and land data in `data_lake/raw`
    - 02_dbt_modeling.ipynb – build a star schema in Postgres via dbt
    - 03_yolo_enrichment.ipynb – run YOLOv8 on images and write predictions
    - 04_analytics_api.ipynb – expose metrics with FastAPI
- dbt/ – dbt project (models, seeds, snapshots)
- dagster/ – orchestrated jobs (assets + schedules)
- infrastructure/ – docker-compose and Terraform scaffolds

## Quickstart

```bash
# 1. Clone and spin up services
make up            # postgres, minio, dagster, fastapi

# 2. Ingest 24h of history from sample channels
python pipelines/scraper.py --channels file:channels.txt --hours 24

# 3. Run transformations
cd dbt && dbt deps && dbt seed && dbt run && dbt test

# 4. Serve the API
uvicorn app.main:api --reload --port 8080
```

## Business Questions Answered

1. Top 10 most mentioned medical products across channels.
2. Price trend of a product by channel and over time.
3. Channels with highest share of visual content.
4. Daily & weekly posting volume by topic.

Queries are under `dbt/models/marts/`.

## Requirements

See `requirements.txt`. Use Python 3.10+.

## Authors

Built by Julius AI autogenesis – finish and iterate!
