Here’s the updated version of the `README.md` and steps with the placeholders for customization:

---

# **Pipe POP Node Setup Guide**

This guide walks you through setting up the Pipe POP Node, including automatic startup using `systemd`, and using the referral and reputation systems.

## **Requirements**
- Linux system (Ubuntu recommended)
- `pop` binary downloaded from email

## **Setup Instructions**

### **1. Create the Working Directory**

Create the directory for the `pop` node files and the cache directory:

```bash
mkdir -p /root/pipenetwork
mkdir -p /root/pipenetwork/download_cache
cd /root/pipenetwork
```

### **2. Set Up the Systemd Service**

1. **Create the systemd service file** at `/etc/systemd/system/pipe-pop.service` with the following content:

    ```ini
    [Unit]
    Description=Pipe POP Node Service
    After=network.target
    Wants=network-online.target

    [Service]
    User=root
    Group=root
    WorkingDirectory=/root/pipenetwork
    ExecStart=/root/pipenetwork/pop \
        --ram <Your RAM size> \
        --max-disk <Your server disk max> \
        --cache-dir /root/pipenetwork/download_cache \
        --pubKey <Your Solana Address> \
        --signup-by-referral-route <Your Referral Code>
    Restart=always
    RestartSec=5
    LimitNOFILE=65536
    LimitNPROC=4096
    StandardOutput=journal
    StandardError=journal
    SyslogIdentifier=dcdn-node

    [Install]
    WantedBy=multi-user.target
    ```

    **Explanation of the Parameters**:
    - `--ram <Your RAM size>`: Replace `<Your RAM size>` with the desired RAM allocation in GB (e.g., `8`).
    - `--max-disk <Your server disk max>`: Replace `<Your server disk max>` with the desired disk allocation in GB (e.g., `500`).
    - `--pubKey <Your Solana Address>`: Replace `<Your Solana Address>` with your actual Solana public key.
    - `--signup-by-referral-route <Your Referral Code>`: Replace `<Your Referral Code>` with your actual referral code (e.g., `65dcec6a1eb60cf9`).

---

### **3. Enable and Start the Service**

1. **Reload systemd**:
    ```bash
    sudo systemctl daemon-reload
    ```
2. **Enable the service to start on boot**:
    ```bash
    sudo systemctl enable pipe-pop
    ```
3. **Start the service**:
    ```bash
    sudo systemctl start pipe-pop
    ```

---

### **4. Verify the Service**

To verify that the service is running, use the following command:

```bash
sudo systemctl status pipe-pop
```

---

### **5. Monitoring and Logs**

You can monitor the logs in real-time by running:

```bash
journalctl -u pipe-pop -f
```

---

### **6. Generate Referral Code**

To generate your referral code, use the following command:

```bash
./pop --gen-referral-route
```

This will generate a referral code that you can share with new users who wish to sign up using your referral.

---

### **7. New Users Sign Up Using Referral**

New nodes can sign up using your referral code by running:

```bash
./pop --signup-by-referral-route <REFERRAL_CODE>
```

This allows new users to enter your referral code and join the network under your referral.

---

### **8. Check Reputation and Points**

To check your node's reputation and status, including points from referrals, run:

```bash
./pop --status
```

This will show the breakdown of the node’s reputation metrics and overall score, including:

- **Uptime Score** (40%)
- **Egress Score** (30%)
- **Historical Score** (30%)

---

### **9. Reboot and Auto Start on Boot**

The service is set to start on boot. To test this, simply reboot the system:

```bash
sudo reboot
```

After reboot, check the service status:

```bash
sudo systemctl status pipe-pop
```

---

### **10. Restarting or Stopping the Service**

If you need to restart or stop the service, you can use:

- **Restart the service**:
    ```bash
    sudo systemctl restart pipe-pop
    ```
- **Stop the service**:
    ```bash
    sudo systemctl stop pipe-pop
    ```

---

### **11. FAQ and Notes**

#### **Referral System**:
- Referrers earn 10 points when a referred node:
  - Stays active for 7+ days.
  - Maintains a reputation score > 0.5.
- Nodes with higher reputation scores will have:
  - **Priority for P2P transfers** (score > 0.7).
  - **Eligibility for referral rewards** (score > 0.5).
- The node generating the referral must also maintain a good reputation score for the referral to be valid.

#### **Reputation Score Calculation**:
- **Uptime** (40%): Based on uptime over the past 7 days (maximum score for 168 hours).
- **Egress** (30%): Based on data served in the past 24 hours (maximum score for 1TB+ served).
- **Historical** (30%): Based on consistent reporting over the past 7 days (maximum score for 168 reports).

#### **File Size Limits & Performance**:
- Default RAM: 4GB
- Default disk cache: 100GB
- Chunk size: 64MB per transfer
- Max concurrent downloads controlled by RAM allocation

---

### **Sample `README.md`**

```markdown
# Pipe POP Node Setup Guide

This guide walks you through setting up the Pipe POP Node, including automatic startup using `systemd`, and using the referral and reputation systems.

## Requirements
- Linux system (Ubuntu recommended)
- `pop` binary downloaded from email

## Setup Instructions

### 1. Create the Working Directory

Create the directory for the `pop` node files and the cache directory:

```bash
mkdir -p /root/pipenetwork
mkdir -p /root/pipenetwork/download_cache
cd /root/pipenetwork
```

### 2. Set Up the Systemd Service

1. Create the systemd service file at `/etc/systemd/system/pipe-pop.service` with the following content:
   ```ini
   [Unit]
   Description=Pipe POP Node Service
   After=network.target
   Wants=network-online.target

   [Service]
   User=root
   Group=root
   WorkingDirectory=/root/pipenetwork
   ExecStart=/root/pipenetwork/pop \
       --ram <Your RAM size> \
       --max-disk <Your server disk max> \
       --cache-dir /root/pipenetwork/download_cache \
       --pubKey <Your Solana Address> \
       --signup-by-referral-route <Your Referral Code>
   Restart=always
   RestartSec=5
   LimitNOFILE=65536
   LimitNPROC=4096
   StandardOutput=journal
   StandardError=journal
   SyslogIdentifier=dcdn-node

   [Install]
   WantedBy=multi-user.target
   ```

### 3. Enable and Start the Service

1. Reload systemd:
   ```bash
   sudo systemctl daemon-reload
   ```
2. Enable the service to start on boot:
   ```bash
   sudo systemctl enable pipe-pop
   ```
3. Start the service:
   ```bash
   sudo systemctl start pipe-pop
   ```

### 4. Generate and Use Referral Codes

- To generate your referral code:
   ```bash
   ./pop --gen-referral-route
   ```
- To sign up with a referral code:
   ```bash
   ./pop --signup-by-referral-route <REFERRAL_CODE>
   ```

### 5. Check Your Node's Status

Check your node's reputation and points:
```bash
./pop --status
```

---
