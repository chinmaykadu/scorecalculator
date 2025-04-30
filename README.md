# Score Calculator

This project contains a Python script to calculate the marks of an exam based on a response sheet from the Objection Raising Portal. The script extracts the candidate's responses and compares them to the correct options, both of which are available in the HTML file of the response sheet, and calculates the score accordingly. The script works by parsing the HTML file using the BeautifulSoup library.

## Requirements

To run the script, the following Python libraries are required:
-	beautifulsoup4: To parse the HTML file and extract data.
-	requests (optional if you choose to download the file from a URL).


## Getting Started

### 1. Clone the Repository
First, clone the repository to your local machine or GitHub Codespace:
```bash
git clone https://github.com/yourusername/exam-marks-calculator.git
cd exam-marks-calculator
```
### 2. Set up Google Colab (Optional, if using Google Colab for file access):
If you want to use the Google Colab environment to read the HTML file from your Google Drive, follow these steps:
-Upload your HTML response file to Google Drive.
- In Google Colab, mount your Google Drive:
```python
from google.colab import drive
drive.mount('/content/drive')
```
- Use the path to your file from Google Drive:
```python
file_path = '/content/drive/MyDrive/Colab Notebooks/Assessment - Objection Tracker Portal_ Response Sheet.html'
```
### 3. Running the Script Locally
If you don't want to use Google Colab and prefer to run the script locally:
- You will need to download the .html file and place it in the same directory as the script or provide the correct path to the HTML file in your local file system.
- Install the required dependencies:
```bash
pip install beautifulsoup4
```
- Run the Python script:
```bash
python calculate_marks.py
```
The script will output the calculated score and a breakdown of the responses.

## Script Overview
### Code Workflow

#### 1. Mount Google Drive (Optional):
If using Google Colab, the script will mount your Google Drive to access the HTML file.
#### 2. Read the HTML File:
The script reads the HTML file containing the response sheet.
#### 3. Extract Tables:
The script looks for tables in the HTML file where each table represents a question, and the <span> elements contain the correct option and the candidate's response.
#### 4. Compare Responses:
The script compares the candidate's selected answer with the correct option and increments the score for each correct answer.
#### 5. Output the Score:
The total score is printed, and a breakdown of each question's response is shown (whether the answer was correct or incorrect).

## Example Output:

The script will output a list of the correct and incorrect answers and the total score.
#### Example:
```Breakdown:
Correct Option: A, Candidate Response: A, Status: Correct
Correct Option: B, Candidate Response: A, Status: Incorrect
...
Total Score: 15
```
## Code Block
```python
from google.colab import drive
drive.mount('/content/drive')

from bs4 import BeautifulSoup

file_path = '/content/drive/MyDrive/Colab Notebooks/Assessment - Objection Tracker Portal_ Response Sheet.html'

with open(file_path, 'r', encoding = 'utf-8') as file:
  soup = BeautifulSoup(file, 'html.parser')

# Initialize score
score = 0
breakdown = []

# Find all tables containing Correct Option and Candidate Response
tables = soup.find_all('table', class_='table table-responsive table-bordered center')

# Calculate the score and create a breakdown table
for table in tables:
    spans = table.find_all('span')
    if len(spans) >= 2:
        correct_option = spans[0].text.strip()
        candidate_response = spans[1].text.strip()

        if correct_option == candidate_response:
            score += 1
            status = "Correct"
        else:
            status = "Incorrect"

    breakdown.append((correct_option, candidate_response, status))

print("Breakdown:")
for item in breakdown:
    print(f"Correct Option: {item[0]}, Candidate Response: {item[1]}, Status: {item[2]}")
print(f"Total Score: {score}")
```


## Acknowledgement

- This project uses BeautifulSoup for parsing HTML content.
- Thanks to Google Colab for providing an easy way to mount Google Drive for seamless file access.
