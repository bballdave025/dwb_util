# dwb_bash_util


## Safe-and-Portable

Utility scripts and references for safer, more portable Bash scripting.

### Files

- **safe_default_header.sh**  
  A consolidated header providing:
  - `set -euo pipefail` safety defaults  
  - Logging helpers: `info`, `warn`, `die`  
  - `require_cmd` for dependency checks  
  - Error tracing with last command + line number  
  - Temporary directory helper (`mktempdir`) with auto-cleanup  
  - Scoped failure-tolerant helpers: `try`, `maybe`  
  - Quiet pushd/popd wrappers  

  Use this by either **sourcing** it:
  ```bash
  source ~/my_repos_dwb/dwb_bash_util/safe_default_header.sh
  ```
  or **copy/pasting** it into the top of a script.

- **bash_patterns_*.md**  
  A set of real-world usage examples for the helpers, including:
  - Grep that may not match (`try` / `maybe`)  
  - Tar with excludes and transient-file safety  
  - Pipelines with `aws` and `jq`  
  - Optional features (skip gracefully if command missing)  
  - Safe loops over filenames (with NUL separation)  
  - Soft environment validation (e.g. TensorFlow import test)

### Workflow Tips

- When starting a new script, begin with:
  ```bash
  #!/usr/bin/env bash
  source ~/my_repos_dwb/dwb_bash_util/safe_default_header.sh
  ```

- When debugging or hardening an old script, you can prepend the header:
  ```bash
  cat safe_default_header.sh old_script.sh > tmp && mv tmp old_script.sh
  ```
  (make sure to remove duplicate shebangs if present).

- If a command is *expected* to fail sometimes, wrap with `try` or `maybe`
  instead of disabling `set -e` globally.

### Philosophy

The goal of this part of the repo is to make Bash scripting:
- Safer (fail early on real errors)
- More portable (works on Linux, macOS, AWS shells)
- Easier to debug (clear logs, error tracing)
- Easier to extend (drop-in helpers and patterns)

---

Happy scripting!
