# dublincore-bash
A bash script to read, validate and write Dublin Core metadata.

# Specifications
  - Requires bash 4.0 or higher (uses associative arrays)
  - Follows the "Guidelines for implementing Dublin Core™ in XML" 2003-04-02 https://www.dublincore.org/specifications/dublin-core/dc-xml-guidelines/2003-04-02/
  - Follows the "DCMI Metadata Terms" 2008-01-14 https://www.dublincore.org/specifications/dublin-core/dcmi-terms/2008-01-14/

# Usage

## Basic Operations
```bash
# Read and display metadata
dublincore.sh --read metadata.xml

# Validate Dublin Core compliance
dublincore.sh --validate --read metadata.xml

# Convert between formats
dublincore.sh --read metadata.txt --format xml --output metadata.xml
dublincore.sh --read metadata.xml --format html --output metadata.html
dublincore.sh --read metadata.html --format text --output metadata.txt

# Extract specific term
dublincore.sh --term "title" --read metadata.xml
dublincore.sh --term "creator" --read metadata.txt

# Extract term with clean output (values only, semicolon-separated if multiple)
dublincore.sh --term "title" --clean --read metadata.xml
dublincore.sh --term "creator" --clean --read metadata.xml  # Multiple values: "Smith, Jane;Johnson, Bob"

# Select specific term value by position (1-based index)
dublincore.sh --term "creator" --select 1 --read metadata.xml          # Gets first creator value
dublincore.sh --term "creator" --select 2 --read metadata.xml          # Gets second creator value  
dublincore.sh --term "creator" --select 2 --clean --read metadata.xml  # Gets second creator, clean output
dublincore.sh --term "identifier" --select 1 --clean --read metadata.xml  # Gets first identifier, values only

# Create subset files with multiple terms
dublincore.sh --read input.xml --term title --term creator --format xml --output subset.xml
dublincore.sh --read input.xml --term title --term date --term publisher --format text --output subset.txt
dublincore.sh --read input.xml --term abstract --term license --format html --output subset.html

# Multiple terms with verbose output for debugging
dublincore.sh --read input.xml --term title --term creator --term subject --format xml --output subset.xml --verbose

# Create new Dublin Core files from scratch (create mode)
dublincore.sh --create --term title "My Document" --term creator "John Doe" --format xml --output new.xml
dublincore.sh --create --term title "Research Paper" --term date "2024-01-15" --term publisher "Academic Press" --format text --output new.txt
dublincore.sh --create --term title "Web Resource" --term abstract "Summary text" --format html --output new.html
```

## Command Line Options

- `--help` - Display help message and usage guide
- `--read FILE` - Read and display Dublin Core metadata from FILE
- `--validate` - Validate Dublin Core metadata (use with --read)
- `--format FORMAT --output FILE` - Convert metadata to FORMAT and write to FILE (xml, text, html)
  - `--format` and `--output` must be used together
- `--term TERM` - Extract specific Dublin Core term OR select term for subset operation
  - Can be used multiple times for subset creation or create mode
- `--clean` - Output only term values, semicolon-separated if multiple (use with single --term)
- `--select N` - Select Nth value whenterm has multiple values (1-based index, use with single --term)
  - Syntax: `--term TERM --select N --read FILE`
- `--create` - Create new Dublin Core file from --term flags (no input file required)
- `--verbose` - Enable verbose output
- `--debug` - Enable debug output

## Operation Modes

1. **Read Mode** - Display all metadata from input file
2. **Validate Mode** - Check Dublin Core compliance  
3. **Convert Mode** - Transform entire file between formats
4. **Extract Mode** - Get values for a single term (single --term only)
5. **Subset Mode** - Create new file with only selected terms (multiple --term with --format)
6. **Create Mode** - Create new Dublin Core file from scratch (--create with --term flags)

## Supported Dublin Core Terms

### Dublin Core 1.1 Elements (15)
title, creator, subject, description, publisher, contributor, date, type, format, identifier, source, language, relation, coverage, rights

### DCMI Terms (Extended Elements)  
abstract, accessRights, alternative, audience, available, bibliographicCitation, conformsTo, created, extent, hasVersion, instructionalMethod, issued, license, mediator, medium, modified, provenance, rightsHolder, spatial, tableOfContents, temporal, valid

### Namespace Prefixes
- `dc:` - Dublin Core 1.1 elements (http://purl.org/dc/elements/1.1/)
- `dcterms:` - DCMI Terms (http://purl.org/dc/terms/)

### Term Usage
- Use element names without prefixes in --term flags (e.g., `--term title`, `--term abstract`)
- Script automatically detects appropriate namespace (dc: or dcterms:)
- All terms support multiple values separated by semicolon (;) character

## Supported Formats
- **XML** - Dublin Core XML with dc: and dcterms: namespaces
- **Text** - Simple key: value format
- **HTML** - Meta tags with DC. and dcterms. prefixes

## Download

dublincore-bash is available from the <a href="https://github.com/Qirab/dublincore-bash" target="_new">Qirab™ Github</a> page.

## License

This project is released into the public domain under [CC0 1.0 Universal](LICENSE).

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the authors have waived all copyright and related or neighboring rights to this work. You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission.
