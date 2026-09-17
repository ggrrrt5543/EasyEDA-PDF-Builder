# PDF2EasyEDA AI

AI-assisted pipeline for reconstructing electronic schematics and PCB projects from PDF files and exporting them to **EasyEDA Standard**.

> [!WARNING]
> This project is under active development. The current MVP can analyze PDFs, communicate with AI providers, build a structured circuit model, and validate extracted data. Native EasyEDA SCH and PCB generation is still in development.

## Project goal

The final workflow is designed to be simple:

1. Select a circuit PDF.
2. Select an AI provider and model.
3. Enter your API key.
4. Click **Generate**.
5. Receive validated EasyEDA files.

Expected output:

```text
output/
├── project_SCH.json
├── project_PCB.json
├── circuit_model.json
└── validation_report.html
```

## How it works

```text
PDF
 ↓
Page rendering and image preprocessing
 ↓
AI vision provider
 ↓
Structured circuit model
 ↓
Component, pin, net and footprint validation
 ↓
EasyEDA schematic generation
 ↓
PCB outline and component placement
 ↓
Netlist comparison, ERC and DRC
 ↓
EasyEDA SCH.json and PCB.json
```

The AI is used for recognition and interpretation. Deterministic code is responsible for validation and export.

The program must not silently guess when information is missing or ambiguous. Uncertain components, connections, pin numbers, footprints, or polarities should block the final export until they are resolved.

## Current features

- [x] Windows desktop interface
- [x] PDF page extraction
- [x] Multi-page PDF support
- [x] OpenAI-compatible provider adapter
- [x] Google Gemini adapter
- [x] Anthropic adapter
- [x] Offline mock provider for testing
- [x] Structured intermediate circuit model
- [x] Component, pin and net validation
- [x] Guarded pipeline that blocks invalid output
- [x] Reference EasyEDA and Specctra format checks
- [ ] Native EasyEDA schematic generator
- [ ] Automatic symbol matching
- [ ] Automatic footprint matching
- [ ] Native EasyEDA PCB generator
- [ ] PCB outline reconstruction
- [ ] Automatic component placement
- [ ] DSN/SES autorouter integration
- [ ] Automatic ERC and DRC integration
- [ ] Final one-click workflow

## Supported AI providers

- OpenAI and OpenAI-compatible APIs
- Google Gemini
- Anthropic Claude
- Local or custom OpenAI-compatible endpoints
- Mock/offline provider for development

Provider adapters are separated from the processing pipeline, allowing additional AI services to be added later.

## Installation on Windows

### Requirements

- Windows 10 or Windows 11
- Python 3.10 or newer
- Python 3.12 recommended
- API key for the selected AI provider

### Setup

Clone or download the repository:

```powershell
git clone https://github.com/YOUR_USERNAME/pdf2easyeda-ai.git
cd pdf2easyeda-ai
```

Run:

```text
install_windows.bat
```

After installation, start the application with:

```text
start_windows.bat
```

## Target one-click workflow

1. Open the application.
2. Select the input PDF.
3. Select an AI provider.
4. Enter the model name and API key.
5. Select the output directory.
6. Click **Generate EasyEDA project**.
7. Review any detected ambiguities.
8. Download the generated SCH, PCB, and validation report.

## Validation strategy

Before a result can be marked as valid, the pipeline should verify:

- Every component has a unique reference designator.
- All required component properties are present.
- Pin numbers match symbol definitions.
- Symbol pins match footprint pads.
- Every electrical net is internally consistent.
- No connections were silently added or removed.
- Power pins and power nets are verified.
- Polarized components have an identified orientation.
- Connector orientation and pin 1 are verified.
- Schematic and PCB netlists match.
- There are no unresolved recognition ambiguities.
- ERC completes without blocking errors.
- DRC completes without blocking errors.
- No unrouted connections remain when autorouting is enabled.

## Accuracy and safety

A PDF is a visual representation, not a complete machine-readable netlist. Some PDFs may contain missing labels, low-resolution images, hidden connections, unclear pin numbers, or undocumented footprints.

For this reason, PDF-only conversion cannot honestly guarantee perfect electrical or manufacturing correctness.

For improved accuracy, the pipeline may optionally accept:

- BOM files
- Existing netlists
- Datasheets
- Reference SCH/PCB files
- Footprint libraries
- Board photographs

> [!CAUTION]
> Never send an automatically generated PCB directly to manufacturing without engineering review. Verify power rails, pin numbering, connector orientation, component polarity, footprints, ERC, DRC, and the final netlist. Build and test a prototype before production.

## API key security

- API keys must not be stored in the repository.
- API keys must not be written to logs.
- Use environment variables, local secure storage, or temporary in-memory input.
- Never commit `.env` files.
- Cloud AI providers may receive PDF pages or extracted content. Review the provider's privacy policy before processing confidential designs.

Recommended `.gitignore` entries:

```gitignore
.env
.env.*
.venv/
venv/
__pycache__/
*.pyc
output/
logs/
```

## Roadmap

### v0.1 — Processing core

- PDF parsing
- AI provider adapters
- Structured circuit model
- Strict validation
- Windows launcher

### v0.2 — Schematic export

- Symbol library
- Net generation
- EasyEDA Standard SCH export
- Schematic validation

### v0.3 — PCB export

- Footprint library
- Board outline detection
- Component placement
- EasyEDA Standard PCB export

### v0.4 — Routing and verification

- DSN export
- Autorouter integration
- SES import
- ERC and DRC reports
- Final netlist comparison

### v1.0 — One-click conversion

```text
PDF + API key → validated EasyEDA SCH and PCB
```

## Contributing

Contributions, testing files, bug reports, symbol libraries, footprint mappings, and EasyEDA format research are welcome.

When reporting an issue, please remove API keys and confidential circuit information before uploading files.

## License

MIT License. See `LICENSE` for details.
