# Configure PortSecurity

Demo role to configure basic port-security as well as generate a compliance report for portsec on a Cisco Catalyst switch.

## Requirements

- cisco.ios
- ansible.netcommon

## Role Variables

- **portsecurity_interfaces**: List of interface names in short and long format to configure port-security.

  **Example**:

  ```yaml
  portsecurity_interfaces:
    - long: "GigabitEthernet1/0/17"
      short: "Gi1/0/17"
    - long: "GigabitEthernet1/0/18"
      short: "Gi1/0/18"
  ```

  Cisco PyATS parser for portsec uses the abbreviated (Gi1/0/1) as opposed to the full name (GigabitEthernet1/0/1). The ios_config module requires full interface name for idempotency so both are listed

## Configuration Template

The example template for port-security configuration is found under roles/configure_portsecurity/templates/ios_portsec.j2. Modify as needed to match your configuration.

## Example Playbook

**Configure PortSec**:

```yaml
- hosts: all
  gather_facts: false

  vars:
    portsecurity_interfaces:
      - long: "GigabitEthernet1/0/17"
        short: "Gi1/0/17"
      - long: "GigabitEthernet1/0/18"
        short: "Gi1/0/18"

  tasks:
    - name: Configure Port Security
      include_role:
      name: config_portsecurity
      tasks_from: ios_portsec_configure
```

Generate PortSec Report:

```yaml
- hosts: all
  gather_facts: false

  vars:
    portsecurity_interfaces:
      - long: "GigabitEthernet1/0/17"
        short: "Gi1/0/17"
      - long: "GigabitEthernet1/0/18"
        short: "Gi1/0/18"

  tasks:
    - name: Run Port Security Report
      include_role:
        name: config_portsecurity
        tasks_from: ios_portsec_report
```
