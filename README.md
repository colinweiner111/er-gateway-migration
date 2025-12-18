# Azure ExpressRoute Gateway SKU Migration (Portal)

This document summarizes **which Azure ExpressRoute Gateway SKUs are supported** when using the **Azure portal–based Gateway SKU Migration** method, along with key constraints and links to official Microsoft documentation.

---

## Overview

Azure provides a **gateway migration experience in the Azure portal** that allows you to change the SKU of an existing **ExpressRoute virtual network gateway** *without deleting and recreating the gateway*.

Key characteristics of this migration method:

* ✅ No gateway deletion required
* ✅ ExpressRoute circuit remains connected
* ✅ Supports moving to **equal or higher capacity SKUs only**
* ❌ Downgrades are **not supported**

📘 **Microsoft Docs (Primary Reference):**
[https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal](https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal)

---

## Supported ExpressRoute Gateway SKUs

The following **ExpressRoute virtual network gateway SKUs** are supported as **source and/or target SKUs** when using the migration experience:

### Legacy (Non–Availability Zone) SKUs

* **Standard**
* **HighPerformance**
* **UltraPerformance**

### Availability Zone–Enabled SKUs

* **ErGw1Az** (AZ-enabled equivalent of Standard)
* **ErGw2Az** (AZ-enabled equivalent of HighPerformance)
* **ErGw3Az** (AZ-enabled equivalent of UltraPerformance)

### Scalable SKU

* **ErGwScale**

📘 **Gateway SKU reference:**
[https://learn.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://learn.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)

---

## Supported Migration Paths

When using the portal migration flow, you can migrate to **any equal or higher SKU only**.

### Migration Path Guidelines

The Azure portal allows migration to **any equal or higher performance tier SKU**, including:

* **Legacy to AZ-enabled** (e.g., Standard → ErGw1Az, HighPerformance → ErGw2Az)
* **Within same family** (e.g., ErGw1Az → ErGw2Az → ErGw3Az)
* **To ErGwScale** from any supported SKU
* **Upward within legacy SKUs** (e.g., Standard → HighPerformance → UltraPerformance)

**Performance tiers (generally):**
- **Tier 1:** Standard / ErGw1Az
- **Tier 2:** HighPerformance / ErGw2Az
- **Tier 3:** UltraPerformance / ErGw3Az
- **Scalable:** ErGwScale

> ⚠️ **Note:** The Azure portal's **validation step** during migration will confirm if your specific migration path is supported. Always validate before proceeding.

📘 **Migration behavior and constraints:**
[https://learn.microsoft.com/azure/expressroute/gateway-migration](https://learn.microsoft.com/azure/expressroute/gateway-migration)

---

## Important Limitations

The following actions are **not supported** using this migration method:

* ❌ Downgrading to a lower-capacity SKU
* ❌ Migrating gateways using the **"default" SKU**
* ❌ Legacy gateways created or connected to circuits in **2017 or earlier**
* ❌ Changing gateway type (e.g., ExpressRoute ↔ VPN)
* ❌ Cross-region or cross-subscription migrations
* ❌ GatewaySubnet smaller than **/27**

If a downgrade or unsupported change is required, the gateway must be **deleted and recreated**, which will cause connectivity downtime.

📘 **Full limitations list:**
[https://learn.microsoft.com/azure/expressroute/gateway-migration#limitations](https://learn.microsoft.com/azure/expressroute/gateway-migration#limitations)

---

## Migration Process

The migration follows a **four-step process**:

1. **Validate** - Confirms gateway eligibility and prerequisites
2. **Prepare** - Creates new gateway with desired SKU (up to 45 minutes)
3. **Migrate** - Transfers traffic to new gateway (up to 15 minutes, brief interruption possible)
4. **Commit** - Deletes old gateway and finalizes migration

> 💡 **Tip:** You can abort the migration before committing by reverting traffic back to the original gateway.

📘 **Step-by-step guide:**
[https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal#steps-to-migrate-to-a-new-gateway-in-azure-portal](https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal#steps-to-migrate-to-a-new-gateway-in-azure-portal)

---

## When to Use Gateway SKU Migration

This migration method is ideal when:

* You want to move from **legacy SKUs to AZ-enabled SKUs**
* You need **higher throughput or resiliency**
* You want to adopt **ErGwScale** without downtime
* You want to avoid reconfiguring ExpressRoute circuits
* You need to upgrade before **Basic SKU public IP retirement** (Sept 30, 2025)

---

## References

* **ExpressRoute Gateway SKU Migration (Portal):**
  [https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal](https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal)

* **About ExpressRoute Gateway Migration:**
  [https://learn.microsoft.com/azure/expressroute/gateway-migration](https://learn.microsoft.com/azure/expressroute/gateway-migration)

* **About ExpressRoute Virtual Network Gateways:**
  [https://learn.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://learn.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)

* **About Scalable Gateway (ErGwScale):**
  [https://learn.microsoft.com/azure/expressroute/scalable-gateway](https://learn.microsoft.com/azure/expressroute/scalable-gateway)

* **Troubleshooting Gateway Migration:**
  [https://learn.microsoft.com/azure/expressroute/gateway-migration-error-messaging](https://learn.microsoft.com/azure/expressroute/gateway-migration-error-messaging)

---

## TL;DR

> The Azure portal ExpressRoute Gateway migration supports **Standard, HighPerformance, UltraPerformance, ErGw1Az, ErGw2Az, ErGw3Az, and ErGwScale** SKUs, with migrations allowed **only to equal or higher SKUs**. Downgrades are not supported. The portal validation step confirms your specific migration path.

---

*Last updated: December 2025*
