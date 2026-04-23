# Magika

A fork of [google/magika](https://github.com/google/magika) — AI-powered file type detection using deep learning.

Magika uses a custom, lightweight deep learning model to detect file content types with high accuracy, even for files with misleading extensions or no extensions at all.

## Features

- **Fast & accurate**: Powered by a lightweight Keras model (~1MB)
- **Wide format support**: Detects 100+ file types including code, documents, media, and more
- **CLI tool**: Simple command-line interface for quick file inspection
- **Python API**: Easy integration into your Python projects
- **Rust bindings**: High-performance Rust implementation available
- **Docker support**: Ready-to-use container image

## Installation

### Python (recommended)

```bash
pip install magika
```

### From source

```bash
git clone https://github.com/your-org/magika.git
cd magika
pip install -e python/
```

## Quick Start

### CLI

```bash
# Detect file type
magika ./path/to/file

# Detect multiple files
magika file1.txt file2.bin file3.unknown

# Output as JSON
magika --json ./path/to/file

# Recursive directory scan
magika -r ./directory/
```

### Python API

```python
from magika import Magika

m = Magika()

# Detect from file path
result = m.identify_path("./path/to/file")
print(result.output.ct_label)   # e.g. "python"
print(result.output.mime_type)  # e.g. "text/x-python"
print(result.score)             # confidence score 0.0-1.0

# Detect from bytes
result = m.identify_bytes(b"#!/usr/bin/env python3\nprint('hello')")
print(result.output.ct_label)  # "python"

# Use HIGH_CONFIDENCE mode to reduce false positives (recommended for production use)
# Results below the confidence threshold fall back to a generic content-type label
from magika.types import MagikaPredictionMode
m = Magika(prediction_mode=MagikaPredictionMode.HIGH_CONFIDENCE)

# Batch identify multiple files at once (more efficient than calling identify_path in a loop)
from pathlib import Path
results = m.identify_paths([Path("file1.py"), Path("file2.js"), Path("file3.bin")])
for path, result in zip(["file1.py", "file2.js", "file3.bin"], results):
    print(f"{path}: {result.output.ct_label} ({result.score:.2f})")
```

## Supported File Types

Magika supports detection of 100+ content types. Run `magika --list-output-content-types` to see the full list.

Some highlights:
- Source code: Python, JavaScript, TypeScript, Rust, Go, C/C++, Java, and more
- Documents: PDF, DOCX, XLSX, PPTX, ODT
- Media: JPEG, PNG, GIF, MP4, MP3, WAV
- Archives: ZIP, TAR, GZ, 7Z
- Data: JSON, XML, CSV, YAML
- Executables: ELF, PE, Mach-O

## Development

### Prerequisites

- Python 3.8+
- Rust (for Rust bindings)
- Docker (optional)

### Setup

```bash
git clone https://github.com/your-org/magika.git
cd magika

# Python development
cd python
pip install -e ".[dev]"

# Run tests
pytest tests/
```

### Docker

```bash
# Build image
docker build -t magika .

# Run container
docker run --rm -v $(pwd):/data magika /data/y
```

## Notes

Personal fork used for learning and experimentation. For production use, refer to the [upstream repository](https://github.com/google/magika) which receives active maintenance and model updates.
