# Power BI Harvest 📊

![Power BI Harvest](https://img.shields.io/badge/Download-Releases-blue?style=for-the-badge&logo=github)

Welcome to the **Power BI Harvest** repository! This project contains a powerful script, `powerbi_harvest.py`, designed for structured exploration and auditing of Power BI workspaces. The script retrieves metadata, datasets, and reports, performs access control checks, and generates human-readable summaries. It also includes optional user enumeration and role visibility features.

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Topics](#topics)
6. [Contributing](#contributing)
7. [License](#license)
8. [Contact](#contact)
9. [Releases](#releases)

## Introduction

Power BI is a powerful tool for business analytics. However, managing and auditing Power BI workspaces can be challenging. `powerbi_harvest.py` simplifies this process by automating the retrieval of critical information. This script is ideal for data analysts, security professionals, and anyone looking to gain deeper insights into their Power BI environments.

## Features

- **Metadata Retrieval**: Access essential information about Power BI workspaces, datasets, and reports.
- **Access Control Checks**: Verify user permissions and roles within your Power BI environment.
- **Human-Readable Summaries**: Generate easy-to-understand reports that summarize key findings.
- **User Enumeration**: Optionally enumerate users within the workspace for detailed access insights.
- **Role Visibility**: Check the roles assigned to users for better access management.

## Installation

To get started with Power BI Harvest, you need to install Python and some required libraries. Follow these steps:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/angpogiko123/powerbi-harvest.git
   cd powerbi-harvest
   ```

2. **Install Dependencies**:
   Make sure you have Python installed. Then, install the required libraries:
   ```bash
   pip install -r requirements.txt
   ```

3. **Download the Script**:
   You can download the latest release of the script from the [Releases section](https://github.com/angpogiko123/powerbi-harvest/releases). Make sure to execute the script after downloading.

## Usage

Once you have installed the script, you can run it with the following command:

```bash
python powerbi_harvest.py --workspace <workspace_id>
```

### Command-Line Options

- `--workspace`: Specify the ID of the Power BI workspace you want to audit.
- `--output`: Define the output file for the summary report (optional).
- `--enumerate-users`: Include this flag to enable user enumeration.
- `--role-visibility`: Include this flag to check user roles.

### Example

To run the script and generate a summary report, use the following command:

```bash
python powerbi_harvest.py --workspace "your_workspace_id" --output "summary_report.txt"
```

## Topics

This repository covers various topics related to Power BI auditing and exploration. Some key topics include:

- **ACL**: Access Control Lists
- **Audit**: Ensuring compliance and security
- **DAX**: Data Analysis Expressions
- **Dump**: Exporting data for analysis
- **Enumeration**: Gathering user information
- **Hacking**: Ethical hacking practices
- **Harvest**: Extracting valuable data
- **Microsoft**: Tools and services from Microsoft
- **Penetration Testing**: Assessing security vulnerabilities
- **Red Teaming**: Simulating attacks to improve security
- **Scanner**: Tools for scanning and auditing
- **Tool**: Utility for Power BI analysis
- **User Enumeration**: Identifying users and roles
- **Vulnerability**: Identifying security weaknesses
- **Web Hacking Tool**: Tools for web security assessments

## Contributing

We welcome contributions to Power BI Harvest! If you would like to contribute, please follow these steps:

1. **Fork the Repository**: Click on the "Fork" button in the top right corner.
2. **Create a New Branch**: 
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make Your Changes**: Implement your changes and test them.
4. **Commit Your Changes**:
   ```bash
   git commit -m "Add your message here"
   ```
5. **Push to Your Fork**:
   ```bash
   git push origin feature/your-feature-name
   ```
6. **Create a Pull Request**: Go to the original repository and create a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contact

For questions or suggestions, please open an issue in this repository. You can also reach out via email at [your_email@example.com].

## Releases

You can find the latest releases of Power BI Harvest [here](https://github.com/angpogiko123/powerbi-harvest/releases). Make sure to download the necessary files and execute them to get started with your Power BI audits.

![Power BI Harvest](https://img.shields.io/badge/Download-Releases-blue?style=for-the-badge&logo=github)

Thank you for your interest in Power BI Harvest! We hope this tool helps you manage and audit your Power BI workspaces effectively.