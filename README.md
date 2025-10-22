# Kali Tools MCP Server

A Model Context Protocol (MCP) server that provides comprehensive access to Kali Linux Tools documentation through Claude Desktop. Easily search, explore, and retrieve detailed information about over 600 penetration testing and security tools included in Kali Linux.

## What is Kali Linux?
Kali Linux is the world's leading penetration testing platform, featuring hundreds of tools for security testing, digital forensics, and reverse engineering. This MCP server brings the full Kali toolset documentation directly into your Claude Desktop environment.

## ✨ Features
- 🔍 **Smart Search:** Find tools by name, functionality, or category across 600+ Kali tools
- 📋 **Category Organization:** Browse tools by security domain (web apps, forensics, wireless, etc.)
- 📖 **Detailed Documentation:** Access installation instructions, usage examples, and command syntax
- 🎯 **Usage Examples:** Real command-line examples with explanations
- 🐳 **Dockerized:** Simple deployment with up-to-date data from the official repository
- 🍎 **Apple Silicon Optimized:** Native ARM64 support for M1/M2/M3 Macs
- 🚀 **On-Demand:** Loads fresh data for every session

## Use Cases
- **Penetration Testing:** Quick reference for tools during security assessments
- **Security Research:** Discover the right tool for specific testing scenarios
- **Certification Prep (OSCP/CEH):** Learn about tools used in ethical hacking certifications
- **Red Team Operations:** Find specialized tools for various attack vectors
- **Blue Team Defense:** Understand attacker tools to improve defenses
- **Education:** Comprehensive learning resource for cybersecurity students
- **CTF Competitions:** Fast lookup of available tools and their capabilities

## Tool Categories
- Information Gathering: DNS enumeration, port scanning, reconnaissance
- Vulnerability Analysis: Vulnerability scanners, exploit databases
- Web Applications: SQL injection, XSS testing, web crawlers
- Database Assessment: Database-specific security tools
- Password Attacks: Hash cracking, brute force, wordlist generation
- Wireless Attacks: WiFi security testing, Bluetooth analysis
- Reverse Engineering: Binary analysis, debugging, disassembly
- Exploitation Tools: Exploit frameworks, payload generators
- Forensics: Digital forensics, data recovery, memory analysis
- Sniffing & Spoofing: Network monitoring, packet capture, MITM
- Post Exploitation: Privilege escalation, persistence, lateral movement
- Reporting Tools: Documentation and report generation

## Quick Start

### Prerequisites
- Docker installed and running
- Claude Desktop application
- Apple Silicon Mac (M1/M2/M3) or compatible system

### 1. Set Up Project
```bash
# Create project directory
mkdir kali-tools-mcp-server
cd kali-tools-mcp-server

# Copy the following files to this directory:
# - Dockerfile
# - requirements.txt
# - server.py
```

### 2. Build Docker Image
```bash
# Build the image for Apple Silicon (includes web scraping, takes 3-5 minutes)
docker build --platform linux/arm64 -t kali-tools-mcp-server .
```

### 3. Test the Build
```bash
docker run --rm --platform linux/arm64 kali-tools-mcp-server python -c "
import sys; sys.path.append('/app')
from server import KaliToolsServer
server = KaliToolsServer()
print(f'✅ Successfully loaded {len(server.tools_data)} Kali tools')
print(f'Categories: {list(server.categories.keys())}')
"
```

### 4. Configure Claude Desktop
Edit your Claude Desktop configuration file:

Location: `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "kali-tools": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "--platform",
        "linux/arm64",
        "kali-tools-mcp-server",
        "python",
        "/app/server.py"
      ],
      "env": {}
    }
  }
}
```

### 5. Restart Claude Desktop
Completely quit and restart Claude Desktop to load the new MCP server.

## Usage Examples
Once configured, interact with the Kali Tools database directly through Claude Desktop:
- **Search for Tools:** "What Kali tools are available for SQL injection testing?"
- **Get Tool Details:** "Get detailed information about sqlmap"
- **Browse by Category:** "List all web application testing tools"
- **Usage and Examples:** "Show me sqlmap usage examples"

## MCP Server Tools
The MCP server provides five main tools:
1. `search_kali_tools` — Search the Kali tools database by name, description, or functionality
2. `get_tool_details` — Get comprehensive information about a specific tool
3. `list_tools_by_category` — List all tools in a specific security category
4. `get_tool_usage` — Get usage examples and command syntax for a specific tool
5. `list_categories` — Show all available tool categories with tool counts

## Project Structure
```
kali-tools-mcp-server/
├── Dockerfile              # Container with Kali tools GitLab repository
├── requirements.txt        # Python dependencies (MCP, PyYAML)
├── server.py               # Main MCP server with PackagesInfo parsing
├── build.sh                # Automated setup script
└── README.md               # This documentation
```

## Architecture
- **Repository Cloning:** Clones official Kali tools documentation from GitLab during build
- **PackagesInfo Parsing:** Extracts tool descriptions from PackagesInfo sections in markdown files
- **Smart Categorization:** Automatically categorizes tools by functionality using keyword analysis
- **Content Extraction:** Parses YAML frontmatter, usage examples, and installation instructions
- **MCP Interface:** Provides search and query capabilities through MCP tools
- **On-Demand Execution:** Runs fresh containers for each Claude Desktop session

## Data Sources
- Official Kali Linux tools documentation from GitLab repository
- Individual tool directories containing index.md files
- PackagesInfo sections, YAML frontmatter, usage examples
- Intelligent categorization based on tool descriptions and functionality

## Key Parsing Features
- **PackagesInfo Detection:** Automatically finds and extracts content following "PackagesInfo:" sections
- **YAML Frontmatter:** Extracts title, homepage, repository, and other metadata
- **Content Cleaning:** Filters out Hugo shortcodes and HTML comments

## Troubleshooting
- **Build takes longer than expected:** The build includes cloning the full Kali tools GitLab repository
- **Limited tools loaded:** Check that the GitLab repository was cloned successfully
- **Description parsing issues:** Most tools should have meaningful descriptions from PackagesInfo sections

## License & Attribution
This project is for educational and legitimate security research purposes.
- Kali Tools Data: Subject to Kali Linux documentation license
- GitLab Repository: https://gitlab.com/kalilinux/documentation/kali-tools
- MCP Server Code: Educational use with responsible security practices
- Docker Configuration: Freely usable for legitimate security research

---
For setup instructions, see `setup.md`.