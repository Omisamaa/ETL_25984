# U.S. Non-Farm Payrolls OLAP Analysis

This project provides an interactive dashboard for analyzing U.S. non-farm payroll data using an **ETL pipeline** and a **Streamlit application**. The ETL process extracts employment data from the FRED API, transforms it, and loads it into a PostgreSQL database. The Streamlit app then connects to this database to perform **OLAP (Online Analytical Processing)** analyses, including Slicing, Dicing, Roll-up, and Drill-down operations.

-----

## 🚀 Getting Started

Follow these steps to set up and run the project locally.

### Prerequisites

You'll need the following installed on your machine:

  - Python 3.8 or higher
  - PostgreSQL
  - A FRED API key (get one for free from the [FRED website](https://fred.stlouisfed.org/docs/api/api_key.html))

### Installation

1.  **Clone the repository:**

    ```bash
    git clone <repository_url>
    cd <repository_name>
    ```

2.  **Create a virtual environment and install dependencies:**

    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    pip install -r requirements.txt
    ```

    A `requirements.txt` file is not included in the provided code, but you can create one with the necessary packages:

    ```
    streamlit
    pandas
    psycopg2-binary
    plotly
    fredapi
    ```

-----

## ⚙️ ETL Pipeline

The ETL pipeline is responsible for preparing the data for the dashboard.

### `etl.py`

This script performs the following tasks:

  - **Extract**: Fetches the monthly non-farm payroll data (`PAYEMS`) from the FRED API using your API key.
  - **Transform**: Calculates the month-over-month (MoM) absolute and percentage changes in employment. It also renames the columns for clarity.
  - **Load**: Connects to a PostgreSQL database, creates the `nonfarm_payrolls` table if it doesn't exist, and loads the transformed data into it.

**To run the ETL script:**

1.  **Set up your environment variables** for the database connection and FRED API key. While the provided code uses hardcoded values, it's best practice to set these as environment variables to protect sensitive information.

      - `FRED_API_KEY`
      - `DB_NAME`
      - `DB_USER`
      - `DB_PASSWORD`
      - `DB_HOST`
      - `DB_PORT`

    For example, on Linux/macOS:

    ```bash
    export DB_USER='postgres'
    export DB_PASSWORD='413169'
    export FRED_API_KEY='d6b1ddc7bf442bd04ac1a58004015046'
    # ... and so on
    ```

2.  **Execute the script:**

    ```bash
    python etl.py
    ```

    This will populate your `nonfarm_payrolls` table in the `ETL` database.

-----

## 📊 Streamlit Dashboard

The Streamlit app provides an interactive and visual interface for data analysis.

### `app.py`

This script creates the dashboard and includes the following key components:

  - **Database Connection**: The `load_data()` function connects to the PostgreSQL database, queries the `nonfarm_payrolls` table, and uses **Streamlit's caching (`@st.cache_data`)** to ensure data is loaded only once, improving performance.
  - **Custom Styling**: The `add_custom_css()` function injects custom CSS to give the dashboard a clean and professional look with rounded corners, shadows, and a light background.
  - **OLAP Analyses**: The dashboard is structured with a sidebar navigation that allows users to select different types of OLAP analyses:
      - **Slicing**: Filters the data to show a specific range, such as average jobs created within a selected year range or a comparison between specific months in different years.
      - **Dicing**: Creates a subset of the data based on multiple criteria, like identifying months with a significant MoM drop or analyzing quarterly trends.
      - **Roll-up**: Aggregates data to a higher level of granularity, such as calculating quarter-over-quarter or year-over-year growth rates.
      - **Drill-down**: Explores data at a more detailed level, for example, by breaking down the highest annual employment gain into its monthly or quarterly components.

**To run the Streamlit app:**

1.  Ensure your database is running and populated with data from the ETL script.

2.  Navigate to the project directory and run:

    ```bash
    streamlit run app.py
    ```

    This will open the dashboard in your web browser.
