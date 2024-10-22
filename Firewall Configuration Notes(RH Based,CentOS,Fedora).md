# Firewall Configuration Notes

The commands for Red Hat-based Linux distributions, primarily those that use firewalld as their firewall management tool. This includes the following distros:
- Red Hat Enterprise Linux (RHEL)
- CentOS
- Fedora
- AlmaLinux
- Rocky Linux
- Oracle Linux
#
1. `Open HTTP/HTTPS Services (Temporary)`
    
    ```
    firewall-cmd --zone=public --add-service=http
    firewall-cmd --zone=public --add-service=https
    ```
  - *OR (if you don't specify a zone, the default zone is used)*
    ```
    firewall-cmd --add-service=http
    firewall-cmd --add-service=https
    ```


2. `Save Running Configuration Permanently`
    ```
    firewall-cmd --runtime-to-permanent
    ```

3. `Show Open Services and Zone Information`

    - *List open services in the `home` zone*
         ```
        firewall-cmd --zone=home --list-services
        ```
    - *Get the default zone*
        ```
        firewall-cmd --get-default-zone
        ```
4. `List All Settings and Zones`

    - *List all services and settings in the `default` zone*
        ```
        firewall-cmd --list-all
        ```
    - *Get a list of `available` zones*
        ```
        firewall-cmd --get-zones
        ```
    - *List all services and settings in the `work` zone*
        ```
        firewall-cmd --zone=work --list-all
        ```
    - *List all services and settings in the `home` zone*
        ```
        firewall-cmd --zone=home --list-all
        ```
5. `Manage Network Interfaces with Zones`

    - *Change interface (e.g., eth0) to be associated with the `home` zone*
        ```
        firewall-cmd --zone=home --change-interface=eth0
        ```
    - *Change interface (e.g., eth0) to be associated with the `public` zone*
        ```
        firewall-cmd --zone=public --change-interface=eth0
        ```
6. `Open and Remove Services (Permanent)`

    - *Open HTTP and HTTPS services permanently in the `public` zone*
        ```
        firewall-cmd --zone=public --add-service=http --permanent
        firewall-cmd --zone=public --add-service=https --permanent
        ```
    - *Remove HTTP and HTTPS services permanently from the `public` zone*
        ```
        firewall-cmd --zone=public --remove-service=http --permanent
        firewall-cmd --zone=public --remove-service=https --permanent
        ```
7. `Restart Firewall Service`

    - *Restart firewalld to apply permanent changes*
        ```
        systemctl restart firewalld
        ```
8. `Open and Remove Specific Ports`

    - *`Open` a specific port (`e.g., 8069/tcp`)*

        ```
        firewall-cmd --add-port=8069/tcp
        ```
    - *`Remove` a specific port (`e.g., 8069/tcp`)*
        ```
        firewall-cmd --remove-port=8069/tcp
        ```