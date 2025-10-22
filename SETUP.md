# Setup Instructions for kali-mcp

This guide explains how to set up and run the Kali Tools MCP Server locally or in a containerized environment.

## Prerequisites
- Python 3.8+
- Docker (recommended)
- Git (to clone the Kali tools documentation repository)

## 1. Clone Kali Tools Documentation
The server expects the Kali tools documentation to be available at `/app/kali-tools-data` (for Docker) or a local path. Clone the repository from GitLab:

```bash
git clone https://gitlab.com/kalilinux/documentation/kali-tools.git kali-tools-data
```

## 2. Install Python Dependencies
Install required Python packages using pip:

```bash
pip install -r requirements.txt
```

## 3. Run the Server Locally
You can start the server using:

```bash
python server.py
```

## 4. Run with Docker
Build and run the Docker container:

```bash
docker build -t kali-mcp .
docker run -it --rm -v $(pwd)/kali-tools-data:/app/kali-tools-data kali-mcp
```

## 5. Claude Desktop Integration
See the README for configuration and integration with Claude Desktop.

## 6. Troubleshooting
- Ensure the `kali-tools-data` directory exists and contains markdown files.
- Check Python and package versions if you encounter errors.
- Debug information is printed to stderr.

## 7. Useful Commands
- Update Kali tools data:
  ```bash
  cd kali-tools-data && git pull
  ```
- Rebuild Docker image:
  ```bash
  docker build -t kali-mcp .
  ```

---
For more details, see README.md.
