# Ubuntu Server on VirtualBox with NAT and Port Forwarding

This documentation provides step-by-step instructions for setting up an Ubuntu Server inside VirtualBox using NAT networking and configuring port forwarding to allow access from your host machine (laptop).

---

## 📦 Environment Overview

- **Host OS:** Any (e.g., Windows, Linux)
- **Virtualization Software:** VirtualBox
- **Guest OS:** Ubuntu Server (No GUI)
- **Network Mode:** NAT
- **Access Method:** SSH (with port forwarding)

---

## ⚙️ Step-by-Step Setup

### 1. Create and Configure the Ubuntu Server VM

- Install Ubuntu Server ISO on VirtualBox.
- During setup, configure the username and password.
- Keep network adapter as default (**NAT**).

### 2. Set Up Port Forwarding in VirtualBox

1. Open **VirtualBox** and select your Ubuntu Server VM.
2. Click on **Settings** > **Network** > **Adapter 1**.
   - Ensure **Attached to:** `NAT`
   - Click **Advanced** > **Port Forwarding**

3. Add a new rule:

| Name       | Protocol | Host IP    | Host Port | Guest IP | Guest Port |
|------------|----------|------------|-----------|----------|------------|
| SSH Access | TCP      | 127.0.0.1  | 2222      | (blank)  | 22         |

> ✅ You can customize the **Name** (e.g., `Rahim-SSH`) to your preference.
> 🔒 Leave **Guest IP** blank unless you have a static IP configured inside the guest.

4. Save and start your VM.

---

## 🔐 SSH Access from Host

Once the VM is running, open your host terminal and run:

```bash
ssh <username>@localhost -p 2222
```

Replace <username> with the actual user you created inside the Ubuntu Server VM.

Example:

```bash
ssh abrahim@localhost -p 2222
```