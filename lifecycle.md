---

copyright:
  years: 2017, 2025
lastupdated: "2025-05-27"

keywords: operating system end of support (eos), add on end of support (eos), vyatta

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Lifecycle for Vyatta Network Operating System on the {{site.data.keyword.cloud_notm}} Virtual Router Appliance
{: #product-lifecycle-vyatta}

In the lifecycle of a product, end of support (EOS) is the last date that {{site.data.keyword.cloud}} delivers standard support for a version or release of a product. The {{site.data.keyword.cloud_notm}} end of support date for Ciena Vyatta Network Operating System (NOS) on {{site.data.keyword.cloud_notm}}'s Virtual Router Appliance (VRA) is aligned with Ciena's EOL date and community support dates. It's also the effective date that the version can no longer be ordered on a new VRA, or reloaded on an existing VRA. Ciena's full support ends on the maintenance support date; maintenance support ends on the EOS date. See the following information for Ciena Vyatta NOS EOS dates.
{: shortdesc}

| Operating system | Code name | Debian | DPDK | Maintenance Support | End of support |
| ----------------- | ---------------- | ---------------- | ---------------- | ---------------- | ---------------- |
| 2308 | Nottingham | 11 | 21.11 | 31 August 2025 | 31 December 2027 |
| 2204 | Kington | 10 | 20.11 | 04 October 2024 | 31 March 2026 |
| 2110 | Jarrow | 10 | 20.11 | 19 October 2023 | 19 April 2024 |
| 2012 | Halifax | 10 | 20.11 | 14 December 2023 | 1 September 2024 |
| 1912 | Edinburgh | 9 | 18.11 | 30 June 2022 | 31 December 2022 |
| 1908 | Dartmouth | 9 | 18.11 | 30 June 2022 | 31 December 2022 |
| 1801 | Yountville | 9 | 17.08 | 29 January 2020 | January 2022 |
{: caption="Lifecycle for Vyatta OS versions" caption-side="top"}

## Ciena support terminology
{: #ciena-support-terms}

Full support
:    For the first two years after a release, or up to the Debian end of life (EOL) date, whichever comes first, Vyatta will be fully supported. Full support means that patches are made available that include fixes for security vulnerabilities and customer requested issues.

Maintenance support
:    After a Vyatta version (released by Ciena) is more than 2 years old, the Vyatta release enters the maintenance support phase. In this phase, patches are restricted to only blocker and critical security vulnerability fixes. This phase is often referred to as end of engineering (EOE). If a bug or non-blocker/critical CVE is found on a version in maintenance support, the path forward is to update to the latest available full support version.

## Latest version availability
{: #latest-version-availability}

Ciena Vyatta NOS is updated regularly, sometimes more than one time per month, and newly released versions are run through a vetting process on {{site.data.keyword.cloud_notm}} hardware by {{site.data.keyword.cloud_notm}} Support before being released to you. You can find information on the latest available versions in [Vyatta 5600 vRouter software patches](/docs/virtual-router-appliance?topic=virtual-router-appliance-ciena-vyatta-5600-vrouter-software-patches). 2204h has been released and is available through OS reload and the provisioning process. You can also use the information in [Upgrading the OS](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os) to update Vyatta NOS version on the VRA using the Vyatta command line interface. You can also use the information in [Resolving upgrade issues](/docs/virtual-router-appliance?topic=virtual-router-appliance-upgrade-issues) to find help when an upgrade fails.
