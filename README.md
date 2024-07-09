# DNSTwister-Application
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
    git clone https://github.com/michaelricci/dnstwister-application.git
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
3. Save the returned data to a csv file.

## Create the Application

1. If you dont want to run the application through traditional command line you can use pyinstaller to compile the code into a single executable.

2. Simply change directories to the dnstwister sript
   
4. Use:
 ```sh
   pyinstaller --name=dnstwister --onefile --windowed dnstwister.py
   ```
4. After a minute or so two folders will be added to wherever folder you have the script saved. The folder names will be dist and build.
   
6. If you go into the dist folder you will see your .exe file.
   
8. Click it to run or send it to someone who could use it! You only need to share the .exe file now that everything is compiled. They wont need python or need to download any dependencies.
   
## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
