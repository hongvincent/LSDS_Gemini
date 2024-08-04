# LSDS: LLM based Smishing Detection System

## Diagram
![System Diagram](https://github.com/hongvincent/LSDS_Gemini/blob/main/lsds_gemini_seq_diagram.png)

This project provides a REST API built using FastAPI to detect potential phishing URLs from SMS text messages. It utilizes a combination of LLM(via Google's Gemini), web scraping, and external APIs to analyze and determine the likelihood of a URL being part of a Smishing attempt.

## Features

- **URL Extraction**: Extracts URLs from SMS text messages.
- **Phishing Detection**: Integrates with an external `criminalip.io` API to determine if a URL is associated with malicious activity.
- **Web Scraping**: Uses Selenium and BeautifulSoup to analyze web page content for phishing indicators.
- **LLM Integration**: Leverages a Generative AI model (Google Gemini) to perform an in-depth analysis of web page source code, looking for phishing characteristics.
- **Response Classification**: Classifies URLs into categories like "phishing", "caution", "attention", or "safe" based on the analysis results.

## Installation

### Prerequisites

- Python 3.8 or higher
- [Poetry](https://python-poetry.org/) or `pip` for package management
- ChromeDriver for Selenium (ensure compatibility with your Chrome version)
- A `.env` file with the following environment variables:
  - `GEMINI_API_KEY`: Your API key for Google Gemini AI.
  - `CRIMINAL_API_KEY`: Your API key for the `criminalip.io` service.

### Step-by-Step Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/phishing-detection-api.git
   cd phishing-detection-api
   ```

2. **Install dependencies**:
   If you use Poetry:
   ```bash
   poetry install
   ```
   Or using pip:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up the `.env` file**:
   Create a `.env` file in the project root directory and add your API keys:
   ```env
   GEMINI_API_KEY=your_gemini_api_key
   CRIMINAL_API_KEY=your_criminal_api_key
   ```

4. **Run the application**:
   ```bash
   uvicorn main:app --reload
   ```

   The API will be accessible at `http://0.0.0.0:8000`.

## Usage

### API Endpoints

#### 1. Check SMS for Phishing URLs
- **Endpoint**: `/check-sms`
- **Method**: `POST`
- **Request Body**: 
  ```json
  {
    "sms_text": "Your SMS message containing URLs"
  }
  ```
- **Response**:
  ```json
  {
    "result": "phishing" | "caution" | "attention" | "safe"
  }
  ```

#### 2. Direct LLM Phishing Detection
- **Endpoint**: `/llm`
- **Method**: `POST`
- **Request Body**:
  ```json
  {
    "sms_text": "A single URL to be analyzed"
  }
  ```
- **Response**:
  ```json
  {
    "result": "[totalscore: <score>]"
  }
  ```

### Example Request

Using `curl` to send an SMS text for phishing analysis:
```bash
curl -X POST "http://localhost:8000/check-sms" -H "Content-Type: application/json" -d '{"sms_text": "Check out this link http://example.com"}'
```

### Expected Output
The API will analyze the URL(s) and return a result indicating whether the URL is safe or a potential phishing site.

## Project Structure

- **`main.py`**: The main application file where all endpoints and logic are defined.
- **`requirements.txt`**: Dependencies required to run the application.
- **`.env`**: Environment file to store API keys (not included in the repository for security reasons).

## Technologies Used

- **FastAPI**: A modern, fast web framework for building APIs with Python.
- **Pydantic**: Data validation and settings management using Python type annotations.
- **Selenium**: For automating web browser interaction and scraping web content.
- **BeautifulSoup**: For parsing HTML and extracting specific elements from web pages.
- **Google Gemini AI**: Used to perform machine learning analysis on web page content.
- **CriminalIP API**: An external API to check if a URL is associated with malicious activity.

## Contributing

1. Fork the repository.
2. Create a new feature branch (`git checkout -b feature/new-feature`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature/new-feature`).
5. Create a Pull Request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgements

Special thanks to the developers and maintainers of the Google Gemini, FastAPI, Selenium, and libraries, which made this project possible.
