# 02 — Secure remote access

## Objective

Provide reliable administration from the local network and from external networks without exposing SSH directly through the home router.

## OpenSSH

The OpenSSH server was installed and enabled on Ubuntu. Service state was validated using `systemctl`, and SSH access was tested from a Windows PowerShell client.

Example local connection:

```powershell
ssh jake@<SERVER_LAN_IP>
```

## Host firewall

UFW was enabled with an explicit allowance for OpenSSH:

```bash
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```

This confirmed both IPv4 and IPv6 OpenSSH rules while retaining a default-deny posture for unsolicited inbound traffic.

## Tailscale overlay network

Tailscale was installed on the Ubuntu server and the Windows administration workstation. The `tailscaled` service was verified as enabled and running:

```bash
systemctl status tailscaled
```

After authenticating both devices to the same tailnet, SSH was tested successfully over the Tailscale address from an external network.

## Security decision

No SSH port-forwarding rule was created on the Vodafone router. Tailscale was selected because it provides:

- Encrypted device-to-device connectivity
- NAT traversal without exposing TCP 22 publicly
- Device identity and central revocation
- Stable overlay addresses across different networks
- A smaller externally exposed attack surface

## Recommended ongoing controls

- Require MFA on the Tailscale identity provider
- Review and approve tailnet devices
- Remove unused devices promptly
- Use SSH keys and disable password authentication after recovery access is tested
- Apply least-privilege Tailscale ACLs as the lab grows
- Keep Ubuntu, OpenSSH and Tailscale updated

