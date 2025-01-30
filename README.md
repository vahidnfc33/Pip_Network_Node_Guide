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
        --signup-by-referral-route 65dcec6a1eb60cf9
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
