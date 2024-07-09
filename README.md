# DNSTwist-Application
Scripts aimed at making a CTI analyst's life easier!
# DNS Twister Scraper

A Python application to scrape data from the DNS Twister website and save it as JSON. The application uses Selenium for web scraping, Pandas for data manipulation, and Tkinter for the graphical user interface.

## Features

- Fetch DNS Twister data for multiple domains
- Save data as JSON
- User-friendly GUI

## Installation

1. Clone the repository:
    ```sh
    git clone https://github.com/michaelricci/dns-twister-scraper.git
    cd dns-twister-scraper
    ```

2. Create and activate a virtual environment:
    ```sh
    python3 -m venv venv
    source venv/bin/activate   # On Windows use `venv\Scripts\activate`
    ```

3. Install the dependencies:
    ```sh
    pip install -r requirements.txt
    ```

## Usage

1. Run the application:
    ```sh
    python src/gui.py
    ```

2. Enter the domains (comma-separated) in the input field and click "Submit".

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
