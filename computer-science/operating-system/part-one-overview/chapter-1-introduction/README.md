# Chapter 1 Introduction

## 1.1 What Operating Systems Do

A computer system can be divided roughly into four components: the **hardware,** the **operating system,** the **application programs,** and a **user** \(Figure 1.1\).

![Figure 1.1  Abstract view of the components of a computer system.](../../../../.gitbook/assets/figure-1.1.png)

{% hint style="info" %}
We can also view a computer system as consisting of hardware, software and data.
{% endhint %}

### 1.1.1 User View

The operating system is designed mostly for **ease of use**, with some attention paid to performance and security and none paid to **resource utilization.**

{% hint style="warning" %}
Some computers have little or no user view. For example, **embedded computers**.
{% endhint %}

### 1.1.2 System View

From the computer's points of view, the operating system is a **resource allocator** or a **control program** that manages the execution of user programs to prevent errors and improper user of the computer.

### 1.1.3 Defining Operating Systems

In general, we have no completely adequate definition of an operating system. 

In addition, we have no universally accepted definition of what is part of the operating system.

{% hint style="warning" %}
**Note:**

**A common definition:**

* **Kernel:** The one program running at all times.
* **System Programs:** Associated with the operating system but not necessarily part of the **kernel**.
* **Application Programs:** All programs not associated with the operating system.
{% endhint %}

## 1.2 Computer-System Organization

One or more CPUs, device controllers connect through common **bus** providing access to shared memory.

{% hint style="info" %}
There may be many buses with a computer system, but the system bus is the main communications path between the major components.
{% endhint %}

Concurrent execution of CPUs and devices competing for memory cycles.

Each device controller has a local buffer and each type has an operating system **device driver** to manage it.

![Figure 1.2  A typical PC computer system.](../../../../.gitbook/assets/image.png)

### 1.2.1 Interrupts

Device controller informs CPU that it has finished its operation by causing an **interrupt**. Interrupt is a key part of how operating system and hardware interact.

#### 1.2.1.1 Overview

![Figure 1.3  Interrupt timeline for a single program doing output.](../../../../.gitbook/assets/os-figure-1.3.png)

Interrupt transers control to the interrupt service routine generally, through the **interrupt vector**, which contains the addresses of all the service routines.

The interrupt architecture must also save the state information of whatever was interrupted.

#### 1.2.1.2 Implementation

When the CPU detects that a controller has asserted a signaal on the **interrupt-request line**, it reads the interrupt number and jumps to the **interrupt-handler routine** by using that interrupt number as an index into the interrupt vector.

{% hint style="info" %}
The CPU hardware has a wire called the **interrupt-request line** that the CPU senses after executing every instruciton.

We say that the device controller _**raises**_ an interrupt by asserting a signal on the interrupt request line, the CPU _**catches**_ the interrupt and _**dispatches**_ it to the interrupt handler, and the handler _**clears**_ the interrupt by servicing the device.
{% endhint %}

{% hint style="warning" %}
**Note:**

In practice, computers have more devices that they have address elements in the interrupt vector.

A common way to solve this problem is to use **interrupt chaining**, in which each element in the interrupt vector points to the head of a list of interrupt handlers.

When an interrupt is raised, the handlers on the corresponding list are called one by one, until one is found that can services the request.
{% endhint %}



_**Most CPUs have two interrupt request lines:**_

1. **The nonmaskable interrupt**, which is reserved for events such as unrecoverable memory errors.
2. **Maskable:** it can be turned off by the CPU before the execution of critical instruction sequences that must not be interrupted. It's used by device controllers to request services.

![Figure 1.4. Interrupt-driven I/O cycle.](../../../../.gitbook/assets/os-figure-1.4.png)

{% hint style="info" %}
**In modern computer hardware, we need more sophisticated interrupte-handling features:**

1. We need **the ability to defer interrupt handling** during critical processing.
2. We need **an efficient way to dispatch** to the proper interrupt handler for a device.
3. We need **multilevel interrupts**, so that the operating system can distinguish between high- and low-priority interrupts and can respond with the appropriate degree of urgency.

These thress features are provided by the **CPU** and the **interrupt-controller hardware**.
{% endhint %}

### 1.2.2 Storage Structure



