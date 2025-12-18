# Azure ExpressRoute Gateway SKU Migration (Portal)

This document summarizes **which Azure ExpressRoute Gateway SKUs are supported** when using the **Azure portal–based Gateway SKU Migration** method, along with key constraints and links to official Microsoft documentation.

---

## Zero-Cost Zone-Redundant Migration

> **🎉 Important:** Migrating to an Availability Zone-enabled SKU at the **same performance tier is FREE!**

Seamlessly convert your existing ExpressRoute Gateway to zone-redundant at **no additional cost**:

| Current SKU | Target SKU (Zero Cost) | Monthly Cost Change |
|-------------|------------------------|---------------------|
| Standard | **ErGw1Az** | None (✅ FREE) |
| HighPerformance | **ErGw2Az** | None (✅ FREE) |
| UltraPerformance | **ErGw3Az** | None (✅ FREE) |

> ⚠️ **Critical:** When using the migration tool, **select the same performance tier** to avoid additional costs. Selecting a higher SKU (e.g., Standard → ErGw2Az) **will result in higher monthly charges**.

---

## Overview

Azure provides a **gateway migration experience in the Azure portal** that allows you to change the SKU of an existing **ExpressRoute virtual network gateway** *without deleting and recreating the gateway*.

Key characteristics of this migration method:

* ✅ No gateway deletion required
* ✅ ExpressRoute circuit remains connected
* ✅ Supports moving to **equal or higher capacity SKUs only**
* ✅ **Zero cost** when migrating to zone-redundant at same tier
* ❌ Downgrades are **not supported**

📘 **Microsoft Docs (Primary Reference):**
[https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal](https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal)

---

## Prerequisites

Before starting the migration, ensure you have:

- ✅ **Azure Subscription** - Active subscription with appropriate permissions
- ✅ **Existing ExpressRoute Gateway** - Non-AZ-enabled (Standard, HighPerformance, or UltraPerformance)
- ✅ **Gateway Subnet** - Minimum **/27 prefix** or larger
- ✅ **Unlocked Resources** - Remove any resource locks on the gateway and connected resources
- ✅ **Maintenance Window** - Plan migration during scheduled maintenance to minimize impact

> 💡 **Best Practice:** You can prepare the new gateway up to **3 days in advance** and migrate during your maintenance window.

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

## Unsupported Gateway SKUs

The following gateway SKUs are **not supported** when using the ExpressRoute Gateway SKU migration (portal) experience:

### ❌ Not Supported for Migration

**Basic gateway SKU**
- Basic is a VPN gateway–only SKU and is not supported for ExpressRoute.

**VPN-only gateway SKUs (non‑ExpressRoute):**
- VpnGw1, VpnGw2, VpnGw3, VpnGw4, VpnGw5
- VpnGw1Az, VpnGw2Az, VpnGw3Az, VpnGw4Az, VpnGw5Az

**Unsupported downgrade targets**
- Any migration that would result in a lower-capacity SKU (for example, ErGw3Az → ErGw2Az) is blocked.

**Gateway type changes**
- ExpressRoute ↔ VPN gateway type changes are not supported via migration.
---

## Supported Migration Paths

When using the portal migration flow, you can migrate to **any equal or higher SKU only**.

### Migration Path Guidelines

The Azure portal allows migration to **any equal or higher performance tier SKU**, including:

* **Legacy to AZ-enabled (ZERO COST)** - Standard → ErGw1Az, HighPerformance → ErGw2Az, UltraPerformance → ErGw3Az
* **Within same family** (e.g., ErGw1Az → ErGw2Az → ErGw3Az)
* **To ErGwScale** from any supported SKU
* **Upward within legacy SKUs** (e.g., Standard → HighPerformance → UltraPerformance)

**Performance tiers:**
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

### 1. Validate
Confirms gateway eligibility and prerequisites

![Validate Step](https://learn.microsoft.com/en-us/azure/expressroute/media/gateway-migration/validate-step.png)

### 2. Prepare
Creates new gateway with desired SKU (up to 45 minutes)

**Important:** When selecting the target SKU:
- For **zero-cost zone-redundant upgrade**: Select the equivalent AZ-enabled SKU (Standard → ErGw1Az)
- Microsoft will auto-assign a zone-redundant public IP

![Prepare Step](https://learn.microsoft.com/en-us/azure/expressroute/media/gateway-migration/gateway-prepare-update.png)

### 3. Migrate
Transfers traffic to new gateway (up to 15 minutes, brief interruption possible)

![Migrate Traffic Step](https://learn.microsoft.com/en-us/azure/expressroute/media/gateway-migration/migrate-traffic-step.png)

### 4. Commit
Deletes old gateway and finalizes migration

**Before committing:**
- ✅ Validate new gateway connectivity
- ✅ Verify traffic is flowing correctly
- ✅ Confirm no critical issues

![Commit Step](https://learn.microsoft.com/en-us/azure/expressroute/media/gateway-migration/commit-step.png)

> 💡 **Tip:** You can abort the migration before committing by reverting traffic back to the original gateway. You have up to **15 days** to commit after migration.

📘 **Step-by-step guide:**
[https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal#steps-to-migrate-to-a-new-gateway-in-azure-portal](https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal#steps-to-migrate-to-a-new-gateway-in-azure-portal)

---

## When to Use Gateway SKU Migration

This migration method is ideal when:

* You want to move from **legacy SKUs to AZ-enabled SKUs at no cost**
* You need **zone redundancy and automatic failover**
* You need **higher throughput or resiliency**
* You want to adopt **ErGwScale** without downtime
* You want to avoid reconfiguring ExpressRoute circuits
* You need to upgrade before **Basic SKU public IP retirement** (Sept 30, 2025)

### Benefits of Zone-Redundant Gateways

* 🔄 **Automatic failover** during zonal failures
* 📈 **Built-in resiliency** with no additional configuration
* 💰 **No additional cost** when migrating same-tier SKUs
* ⚡ **Consistent performance** across availability zones

---

## References

### Official Microsoft Documentation

* **ExpressRoute Gateway SKU Migration (Portal):**
  [https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal](https://learn.microsoft.com/azure/expressroute/expressroute-howto-gateway-migration-portal)

* **About ExpressRoute Gateway Migration:**
  [https://learn.microsoft.com/azure/expressroute/gateway-migration](https://learn.microsoft.com/azure/expressroute/gateway-migration)

* **About ExpressRoute Virtual Network Gateways:**
  [https://learn.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways](https://learn.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)

* **About Zone-Redundant Gateways:**
  [https://learn.microsoft.com/azure/vpn-gateway/about-zone-redundant-vnet-gateways](https://learn.microsoft.com/azure/vpn-gateway/about-zone-redundant-vnet-gateways)

* **About Scalable Gateway (ErGwScale):**
  [https://learn.microsoft.com/azure/expressroute/scalable-gateway](https://learn.microsoft.com/azure/expressroute/scalable-gateway)

* **Troubleshooting Gateway Migration:**
  [https://learn.microsoft.com/azure/expressroute/gateway-migration-error-messaging](https://learn.microsoft.com/azure/expressroute/gateway-migration-error-messaging)

### Additional Resources

* **🎥 Migration Walk-through Video:**
  Coming soon

---

## TL;DR

> The Azure portal ExpressRoute Gateway migration supports **Standard, HighPerformance, UltraPerformance, ErGw1Az, ErGw2Az, ErGw3Az, and ErGwScale** SKUs, with migrations allowed **only to equal or higher SKUs**. Migrating to zone-redundant at the **same performance tier is FREE** (Standard → ErGw1Az, HighPerformance → ErGw2Az, UltraPerformance → ErGw3Az). Always select the equivalent SKU to avoid additional costs.

---

*Last updated: December 2025*





