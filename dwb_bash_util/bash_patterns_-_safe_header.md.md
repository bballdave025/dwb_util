# Bash Patterns You'll Use a Lot

This file accompanies `safe_default_header.sh` in your `dwb_bash_util` repo.

---

## 1. Grep that may not match

```bash
# OK if not found (do nothing)
try grep -q "needle" file.txt

# Or capture status with maybe
if maybe grep -q "needle" file.txt; then
  info "Found needle"
else
  warn "Needle not found (continuing)"
fi
```

---

## 2. Tar that might see transient files

```bash
require_cmd tar
OUT="backups/snap_$(date +'%s').tar.gz"
mkdir -p backups
try tar -czf "$OUT"   --exclude='./backups/*'   --warning=no-file-changed   --ignore-failed-read   .
info "Wrote $OUT"
```

---

## 3. Pipelines with jq/AWS

```bash
require_cmd aws jq

# Hard fail if any step fails
aws s3 ls "s3://my-bucket/prefix/" | jq -r '.[].Key' | head

# Soft list (don’t abort if bucket missing, just warn)
if ! maybe aws s3 ls "s3://maybe-missing" >/dev/null; then
  warn "Bucket not accessible; skipping optional step"
fi
```

---

## 4. Optional features without blowing up strict mode

```bash
# run tool only if present
if command -v pre-commit >/dev/null 2>&1; then
  try pre-commit run --all-files
else
  info "pre-commit not installed; skipping"
fi
```

---

## 5. Safe loops over filenames with spaces/newlines

```bash
# NUL-terminated find + read
find . -type f -name '*.py' -print0 |
while IFS= read -r -d '' f; do
  info "Checking $f"
  # …
done
```

---

## 6. Soft environment validation

```bash
require_cmd python
if ! maybe python - <<'PY'
import sys, tensorflow as tf
print("TF:", tf.__version__)
PY
then
  die "TensorFlow not importable in this env"
fi
```

---

**Tip:** You can `source safe_default_header.sh` in new scripts, or paste it directly.
