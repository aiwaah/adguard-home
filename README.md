[README.md](https://github.com/user-attachments/files/24157884/README.md)
# Ultra-clean DNS Filter (AdGuard Home)

A **sanitized, deduplicated, production-grade DNS blocklist** in AdGuard/AdBlock filter syntax.

- **Target:** AdGuard Home (DNS filtering)
- **Format:** `||domain.tld^` (blocks the domain + subdomains)
- **Optional exceptions:** `@@||domain.tld^`

## Files

- `ultra-clean-dns-filter.txt` — the final filter list (ready to paste into AdGuard Home → *Filters*).

## What this list does

- Blocks ad/tracker/malware domains at the **DNS level** (works for all clients that use your DNS).
- **Deduplicated intelligently:** if `example.com` is blocked, redundant entries like `a.b.example.com` are removed.
- **Sanitized:** invalid/wildcard/path/regex rules that are unsafe or not DNS-friendly are removed.

## Upstream sources

This list is built by merging (then cleaning) the following upstream lists:

- https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_27.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_47.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_51.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_63.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_61.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_12.txt
- https://adguardteam.github.io/HostlistsRegistry/assets/filter_55.txt
- https://v.firebog.net/hosts/Admiral.txt
- https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/doh.txt

## Usage (AdGuard Home)

1. Open AdGuard Home Admin UI
2. Go to **Filters → DNS blocklists**
3. Add a new list and paste the raw URL of `ultra-clean-dns-filter.txt` from your GitHub repository
4. Update filters

## Notes / design choices

- This is a **DNS** list, so the safest stable rule type is `||domain^`.
- Wildcard patterns like `||foo-*.example.com^` are discarded (DNS filters don’t support arbitrary globbing safely).
- URL/path rules (`/path`, query rules) are discarded.
- If you add your own allowlist (banks/gov), use `@@||domain.tld^` — this allows the domain and all its subdomains.

## Build process (how to reproduce)

The build logic is:

1. Normalize & parse input lines (AdBlock + hosts formats)
2. Convert to a single canonical format
3. Validate domains (including IDN/punycode)
4. Remove invalid patterns
5. Deduplicate + remove subdomains covered by a parent domain
6. Output clean filter file

## Stats (this build)

- Block entries: 364491
- Allow entries: 11
- Build date: 2025-12-15

## License

Each upstream list has its own license/terms. This repository contains a merged derivative; please review upstream licenses before redistributing commercially.
