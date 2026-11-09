# bruteforce-lists

A collection of wordlists for content discovery, forced browsing and directory brute-forcing during authorised security testing. Most entries are full URL paths (starting with `/`), suitable for tools such as ffuf, gobuster, dirsearch and ffuf-style fuzzers; others are bare filenames for host-path enumeration. Big thanks to [GreyNoise](https://greynoise.io/) for the intel!

## Contents

| File | Lines | Targets |
|---|---|---|
| `combined_directories.txt` | 1,377,725 | Large merged directory/file list for general content discovery |
| `xml.txt` | 3,811,361 | XML files (`.xml`, feeds, sitemaps, config XML) |
| `sql.txt` | 520,037 | SQL dumps and SQLite databases (`.sql`, `.sqlite3`) |
| `zips.txt` | 295,815 | ZIP archives, including backup-style names |
| `ini.txt` | 174,743 | INI and configuration-style files |
| `exe.txt` | 117,165 | Windows executables (`.exe`) |
| `pods.txt` | 89,100 | Kubernetes/OpenShift pod names |
| `dll.txt` | 77,156 | Windows DLLs (`.dll`) |
| `aspx.txt` | 70,851 | ASP.NET `.aspx` pages |
| `needstobefixed.txt` | 59,712 | Assorted paths awaiting curation |
| `xlsx.txt` | 36,132 | Excel spreadsheets (`.xls`/`.xlsx`) |
| `larv.txt` | 30,134 | Laravel application paths (migrations, routes, etc.) |
| `asp.txt` | 20,239 | Classic ASP (`.asp`) pages |
| `ashx.txt` | 19,252 | ASP.NET `.ashx` handlers |
| `pdfs.txt` | 16,646 | PDF files |
| `php.txt` | 12,924 | PHP files (config, admin, misc scripts) |
| `rar.txt` | 10,420 | RAR archives |
| `old.txt` | 9,897 | Backup/old file variants (`.OLD`, `.BAK` suffixes) |
| `windows-lfi.txt` | 6,610 | Windows file paths for local file inclusion (`$Recycle.Bin`, `AppxLogs`, `web.config`, etc.) |
| `cs.txt` | 4,678 | C# source files (`.cs`) |
| `log.txt` | 4,628 | Log files (`.log`) |
| `params.txt` | 2,583 | Common GET parameter names |
| `wp-content.txt` | 2,155 | WordPress `wp-content` paths |
| `asmx.txt` | 2,040 | ASP.NET `.asmx` web services |
| `honey.txt` | 1,786 | Paths observed being hit by internet scanners (honeypot/GreyNoise-derived) |
| `reallysafe-not.txt` | 1,674 | Paths commonly probed by scanners — "really safe" only if you do not expose them |
| `svc.txt` | 1,672 | `.svc` WCF service files |
| `htm.txt` | 1,524 | `.htm`/`.html` pages |
| `env.txt` | 1,396 | `.env` files and environment variable exposures, including traversal variants |
| `adminer.txt` | 1,369 | Adminer installations and versioned releases |
| `dotfiles.txt` | 1,344 | Dotfiles and dot-directories (`.git`, `.env`, `.ssh`, etc.) |
| `perl-files.txt` | 1,236 | Perl scripts (`.pl`) |
| `api.txt` | 1,231 | API endpoints and documentation paths |
| `cgi-files.txt` | 1,192 | CGI scripts |
| `cfm.txt` | 1,006 | ColdFusion (`.cfm`) files |
| `js.txt` | 986 | JavaScript files |
| `phpunit.txt` | 625 | Exposed `phpunit/src/Util/PHP/eval-stdin.php` paths (CVE-2017-9841) |
| `wordpress-random.txt` | 494 | WordPress plugin paths including known exploitable query strings |
| `ppk.txt` | 263 | PuTTY private keys (`.ppk`) |
| `swagger.txt` | 207 | Swagger/OpenAPI documentation endpoints |
| `phpmyadmin.txt` | 199 | phpMyAdmin installation paths |
| `iso.txt` | 185 | ISO images |
| `bitnami.txt` | 127 | Bitnami stack credentials and config files |
| `joomla.txt` | 118 | Joomla component LFI payloads |
| `yaml.txt` | 113 | YAML config files (CI pipelines, site configs) |
| `cgi-bin.txt` | 90 | `/cgi-bin/` scripts |
| `webconfig.txt` | 89 | `web.config` variants |
| `web-inf.txt` | 77 | Java `WEB-INF` deployment descriptors |
| `wad.txt` | 73 | Doom WAD files |
| `k8s.txt` | 62 | Kubernetes secrets (`ca.pem`, `token_auth.csv`, `apiserver.pem`) |
| `peoplesoft.txt` | 58 | PeopleSoft environment names |
| `npmrc.txt` | 51 | `.npmrc` files (may contain registry tokens) |
| `git_config.txt` | 49 | `.git/config` paths |
| `ws-dirs.txt` | 48 | Web service directories |
| `config.txt` | 48 | Configuration files |
| `rpt.txt` | 42 | Crystal Reports (`.rpt`) files |
| `apk.txt` | 40 | Android APKs |
| `properties-files.txt` | 39 | Java `.properties` files |
| `workflows.txt` | 29 | GitHub Actions workflow files |
| `pem.txt` | 28 | PEM keys and certificates |
| `ec2.txt` | 22 | EC2 instance metadata paths (`/var/lib/cloud/...`) |
| `csproj.txt` | 26 | Visual Studio project files |
| `keys.txt` | 7 | Kubernetes API server keys |
| `7z.txt` | 3 | 7-Zip archives |
| `asax.txt` | 2 | `Global.asax` files |

## Format

- Path lists (the majority, e.g. `api.txt`, `honey.txt`, `phpunit.txt`) contain one full URL path per line, beginning with `/`. Pass them as the wordlist to a fuzzing tool that prefixes a base URL.
- Filename lists (e.g. `exe.txt`, `dll.txt`, `ini.txt`, `pods.txt`) contain bare names without a leading slash, for enumerating files on a host path.
- `params.txt` contains parameter names for query-string fuzzing.
- Some lists include traversal variants (`../`) and URL-encoded payloads for path traversal testing.

## Usage examples

ffuf — general directory discovery:

```bash
ffuf -u https://example.com/FUZZ -w combined_directories.txt -mc 200,301,302,403
```

ffuf — hunting exposed `.env` files:

```bash
ffuf -u https://example.com/FUZZ -w env.txt -mc 200
```

gobuster — API endpoint discovery:

```bash
gobuster dir -u https://example.com -w api.txt -t 50
```

dirsearch — phpMyAdmin and admin panels:

```bash
dirsearch -u https://example.com -w phpmyadmin.txt
```

hydra-style tools are not the intended use here (these are not password lists) — these lists are for URL/content brute-forcing. If fuzzing WordPress, `wp-content.txt` and `wordpress-random.txt` work as dirsearch/ffuf wordlists:

```bash
ffuf -u https://example.com/FUZZ -w wp-content.txt -mc 200,403
```

Run from a VPS for scale — [DigitalOcean](https://www.digitalocean.com/?refcode=e22bbff5f6f1&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge) offers $200 free credit for 60 days.

## How the lists were built

The lists were accumulated from real-world observation rather than generated: many entries correspond to paths seen being scanned or probed on the internet (with thanks to [GreyNoise](https://greynoise.io/) for the intelligence behind lists such as `honey.txt` and `reallysafe-not.txt`), while others were collected from bug bounty research, public exploit disclosures (for example `phpunit.txt` targets the known PHPUnit CVE-2017-9841 eval-stdin exposure) and file-name harvesting. The git history shows over 200 commits of ongoing additions and updates to individual lists.

## Disclaimer / Authorised use

These lists are provided for authorised security testing, bug bounty work within programme scope, and responsible disclosure only. Do not use them against systems you do not have permission to test.

## Author

Robbie Wiggins — [What-Security](https://www.what-security.co.uk) · [GitHub](https://github.com/random-robbie)

If a nice bounty comes out of this, buy me a pint: [paypal.me/txt3rob](https://paypal.me/txt3rob)
