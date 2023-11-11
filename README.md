# wazuh
 ## Download the virtual appliance (OVA)

 - [Download the virtual appliance (OVA)](https://packages.wazuh.com/4.x/vm/wazuh-4.6.0.ova)

 - [https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html](https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html)

 ## Exctract the contents of the file using archiver

 ## Convert the disk to .vhdx (for Hyper-V compatability)

 - I think the machine should be a Generation 2
 - Download [QEMU] (https://qemu.weilnetz.de/w64/)
 - Convert the disk with thefollowing command: ```qemu-img convert -f vmdk -O raw YourDisk.vmdk OutputDisk.vhdx```
 
# Enable Wazuh Vulnerability Scan

1. Copy the original file just in case:

```bash
sudo su
```
```bash
cp /var/ossec/etc/ossec.conf /var/ossec/etc/ossec.conf.original
```
```bash
nano /var/ossec/etc/ossec.conf
```
```bash
<ossec_config>
    <!-- Other configurations -->
    <wodle name="syscollector">
        <disabled>no</disabled>
        <interval>1h</interval>
        <os>yes</os>
        <packages>yes</packages>
        <hotfixes>yes</hotfixes>
    </wodle>
    <!-- Other configurations -->
</ossec_config>
```
```bash
systemctl restart wazuh-manager
```
2. Enable windows scanning

```bash
cp /var/ossec/etc/agent.conf /var/ossec/etc/agent.conf.original
```
```bash
nano /var/ossec/etc/agent.conf
```
```bash
<vulnerability-detector>
  <enabled>yes</enabled>
  <interval>5m</interval>
  <min_full_scan_interval>6h</min_full_scan_interval>
  <run_on_start>yes</run_on_start>
  <!-- Windows OS vulnerabilities -->
  <provider name=\"msu\">
    <enabled>yes</enabled>
    <update_interval>1h</update_interval>
  </provider>
</vulnerability-detector>
```

```bash
systemctl restart wazuh-manager
```

3. Enable Email Alerts

```bash
nano /var/ossec/etc/ossec.conf
```

```bash
<email_notification>
  <smtp_server>your_smtp_server</smtp_server>
  <email_from>wazuh@example.com</email_from>
  <email_to>your_email@example.com</email_to>
  <email_subject>Wazuh Alert
  </email_subject>
</email_notification>
```

```bash
systemctl restart wazuh-manager
```