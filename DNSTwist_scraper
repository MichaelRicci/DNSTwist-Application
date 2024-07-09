import time
import pandas as pd
import tkinter as tk
from tkinter import messagebox, filedialog, scrolledtext, ttk
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import threading
from subprocess import Popen, PIPE
import json

def wait_for_page_load(driver, timeout=60):
    WebDriverWait(driver, timeout).until(
        lambda d: d.execute_script('return document.readyState') == 'complete'
    )

def wait_for_page_height_to_stabilize(driver, element_selector, timeout=120, interval=5):
    previous_height = 0.0
    end_time = time.time() + timeout

    while time.time() < end_time:
        element = driver.find_element(By.CSS_SELECTOR, element_selector)
        current_height = float(element.value_of_css_property("height").replace("px", ""))
        if current_height > previous_height:
            previous_height = current_height
        else:
            return
        time.sleep(interval)

def get_dnstwister_data(driver, domain):
    try:
        driver.get("https://dnstwister.report/")
        wait_for_page_load(driver)
        input_field = WebDriverWait(driver, 30).until(
            EC.presence_of_element_located((By.CLASS_NAME, "search-input"))
        )
        input_field.clear()
        input_field.send_keys(domain)
        input_field.send_keys(Keys.RETURN)
        WebDriverWait(driver, 90).until(
            EC.presence_of_element_located((By.CLASS_NAME, "content-wrap"))
        )
        wait_for_page_height_to_stabilize(driver, ".content-wrap")
        headers = driver.find_elements(By.CSS_SELECTOR, "#main_report thead th")
        header_texts = [header.text for header in headers]
        rows = driver.find_elements(By.CSS_SELECTOR, "#main_report tbody tr")
        data = []
        for row in rows:
            columns = row.find_elements(By.TAG_NAME, "td")
            if columns:
                data.append([column.text for column in columns])
        return header_texts, data
    finally:
        if 'data' in locals() and not data:
            print("Final page source for debugging:\n", driver.page_source)

def fetch_data(domains):
    options = webdriver.ChromeOptions()
    options.add_argument('--ignore-certificate-errors')
    options.add_argument('--ignore-ssl-errors')
    driver = webdriver.Chrome(options=options)
    all_data = []
    headers = None
    for domain in domains:
        domain = domain.strip()
        if domain:
            current_headers, data = get_dnstwister_data(driver, domain)
            if data:
                all_data.extend(data)
                if not headers:
                    headers = current_headers
            else:
                print(f"No data found for {domain}.")
    driver.quit()
    return headers, all_data

def save_data_as_json(headers, data):
    if data:
        df = pd.DataFrame(data, columns=headers)
        file_path = filedialog.asksaveasfilename(defaultextension=".json", filetypes=[("JSON files", "*.json")])
        if file_path:
            df.to_json(file_path, orient='records', lines=True)
            messagebox.showinfo("Success", f"Results saved to {file_path}")
    else:
        messagebox.showwarning("No Data", "No data to save.")

def get_data_as_json(headers, data):
    if data:
        df = pd
        .DataFrame(data, columns=headers)
        return df.to_json(orient='records')
    return json.dumps([])

def run_fetch_save(domains, output_text_widget):
    headers, data = fetch_data(domains)
    save_data_as_json(headers, data)
    output_text_widget.config(state=tk.NORMAL)
    output_text_widget.delete(1.0, tk.END)
    if data:
        output_text_widget.insert(tk.END, "Data fetched and saved successfully.")
    else:
        output_text_widget.insert(tk.END, "No data found.")
    output_text_widget.config(state=tk.DISABLED)

def on_submit_soc():
    domains = soc_domain_entry.get().split(',')
    threading.Thread(target=run_fetch_save, args=(domains, soc_output_text)).start()

# Create the main application window
app = tk.Tk()
app.title("DNS Twister Scraper")
app.geometry("600x400")
app.configure(bg="#007FFF")

# Add widgets to SOC tab
soc_title_label = tk.Label(app, text="DNS Twister Scraper", font=("Arial", 16), bg="#DBE2E9")
soc_title_label.pack(pady=10)

soc_domain_label = tk.Label(app, text="Enter domains (comma-separated):", font=("Arial", 12), bg="#DBE2E9")
soc_domain_label.pack(pady=5)

soc_domain_entry = tk.Entry(app, width=50, font=("Arial", 12))
soc_domain_entry.pack(pady=5)

soc_submit_button = tk.Button(app, text="Submit", command=on_submit_soc, font=("Arial", 12), bg="#4CAF50", fg="white", activebackground="#45a049")
soc_submit_button.pack(pady=20)

soc_output_text = scrolledtext.ScrolledText(app, width=90, height=20, font=("Arial", 10), state=tk.DISABLED)
soc_output_text.pack(pady=10)

app.mainloop()
