# AzureGameHighlightsProcessorTerraform
Here’s a detailed **README** for the **Azure Game Highlight Processor** project that includes all the necessary setup, instructions, and details for using the Terraform automation:

---

# Azure Game Highlight Processor with Terraform

## Overview

The **Azure Game Highlight Processor** is a project that automates the process of fetching **NCAA basketball highlights** using the **RapidAPI Sports Highlights API**, storing the data in **Azure Blob Storage**, and processing the videos for download. This project leverages **Terraform** for automating the provisioning of Azure resources, including the **Storage Account** and **Blob Container**. 

The solution is designed to be scalable and reproducible, with the ability to handle both the fetching of highlight metadata and the processing of video content.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Project Structure](#project-structure)
3. [Getting Started](#getting-started)
4. [Terraform Configuration](#terraform-configuration)
5. [How to Use the Python Scripts](#how-to-use-the-python-scripts)
6. [Automation with Terraform](#automation-with-terraform)
7. [Future Enhancements](#future-enhancements)
8. [License](#license)

---

## Prerequisites

Before running the scripts, make sure you have the following set up:

### 1. **RapidAPI Account**
You’ll need a **RapidAPI** account to fetch basketball game highlights from the **Sports Highlights API**.

- [Sports Highlights API](https://rapidapi.com/highlightly-api-highlightly-api-default/api/sport-highlights-api/playground/apiendpoint_16dd5813-39c6-43f0-aebe-11f891fe5149)

### 2. **Azure Account**
You’ll need an **Azure account** to provision the necessary resources for storing and processing the highlight videos. You can sign up for a free Azure account here:

- [Azure Free Account](https://azure.microsoft.com/en-us/free/)
- [Azure For Students](https://azure.microsoft.com/en-us/free/students)

After logging into your **Azure** account, retrieve your **Subscription ID** by running:

```bash
az account show --query id -o tsv
```

### 3. **Python 3.x**
Make sure **Python 3.x** is installed on your machine. You can check your version with:

```bash
python3 --version
```

### 4. **Git**
Git is required to clone the repository. Install it from [Git](https://git-scm.com/downloads) if not already installed.

---

## Project Structure

The project directory is organized as follows:

```bash
├── .env                        # Environment variables for app configuration
├── .gitignore                  # Files/directories to ignore in Git
├── requirements.txt            # Python dependencies (requests, azure-storage-blob, etc.)
├── config.py                   # Loads environment variables and builds the Azure connection string
├── fetch.py                    # Fetches highlights from RapidAPI and uploads JSON to Azure Blob Storage
├── process_one_video.py        # Downloads JSON, extracts video URL, downloads video, and uploads to Blob Storage
├── run_all.py                  # Orchestrates fetch.py and process_one_video.py sequentially with retry logic
├── update_env.py               # Updates the .env file with Terraform outputs automatically
└── terraform/                  # Terraform configuration for provisioning Azure resources
    ├── main.tf                 # Defines Azure resource group, storage account, and blob container
    ├── variables.tf            # Declares variables for Terraform configuration
    ├── outputs.tf              # Outputs the storage account name, primary key, and container name
    └── terraform.tfvars        # For variable values 
```

---

## Getting Started

Follow these steps to get the project up and running:

### Step 1: Clone the Repository

Clone the repository to your local machine:

```bash
git clone https://github.com/Jepkosgei3/AzureGameHighlightsProcessorTerraform.git
cd src
```

### Step 2: Update `.env` File

Create or modify the `.env` file to include the following environment variables:

```env
RAPIDAPI_KEY=your_rapidapi_key
AZURE_SUBSCRIPTION_ID=your_azure_subscription_id
AZURE_RESOURCE_GROUP=your_resource_group
```

Make sure to replace the placeholders with your actual credentials.

### Step 3: Secure `.env` File

Run the following command to secure your `.env` file:

```bash
chmod 600 .env
```

### Step 4: Update `terraform.tfvars`

Provide specific values for the following variables in `terraform.tfvars`:

```hcl
subscription_id       = "your_azure_subscription_id"
resource_group_name   = "your_resource_group_name"
storage_account_name  = "your_storage_account_name"
container_name        = "your_container_name"
```

### Step 5: Set Up Python Virtual Environment

For macOS/Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

For Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

### Step 6: Install Dependencies

Install the required Python libraries by running:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

---

## Terraform Configuration

### 1. Initialize Terraform

Run the following commands to initialize the **Terraform** working directory:

```bash
terraform init
```

### 2. Preview Changes

Before applying the changes, preview the resources Terraform will create:

```bash
terraform plan
```

### 3. Apply Changes

Apply the configuration to provision the Azure resources:

```bash
terraform apply
```

Type **`yes`** when prompted to confirm the creation of resources.

---

## How to Use the Python Scripts

### 1. **Update `.env` File Automatically**

After Terraform has provisioned the resources, run the following Python script to automatically update the `.env` file with the output values from Terraform:

```bash
python update_env.py
```

This will update the following variables in your `.env` file:

- **AZURE_STORAGE_ACCOUNT_NAME**
- **AZURE_STORAGE_ACCOUNT_KEY**
- **AZURE_BLOB_CONTAINER_NAME**

### 2. **Run the Data Processing Pipeline**

Execute the data pipeline to fetch highlights, download the video, and upload it back to Azure Blob Storage:

```bash
python run_all.py
```

The script will run **`fetch.py`** to fetch highlights and upload the metadata to Azure Blob Storage, and **`process_one_video.py`** will download the metadata, extract the video URL, download the video, and upload it back to the Blob Storage.

---

## Automation with Terraform

Terraform automates the provisioning of the necessary Azure resources, including:

1. **Resource Group** - A logical container for managing Azure resources.
2. **Storage Account** - The Azure storage account where highlight metadata and video files are stored.
3. **Blob Container** - The container within the storage account where video files are uploaded.

---

## Future Enhancements

Here are a few ideas for future improvements to this project:

1. **CI/CD Integration** - Automate the Terraform provisioning and `.env` file update as part of the deployment pipeline.
2. **Video Processing** - Enhance videos using Azure Media Services (currently removed in favor of simpler processing).
3. **Security Enhancements** - Implement better encryption and access controls.
4. **Scalability** - Deploy to Azure Kubernetes Service (AKS) or Azure Container Instances (ACI) for more efficient scaling.
5. **Logging & Monitoring** - Add more robust logging and error-handling mechanisms to track issues and improve monitoring.


---

By following the steps above, you can deploy and automate the **Azure Game Highlight Processor** using **Terraform** and **Python**. Enjoy the seamless automation and cloud management for your game highlight processing pipeline!

---

Let me know if you need any further modifications or additional sections in the README!
