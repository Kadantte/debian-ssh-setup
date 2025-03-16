
# SSH Remote Access to Debian via Tailscale

Follow these steps carefully to remotely access your Debian system securely via SSH using Tailscale (without public IP):

---

## 🟢 Step 1: Install and Enable SSH Server

Update and install OpenSSH server:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
```

Check SSH status:

```bash
sudo systemctl status ssh
```

Verify SSH listens on port 22:

```bash
sudo ss -tulpn | grep :22
```

You should see output similar to:

```
tcp   LISTEN 0      128      0.0.0.0:22      0.0.0.0:*     users:(("sshd",pid=XXX,...))
```

---

## 🟢 Step 2: Configure SSH to Listen on All Interfaces

Edit SSH config:

```bash
sudo nano /etc/ssh/sshd_config
```

Make sure you explicitly set:

```
ListenAddress 0.0.0.0
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

---

## 🟢 Step 3: Configure Firewall to Allow SSH (Recommended)

Install and configure UFW firewall (if not already configured):

```bash
sudo apt install ufw -y
sudo ufw allow ssh
sudo ufw enable
sudo ufw reload
```

Check firewall status:

```bash
sudo ufw status
```

Ensure it allows SSH access.

---

## 🟢 Step 4: Install and Set Up Tailscale

Install Tailscale on your Debian machine:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

When prompted, authenticate via your browser.

Check Tailscale status:

```bash
tailscale status
```

Ensure your device shows as "active" with an IP address starting with `1.x.x.x`.

---

## 🟢 Step 5: Connect Remotely via SSH

From any remote computer with Tailscale installed, connect using SSH:

```bash
ssh yourusername@<tailscale-ip-address>
```

Replace `<tailscale-ip-address>` with your Debian Tailscale IP (example: `ssh root@0.0.0.0`).

---

## 🚨 Troubleshooting Checklist

If you encounter connection issues:

- ✅ Verify SSH service: `systemctl status ssh`
- ✅ Verify SSH port listening: `sudo ss -tulpn | grep :22`
- ✅ Confirm firewall rules: `sudo ufw status`
- ✅ Confirm Tailscale connectivity: `tailscale status`

---

## 🛡 Security Tips

- Use SSH keys instead of passwords.
- Regularly update your Debian system.

---

You now have secure remote SSH access to your Debian machine from anywhere via Tailscale!
