# SSC Manifest Converter

A single-file browser tool that converts an Excel or CSV package list into an **ASYCUDA World AWBOLDS degroupage XML** file for submission to Jamaica Customs.

No installation, no server, no data leaves your machine.

---

## Usage

1. Open `Manifest-Converter-SSC.html` in any modern browser (Chrome, Edge, Firefox, or Safari).
2. **Step 1 – Package list**: Drop or select an `.xlsx` or `.csv` export of the packages on the flight. The tool auto-detects column headers; use the *Column matching* expander to fix any mismatches.
3. **Step 2 – Master air waybill**: Fill in the customs office code, flight number, departure date, MAWB number, and the UN/LOCODEs for origin and destination.
4. **Step 3 – House bill defaults**: Set your exporter/shipper details, consignee address template, waybill type, package type, and freight payment indicator. These are saved to `localStorage` so you only do it once.
5. **Step 4 – Review & export**: Check the validation panel, then download the XML or a sorted Excel copy of the manifest.

---

## Input file requirements

| Column | Auto-detected aliases |
|---|---|
| Tracking number | `tracking`, `tracking number`, `tracking no` |
| Pack code / HAWB | `pack code`, `package code`, `hawb`, `house waybill` |
| Courier | `courier`, `carrier` |
| Package courier | `package courier`, `client courier`, `company` |
| Description | `description`, `goods description`, `contents` |
| Customer name | `customer`, `customer name`, `consignee`, `name` |
| Account ID | `account id`, `account`, `account no` |
| Weight | `weight`, `weight (lbs)`, `weight lbs`, `gross weight` |

Column names are matched case-insensitively. Weight values can be in **lb** or **kg** (the tool converts automatically); values without a unit are treated as lb.

---

## House bill modes

| Mode | Behaviour |
|---|---|
| One per package | Each row in the spreadsheet becomes its own house bill. The pack code or tracking number is used as the bill reference. |
| One per customer | All packages for the same account/customer under the same package courier are grouped into a single house bill. |

---

## Output

### XML (`AWBOLDS_<flight>_<date>.xml`)

ASYCUDA World **Bill of Lading Degroupage Stream** format. To import it:

1. In ASYCUDA, open **Cargo Manifest → XML Integration → Degroupage**.
2. Choose the downloaded file, press **Validate**, then **Upload**.

> The airline must first authorise your TRN on the master waybill before the upload will succeed.

### Sorted Excel (`Sorted_Manifest_<flight>_<date>.xlsx`)

A reformatted copy of the manifest sorted by package courier → customer → weight (heaviest first), with a totals row and frozen header.

---

## Address placeholders

The consignee address field supports placeholders that are filled per bill at export time:

| Placeholder | Replaced with |
|---|---|
| `{Package Courier}` | The package courier for that bill |
| `{Customer}` | The customer name |
| `{Account ID}` | The customer's account ID |
| `{Courier}` | The courier column value |

Example: `C/O {Package Courier}, KINGSTON, JAMAICA`

---

## Browser support

Requires the [Compression Streams API](https://developer.mozilla.org/en-US/docs/Web/API/Compression_Streams_API) (`DecompressionStream`) to read `.xlsx` files. Supported in all current versions of Chrome, Edge, Firefox, and Safari. CSV files work in any browser.

---

## Privacy

The file is processed entirely in the browser. No data is sent to any server.
