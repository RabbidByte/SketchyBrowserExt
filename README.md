# Suspicious Browser Extensions: A Personal Watchlist

A list of **7,569 browser extension IDs**, with a name for most of them, that I consider suspicious or worth a closer look. It's compiled from publicly available research and my own opinions. Use it as a starting point to review the browsers on your machines, not as a verdict on any extension or its developer.

> **Please read the [Disclaimer](#disclaimer) before using or sharing this list.**

| | |
|---|---|
| **File** | [Malicious_or_Shady_BrowserExtensions.csv](Malicious_or_Shady_BrowserExtensions.csv) |
| **Entries** | 7,569 (all unique IDs) |
| **Names** | 7,328 entries have a name; 241 are blank |
| **Format** | CSV, UTF-8, with four columns: `ID`, `Name`, `Browser` and `Source` |
| **ID style** | 32-character Chromium-style extension IDs (`a`–`p`) |
| **Base list** | [The-Privacy-Commons-Institute/chrome-mal-ids](https://github.com/The-Privacy-Commons-Institute/chrome-mal-ids), plus my own additions |

## Disclaimer

**This list is my opinion. It is not a statement of fact.**

- **Inclusion is not an accusation.** An extension appearing here does **not** mean it is malware, that it is illegal, or that its developer acted in bad faith or did anything wrong. Many entries may be legitimate, and some may be false positives. I am not alleging wrongdoing by any person or company.
- **How the list was made.** It started from the [chrome-mal-ids](https://github.com/The-Privacy-Commons-Institute/chrome-mal-ids) list, which I then added to. It is based on research and reporting that others have published on the internet, plus my own subjective judgment about what looks suspicious, intrusive or unwanted. I haven't independently verified every entry, and I haven't audited the code of the listed extensions.
- **It may be wrong or out of date.** Extensions change hands, get updated, get fixed, or get removed. A listing reflects my view at some point in time and may no longer be accurate.
- **Do your own research.** Verify an extension yourself before you rely on this list to remove, block or report it, or to make any security, business or legal decision. Don't treat the list as a substitute for professional security advice.
- **No warranty and no liability.** The list is provided "as is", with no warranty of any kind, express or implied, including accuracy, completeness or fitness for a particular purpose. To the fullest extent permitted by law, I accept no liability for any loss or damage arising from its use or from reliance on it.
- **Names and trademarks.** Extension names are used only to identify the listed IDs. They belong to their respective owners. I have no affiliation with, and no endorsement from, any of them.
- **Corrections and removals.** If you own or develop a listed extension and believe it shouldn't be here, or the information is wrong, contact me at **[add contact method]** with the extension ID. I will review the request promptly and remove or correct the entry where appropriate.

## Data format

```csv
ID,Name,Browser,Source
acmnokigkgihogfbeooklgemindnbine,,Chrome,https://awakesecurity.com/wp-content/uploads/2020/06/GalComm-Malicious-Chrome-Extensions-Appendix-B.txt
aljmdjbcbkanlhnmcdjbefaomgbekhno,Color by Number,Edge,https://microsoftedge.github.io/edgevr/posts/Inside-StegoAd-How-We-Disrupted-a-Massive-Malicious-Extension-Campaign/
...
```

- The file has a header row, so skip the first line when parsing.
- `Name` is the extension's display name and may be empty. Names that contain commas are quoted, so use a real CSV parser for the `Name` column. The `ID` column always comes first and never contains commas.
- `Browser` is the browser the extension targets: `Chrome`, `Edge`, or `"Chrome, Edge"` when the same ID targets both. The column also allows `Firefox`, but no entry uses it yet.
- `Source` is the URL where the entry was reported, as recorded in the base list's `SOURCE` field. Entries with no recorded source point to the base list itself. Many entries come from the [malicious_extension_sentry](https://github.com/toborrm9/malicious_extension_sentry) feed, which monitors store removals and security blogs, so a source there may be an aggregator and not the original research.
- Names aren't unique. Several different IDs are called "Search Manager", for example, so always match on `ID`.
- Line endings are CRLF. Strip `\r` when using it with Unix tools.
- Every ID is a valid Chromium-style extension ID, so it works for Chrome, Edge, Brave, Opera, Vivaldi and other Chromium browsers. Edge entries use IDs from the Edge Add-ons store and may differ from the Chrome Web Store.

## Usage

### Look up an ID

An extension's Chrome Web Store page is `https://chromewebstore.google.com/detail/<ID>`. Edge entries live on the Edge Add-ons store instead. Removed extensions may return a 404.

### Audit locally installed extensions

Check the extensions installed in a Chromium-based browser profile against the list (Linux, Google Chrome shown):

```bash
# Installed extension IDs are the folder names under Extensions/
ls ~/.config/google-chrome/*/Extensions/ \
  | grep -E '^[a-p]{32}$' | sort -u > /tmp/installed.txt

# Extract the ID column (drop header and CRLF)
tail -n +2 Malicious_or_Shady_BrowserExtensions.csv | tr -d '\r' | cut -d, -f1 | sort -u > /tmp/bad.txt

# Show matches
comm -12 /tmp/installed.txt /tmp/bad.txt
```

To print the names of any matches:

```python
import csv

hits = set(open('/tmp/installed.txt').read().split()) & set(open('/tmp/bad.txt').read().split())
with open('Malicious_or_Shady_BrowserExtensions.csv', encoding='utf-8', newline='') as f:
    for row in csv.DictReader(f):
        if row['ID'] in hits:
            print(row['ID'], row['Name'] or '(no name)', row['Browser'], row['Source'])
```

Other browsers use different profile paths, for example:

| Browser | Linux path |
|---|---|
| Chromium | `~/.config/chromium/*/Extensions/` |
| Brave | `~/.config/BraveSoftware/Brave-Browser/*/Extensions/` |
| Edge | `~/.config/microsoft-edge/*/Extensions/` |

On macOS and Windows the profile directories differ, but the folder-name-is-the-ID convention is the same.

## Caveats

- **Little context.** An ID and a name don't tell you *why* something was flagged. The `Source` column links to where each entry was reported, but there are no dates or severity ratings, and 241 entries have no name at all. Investigate a hit before you act on it, and check the [Disclaimer](#disclaimer).
- **"Suspicious" is a judgment call.** It reflects opinion, not proof. The list may include extensions that are perfectly legitimate, and some entries may be false positives.
- **Extensions change.** A clean extension can be sold or updated into a malicious one, and a listed extension may have been cleaned up or removed from the store.
- **The list isn't exhaustive.** An ID not being here doesn't mean an extension is safe.
