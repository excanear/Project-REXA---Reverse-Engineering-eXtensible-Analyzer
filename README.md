<div align="center">

# REXA

### Reverse Engineering eXtensible Analyzer

*A modern, high-performance reverse engineering framework built in Rust*

[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](#license)
[![Rust Version](https://img.shields.io/badge/rust-1.75+-orange.svg)](https://www.rust-lang.org)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

[Features](#-features) • [Quick Start](#-quick-start) • [Architecture](#-architecture) • [Documentation](#-documentation) • [Contributing](#-contributing)

</div>

---

## 📖 Overview

**REXA** is a next-generation reverse engineering framework designed for speed, extensibility, and advanced analysis. Built entirely in Rust, REXA combines modern software engineering practices with cutting-edge binary analysis techniques to provide a powerful platform for security researchers, malware analysts, and reverse engineers.

### Why REXA?

- **🚀 Blazing Fast**: Rust-powered performance with zero-cost abstractions
- **🧠 AI-Powered**: Native integration with LLMs for intelligent code analysis
- **🔌 Highly Extensible**: Rich plugin system supporting Rust, Python, and Lua
- **🔬 Advanced Decompilation**: SSA-based decompiler with aggressive optimizations
- **🛡️ Security-Focused**: Built-in vulnerability detection and symbolic execution
- **🎯 Modular Architecture**: 19 specialized crates for maximum flexibility

---

## ✨ Features

### Core Capabilities

#### 🔬 Advanced Decompiler

- **Complete Pipeline**: Assembly → IR → SSA → Optimizations → Structuring → C Code
- **140+ x86/x64 Instructions**: MOV, arithmetic, SIMD (SSE/AVX), FPU, string operations
- **6 Optimization Passes**: 
  - Dead Code Elimination (DCE)
  - Constant Folding & Propagation
  - Copy Propagation
  - Common Subexpression Elimination (CSE)
  - Algebraic Simplification
  - Strength Reduction
- **Control Flow Recovery**: Automatic detection of if/else, loops, and switch statements using the Cifuentes algorithm
- **Clean Code Generation**: Produces readable C code with proper precedence, minimal casts, and helpful comments

#### 🧠 AI Integration

Leverage the power of AI for intelligent binary analysis:

- **Function Explanation**: Automatic summarization of what functions do
- **Smart Renaming**: Intelligent variable and function name suggestions
- **Vulnerability Detection**: AI-powered identification of potential security issues
- **Code Similarity**: Semantic search across codebases
- **Type Inference**: ML-assisted type recovery
- **Multi-Provider Support**: OpenAI, Anthropic, and local models

#### 🛡️ Security Analysis

Comprehensive security analysis capabilities:

- **Vulnerability Scanner**: 
  - Buffer overflows and underflows
  - Use-after-free and double-free
  - Format string vulnerabilities
  - Integer overflow/underflow
  - Null pointer dereferences
  - Race conditions
- **CVE Database**: Pattern matching against known vulnerabilities
- **Symbolic Execution**: Z3-powered constraint solving for automatic vulnerability discovery
- **Taint Analysis**: Track untrusted input through execution paths
- **Path Exploration**: BFS/DFS strategies with coverage-guided exploration

#### 🔌 Plugin System

Powerful, multi-language plugin architecture:

**Rust Plugins** - Maximum performance:
```rust
use rexa_plugin::prelude::*;

#[rexa_plugin]
pub struct CryptoScanner;

impl Plugin for CryptoScanner {
    fn on_function_analyzed(&mut self, func: &Function) {
        if func.contains_constant(0x67452301) {
            self.report("MD5 constant detected");
        }
    }
}
```

**Python Plugins** - Rapid prototyping:
```python
from rexa import Plugin

class MyAnalyzer(Plugin):
    def on_function_analyzed(self, func):
        if "crypto" in func.name.lower():
            self.log(f"Crypto function found: {func.name}")
```

**Lua Plugins** - Lightweight scripting:
```lua
function on_function_analyzed(func)
    if string.match(func.name, "^crypt") then
        log("Found encryption function")
    end
end
```

Features:
- **Hot-Reload**: Develop plugins without restarting
- **Rich API**: 200+ functions for analysis, GUI, and emulation
- **Event-Driven**: 9 event types with structured data
- **Thread-Safe**: Concurrent plugin execution

#### 📦 Binary Format Support

- **PE** (Windows executables, DLLs, drivers)
- **ELF** (Linux binaries, shared libraries)
- **Mach-O** (macOS executables, frameworks)
- **Raw Binary** (firmware, bootloaders)

#### 🖥️ Architecture Support

- **x86** (32-bit Intel/AMD)
- **x86-64** (64-bit Intel/AMD)
- **ARM** / **ARM64** (AArch64)
- **MIPS** (planned)
- **RISC-V** (planned)

#### 🔍 Additional Analysis Tools

- **Binary Diffing**: Function-level comparison, patch detection, and security impact assessment
- **Type Inference**: Advanced type propagation and structure recovery
- **Signature System**: FLIRT-compatible signatures, YARA rules, library identification
- **Emulator**: Dynamic analysis with CPU emulation and syscall interception

---

## 🚀 Quick Start

### Prerequisites

- **Rust** 1.75 or higher ([Install Rust](https://rustup.rs/))
- **Git**

### Installation

```bash
# Clone the repository
git clone https://github.com/rexa-re/rexa.git
cd rexa

# Build the project (release mode for best performance)
cargo build --release

# Run tests to verify installation
cargo test --all

# Install the CLI tool globally
cargo install --path crates/rexa-cli
```

### Basic Usage

```bash
# Analyze a binary
rexa analyze binary.exe

# Decompile a specific function
rexa decompile --address 0x401000 binary.exe

# Launch the GUI
rexa gui binary.exe

# Run with a plugin
rexa analyze --plugin crypto-scanner binary.exe

# Export analysis results
rexa export --format json binary.exe > analysis.json
```

### GUI Mode

The GUI provides an intuitive interface for binary analysis:

```bash
rexa gui /path/to/binary
```

Features:
- Interactive disassembly view
- Function list and call graphs
- Hex editor
- Decompiler output
- Plugin management

---

## 🏗️ Architecture

REXA is organized into 19 specialized crates, each handling a specific aspect of binary analysis:

```
rexa/
├── rexa-core          # Core data structures, traits, and interfaces
├── rexa-loader        # Binary file format parsing (PE, ELF, Mach-O)
├── rexa-disasm        # Disassembly engine (Capstone wrapper)
├── rexa-ir            # Intermediate representation (IR)
├── rexa-decompiler    # High-level decompilation pipeline
├── rexa-analysis      # Static analysis passes and algorithms
├── rexa-ai            # AI/LLM integration layer
├── rexa-plugins       # Plugin system and API
├── rexa-cli           # Command-line interface
├── rexa-gui           # Graphical interface (egui)
├── rexa-api           # REST API server for remote access
├── rexa-emulator      # CPU emulation for dynamic analysis
├── rexa-vuln          # Vulnerability detection engine
├── rexa-diff          # Binary diffing algorithms
├── rexa-types         # Type system and inference
├── rexa-signatures    # Pattern matching and signatures
├── rexa-symbolic      # Symbolic execution engine
├── rexa-utils         # Common utilities and helpers
└── rexa-filesystem    # Virtual filesystem abstraction
```

Each crate is:
- **Independently testable**
- **Usable as a standalone library**
- **Well-documented with examples**
- **Following Rust best practices**

### Data Flow

```
Binary File → Loader → Disassembler → IR → Decompiler → High-Level Code
                ↓          ↓          ↓        ↓
            Analysis ← Plugins ← AI ← Types
```

---

## 📚 Documentation

- **[Roadmap](docs/ROADMAP_VISUAL.md)** - Project roadmap and milestones
- **[API Reference](https://docs.rs/rexa)** - Complete API documentation
- **[Plugin Development Guide](docs/plugins.md)** - Create your own plugins
- **[Architecture Deep Dive](docs/architecture.md)** - Internal system design
- **[Contributing Guide](CONTRIBUTING.md)** - How to contribute

---

## 🤝 Contributing

We welcome contributions from the community! Whether you're fixing bugs, adding features, improving documentation, or creating plugins, your help is appreciated.

### Ways to Contribute

- 🐛 **Report Bugs**: Open an issue with detailed reproduction steps
- ✨ **Request Features**: Propose new features through GitHub issues
- 📖 **Improve Documentation**: Help us make docs clearer and more comprehensive
- 🧪 **Write Tests**: Increase code coverage and reliability
- 🔌 **Create Plugins**: Share your analysis tools with the community
- 💻 **Submit Code**: Fix bugs or implement new features

### Development Setup

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/rexa.git
cd rexa

# Create a feature branch
git checkout -b feature/my-awesome-feature

# Make your changes and test
cargo test --all
cargo clippy --all-targets --all-features
cargo fmt --all

# Commit and push
git commit -am "Add my awesome feature"
git push origin feature/my-awesome-feature
```

Then open a Pull Request on GitHub with a clear description of your changes.

### Code Standards

- Follow Rust idioms and best practices
- Write tests for new functionality
- Document public APIs with doc comments
- Run `cargo fmt` and `cargo clippy` before committing
- Keep commits focused and atomic

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

---

## 🛣️ Roadmap

### Short Term (Q1 2026)

- [ ] Complete type inference system
- [ ] Expand architecture support (ARM64 improvements)
- [ ] Enhanced GUI features
- [ ] 70%+ test coverage
- [ ] Comprehensive documentation

### Medium Term (Q2-Q3 2026)

- [ ] Production-ready AI features
- [ ] Advanced vulnerability scanner
- [ ] Binary diffing improvements
- [ ] Performance optimizations
- [ ] Plugin marketplace

### Long Term (Q4 2026+)

- [ ] Cloud-based distributed analysis
- [ ] Real-time collaboration features
- [ ] Enterprise features (SSO, audit logs)
- [ ] Mobile architecture support
- [ ] Advanced emulation capabilities

See [docs/ROADMAP_VISUAL.md](docs/ROADMAP_VISUAL.md) for the complete roadmap.

---

## 📊 Project Status

REXA is in active development. Key components are functional, with continuous improvements being made.

**Current Focus**: Stability, testing, and documentation

**Release Status**: Alpha (v0.1.0)

---

## 📄 License

REXA is dual-licensed under your choice of:

- **MIT License** ([LICENSE-MIT](LICENSE-MIT))
- **Apache License 2.0** ([LICENSE-APACHE](LICENSE-APACHE))

This means you can use REXA under the terms of either license.

---

## 🙏 Acknowledgments

REXA is built on top of excellent open-source projects:

- **[Capstone](http://www.capstone-engine.org/)** - Multi-architecture disassembly framework
- **[Goblin](https://github.com/m4b/goblin)** - Binary parsing library for Rust
- **[egui](https://github.com/emilk/egui)** - Immediate mode GUI framework
- **[Z3](https://github.com/Z3Prover/z3)** - SMT solver for symbolic execution
- **Rust Community** - For creating an amazing ecosystem

Special thanks to all contributors and supporters of the project.

---

## ⚠️ Disclaimer

REXA is designed for legitimate security research, education, and software analysis. Users are responsible for ensuring their use complies with all applicable laws and regulations. The developers assume no liability for misuse of this tool.

---

<div align="center">

**Built with ❤️ from Escanearcpl**

[GitHub](https://github.com/rexa-re/rexa) • [Documentation](https://docs.rs/rexa) • [Report Bug](https://github.com/rexa-re/rexa/issues) • [Request Feature](https://github.com/rexa-re/rexa/issues)

⭐ **Star us on GitHub** if you find REXA useful!

</div>
