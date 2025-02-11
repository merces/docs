---
description: Description of 'load' command in HyperDbg.
---

# load \(load the kernel modules\)

### Command

> load

### Syntax

> load \[module name\]

### Description

Loads the HyperDbg's drivers and kernel modules into the target system.

### Parameters

**\[module name\]**

          The name of the module that you want to load

### Modules

| Module Name | Description |
| :--- | :--- |
| vmm | Hypervisor-related capabilities |

{% hint style="info" %}
The debugger functions are implemented on top of '**vmm**' module.
{% endhint %}

**vmm** : this module contains commands related to the debugger and all hypervisor-related capabilities. Currently, **vmm** is the only module of **HyperDbg**.

### Examples

The following example loads `vmm` module.

```text
HyperDbg> load vmm
```

### IOCTL

This command causes to run a `CreateFile` in the target system, in `IRP_MJ_CREATE`, the driver loads the modules like vt-x and debugger and all other kernel modules, so it doesn't have an IOCTL. You can call `CreateFile` to the driver's device and get a handle.

If you're using APIs, the following export in **hprdbgctrl** can be used.

```text
HPRDBGCTRL_API int HyperdbgLoadVmm();
```

### **Remarks**

1. Only one application can get the device handle; after that, no other application is able to create a handle from the device or, in other words, is not able to call `load`command until the first app releases the handle \(by `CloseHandle`\) or call `unload` command. 
2. The application that requests to load the kernel modules should have `SeDebugPrivilege` to obtain a handle, otherwise an `ACCESS_DENIED` is thrown.

This command will continue the debuggee for some time \(in Debugger Mode\). This means that you lose the current context \(registers & memory\) after executing this command.

### Requirements

* Intel VT-x is required to be enabled to perform this action.
* Intel Extended Page Table \(a.k.a. SLAT\) should be present in the processor. If you have a Nehalem \(2008\) processor or a newer processor, then it supports this feature.

### Related

[unload \(unload the kernel modules\)](https://docs.hyperdbg.com/commands/debugging-commands/unload)

