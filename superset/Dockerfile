ARG SUPERSET_VERSION=5.0.0
FROM apache/superset:${SUPERSET_VERSION}

USER root

ENV PLAYWRIGHT_BROWSERS_PATH=/usr/local/share/playwright-browsers

RUN apt-get update -qy && \
  apt-get install build-essential python3-dev default-libmysqlclient-dev pkg-config -yq

RUN . /app/.venv/bin/activate && \
  uv pip install \
  psycopg2-binary \
  .[postgres] \
  .[bigquery] \
  google-cloud-bigquery-storage \
  mysqlclient \
  clickhouse-connect \
  .[elasticsearch] \
  mysqlclient \
  pymssql \
  # package needed for using single-sign on authentication:
  Authlib \
  # openpyxl to be able to upload Excel files
  openpyxl \
  # Pillow for Alerts & Reports to generate PDFs of dashboards
  Pillow \
  # install Playwright for taking screenshots for Alerts & Reports. This assumes the feature flag PLAYWRIGHT_REPORTS_AND_THUMBNAILS is enabled
  # That feature flag will default to True starting in 6.0.0
  # Playwright works only with Chrome.
  # If you are still using Selenium instead of Playwright, you would instead install here the selenium package and a headless browser & webdriver
  playwright \
  && playwright install-deps \
  && PLAYWRIGHT_BROWSERS_PATH=/usr/local/share/playwright-browsers playwright install chromium

USER superset

CMD ["/app/docker/entrypoints/run-server.sh"]
