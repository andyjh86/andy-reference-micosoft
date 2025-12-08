# Enabling Nested Virtualisation & MAC Spoofing in Hyper-V

This guide shows how to configure a Hyper-V virtual machine (VM) to support:

- **Nested virtualisation** — required to run Hyper-V inside the VM  
- **MAC address spoofing** — required when nested VMs need network access

### The examples below use a VM named **`vm-example`**. Adjust the name as needed.

---

## 1. Enable Nested Virtualisation

Nested virtualisation allows the VM to run Hyper-V or other hypervisors.

### Command
powershell
Set-VMProcessor -VMName "vm-example" -ExposeVirtualizationExtensions $true

### To Verify

Get-VMProcessor -VMName "vm-example" | Select-Object VMName, ExposeVirtualizationExtensions

### Expected output:
VMName           ExposeVirtualizationExtensions
------           ------------------------------
vmname                 True

## 2. Enable MAC Address Spoofing


### Command
powershell
Set-VMNetworkAdapter -VMName "vm-example" -MacAddressSpoofing On

### To Verify

Get-VMNetworkAdapter -VMName "vm-example" | Select-Object VMName, MacAddressSpoofing

### Expected output

VMName           MacAddressSpoofing
------           ------------------
vm-example                 On

## Why MAC Spoofing Is Needed

Inside your **outer VM** (`vmexample`), you will run **inner VMs**.  
These inner VMs need to send and receive traffic using **their own MAC addresses**, not the MAC of the outer VM.

---

### Without MAC Spoofing

Hyper-V blocks outbound traffic if the **source MAC address** does not match the MAC assigned to the outer VM’s virtual network adapter.

Because of this, the following issues occur:

- Inner VMs cannot access the network  
- DHCP will not work  
- DNS will not resolve  
- Internet access will fail  
- The entire nested Hyper-V environment becomes isolated

Hyper-V enforces this by default as a **security measure** to prevent MAC address impersonation.

