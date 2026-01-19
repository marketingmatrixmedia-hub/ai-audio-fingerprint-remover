# Dependency Migration Guide

This guide provides step-by-step instructions for updating the AI Audio Fingerprint Remover dependencies from legacy versions to the recommended modern versions.

## Quick Start

```bash
# Backup current environment
pip freeze > requirements-backup.txt

# Install recommended dependencies
pip install -r requirements-recommended.txt

# Run tests to verify everything works
python test_all_methods.py
python test_fixes.py
```

## Detailed Migration Steps

### Step 1: Backup and Preparation

```bash
# 1. Backup current working environment
pip freeze > requirements-backup.txt

# 2. Create a test branch (if using git)
git checkout -b update-dependencies

# 3. Verify current Python version (should be 3.11+)
python --version  # Should show 3.11.x or higher
```

### Step 2: Update Dependencies

#### Option A: Gradual Update (Recommended for Production)

Update one package at a time and test:

```bash
# 1. Start with numpy (most critical)
pip install "numpy~=2.0.0"
python test_fixes.py  # Quick validation

# 2. Update scipy
pip install "scipy~=1.14.0"
python test_fixes.py

# 3. Update librosa
pip install "librosa~=0.11.0"
python test_fixes.py

# 4. Update soundfile
pip install "soundfile~=0.13.0"
python test_fixes.py

# 5. Update mutagen
pip install "mutagen~=1.47.0"
python test_fixes.py

# 6. Update matplotlib (if needed)
pip install "matplotlib~=3.9.0"
```

#### Option B: Bulk Update (Faster, for Development)

```bash
# Install all recommended dependencies at once
pip install -r requirements-recommended.txt

# Run comprehensive tests
python test_all_methods.py
```

### Step 3: Critical Testing

Run all tests to ensure functionality is preserved:

```bash
# 1. Run fix validation tests
python test_fixes.py

# 2. Run comprehensive method tests
python test_all_methods.py

# 3. Test with a sample file (replace with your test file)
python ai_audio_fingerprint_remover.py test_input.wav test_output.wav --level aggressive

# 4. Verify no silent outputs (check RMS)
python -c "import soundfile as sf; import numpy as np; data, sr = sf.read('test_output.wav'); print(f'RMS: {np.sqrt(np.mean(data**2))}')"

# 5. Run effectiveness tests
python watermark_effectiveness_tester.py original.mp3 test_output.wav
```

### Step 4: numpy 2.x Specific Changes

numpy 2.0 introduced breaking changes. Check for these issues:

#### Common Issues and Fixes

**1. Deprecated dtypes**
```python
# OLD (may warn or fail)
arr = np.array([1, 2, 3], dtype=np.int)

# NEW (numpy 2.x compatible)
arr = np.array([1, 2, 3], dtype=np.int64)
```

**2. String dtype changes**
```python
# OLD
arr = np.array(['a', 'b'], dtype='S1')

# NEW
arr = np.array(['a', 'b'], dtype='S1')  # Still works but check warnings
```

**3. Boolean indexing behavior**
```python
# If you see deprecation warnings about boolean indexing,
# review the numpy 2.0 migration guide
```

**Check for issues in code:**
```bash
# Search for potentially problematic patterns
grep -r "dtype=np\.int" *.py
grep -r "dtype=np\.float" *.py
grep -r "\.tostring()" *.py  # Deprecated, use .tobytes()
```

### Step 5: Validation Checklist

- [ ] All imports work without errors
- [ ] test_fixes.py passes all tests
- [ ] test_all_methods.py completes successfully
- [ ] Audio files process without creating silent outputs
- [ ] RMS values are non-zero for processed files
- [ ] Watermark detection rates remain high (85-95%)
- [ ] Quality metrics remain acceptable (75-85%)
- [ ] Metadata stripping works correctly
- [ ] All processing levels work (gentle, moderate, aggressive, extreme)
- [ ] Directory batch processing works
- [ ] No new warnings or deprecation messages

### Step 6: Rollback if Needed

If issues occur:

```bash
# Uninstall new versions
pip uninstall numpy scipy librosa soundfile mutagen matplotlib

# Restore from backup
pip install -r requirements-backup.txt

# Verify restoration
pip freeze | grep -E "(numpy|scipy|librosa|soundfile|mutagen|matplotlib)"
```

## Known Migration Issues

### Issue 1: numpy 2.x Import Errors

**Symptom**: `ImportError: numpy.core.multiarray failed to import`

**Solution**:
```bash
pip install --upgrade numpy
pip install --force-reinstall numpy
```

### Issue 2: librosa Compatibility

**Symptom**: Warnings about numpy compatibility

**Solution**:
```bash
# Ensure librosa 0.11.0+ which supports numpy 2.x
pip install "librosa>=0.11.0"
```

### Issue 3: scipy FFT Changes

**Symptom**: Different FFT results

**Solution**: scipy.fft API is stable, but verify filter responses:
```python
# Add validation after critical FFT operations
import numpy as np
from scipy.fft import fft

# Verify FFT output is reasonable
result = fft(audio_data)
assert not np.any(np.isnan(result)), "FFT produced NaN"
assert not np.any(np.isinf(result)), "FFT produced Inf"
```

### Issue 4: Performance Differences

**Symptom**: Slower/faster processing

**Solution**: numpy 2.x is generally faster. If slower:
```bash
# Check for MKL/OpenBLAS issues
python -c "import numpy; numpy.show_config()"

# Consider reinstalling with specific BLAS
pip install --force-reinstall numpy
```

## Performance Benchmarking

Compare performance before and after:

```bash
# Create benchmark script
cat > benchmark.py << 'EOF'
import time
import numpy as np
from ai_audio_fingerprint_remover import process_audio

start = time.time()
# Run your typical workload
process_audio("test_file.wav", "output.wav")
end = time.time()

print(f"Processing time: {end - start:.2f}s")
EOF

# Run before update
python benchmark.py  # Note the time

# Update dependencies
pip install -r requirements-recommended.txt

# Run after update
python benchmark.py  # Compare the time
```

## Continuous Integration Updates

If using CI/CD, update your workflow:

```yaml
# .github/workflows/test.yml
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install -r requirements-recommended.txt

- name: Run tests
  run: |
    python test_fixes.py
    python test_all_methods.py
```

## Docker Updates

If using Docker:

```dockerfile
# Update Dockerfile
FROM python:3.11-slim

# Copy new requirements
COPY requirements-recommended.txt .
RUN pip install --no-cache-dir -r requirements-recommended.txt

# Rest of Dockerfile...
```

## Environment-Specific Notes

### Virtual Environments

```bash
# Create fresh venv with new dependencies
python3 -m venv venv-new
source venv-new/bin/activate
pip install -r requirements-recommended.txt
```

### Conda Environments

```bash
# Create new conda environment
conda create -n audio-fingerprint python=3.11
conda activate audio-fingerprint
pip install -r requirements-recommended.txt
```

## Post-Migration

### 1. Update Documentation

Update installation instructions in README.md:

```markdown
## Installation

\```bash
pip install -r requirements-recommended.txt
\```

For minimal installation (production):
\```bash
pip install -r requirements-core.txt
\```

For development with testing tools:
\```bash
pip install -r requirements-dev.txt
\```
```

### 2. Update CLAUDE.md

Update the "Key Libraries" section with new version numbers.

### 3. Monitor for Issues

Watch for:
- User bug reports
- Performance regressions
- New deprecation warnings
- Compatibility issues with other tools

## Getting Help

If you encounter issues:

1. Check the [numpy 2.0 migration guide](https://numpy.org/doc/stable/numpy_2_0_migration_guide.html)
2. Review the DEPENDENCY_AUDIT_REPORT.md for detailed analysis
3. Search for similar issues in package repositories
4. Rollback to previous versions if critical issues occur

## Success Criteria

Migration is successful when:

- ✅ All tests pass
- ✅ Audio processing produces non-silent outputs
- ✅ Watermark removal effectiveness maintained (85-95%)
- ✅ Quality metrics preserved (75-85%)
- ✅ No critical warnings or errors
- ✅ Performance is maintained or improved

---

**Created**: 2026-01-19
**Last Updated**: 2026-01-19
**Review Date**: After first production migration
