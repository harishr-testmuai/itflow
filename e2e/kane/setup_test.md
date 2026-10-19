---
mode: testing
url: http://localhost:4910/
max_steps: 60
---

# Setup wizard

## Checks and database
Click "Begin Setup".
Verify the "Setup Checks" step shows "php-mysqli installed" as Installed, then click "Next (Database)".
Type "itflow" as the Database Name, "itflow-db" as the Database Host, "itflow" as the Database User and "itflow-e2e" as the Database Password, then click "Next (First User)".
Verify the "Create your first user" step is shown.

## First user and company
Type "Alex Morgan" as the Name, "alex@example.com" as the Email and "ITFlow-Test-2026" as the Password, then click "Next (Company details)".
Type "Morgan IT Services" as the Company Name, select "United States" as the Country, then click "Next (Localization)".
Verify the "Localization" step is shown.

## Localization and finish
Select "English (United States)" as the language, "USD - United States Dollar" as the currency and "America/New_York" as the timezone, then click "Next (Telemetry Settings)".
Leave "Share" unchecked and click "Finish and Sign in".

## Login is HTTPS-only by default
Go to http://localhost:4910/login.php and wait for the page to load.
Verify the page text contains "Login is restricted as ITFlow defaults to HTTPS-only".
