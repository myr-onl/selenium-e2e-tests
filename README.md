# Selenium IDE Tests for Aspen Discovery

This directory contains Selenium IDE tests for Aspen Discovery.

## Directory Structure

- `selenium-ide/` - Contains the original test files with variables
	- `sites/` - Contains site-specific configuration files and processed test files
		- `example/` - Example site directory (included in git)
			- `conf/` - Example configuration files for the example site (included in git)
				- `example-test-config.json` - Example configuration file (included in git)
		- `[site-name]/` - Site-specific directories (not included in git)
			- `conf/` - Site-specific configuration files (not included in git)
				- `[site-name]-test-config.json` - Site-specific configuration file (not included in git)

## First-time Setup

1. Make sure you have Node.js installed on your system. (Download from [nodejs.org](https://nodejs.org/en/download); if
   you're a testing librarian, you probably do NOT want the Docker version... scroll down until you see "get a prebuilt
   Node.js® for...")
1. Make sure you have a browser with the Selenium IDE extension installed. The two browsers that currently work with the
   Selenium IDE Add-on are Edge and
   FIrefox. [Microsoft Edge Add-ons](https://microsoftedge.microsoft.com/addons/detail/selenium-ide/ajdpfmkffanmkhejnopjppegokpogffp), [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/)
1. Create a site-specific directory at `aspen-discovery/tests/e2e/selenium-ide/sites/[site-name]` with your site name (
   e.g., "nashville.production").
1. Create a `conf` directory within your [site-name] directory:
1. Copy the example site's conf file
   `aspen-discovery/tests/e2e/selenium-ide/sites/example/conf/example-test-config.json` into your [site-name] `conf`
   directory
1. Change the new conf file to `[site-name]-test-config.json` (e.g., "nashville-test-config.json").
1. Edit the configuration file to set the values for your site. See the "Configuration File" section below for details.

## Configuration File

- `siteName` - A unique name for your site (e.g., "nashville.production")
- `url` - The URL of your Aspen Discovery instance
- `patron` - Test patron with hold and borrow privileges
  - `username` - Patron username
  - `password` - Patron password
  - `invalidPassword` - Invalid password for the patron
- `holdBook` - A grouped work with a book with a bibliographic record in your ILS that can be placed on hold
    `title` - Title of the book to hold, exactly as it appears in Aspen Discovery
    `groupedWorkId` - The ID of the grouped work in Aspen Discovery
    `bibRecordId` - The ID of the bibliographic record in your ILS

## Running the Tests

### Step 1: Prepare the Test File

Before running the tests in Selenium IDE, you need to prepare the test file by replacing variables with values from your
configuration:

#### Option 1: Using the batch file (Windows)

Simply double-click the `prepare-selenium-tests.bat` file in Windows Explorer.

#### Option 2: Using the shell script (Linux/macOS)

Make the script executable and run it:

```
chmod +x prepare-selenium-tests.sh
./prepare-selenium-tests.sh
```

#### Option 3: Using Node.js directly

```
node prepare-selenium-tests.js
```

Any of these methods will:

1. Find all site configuration files in `selenium-ide/sites/[siteName]/conf/[siteName]-test-config.json`
2. Create site-specific directories based on the site names if they don't exist
3. Process the test file for each site by replacing variables with values from the configurations
4. Save the processed files to `selenium-ide/sites/[siteName]/`

### Step 2: Open the Processed File in Selenium IDE

1. Install the Selenium IDE extension for Chrome.
2. Open Selenium IDE in Chrome.
3. Click "Open an existing project".
4. Navigate to and select the processed file from your site directory:
   `selenium-ide/sites/[siteName]/AspenDiscovery.processed.side`.

**Important:** Always open the processed file in your site directory, not the original file in the `test-templates`
directory. The original file contains variables that need to be replaced before running the tests.

**Note:** Each site has its own directory under `selenium-ide/sites/` where processed test files are stored. This
ensures that site-specific test files don't accidentally get committed to the shared repository. Only the `example` site
directory is included in git.

### Troubleshooting

If you see an error like this when trying to open the file in Selenium IDE:

```
200: filename content-length last-modified file-type
201: META-INF/ 0 Tue,%2001%20Jan%201980%2006:00:00%20GMT DIRECTORY
201: assets/ 0 Tue,%2001%20Jan%201980%2006:00:00%20GMT DIRECTORY
...
```

It means you're trying to open the original file with variables instead of the processed file. Make sure to run
`node prepare-selenium-tests.js` first and then open the processed file.

## Updating the Tests

// TO DO: Add instructions for updating tests

## Adding a New Site

To add a new site:

1. Create a new directory in `selenium-ide/sites/` with your site name:
   ```
   mkdir selenium-ide/sites/[site-name]
   ```
2. Create a configuration directory within your site directory:
   ```
   mkdir selenium-ide/sites/[site-name]/conf
   ```
3. Create a configuration file named `[site-name]-test-config.json` in the new `conf/` directory:
   ```
   cp selenium-ide/sites/example/conf/example-test-config.json selenium-ide/sites/[site-name]/conf/[site-name]-test-config.json
   ```
3. Edit the configuration file to set your site-specific values.
4. Follow the steps in the "Running the Tests" section to prepare and run the tests for your new site.
