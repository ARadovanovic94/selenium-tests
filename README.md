# Selenium Tests

A Python UI test project using Selenium WebDriver and pytest to practice browser automation on [Practice Test Automation](https://practicetestautomation.com/).

## Test coverage

- Successful login with valid credentials
- Login errors for an invalid username or password
- Adding a dynamically loaded input row
- Saving a value in the second row and checking the confirmation message
- Checking that instructions disappear after adding a row

The repository also contains small arithmetic tests used for pytest practice.

## Stack

Python, Selenium WebDriver, pytest, pytest-html and pytest-xdist.

## Project structure

```text
page_objects/       Page classes, locators and shared browser actions
tests/              Test scenarios and browser fixture
reports/            Example HTML report
pytest.ini          Registered test markers
requirements.txt    Project dependencies
```

Tests use the Page Object Model: page classes encapsulate locators and interactions, while test methods contain scenario steps and assertions. The browser fixture creates a new browser session for each test and closes it afterwards.

## Setup

Install Python 3 and Google Chrome.

```bash
git clone https://github.com/ARadovanovic94/selenium-tests.git
cd selenium-tests
python -m venv .venv
```

Activate the virtual environment:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

Install dependencies from the repository root:

```bash
python -m pip install -r requirements.txt
```

Use `python3` instead of `python` if that is how Python is installed on your system. Internet access is required to reach the practice website and, when needed, download a compatible browser driver through Selenium Manager.

## Run tests

Run commands from the repository root. Chrome is the default browser.

```bash
# Login scenarios
python -m pytest -m login -v

# Positive or negative login scenarios
python -m pytest -m "login and positive" -v
python -m pytest -m "login and negative" -v

# Dynamic-element scenarios
python -m pytest -m exceptions -v

# All tests
python -m pytest -v
```

Safari is also supported on macOS. Enable Safari's remote automation setting before running:

```bash
python -m pytest -m login --browser safari -v
```

The current fixture opens a visible browser window; it does not provide a headless option.

## Reports

Generate a new self-contained HTML report:

```bash
python -m pytest -m login --html=reports/report.html --self-contained-html
```

Open `reports/report.html` in a browser after the run. Committed reports are examples from earlier runs, not evidence of the current test results.

## Parallel execution

Run Chrome tests with two workers:

```bash
python -m pytest -m login --browser chrome -n 2
```

Each test uses its own browser session. Parallel browser runs require additional system resources.

## Current limitations

- `test_small_numbers` currently expects `30` for `1 + 2`, so the full test suite includes a failing arithmetic test.
- Two methods in `tests/test_exceptions.py` share the name `test_invalid_element_state_exception`. Python retains the later definition, so the earlier row-editing scenario is not collected.
- The timeout scenario currently repeats the add-row check.
- Implicit and explicit waits are both used; consolidating the waiting strategy would improve predictability.
- Tests depend on an external practice website and may be affected by changes or downtime.

The login credentials in the tests are public sample credentials provided by the practice website.
