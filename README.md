# ☸️ Setting Up a Kubernetes Environment on Google Cloud

This guide walks you through creating a **Kubernetes cluster on GKE**, provisioning a **bastion VM instance**, configuring SSH access, and preparing the machine to interact with the cluster and GitLab.

---

  - [☸️ Creating a Kubernetes Cluster on GKE](#creating-a-kubernetes-cluster-on-gke)  
    - [Step 1 – Access Google Cloud and GKE](#step-1--access-google-cloud-and-gke)  
    - [Step 2 – Start Cluster Creation](#step-2--start-cluster-creation)  
    - [Step 3 – Configure the Cluster](#step-3--configure-the-cluster)  
    - [Step 4 – Create the Cluster](#step-4--create-the-cluster)  

  - [🖥️ Creating the VM Instance (Bastion)](#creating-the-vm-instance-bastion)  
    - [Step 1 – Access VM Instances](#step-1--access-vm-instances)  
    - [Step 2 – Create a New VM](#step-2--create-a-new-vm)  
    - [Step 3 – Configure Region and Settings](#step-3--configure-region-and-settings)  
    - [Step 4 – Deploy the VM Instance](#step-4--deploy-the-vm-instance)  

  - [🔑 Configuring SSH Access](#configuring-ssh-access)  
    - [Step 1 – Generate an SSH Key Pair](#step-1--generate-an-ssh-key-pair)  
    - [Step 2 – Add the Public Key to the VM](#step-2--add-the-public-key-to-the-vm)  
    - [Step 3 – Connect to the VM Using PuTTY](#step-3--connect-to-the-vm-using-putty)  

  - [⚙️ Preparing the VM for Cluster Communication](#preparing-the-vm-for-cluster-communication)  
    - [Step 1 – Switch to Root User](#step-1--switch-to-root-user)  
    - [Step 2 – Update System Packages](#step-2--update-system-packages)  


---

## ☸️ Creating a Kubernetes Cluster on GKE

### Step 1 – Access Google Cloud and GKE
- Log in to **Google Cloud Console**.  
- Search for **GKE – Kubernetes Engine** in the navigation menu.
  
  <details><summary>Click to show details</summary> <img width="1006" height="265" alt="image" src="https://github.com/user-attachments/assets/bcc5ab52-5669-4481-9f4d-61814a00d606" /> </details> 
  
- Open the service.

### Step 2 – Start Cluster Creation
- Click **Create** to begin creating a new Kubernetes cluster.

### Step 3 – Configure the Cluster
- Select the **Standard Cluster** option.
  
  <details><summary>Click to show details</summary> <img width="1185" height="565" alt="image" src="https://github.com/user-attachments/assets/88b5e1ce-ff38-491c-9acf-0e014567b35f" /> </details>
      
- Choose the appropriate **zone**, for example: `southamerica-east1-a`.  
  > ⚠️ This zone will be important later when creating the VM - Bastion.  
- Under **default-pool**, review the node configuration.  
  - By default, Google Kubernetes Engine (GKE) automatically provisions 3 virtual machine (VM) nodes to ensure high availability and load distribution for your workloads.
  

### Step 4 – Create the Cluster
- Review your settings and click **Create**.  
- Wait for the cluster provisioning to complete.

  <details><summary>Click to show details</summary> <img width="1181" height="150" alt="image" src="https://github.com/user-attachments/assets/3a7cf7b6-9d51-4ad4-9667-0de9c3c0b60b" />  </details>
  
---

## 🖥️ Creating the VM Instance (Bastion)

### Step 1 – Access VM Instances
- From the Google Cloud navigation menu, go to **Compute Engine → VM Instances**.
  <details><summary>Click to show details</summary>   <img width="1173" height="445" alt="image" src="https://github.com/user-attachments/assets/f2a19a27-a966-47e3-81fe-d0ea0b2d2eea" />  </details>

### Step 2 – Create a New VM
- Click **Create Instance**.

### Step 3 – Configure Region and Settings
- Select the **same region and zone as your Kubernetes cluster**, such as `southamerica-east1-a`.  
- Configure machine type and settings as needed.
  
  <details><summary>Click to show details</summary> <img width="1169" height="617" alt="image" src="https://github.com/user-attachments/assets/5c84f140-34c3-402b-9c63-bfaa4c773725" /> </details>

### Step 4 – Deploy the VM Instance
- Click **Create**.  
- You now have:
  - ✔️ A **Kubernetes cluster**  
  - ✔️ A **bastion VM** in the same network and zone
  - <details><summary>Click to show details</summary>  <img width="1186" height="287" alt="image" src="https://github.com/user-attachments/assets/521cf990-9f81-46fb-8924-e33badf7f063" /> </details>




This ensures proper communication between the VM and the cluster.

---

## 🔑 Configuring SSH Access

### Step 1 – Generate an SSH Key Pair
- Download and open **PuTTYgen**.  
- Click **Generate** to create a key pair.  
- In the **Key comment** field, set the username (example: `gcp`).  
- This username will be used when connecting.
- Save the Private Key as **`gcp-bootcamp.ppk`**

  <details><summary>Click to show details</summary> <img width="1101" height="701" alt="image" src="https://github.com/user-attachments/assets/7d40b74e-8d83-4d05-9fcd-5f6e93a925f6" /> </details>

### Step 2 – Add the Public Key to the VM
- Copy the generated **public key**.  
- In Google Cloud:
  - Open the VM → **Edit**
    
    <details><summary>Click to show details</summary> <img width="1165" height="366" alt="image" src="https://github.com/user-attachments/assets/78603aec-58ec-45e4-892d-5763f00a3c11" /> </details>

  - Scroll to **SSH Keys** → **Add Item**  
  - Paste the public key.
    
    <details><summary>Click to show details</summary> <img width="906" height="229" alt="image" src="https://github.com/user-attachments/assets/b0beadce-e625-45b3-af5a-5fce3dc058d3" /> </details>

### Step 3 – Connect to the VM Using PuTTY
- Open **PuTTY**.  
- Enter the VM’s external IP.
  
  <details><summary>Click to show details</summary>  <img width="1124" height="633" alt="image" src="https://github.com/user-attachments/assets/43a8c8d2-c478-4438-abb8-0eb37f26d12b" />  </details>

- Set the SSH authentication to use `gcp-bootcamp.ppk`.  
- Log in as the username defined in the key comment (`gcp`).

---

## ⚙️ Preparing the VM for Cluster Communication

### Step 1 – Switch to Root User
After connecting to the VM, run:

```bash
sudo passwd root
su
```

### Step 2 – Update System Packages

Run the following commands:

```bash
apt-get update
apt-get upgrade -y
```

