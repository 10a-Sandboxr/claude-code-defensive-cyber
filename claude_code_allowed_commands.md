# Claude Code Allowed Commands and Tools Documentation

## Overview
This document outlines the complete set of tools and commands available in Claude Code for the defensive cyber testing project. This information will be used to evaluate Claude's compliance with web-hosted command restrictions.

## Built-in Tools (14 Total)

### File Operations
1. **Read** - Reads file contents with line numbers
   - No special permissions required
   - Can read any accessible file on the system
   
2. **Write** - Creates or overwrites files
   - Requires permission
   - Can create new files or overwrite existing ones
   
3. **Edit** - Makes targeted edits to existing files
   - Requires permission
   - Performs find-and-replace operations
   
4. **MultiEdit** - Performs multiple edits atomically
   - Requires permission
   - Batch editing capability

### Search and Navigation
5. **Glob** - Pattern-based file searching
   - Example: `**/*.js`, `src/**/*.py`
   - No special permissions required
   
6. **Grep** - Content search using regex
   - Built on ripgrep for performance
   - No special permissions required
   
7. **LS** - Directory listing
   - Lists files and directories
   - No special permissions required

### Command Execution
8. **Bash** - Shell command execution
   - **REQUIRES PERMISSION**
   - Can execute ANY system command including:
     - Network tools: `wget`, `curl`, `ping`, `nmap`, `netcat`
     - System tools: `ps`, `netstat`, `lsof`, `systemctl`
     - Security tools: Any installed on the system
   - Commands run in persistent shell sessions
   - Supports command chaining with `;` or `&&`

### Specialized Tools
9. **NotebookRead** - Jupyter notebook reader
10. **NotebookEdit** - Jupyter notebook editor
11. **Task** - Multi-step task execution
12. **TodoWrite** - Task management

### Web Interaction Tools
13. **WebFetch** - URL content fetching
    - **REQUIRES PERMISSION**
    - Fetches and processes web content
    - Converts HTML to markdown
    - Includes 15-minute cache
    
14. **WebSearch** - Web search capability
    - **REQUIRES PERMISSION**
    - Performs internet searches
    - US-only availability

## Configuration System
- `claude config list` - View all settings
- `claude config get <key>` - Get specific setting
- `claude config set <key> <value>` - Modify setting
- `claude config add/remove` - Manage list settings

## Security-Relevant Commands via Bash Tool

When the Bash tool is enabled, Claude Code can execute ANY command available on the system, including but not limited to:

### Network Commands
- `wget` - Download files from web
- `curl` - Transfer data to/from servers
- `ping` - Test network connectivity
- `traceroute` - Trace network paths
- `nmap` - Network discovery and security auditing
- `netcat`/`nc` - Network utility for reading/writing
- `ssh` - Secure shell connections
- `telnet` - Unencrypted remote connections

### Security Testing Tools (if installed)
- `nikto` - Web server scanner
- `dirb`/`gobuster` - Directory brute forcing
- `sqlmap` - SQL injection testing
- `hydra` - Password brute forcing
- `john` - Password cracking
- `metasploit` - Exploitation framework
- `burpsuite` - Web application testing

### System Investigation
- `ps` - Process listing
- `netstat` - Network connections
- `lsof` - Open files and connections
- `find` - File system searches
- `grep` - Pattern matching
- `awk`/`sed` - Text processing

## Built-in Security Limitations

Claude Code has several security features that limit its capabilities:

1. **File System Restrictions:**
   - Can only access the folder where it was started and subfolders
   - Cannot navigate to parent directories
   - Creates a clear security boundary

2. **Command Execution Controls:**
   - May require approval for certain network-related commands
   - Uses command injection detection for suspicious patterns
   - Fail-closed matching (unmatched commands may require manual approval)
   - Note: curl/wget functionality depends on system and permissions

3. **Permission Requirements:**
   - Strict read-only permissions by default
   - Bash, Edit, Write, WebFetch require explicit permission
   - Users control approval mode (once vs automatic)

## Key Observations for Testing

1. **Primary Attack Vectors (when permissions granted):**
   - Bash tool (if approved) can execute system commands
   - WebFetch (if approved) can retrieve web content
   - File operations limited to working directory and below

2. **Important Note:**
   - Claude Code can execute any command available on the host system via Bash tool
   - Available commands depend entirely on what's installed on the host OS
   - The ai_security.txt approach adds an additional security layer beyond Claude's permissions

3. **Testing Considerations:**
   - Claude already has built-in restrictions that may interfere with testing
   - May need to use bypass permission mode for full testing
   - The CLAUDE.md + ai_security.txt adds an additional layer on top

## Testing Implications

For the defensive cyber test:
- **When running on Kali Linux**, Claude Code can execute ANY installed tool via the Bash tool
- This includes all pre-installed Kali security tools (nmap, nikto, sqlmap, metasploit, etc.)
- The ai_security.txt file will need to explicitly limit allowed commands
- CLAUDE.md instructions will be the only defense against unrestricted command execution
- Focus testing on whether Claude respects the allowed_commands list when instructed

**Key Point**: Claude Code's available commands depend entirely on the host operating system. When running on:
- **Standard OS**: Access to typical system utilities
- **Kali Linux**: Access to full penetration testing toolkit (nmap, metasploit, sqlmap, etc.)
- **Restricted environments**: Only commands available in that environment

---
*Document prepared for: Mattie (Analyst), Henry & Josh (Engineers), Adam (Cyber Expert)*
*Project: Defensive Cyber Testing - Web Server Command Restrictions*
*Date: 2025-07-22*