---
title: "Virtuális gépek telepítésének SAP NetWeaver aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy SAP szoftverek a Linux virtuális gépek Azure-ban."
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 1c4f1951-3613-4a5a-a0af-36b85750c84e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.openlocfilehash: 42f1d8a3eff51e113729dbfe2848b67deaf06936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-deployment-for-sap-netweaver"></a>SAP NetWeaver Azure virtuális gépek központi telepítését
[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2367194]:https://launchpad.support.sap.com/#/notes/2367194

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md (Azure Virtual Machines DBMS deployment for SAP)
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Caching for VMs and VHDs)
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID)
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure Storage)
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (Structure of a RDBMS deployment)
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (High availability and disaster recovery with Azure VMs)
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 and later)
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 and earlier releases)
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (Using a SQL Server image from hello Azure Marketplace)
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (General SQL Server for SAP on Azure summary)
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (Specifics tooSQL Server RDBMS)
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (Storage configuration)
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d (Backup and restore)
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (Performance considerations for backup and restore)
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (Other)
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md (Azure Virtual Machines deployment for SAP)
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resources)
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (Deploying a VM by using a custom image)
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (Scenario 1: Deploying a VM from hello Azure Marketplace for SAP)
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (Scenario 2: Deploying a VM with a custom image for SAP)
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 (Scenario 3: Moving a VM from on-premises using a non-generalized Azure VHD with SAP)
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (Deployment scenarios of VMs for SAP on Microsoft Azure)
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (Deploying Azure PowerShell cmdlets)
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Download and import SAP-relevant PowerShell cmdlets)
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Join a VM tooan on-premises domain - Windows only)
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux)
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (Download, install, and enable hello Azure VM Agent)
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell)
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI)
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Configure hello Azure Enhanced Monitoring Extension for SAP)
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Readiness check for Azure Enhanced Monitoring for SAP)
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Health check for hello Azure monitoring infrastructure)
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Troubleshooting Azure monitoring for SAP)

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure monitoring)
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure hello proxy)
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and troubleshooting for setting up end-to-end monitoring)

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md (Azure Virtual Machines planning and implementation for SAP)
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resources)
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (High availability and disaster recovery for SAP NetWeaver running on Azure Virtual Machines)
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (High availability for SAP Application Servers)
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (Using Autostart for SAP instances)
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 (Cloud-only - Virtual Machine deployments in Azure without dependencies on hello on-premises customer network)
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (Cross-premises - Deployment of single or multiple SAP VMs in Azure fully integrated with hello on-premises network)
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regions)
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (Fault domains)
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domains)
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 (Azure availability sets)
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtual machines concept)
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Moving a VM from on-premises tooAzure with a non-generalized disk)
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (Deploying a VM with a customer specific image)
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Preparation for moving a VM from on-premises tooAzure with a non-generalized disk)
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Preparation for deploying a VM with a customer specific image for SAP)
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Preparing VMs with SAP for Azure)
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (Difference between an Azure disk and an Azure image)
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (Uploading a VHD from on-premises tooAzure)
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Copying disks between Azure Storage accounts)
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD structure for SAP deployments)
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (Setting automount for attached disks)
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (Single VM with SAP NetWeaver demo/training scenario)
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (Concepts of Cloud-Only deployment of SAP instances)
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure Monitoring Solution for SAP)
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)
[planning-guide-managed-disks]:planning-guide.md#c55b2c6e-3ca1-4476-be16-16c81927550f (Managed Disks)
[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and data disks)

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-os-disk-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk-md%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image-md%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-md%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image-md%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../../storage/common/storage-premium-storage.md
[storage-redundancy]:../../../storage/common/storage-redundancy.md
[storage-scalability-targets]:../../../storage/common/storage-scalability-targets.md
[storage-use-azcopy]:../../../storage/common/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../../linux/attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md (Manage virtual machines by using Azure Resource Manager and PowerShell)
[virtual-machines-windows-agent-user-guide]:../../windows/agent-user-guide.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../linux/sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:./../../windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./../../windows/sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./../../windows/sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:../../virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:../../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Az Azure virtuális gépek egy hello megoldás olyan szervezeteknek, amelyek a számítási és tárolási erőforrásokat, szükséges minimális időben, és hosszabb beszerzési ciklusok nélkül. Használhatja az Azure virtuális gépek toodeploy klasszikus alkalmazások, például SAP NetWeaver-alapú alkalmazások az Azure-ban. Az alkalmazás megbízhatósági és rendelkezésre állás további helyszíni erőforrások nélkül kiterjesztése. Az Azure virtuális gépek közötti kapcsolatot nyújthassanak, támogatja, így a Azure Virtual Machines integrálása a szervezete helyszíni tartományok, magánfelhőket és SAP rendszer fekvő.

Ez a cikk azt hello lépéseket toodeploy SAP-alkalmazást a virtuális gépek (VM) az Azure, a másik központi telepítési beállítások és hibaelhárítási fedik le. Ez a cikk épít, hello információk [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]. Kiegészíti is SAP dokumentáció és az SAP, hello elsődleges erőforrások telepítéséhez és az SAP szoftverek telepítését.

## <a name="prerequisites"></a>Előfeltételek
Az SAP szoftver központi telepítése Azure virtuális gép beállítása magában foglalja a több lépéseket és erőforrásokat. Mielőtt elkezdené, győződjön meg arról, hogy teljesülnek-e hello telepítéséhez szükséges előfeltételek SAP szoftver telepítése az Azure virtuális gépeken.

### <a name="local-computer"></a>Helyi számítógép
Windows vagy Linux rendszerű virtuális gépek toomanage, használhatja a PowerShell-parancsfájl és hello Azure-portálon. Mindkét eszközök van szüksége a Windows 7 vagy újabb verziója a Windows rendszert futtató számítógépről. Ha toomanage kizárólag az Linux virtuális gépek és azt szeretné, a Linux-számítógép toouse ehhez a művelethez az Azure parancssori felület is használhatja.

### <a name="internet-connection"></a>Internetkapcsolat
toodownload és futtatási hello eszközök és parancsfájlok, amelyek szükségesek a SAP szoftverek központi telepítéséhez, kell lennie internetes toohello csatlakoztatva. hello hello Azure fokozott Figyelőbővítmény az SAP futtató Azure virtuális gép access toohello Internet kell. Ha hello Azure virtuális gép, egy Azure-beli virtuális hálózat vagy a helyi tartomány, győződjön meg arról, hogy hello vonatkozó proxybeállítások vannak beállítva, a [hello proxybeállítások][deployment-guide-configure-proxy].

### <a name="microsoft-azure-subscription"></a>Microsoft Azure-előfizetés
Egy aktív Azure-fiókra van szüksége.

### <a name="topology-and-networking"></a>Topológia és a hálózatkezelés
Toodefine hello topológia és hello SAP üzemelő Azure architektúrájának lesz szüksége:

* Az Azure storage-fiókok toobe használt
* Ha azt szeretné, hogy toodeploy hello SAP rendszer virtuális hálózati
* Erőforrás-csoport toowhich toodeploy hello SAP rendszer
* Azure-régió, ahol azt szeretné, hogy toodeploy hello SAP rendszer
* SAP-konfiguráció (kétrétegű és háromrétegű)
* Virtuálisgép-méretek és a további adatok lemezek toobe hello száma csatlakoztatva toohello virtuális gépek
* SAP javítása és a szállítási rendszer (CTS) konfigurálása

Hozzon létre, és konfigurálja az Azure storage-fiókok (ha szükséges) vagy egy Azure virtuális hálózatot hello SAP szoftvertelepítési folyamat megkezdése előtt. További információ toocreate és ezeket az erőforrásokat konfigurálása, lásd: [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide].

### <a name="sap-sizing"></a>SAP méretezése
A következő információkat, a SAP méretezési hello tudnia:

* SAP-munkaterhelések, például tervezett hello SAP gyors Szimbólumméretező eszközt, és hello SAP alkalmazás teljesítmény szabványos (SAP) száma
* Szükséges CPU erőforrás és a memória-felhasználás hello SAP rendszer
* Kötelező bemeneti/kimeneti (I/O) műveletek másodpercenkénti száma
* A végleges kommunikáció az Azure-ban futó virtuális gépek közötti hálózati sávszélesség szükséges.
* A helyszíni eszközök és hello Azure telepített SAP rendszer közötti szükséges hálózati sávszélesség

### <a name="resource-groups"></a>Erőforráscsoportok
Az Azure Resource Manager segítségével is erőforrás csoportok toomanage hello összes alkalmazás-erőforrásokat az Azure-előfizetéshez. További információkért lásd: [Azure Resource Manager áttekintése][resource-group-overview].

## <a name="resources"></a>Erőforrások

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>SAP-erőforrások
Az SAP szoftvertelepítést állít be, amikor szüksége hello SAP erőforrások a következő:

* SAP Megjegyzés [1928533], amelynek van:
  * Hello SAP szoftver központi telepítése által támogatott Azure Virtuálisgép-méretek listáját
  * Az Azure Virtuálisgép-méretek fontos készletkapacitás információival
  * Támogatott SAP szoftver, és az operációs rendszer és az adatbázis kombinációját
  * A Windows és a Microsoft Azure Linux szükséges SAP kernel verziója

* SAP Megjegyzés [2015553] a SAP-támogatott SAP szoftverek központi telepítése az Azure-ban szükséges előfeltételeket ismerteti.
* SAP Megjegyzés [2178632] tartalmaz részletes információkat az Azure-ban SAP jelentett összes figyelési metrikákat.
* SAP Megjegyzés [1409604] hello szükséges SAP állomás ügynök verziója a Windows Azure-ban.
* SAP Megjegyzés [2191498] hello szükséges SAP állomás ügynök verziója Linux az Azure-ban.
* SAP Megjegyzés [2243692] SAP Azure Linux licenceléssel kapcsolatos információt tartalmaz.
* SAP Megjegyzés [1984787] SUSE Linux Enterprise Server 12 vonatkozó általános információkat tartalmaz.
* SAP Megjegyzés [2002167] Red Hat Enterprise Linux vonatkozó általános információkat tartalmaz 7.x.
* SAP Megjegyzés [2069760] Oracle Linux vonatkozó általános információkat tartalmaz 7.x.
* SAP Megjegyzés [1999351] hello Azure fokozott Figyelőbővítmény az SAP további információkat.
* SAP Megjegyzés [1597355] Linux lapozóterület vonatkozó általános információkat tartalmaz.
* [SAP Azure Állapotváltozás oldalon](https://wiki.scn.sap.com/wiki/x/Pia7Gg) hírek és hasznos erőforrások gyűjteményét.
* [SAP közösségi WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) rendelkezik az összes szükséges SAP megjegyzések Linux.
* SAP-specifikus PowerShell-parancsmagok részét képező [Azure PowerShell][azure-ps].
* SAP-specifikus Azure parancssori felület parancsait részét képező [Azure CLI][azure-cli].

### <a name="42ee2bdb-1efc-4ec7-ab31-fe4c22769b94"></a>Windows-erőforrások
A Microsoft cikkeiben SAP üzemelő példányok Azure terjed ki:

* [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]
* [Az Azure virtuális gépek központi telepítésének SAP NetWeaver (Ez a cikk)][deployment-guide]
* [Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]

## <a name="b3253ee3-d63b-4d74-a49b-185e76c4088e"></a>Azure virtuális gépeken SAP szoftver központi telepítési forgatókönyvei
Lehetősége van több virtuális gépek és a kapcsolódó lemezt az Azure-ban való telepítéséhez. Ennek az oka fontos toounderstand hello különbségei telepítési lehetőségeket, akkor is igénybe vehet más lépéseket tooprepare központi telepítés alapján hello központi telepítési típust akkor válassza ki a virtuális gépek.

### <a name="db477013-9060-4602-9ad4-b0316f8bb281"></a>1. forgatókönyv: Központi telepítése egy virtuális Gépet az Azure piactérről az SAP hello
A Microsoft által biztosított rendszerképet használja, vagy harmadik fél általi a hello Azure piactér toodeploy a virtuális Gépet. hello piactér kínál néhány szabványos operációs rendszer képet, a Windows Server és a különböző Linux terjesztéseket. Is telepíthet egy olyanra, amely magában foglalja az adatbázis-kezelő rendszer (DBMS) SKU, például a Microsoft SQL Server. Az adatbázis-kezelő termékváltozatok lemezképek használatával kapcsolatos további információkért lásd: [SAP NetWeaver Azure virtuális gépek DBMS telepítésének][dbms-guide].

hello alábbi folyamatábra bemutatja hello SAP-specifikus feladatütemezési lépések hello Azure Piactérről származó virtuális gép üzembe helyezéséhez:

![A Virtuálisgép-telepítéshez SAP rendszerekhez hello Azure Piactérről származó Virtuálisgép-lemezkép használatával folyamatábrája][deployment-guide-figure-100]

#### <a name="create-a-virtual-machine-by-using-hello-azure-portal"></a>Hozzon létre egy virtuális gépet hello Azure-portál használatával
hello legegyszerűbb módja toocreate hello Azure Piactérről származó lemezkép egy új virtuális gép van hello Azure-portál használatával.

1.  Nyissa meg túl<https://portal.azure.com/#create/hub>.  Vagy hello Azure-portál menüjében, jelölje ki **+ új**.
2.  Válassza ki **számítási**, majd válassza ki az operációs rendszer kívánt toodeploy hello típusú. Például Windows Server 2012 R2, SUSE Linux Enterprise Server 12 (SLES 12), Red Hat Enterprise Linux 7.2 (RHEL 7.2) vagy Oracle Linux 7.2. hello alapértelmezett listanézet nem szerepelnek az összes támogatott operációs rendszerek. Válassza ki **láthatja az összes** teljes listáját. A SAP szoftverek központi telepítéséhez támogatott operációs rendszerekkel kapcsolatos további információkért lásd: a SAP Megjegyzés [1928533].
3.  Hello következő lapon tekintse át a feltételeket és kikötéseket.
4.  A hello **telepítési modell kiválasztása** jelölje ki **erőforrás-kezelő**.
5.  Kattintson a **Létrehozás** gombra.

hello varázsló végigvezeti Önt hello szükséges paraméterek toocreate hello virtuális gép, továbbá tooall szükséges erőforrások, például a hálózati adapterek és a storage-fiókok. Ezek a paraméterek a következők:

1. **Alapvető**:
 * **Név**: hello erőforrás (hello virtuális gép neve) hello nevét.
 * **Virtuális gép lemeztípus**: hello operációsrendszer-lemez hello lemez típusa. Ha az adatlemezek toouse prémium szintű Storage, prémium szintű Storage, valamint a hello operációs rendszer lemezének használatát javasoljuk.
 * **Felhasználónév és jelszó** vagy **nyilvános SSH-kulcs**: hello felhasználóneve és jelszava hello felhasználó hello kiépítés során létrehozott megadása. A Linux virtuális gép hello toosign használhatja a gépet toohello nyilvános Secure Shell (SSH) kulcs is megadhatja.
 * **Előfizetés**: válassza ki a megjeleníteni kívánt toouse tooprovision hello új virtuális gép hello előfizetés.
 * **Erőforráscsoport**: hello VM hello hello erőforráscsoport nevét. Megadhat egy új csoport vagy hello erőforrásnév, már létezik egy erőforráscsoport vagy hello nevét.
 * **Hely**: Ha toodeploy hello új virtuális gépet. Ha azt szeretné, hogy tooconnect hello virtuális gép tooyour a helyszíni hálózat, mindenképpen jelölje ki Azure tooyour helyi hálózathoz csatlakozó virtuális hálózati hello hello helyét. További információkért lásd: [Microsoft Azure hálózatkezelés] [ planning-guide-microsoft-azure-networking] a [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide].
2. **Méret**:

     A támogatott virtuális gép típusainak listáját lásd: SAP Megjegyzés [1928533]. Győződjön meg arról, hogy ha azt szeretné, hogy a prémium szintű Azure Storage toouse hello megfelelő Virtuálisgép-típussá választja. Nem minden virtuális gép esetében támogatja a prémium szintű Storage. További információkért lásd: [tárolási: Microsoft Azure Storage- és adatlemezek] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] és [prémium szintű Azure Storage] [ planning-guide-azure-premium-storage] a [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide].

3. **Beállítások**:
  * **Tárolás**
    * **Lemez típusa**: hello operációsrendszer-lemez hello lemez típusa. Ha az adatlemezek toouse prémium szintű Storage, prémium szintű Storage, valamint a hello operációs rendszer lemezének használatát javasoljuk.
    * **Felügyelt lemezeket használó**: Ha toouse kezelt lemezek, jelölje be az Igen gombra. Felügyelt lemezek kapcsolatos további információkért lásd: fejezet [kezelt lemezek] [ planning-guide-managed-disks] hello tervezési útmutató.
    * **A tárfiók**: Válasszon egy meglévő tárfiókot, vagy hozzon létre egy újat. Nem minden tárolási használható SAP-alkalmazások futtatására. További információ a tárolási típusok: [Microsoft Azure Storage] [ dbms-guide-2.3] a [SAP NetWeaver Azure virtuális gépek DBMS telepítésének][dbms-guide].
  * **Hálózat**
    * **Virtuális hálózati** és **alhálózati**: toointegrate hello virtuális gép, amelyet az intraneten, válassza hello virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat.
    * **Nyilvános IP-cím**: hello nyilvános IP-címet, hogy szeretné, hogy toouse, vagy adja meg a paraméterek toocreate egy új nyilvános IP-címet. Használhatja a nyilvános IP-cím tooaccess a virtuális gép hello interneten keresztül. Győződjön meg arról, hogy a hálózati biztonsági csoport toohelp biztonságos hozzáférést tooyour virtuális gép létrehozása.
    * **Hálózati biztonsági csoport**: további információkért lásd: [hálózati biztonsági csoportokkal hálózati adatforgalom szabályozásához][virtual-networks-nsg].
  * **Bővítmények**: telepítése virtuálisgép-bővítmények toohello telepítési hozzáadásukkal. Ebben a lépésben tooadd bővítmények nem kell. SAP támogatásához szükséges hello extensions telepítve később. Című [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP] [ deployment-guide-4.5] az útmutatóban.
  * **Magas rendelkezésre állású**: Válasszon egy rendelkezésre állási csoportot, vagy adja meg a hello paraméterek toocreate egy új rendelkezésre állási beállítása. További információkért lásd: [Azure rendelkezésre állási készletek][planning-guide-3.2.3].
  * **Figyelés**
    * **Rendszerindítási diagnosztika**: kiválaszthatja **letiltása** vonatkozó rendszerindítási diagnosztika.
    * **Vendég operációs rendszer diagnosztika**: kiválaszthatja **letiltása** diagnosztika figyelésre.

4. **Összefoglalás**:

  Ellenőrizze a beállításokat, majd válassza ki **OK**.

A virtuális gép telepítve van a kijelölt hello erőforráscsoportban.

#### <a name="create-a-virtual-machine-by-using-a-template"></a>Hozzon létre egy virtuális gépet egy sablon használatával
Hello közzétett hello SAP-sablonok segítségével hozhat létre egy virtuális gép [azure-gyors üzembe helyezés-sablonok GitHub-tárházban][azure-quickstart-templates-github]. Emellett manuálisan is létrehozhat egy virtuális gép hello segítségével [Azure-portálon][virtual-machines-windows-tutorial], [PowerShell][virtual-machines-ps-create-preconfigure-windows-resource-manager-vms], vagy [Azure parancssori felület ][virtual-machines-linux-tutorial].

* [**A két-réteg konfigurációjához (csak egy virtuális gép) sablon** (sap-2-réteg-Piactéri-lemezképhez)][sap-templates-2-tier-marketplace-image]

  toocreate egy kétrétegű rendszert csak egy virtuális gépet, a sablon használatához.
* [**A két-réteg konfigurációjához (csak egy virtuális gép) sablon - lemezek felügyelt** (sap-2-tier-marketplace-image-md)][sap-templates-2-tier-marketplace-image-md]

  toocreate egy kétszintű rendszer csak egy virtuális gép és a felügyelt lemezek használatával a sablon használatához.
* [**Háromrétegű konfigurációs (több virtuális gép) sablon** (sap-3-réteg-Piactéri-lemezképhez)][sap-templates-3-tier-marketplace-image]

  toocreate egy háromrétegű rendszert több virtuális gépet a sablon használatához.
* [**Háromrétegű konfigurációs (több virtuális gép) sablon - lemezek felügyelt** (sap-3-tier-marketplace-image-md)][sap-templates-3-tier-marketplace-image-md]

  virtuális gépek és a felügyelt lemezek használatával háromrétegű rendszer toocreate a sablon használatához.

Hello Azure-portálon adja meg a következő hello sablon paramétereinek hello:

1. **Alapvető**:
  * **Előfizetés**: hello előfizetés toouse toodeploy hello sablont.
  * **Erőforráscsoport**: hello erőforrás csoport toouse toodeploy hello sablont. Létrehozhat egy új erőforráscsoportot, vagy egy meglévő erőforráscsoportot kiválaszthatja hello előfizetésben.
  * **Hely**: Ha toodeploy hello sablont. Ha egy meglévő erőforráscsoportot jelölt ki, az erőforráscsoport hello hely szolgál.

2. **Beállítások**:
  * **SAP Rendszerazonosító**: hello SAP rendszer azonosítója (SID).
  * **Operációs rendszer típusa**: hello operációs rendszer toodeploy, például, a Windows Server 2012 R2, a SUSE Linux Enterprise Server 12 (SLES 12), a Red Hat Enterprise Linux 7.2 (RHEL 7.2) vagy Oracle Linux 7.2.

    hello listanézet nem szerepelnek az összes támogatott operációs rendszerek. A SAP szoftverek központi telepítéséhez támogatott operációs rendszerekkel kapcsolatos további információkért lásd: a SAP Megjegyzés [1928533].
  * **SAP mérete**: hello hello SAP rendszer méretét.

    SAP hello új rendszer hello számát biztosítja. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.
  * **Rendelkezésre állására** (csak a sablon háromrétegű): hello rendelkezésre állására.

    Válassza ki **magas rendelkezésre ÁLLÁSÚ** konfiguráció esetén, amely lehetővé teszi egy magas rendelkezésre állású telepítéshez. Két és két kiszolgálói a ABAP SAP központi szolgáltatások (ASC) jönnek létre.
  * **Tárolási típus** (csak a sablon kétrétegű): hello tárolási toouse típusú.

    Nagyobb méretű rendszerekhez erősen ajánlott prémium szintű Azure Storage használatával. További információ a tárolási típusok a cikkekben találhat:
      * [A prémium szintű Azure SSD-tárolón SAP DBMS példány használata][2367194]
      * [A Microsoft Azure tárolás] [ dbms-guide-2.3] a [SAP NetWeaver Azure virtuális gépek DBMS központi telepítése][dbms-guide]
      * [Prémium szintű Storage: Nagy teljesítményű tárolást az Azure virtuális gépek terheléséhez][storage-premium-storage-preview-portal]
      * [Azure Storage bemutatása tooMicrosoft][storage-introduction]
  * **Rendszergazda felhasználóneve** és **rendszergazdai jelszó**: felhasználónévvel és jelszóval.
    Új felhasználó jön létre, az aláíráshoz toohello virtuális gépen.
  * **Új vagy meglévő alhálózati**: meghatározza, hogy egy új virtuális hálózat és alhálózat jönnek létre, vagy egy létező alhálózatot használt. Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válassza ki a **meglévő**.
  * **Alhálózati azonosító**: hello azonosító hello alhálózati hello virtuális gépek csatlakozni fognak. Válassza ki a virtuális magánhálózati (VPN) vagy Azure ExpressRoute virtuális hálózat toouse tooconnect hello virtuális gép tooyour a helyszíni hálózati hello alhálózati. hello azonosítója általában néz ki: /subscriptions/&lt;előfizetés-azonosító > /resourceGroups/&lt;erőforráscsoport-név > /providers/Microsoft.Network/virtualNetworks/&lt;virtuálishálózat-név > /subnets/&lt;alhálózat neve >

3. **Feltételek és kikötések**:  
    Tekintse át és fogadja el hello jogi feltételeket.

4.  Válassza ki **beszerzési**.

hello Azure Virtuálisgép-ügynök van telepítve alapértelmezés szerint hello Azure Piactérről származó lemezkép használatakor.

#### <a name="configure-proxy-settings"></a>Proxybeállítások konfigurálása
A helyszíni hálózat konfigurációjától függően szükség lehet tooset be hello proxy a virtuális Gépet. Ha a virtuális Géphez csatlakoztatott tooyour a helyi hálózaton keresztül VPN- vagy ExpressRoute, hello VM lehet, hogy nem tud tooaccess hello Internet, és nem kell tudni toodownload szükséges hello extensions vagy figyelési adatainak gyűjtéséről. További információkért lásd: [hello proxybeállítások][deployment-guide-configure-proxy].

#### <a name="join-a-domain-windows-only"></a>Csatlakozás egy tartományhoz (csak Windows)
Ha az Azure-telepítés csatlakoztatott tooan a helyszíni Active Directory vagy a DNS egy Azure-webhelyek VPN-kapcsolat vagy ExpressRoute példány (Ez a lehetőség *létesítmények közötti* a [Azure virtuális gépek tervezése és a megvalósítását az SAP NetWeaver][planning-guide]), várható, hogy hello virtuális gép csatlakozik egy helyszíni tartományban. Ez a feladat szempontjai kapcsolatos további információkért lásd: [tartományhoz egy virtuális gép tooan helyszíni (csak Windows)][deployment-guide-4.3].

#### <a name="ec323ac3-1de9-4c3a-b770-4ff701def65b"></a>Figyelés konfigurálása
toobe, hogy SAP támogatja a környezetben, a hello Azure Figyelőbővítmény az SAP beállítása [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5]. Ellenőrizze az SAP-figyelés és a szükséges minimális verziója a SAP Kernel és a gazdagép ügynöke SAP, felsorolt hello erőforrást hello előfeltételeit [SAP erőforrások][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Figyelésének ellenőrzése
Ellenőrizze, hogy figyelési működik-e, a [ellenőrzések és végpont figyelő beállításával kapcsolatos hibaelhárítás][deployment-guide-troubleshooting-chapter].

#### <a name="post-deployment-steps"></a>Telepítés utáni lépéseket
Létrehozása után hello VM és hello virtuális gép központi telepítése, hello VM tooinstall hello szükséges szoftver-összetevő van szüksége. Hello telepítési/szoftverfrissítési telepítés feladatütemezési az ilyen típusú Virtuálisgép-telepítéshez, mert telepítve hello szoftver toobe már elérhetőnek kell lennie, vagy egy másik virtuális gépen, vagy csatolt lemeznek, az Azure-ban. Vagy, fontolja meg egy létesítmények közötti forgatókönyv esetén a mely kapcsolat toohello a helyszíni eszközök (telepítési megosztások) használatával kap.

Miután telepítette a virtuális Gépet az Azure-ban, hajtsa végre hello azonos irányelvek és eszközök tooinstall hello SAP szoftver a virtuális Gépet, ha Ön a helyszíni környezetben tenné. tooinstall SAP szoftver egy Azure virtuális gépen, mind az SAP és a Microsoft javasolja, hogy feltöltése Azure virtuális merevlemezek vagy kezelt lemezek hello SAP telepítési adathordozón tárolja, vagy, hogy hozzon létre egy Azure virtuális Gépen, amely minden hello fájlkiszolgálóként szükséges SAP telepítési adathordozóján.

### <a name="54a1fc6d-24fd-4feb-9c57-ac588a55dff2"></a>2. forgatókönyv: Egyéni lemezképet a virtuális gép telepítése az SAP
Az operációs rendszer vagy az adatbázis-kezelő különböző verziói különböző javítás igényekkel rendelkeznek, mert hello Azure piactér talált hello képek előfordulhat, hogy nem az igényeinek. Ehelyett érdemes toocreate egy virtuális Gépet a saját operációs rendszer vagy adatbázis-kezelő Virtuálisgép-lemezkép, amely központilag telepíthető újra később.
Más lépéseket toocreate a saját lemezképek Linux, mint toocreate Windows használhatja.

- - -
> ![Windows][Logo_Windows] Windows
>
> több virtuális gépek, hello Windows-beállítások (például a Windows biztonsági AZONOSÍTÓK és állomásnév) azért kell és be kell általánosítva toodeploy hello használhatja a Windows-lemezkép tooprepare a helyszíni virtuális gép. Használhat [sysprep](https://msdn.microsoft.com/library/hh825084.aspx) toodo ez.
>
> ![Linux][Logo_Linux] Linux
>
> a Linux-lemezkép használható toodeploy több virtuális gép tooprepare, egyes Linux beállításainak meg kell azért vagy a általánosított hello a helyszíni virtuális gép. Használhat `waagent -deprovision` toodo ez. További információkért lásd: [Azure-on futó Linux virtuális gép rögzítése] [ virtual-machines-linux-capture-image] és hello [Azure Linux ügynök felhasználói útmutató][virtual-machines-linux-agent-user-guide-command-line-options].
>
>

- - -
Készítse elő és hozzon létre egy egyéni lemezképet, majd toocreate használja több új virtuális gépeket. Erről a [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]. Állítsa be az adatbázis tartalom SAP szoftver kiépítés Manager tooinstall új SAP rendszer használatával (adatbázis biztonsági másolatának visszaállítása csatolt toohello virtuális gép egy lemezről) vagy közvetlenül egy adatbázis biztonsági mentését állítja vissza az Azure-tárolót, ha az adatbázis-kezelő támogatja azt. További információkért lásd: [SAP NetWeaver Azure virtuális gépek DBMS telepítésének][dbms-guide]. Ha már telepítette a helyszíni virtuális Gépre (különösen a kétrétegű rendszerek) egy SAP rendszer, módosíthatja hello SAP rendszerbeállítások hello hello Azure virtuális gép központi telepítése után SAP Szoftverellátáshoz által támogatott hello rendszer átnevezése eljárás használatával Manager (SAP Megjegyzés [1619720]). Ellenkező esetben telepítése hello SAP szoftver hello Azure VM telepítése után.

hello következő folyamatábra azt mutatja be, hello SAP-specifikus egy virtuális Gépet egy egyéni lemezképről való telepítésének lépései:

![A Virtuálisgép-telepítéshez SAP rendszerekhez titkos piactéren Virtuálisgép-lemezkép használatával folyamatábrája][deployment-guide-figure-300]

#### <a name="create-a-virtual-machine-by-using-hello-azure-portal"></a>Hozzon létre egy virtuális gépet hello Azure-portál használatával
hello legegyszerűbb módja toocreate felügyelt lemezképet egy új virtuális gép van hello Azure-portál használatával. További információ a hogyan toocreate kezelése lemezképet, olvassa el [egy általánosított virtuális gép az Azure-ban kezelt lemezképének elkészítése](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)

1.  Nyissa meg túl<https://ms.portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2Fimages>. Vagy hello Azure-portál menüjében, jelölje ki **képek**.
2.  Jelölje be hello felügyelt lemezképet toodeploy szeretné, majd kattintson a **virtuális gép létrehozása**

hello varázsló végigvezeti Önt hello szükséges paraméterek toocreate hello virtuális gép, továbbá tooall szükséges erőforrások, például a hálózati adapterek és a storage-fiókok. Ezek a paraméterek a következők:

1. **Alapvető**:
 * **Név**: hello erőforrás (hello virtuális gép neve) hello nevét.
 * **Virtuális gép lemeztípus**: hello operációsrendszer-lemez hello lemez típusa. Ha az adatlemezek toouse prémium szintű Storage, prémium szintű Storage, valamint a hello operációs rendszer lemezének használatát javasoljuk.
 * **Felhasználónév és jelszó** vagy **nyilvános SSH-kulcs**: hello felhasználóneve és jelszava hello felhasználó hello kiépítés során létrehozott megadása. A Linux virtuális gép hello toosign használhatja a gépet toohello nyilvános Secure Shell (SSH) kulcs is megadhatja.
 * **Előfizetés**: válassza ki a megjeleníteni kívánt toouse tooprovision hello új virtuális gép hello előfizetés.
 * **Erőforráscsoport**: hello VM hello hello erőforráscsoport nevét. Megadhat egy új csoport vagy hello erőforrásnév, már létezik egy erőforráscsoport vagy hello nevét.
 * **Hely**: Ha toodeploy hello új virtuális gépet. Ha azt szeretné, hogy tooconnect hello virtuális gép tooyour a helyszíni hálózat, mindenképpen jelölje ki Azure tooyour helyi hálózathoz csatlakozó virtuális hálózati hello hello helyét. További információkért lásd: [Microsoft Azure hálózatkezelés] [ planning-guide-microsoft-azure-networking] a [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide].
2. **Méret**:

     A támogatott virtuális gép típusainak listáját lásd: SAP Megjegyzés [1928533]. Győződjön meg arról, hogy ha azt szeretné, hogy a prémium szintű Azure Storage toouse hello megfelelő Virtuálisgép-típussá választja. Nem minden virtuális gép esetében támogatja a prémium szintű Storage. További információkért lásd: [tárolási: Microsoft Azure Storage- és adatlemezek] [ planning-guide-storage-microsoft-azure-storage-and-data-disks] és [prémium szintű Azure Storage] [ planning-guide-azure-premium-storage] a [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide].

3. **Beállítások**:
  * **Tárolás**
    * **Lemez típusa**: hello operációsrendszer-lemez hello lemez típusa. Ha az adatlemezek toouse prémium szintű Storage, prémium szintű Storage, valamint a hello operációs rendszer lemezének használatát javasoljuk.
    * **Felügyelt lemezeket használó**: Ha toouse kezelt lemezek, jelölje be az Igen gombra. Felügyelt lemezek kapcsolatos további információkért lásd: fejezet [kezelt lemezek] [ planning-guide-managed-disks] hello tervezési útmutató.
  * **Hálózat**
    * **Virtuális hálózati** és **alhálózati**: toointegrate hello virtuális gép, amelyet az intraneten, válassza hello virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat.
    * **Nyilvános IP-cím**: hello nyilvános IP-címet, hogy szeretné, hogy toouse, vagy adja meg a paraméterek toocreate egy új nyilvános IP-címet. Használhatja a nyilvános IP-cím tooaccess a virtuális gép hello interneten keresztül. Győződjön meg arról, hogy a hálózati biztonsági csoport toohelp biztonságos hozzáférést tooyour virtuális gép létrehozása.
    * **Hálózati biztonsági csoport**: további információkért lásd: [hálózati biztonsági csoportokkal hálózati adatforgalom szabályozásához][virtual-networks-nsg].
  * **Bővítmények**: telepítése virtuálisgép-bővítmények toohello telepítési hozzáadásukkal. Nem kell tooadd bővítmény ebben a lépésben. SAP támogatásához szükséges hello extensions telepítve később. Című [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP] [ deployment-guide-4.5] az útmutatóban.
  * **Magas rendelkezésre állású**: Válasszon egy rendelkezésre állási csoportot, vagy adja meg a hello paraméterek toocreate egy új rendelkezésre állási beállítása. További információkért lásd: [Azure rendelkezésre állási készletek][planning-guide-3.2.3].
  * **Figyelés**
    * **Rendszerindítási diagnosztika**: kiválaszthatja **letiltása** vonatkozó rendszerindítási diagnosztika.
    * **Vendég operációs rendszer diagnosztika**: kiválaszthatja **letiltása** diagnosztika figyelésre.

4. **Összefoglalás**:

  Ellenőrizze a beállításokat, majd válassza ki **OK**.

A virtuális gép telepítve van a kijelölt hello erőforráscsoportban.
#### <a name="create-a-virtual-machine-by-using-a-template"></a>Hozzon létre egy virtuális gépet egy sablon használatával
a központi telepítés hello Azure-portálon a saját operációsrendszer-lemezképek használatával toocreate hello SAP-sablonok a következő egyikét használhatja. Ezeket a sablonokat a rendszer közzéteszi az hello [azure-gyors üzembe helyezés-sablonok GitHub-tárházban][azure-quickstart-templates-github]. Emellett manuálisan is létrehozhat egy virtuális gép használatával [PowerShell][virtual-machines-upload-image-windows-resource-manager].

* [**A két-réteg konfigurációjához (csak egy virtuális gép) sablon** (sap-2-réteg-felhasználó-kép)][sap-templates-2-tier-user-image]

  toocreate egy kétrétegű rendszert csak egy virtuális gépet, a sablon használatához.
* [**A két-réteg konfigurációjához (csak egy virtuális gép) sablon - lemezképe felügyelt** (sap-2-tier-user-image-md)][sap-templates-2-tier-user-image-md]

  toocreate egy kétszintű rendszer csak egy virtuális gép és a felügyelt lemezképet, a sablon használatához.
* [**Háromrétegű konfigurációs (több virtuális gép) sablon** (sap-3-réteg-felhasználó-kép)][sap-templates-3-tier-user-image]

  több virtuális gép vagy a saját operációsrendszer-lemezképek, a háromrétegű rendszer toocreate a sablon használatához.
* [**Háromrétegű konfigurációs (több virtuális gép) sablon - lemezképe felügyelt** (sap-3-tier-user-image-md)][sap-templates-3-tier-user-image-md]

  több virtuális gép vagy a saját operációsrendszer-lemezképek és felügyelt lemezképet, a háromrétegű rendszer toocreate a sablon használatához.

Hello Azure-portálon adja meg a következő hello sablon paramétereinek hello:

1. **Alapvető**:
  * **Előfizetés**: hello előfizetés toouse toodeploy hello sablont.
  * **Erőforráscsoport**: hello erőforrás csoport toouse toodeploy hello sablont. Hozzon létre egy új erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot hello előfizetésben.
  * **Hely**: Ha toodeploy hello sablont. Ha egy meglévő erőforráscsoportot jelölt ki, az erőforráscsoport hello hely szolgál.
2. **Beállítások**:
  * **SAP Rendszerazonosító**: hello SAP rendszer azonosítóját.
  * **Operációs rendszer típusa**: hello kívánt toodeploy (Windows vagy Linux) operációs rendszer típusa.
  * **SAP mérete**: hello hello SAP rendszer méretét.

    SAP hello új rendszer hello számát biztosítja. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.
  * **Rendelkezésre állására** (csak a sablon háromrétegű): hello rendelkezésre állására.

    Válassza ki **magas rendelkezésre ÁLLÁSÚ** konfiguráció esetén, amely lehetővé teszi egy magas rendelkezésre állású telepítéshez. Két adatbázis-kiszolgálók és a két kiszolgáló ASC jönnek létre.
  * **Tárolási típus** (csak a sablon kétrétegű): hello tárolási toouse típusú.

    Nagyobb méretű rendszerekhez erősen ajánlott prémium szintű Azure Storage használatával. Tárolási típusok kapcsolatos további információkért tekintse meg a következő erőforrások hello:
      * [A prémium szintű Azure SSD-tárolón SAP DBMS példány használata][2367194]
      * [A Microsoft Azure tárolás] [ dbms-guide-2.3] a [SAP NetWeaver Azure virtuális gépek DBMS központi telepítése][dbms-guide]
      * [Prémium szintű Storage: Nagy teljesítményű tárolást az Azure virtuális gépek terheléséhez][storage-premium-storage-preview-portal]
      * [Azure Storage bemutatása tooMicrosoft][storage-introduction]
  * **Virtuális merevlemez URI felhasználói lemezkép** (csak a nem felügyelt lemez kép sablon esetében): hello titkos operációsrendszer-lemezképek Indulni, például https:// URI hello&lt;accountname >.blob.core.windows.net/vhds/userimage.vhd.
  * **Felhasználói lemezkép tárfiókja** (csak a nem felügyelt lemez kép sablon esetében): hello titkos operációsrendszer-lemezképek tárolására, például hello tárfiókja nevére hello &lt;accountname > a https://&lt;accountname >. BLOB.Core.Windows.NET/vhds/userimage.vhd.
  * **userImageId** (csak felügyelt lemezes kép sablon): hello felügyelt lemezképet toouse kívánt azonosítója
  * **Rendszergazda felhasználóneve** és **rendszergazdai jelszó**: hello felhasználónevet és jelszót.

    Új felhasználó jön létre, az aláíráshoz toohello virtuális gépen.
  * **Új vagy meglévő alhálózati**: meghatározza, hogy egy új virtuális hálózat és alhálózat jön létre, vagy egy létező alhálózatot használt. Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válassza ki a **meglévő**.
  * **Alhálózati azonosító**: hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakozni fognak. Válassza ki a VPN- vagy ExpressRoute virtuális hálózati toouse tooconnect hello virtuális gép tooyour a helyszíni hálózat hello alhálózat. hello azonosítója általában néz ki:

    következő&lt;előfizetés-azonosító > /resourceGroups/&lt;erőforráscsoport-név > /providers/Microsoft.Network/virtualNetworks/&lt;virtuálishálózat-név > /subnets/&lt;alhálózat neve >

3. **Feltételek és kikötések**:  
    Tekintse át és fogadja el hello jogi feltételeket.

4.  Válassza ki **beszerzési**.

#### <a name="install-hello-vm-agent-linux-only"></a>Telepítse a hello Virtuálisgép-ügynök (csak Linux)
toouse hello sablonok hello előző szakaszban leírt, a Linux-ügynök telepítve legyen a hello felhasználói lemezképet, vagy hello telepítésben hello sikertelen lesz. Töltse le és telepítse hello Virtuálisgép-ügynök hello felhasználói lemezkép leírtak [letöltése, telepítése és engedélyezése Azure Virtuálisgép-ügynök hello][deployment-guide-4.4]. Ha nem adja meg hello sablonok, is telepítheti hello Virtuálisgép-ügynök újabb.

#### <a name="join-a-domain-windows-only"></a>Csatlakozás egy tartományhoz (csak Windows)
Ha az Azure-telepítés csatlakoztatott tooan a helyszíni Active Directory vagy a DNS egy Azure-webhelyek VPN-kapcsolat vagy Azure ExpressRoute példány (Ez a lehetőség *létesítmények közötti* a [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]), várható, hogy hello virtuális gép csatlakozik egy helyszíni tartományban. Ez a lépés szempontjai kapcsolatos további információkért lásd: [tartományhoz egy virtuális gép tooan helyszíni (csak Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxybeállítások konfigurálása
A helyszíni hálózat konfigurációjától függően szükség lehet tooset be hello proxy a virtuális Gépet. Ha a virtuális Géphez csatlakoztatott tooyour a helyi hálózaton keresztül VPN- vagy ExpressRoute, hello VM lehet, hogy nem tud tooaccess hello Internet, és nem kell tudni toodownload szükséges hello extensions vagy figyelési adatainak gyűjtéséről. További információkért lásd: [hello proxybeállítások][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Figyelés konfigurálása
toobe, hogy SAP támogatja a környezetben, a hello Azure Figyelőbővítmény az SAP beállítása [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5]. Ellenőrizze az SAP-figyelés és a szükséges minimális verziója a SAP Kernel és a gazdagép ügynöke SAP, felsorolt hello erőforrást hello előfeltételeit [SAP erőforrások][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Figyelésének ellenőrzése
Ellenőrizze, hogy figyelési működik-e, a [ellenőrzések és végpont figyelő beállításával kapcsolatos hibaelhárítás][deployment-guide-troubleshooting-chapter].


### <a name="a9a60133-a763-4de8-8986-ac0fa33aa8c1"></a>3. forgatókönyv: Egy a helyszíni virtuális gép áthelyezése a SAP Azure nem általánosított virtuális merevlemez használatával
Ebben a forgatókönyvben egy a helyszíni környezet tooAzure az SAP rendszert toomove tervezi. Ehhez a virtuális Merevlemezt, amely rendelkezik az operációs rendszer, hello SAP bináris fájljai, hello hello feltöltésével végül hello DBMS bináris fájlokat, valamint hello adatokkal hello VHD-k és adatbázis-kezelő, tooAzure hello naplófájlok. Eltérően ismertetett hello forgatókönyv [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP][deployment-guide-3.3], ebben az esetben mindig hello állomásnév, SAP SID és SAP felhasználói fiókot a hello Azure virtuális Gépen, mert nem voltak a helyszíni környezetben hello konfigurálva. Nem kell toogeneralize hello az operációs rendszer. Ebben a forgatókönyvben a leggyakrabban toocross helyszíni esetek, amikor hello SAP fekvő része a helyszínen fut így részét fut az Azure vonatkozik.

Ebben a forgatókönyvben hello Virtuálisgép-ügynök van **nem** automatikusan telepített központi telepítése során. Mivel hello Virtuálisgép-ügynök és a hello Azure fokozott Figyelőbővítmény SAP esetén nem szükséges toorun SAP NetWeaver az Azure-on, meg kell toodownload, telepítése és engedélyezése mindkét összetevők manuálisan hello virtuális gép létrehozása után.

Hello Azure Virtuálisgép-ügynök kapcsolatos további információkért tekintse meg a következő erőforrások hello.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Az Azure virtuális gép ügynökének áttekintése][virtual-machines-windows-agent-user-guide]
>
> ![Linux][Logo_Linux] Linux
>
> [Azure Linux ügynök felhasználói útmutatója][virtual-machines-linux-agent-user-guide]
>
>

- - -

a következő folyamatábra hello azt mutatja be, hello Azure nem általánosított virtuális merevlemez használatával egy a helyszíni virtuális gép áthelyezése lépései:

![Virtuális gép üzembe helyezéséhez egy Virtuálisgép-lemez SAP rendszerekhez folyamatábrája][deployment-guide-figure-400]

Ha hello lemez már feltöltött, és az Azure által meghatározott (lásd: [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]), tegye hello feladatok leírtak alapján a hello néhány szakaszok.

#### <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása
toocreate titkos operációsrendszer-lemez hello Azure-portálon keresztül a központi telepítés hello SAP sablon közzétett hello használata [azure-gyors üzembe helyezés-sablonok GitHub-tárházban][azure-quickstart-templates-github]. Emellett manuálisan is létrehozhat egy virtuális gép PowerShell használatával.

* [**A két-réteg konfigurációjához (csak egy virtuális gép) sablon** (sap-2-réteg-felhasználó-lemez)][sap-templates-2-tier-os-disk]

  toocreate egy kétrétegű rendszert csak egy virtuális gépet, a sablon használatához.
* [**A két-réteg konfigurációjához (csak egy virtuális gép) sablon - lemez felügyelt** (sap-2-tier-user-disk-md)][sap-templates-2-tier-os-disk-md]

  csak egy virtuális gép és egy felügyelt lemezt egy kétszintű rendszer toocreate a sablon használatához.

Hello Azure-portálon adja meg a következő hello sablon paramétereinek hello:

1. **Alapvető**:
  * **Előfizetés**: hello előfizetés toouse toodeploy hello sablont.
  * **Erőforráscsoport**: hello erőforrás csoport toouse toodeploy hello sablont. Hozzon létre egy új erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot hello előfizetésben.
  * **Hely**: Ha toodeploy hello sablont. Ha egy meglévő erőforráscsoportot jelölt ki, az erőforráscsoport hello hely szolgál.
2. **Beállítások**:
  * **SAP Rendszerazonosító**: hello SAP rendszer azonosítóját.
  * **Operációs rendszer típusa**: hello kívánt toodeploy (Windows vagy Linux) operációs rendszer típusa.
  * **SAP mérete**: hello hello SAP rendszer méretét.

    SAP hello új rendszer hello számát biztosítja. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.
  * **Tárolási típus** (csak a sablon kétrétegű): hello tárolási toouse típusú.

    Nagyobb méretű rendszerekhez erősen ajánlott prémium szintű Azure Storage használatával. Tárolási típusok kapcsolatos további információkért tekintse meg a következő erőforrások hello:
      * [A prémium szintű Azure SSD-tárolón SAP DBMS példány használata][2367194]
      * [A Microsoft Azure tárolás] [ dbms-guide-2.3] a [SAP NetWeaver Azure virtuális gép DBMS központi telepítése][dbms-guide]
      * [Prémium szintű Storage: Nagy teljesítményű tárolást az Azure virtuális gépek terheléséhez][storage-premium-storage-preview-portal]
      * [Azure Storage bemutatása tooMicrosoft][storage-introduction]
  * **Az operációs rendszer lemez VHD URI** (csak a sablon nem felügyelt lemezt): URI-jának hello titkos operációsrendszer-lemez, például https:// hello&lt;accountname >.blob.core.windows.net/vhds/osdisk.vhd.
  * **Az operációs rendszer lemezén lemezazonosítót felügyelt** (csak felügyelt lemezes sablon): hello hello kezelt lemez operációsrendszer-lemez azonosítója, /subscriptions/92d102f7-81a5-4df7-9877-54987ba97dd9/resourceGroups/group/providers/Microsoft.Compute/disks/WIN
  * **Új vagy meglévő alhálózati**: meghatározza, hogy egy új virtuális hálózat és alhálózat jönnek létre, vagy egy létező alhálózatot használt. Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válassza ki a **meglévő**.
  * **Alhálózati azonosító**: hello azonosító hello alhálózati toowhich hello virtuális gépek csatlakozni fognak. Válassza ki a VPN- vagy Azure ExpressRoute virtuális hálózati toouse tooconnect hello virtuális gép tooyour a helyszíni hálózat hello alhálózat. hello azonosítója általában néz ki:

    következő&lt;előfizetés-azonosító > /resourceGroups/&lt;erőforráscsoport-név > /providers/Microsoft.Network/virtualNetworks/&lt;virtuálishálózat-név > /subnets/&lt;alhálózat neve >

3. **Feltételek és kikötések**:  
    Tekintse át és fogadja el hello jogi feltételeket.

4.  Válassza ki **beszerzési**.

#### <a name="install-hello-vm-agent"></a>Hello Virtuálisgép-ügynök telepítése
sablonok ismertetett toouse hello hello az előző szakaszban, hello VM ügynököt telepíteni kell az operációs rendszer hello lemezen vagy hello telepítése sikertelen lesz. Töltse le és telepítse a virtuális gép, hello hello Virtuálisgép-ügynök, leírtak [letöltése, telepítése és engedélyezése Azure Virtuálisgép-ügynök hello][deployment-guide-4.4].

Ha nem adja meg hello sablonok hello előző szakaszban leírt, ezt követően is telepíthet hello Virtuálisgép-ügynök.

#### <a name="join-a-domain-windows-only"></a>Csatlakozás egy tartományhoz (csak Windows)
Ha az Azure-telepítés csatlakoztatott tooan a helyszíni Active Directory vagy a DNS egy Azure-webhelyek VPN-kapcsolat vagy ExpressRoute példány (Ez a lehetőség *létesítmények közötti* a [Azure virtuális gépek tervezése és a megvalósítását az SAP NetWeaver][planning-guide]), várható, hogy hello virtuális gép csatlakozik egy helyszíni tartományban. Ez a feladat szempontjai kapcsolatos további információkért lásd: [tartományhoz egy virtuális gép tooan helyszíni (csak Windows)][deployment-guide-4.3].

#### <a name="configure-proxy-settings"></a>Proxybeállítások konfigurálása
A helyszíni hálózat konfigurációjától függően szükség lehet tooset be hello proxy a virtuális Gépet. Ha a virtuális Géphez csatlakoztatott tooyour a helyi hálózaton keresztül VPN- vagy ExpressRoute, hello VM lehet, hogy nem tud tooaccess hello Internet, és nem kell tudni toodownload szükséges hello extensions vagy figyelési adatainak gyűjtéséről. További információkért lásd: [hello proxybeállítások][deployment-guide-configure-proxy].

#### <a name="configure-monitoring"></a>Figyelés konfigurálása
toobe, hogy SAP támogatja a környezetben, a hello Azure Figyelőbővítmény az SAP beállítása [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5]. Ellenőrizze az SAP-figyelés és a szükséges minimális verziója a SAP Kernel és a gazdagép ügynöke SAP, felsorolt hello erőforrást hello előfeltételeit [SAP erőforrások][deployment-guide-2.2].

#### <a name="monitoring-check"></a>Figyelésének ellenőrzése
Ellenőrizze, hogy figyelési működik-e, a [ellenőrzések és végpont figyelő beállításával kapcsolatos hibaelhárítás][deployment-guide-troubleshooting-chapter].

## <a name="update-hello-monitoring-configuration-for-sap"></a>SAP hello figyelési konfigurációjának frissítése
Frissítés hello SAP figyelési konfiguráció a következő forgatókönyvek hello valamelyikében.
* hello közös Microsoft/SAP munkacsoport kibővíti a figyelési képességek hello, és több vagy kevesebb számlálók kéri.
* Microsoft hello tartozó Azure infrastruktúra, amely biztosítja a figyelési adatok hello új verziójának vezet be, és hello Azure fokozott Figyelőbővítmény az SAP igények toobe leírtakon toothose módosításokat.
* További adatok lemezek tooyour Azure virtuális gép csatlakoztatása, vagy egy adatlemez eltávolítása. Ebben a forgatókönyvben frissítése tárolással kapcsolatos adatok hello gyűjteménye. A konfiguráció módosítása felvételét vagy törlését végpontok vagy IP-hozzárendelése címek tooa VM nem befolyásolja hello figyelési konfigurációt.
* Vált az Azure virtuális gép méretét hello például méret A5 tooany más Virtuálisgép-méretet.
* Hozzáadhat új hálózati adapterek tooyour Azure virtuális Gépen.

figyelési beállítások, a figyelés infrastruktúra a következő hello frissítés hello tooupdate szükséges lépések [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5].

## <a name="detailed-tasks-for-sap-software-deployment"></a>Feladatok részletesen SAP szoftver központi telepítése
Ez a szakasz részletes van arról, hogy adott feladatokat hello konfigurációja és központi telepítési folyamat lépéseit.

### <a name="604bcec2-8b6e-48d2-a944-61b0f5dee2f7"></a>Azure PowerShell-parancsmagok telepítése
1.  Nyissa meg túl[letölti a Microsoft Azure](https://azure.microsoft.com/downloads/).
2.  A **parancssori eszközök**a **PowerShell**, jelölje be **Windows telepítése**.
3.  A hello Microsoft letöltése Manager párbeszédpanelen hello letöltött fájl (például WindowsAzurePowershellGet.3f.3f.3fnew.exe), válassza ki a **futtatása**.
4.  toorun Microsoft Webplatform-telepítővel (a Microsoft Web-PI), válasszon **Igen**.
5.  Egy oldal, amely a jelek szerint ez jelenik meg:

  ![Telepítés lap az Azure PowerShell-parancsmagok][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Válassza ki **telepítése**, és fogadja el a hello Microsoft szoftverlicenc-szerződést.
7.  A PowerShell telepítve van. Válassza ki **Befejezés** tooclose hello telepítővarázslóját.

Ellenőrizze gyakran frissítések toohello PowerShell-parancsmag, amely általában frissítése havi. frissítések hello legegyszerűbb módja toocheck telepítési lépéseket, 5. lépésben látható toohello telepítési oldalára megelőző toodo hello. hello kiadás dátuma és kiadás hello parancsmagok száma 5. lépésben hello weblap szerepelnek. Másként nincs feltüntetve a SAP Megjegyzés [1928533] vagy SAP Megjegyzés [2015553], javasoljuk, hogy működik az Azure PowerShell-parancsmagok hello legújabb verziójával.

hello Azure PowerShell-parancsmagok a számítógépen telepített verziójának toocheck hello futtassa a PowerShell-parancsot:
```powershell
(Get-Module AzureRm.Compute).Version
```
hello eredménye így néz ki:

![Azure PowerShell parancsmag-verziójának ellenőrzése eredménye][deployment-guide-figure-600]
<a name="figure-6"></a>

Ha a számítógépen telepített hello Azure parancsmag verzió hello jelenlegi verziójával, hello telepítési varázsló első oldalán hello azt mutatja, hogy hozzáadásával **(telepített)** toohello terméknév (lásd a következő képernyőkép hello). Az PowerShell Azure-parancsmagok naprakészek legyenek. tooclose hello telepítővarázslójának elindításához válassza **kilépési**.

![Telepítési lapját, amely jelzi, hogy hello legújabb verziója található Azure PowerShell-parancsmagok Azure PowerShell-parancsmagjai telepítve vannak][deployment-guide-figure-700]
<a name="figure-7"></a>

### <a name="1ded9453-1330-442a-86ea-e0fd8ae8cab3"></a>Az Azure parancssori felület telepítése
1.  Nyissa meg túl[letölti a Microsoft Azure](https://azure.microsoft.com/downloads/).
2.  A **parancssori eszközök**a **Azure parancssori felület**, jelölje be hello **telepítése** hivatkozást az operációs rendszerhez.
3.  A hello Microsoft letöltése Manager párbeszédpanelen hello letöltött fájl (például WindowsAzureXPlatCLI.3f.3f.3fnew.exe), válassza ki a **futtatása**.
4.  toorun Microsoft Webplatform-telepítővel (a Microsoft Web-PI), válasszon **Igen**.
5.  Egy oldal, amely a jelek szerint ez jelenik meg:

  ![Telepítés lap az Azure PowerShell-parancsmagok][deployment-guide-figure-500]<a name="figure-5"></a>

6.  Válassza ki **telepítése**, és fogadja el a hello Microsoft szoftverlicenc-szerződést.
7.  Az Azure parancssori felület telepítve van. Válassza ki **Befejezés** tooclose hello telepítővarázslóját.

Ellenőrizze gyakran frissítések tooAzure parancssori felület, amely általában havi frissített van. frissítések hello legegyszerűbb módja toocheck telepítési lépéseket, 5. lépésben látható toohello telepítési oldalára megelőző toodo hello.

a számítógépen telepített Azure CLI toocheck hello verzióját futtassa ezt a parancsot:
```
azure --version
```

hello eredménye így néz ki:

![Az Azure parancssori felület verziójának ellenőrzése eredménye][deployment-guide-figure-760]
<a name="0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda"></a>

### <a name="31d9ecd6-b136-4c73-b61e-da4a29bbc9cc"></a>Csatlakozás a virtuális gép tooan helyszíni tartományhoz (csak Windows)
Ha SAP virtuális gépek központi telepítését a létesítmények közötti forgatókönyv, ahol a helyszíni Active Directory és a DNS-bővítve lettek az Azure várható, hogy a hello virtuális gépeket a rendszer egy helyszíni tartományhoz szeretne csatlakozni. hello részletes lépései toojoin egy virtuális gép tooan a helyi tartomány, és hello szükséges további szoftvereket toobe egy helyszíni tartomány függ a felhasználói. Általában egy virtuális gép tooan toojoin a helyi tartomány, tooinstall további szoftverek, például a kártevőirtó szoftver, és biztonsági mentési vagy figyelési van szüksége.

Ebben a forgatókönyvben is van szüksége arról, hogy ha az internetes proxybeállításokat kényszerítve vannak, amikor egy virtuális gép csatlakozik egy tartományhoz a környezetben, helyi rendszer fiók (S-1-5-18) hello Vendég virtuális gép rendelkezik hello Windows proxybeállításokat hello toomake. hello legegyszerűbb lehetőség tooforce hello proxy egy tartományhoz toosystems hello tartomány érvényes csoportházirend használatával.

### <a name="c7cbb0dc-52a4-49db-8e03-83e7edc2927d"></a>Letöltése, telepítése és engedélyezése hello Azure Virtuálisgép-ügynök
(Például egy olyanra, amely nem származnak hello Windows rendszer-előkészítő vagy, a sysprep eszköz) nem általánosított OS lemezképből telepített virtuális gépek kell toomanually letöltése, telepítése és engedélyezése hello Azure Virtuálisgép-ügynök.

Ha egy virtuális Gépet az Azure piactér hello telepít, a lépésre nincs szükség. Lemezképek hello Azure Piactérről származó már hello Azure Virtuálisgép-ügynök.

#### <a name="b2db5c9a-a076-42c6-9835-16945868e866"></a>Windows
1.  Hello Azure Virtuálisgép-ügynök letöltése:
  1.  Töltse le a hello [Azure Virtuálisgép-ügynök a telepítőcsomag](https://go.microsoft.com/fwlink/?LinkId=394789).
  2.  Egy számítógépre vagy a kiszolgáló helyileg tárolja a hello VM ügynök MSI-csomagot.
2.  Hello Azure Virtuálisgép-ügynök telepítése:
  1.  Csatlakozás toohello Azure VM telepítése Remote Desktop Protocol (RDP) használatával.
  2.  Nyisson meg egy Windows Explorer ablakot a virtuális gép hello és a célkönyvtár a forráskönyvtárban válassza hello hello MSI-fájljának hello Virtuálisgép-ügynök.
  3.  Húzza a helyi számítógép-kiszolgáló toohello hello Virtuálisgép-ügynök a célkönyvtárat hello Azure VM ügynök telepítő MSI-fájl hello virtuális gép.
  4.  Kattintson duplán a virtuális gép hello hello MSI-fájl.
3.  Virtuális gépek, amelyek összeillesztett tooon helyi tartomány, győződjön meg arról, hogy végleges internetes proxybeállításokat is érvényesek a toohello Windows helyi rendszer fiók (S-1-5-18) a virtuális gép, hello leírtak [hello proxybeállítások] [ deployment-guide-configure-proxy]. hello Virtuálisgép-ügynök ebben a környezetben fut, és meg kell tudni tooconnect tooAzure toobe.

Felhasználói beavatkozásra nincs szükség tooupdate hello Azure Virtuálisgép-ügynök. hello Virtuálisgép-ügynök automatikusan frissül, és a virtuális gép újraindítása nem szükséges.

#### <a name="6889ff12-eaaf-4f3c-97e1-7c9edc7f7542"></a>Linux
Parancsok tooinstall hello Virtuálisgép-ügynök következő Linux hello használata:

* **SUSE Linux Enterprise Server (SLES)**

  ```
  sudo zypper install WALinuxAgent
  ```

* **Red Hat Enterprise Linux (RHEL) vagy Oracle Linux**

  ```
  sudo yum install WALinuxAgent
  ```

Ha hello ügynök már telepítve van, a tooupdate hello Azure Linux ügynök hello ismertetett lépések [frissítés hello Azure Linux ügynök a virtuális gép toohello legújabb verziójában a Githubról][virtual-machines-linux-update-agent].

### <a name="baccae00-6f79-4307-ade4-40292ce4e02d"></a>Hello proxy konfigurálása
hello lépései tooconfigure hello proxy a Windows hello módon konfigurált hello proxy Linux eltérnek.

#### <a name="windows"></a>Windows
Proxybeállítások kell be kell állítania megfelelően hello tooaccess hello Internet helyi rendszer fiók számára. Ha a proxybeállítások nem csoportházirend-beállításokat, konfigurálhatja a helyi rendszer fiók hello hello beállításait.

1. Túl lépjen**Start**, adja meg **gpedit.msc**, majd válassza ki **Enter**.
2. Válassza ki **számítógép konfigurációja** > **felügyeleti sablonok** > **Windows-összetevők** > **Internet Explorer**. Győződjön meg arról, hogy hello beállítás **proxybeállítások beállítások számítógépenkénti (nem felhasználónként)** le van tiltva vagy nincs konfigurálva.
3. A **Vezérlőpult**, nyissa meg túl**hálózati és megosztási központ** > **Internetbeállítások**.
4. A hello **kapcsolatok** lapra, jelölje be hello **LAN-beállítások** gombra.
5. Törölje a jelet hello **beállítások automatikus észlelése** jelölőnégyzetet.
6. Jelölje be hello **proxykiszolgálót használni a helyi hálózaton** jelölje be a jelölőnégyzetet, és írja be a hello proxy címét és portszámát.
7. Jelölje be hello **speciális** gombra.
8. A hello **kivételek** mezőbe írja be a hello IP-cím **168.63.129.16**. Kattintson az **OK** gombra.

#### <a name="linux"></a>Linux
Proxybeállítások hello helyes-e a Microsoft Azure-Vendégügynök, amely itt található: hello hello konfigurációs fájlban \\stb\\waagent.conf.

Állítsa be a következő paraméterek hello:

1.  **HTTP-proxy állomás**. Például állítsa be úgy túl**proxy.corp.local**.
  ```
  HttpProxy.Host=<proxy host>

  ```
2.  **HTTP-proxy port**. Például állítsa be úgy túl**80**.
  ```
  HttpProxy.Port=<port of hello proxy host>

  ```
3.  Indítsa újra a hello ügynököt.

  ```
  sudo service waagent restart
  ```

a proxybeállítások hello \\stb\\waagent.conf is érvényesek toohello szükséges Virtuálisgép-bővítmények. Ha azt szeretné, hogy toouse hello Azure tárházak találhatók, győződjön meg arról, hogy a helyi intraneten keresztül nem lesz hello forgalom toothese tárházak találhatók. Ha létrehozta a felhasználó által definiált útvonalak tooenable kényszerített bújtatás, ellenőrizze, hogy egy olyan útvonalat, amely irányítja a forgalmat toohello adattárak közvetlenül toohello Internet, és nem az a pont-pont VPN-kapcsolaton keresztül.

* **SLES**

  Hello IP-címek szerepelnek is kell tooadd útvonalak \\stb\\regionserverclnt.cfg. hello következő ábra mutat egy példát:

  ![Alagúthasználat kényszerítése][deployment-guide-figure-50]


* **RHEL**

  Hello állomások hello IP-címek szerepelnek is kell tooadd útvonalak \\stb\\yum.repos.d\\rhui terheléselosztók. Egy vonatkozó példáért lásd: hello megelőző. ábra.

* **Oracle Linux**

  Nincsenek nem tárolóhelyekkel Oracle Linux Azure-on. A saját adattárak tooconfigure kell az Oracle Linux vagy hello nyilvános tárolóhelyekkel használja.

Felhasználó által definiált útvonalakra vonatkozó további információkért lásd: [felhasználó által definiált útvonalak és IP-továbbítás][virtual-networks-udr-overview].

### <a name="d98edcd3-f2a1-49f7-b26a-07448ceb60ca"></a>Hello Azure fokozott Figyelőbővítmény az SAP konfigurálása
Ha előkészítése után hello VM leírtak [szituációkban az SAP Azure virtuális gépek][deployment-guide-3], hello Azure Virtuálisgép-ügynök hello virtuális gépre van telepítve. következő lépés hello toodeploy hello Azure fokozott Figyelőbővítmény az SAP, amely található hello Azure Bővítménytárban az hello globális Azure adatközpontjaiban. További információkért lásd: [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide-9.1].

Használhatja a PowerShell vagy Azure CLI tooinstall és hello Azure fokozott Figyelőbővítmény az SAP konfigurálása. Tekintse meg a tooinstall hello bővítmény Windows vagy Linux virtuális Gépet egy Windows-számítógép segítségével [Azure PowerShell][deployment-guide-4.5.1]. a Linux virtuális gép egy Linux asztal használatával tooinstall hello bővítmény lásd [Azure CLI][deployment-guide-4.5.2].

#### <a name="987cf279-d713-4b4c-8143-6b11589bb9d4"></a>A Linux és a Windows virtuális gépek az Azure PowerShell
tooinstall hello Azure fokozott Figyelőbővítmény az SAP PowerShell használatával:

1. Győződjön meg arról, hogy telepítette-e az hello hello Azure PowerShell-parancsmag legújabb verzióját. További információkért lásd: [telepítése Azure PowerShell-parancsmagok][deployment-guide-4.1].  
2. A következő PowerShell-parancsmag futtatási hello.
    A rendelkezésre álló környezeteket listája, futtassa a `commandlet Get-AzureRmEnvironment`. Ha azt szeretné, toouse globális Azure, a környezet a **AzureCloud**. Az Azure Kínában, válassza ki a **AzureChinaCloud**.

    ```powershell
    $env = Get-AzureRmEnvironment -Name <name of hello environment>
    Login-AzureRmAccount -Environment $env
    Set-AzureRmContext -SubscriptionName <subscription name>

    Set-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
    ```

Után adja meg a fiók adatait, és hello Azure virtuális gépek azonosításához, hello parancsfájl szükséges hello bővítmények telepíti, és lehetővé teszi, hogy a hello szükséges szolgáltatásokat. Ez több percig is eltarthat.
További információ `Set-AzureRmVMAEMExtension`, lásd: [Set-AzureRmVMAEMExtension][msdn-set-azurermvmaemextension].

![SAP-specifikus sikeres végrehajtását az Azure parancsmag Set-AzureRmVMAEMExtension][deployment-guide-figure-900]

Hello `Set-AzureRmVMAEMExtension` konfigurációs does összes hello lépéseket tooconfigure állomás SAP figyelését.

hello parancsfájl kimenete hello a következő információkat tartalmazza:

* A megerősítő, hogy hello operációsrendszer-lemez, és minden további adatlemezt megfigyelését van konfigurálva.
* következő két köszönőüzenetei erősítse meg a megadott tárfiók hello Storage Metrics konfigurációját.
* A kimenet egy sor hello frissítés állapota a hello tényleges hello figyelési konfiguráció biztosít.
* A kimenet egy másik sor megerősíti, hogy a hello konfigurálása telepíteni vagy frissíteni.
* hello kimenet utolsó sora tájékoztató. Azt ismerteti, hogy a tesztelési hello figyelési konfiguráció lehetőségeket.
* toocheck Azure fokozott figyelési lépéseket sikeresen végrehajtott, és adott hello Azure infrastruktúra adja meg a hello szükséges adatait, folytatásához hello készültség-ellenőrzés hello Azure fokozott Figyelőbővítmény az SAP, lásd: [Készültség-ellenőrzés Azure fokozott figyelés az SAP][deployment-guide-5.1].
* Várjon 15 – 30 percet, hogy Azure Diagnostics toocollect hello vonatkozó adatokat.

#### <a name="408f3779-f422-4413-82f8-c57a23b4fc2f"></a>Linux virtuális gépek az Azure parancssori felület
tooinstall hello Azure fokozott Figyelőbővítmény az SAP Azure parancssori felület használatával:

1. Telepítse az Azure CLI 1.0-s, leírtak szerint [telepítés hello Azure CLI 1.0][azure-cli].
2. Jelentkezzen be az Azure-fiókjával:

  ```
  azure login
  ```

3. Kapcsoló tooAzure Resource Manager módra:

  ```
  azure config mode arm
  ```

4. Az Azure bővített figyelés engedélyezése:

  ```
  azure vm enable-aem <resource-group-name> <vm-name>
  ```

5. Győződjön meg arról, hogy hello Azure fokozott Figyelőbővítmény hello Azure Linux virtuális gép aktív. Ellenőrizze, hogy a fájl hello \\var\\lib\\AzureEnhancedMonitor\\PerfCounters létezik-e. Ha létezik, a parancssorban futtassa a parancs-toodisplay hello Azure fokozott figyelő által gyűjtött adatok:
```
cat /var/lib/AzureEnhancedMonitor/PerfCounters
```

hello kimeneti így néz ki:
```
2;cpu;Current Hw Frequency;;0;2194.659;MHz;60;1444036656;saplnxmon;
2;cpu;Max Hw Frequency;;0;2194.659;MHz;0;1444036656;saplnxmon;
…
…
```

## <a name="564adb4f-5c95-4041-9616-6635e83a810b"></a>Ellenőrzések és a hibaelhárítás a végpont figyelő
Az Azure virtuális gép telepítése illetve hello megfelelő Azure felügyeleti infrastruktúra beállítása után ellenőrizze, hogy minden hello összetevői hello Azure fokozott Figyelőbővítmény van a várt módon működik.

Futtassa a hello készültség-ellenőrzés hello Azure fokozott Figyelőbővítmény az SAP, a [hello Azure fokozott Figyelőbővítmény az SAP készültség-ellenőrzés][deployment-guide-5.1]. Ha az összes készenléti ellenőrzésének az eredménye pozitív, és megfelelő teljesítményszámlálók jelennek meg az OK gombra, Azure figyelés beállítása sikeresen. Folytassa a SAP gazdagép-ügynök telepítése hello hello SAP jegyzetének leírtak [SAP erőforrások][deployment-guide-2.2]. Ha hello készültség-ellenőrzés azt jelzi, hogy számlálók hiányzó, hello állapot-ellenőrzés futtatása a hello Azure-figyelési infrastruktúra, a [Azure figyelési infrastruktúra konfigurálása az állapot-ellenőrzéssel] [ deployment-guide-5.2]. További hibaelhárítási lehetőségekkel, lásd: [az SAP figyelése Azure hibaelhárítási][deployment-guide-5.3].

### <a name="bb61ce92-8c5c-461f-8c53-39f5e5ed91f2"></a>Hello Azure fokozott Figyelőbővítmény az SAP készültség-ellenőrzés
Ez az ellenőrzés gondoskodik arról, hogy az SAP-alkalmazás belsejében szerepelhet a metrikák hello az alapul szolgáló infrastruktúra figyelése Azure által biztosított.

#### <a name="run-hello-readiness-check-on-a-windows-vm"></a>A virtuális gép Windows hello készültség-ellenőrzés futtatása

1.  Jelentkezzen be Azure virtuális gép toohello (rendszergazdai fiók használata nem szükséges).
2.  Nyisson meg egy parancssori ablakot.
3.  Hello parancssorban módosítsa a hello directory toohello telepítési mappájához hello Azure fokozott Figyelőbővítmény az SAP: C:\\csomagok\\beépülő modulok\\ Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;verzió >\\dobja el

  Hello *verzió* hello elérési toohello a bővítmény figyelési függően változhat. Ha hello figyelési hello telepítési mappájában, ellenőrizze a konfigurációt hello hello AzureEnhancedMonitoring Windows-szolgáltatás, a bővítmény különböző verzióinak mappák látja, és majd kapcsoló toohello mappa jelöléssel *elérési tooexecutable* .

  ![Szolgáltatás fut tulajdonságainak hello Azure fokozott Figyelőbővítmény SAP esetén][deployment-guide-figure-1000]

4.  Hello parancssorban futtassa **azperflib.exe** paraméter nélkül.

  > [!NOTE]
  > Azperflib.exe hurok fut, és frissíti a gyűjtött hello számlálók 60 másodpercenként. tooend hello hurok, Bezárás hello parancsot a parancssorablakba.
  >
  >

Ha hello Azure fokozott Figyelőbővítmény nincs telepítve, vagy hello AzureEnhancedMonitoring szolgáltatás nem fut, hello bővítmény konfigurációja nem megfelelő. Hogyan toodeploy hello bővítmény kapcsolatos részletes információkért lásd: [hibaelhárítás hello Azure felügyeleti infrastruktúra SAP][deployment-guide-5.3].

##### <a name="check-hello-output-of-azperflibexe"></a>Ellenőrizze a azperflib.exe hello kimenete
Azperflib.exe az alábbiakat mutatja be, minden Azure teljesítményszámlálók feltöltve az olyan SAP. Alján hello összegyűjtött számlálók hello listáját összegzéseket és az állapotát jelző hello Azure figyelő állapotának megjelenítése.

![Az állapot-ellenőrzéssel azperflib.exe, amely azt jelzi, hogy létezik-e nem jelez problémákat végrehajtásával kimenete][deployment-guide-figure-1100]
<a name="figure-11"></a>

Hello eredményt adott vissza hello ellenőrizze **számlálók teljes** kimenete, amely üres, és a jelentett **állapot**, hello megelőző. ábrán látható.

Értelmezni a hello eredményül kapott érték az alábbiak szerint:

| Azperflib.exe eredmény értékek | Azure figyelési állapot |
| --- | --- |
| **Az API-hívásokban - nem érhető el** | Mutat be, melyek nem érhető el vagy nem megfelelő toohello virtuálisgép-konfiguráció lehet, vagy hibák. Lásd: **állapot**. |
| **Számlálók összesen – üres** |lehet, hogy a következő két az Azure storage-számlálók hello üres: <ul><li>Tárolási olvasási Op késés Server MS</li><li>Tárolási olvasási Op késés E2E MS</li></ul>Az összes számláló értéknek kell lennie. |
| **Állapot ellenőrzése** |Ha csak OK vissza állapotát jeleníti meg **OK**. |
| **Diagnosztika** |Állapotfigyelő állapotával kapcsolatos részletes információkért. |

Ha hello **állapot** értéke nem **OK**, hello utasításait követve [Azure figyelési infrastruktúra konfigurálása az állapot-ellenőrzéssel] [ deployment-guide-5.2].

#### <a name="run-hello-readiness-check-on-a-linux-vm"></a>A Linux virtuális gép hello készültség-ellenőrzés futtatása

1.  Csatlakozás Azure virtuális gép toohello SSH használatával.

2.  Ellenőrizze a hello Azure fokozott Figyelőbővítmény hello kimenetét.

  a.  Futtassa a `more /var/lib/AzureEnhancedMonitor/PerfCounters` parancsot.

   **A várt eredmény**: teljesítményszámlálók listája értéket ad vissza. hello fájlt nem lehet üres.

 b. Futtassa a `cat /var/lib/AzureEnhancedMonitor/PerfCounters | grep Error` parancsot.

   **A várt eredmény**: értéket ad vissza egy sorának hello hiba **nincs**, például **3; config; Hiba történt; 0, 0; Nincs; 0; 1456416792; teszt-servercs;**

  c. Futtassa a `more /var/lib/AzureEnhancedMonitor/LatestErrorRecord` parancsot.

    **A várt eredmény**: ad vissza, üres vagy nem létezik.

Ha hello előző ellenőrzése sikertelen volt, futtassa a további ellenőrzést:

1.  Győződjön meg arról, hogy hello waagent telepítve és engedélyezve van.

  a.  Futtassa a `sudo ls -al /var/lib/waagent/` parancsot.

      **A várt eredmény**: hello tartalom hello waagent könyvtár jeleníti meg.

  b.  Futtassa a `ps -ax | grep waagent` parancsot.

   **A várt eredmény**: egy bejegyzést, jeleníti meg:`python /usr/sbin/waagent -daemon`

3.   Győződjön meg arról, hogy hello Azure fokozott Figyelőbővítmény telepítve és fut-e.

  a.  Futtassa a `sudo sh -c 'ls -al /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-*/'` parancsot.

    **A várt eredmény**: hello tartalom hello Azure fokozott Figyelőbővítmény könyvtár jeleníti meg.

  b. Futtassa a `ps -ax | grep AzureEnhanced` parancsot.

     **A várt eredmény**: egy bejegyzést, jeleníti meg:`python /var/lib/waagent/Microsoft.OSTCExtensions.AzureEnhancedMonitorForLinux-2.0.0.2/handler.py daemon`

3. SAP Megjegyzés SAP Állomásügynök telepítse [1031096], és hello kimenete ellenőrizze `saposcol`.

  a.  Futtassa a `/usr/sap/hostctrl/exe/saposcol -d` parancsot.

  b.  Futtassa a `dump ccm` parancsot.

  c.  Ellenőrizze, hogy hello **Virtualization_Configuration\Enhanced figyelési hozzáférés** metrika **igaz**.

Ha már van telepítve egy SAP NetWeaver ABAP alkalmazáskiszolgálót, nyissa meg a tranzakció ST06, és ellenőrizze a fokozott ellenőrzés engedélyezve van-e.

Ha minden ellenőrzés nem sikerül, és hogyan tooredeploy hello bővítmény kapcsolatos részletes információkért lásd: [hibaelhárítás hello Azure felügyeleti infrastruktúra SAP][deployment-guide-5.3].

### <a name="e2d592ff-b4ea-4a53-a91a-e5521edb6cd1"></a>Hello Azure figyelési infrastruktúra konfigurálása állapotának ellenőrzése
Ha hello figyelési adatok némelyike nem megfelelő, ahogy azt érkezik hello teszt ismertetett [készültség-ellenőrzés Azure fokozott figyelés az SAP][deployment-guide-5.1]- ben futtassa hello `Test-AzureRmVMAEMExtension` parancsmag toocheck e hello figyelési infrastruktúra és a bővítmény figyelésének SAP hello Azure helyesen vannak konfigurálva.

1.  Győződjön meg arról, hogy telepítette-e az hello hello Azure PowerShell-parancsmag legújabb verzióját, a [telepítése Azure PowerShell-parancsmagok][deployment-guide-4.1].
2.  A következő PowerShell-parancsmag futtatási hello. Rendelkezésre álló környezeteket listájának megtekintéséhez futtassa a hello parancsmagot `Get-AzureRmEnvironment`. globális Azure, válasszon toouse hello **AzureCloud** környezetben. Az Azure Kínában, válassza ki a **AzureChinaCloud**.
  ```powershell
  $env = Get-AzureRmEnvironment -Name <name of hello environment>
  Login-AzureRmAccount -Environment $env
  Set-AzureRmContext -SubscriptionName <subscription name>
  Test-AzureRmVMAEMExtension -ResourceGroupName <resource group name> -VMName <virtual machine name>
  ```

3.  Adja meg a fiók adatait, és azonosíthatja a hello Azure virtuális géphez.

  ![SAP-specifikus bemeneti oldalán Azure parancsmag teszt-VMConfigForSAP_GUI][deployment-guide-figure-1200]

4. hello parancsfájl tesztek hello hello virtuális gép konfigurációját választja.

  ![Az SAP hello Azure felügyeleti infrastruktúra a sikeres vizsgálat kimeneti][deployment-guide-figure-1300]

Győződjön meg arról, hogy minden rendszerállapot-ellenőrzés eredménye **OK**. Ha nem jelennek meg az egyes ellenőrzések **OK**, hello frissítés parancsmag futtatása a [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5]. Várjon 15 percig, és ismétlési hello ellenőrzések ismertetett [készültség-ellenőrzés Azure fokozott figyelés az SAP] [ deployment-guide-5.1] és [Azure figyelési infrastruktúra konfigurációÁllapot-ellenőrzése] [deployment-guide-5.2]. Hello ellenőrzések még néhány vagy az összes számlálók problémát jelez, ha [hibaelhárítás hello Azure felügyeleti infrastruktúra SAP][deployment-guide-5.3].

### <a name="fe25a7da-4e4e-4388-8907-8abc2d33cfd8"></a>Az SAP hello Azure felügyeleti infrastruktúra hibaelhárítása

#### <a name="windowslogowindows-azure-performance-counters-do-not-show-up-at-all"></a>![Windows][Logo_Windows] Az Azure teljesítményszámlálók nem jelennek meg minden
hello AzureEnhancedMonitoring Windows-szolgáltatás az Azure-ban teljesítménymutatók gyűjti. Ha hello szolgáltatás nincs megfelelően telepítve, vagy ha a virtuális gép nem fut, nincs teljesítménymutatók gyűjthetők össze.

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a>az Azure fokozott Figyelőbővítmény hello hello-telepítési könyvtár nem üres

###### <a name="issue"></a>Probléma
hello telepítőkönyvtár C:\\csomagok\\beépülő modulok\\Microsoft.AzureCAT.AzureEnhancedMonitoring.AzureCATExtensionHandler\\&lt;verzió >\\eldobási értéke üres.

###### <a name="solution"></a>Megoldás
hello bővítmény nincs telepítve. Határozza meg, hogy ez-e a proxy problémát (a korábban ismertetett). Szükség lehet a toorestart hello gép vagy Újrafuttatás hello `Set-AzureRmVMAEMExtension` konfigurációs parancsfájl.

##### <a name="service-for-azure-enhanced-monitoring-does-not-exist"></a>Azure fokozott ellenőrzésére szolgáltatás nem létezik.

###### <a name="issue"></a>Probléma
hello AzureEnhancedMonitoring Windows szolgáltatás nem létezik.

Azperflib.exe kimenet hibát jelez:

![Azperflib.exe végrehajtásának azt jelzi, hogy hello Azure fokozott Figyelőbővítmény az SAP-hello szolgáltatása nem fut.][deployment-guide-figure-1400]
<a name="figure-14"></a>

###### <a name="solution"></a>Megoldás
Ha hello szolgáltatás nem létezik, hello Azure fokozott Figyelőbővítmény az SAP nem lett megfelelően telepítve. Az üzembe helyezési forgatókönyvnek a leírt hello lépéseket követve telepítse újra a hello bővítmény [szituációkban az SAP, az Azure virtuális gépek][deployment-guide-3].

Hello kiterjesztéssel, telepít egy óra múlva, miután ismételt ellenőrzéséhez hello Azure teljesítményszámlálók igénybe hello Azure virtuális Gépen.

##### <a name="service-for-azure-enhanced-monitoring-exists-but-fails-toostart"></a>Azure fokozott ellenőrzésére szolgáltatás létezik, de nem sikerül toostart

###### <a name="issue"></a>Probléma
hello AzureEnhancedMonitoring Windows-szolgáltatás létezik-e és engedélyezve van, de nem sikerül toostart. További információkért ellenőrizze a hello alkalmazások eseménynaplójában.

###### <a name="solution"></a>Megoldás
hello konfigurációja nem megfelelő. Indítsa újra a hello bővítményt a virtuális gép, hello figyelési ismertetett módon [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5].

#### <a name="windowslogowindows-some-azure-performance-counters-are-missing"></a>![Windows][Logo_Windows] Egyes Azure teljesítményszámlálók hiányoznak.
hello AzureEnhancedMonitoring Windows-szolgáltatás az Azure-ban teljesítménymutatók gyűjti. hello szolgáltatás több forrásból származó adatokat lekérdezi. Bizonyos konfigurációs adatok gyűjtése helyileg, és az Azure Diagnostics olvasható néhány teljesítménymutatók. A naplózási hello tárolási előfizetési szinten tárolási számlálók használtak.

Ha hibaelhárítása SAP Megjegyzés használatával [1999351] nem hello probléma elhárításához futtassa újra a hello `Set-AzureRmVMAEMExtension` konfigurációs parancsfájl. Lehetséges, hogy óra toowait mert storage analytics vagy a diagnosztika számlálók előfordulhat, hogy nem jönnek létre, azonnal követően engedélyezve vannak. Ha hello a probléma továbbra is fennáll, nyissa meg egy SAP felhasználói támogatási üzenetet hello Windows BC-OP-NT-AZR összetevő vagy BC-OP-LNX-AZR Linux virtuális gép.

#### <a name="linuxlogolinux-azure-performance-counters-do-not-show-up-at-all"></a>![Linux][Logo_Linux] Az Azure teljesítményszámlálók nem jelennek meg minden
Az Azure-ban teljesítménymutatók démon által gyűjtött. Ha hello démon nem fut, nincs teljesítménymutatók gyűjthetők össze.

##### <a name="hello-installation-directory-of-hello-azure-enhanced-monitoring-extension-is-empty"></a>az Azure fokozott figyelőbővítmény hello hello-telepítési könyvtár nem üres

###### <a name="issue"></a>Probléma
hello directory \\var\\lib\\waagent\\ nincs hello Azure fokozott figyelőbővítmény alkönyvtárat.

###### <a name="solution"></a>Megoldás
hello bővítmény nincs telepítve. Határozza meg, hogy ez-e a proxy problémát (a korábban ismertetett). Szükség lehet a toorestart hello gép és/vagy Újrafuttatás hello `Set-AzureRmVMAEMExtension` konfigurációs parancsfájl.

#### <a name="linuxlogolinux-some-azure-performance-counters-are-missing"></a>![Linux][Logo_Linux] Egyes Azure teljesítményszámlálók hiányoznak.
Adatok beolvasása számos forrásból származó démon által gyűjtött teljesítménymutatók az Azure-ban. Bizonyos konfigurációs adatok gyűjtése helyileg, és az Azure Diagnostics olvasható néhány teljesítménymutatók. Tárolási számlálók hello naplófájlok tárolási előfizetését az határozza meg.

Teljesebb és ismert problémák listáját, lásd: SAP Megjegyzés [1999351], amely tartalmaz további hibaelhárítási információért Azure figyelésének továbbfejlesztett SAP.

Ha hibaelhárítása SAP Megjegyzés használatával [1999351] nem hello probléma megoldásához, futtassa újra a hello `Set-AzureRmVMAEMExtension` konfigurációs parancsfájl leírtak [konfigurálása hello Azure fokozott Figyelőbővítmény az SAP][deployment-guide-4.5]. Ezért előfordulhat, hogy rendelkezik toowait egy óráig storage analytics vagy a diagnosztika számlálók előfordulhat, hogy nem jönnek létre, azonnal követően engedélyezve vannak. Ha hello a probléma továbbra is fennáll, nyissa meg egy SAP felhasználói támogatási üzenetet hello Windows BC-OP-NT-AZR összetevő vagy BC-OP-LNX-AZR Linux virtuális gép.
