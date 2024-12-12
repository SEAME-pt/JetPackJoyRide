
# ADR-0004: Automating CAN Interface Setup with Systemd

## Status
Accepted

## Context
The JetRacer project requires a method to automatically configure the CAN interface (`can0`) upon boot. The interface needs to be initialized with the correct bitrate and ensure it is set up properly for communication between the components. The configuration involves using the MCP2515 CAN controller and an associated systemd service to ensure the interface is up and ready before the system starts interacting with CAN-enabled peripherals.

The key considerations are:

- **System Initialization**: The CAN interface must be automatically initialized on boot, without requiring manual intervention.
- **Configuration Consistency**: The setup script needs to correctly configure the CAN interface with the desired bitrate and other parameters.
- **Reliability**: The service should start reliably and be capable of recovering from failures.
- **Security and Permissions**: Proper permissions must be set for the script, and it should be executable by the systemd service to ensure automated execution.

## Decision
The solution for automating the setup of the CAN interface will involve the following steps:

1. **CAN Interface Configuration**:  
   The CAN interface will be configured using a shell script (`setup_can.sh`), which will be triggered by a systemd service (`can_setup.service`). The script will configure the MCP2515 CAN controller with the correct bitrate and initialize the interface on boot.

2. **Systemd Service**:  
   A systemd service will be created to run the configuration script at boot. The service file will be placed in `/etc/systemd/system/` and will run the shell script as a startup process, ensuring the CAN interface is up and ready before other services or components rely on it.

3. **Permissions Handling**:  
   The shell script will be given executable permissions via `chmod +x` to ensure it can be run by the systemd service.  
   The service will be set to run with `sudo` to ensure the necessary permissions to configure network interfaces are granted.

4. **Service and Script Execution**:  
   The `setup_can.sh` script will configure the `can0` interface using the following command, which will be part of the systemd service:
   
   `sudo ip link set can0 up type can bitrate 500000`

   Additionally, the script will include the `dtoverlay` configuration for the MCP2515 controller in `config.txt`:
   
   `dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25`

5. **Error Handling**:  
   The systemd service will be monitored for failures. In case the service fails to start or the script encounters an error, logs will be captured using `journalctl` to identify and troubleshoot the issue.

6. **Systemd Service Configuration**:  
   The systemd service file (`can_setup.service`) will be configured with the following details:
   
   ```ini
   [Unit]
   Description=Setup CAN interface (can0)
   After=network.target
   
   [Service]
   ExecStart=/home/jetpack/SEAME-Cluster-24-25/startup_scripts/setup_can.sh
   Restart=on-failure
   User=jetpack
   
   [Install]
   WantedBy=multi-user.target
´´´


## Benefits
The CAN interface will be automatically configured and initialized on boot without manual intervention, streamlining the startup process.
Systemd ensures that the setup is reliable and persistent across reboots.
Logs from journalctl will provide valuable insights for debugging in case of failures.
Configuration changes can be easily made in the script or systemd service file, allowing for flexible adjustments.
```
