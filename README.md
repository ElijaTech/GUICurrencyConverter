## Documentation for Currency Converter Application
**Overview:** This Python program is a desktop application for currency conversion built using the `tkinter` library for the graphical user interface (GUI). It utilizes the ExchangeRate-API to perform real-time currency conversion and displays the results in a clean, user-friendly interface.
The application allows the user to select source and destination currencies, input an amount, and then calculate the converted value in real-time. The GUI also displays the timestamp of the last update of the conversion rates.
### Features:
1. User-friendly graphical interface for currency conversion.
2. Real-time exchange rate data fetched using `ExchangeRate-API`.
3. Supports multiple currencies from around the world.
4. Displays conversion results and timestamp of last data update.
5. Validates API response and catches errors in case of network issues or incorrect inputs.

### Installation and Setup:
1. **Prerequisites:**
    - Python 3.x
    - Modules: `tkinter`, `requests`

To install `requests`, use the following command:
``` bash
   pip install requests
```
1. **API Setup:**
    - This application uses the ExchangeRate-API for fetching currency exchange rates. You need to have a valid `API_KEY` to use this service.
    - Replace the placeholder `API_KEY` in the code with your own API key.
    - The API endpoints used in the application are:
        - `https://v6.exchangerate-api.com/v6/<API_KEY>/latest/USD` – Fetches supported currencies and their rates.
        - Pair-specific endpoint for conversion:
``` 
       https://v6.exchangerate-api.com/v6/<API_KEY>/pair/<source_currency>/<destination_currency>/<amount>
```
### Code Functionality:
#### 1. **API Requests:**
- The program fetches all available currencies and their exchange rates when initializing the application:
``` python
url = f'https://v6.exchangerate-api.com/v6/{API_KEY}/latest/USD'
response = requests.get(f'{url}').json()
currencies = dict(response['conversion_rates'])
```
- During currency conversion, it makes another call to the ExchangeRate-API to fetch the converted value in real-time:
``` python
result = requests.get(f'https://v6.exchangerate-api.com/v6/{API_KEY}/pair/{source}/{destination}/{amount}').json()
```
#### 2. **GUI Components:**
The GUI is developed using `tkinter`, divided into two main sections: top frame and bottom frame.
- **Top Frame:** Displays the title of the application.
- **Bottom Frame:** Contains input fields and labels for:
    - Source currency (dropdown combo box).
    - Destination currency (dropdown combo box).
    - Amount to be converted (entry box).
    - Convert button to trigger the currency conversion.
    - Fields for displaying the converted result and the last update timestamp.

#### 3. **Currency Conversion Function:**
The `convert_currency()` function handles the process of currency conversion after the user presses the "Convert" button. Its main tasks are:
- Collecting inputs from the GUI.
- Fetching conversion data from the API.
- Parsing the response to extract the conversion result and update timestamp.
- Updating the result on the GUI or showing an error using a message box in case of an issue.

### Key Code Components:
#### a. **Initialization of the Window (`window`)**
``` python
window = Tk()
window.geometry('310x340+500+200')  # Define size and position
window.title('Currency Converter')  # Title of the application
window.resizable(height=False, width=False)  # Disable resizing
```
#### b. **API Initialization**
``` python
url = f'https://v6.exchangerate-api.com/v6/{API_KEY}/latest/USD'
response = requests.get(f'{url}').json()  # Fetch currency rates
currencies = dict(response['conversion_rates'])  # Extract currency dictionary
```
#### c. **Dropdowns for Currency Selection**
``` python
from_currency_combo = ttk.Combobox(bottom_frame, values=list(currencies.keys()), width=14, font=('Poppins 10 bold'))
to_currency_combo = ttk.Combobox(bottom_frame, values=list(currencies.keys()), width=14, font=('Poppins 10 bold'))
```
#### d. **Conversion Function**
``` python
def convert_currency():
    try:
        source = from_currency_combo.get()
        destination = to_currency_combo.get()
        amount = amount_entry.get()
        result = requests.get(
            f'https://v6.exchangerate-api.com/v6/{API_KEY}/pair/{source}/{destination}/{amount}').json()
        converted_result = result['conversion_result']
        formatted_result = f'{amount} {source} = {converted_result} {destination}'
        result_label.config(text=formatted_result)
        time_label.config(text='Updated ' + result['time_last_update_utc'])
    except:
        showerror(title='Error', message='An error occurred. Please try again.')
```
### GUI Layout:
1. **Top Frame:**
    - Displays the title "Currency Converter".
    - Styled with the primary color: `#081F4D`.

2. **Bottom Frame:**
    - Inputs for:
        - Source currency (`from_currency_combo`).
        - Destination currency (`to_currency_combo`).
        - Amount to be converted (`amount_entry`).

    - Button for triggering the conversion (`convert_button`).
    - Labels for displaying the results and timestamps.

### Error Handling:
- The program uses a `try-except` block in the `convert_currency` function to handle errors gracefully:
    - Invalid currency pair or incorrect amount.
    - Network-related issues.
    - Invalid API responses.

- In case of an issue, it displays a message box with the error message using:
``` python
showerror(title='Error', message='An error occurred. Please try again.')
```
### Future Improvements:
1. Add more user-friendly validation for input fields.
2. Store the API key securely instead of hardcoding in the application.
3. Include a loading indicator during API requests to improve responsiveness.
4. Implement offline functionality using cached exchange rates.

### Example Workflow:
1. Select the source ("FROM") currency, e.g., USD.
2. Select the destination ("TO") currency, e.g., EUR.
3. Enter the amount to be converted (e.g., 100).
4. Press the "CONVERT" button.
5. The converted amount, along with the last update time, will be displayed in the app.

(Generated by openai-gpt-4o)
