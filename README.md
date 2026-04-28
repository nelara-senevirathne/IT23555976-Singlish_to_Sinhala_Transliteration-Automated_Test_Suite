# Singlish to Sinhala Transliteration – Automated Test Suite

**IT3040 – ITPM | Assignment 1 | Option 1**

This project automates the testing of the **Chat Sinhala transliteration** function available at [https://www.pixelssuite.com/chat-translator](https://www.pixelssuite.com/chat-translator). It uses **Playwright** to simulate real user interactions and automatically records the actual output and pass/fail status for each test case in an Excel file.

---

## Project Structure

```
test_automation/
├── test_automation.py          # Main Playwright automation script
├── Assignment 1 - Test cases.xlsx  # Excel file with all 50 test cases
└── Commands.txt                # Quick reference for commands
```

---

## Prerequisites

Before running the tests, make sure the following are installed on your machine:

- **Python 3.11 or 3.12** – [Download here](https://www.python.org/downloads/)
- **Google Chrome** (recommended) – [Download here](https://www.google.com/chrome/)
  - Alternatively, Playwright can install its own Chromium browser (see Step 3)

---

## Installation

Follow these steps **once** to set up the project environment.

### Step 1 – Extract the Project

Save the ZIP file to your **D: drive** and extract it so the folder path is:

```
D:\test_automation\
```

### Step 2 – Open Command Prompt

Press `Win + R`, type `cmd`, and press Enter.

### Step 3 – Navigate to the Project Folder

```bash
cd /d D:\test_automation
```

### Step 4 – Install Required Dependencies

Run the following commands one by one:

```bash
pip install -U pip
pip install playwright openpyxl
playwright install
```

> **Note:** The `playwright install` command downloads the necessary browser binaries. This may take a few minutes depending on your internet speed.

---

## Preparing the Excel File

Before running the automation script:

1. Open `Assignment 1 - Test cases.xlsx` from the `test_automation` folder.
2. Make sure the following four columns are filled in for all 50 test cases:
   - **Column A** – `Test Case ID` (e.g., Neg_0001)
   - **Column B** – `Input length type` (S, M, or L)
   - **Column C** – `Input` (the Singlish text to test)
   - **Column D** – `Expected output` (the correct Sinhala translation)
3. **Do NOT enter anything** in Column E (`Actual output`) or Column F (`Status`) — these are filled in automatically by the script.
4. Save and **close** the Excel file before running the script.

---

## Running the Tests

From the Command Prompt (inside `D:\test_automation`), run the following command:

```bash
python test_automation.py --excel "test_automation/Assignment 1 - Test cases.xlsx" --url "https://www.pixelssuite.com/chat-translator" --wait-ms 5000 --type-delay-ms 80 --slow-mo-ms 200 --save-every 1 --keep-open
```

### What Happens When You Run It

1. A **Chrome browser window** opens automatically.
2. The script navigates to the transliteration website.
3. For **each test case row**, the script:
   - Types the Singlish input into the chat text box
   - Clicks the Transliterate button
   - Waits for the Sinhala output to appear
   - Records the actual output in **Column E**
   - Compares it against the expected output and writes **PASS** or **FAIL** in **Column F**
4. Results are **saved after every test case** (`--save-every 1`) so no data is lost if interrupted.
5. The browser stays open at the end (`--keep-open`) — press `CTRL+C` in the terminal to close it.

> **Important:** Do not interact with the browser window while the script is running.

---

## Command Arguments Explained

| Argument | Value Used | Description |
|---|---|---|
| `--excel` | Path to Excel file | Location of the test cases file |
| `--url` | `https://www.pixelssuite.com/chat-translator` | The URL of the application being tested |
| `--wait-ms` | `5000` | Time (ms) to wait for the Sinhala output to appear after each input (5 seconds) |
| `--type-delay-ms` | `80` | Delay (ms) between each keystroke when typing input (simulates human typing) |
| `--slow-mo-ms` | `200` | Adds a 200ms slow-motion delay between all browser actions |
| `--save-every` | `1` | Saves the Excel file after every single test case |
| `--keep-open` | _(flag)_ | Keeps the browser open after all tests finish |
| `--headless` | _(not used)_ | Add this flag to run without opening a visible browser window |

---

## Checking the Results

After the script finishes:

1. Open `Assignment 1 - Test cases.xlsx` from the `test_automation` folder.
2. Column E (`Actual output`) – Contains the text that the application actually produced.
3. Column F (`Status`) – Contains one of the following:
   - **FAIL** – The actual output did not match the expected output ✅ *(expected for all 50 negative test cases)*
   - **PASS** – The actual output matched the expected output *(replace this test case with a better one)*
   - **UI Error** – The script could not interact with the browser for that row *(check your internet connection and re-run)*
4. After verifying, manually complete **Column G** (`Singlish input types covered`) and **Column H** (`Evidence or rationale for the input type covered`) for each test case.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| `pip` is not recognized | Make sure Python is installed and added to PATH. Re-install Python and check "Add to PATH" during setup. |
| `playwright install` fails | Check your internet connection. Try running Command Prompt as Administrator. |
| Browser opens but nothing types | Close the Excel file before running the script. Make sure no other program is using it. |
| All rows show `UI Error` | The website may be down or slow. Try increasing `--wait-ms` to `8000`. |
| Script finishes but Excel is not updated | Make sure you closed the Excel file before running the script. |
| Output column shows previous test's result | Increase `--wait-ms` to allow more time for the page to update between tests. |

---

## Dependencies

| Package | Purpose |
|---|---|
| `playwright` | Browser automation – controls Chrome to type inputs and read outputs |
| `openpyxl` | Excel file read/write – reads test inputs and writes actual outputs and status |

---

## Notes

- This project tests only the **Chat Sinhala** transliteration function. The Standard Sinhala function, backend APIs, and performance/security testing are **out of scope**.
- All 50 test cases use TC IDs starting with `Neg_` since they are **negative test cases** (cases where the system is expected to fail).
- The Excel file covers all **24 Singlish input types** defined in Appendix 1 of the assignment, with at least 2 test cases per type.
- Input lengths are categorised as: **S** (≤ 30 characters), **M** (31–299 characters), **L** (300–450 characters).