# iOS ML Artifacts

Pre-built iOS ML artifacts for on-device inference. Includes ONNX Runtime xcframework, sqlite-vec static libraries, and embedding models optimized for iOS.

## Contents

### 1. ONNX Runtime iOS XCFramework
- **Version**: 1.22.0
- **Size**: ~49MB
- **Architectures**:
  - `ios-arm64` - Physical iOS devices
  - `ios-arm64-simulator` - iOS Simulator (Apple Silicon)
- **Features**: CoreML execution provider for Apple Neural Engine acceleration

### 2. sqlite-vec Static Libraries
- **Version**: 0.1.6
- **Size**: ~340KB per architecture
- **Architectures**:
  - `ios-arm64` - Physical iOS devices
  - `ios-arm64-simulator` - iOS Simulator (Apple Silicon)
- **Purpose**: Vector similarity search extension for SQLite

### 3. BGE-Small Text Embedding Model
- **Model**: `bge-small-en-v1.5-int8.onnx`
- **Size**: 32MB (INT8 quantized)
- **Dimensions**: 384
- **Use Case**: Text embeddings for semantic search
- **Includes**: `tokenizer.json` for text preprocessing

## Quick Start

### Download from GitHub Releases

```bash
# Download all artifacts
gh release download v1.0.0 --repo vannmex/vannmex-ios-ml-artifacts

# Or download specific files
gh release download v1.0.0 --pattern "onnxruntime-*.zip" --repo vannmex/vannmex-ios-ml-artifacts
gh release download v1.0.0 --pattern "sqlite-vec-*.zip" --repo vannmex/vannmex-ios-ml-artifacts
gh release download v1.0.0 --pattern "bge-small-*.zip" --repo vannmex/vannmex-ios-ml-artifacts
```

### Download from Hugging Face

```bash
# Install HF CLI
brew install huggingface-cli

# Download all artifacts
hf download vannmex/ios-ml-artifacts --local-dir ./artifacts

# Or download specific files
hf download vannmex/ios-ml-artifacts onnxruntime/onnxruntime.xcframework --local-dir ./onnxruntime
```

## Integration

### ONNX Runtime in Xcode

1. Unzip `onnxruntime-ios-1.22.0.xcframework.zip`
2. Drag `onnxruntime.xcframework` into your Xcode project
3. In Build Phases > Link Binary With Libraries, ensure it's listed
4. Add to Framework Search Paths if needed

### sqlite-vec in Rust/Tauri

```rust
// Load the extension
unsafe {
    conn.load_extension_enable()?;
    conn.load_extension("libsqlite_vec0", None)?;
    conn.load_extension_disable()?;
}
```

### BGE-Small Embedding Model

```rust
use ort::{Session, SessionBuilder};

let session = SessionBuilder::new()?
    .with_execution_providers([CoreMLExecutionProvider::default().build()])?
    .with_model_from_file("bge-small-en-v1.5-int8.onnx")?;
```

## Checksums

Verify file integrity with SHA-256:

```bash
shasum -a 256 *.zip
```

## License

- **ONNX Runtime**: MIT License
- **sqlite-vec**: MIT License
- **BGE-Small**: MIT License (BAAI)

## Related Projects

- [ONNX Runtime](https://github.com/microsoft/onnxruntime)
- [sqlite-vec](https://github.com/asg017/sqlite-vec)
- [BGE Models](https://huggingface.co/BAAI/bge-small-en-v1.5)
