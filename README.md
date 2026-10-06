## Production Release

This package contains the production build of **R3 Payment Reports**.

The application automates acquisition, processing, reconciliation, and reporting of payment activity from supported banking systems and WoundExpert.

## Included Components

The production installer includes the components required for normal operation:

- R3 Payment Reports UI
- R3 Payment Reports Worker
- Tray Agent
- Folder Picker Helper
- OCR Broker
- Required Playwright/Chromium runtime files
- Production payer configuration
- Production reference configuration

Development-only tools, debugging symbols, and developer-specific credentials are not intended to be included in the production package.

## Application Data Locations

R3 Payment Reports uses Windows-known folders rather than hardcoded user paths.

### User Settings and Credentials

```text
%APPDATA%\R3PaymentReports
```

Contains user-specific application data including:

```text
settings.json
credentials\
    credentials_chase.dat
    credentials_enterprisebanktrust.dat
    credentials_woundexpert.dat
```

Credential files are user-specific protected data and must not be copied from one Windows user to another.

Existing settings and credentials are preserved during normal application upgrades.

### Local Application Data

```text
%LOCALAPPDATA%\R3PaymentReports
```

Used for machine/user-specific runtime data such as:

- Logs
- Run workspaces
- Diagnostics
- Browser profiles
- Playwright browser files
- Bank download data
- Support bundles

### Shared Configuration

```text
%PROGRAMDATA%\R3PaymentReports\Configuration
```

Contains application-wide configuration including:

```text
Payers.json
reference-config.json
```

Existing configuration is preserved according to the application's upgrade and configuration-migration rules.

## Working Folder

The default user-facing working location is based on the current Windows user's **Documents** known folder.

For example:

```text
<Documents>\R3PaymentReports
```

The application resolves the Windows Documents location dynamically and must not assume:

```text
C:\Users\<username>\Documents
```

because Documents may be redirected to OneDrive or another location.

The master workbook is normally located beneath:

```text
<Documents>\R3PaymentReports\MasterLog\R3 H Expert Payment Logsheet.xlsx
```

unless the user has configured another location.

## Production Behavior

Release/Production builds are compiled with the production build boundary enabled.

Production builds:

- Disable development-only UI and behavior.
- Use production-safe browser automation settings.
- Use the installed OCR gateway/broker path for production OCR.
- Do not depend on developer Google credentials.
- Do not depend on source-repository paths.
- Do not require Visual Studio or the .NET SDK on the end user's machine.
- Preserve unresolved or ambiguous payment information for review rather than making unsafe assumptions.

## OCR

Production Remote Deposit OCR uses the installed OCR broker/gateway configuration.

The installer handles initial OCR enrollment and preserves a valid broker credential during supported upgrades.

Developer ADC credentials and developer OCR overrides are not part of the production package.

## Upgrade Behavior

A normal upgrade should preserve existing user data, including:

- Application settings
- Chase credentials
- Enterprise Bank & Trust credentials
- WoundExpert credentials
- Enabled banks
- Schedules
- Working-folder configuration
- Master-workbook configuration
- Persistent browser/session data where applicable
- Previously downloaded bank data
- User-generated reports and workbook data
- Production configuration under ProgramData
- Valid OCR broker credentials

An upgrade must not replace existing user settings with installer defaults.

## Installation

Run the production installer:

```text
R3PaymentReports-Setup-<version>.exe
```

The installer requires administrative privileges for the supported production installation.

The standard installation directory is:

```text
C:\Program Files\R3PaymentReports
```

The installer also provisions the required per-user and shared application directories.

## Production Verification

Before publishing a release, verify:

- Full Release build succeeds.
- Production-relevant automated tests pass.
- Production package verification passes.
- Installer version matches the intended release.
- Installed binaries report the correct version.
- No `.pdb` or development-only artifacts are packaged.
- No personal credentials are packaged.
- No developer-specific paths are embedded as runtime dependencies.
- `Payers.json` and `reference-config.json` are available from the expected ProgramData location.
- Settings and credential paths resolve to the installed Windows user.
- OCR broker setup succeeds.
- Clean installation succeeds.
- Upgrade from the previous production version preserves existing user data.

## Release Metadata

Each production release should include:

- Version number
- Release date
- Installer filename
- Installer SHA-256
- GitHub release URL
- Installer download URL
- Release notes
- Updated `version.json`
- Matching Firebase update manifest

The SHA-256 value must always be calculated from the exact installer that is published.

## Support and Diagnostics

Operational logs and diagnostic artifacts are stored beneath the R3 Payment Reports application-data folders.

When troubleshooting a production issue, preserve the affected run's:

- Run status
- Logs
- Run manifest
- Diagnostics
- Relevant workbook snapshot
- Remote Deposit artifacts, when applicable

Do not include credential files in support packages.
