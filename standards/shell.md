# Shell Scripting Standards

Standards and conventions for shell scripts in the ecosystem.

## Toolchain

- **Shell:** Bash 4+ (for arrays and associative arrays)
- **Linter:** ShellCheck
- **Formatter:** shfmt

## Script Header

Every script must start with:

```bash
#!/usr/bin/env bash
#
# Script Name: script-name.sh
# Description: Brief description of what this script does
# Author: Sebastien Rousseau
# License: MIT OR Apache-2.0
#

set -euo pipefail
IFS=$'\n\t'
```

### Flags Explanation

| Flag | Purpose |
|------|---------|
| `-e` | Exit on error |
| `-u` | Error on undefined variables |
| `-o pipefail` | Fail on pipe errors |
| `IFS` | Safe word splitting |

## Project Structure

```
project/
├── scripts/
│   ├── install.sh
│   ├── build.sh
│   └── lib/
│       └── common.sh
├── tests/
│   └── test-*.sh
├── README.md
└── .github/
    └── workflows/
        └── ci.yml
```

## Naming Conventions

| Item | Convention | Example |
|------|------------|---------|
| Scripts | kebab-case | `run-tests.sh` |
| Functions | snake_case | `do_something` |
| Variables (local) | snake_case | `local_var` |
| Variables (exported) | SCREAMING_SNAKE_CASE | `EXPORT_VAR` |
| Constants | SCREAMING_SNAKE_CASE | `readonly MAX_RETRIES=3` |

## Variables

### Always Quote Variables

```bash
# Good
echo "${variable}"
cp "${source}" "${destination}"

# Bad
echo $variable
cp $source $destination
```

### Use Default Values

```bash
# Default value if unset
name="${NAME:-default}"

# Error if unset
required="${REQUIRED:?'REQUIRED must be set'}"

# Alternative value if set
override="${OVERRIDE:+alternative}"
```

### Declare Local Variables

```bash
my_function() {
    local input="$1"
    local result

    result=$(process "${input}")
    echo "${result}"
}
```

## Functions

### Standard Function Format

```bash
#######################################
# Description of the function.
# Globals:
#   GLOBAL_VAR - description
# Arguments:
#   $1 - First argument description
#   $2 - Second argument description (optional)
# Outputs:
#   Writes to stdout
# Returns:
#   0 on success, non-zero on error
#######################################
my_function() {
    local arg1="$1"
    local arg2="${2:-default}"

    # Function body
    echo "Processing: ${arg1}"

    return 0
}
```

## Error Handling

### Use trap for Cleanup

```bash
cleanup() {
    local exit_code=$?
    rm -rf "${TEMP_DIR:-}"
    exit "${exit_code}"
}

trap cleanup EXIT ERR INT TERM

TEMP_DIR=$(mktemp -d)
```

### Explicit Error Messages

```bash
die() {
    echo "ERROR: $*" >&2
    exit 1
}

# Usage
[[ -f "${config_file}" ]] || die "Config file not found: ${config_file}"
```

## Logging

### Standard Logging Functions

```bash
readonly RED='\033[0;31m'
readonly GREEN='\033[0;32m'
readonly YELLOW='\033[0;33m'
readonly NC='\033[0m' # No Color

log_info() {
    echo -e "${GREEN}[INFO]${NC} $*"
}

log_warn() {
    echo -e "${YELLOW}[WARN]${NC} $*" >&2
}

log_error() {
    echo -e "${RED}[ERROR]${NC} $*" >&2
}
```

## Command Execution

### Check Command Availability

```bash
require_command() {
    command -v "$1" >/dev/null 2>&1 || die "Required command not found: $1"
}

require_command git
require_command docker
```

### Capture Output and Exit Code

```bash
if output=$(some_command 2>&1); then
    log_info "Success: ${output}"
else
    exit_code=$?
    log_error "Failed with code ${exit_code}: ${output}"
    return "${exit_code}"
fi
```

## Arrays

### Iterate Safely

```bash
declare -a items=("one" "two" "three")

for item in "${items[@]}"; do
    echo "Processing: ${item}"
done

# With index
for i in "${!items[@]}"; do
    echo "Item ${i}: ${items[i]}"
done
```

## Testing

### Use bats-core

```bash
#!/usr/bin/env bats

setup() {
    load 'test_helper/common-setup'
    _common_setup
}

@test "script exits successfully with valid input" {
    run ./script.sh --valid
    assert_success
}

@test "script fails with invalid input" {
    run ./script.sh --invalid
    assert_failure
    assert_output --partial "Error"
}
```

## Security

### Never Trust User Input

```bash
# Sanitize input
sanitize() {
    local input="$1"
    # Remove potentially dangerous characters
    echo "${input//[^a-zA-Z0-9_-]/}"
}

user_input=$(sanitize "${1:-}")
```

### Avoid eval

```bash
# Bad - security risk
eval "$user_command"

# Good - use arrays for dynamic commands
declare -a cmd=("$program" "$arg1" "$arg2")
"${cmd[@]}"
```

## CI/CD Requirements

```yaml
# .github/workflows/ci.yml
- name: Lint shell scripts
  run: |
    shellcheck scripts/*.sh
    shfmt -d scripts/*.sh

- name: Run tests
  run: bats tests/
```

## Portability

### Check for Bash Features

```bash
# Require minimum Bash version
if ((BASH_VERSINFO[0] < 4)); then
    die "Bash 4+ required"
fi
```

### Use POSIX When Possible

For maximum portability, prefer POSIX-compliant constructs:

```bash
# POSIX
[ -f "$file" ]

# Bash-specific (preferred in ecosystem)
[[ -f "$file" ]]
```
