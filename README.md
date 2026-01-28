# LLM Security Scan GitHub Action

A GitHub Action that runs **trusys-llm-scan** in your CI to find LLM (Large Language Model) security issues in your code—prompt injection, unsafe model usage, data exposure, and framework-specific risks. Results can be reported as SARIF (for GitHub Security tab), JSON, or console output.

---

## What this action does

1. **Checks out your repo** and sets up Python.
2. **Installs** `trusys-llm-scan` and Semgrep from PyPI.
3. **Runs the scan** on the paths you specify (default: whole repo).
4. **Optionally** uploads SARIF so findings show in the repository **Security** tab.
5. **Optionally** runs AI-based filtering or uploads results to your backend.

No config is required for a basic scan; add inputs only when you need custom paths, severity filters, AI, or upload.

**Requirements:** None in your repo. The action installs Python (via `actions/setup-python`) and the scanner (`trusys-llm-scan`) from PyPI. Your code only needs to be in a GitHub repository.

---

## How to use the action (quick start)

### 1. Add a workflow file

In your repository, create or edit a file under `.github/workflows/`. For example:

- **File:** `.github/workflows/llm-security-scan.yml`

### 2. Minimum workflow (scan on push and pull requests)

Copy this into that file:

```yaml
name: LLM Security Scan

on: [push, pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run LLM Security Scan
        uses: spydra-tech/trusys-llm-security-scan-action@v1
```

That’s it. Every push and pull request will run the scanner. By default it:

- Scans the whole repository (`.`)
- Outputs in **SARIF** and uploads to GitHub so findings appear under **Security → Code scanning**
- Uses Python 3.9

**Where do I see results?**  
With the default `format: 'sarif'`, the action uploads results to GitHub. Open your repo on GitHub → **Security** → **Code scanning** to see the findings. You can also read the job log for the full scan output.

### 3. Optional: pin to a specific version

- `@v1` — use the latest v1.x (recommended for stability).
- `@v1.0.0` — use an exact version.
- `@main` — use the latest from the default branch (may change without notice).

---

## Features

- 🔍 **Comprehensive Scanning**: Detects LLM-specific vulnerabilities across multiple frameworks (OpenAI, Anthropic, Cohere, Langchain, LlamaIndex, Hugging Face, Azure, AWS Bedrock)
- 🤖 **AI-Powered Analysis**: Optional AI filtering to reduce false positives and enhance remediation guidance
- 📊 **Multiple Output Formats**: Supports console, JSON, and SARIF formats
- 🔗 **Backend Integration**: Optional upload of results to your backend API
- ⚡ **Fast & Reliable**: Built on Semgrep for efficient static analysis

---

## Common configurations

| What you want | Add this under `with:` |
|---------------|------------------------|
| Scan only some folders | `paths: 'src,lib'` |
| Only high/critical findings | `severity: 'critical,high'` |
| Ignore tests and deps | `exclude: '**/test/**,**/node_modules/**'` |
| Different Python | `python-version: '3.11'` |
| JSON instead of SARIF | `format: 'json'` |
| AI false-positive filtering | `enable-ai-filter: 'true'` and `ai-api-key: ${{ secrets.OPENAI_API_KEY }}` |
| Send results to your backend | `upload-endpoint: 'https://...'`, `application-id: '...'`, `api-key: ${{ secrets.API_KEY }}` |

Full list of inputs is in the **Inputs** table below.

---

## Usage examples

### Basic (defaults: whole repo, SARIF, Security tab)

```yaml
name: LLM Security Scan

on: [push, pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run LLM Security Scan
        uses: spydra-tech/trusys-llm-security-scan-action@v1
```

### Paths, severity, and exclusions

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    paths: 'src,lib'
    severity: 'high,critical'
    exclude: '**/test/**,**/node_modules/**'
    format: 'sarif'
```

### With AI-powered false-positive filtering

Requires an API key in GitHub Secrets (e.g. `OPENAI_API_KEY` or `ANTHROPIC_API_KEY`).

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    paths: 'src,lib'
    severity: 'high,critical'
    exclude: '**/test/**,**/node_modules/**'
    format: 'sarif'
    enable-ai-filter: 'true'
    ai-provider: 'openai'
    ai-model: 'gpt-4'
    ai-api-key: ${{ secrets.OPENAI_API_KEY }}
    ai-confidence-threshold: '0.8'
    ai-max-findings: '100'
```

### With backend upload

```yaml
name: LLM Security Scan

on: [push, pull_request]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Run LLM Security Scan and Upload
        uses: spydra-tech/trusys-llm-security-scan-action@v1
        with:
          format: 'sarif'
          upload-endpoint: 'https://api.example.com/scans'
          application-id: ${{ secrets.APPLICATION_ID }}
          api-key: ${{ secrets.API_KEY }}
```

## Inputs

All inputs are optional. Omit any you don’t need.

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `paths` | Comma-separated list of paths to scan | No | `.` |
| `severity` | Filter findings by severity (critical, high, medium, low, info) | No | `` |
| `exclude` | Comma-separated list of patterns to exclude | No | `` |
| `include` | Comma-separated list of patterns to include | No | `` |
| `python-version` | Python version to use | No | `3.9` |
| `format` | Output format (console, json, sarif) | No | `sarif` |
| `output` | Output file path | No | `` |
| `enable-ai-filter` | Enable AI-powered false positive filtering | No | `false` |
| `ai-provider` | AI provider (openai, anthropic) | No | `openai` |
| `ai-model` | AI model to use (e.g., gpt-4, claude-3-opus) | No | `gpt-4` |
| `ai-api-key` | API key for AI provider (use secrets) | No | `` |
| `ai-confidence-threshold` | Minimum confidence threshold for AI analysis (0.0-1.0) | No | `0.7` |
| `ai-max-findings` | Maximum number of findings to analyze with AI | No | `` |
| `upload-endpoint` | Backend API endpoint for uploading scan results | No | `` |
| `application-id` | Application ID for backend upload | No | `` |
| `api-key` | API key for backend upload (use secrets) | No | `` |

## Outputs

| Output | Description |
|--------|-------------|
| `scan-findings` | Number of findings detected |
| `sarif-path` | Path to SARIF output file |

## Examples

### Scan Specific Directories

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    paths: 'src,lib,tests'
```

### Filter by Severity

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    severity: 'critical,high'
```

### Exclude Patterns

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    exclude: '**/test/**,**/node_modules/**,**/*.test.py'
```

### AI Analysis with Custom Threshold

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    enable-ai-filter: 'true'
    ai-provider: 'anthropic'
    ai-model: 'claude-3-opus'
    ai-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
    ai-confidence-threshold: '0.85'
    ai-max-findings: '50'
```

### Upload to Backend

```yaml
- uses: spydra-tech/trusys-llm-security-scan-action@v1
  with:
    format: 'json'
    upload-endpoint: 'https://api.example.com/api/scans'
    application-id: 'my-app-123'
    api-key: ${{ secrets.BACKEND_API_KEY }}
```

## Security Best Practices

1. **Never commit API keys**: Always use GitHub Secrets for sensitive values like `ai-api-key` and `api-key`
2. **Use least privilege**: Only grant necessary permissions to the action
3. **Review findings**: Regularly review scan results and update your code accordingly
4. **Keep updated**: Use the latest version of the action for security updates

## Supported Frameworks

- OpenAI
- Anthropic (Claude)
- Cohere
- Langchain
- LlamaIndex
- Hugging Face
- Azure OpenAI
- AWS Bedrock

## Requirements

- **In your repo:** Nothing. The action installs Python (3.9 by default, overridable via `python-version`) and the scanner (`trusys-llm-scan`) from PyPI.
- **GitHub:** A repository with Actions enabled; add the workflow file under `.github/workflows/`.

## Troubleshooting

### Scanner not found
Ensure Python is properly set up. The action uses `actions/setup-python@v5` by default.

### AI analysis not working
- Verify your API key is correctly set in GitHub Secrets
- Check that the AI provider and model are supported
- Review the action logs for detailed error messages

### Upload failing
- Verify all required inputs (`upload-endpoint`, `application-id`, `api-key`) are provided
- Check that your backend API is accessible from GitHub Actions runners
- Review network connectivity and authentication

## License

MIT License

## Support

For issues and questions, please open an issue in the [repository](https://github.com/spydra-tech/trusys-llm-security-scan-action).
