# DNA Center Sites and Settings with Ansible

This is a sample repository illustrating how to use the `wwt.ansible_dnac` collection to create a site hierarchy and associated network settings.

The inventory used in these examples is part of a lab available from WWT [Network Automation with Ansible + DNA Center Lab](https://www.wwt.com/lab/network-automation-with-ansible-dna-center) but you can modify the inventory and variables for any environment you have access to.

## Setup

If you are testing out this sample on your own environment you will need to tackle a couple of things to get started.

>  **TIP:** Review the README.md for the collection for full installation and dependencies for the collection.

1. Clone this repository

   `git clone https://github.com/jandiorio/dnac_site_settings_example`

2. Install the Python dependencies

   `pip install requests geopy timezonefinder==3.4.2`

3. Install the `wwt.ansible_dnac` collection

   `ansible-galaxy collection install wwt.ansible_dnac`

4. Modify `inventory/hosts.yml` to match your environment

## Creating Sites

This sample playbook demonstrates how you can use the `wwt.ansible_dnac` Ansible collection to create sites on DNA Center.

In DNA Center, there are three different types of sites; area, building, floor.  Each of these types of sites take slighly different input data to create the objects in DNA Center.

1. Open the `vars/dnac_sites.yml` file and review the sample sites data

2. Take note of the data structures for each type of site (area, building, floor)

3. Review the `dnac_site` module documentation

   `ansible-doc wwt.ansible_dnac.dnac_site`

4. Open the playbook `playbooks/create-hierarchy.yml`

5. Review the playbook

6. Execute the playbook `ansible-playbook -i inventory/hosts.yml playbooks/create-hierarchy.yml -e "{'host_groups':'dna_2_dnac','desired_state':'present'}" --ask-vault-pass`

> The **host_groups** extra_var can point to the group or host in your inventory.

DNA Center will require a building address and the latitude/longitude when creating a building.  The playbook illustrates using a lookup plugin to populate the latitude/longitude.  The lookup plugin is located in the `plugins/lookup` directory of the collection.  You could optionally resolve the values manually and include them in your dataset.

## Creating Settings

The network settings section of DNA Center allows you to configure the common settings associated with the places in your network.  These settings might include: timezones, DNS settings, DHCP settings, NTP Servers, and serveral others.  This sample playbook will illustrate how to use the `wwt.ansible_dnac` collection to manage these settings.

1. Open the `vars/dnac_settings.yml`, `vars/dnac_credentials.yml` and the `vars/dnac_timezones.yml` files.

2. Review the format of the YAML data that will be used in the second playbook

3. Open the `playbooks/create-network-settings.yml` playbook

4. Review each task and where there is a new module, review the documentation

   `ansible-doc wwt.ansible_dnac.dnac_ntp`

   > **TIP:**  Replace the module name with the one of interest for the task you are reviewing.

5. Execute the playbook

   `ansible-playbook -i inventory/hosts.yml playbooks/create-network-settings.yml -e "{'host_groups':'dna_2_dnac','desired_state':'present'}" --ask-vault-pass`

   >  **NOTE:** The **host_groups** extra_var can point to the group or host in your inventory.

Each of the network settings can be applied globally or at any level in the hierarchy.  This allows you to override global settings at different levels in the hierarch with the appropriate settings.  This playbooks illustrates applying different settings in different sites using the timezone.

The `dnac_timezone` module also allows you to provide a location or pass in the timezone.  If you pass in a location, a lookup is performed to resolve the location to a timezone using the `timezonefinder` python library.

## Wrap Up

This example use case shows how to utilize the `wwt.ansible_dnac` collection to configure your DNA Center environment from Ansible using the DNA Center as a Platform feature.

Here are some useful links relating to this use case:

This repository is featured on the **Cisco DevNet Code Exchange**.

[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/jandiorio/ansible-dnac-modules)

The webinar below was hosted by Redhat and delivered by Jeff Andiorio of World Wide Technology on 8/7/2018.

[WWT / Redhat Ansible Webinar](https://www.ansible.com/resources/webinars-training/lab-automation-by-wwt-with-ansible-tower-and-cisco-dna-center)

AnsibleFest 2019 Presentation
[DO I CHOOSE ANSIBLE, DNA CENTER OR BOTH?](https://www.ansible.com/do-i-choose-ansible-dna-center-or-both)

Additional slides providing an overview of the modules can be found here:  [Ansible DNA Center Modules Overview](https://www.slideshare.net/secret/1l5xe5ORzTN3Uv)

[Collection on Ansible Galaxy](https://galaxy.ansible.com/wwt/ansible_dnac)

