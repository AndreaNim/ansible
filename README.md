# Automate creating a VM Instance and a Cluster using Ansible in GCloud
*The goal of this repository is to automate creating a VM Instance and a Cluster*

Prerequisites
-----
1.Create a [Google Cloud Platform Project.](https://console.cloud.google.com/projectselector/compute/instances)

2.In order for ansible to create Compute Engine instances, you'll need a [Service Account](https://cloud.google.com/compute/docs/access/service-accounts#serviceaccount). Make sure to create a new JSON formatted private key file for this Service Account. note the Email address of this Service Account (should be YOUR_SERVICEACCOUNT@YOUR_PROJECT_ID.iam.gserviceaccount.com) since this will be required in the Ansible configuration files.

3.Next  install the [Cloud SDK](https://cloud.google.com/sdk/) and make sure you've successfully authenticated and set default project as instructed.

4.set up SSH keys that will allow you to access your Compute Engine instances. You can either [manually generate the keys](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys#createsshkeys) and paste the public key into the [metadata server](https://console.cloud.google.com/compute/metadata/sshKeys) or you can use gcloud compute ssh to access an existing Compute Engine instance and it will handle generating the keys and uploading them to the metadata server.


Getting Started
------

   **1.Create A [VM instance](https://cloud.google.com/compute/docs/instances/create-start-instance) in Google Compute Engine**
  
  Requirements

   1. python >= v2.6,
   2. requests >= v2.18.4
   3. google-auth >= v1.3.0
   4. ansible<=v2.7
  
  - Install Python
  - Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - Install libcloud
  
    ```
    sudo pip install apache-libcloud==0.20.1
    ```
    
  - Install request and google-auth
    ```
    pip install requests google-auth
    ```
    **Step 1.2: Provide Credentials as Module Parameters for ansible**
    (refer the gce_cluster folder )
    - Edit gce/gce_cluster/cluster_roles/gke/cluster_vars/main.yml  file and specify your Project ID in the project_id variable, Service Account email address in the service_account_email variable, and the location of your JSON key. (downloaded earlier) 
    
    **Step 1.3: Create a host inventory file**
    - Edit /etc/ansible/hosts.
      ```
      [local]
      <INSTANCE_NAME> ansible_user=<USER> ansible_ssh_private_key_file=<FILE_PATH_TO_SSH_KEY>
       ```
    **Step 1.4: Testing the playbook**
    - Use the --syntax-check and -list-tasks options to do a full syntax check.(errors will be show up in red)
       ```
      ansible-playbook --syntax-check --list-tasks -i <inventory file path>./<playbook yml>
       ```
   **2.Creating the kubernetes cluster by executing the playbook**
   
   - Execute the cluster_site.yml. (gce/gke_cluster/cluster_site.yml)
    
      ```
      ansible-playbook  -i etc/ansible/hosts ./cluster_site.yml
      ```