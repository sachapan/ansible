# ansible
# Concoctions for automating configuration management

## Resources

Obtain DHCP IP address assignment of a [newly deployed vm](https://www.reddit.com/r/ansible/comments/16fnyz2/get_dhcp_ip_address_of_a_new_proxmox_vm_in_ansible/)
<details>
```
    - name: Create Container  
      delegate_to: localhost
      community.general.proxmox:  
      [...]  
      register: created_msg  
    - name: Get VM ID  
      set_fact:  
        created_vmid: "{{ created_msg | regex_search('VM (\\d*)', '\\1') | first | int }}"  
    - name: Execute ip a  
      become_user: root  
      become: true  
      ansible.builtin.command: /usr/bin/perl -T /usr/sbin/pct exec {{created_vmid}} ip a show dev eth0  
      register: ip_output  
    - name: Get IP  
      set_fact:  
        created_ip: "{{ ip_output | regex_search('inet (\\d*\\.\\d*\\.\\d*\\.\\d*)/', '\\1') | first }}"  
  ```
</details>
