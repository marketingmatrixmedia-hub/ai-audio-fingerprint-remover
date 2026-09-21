# AI Audio Fingerprint Remover

A comprehensive Python tool to remove AI-generated fingerprints, watermarks, and metadata from audio files.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Security: pip-audit](https://img.shields.io/badge/security-pip--audit-green.svg)](https://pypi.org/project/pip-audit/)
[![Dependencies: Updated 2026-09](https://img.shields.io/badge/dependencies-updated%202026--09-brightgreen.svg)]()

## Overview

AI Audio Fingerprint Remover is a powerful tool designed to address privacy concerns by removing all identifiable AI-generated traces from audio files. Modern AI systems like Suno, OpenAI, ElevenLabs, and others often embed various types of fingerprints in their generated audio—both visible metadata and hidden statistical patterns.

This tool implements multiple layers of protection to counter all known and theoretical fingerprinting techniques, ensuring your audio files maintain privacy while preserving quality.

## Features

### Complete Metadata Removal

- Strips all standard metadata (ID3, RIFF INFO, FLAC tags)
- Removes AI-specific tags and custom chunks
- Eliminates hidden identifiers in binary data
- Supports multiple formats: MP3, WAV, FLAC, AIFF

### Spectral Watermark Detection & Elimination

- Identifies and removes high-frequency watermarks
- Detects periodic patterns in specific frequency bands
- Applies targeted filters to neutralize watermarks
- Adds naturalistic noise to defeat absence-based fingerprinting

### Statistical Pattern Normalization

- Detects and corrects machine-like timing patterns
- Identifies unnatural amplitude distributions
- Normalizes frequency distributions
- Adds realistic micro-variations in timing

### Human-Like Imperfections

- Introduces subtle non-linearities in harmonics
- Adds realistic micro-dynamics
- Creates natural stereo imaging variations
- Applies minor phase adjustments

### Robust Verification

- Provides detailed reports of modifications
- Offers before/after metadata comparison
- Verifies effectiveness through hash comparison
- Handles batch processing for multiple files

## 🚀 Recent Updates (2026-09-20)

**Major Dependency Upgrade:**
- ✨ All dependencies upgraded to latest stable versions
- 🚀 **Significant performance improvements** with numpy 2.x
- 🔒 **5 years of security patches** applied
- ✅ **Full Python 3.11-3.14 compatibility**
- 📊 **43x improvement** in watermark detection

See [DEPENDENCY_AUDIT_REPORT.md](DEPENDENCY_AUDIT_REPORT.md) for complete details.

## Installation

### Requirements

- **Python 3.11+** (required for modern dependencies)
- Modern audio processing libraries (auto-installed)

### Dependencies (Updated 2026-09-20)

| Package | Version | Purpose |
|---------|---------|---------|
| numpy | ~=2.0.0 | Numerical operations (major performance boost) |
| scipy | ~=1.14.0 | Signal processing and filtering |
| librosa | ~=0.11.0 | Audio analysis and feature extraction |
| soundfile | ~=0.13.0 | Audio I/O operations |
| mutagen | ~=1.47.0 | Metadata manipulation |

**Security Status:** ✅ No known vulnerabilities (verified via pip-audit)

### Setup

1. Clone the repository:

```bash
git clone https://github.com/marketingmatrixmedia-hub/ai-audio-fingerprint-remover.git
cd ai-audio-fingerprint-remover
```

2. Create and activate virtual environment (recommended):

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### Basic Usage

**Process a single file:**

```
python ai_audio_fingerprint_remover.py input.mp3 output.mp3
```

**Process a file in-place:**

```
python ai_audio_fingerprint_remover.py input.wav
```

**Process all audio files in a directory:**

```
python ai_audio_fingerprint_remover.py --directory input_folder output_folder
```

### Advanced Options

**Processing Intensity Levels** (adjusts the balance between effectiveness and audio quality):

```bash
# Gentle - minimal processing, preserves quality but may leave some fingerprints
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --level gentle

# Moderate - balanced approach (default)
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --level moderate

# Aggressive - thorough fingerprint removal with minimal quality impact
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --level aggressive

# Extreme - maximum fingerprint removal, may introduce subtle artifacts
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --level extreme
```

**Legacy Aggressive Mode** (equivalent to --level aggressive):

```
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --aggressive
```

**View Metadata Before Removal:**

```
python ai_audio_fingerprint_remover.py --show input.wav output.wav
```

**Generate Detailed Report:**

```
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --report
```

**Verify Results:**

```
python ai_audio_fingerprint_remover.py input.mp3 output.mp3 --verify
```

## Example Output

```
AI Audio Fingerprint Remover
========================================
Processing level: extreme - Maximum processing - removes all detectable fingerprints but may affect quality


Processing _A Hack Song (Glitchy)_.wav...

Results:
  Processing level: extreme
  Files processed: 1
  Watermarks detected and removed: 5
  Statistical patterns normalized: 1
  Timing adjustments applied: 1

Metadata removed:
  wav_rewrite: 1 items

Verification:
  Original file hash: 511194868d6287c54123b0ace467b321f504f99088d02499915a1fbbf9c63930
  Processed file hash: 400cf80b63d1238b373b5a35db2fc6ffd4c34740adaac901989ae7a7b311b149
  Files are different

Processing complete.
```

## 🎯 Target Platforms

Optimized for removing fingerprints from:
- **Suno AI** (7 specific frequency ranges targeted)
- **OpenAI** audio models
- **ElevenLabs**
- Other AI audio generation platforms

### Suno AI Frequency Targets

The tool specifically targets these watermark bands:
- 19000-20000 Hz (Ultrasonic watermark)
- 15000-16000 Hz (Mid-high watermark)
- 8000-8200 Hz (Mid-range marker)
- 50-150 Hz (Low-frequency steganography)
- 12000-12100 Hz (Secondary marker)
- 17500-18500 Hz (Extended range)
- 22000-23000 Hz (Extended ultrasonic)

## 📊 Performance Metrics

**For Suno AI files (aggressive level):**
- Watermark Removal: **85-95/100**
- Quality Preservation: **75-85/100**
- Perceptual Preservation: **80-90/100**
- Overall Effectiveness: **80-90/100** (Excellent)

**Processing Performance:**
- 30% faster for large files (with numpy 2.x)
- 40% less memory usage via chunked processing
- Adaptive filtering based on file characteristics

## Under the Hood

The tool implements a sophisticated **4-pass approach** to address all known and theoretical AI fingerprinting techniques:

### Pass 1: Complete Metadata Stripping
- Removes all standard metadata (ID3, RIFF INFO, FLAC tags)
- Eliminates AI-specific tags and custom chunks
- Cleans hidden identifiers in binary data
- Uses `mutagen` for comprehensive format support

### Pass 2: Spectral Watermark Detection & Removal
- Identifies watermarks in specific frequency bands
- Applies targeted band-reject filters
- Detects periodic patterns using advanced algorithms
- Adds naturalistic noise to defeat absence-based fingerprinting

### Pass 3: Statistical Pattern Normalization
- Analyzes and corrects machine-like timing patterns
- Normalizes unnatural amplitude distributions
- Adjusts frequency distributions to appear natural
- Applies psychoacoustic processing

### Pass 4: Human-Like Imperfections
- Introduces subtle non-linearities in harmonics
- Adds realistic micro-dynamics and timing variations
- Creates natural stereo imaging variations
- Applies minor phase adjustments to defeat AI detection

## 🛠️ Additional Tools

The repository includes several advanced analysis and testing tools:

### Detection & Analysis
```bash
# Enhanced Suno AI detection
python enhanced_suno_detector.py "audio_file.mp3"

# Detailed watermark pattern analysis
python advanced_watermark_analysis.py "audio_file.mp3" --output "report.txt"

# Quick comparison tool
python quick_comparison.py input.wav

# Neural network-based detection
python neural_watermark_detector.py "audio_file.mp3"
```

### Testing & Validation
```bash
# Test all processing methods
python test_all_methods.py

# Validate removal effectiveness
python watermark_effectiveness_tester.py "original.mp3" "processed.wav"

# Test recent bug fixes
python test_fixes.py

# Compare before/after
python watermark_comparison.py
```

### Advanced Removal Tools
- **aggressive_watermark_remover.py** - Specialized aggressive techniques
- **sota_watermark_remover.py** - State-of-the-art algorithms
- **integrated_system.py** - Integrated multi-method removal
- **next_gen_remover.py** - Next-generation techniques
- **performance_optimizer.py** - Performance optimization layer

## 🔒 Privacy and Security

- ✅ **All processing is local** - no data sent to external servers
- ✅ **No telemetry or logging** unless explicitly requested
- ✅ **Secure by design** with comprehensive input validation
- ✅ **No known vulnerabilities** in dependencies (verified via pip-audit)
- ✅ **Open source** - full transparency of all operations

**Security Audit (2026-09-20):**
- All dependencies scanned with pip-audit
- Zero CVEs detected
- Modern cryptographic practices
- Comprehensive error handling

## ⚠️ Limitations

- Extremely aggressive watermarks may require quality trade-offs
- Some countermeasures may introduce subtle artifacts (typically inaudible)
- Cannot remove content-based fingerprinting (where content itself is the fingerprint)
- Effectiveness varies based on watermarking technique used

## 🐛 Recent Bug Fixes (v2.0)

### Critical Issues Resolved
1. ✅ **Silent Output Fix** - Fixed initialization creating silent outputs
2. ✅ **NaN/Inf Handling** - Safe interpolation instead of aggressive replacement
3. ✅ **Audio Validation** - Added comprehensive validation before writing
4. ✅ **Filter Stability** - Fixed filter order calculations for short segments
5. ✅ **AI Detection Bypass** - Enhanced pattern disruption techniques

### Validation Improvements
- Audio content validation at each processing stage
- RMS and amplitude checks to prevent silent outputs
- Per-channel validation for stereo processing
- Filter stability checks for edge cases

## 📚 Documentation

- **[README.md](README.md)** - This file (quick start and overview)
- **[CLAUDE.md](CLAUDE.md)** - Detailed technical documentation
- **[DEPENDENCY_AUDIT_REPORT.md](DEPENDENCY_AUDIT_REPORT.md)** - Complete dependency analysis
- **[DEPENDENCY_MIGRATION_GUIDE.md](DEPENDENCY_MIGRATION_GUIDE.md)** - Upgrade instructions

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Guidelines

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Follow secure coding practices
4. Maintain comprehensive error handling
5. Add tests for new features
6. Document security considerations
7. Commit your changes (`git commit -m 'Add some amazing feature'`)
8. Push to the branch (`git push origin feature/amazing-feature`)
9. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ⚖️ Legal & Ethical Use

### ⚠️ Important Disclaimer

**This tool is intended for legitimate privacy protection purposes only.**

Users are **solely responsible** for ensuring they comply with all applicable laws, regulations, and terms of service when using this software. The authors and contributors:

- Do **NOT** condone or support any illegal activities
- Do **NOT** endorse circumventing copyright protections
- Do **NOT** encourage violating terms of service
- Are **NOT** liable for misuse of this software

### Permitted Uses

✅ **Protecting personal privacy** in legally obtained content  
✅ **Research and educational purposes** with proper authorization  
✅ **Security testing** with explicit permission  
✅ **Defensive security applications** for privacy protection  
✅ **Academic research** into watermarking and steganography  

### Prohibited Uses

❌ Circumventing copyright protection mechanisms  
❌ Violating platform terms of service  
❌ Fraudulent activities or misrepresentation  
❌ Removing attribution without permission  
❌ Any illegal or unethical purposes  

**Use responsibly and ethically. When in doubt, seek legal counsel.**

## 🙏 Acknowledgments

- Built with excellent open-source libraries: numpy, scipy, librosa, soundfile, mutagen
- Inspired by the need for privacy protection in AI-generated content
- Thanks to the audio processing and security research communities
- Recent dependency upgrades ensure modern, secure operation

## 📞 Support & Contact

- **Issues:** [GitHub Issues](https://github.com/marketingmatrixmedia-hub/ai-audio-fingerprint-remover/issues)
- **Documentation:** See [CLAUDE.md](CLAUDE.md) for technical details
- **Security:** Report security issues via GitHub Issues (private disclosure available)

---

<div align="center">

**🔒 Privacy-focused • 🚀 High-performance • 🛡️ Secure by design**

Made with ❤️ for privacy protection

*Last updated: 2026-09-20 | Version 2.0*

</div>
