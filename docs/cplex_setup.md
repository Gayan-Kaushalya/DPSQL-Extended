# CPLEX Full Version Setup Guide

To handle large-scale database workloads (like TPC-H and SSB), you must use the Full Edition of CPLEX. The standard `pip install cplex` is a Community Edition limited to 1,000 variables and 1,000 constraints.

## 1. Download the Installer

*   **Students/Academics:** Register at the [IBM Academic Initiative](https://ibm.biz/academic) using your university email to download CPLEX for free.
*   **Commercial:** Download via the IBM Passport Advantage portal.
*   **Version:** This project is tested with CPLEX Studio 22.1.1 or newer.

## 2. Install the Studio

### Linux (Fedora/Ubuntu)

1. Make the installer executable: `chmod +x cplex_studioXXXX.linux_x86_64.bin`
2. Run with sudo: `sudo ./cplex_studioXXXX.linux_x86_64.bin`
3. Default path: `/opt/ibm/ILOG/CPLEX_StudioXXXX`

### Windows

1. Run the `.exe` installer as Administrator.
2. Default path: `C:\Program Files\IBM\ILOG\CPLEX_StudioXXXX`

## 3. Python API Integration

IBM no longer includes a python folder in the installation. Follow these steps to link the Full Version to your Python environment:

1. **Activate your virtual environment:**
   ```bash
   source .venv/bin/activate  # Linux
   .venv\Scripts\activate     # Windows
   ```

2. **Install the base packages:**
   ```bash
   pip install cplex docplex
   ```

3. **Link to Local Binaries:**
   Run the docplex utility to upgrade your pip installation to the Full Version:

   **Linux:**
   ```bash
   docplex config --upgrade /opt/ibm/ILOG/CPLEX_StudioXXXX
   ```

   **Windows:**
   ```powershell
   docplex config --upgrade "C:\Program Files\IBM\ILOG\CPLEX_StudioXXXX"
   ```

## 4. Verification

Run the following to ensure the unlimited version is active:

```python
import cplex
c = cplex.Cplex()
print(f"CPLEX Version: {c.get_version()}")
# If this succeeds without a "Promotional Version" warning, you are ready.
```