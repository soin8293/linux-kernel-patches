# Linux Kernel Stable Patches + CVE Database

This repository contains:

- **9,514 stable patches** from the official [kernel.org stable-queue](https://git.kernel.org/pub/scm/linux/kernel/git/stable/stable-queue.git) covering kernel branches: `5.10`, `5.15`, `6.1`, `6.6`, `6.12`, `6.18`, `7.3`
- **165 Linux kernel CVEs** extracted from the [NVD daily CSV export](https://github.com/flying-coyote/nvd-csv) (flying-coyote/nvd-csv)

## Structure

```
.
├── cves/
│   └── linux_kernel_cves_full.csv    # 165 Linux kernel CVEs (NVD schema)
├── patches/
│   ├── queue-5.10/                   # Patches queued for 5.10-stable
│   ├── queue-5.15/
│   ├── queue-6.1/
│   ├── queue-6.6/
│   ├── queue-6.12/
│   ├── queue-6.18/
│   ├── queue-7.3/
│   ├── staging-5.10/                 # Staging patches (not yet in queue)
│   ├── staging-5.15/
│   ├── staging-6.1/
│   ├── staging-6.6/
│   ├── staging-6.12/
│   ├── staging-6.18/
│   └── staging-7.3/
├── active_kernel_versions            # Currently supported kernel versions
└── README                            # Upstream stable-queue README
```

## Usage

### Apply patches for a specific kernel version

```bash
# Clone a kernel tree (e.g., 6.6.y)
git clone --branch linux-6.6.y https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
cd linux

# Apply all queued patches for 6.6
git am /path/to/linux-kernel-patches/patches/queue-6.6/*.patch
```

### CVE database (CSV)

The CSV follows the [NVD schema](https://github.com/flying-coyote/nvd-csv) with these key columns:

- `cve_id` — CVE identifier (e.g., `CVE-2026-99236`)
- `cvss_base_score` — CVSS 3.x base score
- `cvss_vector` — CVSS vector string
- `cwe_ids_all` — CWE IDs (e.g., `CWE-416|CWE-787`)
- `vendors` — Vendor names (e.g., `Linux`)
- `products` — Product names (e.g., `Linux Kernel`)
- `cpes` — CPE identifiers
- `cisa_kev` — Whether on CISA Known Exploited Vulnerabilities list
- `description_en` — English description

```python
import pandas as pd

df = pd.read_csv('cves/linux_kernel_cves_full.csv')

# Critical/High severity
critical = df[df['cvss_base_score'].astype(float) >= 7.0]

# Use-after-free bugs
uaf = df[df['cwe_ids_all'].str.contains('CWE-416', na=False)]

# CISA KEV entries
kev = df[df['cisa_kev'] == '1']
```

## Sources

- **Stable patches**: https://git.kernel.org/pub/scm/linux/kernel/git/stable/stable-queue.git
- **CVE data**: https://github.com/flying-coyote/nvd-csv (daily updated from NVD/CVE Project)

## License

MIT — see [LICENSE](LICENSE)

Patches are copyright their respective authors and licensed under GPL-2.0 (Linux kernel license). CVE data is public domain (NVD).
