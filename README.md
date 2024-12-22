# **How to use the CARLA Python API on Ubuntu**

**Note**: All the following steps were run on Ubuntu 24.04.

To use the CARLA Python API, you need Python 3.7. However, Ubuntu 24.04 uses Python 3.12 by default, which could cause conflicts if you install Python 3.7 directly on the system. To avoid this, we will install Python 3.7 in an isolated directory and create a virtual environment to use whenever you need to run CARLA.

### **Steps to Install and Isolate Python 3.7 on Ubuntu 24.04**

#### **Step 1: Download and Compile Python 3.7 from Source**

This approach installs Python 3.7 in a custom directory, ensuring it doesn't affect the system Python version.

1. **Install Required Build Tools**:
   First, update your package list and install the necessary dependencies to compile Python from source.
   ```bash
   sudo apt update
   sudo apt install -y build-essential zlib1g-dev libncurses5-dev \
       libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev \
       curl libsqlite3-dev wget
   ```

2. **Download Python 3.7 Source Code**:
   Navigate to the `/tmp` directory and download the Python 3.7 source tarball from the official Python website.
   ```bash
   cd /tmp
   wget https://www.python.org/ftp/python/3.7.12/Python-3.7.12.tgz
   ```

3. **Extract the Source Code**:
   Extract the downloaded tarball and navigate to the extracted directory.
   ```bash
   tar -xvf Python-3.7.12.tgz
   cd Python-3.7.12
   ```

4. **Build and Install Python 3.7 in an Isolated Directory**:
   Configure the installation to enable optimizations and specify a custom installation directory (`~/python37`). Then, compile and install Python 3.7.
   ```bash
   ./configure --enable-optimizations --prefix=$HOME/python37
   make -j$(nproc)
   make install
   ```

   This step installs Python 3.7 in `~/python37` to avoid conflicts with the system's Python.

5. **Verify the Installation**:
   Check that Python 3.7 was successfully installed by running the following command:
   ```bash
   ~/python37/bin/python3.7 --version
   ```

---

#### **Step 2: Create a Virtual Environment with Python 3.7**

Now that Python 3.7 is installed, we can create a virtual environment to isolate the dependencies for CARLA.

1. **Create the Virtual Environment**:
   Use the Python 3.7 interpreter to create a new virtual environment.
   ```bash
   ~/python37/bin/python3.7 -m venv carla-env
   ```

2. **Activate the Virtual Environment**:
   Activate the virtual environment so that the Python 3.7 interpreter and dependencies are used within this isolated environment.
   ```bash
   source carla-env/bin/activate
   ```

3. **Check the Python Version in the Environment**:
   Verify that the virtual environment is using Python 3.7.
   ```bash
   python --version
   ```

4. **Install CARLA Requirements**:
   Install the required Python packages for CARLA by pointing to the `requirements.txt` file.
   ```bash
   pip install -r /path/to/requirements.txt
   ```

---

#### **Step 3: Using the Python 3.7 Virtual Environment**

- **Activate the Virtual Environment**:
  Whenever you need to use Python 3.7 for CARLA, activate the environment:
  ```bash
  source carla-env/bin/activate
  ```
- **Run the carla api scripts**:
  for example
  ```bash
  python generate_traffic.py -n 80
  ```

    Note: you might get this error
    ```
    shared libraries: libomp.so.5: cannot open shared object file: No such file or directory
    ```
    it can be solve by installing this package
    ```bash
    sudo apt update
    sudo apt install libomp5
    ```

- **Deactivate the Virtual Environment**:
  When you're done, deactivate the environment to return to the system's default Python:
  ```bash
  deactivate
  ```

---

### **Why This Works**

1. **No System Interference**:
   Python 3.7 is installed in `~/python37`, ensuring that the system Python remains unaffected by the installation.
   
2. **Virtual Environment**:
   The virtual environment isolates your project dependencies, keeping them separate from the system Python packages and allowing for a clean installation of all required libraries.

