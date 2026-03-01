# Automated Target-File Exfiltration Detection via Disk and Network Forensics

Automated prototype for detecting sensitive file exfiltration by correlating artifacts from disk images and captured network traffic.

This is an educational project and has no intentions to turn into a production-level tool (at least for now).
Developed as part of Master Thesis in Cybersecurity for Ivan Franko University of Lviv in 2025.
Do not hesitate to contact for full thesis/documentation using any possible option.

### Input requirements:
- Disk image
- CSV-file with list of files and their filetypes, hashes OR the directory with the files themselves
- Captured Network traffic PCAP file  (Optional)

## Overview

This tool aims to identify when predefined sensitive files have been exfiltrated (copied, modified, or deleted, and transferred) from a system. It combines:
- Disk forensics to locate original, modified, or residual file artifacts
- Network traffic analysis to detect potential transfers (including selected encrypted patterns)
- Machine learning for improved detection of suspicious flows

The approach is research-oriented and tested on controlled datasets.

## Key Features

- File identification using cryptographic and fuzzy hashing (detects exact matches, modifications, deletions)
- Disk image processing to extract artifacts from forensic images
- Network interface analysis (physical and virtual) for transfer detection
- Machine learning classification on selected encrypted traffic features
- Parallel processing to handle large-scale forensic datasets efficiently

## Project Structure

```bash
exfiltration_forensics_tool/ \
├── disk/              # Disk image analysis modules (hashing, artifact extraction) and top-level logic with UI 
├── network/           # Network traffic parsing and ML-based detection 
├── utils/             # Shared utilities (helpers, config, logging, etc.) 
├── requirements.txt   # Python dependencies 
└── README.md          # This file 
```

## Tech Stack

- **Language**: Python 3
- **Forensics**: The Sleuth Kit / Autopsy (disk), Wireshark (network)
- **Hashing**: cryptographic (SHA/MD5/etc.) + fuzzy (ssdeep)
- **Machine Learning**: Scikit-learn / Keras for pattern classification
- **Performance**: multiprocessing / parallel computing
- **Other**: Pandas/numpy for data handling, other minor utilities

See `requirements.txt` for the full list.

## Installation

```bash
git clone https://github.com/peacedetskiy/exfiltration_forensics_tool.git
cd exfiltration_forensics_tool
pip install -r requirements.txt
```

## Usage

Run the tool with:

```bash
python main.py [options]
```

Most parameters have default values defined in config.py. Override only the ones needed for your specific case.

### Available Options

- -i, --image \
Path to the forensic disk image (.dd or .img format) \
Default: from config.IMAGE_PATH \

- -f, --file-list \
Path to CSV file with the list of target (sensitive) files to detect \
Default: from config.FILE_LIST \

- -m, --metadata-csv \
Output path for CSV containing extracted file metadata \
Default: from config.METADATA_CSV \

- -r, --registry-json \
Output path for JSON file with relevant registry artifacts \
Default: from config.REGISTRY_JSON \

- -p, --pcap \
Path to network traffic capture file (.pcapng) \
Default: from config.PCAP_PATH \

- -n, --network-report-dir \
Directory where network analysis output will be saved \
Default: from config.NETWORK_REPORT_DIR \

- -s, --src-ip \
Source IP address of the suspected machine (used to filter traffic) \
Default: from config.SRC_IP \

- -o, --report-path \
Path to save the final summary text report \
Default: from config.REPORT_PATH \

- -d, --matches-dir \
Directory to store hash match results and related files \
Default: from config.MATCHES_DIR \

- -z, --extracted-zip \
Path where extracted matching files will be zipped and saved \
Default: from config.EXTRACTED_ZIP \

- --pre-input-dir \
Directory containing reference files to pre-compute hashes from \
Default: from config.PRE_INPUT_DIR \

- --pre-input-zip \
Zip file containing reference files to pre-compute hashes from \
Default: from config.PRE_INPUT_ZIP \

### Example

```bash
python main.py -i evidence.dd -f sensitive_files.csv -p traffic_capture.pcapng -s 192.168.1.50 -o output/report.txt
```
