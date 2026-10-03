# pdfmt-workflows - Community Workflows for pdfmt

<p align="center">
    <a href="https://github.com/eifelcode/pdfmt-workflows/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="MIT License"></a>
</p>

A collection of ready-to-use, community-maintained shell workflows for [pdfmt](https://github.com/eifelcode/pdfmt).

These workflows combine simple `pdfmt` commands and standard Unix utilities to solve real-world document automation tasks.

---

## Available Workflows

| Workflow                   | Description                                                           | Example Usage                                                              |
|:---------------------------|:----------------------------------------------------------------------|:---------------------------------------------------------------------------|
| `scan-duplex`              | Scans front/back pages, prompts to flip paper, merges into duplex PDF | `pdfmt workflow run scan-duplex adf output.pdf`                            |
| `scan-duplex-split-length` | Scans duplex and splits the result into fixed-length chunks           | `pdfmt workflow run scan-duplex-split-length adf 2 invoice_`               |
| `bulk-stamp-text`          | Stamps all PDFs in a directory with text and saves them with a suffix | `pdfmt workflow run bulk-stamp-text invoices/ "Paid 2026-08-28" " - paid"` |

---

## Installation

Workflows are stored in your local `pdfmt` workflow directory:

```text
~/.local/share/pdfmt/workflows/
```

### Install a single workflow
Ensure your local workflow directory exists, download the script, and make it executable:

```bash
# Name of the workflow to install (example: scan-duplex)
workflow_name=scan-duplex

# Download workflow and install
curl -sSL https://raw.githubusercontent.com/eifelcode/pdfmt-workflows/main/workflows/$workflow_name -o ~/.local/share/pdfmt/workflows/$workflow_name
chmod +x ~/.local/share/pdfmt/workflows/$workflow_name
```

### Install all workflows

Clone this repository and copy all scripts into your workflow directory:

```bash
git clone [https://github.com/eifelcode/pdfmt-workflows.git](https://github.com/eifelcode/pdfmt-workflows.git)
mkdir -p ~/.local/share/pdfmt/workflows
cp pdfmt-workflows/workflows/* ~/.local/share/pdfmt/workflows/
chmod +x ~/.local/share/pdfmt/workflows/*
```

---

### Managing Workflows

Once installed, you can list and run workflows directly using `pdfmt`:

```bash
# List all available installed workflows
pdfmt workflow list

# Inspect detailed info and usage of a specific workflow
pdfmt workflow info scan-duplex

# Execute a workflow
pdfmt workflow run scan-duplex adf output.pdf
```

---

## Contributing

New workflows are always welcome! If you have built a script that automates a common PDF processing task, consider contributing it.

---

### Workflow Guidelines

To maintain consistency across all community workflows, please follow these rules:

- **Shebang & Description**: The first line must be `#!/usr/bin/env bash`. The third line must contain a short description starting with `# ` (this is parsed by `pdfmt workflow` commands).
- **Comment Header**: Include a structured block detailing usage, options, dependencies, and steps.
- **Safety First**: Enable strict mode with `set -euo pipefail`.
- **Structure**: Wrap execution in a `main()` function and use `local -r` for immutable local variables.
- **No Globals**: Do not pollute the environment with global variables.
- **Error Handling**: Validate parameters, preserve temporary files on failure, and return non-zero exit codes when errors occur.

--- 

### Example Template

```bash
#!/usr/bin/env bash
# =====================================================================================================================
# Short one-line summary for pdfmt workflow list.
# Usage:
#   my-workflow <arg1> <arg2>
#
# Examples:
#   my-workflow input.pdf output.pdf
#
# Workflow steps:
#   - Step 1 description
#   - Step 2 description
# =====================================================================================================================
set -euo pipefail

function main()
{
    local -r input="${1:-}"
    local -r output="${2:-}"

    if [[ -z "$input" || -z "$output" ]]; then
        echo "ERROR: Missing required arguments." >&2
        echo "Usage: my-workflow <arg1> <arg2>" >&2
        exit 1
    fi

    # Workflow logic here...
}

main "$@"
```

---

## License

All workflows in this repository are licensed under the **MIT License**. See `LICENSE` for details.
