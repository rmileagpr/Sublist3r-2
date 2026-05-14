# Sublist3r v3.0

Fast subdomain enumeration tool for penetration testers and bug bounty hunters.

Enumerates subdomains using search engines (Google, Yahoo, Bing, Baidu, Ask) and passive sources (Netcraft, VirusTotal, DNSdumpster, SSL Certificates via crt.sh, BufferOverRun, PassiveDNS, CertSpotter). Includes integrated DNS brute-forcing via SubBrute and TCP port scanning.

## Requirements

- Python 3.6+
- Dependencies: `requests`, `dnspython`, `colorama`

## Installation

```
git clone https://github.com/aboul3la/Sublist3r.git
cd Sublist3r
pip install -r requirements.txt
```

Optional: set `VT_API_KEY` environment variable for unlimited VirusTotal lookups.

## Usage

```
python sublist3r.py -d example.com
```

### Options

| Flag | Long | Description | Default |
|------|------|-------------|---------|
| `-d` | `--domain` | Target domain (required) | |
| `-b` | `--bruteforce` | Enable SubBrute brute-force module | off |
| `-p` | `--ports` | Scan subdomains against TCP ports (comma-separated) | |
| `-v` | `--verbose` | Show results in real time | off |
| `-t` | `--threads` | Thread count for brute-force | 30 (max 64) |
| `-e` | `--engines` | Comma-separated engine list | all |
| `-o` | `--output` | Save results to text file | |
| `-j` | `--json` | Save results to JSON file | off |
| `-n` | `--no-color` | Disable colored output | off |

### Available Engines

`google`, `bing`, `yahoo`, `ask`, `baidu`, `netcraft`, `dnsdumpster`, `virustotal`, `crt`, `bufferover`, `passivedns`, `certspotter`

### Examples

```bash
# Basic enumeration
python sublist3r.py -d example.com

# Verbose with port scan
python sublist3r.py -v -d example.com -p 80,443

# Brute-force with JSON output
python sublist3r.py -b -d example.com -j -o results.txt

# Specific engines only
python sublist3r.py -e google,crt,virustotal -d example.com
```

## Using as a Module

```python
import sublist3r

subdomains = sublist3r.main(
    domain='example.com',
    threads=40,
    savefile='results.txt',
    ports=None,
    silent=False,
    verbose=False,
    enable_bruteforce=False,
    engines=None,
    json_output=False
)
```

## Subdomain Takeover Detection

The included `takeover.py` checks discovered subdomains for dangling CNAMEs and HTTP fingerprints indicating potential subdomain takeover vulnerabilities.

```bash
# From a file of subdomains
python takeover.py -i subdomains.txt -o results.txt -t 20

# Quick check against a domain
python takeover.py -d example.com -v
```

Supported services: GitHub Pages, Heroku, AWS S3, Shopify, Canny.

## Security Notes

- Output file paths are restricted to the current working directory (path traversal protection).
- SSL/TLS warnings are not suppressed during enumeration — MITM indicators remain visible.
- Thread and process counts are capped at 64 to prevent resource exhaustion.
- `takeover.py` intentionally disables certificate verification when probing dangling domains; warnings are scoped narrowly.

## Credits

- [Ahmed Aboul-Ela](https://twitter.com/aboul3la) — original author
- [TheRook](https://github.com/TheRook) — SubBrute module
- [Bitquark](https://github.com/bitquark) — SubBrute wordlist (dnspop research)
- [Shaheer Yasir](https://github.com/shaheeryasir) — v3.0 enhancements

## License

GNU GPL v2. See [LICENSE](LICENSE).
