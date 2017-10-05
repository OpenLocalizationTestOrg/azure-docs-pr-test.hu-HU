---
title: "SAP NetWeaver az Azure virtuális gépeken futó – tervezése és megvalósítása |} Microsoft Docs"
description: "SAP NetWeaver az Azure virtuális gépek (VM) – tervezési és megvalósítási útmutató"
services: virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 2b58a419-c892-4722-8cb6-2f8beef4430c
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0b5968189512d3ca936c0e916274e1df057afb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sap-netweaver-on-azure-windows-virtual-machines-vms--planning-and-implementation-guide"></a>SAP NetWeaver a Azure Windows virtuális gépek (VM) – tervezési és megvalósítási útmutató
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
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[azure-cli]:../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:sap-dbms-guide.md
[dbms-guide-2.1]:sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:sap-deployment-guide.md
[deployment-guide-2.2]:sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:sap-get-started.md
[getting-started-dbms]:sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:classic/sap-get-started.mdsap-dbms-guide.md
[getting-started-windows-classic-dbms]:classic/sap-get-started.mdsap-dbms-guide.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:classic/sap-get-started.mdsap-dbms-guide.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:classic/sap-get-started.mdsap-dbms-guide.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:classic/sap-get-started.mdsap-dbms-guide.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:classic/sap-get-started.mdsap-dbms-guide.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:sap-planning-guide.md
[planning-guide-1.2]:sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11.4]:sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[powershell-install-configure]:/powershell/azureps-cmdlets-docs
[resource-group-authoring-templates]:../../resource-group-authoring-templates.md
[resource-group-overview]:../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../storage/storage-premium-storage.md
[storage-redundancy]:../../storage/storage-redundancy.md
[storage-scalability-targets]:../../storage/storage-scalability-targets.md
[storage-use-azcopy]:../../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:../linux/attach-disk-portal.md
[virtual-machines-windows-attach-disk-portal]:../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../linux/capture-image.md#step-2-create-vm-image
[virtual-machines-windows-capture-image]:../virtual-machines-windows-create-vm-generalized.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../virtual-machines-windows-create-vm-generalized.md
[virtual-machines-linux-configure-lvm]:../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../linux/update-agent.md
[virtual-machines-manage-availability]:../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:classic/ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:classic/ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:./sql/virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:./sql/virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:./sql/virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:upload-image.md
[virtual-machines-windows-tutorial]:../virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../xplat-cli-azure-resource-manager.md

A Microsoft Azure lehetővé teszi a vállalatok szerezni a számítási és tárolási erőforrásokat minimális időben hosszadalmas beszerzési ciklusok nélkül. Az Azure virtuális gépek lehetővé teszi a vállalatok például SAP NetWeaver-alapú alkalmazások az Azure klasszikus alkalmazások, központi telepítéséhez, és a megbízhatóság és rendelkezésre állás kiterjesztése anélkül, hogy további erőforrás érhető el a helyszíni. Az Azure virtuális gép szolgáltatások közötti kapcsolatot nyújthassanak, amely lehetővé teszi a vállalatok Azure virtuális gépek aktívan integrálja a helyi tartomány, a Magánfelhők és az SAP rendszer fekvő is támogatja.
Ez a dokumentum ismerteti a Microsoft Azure virtuális gép alapjait és biztosít egy SAP NetWeaver telepítések az Azure-ban tervezési és megvalósítási szempontok segédlet és ilyen kell tényleges megkezdése előtt olvassa el a dokumentumot SAP NetWeaver Azure példányai.
A dokumentum kiegészíti az SAP-dokumentáció és az SAP megadott platformok telepítések és a SAP szoftver központi telepítését az elsődleges erőforrások képviselik.

## <a name="summary"></a>Összefoglalás
A felhőalapú számítástechnika széles körben használt fogalom, amely egyre fontosabbá válik az informatikai iparágban, a kisvállalatok esetében ugyanúgy, mint a nagy, nemzetközi vállalatok számára.

A Microsoft Azure a Microsoft új lehetőségek széles skáláját nyújtó felhőszolgáltatás-platformja. Most már az ügyfelek képesek gyors kiépítése és deaktiválás rendelkezés alkalmazások felhőalapú szolgáltatásként úgy, hogy azok nem technikai vagy költségvetési korlátozások korlátozódik. A vállalatoknak nem kell időt és forrásokat befektetni a hardverinfrastruktúrába: ehelyett az alkalmazásra, az üzleti folyamatokra és az ügyfeleknek és felhasználóknak nyújtott értékre koncentrálhatnak.

A Microsoft Azure Virtual Machine Services szolgáltatással a Microsoft átfogó szolgáltatott infrastruktúra (IaaS) platformot kínál. Az Azure virtuális gépek támogatják az SAP NetWeaver-alapú alkalmazásokat (IaaS). E tanulmány azt ismerteti, hogyan tervezésével és megvalósításával SAP NetWeaver-alapú alkalmazások a Microsoft Azure-ban, a platform választott.

Maga a dokumentum összpontosítanak két fő szempontjait:

* Az első rész ismerteti, két támogatott üzembe helyezési mintát SAP NetWeaver alapú alkalmazásokhoz az Azure-on. Azt is leírja az SAP üzemelő példányokkal figyelembe Azure általános kezelését.
* A második rész részletesen ismerteti az első részében leírt két különböző forgatókönyv megvalósításához.

További források című fejezet [erőforrások] [ planning-guide-1.2] ebben a dokumentumban.

### <a name="definitions-upfront"></a>Definíciók előzetes megfizetése esetén
A dokumentum a következő kifejezéseket használjuk:

* IaaS: Szolgáltatott infrastruktúra.
* A PaaS: Platform szolgáltatásként.
* SaaS: Szolgáltatott szoftver.
* ARM: Az Azure Resource Manager
* SAP összetevő: az egyes SAP alkalmazások például ECC, a Sávszélesség, a megoldás Manager vagy a EP  SAP-összetevők a hagyományos ABAP vagy Java technológiák vagy egy nem NetWeaver alapú alkalmazás, például üzleti objektumok is alapulhat.
* SAP-környezetben: egy vagy több SAP összetevők logikailag egy csoportba például fejlesztési, a QAS, a képzési, a vész-Helyreállítási vagy a termelési üzleti függvény végrehajtásához.
* SAP fekvő: Ez vonatkozik az egész SAP eszközök az ügyfél informatikai fekvő. Az SAP fekvő minden üzemi és nem éles környezetben tartalmaz.
* SAP-verzió: DBMS réteg és kombinációja alkalmazásréteg például egy SAP ERP fejlesztőrendszer SAP BW gazdagépes tesztrendszer, SAP CRM éles rendszer, stb. Azure telepítések esetén nem támogatott a két réteg között a helyszíni és az Azure felosztása. Ez azt jelenti, hogy egy SAP rendszer vagy a helyszínen telepített vagy telepítve van az Azure-ban. Azonban telepítheti a másik egy SAP fekvő Azure vagy a helyszíni rendszeren. Például sikerült telepíteni a SAP CRM-fejlesztői és tesztrendszerek Azure, de az SAP CRM éles rendszer helyszíni.
* Csak felhőalapú telepítési: egy központi telepítést, amikor az Azure-előfizetés nem csatlakozik a pont-pont vagy ExpressRoute-kapcsolat a helyszíni hálózati infrastruktúrára. Közös Azure dokumentációja az ilyen típusú központi telepítések egyaránt "Csak felhőalapú" telepítések leírása. Ezzel a módszerrel telepített virtuális gépek az interneten, és egy nyilvános IP-címet és/vagy egy nyilvános DNS-nevet a virtuális gépeket az Azure-ban rendelt keresztül érhetők el. A Microsoft Windows a helyszíni Active Directory (AD) és a DNS nem bővítette ki az Azure-ba, az ilyen típusú központi telepítések. Ezért a virtuális gépek nincsenek a helyszíni Active Directory részét. Is igaz Linux-megvalósítások esetében pl. OpenLDAP + Kerberos használatával.

> [!NOTE]
> Ebben a dokumentumban csak felhőalapú telepítések definiált teljes SAP tájak kizárólag az Azure Active Directory kiterjesztés nélkül futnak / OpenLDAP vagy a nyilvános felhőbe helyszíni névfeloldás. Csak felhőalapú konfigurációk nem támogatottak a termelési SAP rendszerek vagy konfigurációk, ahol az SAP STM vagy más helyszíni erőforrásokat kell használható Azure és a helyszínen található erőforrások üzemeltetett SAP-rendszerek között.
>
>

* Létesítmények közötti: Bemutatja egy olyan forgatókönyvet, ahol a virtuális gépek Azure-előfizetéshez, amely rendelkezik pont-pont, többhelyes vagy a helyszíni datacenter(s) és az Azure között ExpressRoute kapcsolat van telepítve. Közös Azure dokumentációja, az ilyen típusú központi telepítések egyaránt létesítmények közötti forgatókönyv leírása. A OK kibővítheti a helyszíni tartományok: a kapcsolat a helyszíni Active Directory / OpenLDAP és a helyszíni DNS az Azure. A helyszíni fekvő az időtartam, az előfizetéshez tartozó Azure eszközökre. Ha ezt a bővítményt, a virtuális gépek a helyi tartomány része lehet. A helyi tartomány tartományi felhasználók férhetnek hozzá a kiszolgálókat, és szolgáltatásokat futtathatja virtuális gépek (például adatbázis-kezelő szolgáltatás). Telepített virtuális gépek a helyszíni és az Azure telepített virtuális gépek közötti kommunikációt és a névfeloldás lehetőség. Ez a forgatókönyv a legtöbb SAP eszközök telepítendő várhatóan.  Lásd: [ez] [ vpn-gateway-cross-premises-options] cikk és [ez] [ vpn-gateway-site-to-site-create] további információt.

> [!NOTE]
> Létesítmények közötti telepítések SAP rendszert futtató Azure virtuális gépek esetén a helyszíni-tartománybeli tagsággal SAP rendszerek éles SAP rendszerek támogatottak. Létesítmények közötti konfigurációk támogatottak a kijelzők telepítése, vagy végezze el az SAP tájak az Azure. A teljes SAP fekvő az Azure-ban futó szükség van arra, azokat a virtuális gépek helyszíni tartományban és REKLÁMOK részévé / OpenLDAP. A dokumentáció korábbi verzióiban megtartásról kapcsolatos hibrid-IT forgatókönyvek, ahol a "Hibrid" kifejezés a tényt, hogy van-e a létesítmények közötti kapcsolat a helyszíni és az Azure közötti a feltört eszköz. Plusz a tényt, hogy a virtuális gépeket az Azure-ban a helyszíni Active Directory részét / OpenLDAP.
>
>

Néhány Microsoft dokumentációjában létesítmények közötti forgatókönyv különösen az adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ konfiguráció egy kicsit másképp ismerteti. Az SAP esetén kapcsolódó dokumentumok, csak a létesítmények közötti forgatókönyv forrni kezd, hogy a pont-pont vagy titkos (ExpressRoute) kapcsolatot és azt a tényt, hogy a SAP fekvő elosztása a helyszíni és az Azure között.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Erőforrások
Az alábbi kiegészítő útmutatók érhetők el a témakör az SAP-telepítések az Azure-on:

* [SAP NetWeaver az Azure virtuális gépek (VM) – tervezési és megvalósítási útmutató (Ez a dokumentum)][planning-guide]
* [SAP NetWeaver az Azure virtuális gépek (VM) – telepítési útmutató][deployment-guide]
* [SAP NetWeaver az Azure virtuális gépek (VM) – adatbázis-kezelő telepítési útmutatója][dbms-guide]
* [SAP NetWeaver az Azure virtuális gépek (VM) – magas rendelkezésre állású telepítési útmutatója][ha-guide]

> [!IMPORTANT]
> Ahol csak lehetséges egy hivatkozást a hivatkozó SAP telepítési útmutató használatos (hivatkozás InstGuide-01, lásd: <http://service.sap.com/instguides>). Az Előfeltételek és a telepítési folyamat ismét, az SAP NetWeaver telepítési útmutatók mindig kell olvassa el, mivel ez a dokumentum csak hozzá van rendelve egy Microsoft Azure virtuális gépen telepített SAP NetWeaver rendszerek az egyes feladatok.
>
>

Az alábbi SAP megjegyzések Azure SAP témakör kapcsolódnak:

| Megjegyzés száma | Cím |
| --- | --- |
| [1928533] |Az Azure-on SAP-alkalmazásokból: támogatott termékek és méretezése |
| [2015553] |A Microsoft Azure SAP: Előfeltételek támogatja |
| [1999351] |Hibaelhárítási továbbfejlesztett Azure megfigyelése az SAP |
| [2178632] |Kulcs figyelési metrikákat az SAP, a Microsoft Azure |
| [1409604] |A Windows virtualizálási: fokozott figyelése |
| [2191498] |Az Azure Linux SAP: fokozott figyelése |
| [2243692] |A Microsoft Azure (IaaS) virtuális gép Linux: SAP licenc problémák |
| [1984787] |SUSE LINUX Enterprise Server 12: Telepítési megjegyzések |
| [2002167] |Red Hat Enterprise Linux 7.x: telepítés és frissítés |

Emellett olvassa el a [Állapotváltozás Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux tartalmazó összes SAP-jegyzetet.

Általános alapértelmezett korlátozások és az Azure-előfizetések maximális korlátai található [Ez a cikk][azure-subscription-service-limits-subscription]

## <a name="possible-scenarios"></a>A következő eljárások
SAP gyakran előforduló, a legtöbb létfontosságú alkalmazások belül vállalatok számára rendelkezésre álló. Az architektúra és a műveletek az alkalmazások többnyire nagyon bonyolult, és győződjön meg arról, hogy megfelel a követelményeknek, a rendelkezésre állás és teljesítmény fontos.

Így a vállalat rendelkezik gondolja, hogy alaposan arról, hogy mely alkalmazások futtathatja a kiválasztott felhő szolgáltató független nyilvános felhő környezetben.

SAP NetWeaver telepítéséhez lehetséges rendszertípusok-alapú alkalmazások nyilvános felhőben környezetekben alábbiakban találhatók:

1. Közepes méretű éles rendszerek esetén.
2. Fejlesztési rendszerek
3. Tesztelési rendszerek
4. Prototípus rendszerek
5. Tanulási / bemutató rendszerek

Sikeresen üzembe helyezésének SAP rendszerek Azure IaaS vagy infrastruktúra-szolgáltatási általában, fontos a hagyományos outsourcers vagy a szolgáltatók által kínált és infrastruktúra-szolgáltatási ajánlatok között jelentős különbségek megismeréséhez. Mivel a hagyományos szolgáltató vagy outsourcer fog támogató infrastruktúra (hálózati, tárolási és típus) egy ügyfél szeretne üzemeltetni a munkaterhelés, feladata ehelyett az ügyfél válassza ki a megfelelő munkaterhelés IaaS telepítésekhez.

Első lépésként szükséges ellenőrizze az alábbiakat:

* Az SAP támogatott Azure VM típusai
* Az SAP termékek/verziókban támogatott Azure-on
* A támogatott operációs rendszer és adatbázis-kezelő feloldja az adott SAP kiadásokban az Azure-ban
* Különböző Azure-SKU által biztosított SAP átviteli sebesség

Ezekre a kérdésekre adott válaszokat az SAP megjegyzés olvasható [1928533].

A második lépésben az Azure erőforrás- és sávszélesség korlátozások kell a helyszíni rendszerekben tényleges erőforrás-felhasználásának össze. Ezért szükséges lehet az Azure SAP területén a támogatott típusok különböző lehetőségeinek megismerése:

* Virtuális gép különböző típusú a CPU és memória-erőforrásokat és
* Virtuális gép különböző típusú IOPS sávszélesség és
* Virtuális gép különböző típusú hálózati képességeit.

Az adatok legnagyobb található [Itt][virtual-machines-sizes]

Ne feledje, hogy a fenti hivatkozás szerepel a korlátozások felső határa. Ez nem jelenti azt, hogy az erőforrásokat, például IOPS korlátairól biztosítható, hogy minden esetben. A kivételek, ha a CPU és memória-erőforrások a kiválasztott Virtuálisgép-típussá áll. A SAP által támogatott virtuális gép esetében a CPU és memória-erőforrások fogyasztás belül a virtuális gép számára fenntartott és használja, így bármikor elérhető.

A Microsoft Azure platformon IaaS más platformokon, például a egy több-bérlős platformja. Ez azt jelenti, hogy a tárolási, hálózati és egyéb erőforrások megosztott bérlők között. Intelligens sávszélesség-szabályozás és a kvóta logika szerint egy bérlői megakadályozza más bérlőket (zajos szomszédos) teljesítményét érintő drasztikus módon. Bár az Azure-ban programot próbál eltérések az észlelt sávszélesség alacsony, magas megosztott platformokhoz általában a nagyobb eltérésekre esetleges erőforrás/sávszélesség rendelkezésre állása, mint az ügyfelek a sok megtartása szerepelnek a helyszíni telepítések. Ennek köszönhetően Ön is szembesülhet tanúsítványinformációit (a kötet, valamint késés) hálózati vagy tárolási i/o sávszélesség különböző szintjei perc perc. A valószínűsége annak, hogy egy SAP rendszer Azure volt tapasztalható eltérésekre nagyobb, mint a helyszíni rendszer kell figyelembe kell venni.

Utolsó lépés a rendelkezésre állási követelmények kiértékeléséhez. Előfordulhat, hogy az alapul szolgáló Azure-infrastruktúra frissített, valamint újra kell indítani a virtuális gépeket futtató gazdagépek szükséges. Ezekben az esetekben meg azokon a gazdagépeken futó virtuális gépeket szeretné kell leállításra és újraindításra is. Ilyen karbantartás ütemezése munkaidejében kiegészítő egy adott régióban történik, de viszonylag nagy a potenciális ablak néhány óra során, ami egy újraindul. Nincsenek belül vagy azok egy részét a frissítések hatásának mérsékléséhez konfigurálható az Azure platformon különböző technológiákat. Jövőbeli fejlesztések az Azure platform, adatbázis-kezelő és az SAP alkalmazás tervezték, hogy az ilyen újraindítást gyakorolt hatásának minimalizálása érdekében.

Ahhoz, hogy egy SAP Azure rendszer sikeres telepítéséhez a helyszíni SAP rendszer(ek) operációs rendszer, adatbázis, és SAP-alkalmazásokból szerepelnie kell a SAP Azure támogatási mátrix a elférjen belül az erőforrásokat biztosít az Azure-infrastruktúra és amely is használhatók a rendelkezésre állási SLA-k a Microsoft Azure biztosít. Ezekhez a rendszerekhez meghatározott kell döntenie, hogy a következő két üzembe helyezési forgatókönyveket egyik.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Csak felhőalapú - függőségek a helyszíni ügyfél hálózaton nélkül az Azure virtuális gépek telepítéséhez
![Az SAP bemutató vagy Azure szolgáltatásba telepített képzési forgatókönyv egyetlen VM][planning-guide-figure-100]

Ez a forgatókönyv része jellemzően oktatási anyag vagy bemutató rendszerek, ahol telepítve vannak a SAP és nem SAP szoftverek összes összetevői belül egy virtuális. Éles SAP rendszerek nem támogatottak ebben a telepítési forgatókönyvben. Ebben a forgatókönyvben általában megfelel az alábbi követelményeknek:

* A virtuális gépek magukat a nyilvános hálózaton keresztül érhetők el. A bemutatók vagy oktatási anyag tartalmát, vagy az ügyfél a tulajdonos vagy a vállalat a helyszíni hálózatra a virtuális gépeken futtatott alkalmazások közvetlen kapcsolattal nincs szükség.
* Esetén a oktatási anyag vagy bemutató forgatókönyvet képviselő több virtuális gép hálózati kommunikációt és a névfeloldás kell a virtuális gépek is működnek. De a beállított virtuális gépek közötti kommunikáció kell lehessen, hogy a virtuális gépek több készleteinek egymás mellett beavatkozás nélkül telepíthetők.  
* Internetkapcsolat szükség a végfelhasználó számára az Azure-ban üzemeltetett virtuális gépek való távoli bejelentkezés. Attól függően, hogy a vendég operációs rendszer, Terminal Services/távoli asztali szolgáltatások vagy VNC ssh eléréséhez használt a virtuális Gépet, a képzési feladatok teljesítése, vagy hajtsa végre a különböző demók. Ha az SAP 3200, 3300 & 3600 részleg például portok is elérhetővé tehető az SAP alkalmazáspéldány elérhető bármely internethez csatlakoztatott asztalról.
* Az SAP rendszer(ek) (és a VM(s)) jelentik a önálló Azure, amely csak nyilvános internetkapcsolatra van szükség a felhasználói hozzáférés, és nem szükséges más virtuális gépek Azure-ban való kapcsolat esetén.
* SAPGUI és a böngésző telepítve és fut, közvetlenül a virtuális Gépen.
* A gyors alaphelyzetbe állítani a virtuális gépek az eredeti állapotra és új központi telepítését az eredeti állapotban ismét szükség.
* Bemutató és képzési forgatókönyvek, amelyek több virtuális gép, egy Active Directory rendszer megvalósítani / OpenLDAP és/vagy a DNS szolgáltatásra szükség az egyes virtuális gépek.

![A virtuális gép egy bemutató vagy egy Azure-Felhőszolgáltatásban képzési forgatókönyv képviselő csoport][planning-guide-figure-200]

Fontos vegye figyelembe, hogy a virtuális gép van, a készlet minden egyes kell párhuzamosan, ahol a virtuális gép nevét, a készlet minden egyes azonosak telepíthető.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure-e a követelményeknek, alatt teljesen integrálva a helyszíni hálózat
![A pont-pont kapcsolatban (létesítmények közötti) VPN][planning-guide-figure-300]

Ebben a forgatókönyvben a létesítmények közötti forgatókönyv számos lehetséges telepítési mintákat. Egyszerűen fut az egyes részei a SAP fekvő a helyszíni és az SAP többi részével fekvő Azure leírható. Az SAP-összetevők része az Azure-on futnak tény minden szempontját kell lennie átlátszó, a végfelhasználók számára. Ezért a SAP átviteli javítási rendszer (STM) RFC kommunikációs, nyomtatási, (például egyszeri bejelentkezés) biztonsági, stb működnek zökkenőmentesen az Azure-on futó SAP rendszerekhez. De a létesítmények közötti forgatókönyv is egy olyan forgatókönyvet, ahol a teljes SAP fekvő futtatja az Azure-ban a felhasználói tartomány és a DNS kiterjeszthetőek az Azure ismerteti.

> [!NOTE]
> Ez az a környezetben, amely hatékony SAP rendszereket futtató támogatott.
>
>

Olvasási [Ez a cikk] [ vpn-gateway-create-site-to-site-rm-powershell] bővebben a helyszíni hálózat csatlakoztatása a Microsoft Azure

> [!IMPORTANT]
> Azt is van szó létesítmények közötti forgatókönyv Azure és a helyszíni felhasználói telepítés között, azt keresi, az egész SAP rendszerek részletességű összegzése. Forgatókönyvek, amelyek *nem támogatott* létesítmények közötti forgatókönyv esetén:
>
> * Különböző központi telepítési módszer különböző rétegeinek SAP-alkalmazásokból fut. Például a DBMS réteg a helyszínen, de az SAP alkalmazásréteg futó Azure virtuális gépeken, vagy fordítva telepített virtuális gépek.
> * Azure-ban és egy helyszíni SAP réteg néhány összetevőt. Például a felosztás az SAP alkalmazásréteg-hez és az Azure virtuális gépek közötti példányai.
> * SAP példányát egy olyan rendszert több Azure-régiókat felett futó virtuális gépeket eloszlásáról nem támogatott.
>
> Ezek a korlátozások oka egy SAP rendszerben, különösen az alkalmazáspéldányok és az adatbázis-kezelő réteg SAP rendszer között nagy teljesítményű nagyon alacsony késleltetésű hálózathoz vonatkozó követelmény.
>
>

### <a name="supported-os-and-database-releases"></a>Támogatott operációs rendszer és adatbázis-kiadások
* Microsoft server szoftver esetében az Azure virtuális gép szolgáltatásokat, ez a cikk szerepel a támogatott: <http://support.microsoft.com/kb/2721672>.
* Támogatott operációs rendszer kiadások, adatbázis rendszer verziókban támogatott Azure virtuális gép szolgáltatások SAP szoftver együtt vannak dokumentálva SAP Megjegyzés [1928533].
* SAP-alkalmazásokból és -verziókat támogatja a virtuális gép Azure-szolgáltatásokra ismertetett SAP Megjegyzés [1928533].
* Csak 64 bites bináris fiókként szándékozik futtatni az Azure-ban a Vendég virtuális gépek SAP forgatókönyvek esetén támogatottak. Ez azt is jelenti, hogy csak a 64 bites SAP-alkalmazásokból és az adatbázisok támogatottak.

## <a name="microsoft-azure-virtual-machine-services"></a>Virtuális gép Microsoft Azure-szolgáltatások
A Microsoft Azure platform az egy internet-skálázási szolgáltatások felhőplatform birtokolt, és a Microsoft azon adatközpontjainak üzemeltetni. A platform a Microsoft Azure virtuális gép szolgáltatások (infrastruktúra-szolgáltatás, vagy IaaS) és a gazdag platformok egy Platformszolgáltatási képességek egy készletét tartalmazza.

Az Azure platformon csökkenti a kezdeti technológia szükségességét, és infrastruktúra a későbbi szoftvervásárlások. Egyszerűbbé teszi az karbantartásáért, valamint működő alkalmazások igény szerinti számítási és tárolási, méretezhető és kezelheti a webes alkalmazás és a csatlakoztatott alkalmazások megadásával. Infrastruktúra-kezelés a automatizált egy platform, amely a magas rendelkezésre állás és a dinamikus méretezés beállítással, a használatalapú árképzési modellt használati igényeinek megfelelően.

![A Microsoft Azure virtuális gép szolgáltatások elhelyezéséhez][planning-guide-figure-400]

Az Azure virtuális gép Services Microsoft van így lehetővé teszi egyéni kiszolgálói lemezképek központi telepítése az Azure-ba (lásd a 4. ábra) IaaS-példányként. A virtuális gépek Azure lehet a vendég operációs rendszer különböző operációs rendszert futtató, és a Hyper-V virtuális merevlemezek (VHD) alapulnak.

Működési szempontból az Azure virtuális gép szolgáltatás kínál hasonló feladatait, a helyszínen telepített virtuális gépek. Azonban az jelentős előnye, hogy be kell szereznie, felügyelheti és kezelheti az infrastruktúra felesleges rendelkezik. A fejlesztők és rendszergazdák jogköre teljes. az operációs rendszer lemezképének a virtuális gépekről. A rendszergazdák távolról jelentkezhet be ezeket a virtuális gépeket karbantartási és hibaelhárítási feladatok, valamint a szoftverfrissítések telepítési feladatok végrehajtásához. Fürttelepítésben tekintetében a csak korlátozások vonatkoznak, a méretek és az Azure virtuális gépek képességeit. Ezek nem lehet a finom konfigurációban részletes, mivel ezt megteheti a helyszínen. A Virtuálisgép-típusokon kombinációját jelentik választási lehetőség van:

* Vcpu, száma
* Memória
* Csatolt, VHD-k száma
* Hálózati és tárolási sávszélesség.

A méret és a különböző virtuális gépek különböző méretű korlátozások érhető el a táblázatban látható [Ez a cikk][virtual-machines-sizes]

Mivel különböző családok vagy a virtuális gépek sorozatát fog vegye figyelembe. Től Dec 2015 különböztetheti meg egymástól a következő virtuális gépek családok:

* VM a0-A7 csomag típusok: csak az SAP hitelesít. Első Virtuálisgép-sorozat Azure IaaS bevezetett kapott.
* VM a8-A11 csomag típusok: nagy teljesítményű számítástechnikai példányok. Különböző jobb végrehajtása futtatásának gazdagépek, mint más A-sorozatú virtuális gépek számítási.
* D sorozatú Virtuálisgép-típusokon: A0-A7-nál nagyobb végrehajtása. A virtuális gép közül nem mindegyik az SAP hitelesít.
* DS sorozatnak Virtuálisgép-típusokon: D sorozatú ugyanazon állomások használják, de tudnak csatlakozni a prémium szintű Azure Storage (című [prémium szintű Azure Storage] [ planning-guide-3.3.2] a jelen dokumentum). Újra nem minden virtuális gép esetében az SAP hitelesít.
* G-sorozat Virtuálisgép-típusokon: felső memóriaterület Virtuálisgép-típusokon.
* GS sorozatnak Virtuálisgép-típusokon: G-sorozat hasonló, de többek között a prémium szintű Azure Storage lehetőséget (című [prémium szintű Azure Storage] [ planning-guide-3.3.2] a jelen dokumentum). Adatbázis-kiszolgálók GS sorozatnak virtuális gépek használata esetén kötelező a prémium szintű Storage DB adatok és a tranzakciós naplófájlok

Ugyanaz a CPU és memória-konfigurációk másik Virtuálisgép-sorozat tapasztalhatja. Mindazonáltal a másik adatsorozathoz kívüli virtuális gépeken átviteli teljesítményt keressük előfordulhat, hogy térnek el egymástól jelentősen. Annak ellenére, hogy ugyanazt a CPU és memória konfigurációt. Oka az, hogy a mögöttes állomás kiszolgáló hardver, a virtuális gép különböző bevezetése volt-e a különböző átviteli jellemzőit.  Általában a különbség a látható teljesítménye szempontjából is megjelenik a különböző virtuális gépek árát.

Vegye figyelembe, hogy lehet-e minden más Virtuálisgép-sorozat érhető el (az Azure-régiókat lásd a következő fejezet) Azure-régiókat mindegyiknél. Ügyeljen arra is, hogy nem minden virtuális gép vagy Virtuálisgép-sorozat hitelesített SAP.

> [!IMPORTANT]
> SAP NetWeaver alapuló alkalmazások használatára, csak a Virtuálisgép-típusokon és konfigurációk részét felsorolt SAP Megjegyzés [1928533] támogatottak.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-régiók
A Microsoft lehetővé teszi a virtuális gépek telepítéséhez az úgynevezett "Azure-régiók". Egy Azure-régiót lehet egy vagy több olyan adatközpontokban közel található. A legtöbb a a világ geopolitikai régiók Microsoft van legalább két Azure-régió. Például az Európai "Észak-Európa" és "Nyugat-Európában" egy az Azure-régióban van. Ilyen két Azure-régiókat geopolitikai régión belül vannak elválasztva elég jelentős távolság, így természetes vagy technikai katasztrófák nem befolyásolják a két Azure-régió geopolitikai ugyanabban a régióban. Microsoft folyamatosan globálisan fejleszt kimenő geopolitikai különböző régiókban új Azure-régiókat, mivel a Dec 2015-től érhető el további régiókban már bejelentette 20 Azure régiók száma, és ezek a területek száma folyamatosan növekvő. Ügyfélként SAP rendszerek telepíthetők az összes e régiók, beleértve a két Azure-régiókat Kínában. Az aktuális dátummal záródó részéből. Azure-régiók kapcsolatos információkat lásd: a webhely: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>A Microsoft Azure virtuális gép koncepció
Microsoft Azure kínál az infrastruktúra szolgáltatási (IaaS) megoldásként hasonló funkciókkal rendelkező virtuális gépek gazdagépen történő helyszíni virtualizálási megoldásaként. Tudunk létrehozhatnak virtuális gépeket az Azure portálon, a PowerShell vagy a parancssori felület, amely is kínál a központi telepítési és felügyeleti képességek.

Az Azure Resource Manager lehetővé teszi, hogy alkalmazásait egy deklaratív sablon használatával helyezze üzembe. Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt. Egy sablon használatával ismételten telepítheti az alkalmazás, minden szakasza során az alkalmazás-életciklus.

ARM-sablonokkal kapcsolatos további információkat talál itt:

* [Központi telepítése és virtuális gépek kezelése az Azure Resource Manager-sablonok és az Azure parancssori felület használatával][virtual-machines-linux-cli-deploy-templates]
* [Virtuális gépeket Azure Resource Manager és a PowerShell használatával][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

Egy másik érdekes szolgáltatás azt a képességet képek létrehozása a virtuális gépekről, amely lehetővé teszi bizonyos tárházak találhatók, amelyből le is tudja gyorsan üzembe helyezhet a követelményeknek megfelelő virtuálisgép-példány előkészítése.

A virtuális gépekről lemezképek létrehozásával kapcsolatos további információért megtalálhatók [Ez a cikk (Windows)] [ virtual-machines-windows-capture-image] vagy a [Ez a cikk (Linux)] [ virtual-machines-linux-capture-image].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Tartalék tartományok
Tartalék tartományok képviselik fizikai egység, a hiba, nagyon szorosan kapcsolódó a fizikai infrastruktúra adatközpontok, és amíg a fizikai panel vagy állvány lehet tekinteni a tartalék tartomány található, a kettő között nincs közvetlen egy az egyhez típusú megfeleltetés.

Több virtuális gép egy SAP rendszer a Microsoft Azure virtuális gép szolgáltatás részeként történő telepítésekor befolyásolhatja az Azure Fabric Controller különböző tartalék tartományok, ezáltal a követelményeinek megfelelő az alkalmazás központi telepítése a A Microsoft Azure SLA-t. A tartalék tartományok egy Azure méretezési egységhez (akár több százszor is a számítási csomópontok vagy a tárolási csomópontokat és a hálózatkezelés gyűjteményét) elosztása vagy a hozzárendelés az adott tartalék tartomány virtuális gépek azonban valami, amelyben nincs közvetlen ellenőrzése. Ahhoz, hogy az Azure fabric controller központi telepítése a virtuális gépek halmaza különböző tartalék tartományok keresztül irányítani, akkor hozzá kell rendelni Azure rendelkezésre állási csoport a virtuális gépek központi telepítéskor. Azure rendelkezésre állási csoportokra vonatkozó további információkért lásd: fejezet [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] ebben a dokumentumban.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Frissítési tartományok
Frissítési tartományt határoz meg egy logikai egységet, amelyek segítenek meghatározni, hogyan fog frissülni az SAP rendszerben, alkotó SAP-példány a több virtuális gép fut, a virtuális gépek. Frissítés esetén a Microsoft Azure végig kell vinnie a frissítési tartományok egyenként frissítését. Virtuális gépek terjesszen a központi telepítéskor keresztül különböző frissítési tartományok megvédheti a SAP rendszer részben a várható állásidő. Ahhoz, hogy a különböző frissítési tartományok eloszlatva SAP a rendszer a virtuális gépek telepítése Azure kényszeríti, be kell egy adott attribútum az egyes virtuális gépek központi telepítéskor. Tartalék tartományok hasonlóan egy Azure méretezési egység van osztva több frissítési tartományt. Ahhoz, hogy az Azure-hálót tartományvezérlő központi telepítése a virtuális gépek halmaza különböző frissítési tartományok keresztül irányítani, akkor hozzá kell rendelni Azure rendelkezésre állási csoport a virtuális gépek központi telepítéskor. Azure rendelkezésre állási csoportokra vonatkozó további információkért lásd: fejezet [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] alatt.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Az Azure rendelkezésre állási csoportok
Azure virtuális gépeken belül egy Azure rendelkezésre állási csoport terjeszti a rendszer által az Azure Fabric Controller különböző tartalék és frissítési tartományok keresztül. Különböző tartalék és tartományok frissítése a telepítési célja, megakadályozhatja, hogy a minden virtuális gép egy SAP rendszer éppen leállítás alatt infrastruktúra karbantartása vagy egy tartalék tartomány belül hiba esetén a. Alapértelmezés szerint virtuális gépek részei egy rendelkezésre állási csoportban. A tulajdonság részvételét a virtuális gépek rendelkezésre állási csoport a központi telepítéskor vagy későbbi egy újrakonfigurálása és a virtuális gépek ismételt telepítése által definiált.

Szeretné megtudni, Azure rendelkezésre állási készletek és a rendelkezésre állási készletek kapcsolódó hiba és a frissítési tartományok módon fogalmát, olvassa el [Ez a cikk][virtual-machines-manage-availability]

Rendelkezésre állási készletet ARM egy json-sablon használatával lásd: Adja meg [a rest-api specifikációk](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) , és keressen az "availability".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Tárolás: A Microsoft Azure Storage és adatlemezek
Microsoft Azure virtuális gépek hasznosítani különböző típusú tárolókat. A virtuális gép Azure-szolgáltatásokra SAP végrehajtása során fontos tárolási két fő típusai közötti különbségek megismeréséhez:

* Nem állandó, ideiglenes tárolási.
* Az állandó tároló.

A nem állandó közvetlenül csatlakozik a futó virtuális gépek és a számítási csomópontok maguk – a helyi Egypéldányos tárolás (ideiglenes tároló) található. A méret az üzemelő példány indításakor kiválasztott virtuális gép méretétől függ. A tárolási típus: "volatile", és ezért a lemez inicializálva van-e a virtuálisgép-példány újraindítását követően. Általában a ideiglenes lemezen a lapozófájl méretét, az operációs rendszer található.

- - -
> ![Windows][Logo_Windows] Windows
>
> Az ideiglenes meghajtón a Windows virtuális gépeken csatlakoztatva van a telepített virtuális gépen D:\ meghajtóként.
>
> ![Linux][Logo_Linux] Linux
>
> A Linux virtuális gépeken /mnt/resource vagy /mnt van csatlakoztatva. További részletek itt:
>
> * [Hogyan lehet adatlemezt csatolni egy Linux virtuális gép][virtual-machines-linux-how-to-attach-disk]
> * <http://blogs.msdn.com/b/mast/Archive/2013/12/07/Understanding-the-Temporary-drive-on-Windows-Azure-Virtual-machines.aspx>
>
>

- - -
A tényleges meghajtó nem felejtő, mert első magán a gazdagép-kiszolgálón. Ha a virtuális gép áthelyezését egy újbóli üzembe helyezése (például mert a gazdagép vagy állítsa le, majd indítsa újra a karbantartás) a meghajtó a tartalom elvész. Ezért, nincs lehetőség ezen a meghajtón a fontos adatok tárolására. Ez a tároló típusa szerinti használt adathordozó-típust nagyon különböző teljesítményt nyújt, amely 2015 júniusában frissítésétől tűnik a másik Virtuálisgép-sorozat eltérnek:

* A5-A7: Erősen korlátozott teljesítményét. Nem ajánlott a semmit túl az oldal fájlja
* A8-A11: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* D sorozatú: Majd ezer IOPS nagyon jó teljesítmény jellemzőit és > 1GB/s átviteli sebesség.
* DS-méretek: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* G-sorozat: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* GS sorozatnak: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.

A fenti utasítások alkalmazzák, a virtuális gép típusú, amelynek az SAP hitelesít. A Virtuálisgép-sorozat kiváló IOPS és átviteli használja ki az egyes adatbázis-kezelő szolgáltatásai által jogosultak. Tekintse meg a [adatbázis-kezelő telepítési útmutatója] [ dbms-guide] további részleteket.

Microsoft Azure Storage megőrzött tárolás és a jellemző szintű védelmet és San-alapú tárolón látható redundanciát biztosít. Azure Storage alapján lemezek a virtuális merevlemez (VHD) található, az Azure Storage szolgáltatásainak. A helyi operációsrendszer-lemez (Windows C:\, Linux / (/ dev/sda1)) az Azure Storage tárolja, és további köteteket vagy lemezeket a virtuális géphez csatlakoztatott első tárolja, túl.

Akkor lehet feltölteni egy meglévő VHD-t a helyszíni vagy hozzon létre üres néhányat a meglévők közül az Azure-ban, és csatolja azokat a központilag telepített virtuális gépekhez. Ezen a virtuális merevlemezek Azure lemezként hivatkozott.

Miután létrehozott vagy a virtuális merevlemez feltöltése az Azure Storage csatlakoztatásához, és csatolja azokat egy meglévő virtuális gépet, és másolja a meglévő (nem) virtuális Merevlemezek lehetőség.

Ezen a virtuális merevlemezek megmaradnak, adatokat és változásokkal azokat a rendszer biztonságos, amikor újraindul, és újra létrehozni a virtuálisgép-példányt. Akkor is, ha törölnek egy példányát, a virtuális merevlemezek biztonságban és is újra kell telepíteni, vagy nem operációsrendszer lemezek esetén csatlakoztathatók más virtuális gépek.

Az Azure Storage a hálózaton belül különböző redundancia szintek konfigurálható:

* Minimális választható szintje "helyi redundanciát", amely felel meg az Azure-régióhoz azonos adatközpontba belüli adatok három replikája (című [Azure-régiókat][planning-guide-3.1]).
* Zóna redundáns tárolóról, ami a három lemezképeket fogja eloszlatva különböző adatok erőforrások az Azure-régión belül.
* Alapértelmezett redundanciájának szintje földrajzi redundancia céljából, ami aszinkron módon replikál egy másik Azure-régióba geopolitikai ugyanabban a régióban lévő adatokat egy másik 3 képek be a tartalmat.

Is láthatja, hogy a táblázat a különböző redundancia beállítások esetében ez a cikk fölött: <https://azure.microsoft.com/pricing/details/storage/>

Azure Storage esetében további információk itt találhatók:

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/Site-Recovery>
* <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-Disk-Encryption-for-Linux-and-Windows-Virtual-machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Az Azure standard szintű tárolót
Az Azure szabványos BLOB storage volt a tároló típusa szerinti elérhető Azure IaaS kiadásakor. Nem voltak / egyetlen virtuális merevlemez IOPS kvóta. Tapasztalt késést nem volt ugyanahhoz az osztályhoz tartozik, például a helyben tárolt csúcskategóriás SAP rendszerek általában telepített SAN/NAS-eszközökön. Ettől függetlenül az Azure standard szintű tárolást bizonyult elegendő-e több száz SAP rendszerek időközben telepítve az Azure-ban.

Az Azure standard szintű tárolót a tényleges tárolt adatokat, a kötet a storage-tranzakció, a kimenő adatátviteli és a redundancia lehetőséget kiválasztva alapján számítjuk fel. Sok virtuális merevlemezek, a maximális 1TB méretű is létrehozható, de mindaddig, amíg azok továbbra is üres használata díjmentes. Ha ezután töltse ki egy virtuális Merevlemezt 100GB, fizetnie kell 100GB tárolására, nem pedig a VHD-t a készült névleges méretét.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Prémium szintű Azure Storage
A 2015. áprilisi Microsoft bevezette a prémium szintű Azure Storage. Prémium szintű Storage a célja, hogy adja meg a kapott a következő bevezetett:

* Jobb késleltetéséről.
* Nagyobb átviteli sebesség.
* A i/o-késés kevesebb variancia.

Erre a célra a sok módosítást, amely a két legfontosabb jelentek meg:

* Az Azure Storage csomópontok SSD-lemezek használata
* Egy új olvassa el a helyi SSD egy Azure számítási csomópont által támogatott gyorsítótár

Standard szintű storage, ha nem változtatta képességek leváló mérete a lemez (vagy VHD), a függő prémium szintű Storage jelenleg tartozik 3 másik lemezt kategóriák jelennek meg a gyakran feltett előtt a cikk végén: <https:// Azure.microsoft.com/pricing/details/Storage/>

Láthatja, hogy a lemez és IOPS-/ VHD-átviteli/VHD függenek a lemez mérete kategória

Prémium szintű Storage esetében költség alapja nincs ilyen virtuális merevlemezek, de ilyen virtuális merevlemez, függetlenül a virtuális merevlemez tárolt adatok mennyisége az mérete kategóriáját tárolt tényleges adatmennyiség.

Virtuális merevlemezek a prémium szintű Storage, amely nem közvetlen leképezése látható mérete kategóriákba hozhat létre. Ez a helyzet, lehet, különösen akkor, ha a standard szintű tárolót a prémium szintű tároló virtuális merevlemezek másolását. Ebben az esetben a következő legnagyobb prémium szintű Storage lemezes beállítás hozzárendelésével történik.

Felhívjuk a figyelmét arra, hogy csak bizonyos Virtuálisgép-sorozat a prémium szintű Azure Storage előnyei. Től Dec 2015-öt ezek azok a DS - és GS-méretek. A DS sorozatnak alapvetően azonos, azzal a kivétellel, hogy DS sorozatnak képes a csatlakoztatási prémium szintű Storage D sorozatú továbbá a tárolt virtuális merevlemezek a virtuális gépek alapján Azure standard szintű tárolást. G-sorozat GS sorozatnak képest lehet ugyanaz.

Ha ellenőrzése a részét a DS sorozatnak virtuális gépek kimenő [Ez a cikk] [ virtual-machines-sizes] akkor is fog vegye figyelembe, hogy vannak-e adatok mennyiségi korlátozásai prémium szintű Storage virtuális merevlemezeken a granularitási a virtuális gép szint. Különböző DS-méretek és GS sorozatnak virtuális gépeken is korlátozásokat kell csatlakoztatott virtuális merevlemezek száma szerint. Ezek a korlátozások vannak dokumentálva, valamint a fenti cikk. De lényegében azt jelenti, hogy 32 x P30 lemezek/virtuális merevlemezek egy DS14 virtuális pl. csatlakoztatása nem kaphat 32 x P30 lemez maximális átviteli sebességet. A maximális átviteli sebesség a virtuális gép szintje a cikkben ismertetett inkább adatátvitelt korlátozza.

Prémium szintű Storage további információk itt találhatók: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure Storage-tárfiókok
Szolgáltatások és az Azure virtuális gépeken való telepítésekor virtuális merevlemezek és a Virtuálisgép-lemezképek központi telepítését az Azure Storage-fiókok nevezett egységekben kell beállítani. Egy Azure-telepítés megtervezésekor kell alaposan gondolja át, az Azure korlátozásait. Az egyik oldalon nincs Tárfiókok korlátozott számú Azure előfizetésenként. Bár minden Azure Storage-fiók nagyszámú VHD-fájlokat képes tárolni, nincs rögzített korlát a Tárfiók teljes iops-értéket. SAP virtuális gépek több száz jelentős IO hívások létrehozása DBMS rendszerekkel való telepítésekor ajánlott magas IOPS DBMS VMs között több Azure Storage-fiókok terjesztését. Ügyelni kell a következő korlátozásokkal előfizetésenként az Azure Storage-fiókok a jelenlegi korlátozását. Tárolót, mert az adatbázis egy SAP rendszer központi telepítésének fontos része, a fogalom részletes hivatkozott már tárgyalt [adatbázis-kezelő telepítési útmutatója][dbms-guide].

Azure Storage-fiókokról további információ található a [Ez a cikk][storage-scalability-targets]. A cikk elolvasása meg fog vegye figyelembe, hogy a korlátozások Azure standard szintű Storage-fiókok és a prémium szintű Storage-fiókok közötti különbségek vannak. Fő különbségek ilyen egy Tárfiókon belül tárolt adatok mennyiségét. Standard szintű tárolót a lemez egy nagyobb, mint a prémium szintű Storage nagyságrenddel. A másik oldalon a standard szintű Tárfiók súlyosan korlátozott iops-érték (lásd a "Teljes kérelmek gyakorisága" oszlop), mivel a prémium szintű Azure Storage-fiókjához nincs ilyen korlátozás van érvényben. A központi telepítések SAP rendszerek, különösen az adatbázis-kezelő kiszolgálók ismertetésekor ismertetjük részletek és eltérések eredmények.

Különböző tárolók céljából történő szervezése és kategorizálásához különböző VHD-k létrehozásához lehetősége van a Tárfiókon belül. Ezek a tárolók általában segítségével különböző virtuális gépek például külön virtuális merevlemezeket. Nincsenek nem teljesítményre gyakorolt hatása, csak egy tárolót vagy több tároló alatt egy Azure Storage-fiók használatával.

Az Azure virtuális merevlemez nevét, amelyet az Azure virtuális merevlemez egyedi nevét adja meg az alábbi elnevezési kapcsolatot követi:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Ahogy azt korábban említettük, a fenti karakterláncot kell az Azure Storage a tárolt virtuális merevlemez egyedi azonosításához.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>A Microsoft Azure hálózatkezelés
A Microsoft Azure ad meg a hálózati infrastruktúra, amely lehetővé teszi, hogy a leképezés minden forgatókönyvek, amelyek azt szeretnénk, SAP szoftverrel megvalósításához. A funkciók a következők:

* A hozzáférést kívülről, közvetlenül a virtuális gépeket, a Windows Terminálszolgáltatások vagy ssh/VNC keresztül
* Hozzáférését szolgáltatásokhoz és az adott alkalmazást a virtuális gépek által használt portok
* Belső kommunikációs és a névfeloldás között egy központilag telepített Azure virtuális gépek csoportján
* Az ügyfél helyszíni hálózat és az Azure-hálózat között létesítmények közötti kapcsolat
* Több Azure-régióban vagy az adatközpontok Azure helyek közötti kapcsolat

További információk itt találhatók: <https://azure.microsoft.com/documentation/services/virtual-network/>

Nincsenek nagy mennyiségű nevének és IP-feloldás konfigurálása az Azure-ban való különböző lehetőségeit. Ebben a dokumentumban csak felhőalapú forgatókönyvek az Azure DNS-sel (ellentétben meghatározása egy saját DNS-szolgáltatás) alapértelmezett támaszkodnak. Egy új Azure DNS szolgáltatást, amely helyett a saját DNS-kiszolgáló beállításához használható is van. További információ található [Ez a cikk] [ virtual-networks-manage-dns-in-vnet] és a [ezen a lapon](https://azure.microsoft.com/services/dns/).

A létesítmények közötti forgatókönyv azt vannak támaszkodva a tényt, hogy a helyszíni AD/OpenLDAP/DNS-megtörtént-e keresztül VPN- vagy titkos kapcsolat az Azure-bA. Az egyes forgatókönyvek szerint itt dokumentált lehetőségektől, szükség lehet a egy telepített Azure AD/OpenLDAP replikát.

Mivel a hálózat és névfeloldás az adatbázis egy SAP rendszer központi telepítésének fontos része, a fogalom tárgyalt részletesen a [adatbázis-kezelő telepítési útmutatója][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure virtuális hálózatok
Egy Azure virtuális hálózat épület meghatározhatja a címtartomány a magánhálózati IP-címek Azure DHCP-funkciók le van foglalva. A létesítmények közötti forgatókönyv a megadott IP-címtartomány továbbra is oszt ki DHCP használatával az Azure-ban. Tartomány névfeloldás azonban helyszíni (feltéve, hogy, hogy a virtuális gépeket egy helyszíni tartomány része) történik, és ezért oldhatja címek különböző Azure-Felhőszolgáltatások túl.

[comment]: <> (Továbbra is szükséges MSSedusch? Az Affinitáscsoportokban TODO eredetileg egy Azure virtuális hálózatra lett kötve. Az, hogy egy virtuális hálózatot az Azure-ban készült korlátozni az Azure méretezési egység, amely az Affinitáscsoport lett rendelve. A végén Ez azt jelentette, hogy a virtuális hálózathoz lett korlátozni az Azure skálázási egységben rendelkezésre álló erőforrások. Ez megváltozott azóta, és most is kiterjesztheti az Azure virtuális hálózatok közötti egynél több Azure méretezési egység. Azonban, amely megköveteli, hogy az Azure virtuális hálózatok legyenek ** nem ** társított Affinitáscsoportok többé a létrehozás időpontjában. Azt már említettük, hogy a javaslatok évente, a következő időpontban leváló akkor ** nem kihasználhatja az Azure-Affinitáscsoportok többé **. További információkért lásd: < https://azure.microsoft.com/blog/regional-virtual-networks/>)

Minden virtuális gép az Azure-ban kell kapcsolódnia kell egy virtuális hálózathoz.

További részletek találhatók [Ez a cikk] [ resource-groups-networking] és a [ezen a lapon](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd TODO található egy cikket, amely tartalmazza a OpenLDAP témakör + ARM;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Alapértelmezés szerint a virtuális gépek telepítése után nem módosíthatja a virtuális hálózati konfigurációját. A TCP/IP-beállítások Azure DHCP-kiszolgálóhoz kell hagyni. Alapértelmezett viselkedés a dinamikus IP-hozzárendelés.
>
>

A MAC-címet a virtuális hálózati kártya után újra méretezés és a Windows vagy Linux vendég operációs rendszer felveszi az új hálózati kártya, és automatikusan használja az IP- és DNS címek ebben az esetben hozzárendelni DHCP pl. módosíthatja.

##### <a name="static-ip-assignment"></a>Statikus IP-hozzárendelés
Rögzített vagy fenntartott IP-címek hozzárendelése az Azure virtuális hálózaton belüli virtuális gépek lehetőség. A virtuális gépeket futtató Azure virtuális hálózatban megnyitja kihasználhatják ezt a funkciót, ha szükséges, vagy bizonyos esetekben szükséges nagyszerű lehetőséget. Az IP-hozzárendelés marad érvényes létezik-e a virtuális gép, független, hogy a virtuális gép fut vagy -leállítás során. Ennek eredményeképpen kell a virtuális gépek (fut, és a leállított virtuális Gépeken) teljes számát figyelembe venni, ha az az IP-címek megadásával a virtuális hálózat. Az IP-cím megmarad, amíg nem törli a virtuális gép és a hálózati adapter, vagy amíg az IP-cím hozzárendelése újra deszerializálni lekérdezi. A részletes információkat lásd: [Ez a cikk][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Több hálózati adapter virtuális gépenként
Egy Azure virtuális gépen a több virtuális hálózati adapterrel (vNIC) adhat meg. Ahol például ügyfélforgalmat keresztül történik egy virtuális hálózati és a háttérkiszolgáló forgalom elkülönítése a lehetővé teszi, hogy több vNICs beállítása a hálózati forgalom megkezdheti a egy második virtuális hálózati keresztül történik. Függő a típust a virtuális gépet a különböző korlátozások is vNICs száma szerint. Ezek a cikkek megtalálhatók pontos részleteit, funkcióit és korlátozások:

* [Több hálózati adapterrel rendelkező virtuális gép létrehozása][virtual-networks-multiple-nics]
* [Sablon használatával több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-template]
* [PowerShell-lel több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-ps]
* [Az Azure parancssori felület használatával több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Hely-hely kapcsolatot
Létesítmények közötti Azure virtuális gépek és a helyszíni átlátható és állandó VPN-kapcsolat csatolva. Várható legyen, a leggyakrabban használt SAP telepítési minta az Azure-ban. A rendszer azt feltételezi, hogy a műveleti eljárások és a folyamatok SAP osztályt az Azure-ban transzparens módon kell működnie. Ez azt jelenti, hogy sikerül nyomtatni, ezek a rendszerek kívül kell, valamint a használja az SAP átviteli felügyeleti rendszer (TMS) szállítási vált az Azure-ban fejlesztési rendszerről a teszt rendszer legyen telepítve a helyi. Pont-pont körül további dokumentációjában található [Ez a cikk][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-alagút eszköz
(A helyszíni adatközpont Azure adatközpontba) helyek kapcsolat létrehozásához szüksége lesz az beszerzése, és konfigurálja a VPN-eszköz, vagy használja az Útválasztás és távelérés szolgáltatás (RRAS) operációs rendszerben megjelent a Windows Server szoftver összetevőjeként 2012.

* [Virtuális hálózat létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal][vpn-gateway-create-site-to-site-rm-powershell]
* [VPN-eszközökről a webhelyek közötti VPN átjáró kapcsolatok][vpn-gateway-about-vpn-devices]
* [VPN Gateway – gyakori kérdések][vpn-gateway-vpn-faq]

![A helyszíni és az Azure közötti pont-pont kapcsolat][planning-guide-figure-600]

A fenti ábrán látható, két Azure-előfizetések használatra fenntartva az IP-cím résztartomány rendelkezik az Azure virtuális hálózatairól. A kapcsolat a helyszíni hálózat az Azure VPN-en keresztül jön létre.

#### <a name="point-to-site-vpn"></a>Pont – hely típusú VPN
Pont – hely típusú VPN minden ügyfélszámítógép csatlakozni az Azure, a saját virtuális Magánhálózatokkal igényel. Az SAP helyzetekben azt láthatjuk pont-hely kapcsolatot nincs gyakorlati. Ezért semmilyen további hivatkozások adandó pont-pont VPN-kapcsolatot.

[comment]: <> (MSSedusch – További információk itt találhatók)
[comment]: <> (MShermannd TODO hivatkozás már nem érvényes; Azonban ennek ellenére nem támogatott ARM - lásd a következő hivatkozásra:)
[comment]: <> (MSSedusch--< http://msdn.microsoft.com/library/azure/dn133798.aspx>.)
[comment]: <> (MShermannd TODO pont webhelyhez rendelkező ARM még nem támogatott)
[comment]: <> (MSSedusch--< https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### <a name="multi-site-vpn"></a>Többhelyes VPN
Az Azure ma is kínál arra, hogy hozzon létre egy Azure-előfizetés többhelyes VPN-kapcsolatot. Egy-egy előfizetéshez korábban csak egy pont-pont VPN-kapcsolatot. Ez a korlátozás tűnnek el egyetlen előfizetéssel többhelyes VPN-kapcsolatokkal. Ez lehetővé teszi egy adott előfizetéseket a létesítmények közötti konfigurációk egynél több Azure-régióban ki.

További dokumentációjában talál [Ez a cikk][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO ARM dok csatolás nem található)

#### <a name="vnet-to-vnet-connection"></a>Virtuális hálózat virtuális hálózatot kapcsolathoz
Többhelyes VPN-kapcsolattal, kell külön Azure virtuális hálózat konfigurálása a régió. Azonban nagyon gyakran rendelkezik a követelmény, hogy a különböző régiókban szoftverösszetevőket kell kommunikálnak egymással. Ideális esetben ez a kommunikáció nem lesznek irányítva, a helyszíni egy Azure-régióban, illetve onnan az egyéb Azure-régióban. Parancsikonjára Azure kínál arra, hogy konfiguráljon egy Azure virtuális hálózati kapcsolatot egy másik Azure-beli virtuális hálózathoz egy régióban üzemeltetett egy másik régióban. Ez a funkció neve VNet – VNet-kapcsolatot. Ez a funkció a további részletek itt találhatók: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure--expressroute"></a>Az Azure-bA – ExpressRoute magánhálózati kapcsolat
Microsoft Azure ExpressRoute lehetővé teszi, hogy az Azure-adatközpont és az ügyfél helyszíni infrastruktúra között vagy a közös elhelyezés környezetben magánhálózati kapcsolatok létrehozásához. ExpressRoute felajánlott különböző MPLS (csomagkapcsolt) VPN-szolgáltatókkal vagy az egyéb hálózati szolgáltatók. Az ExpressRoute-kapcsolatok nem a nyilvános interneten haladnak át. ExpressRoute kapcsolatok ajánlatot magasabb biztonsági szint, több megbízhatóság több párhuzamos kapcsolatok, a gyorsabb sebesség és a kisebb késések fordulnak elő, mint a szokásos kapcsolatok az interneten keresztül.

További részleteket a Azure ExpressRoute- és ajánlatok itt található:

* <https://Azure.microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-faqs/>

Expressroute több Azure-előfizetések egy ExpressRoute-kapcsolatcsoportot keresztül lehetővé teszi a itt leírtak szerint

* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-linkvnet-ARM/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-Circuit-ARM/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>A kényszerített bújtatás esetén a létesítmények közötti
A virtuális gépekhez való csatlakozás helyszíni tartományok, webhelyek, pont-hely vagy ExpressRoute győződjön meg arról, hogy az internetes proxybeállításokat a virtuális gépek, valamint a felhasználók első telepített kell. Alapértelmezés szerint a szoftverek futtatásának ezeket a virtuális gépek vagy az internet eléréséhez használt böngésző felhasználók a vállalati proxy nem sikerült, de rögtön Azure az interneten keresztül csatlakozni. De még a proxybeállítást a 100 %-os megoldás át tudja irányítani a forgalmat a vállalati proxy, mivel felelőssége a szoftverek és -szolgáltatások kereséséhez a proxy. Ha a virtuális gépen futó szoftver, amely nem végez, illetve a rendszergazda kezeli a beállításokat, az internetre irányuló forgalom is kell újra detoured közvetlenül az internethez Azure szolgáltatáson keresztül.

Ennek elkerülése érdekében beállíthatja a kényszerített bújtatás a helyszíni és az Azure közötti pont-pont kapcsolatban. A kényszerített bújtatás szolgáltatás részletes leírása itt közzétett <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Kényszerített bújtatás az ExpressRoute engedélyezve van az ügyfelek egy alapértelmezett útvonalat hirdet a ExpressRoute BGP társviszony-létesítési munkameneteket keresztül.

#### <a name="summary-of-azure-networking"></a>Az Azure hálózatkezelés összegzése
Ez a fejezet Azure hálózati számos fontos pontok tartalmazott. Itt a következő fő összefoglalása:

* Az Azure virtuális hálózatok lehetővé teszi, hogy a saját igényeinek megfelelően a hálózat beállítása
* Egy Azure virtuális hálózatot is javítható az IP-címtartományok hozzárendelése a virtuális gépek vagy rögzített IP-címek hozzárendelése a virtuális gépek
* A pont-pont vagy a pont – hely kapcsolat beállításához először hozzon létre egy Azure virtuális hálózatra kell
* Miután telepítette a virtuális gép már nem lehet módosítani a virtuális hálózat a virtuális Géphez rendelt

### <a name="quotas-in-azure-virtual-machine-services"></a>A virtuális gép az Azure-szolgáltatások kvóták
Igazolnia kell a tényt, hogy a tárolási és hálózati infrastruktúra az Azure-infrastruktúra a szolgáltatások különböző futtató virtuális gépek között megosztott tisztában lenni. És az ügyfél saját adatközpontok hasonlóan az infrastruktúrához kapcsolódó erőforrások részének túlzott kiosztása mértékben. A Microsoft Azure Platform használja a lemezek, a CPU, a hálózati és a többi kvóták korlátozni az erőforrás-felhasználás és következetes és determinisztikus teljesítmény megőrzése érdekében.  A virtuális gép különböző (A5, A6 stb.) a száma, Processzor, memória- és hálózati különböző kvótái rendelkezik.

> [!NOTE]
> A virtuális gép típusú SAP által támogatott a CPU és memória-erőforrások előre lefoglalt az állomáscsomópontokon. Ez azt jelenti, hogy a virtuális gép van telepítve, a gazdagép erőforrásainak állnak rendelkezésre a Virtuálisgép-típussá által definiált.
>
>

Tervezési és méretezéssel SAP Azure megoldásokról minden virtuális gép méretét kvótái kell tekinteni.  A virtuális gép kvóták ismertetjük [Itt][virtual-machines-sizes].

A leírt kvóták elméleti maximális értékeket jelölik.  A virtuális merevlemez iops-értéket korlátot kis IOS (8 KB-os) érhető el, de valószínűleg nem lehet elérni a nagy IOs (1Mb).  Az IOPS-korláttal megvalósul a lépésköz legyen egyetlen virtuális merevlemezeket.

A nyers döntési fa eldöntheti, hogy egy SAP rendszer Azure virtuális gép szolgáltatások és platformképességei vagy hogy egy meglévő rendszer kell megadni eltérően a rendszer az Azure telepítéséhez illeszkedik, mint a döntési fa alábbi használhatók:

![Döntse el, hogy lehetővé teszi az Azure-on SAP telepítését döntési fája][planning-guide-figure-700]

**1. lépés**: A legfontosabb adatokat kezdődnie esetében az SAP követelmény egy adott SAP rendszerhez. Az SAP-követelmények kell bontható az adatbázis-kezelő és az SAP alkalmazás részét, még akkor is, ha az SAP rendszer már telepített helyszíni 2 szintű konfiguráció. Meglévő rendszerek a SAP, gyakran kapcsolódik a a hardver meghatározott vagy meglévő SAP-referenciaalapok alapján. Az eredmények itt található: <http://global.sap.com/campaigns/benchmark/index.epx>.
Az újonnan telepített SAP rendszerek kell megtettünk mindent keresztül egy méretezési feladat, amely kell meghatározni, hogy a rendszer a SAP követelményeinek.
Lásd még: Ebben a blogban és az SAP méretezési csatolt dokumentumot Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**2. lépés**: meglévő rendszerek esetén az i/o-kötet és az adatbázis-kezelő kiszolgáló másodpercenkénti i/o-művelet kell értékelni. Újonnan tervezett rendszerek esetén a méretezési a gyakorlatban az új rendszer is biztosítani az i/o-követelményeinek nyers ötleteket DBMS oldalán. Ha nem ismeri, végül olyan szeretne egy a koncepció igazolása.

**3. lépés**: hasonlítsa össze a SAP követelmény az SAP, Azure virtuális gép különböző típusú biztosíthat az adatbázis-kezelő kiszolgáló. A SAP adatait az Azure virtuális gép különböző típusú SAP Megjegyzés ismertetett [1928533]. A fókusz kell az adatbázis-kezelő virtuális gépen először az adatbázisrétegben megadása lehet a réteg egy terjessze ki a legtöbb központi telepítések SAP NetWeaver rendszeren. Ezzel szemben az SAP alkalmazásréteg kiterjeszthető. Ha az SAP egyik támogatott Azure Virtuálisgép-típusokon biztosíthat a szükséges SAP, a munkaterhelés a tervezett SAP-rendszer nem futtatható, az Azure-on. Vagy újra kell telepíteni, a rendszer a helyszíni vagy módosítania kell a munkaterhelés kötetet, a rendszer.

**4. lépés**: dokumentált [Itt][virtual-machines-sizes], Azure akár tárolási Standard vagy prémium szintű Storage érvénybe lépteti az IOPS kvótát egy virtuális merevlemez független. A Virtuálisgép-típussá függ, a virtuális merevlemezeket, amely lehet csatlakoztatni száma függ. Ennek eredményeképpen kiszámításához a maximális iops-érték, amely elérhető az összes, a virtuális gép különböző típusú. Az adatbázis-fájlelrendezés függ, virtuális merevlemezek legyen, a vendég operációs rendszer egy kötet képes paritásos. Azonban ha egy telepített SAP rendszer aktuális IOPS mennyisége meghaladja a legnagyobb virtuális gép típusát és az Azure-e több memória kiegyensúlyozása lehetősége nélkül számított határain, a munkaterhelés, a SAP rendszer befolyásolhatja, súlyosan. Ebben az esetben kattintson az a pont, ahol nem kell telepíteni a rendszer az Azure-on.

**5. lépés**: különösen SAP rendszereket telepítette a helyszíni 2 szintű konfigurációk, az valószínűleg, hogy a rendszer előfordulhat, hogy be kell állítani az Azure-on 3 szintű konfiguráció. Ebben a lépésben kell ellenőrizni, hogy van-e egy összetevő az SAP alkalmazásréteg, amely nem terjeszthető ki, és amely volna nem felelnek meg a CPU és memória-erőforrásokat kínálnak a különböző Azure virtuális Gépen. Ha valóban ilyen összetevőt, a SAP rendszer és a munkaterhelés nem telepíthető az Azure. De ha akkor is kibővíthető a SAP alkalmazás-összetevők a több Azure virtuális gépek, a rendszer az Azure is telepíthető.

**6. lépés**: az adatbázis-kezelő és az SAP alkalmazás réteg összetevőinek az Azure virtuális gépeken is futtatható, ha a konfigurációs kell tekintettel definiálni:

* Az Azure virtuális gépek száma
* Az egyes összetevők Virtuálisgép-típusokon
* Adatbázis-kezelő virtuális gépen elég iops értéket biztosítanak, virtuális merevlemezek száma

## <a name="managing-azure-assets"></a>Az Azure-eszközök kezelése
### <a name="azure-portal"></a>Azure Portal
Az Azure portálon az Azure virtuális gép központi telepítések felügyeletéhez szükséges három felületek egyik. Az alapvető felügyeleti feladatokat, például a lemezképeket, a virtuális gépek telepítése végezhető el az Azure portálon. Emellett a Tárfiókokat, virtuális hálózatok és egyéb Azure összetevők létrehozását is is az Azure portál jól kezelhető feladatokat. Azonban funkciók például a virtuális merevlemezek feltöltését a helyszíni Azure vagy az Azure virtuális merevlemez másolása ehhez szükséges, harmadik féltől származó eszközök vagy a PowerShell vagy a parancssori felületen keresztül felügyeleti feladatokat.

![A Microsoft Azure portál – a virtuális gépek – áttekintés][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Felügyeleti és konfigurációs műveleteket a virtuálisgép-példányt a rendszer az Azure-portálon lehetséges.

Újraindítása és leállítása a virtuális gép mellett is csatolása, válassza le, és hozzon létre virtuálisgép-példány, rögzítheti a lemezkép előkészítése példányát, és a virtuálisgép-példányt méretének beállítása adatlemezek.

Az Azure portál olyan virtuális gépek és sok más Azure-szolgáltatások telepítését és konfigurálását alapvető funkciókat biztosít. Az Azure portál azonban nem az összes rendelkezésre álló funkciók vonatkozik. Az Azure portálon nincs lehetőség hasonló feladatok elvégzéséhez:

* VHD feltöltése az Azure-bA
* Virtuális gépek másolása

[comment]: <> (MShermannd TODO Mi a helyzet automation szolgáltatás SAP virtuális gépekhez?)
[comment]: <> (Több virtuális gépek, operációs rendszer időközben lehetséges MSSedusch telepítése)
[comment]: <> (MSSedusch is nincs lehetőség az Azure portálon az automatizálás vonatkozó központi telepítési típussal. Feladatokat, mint a több virtuális gépek parancsfájlalapú telepítés nincs lehetőség az Azure portálon.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>A Microsoft Azure PowerShell-parancsmagok segítségével kezelése
A Windows PowerShell telepítése az Azure-ban rendszerek nagyobb mennyiségű ügyfél széles körben elfogadott sokoldalú és bővíthető keretrendszer. A PowerShell-parancsmagok egy asztali, a laptop vagy a dedikált felügyeleti állomás a telepítés után a PowerShell-parancsmagok távolról futtatható.

A folyamat ahhoz, hogy az Azure PowerShell-parancsmagok és a konfigurálása az Azure szóló használati ismertetett azok használatát a helyi asztali vagy hordozható [Ez a cikk][powershell-install-configure].

További részletes lépéseit telepítése, frissítése és konfigurálása az Azure PowerShell parancsmagok is megtalálhatók [a telepítési útmutató ezen fejezete][deployment-guide-4.1].

Felhasználói élmény eddig lett, hogy PowerShell (Elképzelhető), hogy biztosan a virtuális gépek telepítéséhez, és hozzon létre egyéni lépéseket a virtuális gépek központi hatékonyabb eszköz. SAP példányok Azure-ban futó ügyfelek használnak a PS-parancsmagok kiegészíti a felügyeleti feladatokat az Azure portálon teheti meg, vagy akár az PS parancsmagok kizárólag kezeli a telepítések az Azure-ban. Mivel az Azure egyes parancsmagokat az azonos elnevezési a Windows 2000-nél több kapcsolódó parancsmagok megosztásához célszerű a Windows rendszergazdák számára, hogy ezek a parancsmagok egyszerű feladat.

Itt a következő példa: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd Teendőlista új CLI parancs tesztelésekor írják le.)
Az Azure Figyelőbővítmény az SAP telepítését (című [Azure figyelésére szolgáló megoldás az SAP] [ planning-guide-9.1] ebben a dokumentumban) csak akkor PowerShell vagy a parancssori felületen keresztül lehetséges. Ezért is kötelező, és konfigurálhatja a PowerShell vagy a parancssori felület üzembe helyezésekor és felügyelete az SAP NetWeaver rendszer az Azure-ban.  

Azure több funkciót biztosít, új PS-parancsmagok is lehet hozzáadni, amelyhez a parancsmagok frissítése. Ezért érdemes ellenőrizni a Azure letöltési hely legalább egyszer a hónap <https://azure.microsoft.com/downloads/> a parancsmagok egy új verziója. Az új verzió csak lesz telepítve a korábbi verzióra.

Az Azure listáját általános kapcsolódó PowerShell-parancsok be a négyzetet: <https://msdn.microsoft.com/library/azure/dn708514.aspx>.

### <a name="management-via-microsoft-azure-cli-commands"></a>A Microsoft Azure parancssori felület parancsait keresztül kezelése
Azure kezelni kívánt Linux felhasználók erőforrások Powershell beállítást nem lehet. A Microsoft Azure CLI alternatívájaként kínál.
Az Azure parancssori felület számos nyílt forráskódú, platformok közötti parancsok végzett munka az Azure platformra. Az Azure parancssori felület több ugyanazokat a funkció az Azure portálon található tartalmazza.

További információ a telepítéshez, konfiguráláshoz és parancssori felület használatával parancsok Azure feladatok elvégzését:

* [Az Azure parancssori felület telepítése][xplat-cli]
* [Központi telepítése és virtuális gépek kezelése az Azure Resource Manager-sablonok és az Azure parancssori felület használatával][virtual-machines-linux-cli-deploy-templates]
* [Használja az Azure parancssori felület Mac, Linux és a Windows az Azure Resource Manager eszközzel][xplat-cli-azure-resource-manager]

Olvassa el is fejezet [Linux virtuális gépek Azure CLI] [ deployment-guide-4.5.2] a a [telepítési útmutató] [ planning-guide] központi telepítése az Azure-figyelés az Azure parancssori felület használatával Az SAP-kiterjesztés.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Az SAP, az Azure virtuális gépek telepítése különböző módjai
Ez a fejezet megtanulhatja a különböző módon telepíthet egy Azure-ban. Ez a fejezet további előkészítést eljárásokat, valamint a virtuális merevlemezek és az Azure virtuális gépek kezelésének ismertetnek.

### <a name="deployment-of-vms-for-sap"></a>Az SAP virtuális gépek telepítése
A Microsoft Azure virtuális gépek és a kapcsolódó lemezek telepítendő több lehetőséget is kínál. Így nagyon fontos különbségek megismeréséhez, mivel a virtuális gépek előkészített központi telepítési módszertől függően eltérőek lehetnek. Most elindítjuk általában egy pillantást az alábbi esetekben:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Áthelyezését a virtuális gépek helyszíni Azure olyan nem általánosított tartalmazó
Kívánja áthelyezni a megadott számú SAP-rendszer a helyszíni Azure-bA. Ezt úgy teheti a virtuális Merevlemezt, amely tartalmazza az operációs rendszer, a SAP bináris fájljait és adatbázis-kezelő bináris fájljait és a virtuális merevlemezek feltöltését a DBMS Azure adatainak és naplókönyvtárainak fájlokkal. Kal ellentétben a [az alábbi #2. forgatókönyv][planning-guide-5.1.2], megakadályozhatja a állomásnév, SAP SID és SAP felhasználói fiókok az Azure virtuális gépen, akkor be lett állítva a helyszíni környezetben. A kép normalizálása ezért nem szükséges. Tekintse meg a fejezetek [áthelyezését a virtuális gépek helyszíni Azure olyan nem általánosított tartalmazó előkészítése] [ planning-guide-5.2.1] a jelen dokumentum előkészítő lépések helyszíni és a feltöltése nem általánosított virtuális gépek vagy virtuális merevlemezek Azure. Olvassa el a fejezet [3. forgatókönyv: egy virtuális gép áthelyezése a helyszíni Azure nem általánosított virtuális Merevlemezt használ a SAP] [ deployment-guide-3.4] a a [Deployment Guide] [ deployment-guide]egy lemezképet, az Azure-ban történő telepítésének részletes leírást.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Virtuális gép és egy ügyfél konkrét lemezkép központi telepítése
Adott javítás szükségletek az operációs rendszer vagy az adatbázis-kezelő verzió a megadott lemezképek az Azure piactéren előfordulhat, hogy nem felelnek meg az igényeinek. Ezért szükség lehet a saját "private" az operációs rendszer vagy adatbázis-kezelő Virtuálisgép-lemezkép többször később is telepíthető, amely virtuális gép létrehozása. Az ilyen "private" lemezkép előkészítése duplikálva lettek-e, a következő elemeket kell tekinteni:


[comment]: <> (MSSedusch > Itt további részletek:)
[comment]: <> (MShermannd TODO első hivatkozás a klasszikus modellt tárgya. Nem található az Azure dokumentum-cikk)
[comment]: <> (MSSedusch >< https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>)
[comment]: <> (MSSedusch >< http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
- - -
> ![Windows][Logo_Windows] Windows
>
> A Windows-beállítások (például a Windows biztonsági AZONOSÍTÓK és állomásnév) kell lennie a helyszíni virtuális Gépre a sysprep parancsot keresztül azért/általánosítva.
>
>
> ![Linux][Logo_Linux] Linux
>
> Kövesse az e cikkekben leírt lépéseket [SUSE] [ virtual-machines-linux-create-upload-vhd-suse] vagy [Red Hat] [ virtual-machines-linux-redhat-create-upload-vhd] előkészítése az Azure-bA feltölteni kívánt virtuális Merevlemezt.
>
>

- - -
Ha már telepítette a helyszíni virtuális gépre (különösen a 2-réteg rendszerek) SAP tartalom, módosíthatja az SAP rendszerbeállítások után az Azure virtuális Gépen keresztül a példány központi telepítését, nevezze át a SAP szoftver kiépítés Manager által támogatott eljárás (SAP Megjegyzés [1619720]). Lásd: fejezetek [előkészítése virtuális gép és egy ügyfél konkrét kép telepítése az SAP] [ planning-guide-5.2.2] és [feltöltése az Azure-bA a helyi virtuális merevlemez] [ planning-guide-5.3.2]a jelen dokumentum előkészítő lépések helyszíni és a feltöltése a általánosított virtuális gépek Azure-bA. Olvassa el a fejezet [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP] [ deployment-guide-3.3] a a [telepítési útmutató] [ deployment-guide] a lépésenkénti útmutató ilyen lemezkép az Azure-ban telepítése.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Az Azure piactéren kívüli virtuális gép telepítése
Szeretné használni a Microsoft, illetve 3. fél megadott Virtuálisgép-lemezkép központi telepítése a virtuális Gépet az Azure piactérről. Miután telepítette a virtuális Gépet az Azure-ban, az azonos irányelvek és eszközök telepítése, a SAP szoftver és/vagy az adatbázis-kezelő a virtuális Gépen belül, mint a helyszíni környezetben hajtsa végre. Részletes leírás a telepítés, ellenőrizze a fejezet [1. forgatókönyv: központi telepítése egy virtuális Gépet az Azure piactéren az SAP kívül] [ deployment-guide-3.2] a a [telepítési útmutató] [deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Az Azure SAP rendelkező virtuális gépek előkészítése
Azure meg kell győződnie, hogy a virtuális gépek feltöltés előtt a virtuális gépek és VHD-k bizonyos követelmények teljesítéséhez. A használt telepítési módszertől függően kisebb különbségek vannak.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Áthelyezését a virtuális gépek helyszíni Azure olyan nem általánosított tartalmazó előkészítése
Gyakori telepítési módja, hogy egy meglévő virtuális Gépre, amely egy SAP rendszer fut a helyszíni Azure. Ezt a virtuális Gépet és az SAP-rendszer a virtuális gép csak fusson az Azure-ban az azonos állomásnévvel és nagyon valószínű SAP SID AZONOSÍTÓVAL. Ebben az esetben a vendég operációs rendszer a virtuális Gépen kell nem általánosítva több-telepítéshez. Ha a helyszíni hálózat lett kiterjesztve, az Azure (című [létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure-e a követelményeknek, teljesen integrálva a helyszíni hálózatra] [ planning-guide-2.2] ebben a dokumentumban), akkor még a azonos tartományi fiókokat is használhat a virtuális Gépen belül azok helyszíni előtt használt.

A saját Azure VM lemez előkészítésekor követelmények a következők:

* Eredetileg az operációs rendszert tartalmazó virtuális merevlemez maximális mérete 127GB csak rendelkezhetnek. Ez a korlátozás 2015. márciusi végén kapott szüntetni. Most a virtuális Merevlemezt, amely tartalmazza az operációs rendszer lehet akár 1TB méretű egyéb Azure Storage tárolt VHD-t is.

[comment]: <> (Ellenőrizze, hogy ha CLI is alakítja statikus kell MShermannd TODO)
* Kell lennie a rögzített méretű VHD formátumban. Dinamikus VHD vagy VHDx formátumú virtuális merevlemezeket még nem támogatottak az Azure-on. Dinamikus virtuális merevlemezek konvertálja statikus virtuális merevlemezek a virtuális Merevlemezt a PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Virtuális merevlemezek, amely a virtuális géphez csatlakoztatott, és legyen csatlakoztatva újra az Azure-ban, valamint egy rögzített méretű VHD formátumú virtuális gép szükséges. Az azonos az operációsrendszer-lemezképet mérethatárát adatlemezek is vonatkozik. Virtuális merevlemezek mérete 1 TB-os lehet. Dinamikus virtuális merevlemezek konvertálja statikus virtuális merevlemezek a virtuális Merevlemezt a PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Adjon hozzá egy másik helyi fiók, amely használható a Microsoft támogatási szolgálatához vagy a szolgáltatások és alkalmazások futtatásához, amíg a rendszer a virtuális gép környezeteként rendelhető és a megfelelő felhasználók rendszergazdai jogosultságokkal is használható.
* A kis-és egy kizárólag felhőalapú környezet-forgatókönyv használatával (című [csak felhőalapú - függőségek a helyszíni ügyfél hálózaton nélkül az Azure virtuális gépek telepítéséhez] [ planning-guide-2.1] a jelen dokumentum) a Ez a telepítési módszer együtt, tartományi fiókokat, hogy nem működik után az Azure lemez a rendszer az Azure-ban. Ez különösen igaz, ha a szolgáltatásokat, mint az adatbázis-kezelő vagy SAP alkalmazások futtatásához használt fióknak. Ezért kell ilyen tartományi fiókok cserélje le a virtuális gép helyi fiókokhoz és a helyszíni tartományi fiókokat a virtuális gép törlése. A helyszíni tartományi felhasználók megőrzi a Virtuálisgép-lemezkép található darabolása nem okoz problémát telepítésekor a virtuális Gépet a létesítmények közötti forgatókönyv fejezetben leírtak [létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure-e a követelményeknek, hogy az teljesen integrálva a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban.
* Ha tartományi fiókokat volt az adatbázis-kezelő bejelentkezést, vagy a felhasználók fut, a rendszer a helyi és virtuális gépek elvárt befejezési csak felhőalapú forgatókönyveket telepíteni, a tartományi felhasználók törölni kell. Győződjön meg arról, hogy a helyi rendszergazda, valamint egy másik virtuális gép helyi felhasználói meg van adva a bejelentkezési/felhasználó az adatbázis-kezelő, a rendszergazdáknak be kell.
* Más helyi fiókok hozzáadása, azok szükség lehet az adott környezet-forgatókönyv szerint.

- - -
> ![Windows][Logo_Windows] Windows
>
> Ebben a forgatókönyvben nem általánosítása (sysprep) a virtuális gép frissítése és az Azure virtuális gép telepítése szükséges.
> Győződjön meg róla, hogy nincs D:\ meghajtót használja Set lemez automount csatlakoztatott lemezek fejezetben leírtak [beállítás automount a csatlakoztatott lemezekkel] [ planning-guide-5.5.3] ebben a dokumentumban.
>
> ![Linux][Logo_Linux] Linux
>
> Ebben a forgatókönyvben nem általánosítása (waagent-deprovision) a virtuális gép szükséges feltöltése és telepítése az Azure virtuális gép.
> Győződjön meg arról, hogy az összes lemez csatlakoztatva van-e keresztül uuid és, hogy nem használja a/mnt és az erőforrások. Győződjön meg arról, hogy a rendszertöltő bejegyzés is tükrözi az uuid-alapú csatlakoztatási az operációsrendszer-lemezképet.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Virtuális gép és egy ügyfél konkrét kép telepítése az SAP-előkészítése
Egy általánosított OS tartalmazó VHD-fájlokat is tárolódnak az Azure Storage-fiókok a tárolók. Vezérlőpultjának a virtuális Merevlemezt a virtuális merevlemez a központi telepítési sablon fájlokban forrásaként, fejezetben leírtak szerint telepíthet egy új virtuális gép lemezképből ilyen virtuális merevlemez [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP] [ deployment-guide-3.3] , a [ Telepítési útmutató][deployment-guide].

Ha a saját Azure Virtuálisgép-lemezkép előkészítése követelményei vannak:

* Eredetileg az operációs rendszert tartalmazó virtuális merevlemez maximális mérete 127GB csak rendelkezhetnek. Ez a korlátozás 2015. márciusi végén kapott szüntetni. Most a virtuális Merevlemezt, amely tartalmazza az operációs rendszer lehet akár 1TB méretű egyéb Azure Storage tárolt VHD-t is.

[comment]: <> (Ellenőrizze, hogy ha CLI is alakítja statikus kell MShermannd TODO)
* Kell lennie a rögzített méretű VHD formátumban. Dinamikus VHD vagy VHDx formátumú virtuális merevlemezeket még nem támogatottak az Azure-on. Dinamikus virtuális merevlemezek konvertálja statikus virtuális merevlemezek a virtuális Merevlemezt a PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Virtuális merevlemezek, amely a virtuális géphez csatlakoztatott, és legyen csatlakoztatva újra az Azure-ban, valamint egy rögzített méretű VHD formátumú virtuális gép szükséges. Az azonos az operációsrendszer-lemezképet mérethatárát adatlemezek is vonatkozik. Virtuális merevlemezek mérete 1 TB-os lehet. Dinamikus virtuális merevlemezek konvertálja statikus virtuális merevlemezek a virtuális Merevlemezt a PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Mivel a tartományi felhasználók regisztrált felhasználók a virtuális gép nem szerepel egy kizárólag felhőalapú forgatókönyv (című [csak felhőalapú - függőségek a helyszíni ügyfél hálózaton nélkül az Azure virtuális gépek telepítéséhez] [ planning-guide-2.1] a jelen dokumentum), a szolgáltatások ilyen fiókok nem működik, ha a lemezkép központi telepítésekor az Azure-tartomány segítségével. Ez különösen igaz szolgáltatások például az adatbázis-kezelő vagy SAP alkalmazások futtatásához használt fiók. Ezért kell ilyen tartományi fiókok cserélje le a virtuális gép helyi fiókokhoz és a helyszíni tartományi fiókokat a virtuális gép törlése. Megőrzi a helyszíni tartományi felhasználók található a Virtuálisgép-lemezkép nem lehet probléma telepítésekor a virtuális Gépet a létesítmények közötti forgatókönyv fejezetben leírtak [létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure-e a követelményeknek, hogy az teljesen integrálva a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban.
* Adjon hozzá egy másik helyi fiók rendszergazdai jogosultságokkal, amely probléma vizsgálatok vagy a szolgáltatások és alkalmazások futtatásához, amíg a rendszer a virtuális gép környezeteként is hozzárendelhető a Microsoft támogatási szolgálatához, és jobban megfelelő felhasználók által használható használható.
* A csak felhőalapú telepítések és ahol tartományi fiókok volt az adatbázis-kezelő bejelentkezések vagy felhasználók futtatásakor használt a rendszer a helyi a tartományi felhasználók törölni kell. Győződjön meg arról, hogy a helyi rendszergazda, valamint egy másik virtuális gép helyi felhasználói meg van adva a rendszergazdáknak az adatbázis-kezelő a bejelentkezési/felhasználójának kell.
* Más helyi fiókok hozzáadása, azok szükség lehet az adott környezet-forgatókönyv szerint.
* Ha a rendszerkép tartalmazza az SAP NetWeaver egy telepítése és az eredeti neve az Azure-telepítés helyén állomásnevének átnevezése valószínű, javasoljuk, hogy a legújabb verzióját a SAP szoftver kiépítés Manager DVD átmásolja a sablont. Ez lehetővé teszi a könnyen az SAP megadott Átnevezés funkció segítségével igazítja a módosított állomásnév és/vagy módosítsa a SAP rendszer belül a telepített Virtuálisgép-lemezkép biztonsági azonosítója, amint egy új példányt elindult.

- - -
> ![Windows][Logo_Windows] Windows
>
> Győződjön meg róla, hogy nincs D:\ meghajtót használja Set lemez automount csatlakoztatott lemezek fejezetben leírtak [beállítás automount a csatlakoztatott lemezekkel] [ planning-guide-5.5.3] ebben a dokumentumban.
>
> ![Linux][Logo_Linux] Linux
>
> Győződjön meg arról, hogy az összes lemez csatlakoztatva van-e keresztül uuid és, hogy nem használja a/mnt és az erőforrások. A az operációs rendszer lemez győződjön meg arról, hogy az a rendszertöltő bejegyzés is tükrözi az uuid-alapú csatlakoztatási.
>
>

- - -
* SAP grafikus felhasználói felület (a felügyeleti és beállítására céljából) a sablon előre telepítve.
* Egyéb szoftverek szükségesek, hogy a virtuális gépek futnak a létesítmények közötti forgatókönyv telepíthető, amíg ez a szoftver együttműködik a nevezze át a virtuális gép.

Ha a virtuális gép megfelelően előkészített általános, és végül független a fiókok/felhasználók számára nem érhető el a célként megadott Azure környezetben, az utolsó előkészítési lépés ilyen lemezkép normalizálása végzik.

##### <a name="generalizing-a-vm"></a>A virtuális gépek normalizálása
- - -
[comment]: <> (Jobb cikkekhez található rendelkezik MShermannd TODO / d kapcsolatos normalizálása a virtuális gépeket az ARM)
> ![Windows][Logo_Windows] Windows
>
> Az utolsó lépése, hogy jelentkezzen be rendszergazdaként egy virtuális Gépet. Nyisson meg egy Windows parancsablakot "rendszergazdaként. Ugrás a ...\windows\system32\sysprep, majd hajtsa végre a sysprep.exe.
> Egy kis ablakban jelenik meg. Fontos, jelölje be a "Generalize" beállítást (az alapértelmezett érték a nem ellenőrzött), és módosítsa a leállítási lehetőséget az "Újraindítás" alapértelmezett "Leállítás". Ez az eljárás azt feltételezi, hogy a sysprep folyamat végrehajtott helyszíni a virtuális gépek vendég operációs rendszerben.
> Ha azt szeretné, hogy hajtsa végre az Azure-ban már futó virtuális gépen, az ismertetett lépéseket követve [Ez a cikk][virtual-machines-windows-capture-image].
>
> ![Linux][Logo_Linux] Linux
>
> [A Resource Manager sablonként használni egy Linux virtuális gép rögzítése][virtual-machines-linux-capture-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Virtuális gépek és virtuális merevlemezek átvitele az Azure-bA helyszíni között
Mivel a Virtuálisgép-lemezképet és lemezt feltöltése az Azure-ba nem lehetséges az Azure portálon, kell használnia az Azure PowerShell-parancsmagjaival vagy a parancssori felület. Egy másik lehetőség, az eszköz "AzCopy" használatát. Az eszköz másolható VHD-k a helyszíni és az Azure közötti (kétirányú). Azt is másolhatja Azure-régiók közötti virtuális merevlemezeket. További részleteket [ebben a dokumentációban] [ storage-use-azcopy] a letöltésről és az AzCopy használata.

Harmadik lehetőségként lenne harmadik féltől származó készült grafikus felhasználói felület különböző eszközöket. Azonban győződjön meg arról, hogy ezek az eszközök támogatják a Lapblobokat Azure. A célra a Azure oldalakra vonatkozó Blob tároló használatára kell (a különbségek a dokumentum ismerteti: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). Az Azure által biztosított eszközök is nagyon hatékonyak a tömörítés a virtuális gépek és VHD, amelyek fel kell tölteni. Ez azért fontos, mert ez a hatékonyság a tömörítés csökkenti a kísérlet ideje (ami függ ennek ellenére a feltöltési hivatkozás az internetre a helyszíni létesítményt a személyes és a megcélzott Azure-telepítés régió). A valós azt feltételezi, hogy a virtuális gép vagy a VHD-Európai helyről az Egyesült Államok feltöltése alapú adatközpontok, mint az azonos virtuális gépek vagy virtuális merevlemezek feltöltését a Európai Azure adatközpontok hosszabb ideig tart az Azure data.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Az Azure-bA a helyi virtuális merevlemez feltöltése
A helyi hálózatról egy meglévő virtuális vagy virtuális merevlemez feltöltéséhez ilyen virtuális Merevlemezt vagy virtuális gép kell megfeleljenek a fejezetben felsorolt [áthelyezését a virtuális gépek helyszíni Azure olyan nem általánosított tartalmazó előkészítése] [ planning-guide-5.2.1]ebben a dokumentumban.

Egy virtuális Gépet nem kell általánosítva, és az állapot és rendelkezik a helyszíni oldalon leállást követően alakzat fel kell tölteni. Ugyanez érvényes további VHD, amelyek nem tartalmaznak minden operációs rendszer.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Virtuális merevlemez feltöltése, majd elérhetővé tétele az Azure-lemez
Ebben az esetben szeretnénk töltse fel a virtuális Merevlemezt, vagy anélkül, hogy az operációs és egy virtuális géphez, amely adatok lemezként csatlakoztatni vagy használni az operációsrendszer-lemez. Ez az folyamat

**PowerShell**

* Előfizetés-bejelentkezési *Login-AzureRmAccount*
* Az előfizetés és a környezet beállításához *Set-AzureRmContext* és előfizetés-azonosító paramétert vagy SubscriptionName - <https://msdn.microsoft.com/library/mt619263.aspx>
* A virtuális merevlemez feltöltése *Add-AzureRmVhd* egy Azure Storage-fiók – lásd: <https://msdn.microsoft.com/library/mt603554.aspx>
* Az operációsrendszer-lemezképet, az új Virtuálisgép-konfiguráció beállítása a virtuális Merevlemezt *Set-AzureRmVMOSDisk* -lásd <https://msdn.microsoft.com/library/mt603746.aspx>
* Hozzon létre egy új virtuális Gépet a a Virtuálisgép-konfiguráció a *New-AzureRmVM* -lásd <https://msdn.microsoft.com/library/mt603754.aspx>
* Adatlemez hozzáadása új virtuális gép és *Add-AzureRmVMDataDisk* -lásd <https://msdn.microsoft.com/library/mt603673.aspx>

**Azure CLI**

* Váltson Azure Resource Manager módra a *azure config mód arm*
* Előfizetés-bejelentkezési *azure bejelentkezési*
* Válassza ki az előfizetés *azure-fiók beállítása`<subscription name or id`>*
* A virtuális merevlemez feltöltése *az azure storage-blob feltöltése* -lásd [az Azure Storage az Azure parancssori felület használatával][storage-azure-cli]
* Hozzon létre egy új virtuális Gépet a feltöltött virtuális merevlemez megadása az operációs rendszer lemezeként *azure virtuális gép létrehozása* és paraméter -d
* Adatlemez hozzáadása új virtuális gép és *méretű lemez csatolása és új*

**Sablon**

* A Powershell vagy Azure CLI feltöltéséhez
* Telepítse a virtuális Gépet egy JSON-sablon hivatkozik a VHD-t, ahogy az a [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>A Virtuálisgép-lemezkép központi telepítése
Egy meglévő virtuális vagy virtuális merevlemez feltöltése a helyi hálózatról történő használatához azt egy Azure Virtuálisgép-lemezkép egy virtuális gép vagy VHD-t kell fejezetben felsorolt követelményeknek [előkészítése virtuális gép és egy ügyfél konkrét kép telepítése az SAP] [ planning-guide-5.2.2] ebben a dokumentumban.

* Használja *sysprep* Windows vagy *waagent-deprovision* tekintse meg a VM - általánosítja Linux [rögzítése a Resource Manager üzembe helyezési modellel Windows virtuális gépek] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] vagy [erőforrás-kezelő sablonként használni egy Linux virtuális gép rögzítése][virtual-machines-linux-capture-image-capture]
* Előfizetés-bejelentkezési *Login-AzureRmAccount*
* Az előfizetés és a környezet beállításához *Set-AzureRmContext* és előfizetés-azonosító paramétert vagy SubscriptionName - <https://msdn.microsoft.com/library/mt619263.aspx>
* A virtuális merevlemez feltöltése *Add-AzureRmVhd* egy Azure Storage-fiók – lásd: <https://msdn.microsoft.com/library/mt603554.aspx>
* Az operációsrendszer-lemezképet, az új Virtuálisgép-konfiguráció beállítása a virtuális Merevlemezt *Set-AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -lásd <https://msdn.microsoft.com/library/mt603746.aspx>
* Hozzon létre egy új virtuális Gépet a a Virtuálisgép-konfiguráció a *New-AzureRmVM* -lásd <https://msdn.microsoft.com/library/mt603754.aspx>

**Azure CLI**

* Használja *sysprep* Windows vagy *waagent-deprovision* tekintse meg a VM - általánosítja Linux [rögzítése a Resource Manager üzembe helyezési modellel Windows virtuális gépek] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] vagy [rögzítése a Resource Manager sablonként használni egy Linux virtuális gép] [ virtual-machines-linux-capture-image-capture] Linux
* Váltson Azure Resource Manager módra a *azure config mód arm*
* Előfizetés-bejelentkezési *azure bejelentkezési*
* Válassza ki az előfizetés *azure-fiók beállítása`<subscription name or id`>*
* A virtuális merevlemez feltöltése *az azure storage-blob feltöltése* -lásd [az Azure Storage az Azure parancssori felület használatával][storage-azure-cli]
* Hozzon létre egy új virtuális Gépet a feltöltött virtuális merevlemez megadása az operációs rendszer lemezeként *azure virtuális gép létrehozása* és paraméter -K

**Sablon**

* Használja *sysprep* Windows vagy *waagent-deprovision* tekintse meg a VM - általánosítja Linux [rögzítése a Resource Manager üzembe helyezési modellel Windows virtuális gépek] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] vagy [rögzítése a Resource Manager sablonként használni egy Linux virtuális gép] [ virtual-machines-linux-capture-image-capture] Linux
* A Powershell vagy Azure CLI feltöltéséhez
* Telepítse a virtuális Gépet egy JSON-sablon hivatkozik a kép VHD, ahogy az a [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-to-on-premises"></a>A helyszíni virtuális merevlemezek letöltése
Azure-infrastruktúra szolgáltatásként nincs csak a virtuális merevlemezek és az SAP tölthet fel éppen egy egyirányú utcaneve rendszerek. SAP áthelyezheti az Azure-ból rendszerek vissza az helyszíni világon is.

A letöltés ideje alatt a virtuális merevlemezeket nem lehet aktív. Akkor is, ha a letöltés VHD, amelyek csatlakoztatva vannak a virtuális gépekhez, a virtuális Gépet újra kell indítani. Ha szeretné letölteni a tartalmat, amely majd használandó új rendszer beállítása a helyszíni és elfogadható, ha a letöltési ideje és az új rendszer, hogy a rendszer az Azure-ban a telepítés során, működésbe adatbázis , nem sikerült egy hosszú állásidő elkerülheti, ha a tömörített adatbázis biztonsági mentést egy VHD-be, és csak töltse le az adott VHD-t is az alap operációs rendszer virtuális gép letöltésük helyett.

#### <a name="powershell"></a>PowerShell
Miután az SAP rendszer le van állítva, és a virtuális gép leáll, segítségével a PowerShell-parancsmag mentés-AzureRmVhd a helyszíni célszámítógépen töltse le a VHD lemezek vissza a helyszíni tétele. Művelet végrehajtásához, hogy telepíteni kell-e a virtuális Merevlemezt, amely az URL-CÍMÉT az Azure portálon (szükség keresse meg a Tárfiók és a tároló, amelyen létrehozták a VHD-t) "tárolási szakaszának", és ismernie kell ahol a virtuális Merevlemezt kell átmásolni.

Majd kihasználhatják a parancs által egyszerűen a paramétert definiáló SourceUri töltse le a VHD-és a LocalFilePath az URL-címet a virtuális merevlemez (beleértve a neve) fizikai helye. A parancs nézhet:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

A Save-AzureRmVhd parancsmag további részletekért ellenőrizze Itt <https://msdn.microsoft.com/library/mt622705.aspx>.

#### <a name="cli"></a>parancssori felület
Miután az SAP rendszer le van állítva, és a virtuális gép leáll, segítségével az Azure CLI parancs az azure storage blob letöltési a helyszíni célszámítógépen töltse le a VHD lemezek vissza a helyszíni tétele. A "tárolási szakaszának" az Azure portálon (Nyissa meg a Tárfiók és a tároló, ahol a virtuális merevlemez létrehozásának szükség), és ahol a virtuális merevlemez fel kell tudnia kell keresés copi művelet végrehajtásához, hogy kell-e a neve és a VHD-fájl, melyet a tároló a kiadás alatt.

A parancs kihasználhatják majd meghatározásával egyszerűen a paraméterek blob és a tároló a VHD-fájl letöltése és a cél fizikai célhelyként a VHD-fájl (beleértve a nevét). A parancs nézhet:

```
azure storage blob download --blob <name of the VHD to download> --container <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --destination <destination of the VHD to download>
```

### <a name="transferring-vms-and-vhds-within-azure"></a>Átadó virtuális gépek és virtuális merevlemezek Azure-ban
#### <a name="copying-sap-systems-within-azure"></a>Az Azure SAP rendszerek másolása
Egy SAP rendszert vagy akár dedikált adatbázis-kezelő kiszolgáló egy SAP alkalmazásréteg valószínűleg áll, amely több VHD, amelyek tartalmazzák az operációs rendszer, a bináris fájljait, vagy az adatok és a naplófájl (oka) t az SAP-adatbázis. Azure funkcióinak átmásolni a virtuális merevlemezeket, sem virtuális merevlemezek lemezre mentése Azure funkcióinak rendelkezik egy szinkronizálási mechanizmus, amely volna pillanatkép-szinkron módon csak egy virtuális merevlemezt. Ezért a másolt vagy mentett virtuális merevlemezek állapota akkor is, ha azokat, szemben az azonos virtuális gép csatlakoztatott eltérő lenne. Ez azt jelenti, hogy, hogy a különböző adatok és a különböző virtuális merevlemezek szereplő logfile(s) a konkrét esetben a végén adatbázis lenne inkonzisztens.

**Megkötését: Másolja vagy mentse a VHD-k egy SAP rendszerkonfiguráció részét képező van szüksége az SAP rendszer leállítása, és állítsa le a telepített virtuális gép is kell. Csak ezután képes másolja vagy töltse le a virtuális merevlemezek vagy hozzon létre egy példányát a SAP rendszer Azure vagy a helyszíni készlete.**

Adatlemezek egy Azure Storage-fiókot a VHD-fájlokban tárolódnak, és közvetlenül egy virtuális géphez csatolni vagy képként használni. Ebben az esetben a VHD-t a virtuális géphez csatolt beeing előtt egy másik helyre másolja. A teljes nevet, a VHD-fájl az Azure-ban Azure belül egyedinek kell lennie. Amint azt korábban már említettük, az értéke milyen a következőhöz hasonló háromrészes név:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### <a name="powershell"></a>PowerShell
Azure PowerShell-parancsmagok segítségével másolja a virtuális merevlemez, ahogy az [Ez a cikk][storage-powershell-guide-full-copy-vhd].

##### <a name="cli"></a>parancssori felület
Azure CLI segítségével másolja a virtuális merevlemez, ahogy az [Ez a cikk][storage-azure-cli-copy-blobs]

##### <a name="azure-storage-tools"></a>Az Azure Storage-eszközök
* <http://azurestorageexplorer.Codeplex.com/releases/View/125870>

Nincsenek is professional kiadása Azure Tártallózók, amely itt található:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

A virtuális merevlemez magát egy tárfiókon belül példány egy folyamatot, amely (SAN-hardverre pillanatképeinek Lusta és az írás hasonlóan) csupán néhány másodpercet vesz igénybe. Miután egy másolatot a VHD-fájl csatlakoztatása egy virtuális gép vagy másolja a VHD-fájl csatolása a virtuális gépek képként használni.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### <a name="cli"></a>parancssori felület
```
azure config mode arm

# attach a vhd to a vm
azure vm disk attach <resource group name> <vm name> <path to vhd>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Azure Storage-fiókok között a lemezek másolása
Ez a feladat nem hajtható végre, az Azure portálon. Ise Azure PowerShell parancsmagok, az Azure parancssori felület vagy harmadik féltől származó tárolási böngésző is. A PowerShell-parancsmagjaival vagy a parancssori felület parancsait hozhat létre és kezelheti a blobok, amelyek közé tartozik a másolandó aszinkron módon BLOB Storage-fiókok és az Azure-előfizetés belül régiók között.

##### <a name="powershell"></a>PowerShell
Másolás a VHD-k között előfizetések esetében is lehetséges. További információk [Ez a cikk][storage-powershell-guide-full-copy-vhd].

A PS-parancsmag logika alapvető áramló így néz ki:

* A tárfiók környezetét a forrás tárfiók létrehozása *New-AzureStorageContext* -lásd <https://msdn.microsoft.com/library/dn806380.aspx>
* A tárfiók környezetét a cél tárfiók létrehozása *New-AzureStorageContext* -lásd <https://msdn.microsoft.com/library/dn806380.aspx>
* Indítsa el a másolatot

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* A hurok-ben található példány állapotának ellenőrzése

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Az új virtuális merevlemez csatlakoztatása egy virtuális gép fent leírt módon.

További példák: [Ez a cikk][storage-powershell-guide-full-copy-vhd]

##### <a name="cli"></a>parancssori felület
* Indítsa el a másolatot

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Ellenőrizze az állapotát, ha az ismétlődő másolása

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Az új virtuális merevlemez csatlakoztatása egy virtuális gép fent leírt módon.

További példák: [Ez a cikk][storage-azure-cli-copy-blobs]

### <a name="disk-handling"></a>Lemez kezelése
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>VM-/ VHD-szerkezet SAP központi telepítések
Ideális esetben a virtuális gépek és a kapcsolódó virtuális merevlemeze szerkezete kezelésének kell nagyon egyszerű. A helyszíni telepítésekre az ügyfelek szerkezetének kialakítása a kiszolgáló telepítése az sokféleképpen ki.

* Egy alap virtuális Merevlemezt, amely tartalmazza az operációs rendszer és az adatbázis-kezelő és/vagy az SAP bináris fájljait. 2015. márciusi, mert a virtuális merevlemez lehet akár 1TB-nál korábbi korlátozások korlátozott a 127 GB helyett.
* Egy vagy több virtuális merevlemezzel, amely tartalmazza az adatbázis-kezelő naplófájlt az SAP-adatbázisokhoz és az adatbázis-kezelő ideiglenes tárterület a naplófájlt (ha az adatbázis-kezelő támogatja ezt). Ha az adatbázis napló IOPS követelményei magas, szüksége több virtuális merevlemezzel paritásos az IOPS-kötet szükséges elérése érdekében.
* Virtuális merevlemezek tartalmazó egy vagy két adatbázis az SAP-adatbázis és az adatbázis-kezelő ideiglenes adatfájlok is (ha az adatbázis-kezelő támogatja ezt) száma.

![Az Azure infrastruktúra-szolgáltatási virtuális gép SAP referencia-konfiguráció][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO Linux struktúra írják le.)

- - -
> ![Windows][Logo_Windows] Windows
>
> Sok ügyfél az imént látott konfigurációkat, például SAP- és adatbázis-kezelő bináris fájlok nincsenek telepítve a c:\ meghajtóra, amelyre az operációs rendszer telepítve lett. Ennek számos oka volt, de azt vissza a legfelső szintű frissülő, általában, hogy a meghajtók kicsi volt, és az operációs rendszer frissítései szükséges további terület éve 10 – 15. Mindkét feltétel nem vonatkoznak ezek a napok túl gyakran többé. Ma a c:\ meghajtó nagy méretű lemezek vagy a virtuális gépek rendelhetők. Ahhoz, hogy a központi telepítések a struktúrában egyszerű tartani, javasoljuk, hogy kövesse a következő telepítési minta SAP NetWeaver rendszerekhez az Azure-ban
>
> A Windows operációs rendszer lapozófájl méretét a d meghajtón (nem állandó lemez) kell lennie.
>
> ![Linux][Logo_Linux] Linux
>
> Helyezze a Linux swapfile /mnt/mnt és az erőforrások a Linux leírtak [Ez a cikk][virtual-machines-linux-agent-user-guide]. A lapozófájl konfigurálható a Linux-ügynök /etc/waagent.conf konfigurációs fájljában. Adja hozzá, vagy módosítsa a következő beállításokat:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Aktiválja a változtatásokat, újra kell indítania a Linux-ügynök

```
sudo service waagent restart
```

Olvassa el az SAP Megjegyzés [1597355] kapcsolatban további részleteket a javasolt lapozófájl méretét a

- - -
Az adatbázis-kezelő az adatfájlok és a virtuális merevlemezek üzemeltetett Azure tárolási típusát a használt virtuális merevlemezek száma kell határozza meg az IOPS-követelmények és a késés szükséges. Ismerteti a pontos kvóták [Ez a cikk][virtual-machines-sizes]

SAP-telepítések az utolsó 2 évek élménye tanított velünk során néhány tapasztalatokat, amely mint összegezhető:

* Különböző adatfájlok IOPS-forgalom nincs mindig azonos óta a meglévő ügyfél-rendszereken előfordulhat, hogy rendelkezik másképp méretű képviselő az SAP adatbázisának vagy adatbázisainak adatfájljait. Ennek eredményeképpen az kapcsolni lehet jobban RAID konfiguráció over használatával több virtuális merevlemezzel a logikai egységek faragottnak kívül azokat adatok fájlok. Nem voltak olyan helyzetekben, különösen a Azure standard szintű tárolást, ahol az IOPS-ráta elérte az adatbázis-kezelő tranzakciónapló szemben egyetlen virtuális merevlemez a beállított kvótát. Ilyen esetekben a prémium szintű Storage használata ajánlott, vagy másik lehetőségként a Standard tárolási csak egy virtuális merevlemezt egy szoftverek RAID összesítése.

- - -
> ![Windows][Logo_Windows] Windows
>
> * [SQL Server Azure virtuális gépek teljesítménye ajánlott eljárásai][virtual-machines-sql-server-performance-best-practices]
>
> ![Linux][Logo_Linux] Linux
>
> * [Szoftveres RAID Linux konfigurálása][virtual-machines-linux-configure-raid]
> * [A Linux virtuális gép az Azure-ban LVM konfigurálása][virtual-machines-linux-configure-lvm]
> * [Az Azure Storage titkos kulcsok és a Linux i/o-optimalizálás](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
>
>

- - -
* Prémium szintű Storage jelentős jobb teljesítményt, különösen a kritikus tranzakciós napló írása láthatók. Képes biztosítani az éleshez hasonlító teljesítmény várt SAP-forgatókönyvek esetén ajánlott használni, amelyek kihasználhatják a prémium szintű Azure Storage Virtuálisgép-sorozat.

A következőket kell figyelembe venni, hogy a virtuális Merevlemezt, amely tartalmazza az operációs rendszer, és azt javasoljuk, a SAP bináris fájljait és az adatbázis (alap virtuális gép), valamint nem többé legfeljebb 127GB. Most rendelkezhet akár 1TB-nál. Így például SAP kötegelt feladatnaplóit összes szükséges fájl megtartandó elég hely legyen.

További javaslatokat és adatbázis-kezelő virtuális gépeken, kifejezetten a további részletekért tekintse meg a [adatbázis-kezelő telepítési útmutatója][dbms-guide]

#### <a name="disk-handling"></a>Lemez kezelése
A legtöbb esetben szüksége ahhoz, hogy az SAP-adatbázisokhoz üzembe helyezés a virtuális gép további lemezek létrehozásához. A fejezet a VHD-k számára a használata megtartásról [VM-/ VHD-szerkezet SAP központi telepítések] [ planning-guide-5.5.1] ebben a dokumentumban. Az Azure portál lehetővé teszi, hogy a csatolási és leválasztási lemezek, amennyiben egy alap virtuális Gépre van telepítve. A lemezek csak csatlakoztatott vagy leválasztott amikor a virtuális gép működik-e és fut, valamint ha le van állítva. Lemez csatlakoztatása, az Azure portál kínál, üres lemez vagy egy meglévő lemez, amely ezen a ponton a időben nem egy másik virtuális Géphez van csatolva.

**Megjegyzés:**: virtuális merevlemezek csak a csatlakoztatható egy virtuális gép egy adott időpontban.

![Csatlakoztassa / lemezeken Azure standard szintű tárolóval leválasztása][planning-guide-figure-1400]

Döntse el, hogy kívánja-e új és üres virtuális merevlemez (amely a virtuális gép alapjául Tárfiókon létrehozott), illetve hogy szeretné kiválasztani, amely korábban feltöltött, és most kell csatlakoztatni a virtuális Gépen meglévő virtuális merevlemez létrehozásához szüksége.

**FONTOS**: akkor **nem** állomás gyorsítótárazását használja Azure standard szintű tárolást. A gazdagép gyorsítótár beállítások hagyja meg az alapértelmezés szerinti nincs. Prémium szintű Azure Storage kell engedélyeznie, olvasási gyorsítótárazás Ha az i/o-jellemző többnyire olvasható például tipikus i/o-forgalom adatbázis adatok fájlokra vonatkozóan. Adatbázis-tranzakciós naplófájl esetén gyorsítótárazása használata ajánlott.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Hogyan adatlemezzel az Azure-portálon][virtual-machines-windows-attach-disk-portal]
>
> Ha a lemezek vannak csatolva hozzá, kell bejelentkezni a Windows-kezelő megnyitása a virtuális gép. Ha automount nincs engedélyezve a fejezet ajánlott [beállítás automount a csatlakoztatott lemezekkel][planning-guide-5.5.3], az újonnan csatolt kötet online állapotban és inicializálva kell fordítani kell.
>
> ![Linux][Logo_Linux] Linux
>
> Ha a lemezek vannak csatolva hozzá, szeretné-e a virtuális gép bejelentkezni, és inicializálja a lemezeket, a [Ez a cikk][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Ha az új lemez üres lemez, meg kell formázza a lemezt is. Formázásához, különösen az adatbázis-kezelő adatok és a napló az azonos javaslatok az adatbázis-kezelő az operációs rendszer nélküli telepítések esetében érvényesek.

Említettek szerint fejezetben [a Microsoft Azure virtuális gép koncepció][planning-guide-3.2], egy Azure Storage-fiók nem rendelkezik végtelen erőforrásokkal tekintetében i/o-kötet, iops-érték és az adatok kötet. Adatbázis-kezelő virtuális gépek általában leginkább által érintett. A legjobb, ha az egyes virtuális gépek egy külön Tárfiókot használja, ha néhány nagy i/o adatmennyiség ahhoz, hogy az Azure Storage-fiók kötet belül maradnak telepítendő virtuális gépek lehet. Ellenkező esetben szeretne látni, hogyan anélkül, hogy elérte-e minden egyetlen Tárfiók legfeljebb különböző Storage-fiókok közötti virtuális gépeken is el tudnak osztani. További részleteket ismertetik a [adatbázis-kezelő telepítési útmutatója][dbms-guide]. Ezek a korlátozások is tiszta SAP alkalmazás kiszolgáló virtuális gépek vagy más virtuális gépek, amelyek idővel előfordulhat, hogy további virtuális merevlemezek figyelembe kell venni.

Egy másik témakör, amely a Storage-fiókok szükséges, hogy a virtuális merevlemezek tárfiókokban georeplikált kap. A georeplikáció engedélyezve van-e a Tárfiók szintjén, és nem a virtuális gép szintről. A georeplikáció engedélyezve van, ha a virtuális merevlemezeket a Tárfiókon belül a régión belül egy másik Azure adatközpontba történő volna replikálható. Mielőtt eldönti, ez, gondolja a következő korlátozást:

Azure georeplikáció helyileg működik-e az egyes virtuális merevlemez virtuális gépen, és nem replikálja az IOs időrendben között csak egy virtuális merevlemezt a virtuális gép. Ezért a virtuális Merevlemezt, amely jelöli az alap virtuális gép, valamint minden további virtuális merevlemezek a virtuális Géphez csatlakozik, a rendszer replikálja egymástól független. Ez azt jelenti, hogy nincs szinkronizálás között a különböző virtuális merevlemezek változásait. Azt a tényt, hogy az IOs replikálva vannak-e a sorrendet, amelyben azt jelenti, hogy megírásának módjától függetlenül, hogy a georeplikáció nincs a hozzájuk tartozó adatbázisok elosztva a több virtuális merevlemezzel rendelkező adatbázis-kiszolgálók számára. A DBMS mellett is előfordulhat, más alkalmazások, ahol írási figyelmen kívül hagyott folyamatok kezelhetők adatok különböző virtuális merevlemezeken, és ezért fontos változások sorrendjének tartani. Ha ez a követelmény, az Azure-ban a georeplikáció nem engedélyezni kell. E szükséges, vagy szeretné georeplikáció állítja be a virtuális gépek, de nem egy másik készlet függ, már rendezheti az virtuális gépek és azok kapcsolódó virtuális merevlemezek be különböző Storage-fiókok, amelyek rendelkeznek a georeplikáció engedélyezve vagy letiltva.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>A csatlakoztatott lemezek automount beállítása
- - -
> ![Windows][Logo_Windows] Windows
>
> Virtuális gépek, amelyek a saját lemezképek vagy lemezek jönnek létre fontos ellenőrizni, és esetleg a automount paraméter. A paraméter lehetővé teszi a virtuális gép újraindítása vagy újbóli üzembe helyezése az Azure automatikusan újra csatlakoztatni a csatolt/csatlakoztatott meghajtók után.
> A paraméter értéke a lemezképek az Azure piactér a Microsoft biztosítja.
>
> Ahhoz, hogy állítsa be a automount, ellenőrizze a dokumentációt a parancssori végrehajtható diskpart.exe itt:
>
> * [A DiskPart parancssori kapcsolók](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> A Windows parancssori ablakot rendszergazdaként kell megnyitni.
>
> Ha a lemezek vannak csatolva hozzá, kell bejelentkezni a Windows-kezelő megnyitása a virtuális gép. Ha automount fejezetben szereplő ajánlás szerint nem engedélyezett [beállítás automount a csatlakoztatott lemezekkel][planning-guide-5.5.3], az újonnan csatolt kötet > online állapotban és inicializálva kell állítani.
>
> ![Linux][Logo_Linux] Linux
>
> Újonnan csatolt üres lemez inicializálása leírtak szerint kell [Ez a cikk][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Szükség új lemezek hozzáadása a /etc/fstab.
>
>

- - -
### <a name="final-deployment"></a>Végső telepítés
A végső központi telepítés és szövegéről, különösen a központi telepítés SAP kiterjesztett figyelést, jelenítik tekintse meg a [telepítési útmutató][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Azure virtuális gépeken futó SAP rendszerek elérése
Csak felhőalapú forgatókönyvek esetén érdemes SAP grafikus felhasználói felülettel a nyilvános interneten keresztül csatlakozni az SAP rendszereket. Ezekben az esetekben a következő eljárásokat kell alkalmazni.

A dokumentum későbbi szakaszában ismertetjük a más fő forgatókönyv, létesítmények közötti telepítések esetén, amelyek a pont-pont (VPN-alagút) vagy Azure ExpressRoute-kapcsolatot a helyszíni és az Azure rendszerek közötti SAP rendszerek csatlakozik.

### <a name="remote-access-to-sap-systems"></a>Távelérés SAP rendszerekhez
Az Azure Resource Manager nincsenek alapértelmezett végpontok többé például a korábbi klasszikus modellt. Egy Azure ARM VM összes portjai nyitva, amíg:

1. Nincs hálózati biztonsági csoport az alhálózatra vagy a hálózati adapter van definiálva. Azure virtuális gépek hálózati forgalmának úgynevezett "hálózati biztonsági csoportok" via védve legyenek. További információ: [Mi az a hálózati biztonsági csoport (NSG)?][virtual-networks-nsg]
2. Nincsenek Azure Load Balancer a hálózati adapter van definiálva.   

Tekintse meg a klasszikus modellt és ARM architektúra különbségének a [Ez a cikk][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Csak felhőalapú forgatókönyvhöz SAP rendszer és az SAP grafikus felhasználói Felülettel kapcsolat konfigurációja
Tekintse meg az ebben a cikkben, amely részleteket a témakör ismerteti: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Virtuális Gépen belül a tűzfal beállításainak módosítása
A tűzfal konfigurálása a virtuális gépeken, a SAP rendszerhez bejövő adatforgalom engedélyezésére szükség lehet.

- - -
> ![Windows][Logo_Windows] Windows
>
> Egy Azure telepített virtuális Gépen belül a Windows tűzfal alapértelmezés szerint be van kapcsolva. Most engedélyeznie kell az SAP-portot kell megnyitni, ellenkező esetben a SAP grafikus felhasználói felület nem tudnak kapcsolódni.
> Ehhez tegye a következőket:
>
> * Nyissa meg vezérlőpult\rendszer és biztonság\windows tűzfal "Speciális beállítások".
> * Most kattintson a jobb gombbal a bejövő szabályok és a kiválasztott új-szabály.
> * Az a következő varázsló válassza egy új "Port" szabály létrehozásához.
> * A varázsló a következő lépésben hagyja meg az értéket, az TCP, és írja be a megnyitni kívánt portszámot. Mivel az SAP-példány azonosítója 00, azt 3200 vett igénybe. Ha a példány egy másik példány a szám, a korábbi példányok száma alapján meghatározott portot kell megnyitni.
> * A varázsló a következő részén kell az elem "Engedélyezése kapcsolat" a be van jelölve.
> * A varázsló a következő lépésben kell határozza meg, hogy a szabály vonatkozik-e tartományi, privát és nyilvános hálózat. Állítsa be, ha szükséges, az igényeinek megfelelően. Azonban összekötésével SAP grafikus felhasználói Felülettel rendelkező nyilvános hálózaton kívülről kell, hogy a szabály vonatkozik. a nyilvános hálózat.
> * A varázsló utolsó lépése meg kell adni a szabály nevét, és mentse a szabály billentyűkombináció lenyomásával a "Befejezés"
>
> A szabály azonnal érvénybe lép.
>
> ![Port szabályát leíró definíció beolvasása][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> A Linux-lemezképek, az Azure piactéren alapértelmezés szerint nem engedélyezi a iptables tűzfal és a kapcsolatot a SAP rendszerrel működnek. Ha iptables vagy más tűzfal engedélyezve, tekintse meg a iptables vagy a használt tűzfalat, hogy engedélyezze a bejövő TCP-forgalom port 32xx (ahol a xx azt a SAP rendszer rendszer száma) a dokumentációban.
>
>

- - -
#### <a name="security-recommendations"></a>Biztonsági javaslatok
Az SAP grafikus felhasználói felület nem csatlakozik közvetlenül bármelyik az SAP-példányok (port 32xx) fut, de először csatlakozik az SAP üzenet kiszolgáló folyamat (port 36xx) megnyitott porton keresztül. A múltban nagyon ugyanazt a portot lett megadva az üzenet-kiszolgáló által az alkalmazás-példányokban kívánja a belső kommunikáció. Megelőzése érdekében a helyszíni akaratlanul is módosíthatja a belső kommunikációs portok Azure üzenet folytatott kommunikáció az alkalmazás-kiszolgálókat. Ajánlott a SAP üzenet kiszolgáló és az alkalmazás-példányok közötti belső kommunikációs váltson egy másik portszámot a rendelkezik a helyszíni rendszerekre, például egy projekt tesztelési stb fejlesztésére klónja lett klónozása rendszereken. Ezt megteheti az alapértelmezett profil paraméterrel:

> rdisp/msserv_internal
>
>

Ahogy: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm>

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>SAP-példányok csak felhőalapú telepítés fogalmak
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Az SAP NetWeaver bemutató/képzési forgatókönyv egyetlen VM
![Egyetlen virtuális gép SAP bemutató rendszereket futtató azonos nevű virtuális gép, egymástól el vannak különítve az Azure Cloud Services csomag][planning-guide-figure-1700]

Ebben a forgatókönyvben (lásd a fejezet [csak felhőalapú] [ planning-guide-2.1] a jelen dokumentum) azt webkiszolgálókból egy tipikus képzési/bemutató rendszer forgatókönyv, ahol a teljes képzési/bemutató forgatókönyvet magában foglal egy virtuális. Feltételezzük, hogy a központi telepítés Virtuálisgép-lemezkép sablonok keresztül történik. Is feltételezzük, hogy bemutató/oktatási anyag többszöröse is be kell állítani a virtuális gépek azonos nevű virtuális gépek kell.

A feltételezése létrehozott Virtuálisgép-lemezkép fejezet egyes szakaszokban ismertetett módon [előkészítése virtuális gépek Azure-beli SAP] [ planning-guide-5.2] ebben a dokumentumban.

A forgatókönyv végrehajtásához események sorozatát így néz ki:

[comment]: <> (MShermannd TODO meg kell adni az ARM-minta / leírása a json-sablon + problémával kapcsolatos ARM virtuális hálózaton belül egyedi virtuális gép neve)   
##### <a name="powershell"></a>PowerShell
* Hozzon létre egy új resoure csoportot minden képzési/bemutató fekvő

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```

* Új tárfiók létrehozása

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Hozzon létre egy új virtuális hálózat minden képzési/bemutató fekvő ahhoz, hogy az azonos állomásnévvel és IP-címek kihasználtsága. A virtuális hálózat, amely csak az SSH lehetővé teszi, hogy a távoli asztal hozzáférés engedélyezéséhez a 3389-es port és a 22-es portot felé irányuló forgalmat hálózati biztonsági csoport védi.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Hozzon létre egy új nyilvános IP-címet, a virtuális gép az internetről való eléréséhez használható

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Hozzon létre egy új hálózati illesztő a virtuális géphez

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Hozzon létre egy virtuális gépet. A csak felhőalapú alkalmazás esetében minden virtuális gép lesz a néven. A virtuális gépek SAP NetWeaver példánya SAP SID ugyanaz lesz is. Az Azure erőforráscsoporton belül a virtuális gép nevének egyedinek kell lennie, de különböző Azure erőforráscsoportok futtatható virtuális gépek ugyanazzal a névvel. A Windows vagy Linux rendszeren a "Gyökér" alapértelmezett "Rendszergazda" fiók nem érvényesek. Ezért egy másik rendszergazda felhasználónevet kell definiálni és jelszóval. A virtuális gép méretétől is kell definiálni.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES" -Skus "12" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="os"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Lehetősége van további lemezek hozzáadása, és állítsa vissza a szükséges tartalom. Vegye figyelembe, hogy az összes blob nevének (kívánt URL-címeket a bináris objektumok) Azure belül egyedieknek kell lenniük.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### <a name="cli"></a>parancssori felület
Az alábbi példakód Linux rendszeren használható. Windows, vagy használjon PowerShell fent leírt módon, vagy % rgName %$rgName helyett használja, és a környezeti változót a Windows-paranccsal például igazítja *beállítása*.

* Hozzon létre egy új resoure csoportot minden képzési/bemutató fekvő

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
azure group create $rgName "North Europe"
```

* Új tárfiók létrehozása

```
azure storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku-name LRS $rgNameLower
```

* Hozzon létre egy új virtuális hálózat minden képzési/bemutató fekvő ahhoz, hogy az azonos állomásnévvel és IP-címek kihasználtsága. A virtuális hálózat, amely csak az SSH lehetővé teszi, hogy a távoli asztal hozzáférés engedélyezéséhez a 3389-es port és a 22-es portot felé irányuló forgalmat hálózati biztonsági csoport védi.

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Hozzon létre egy új nyilvános IP-címet, a virtuális gép az internetről való eléréséhez használható

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Hozzon létre egy új hálózati illesztő a virtuális géphez

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet
```

* Hozzon létre egy virtuális gépet. A csak felhőalapú alkalmazás esetében minden virtuális gép lesz a néven. A virtuális gépek SAP NetWeaver példánya SAP SID ugyanaz lesz is. Az Azure erőforráscsoporton belül a virtuális gép nevének egyedinek kell lennie, de különböző Azure erőforráscsoportok futtatható virtuális gépek ugyanazzal a névvel. A Windows vagy Linux rendszeren a "Gyökér" alapértelmezett "Rendszergazda" fiók nem érvényesek. Ezért egy másik rendszergazda felhasználónevet kell definiálni és jelszóval. A virtuális gép méretétől is kell definiálni.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
```

* Lehetősége van további lemezek hozzáadása, és állítsa vissza a szükséges tartalom. Vegye figyelembe, hogy az összes blob nevének (kívánt URL-címeket a bináris objektumok) Azure belül egyedieknek kell lenniük.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### <a name="template"></a>Sablon
A minta sablonokat használhat a githubon az azure-gyors üzembe helyezés-sablonok tárházba.

* [Egyszerű Linux virtuális gép](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Egyszerű Windows virtuális gép](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [A kép VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Virtuális gépek, amelyeknek az Azure készletének megvalósítását
Ez csak felhőalapú forgatókönyv, a jellemző forgatókönyv, képzés és bemutató jellegű helyét a szoftver a bemutató/képzési képviselő forgatókönyv nyúlik át több virtuális gép. A különböző virtuális gépek telepítve a különböző összetevőket kell kommunikálnak egymással. Ebben az esetben ebben a forgatókönyvben nem a helyi hálózati kommunikáció, vagy a létesítmények közötti forgatókönyv van szükség.

Ebben a forgatókönyvben a fejezetben ismertetett telepítési kiterjesztése [rendelkező SAP NetWeaver bemutató/képzési forgatókönyv egyetlen virtuális] [ planning-guide-7.1] ebben a dokumentumban. Több virtuális gép ekkor bekerül egy meglévő erőforráscsoportot. Az alábbi példában a képzés fekvő egy SAP ASC/SCS virtuális Gépre, a virtuális gép fut egy adatbázis-kezelő és az SAP Application Server-példány VM áll.

Mielőtt Ez a forgatókönyv kell gondolniuk az alapbeállítások szerint gyakorolt előtt forgatókönyvben.

#### <a name="resource-group-and-virtual-machine-naming"></a>Erőforráscsoport és a virtuális gép elnevezése
Az összes erőforráscsoport-nevek egyedinek kell lennie. A saját elnevezési sémát az erőforrások, például a fejlesztés `<rg-name`>-utótagot.

A virtuális gép neve nem lehet egyedi az erőforráscsoporton belül.

#### <a name="setup-network-for-communication-between-the-different-vms"></a>A különböző virtuális gépek közötti kommunikáció a hálózat beállítása
![Egy Azure virtuális hálózaton belüli virtuális gépek halmaza][planning-guide-figure-1900]

Megelőzése érdekében ütközésének a klónozásával jár, a képzési/bemutató azonos tájak, hozzon létre egy Azure virtuális hálózatot minden fekvő kell. Azure DNS-név feloldása biztosítja, vagy beállíthatja, hogy a saját DNS-kiszolgáló (hogy nem kell további Microsofttól) Azure kívül. Ebben a forgatókönyvben azt a saját DNS konfigurálása nem. Az összes virtuális gép egy Azure virtuális hálózaton belüli kommunikáció segítségével hostnames engedélyezve lesz.

Virtuális hálózatok, és nem csak erőforráscsoportok képzési vagy bemutató tájak külön lehetnek:

* Az SAP fekvő beállított kell a saját AD OpenLDAP, és a tartomány kiszolgálónak kell lennie a tájak mindegyikének részét.  
* Az SAP fekvő beállított kell működnie a fix IP-címek összetevőket tartalmaz.

További információt az Azure virtuális hálózatok és hogyan adhat meg hozzájuk található [Ez a cikk][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP virtuális gépek telepítése a vállalati hálózati kapcsolat hibája (létesítmények közötti)
Egy SAP fekvő futtatja, és szeretné osztani az operációs rendszer nélküli csúcskategóriás DBMS kiszolgálók között a központi telepítés, helyszíni virtualizált környezetekben alkalmazás rétegek és kisebb 2-réteg SAP rendszerek és az Azure infrastruktúra-szolgáltatási konfigurálva. A kiinduló feltételezése, SAP rendszerek egy SAP fekvő belül kell kommunikálnak egymással, és számos egyéb szoftverösszetevők, a vállalaton belül, a telepítési képernyőn független telepített. Is lehetnek a végfelhasználó SAP grafikus felhasználói felületének vagy egyéb felületek összekapcsolása a telepítési képernyőn által bevezetett nincs különbség. Ezek a feltételek csak akkor kell teljesíteni, ha a helyszíni Active Directory/OpenLDAP és DNS-szolgáltatásokat a webhely-a-site/több-site kapcsolatot vagy Azure ExpressRoute például személyes kapcsolatok Azure rendszerekhez terjeszteni kell.

Ahhoz, hogy kérjen további háttér a SAP Azure megvalósítási részleteit, javasoljuk, hogy olvassa el a fejezet [fogalmak a Cloud-Only telepítési SAP-példányok] [ planning-guide-7] ebben a dokumentumban, amelyből megtudhatja, néhány a alapvető szerkezet Azure, és hogyan ezeket kell használni az SAP-alkalmazások az Azure-ban.

### <a name="scenario-of-a-sap-landscape"></a>Egy SAP fekvő forgatókönyvet
A létesítmények közötti forgatókönyv nagyjából jelentheti például az alábbi grafikus:

![A helyszíni és az Azure eszközök közötti pont-pont kapcsolat][planning-guide-figure-2100]

Egy olyan forgatókönyvet ismertet a fent látható helyzetet, amelyben a helyszíni AD/OpenLDAP és az Azure DNS ki van bővítve. A helyszíni oldalon egy bizonyos IP-címtartomány egy Azure-előfizetés van fenntartva. Az IP-címtartomány társítható egy Azure virtuális hálózatot az Azure oldalán.

#### <a name="security-considerations"></a>Biztonsági szempontok
A minimális követelmény például az SSL/TLS biztonságos kommunikációs protokollok használatának böngészőalapú hozzáférés vagy VPN-alapú kapcsolatot az Azure-szolgáltatások rendszer elérésére. A rendszer azt feltételezi, hogy a vállalatok nagyon másként kezeli-e a VPN-kapcsolat a vállalati hálózat és az Azure között. Egyes vállalatok blankly sérülékennyé portjai. Néhány más vállalatokkal, előfordulhat, hogy szeretne nagyon pontos mely portokat kell megnyitni, stb.

A tipikus SAP az alábbi táblázatban a kommunikációs portok vannak felsorolva. Alapvetően elegendő az SAP-átjáró port megnyitásához.

| Szolgáltatás | Port neve | Példa `<nn`> = 01 | Alapértelmezett tartomány (min, max) | Megjegyzés |
| --- | --- | --- | --- | --- |
| A kézbesítő |sapdp`<nn>` lásd: * |3201 |3200 – 3299 |SAP kézbesítő SAP grafikus felhasználói felület a Windows és a Java által használt |
| Üzenet kiszolgáló |sapms`<sid`> tekintse meg ** |3600 |szabad sapms`<anySID`> |SID SAP-rendszer-ID = |
| Átjáró |sapgw`<nn`> lásd: * |3301 |Ingyenes |SAP-átjáróhoz, CPIC és RFC-kommunikációhoz használt |
| SAP-útválasztó |sapdp99 |3299 |Ingyenes |Csak CI (központi példány) szolgáltatásnevek telepítése után elvégezhető a /etc/services tetszőleges értékre. |

*) nn = SAP-példányok száma

*) sid SAP-rendszer-ID =

További információk a különböző SAP termékek szükséges portok vagy szolgáltatásokat SAP termékek szerint itt található <http://scn.sap.com/docs/DOC-17124>.
Ez a dokumentum a dedikált portok megnyitása a VPN-eszköz adott SAP termékeket és forgatókönyvek szükséges tudni kell lennie.

Egyéb biztonsági intézkedések, amikor a virtuális gépek telepítését ilyen esetben lehet létrehozni egy [hálózati biztonsági csoport] [ virtual-networks-nsg] hozzáférési szabályok meghatározásához.

### <a name="dealing-with-different-virtual-machine-series"></a>A virtuális gép másik Adatsorozathoz foglalkozó
Elmúlt 12 hónap során a Microsoft számos további Virtuálisgép-típusokon, amelyek eltérnek vagy Vcpu, memória számú hozzáadott vagy fontosabb hardveren futó. Nem minden virtuális gépek támogatottak SAP (lásd a támogatott Virtuálisgép-típusokon SAP feljegyzés [1928533]). Néhány virtuális gépek futtasson másik gazdagépet hardver generációt. A gazdagép hardver generációja az időtartományt, egy Azure-Bővítőegysége első telepített. Azt jelenti, hogy olyan esetek is felmerülhetnek, ha úgy döntött, hogy más Virtuálisgép-méretek nem futtatható a azonos skálázási egység. Rendelkezésre állási csoport méretezési egységek különböző hardver alapján span lehetősége van korlátozva.  Például Ha azt szeretné, az adatbázis-kezelő futtatni A5-A11 virtuális gépek és az SAP alkalmazásréteg G sorozatú virtuális gépeken, a rendszer egyetlen SAP rendszer vagy belül másik rendelkezésre állási csoportok különböző SAP-rendszerek telepítése volna kényszeríthető.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Nyomtatás a helyi hálózati SAP példányból az Azure-ban
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>A létesítmények közötti forgatókönyv TCP/IP felett nyomtatás
A helyszíni TCP/IP-alapú hálózati nyomtatók egy Azure virtuális gép telepítése megegyezik a teljes a vállalati hálózathoz, feltéve, hogy Ön rendelkezik egy VPN-helyek alagút vagy ExpressRoute-kapcsolat létrejött.

- - -
> ![Windows][Logo_Windows] Windows
>
> Ehhez tegye a következőket:
>
> * A konfiguráció varázsló, amely segítségével egyszerűen állítsa be a nyomtató egy Azure virtuális gép egyes hálózati nyomtatók rendelkeznek. Ha a nyomtató nem varázsló szoftver került a "manual" állítsa be a nyomtató módja hozzon létre egy új TCP/IP nyomtatóportot.
> * Nyissa meg a Vezérlőpult -> eszközök és nyomtatók -> Nyomtató hozzáadása
> * A Hozzáadás gombra a számítógéphez egy TCP/IP-címét vagy állomásnevét használata
> * Az IP-címet, a nyomtató
> * Standard 9100 nyomtatóportot
> * Szükség esetén telepítse a megfelelő illesztőprogramot manuálisan.
>
> ![Linux][Logo_Linux] Linux
>
> * a Windows csak kövesse, például a hálózati nyomtató telepítésére a szokásos eljárás
> * Kövesse a nyilvános Linux útmutatói [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) vagy [Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) nyomtató felvételéhez.
>
>

- - -
![Hálózati nyomtatás][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Gazdagép-alapú nyomtató (megosztott nyomtató) az SMB protokollt a létesítmények közötti forgatókönyv
A nyomtatók állomásalapú nincsenek hálózati-kompatibilis úgy lett kialakítva. De állomásalapú nyomtató a hálózaton található számítógépek között megosztható, mindaddig, amíg a nyomtató csatlakozik-e kapcsolva a számítógép. Csatlakozás a vállalati hálózati helyek vagy ExpressRoute és helyi nyomtató megosztása. Az SMB protokoll helyett DNS NetBIOS név szolgáltatásként használja. Lehet, hogy a DNS-állomás neve eltér a gazdagép NetBIOS-név. A normál esetben, a NetBIOS-állomásnevet és a DNS-állomásnevet azonosak. A DNS-tartomány nem célszerű a NetBIOS-név területen. Ennek megfelelően a DNS-állomásnevet és a DNS-tartomány teljesen minősített DNS-állomásnevet kell nem használható a NetBIOS-névteret.

A nyomtató megosztási azonosít egy egyedi nevet a hálózat:

* Az SMB-állomás (mindig szükséges) állomásneve.
* A megosztás (mindig szükséges) neve.
* Annak a tartománynak a nevét, ha a nyomtató megosztása nincs SAP rendszer ugyanabban a tartományban.
* Emellett a felhasználónév és jelszó lehet kell a nyomtató megosztás eléréséhez.

Útmutató:

- - -
> ![Windows][Logo_Windows] Windows
>
> Helyi nyomtató megosztása.
> Nyissa meg a Windows Explorer és a nyomtató megosztási név írja be az Azure virtuális Géphez.
> A Nyomtatóáttelepítési varázsló végigvezeti a telepítési folyamat.
>
> ![Linux][Logo_Linux] Linux
>
> Néhány példa a hálózati nyomtatók konfigurálása a Linux vagy többek között a fejezet kapcsolatos dokumentáció a Linux nyomtatásra vonatkozó. Működik ugyanúgy egy Azure Linux virtuális gép mindaddig, amíg a virtuális gép VPN része:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba) _Share_or_Windows_Share>
> * RHEL <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB-nyomtató (nyomtató továbbítás)
Az Azure-ban egy távoli munkamenet a helyi nyomtató eszközökhöz való hozzáférést biztosít a felhasználók a távoli asztali szolgáltatások képességét nem érhető el.

- - -
> ![Windows][Logo_Windows] Windows
>
> Nyomtatás a Windows további információkat itt talál: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Az SAP integrálását javítása és a létesítmények közötti (TMS) rendszer Azure rendszerek
Az SAP-változás- és átviteli rendszer (TMS) kell megadni exportálásához és importálásához az átviteli kérelmet a fekvő rendszerek között. Feltételezzük, hogy a fejlesztési példányok az SAP rendszer (fejlesztés) találhatók az Azure-ban, mivel a minőségi megbízhatósági (QA) és a hatékony rendszerek (PRD) helyszíni. Továbbá feltételezzük, hogy van-e a központi átviteli könyvtár.

##### <a name="configuring-the-transport-domain"></a>A szállítási tartomány konfigurálása
Konfigurálja a átviteli tartományt a rendszeren, lásd: az átviteli tartományvezérlőként kijelölt [az átviteli tartományvezérlő beállítása](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). A rendszer felhasználó TMSADM jön létre és a szükséges RFC cél jön létre. Ezek a tranzakció SM59 segítségével RFC kapcsolatok ellenőrizheti. Állomásnév-feloldási engedélyezni kell a átviteli tartományon keresztül.

Útmutató:

* A mi esetünkben döntöttünk, a helyszíni QAS rendszer lesz a CTS tartományvezérlő. Tranzakció STM hívja. A TMS párbeszédpanel jelenik meg. A szállítási tartomány konfigurálása párbeszédpanel. (Ez a párbeszédpanel csak akkor jelenik meg, ha még nincs konfigurálva az átviteli tartomány.)
* Győződjön meg arról, hogy engedélyezve van-e az automatikusan létrehozott felhasználó TMSADM (SM59 -> ABAP kapcsolat -> TMSADM@E61.DOMAIN_E61 -> Részletek -> Utilities(M) engedélyezési teszt ->). A tranzakció a kezdeti képernyő STM jelenítsen meg, hogy az SAP rendszer most már működik a átviteli tartomány tartományvezérlőjeként itt látható módon:

![A tartományvezérlőn STM tranzakció indítási képernyő][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>SAP rendszerek például az átviteli tartományban
Az SAP-rendszerek például átviteli tartományban sorrendjét a következőképpen néz ki:

* A fejlesztői rendszeren az Azure-ban nyissa meg a szállítási rendszer (000 ügyfél), és tranzakció STM hívja. Egyéb konfigurációs lehetőségek közül választhat a párbeszédpanelt, és folytassa a rendszer közé tartozik a tartományban. Adja meg a tartományvezérlő a cél gazdagépre ([SAP rendszerek például az átviteli tartomány](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). A rendszer most vár az átviteli tartomány szerepelni.
* Biztonsági okokból majd akkor lépjen vissza a tartományvezérlő meg a kérést. Válassza ki a rendszer áttekintése és jóváhagyása a várakozási rendszer. Erősítse meg a parancssor és a konfigurációs lesznek kiosztva.

Az SAP rendszer mostantól a szükséges rendszerekről tartalmaz információt minden a más SAP átviteli a tartományban. Egy időben a címet az új SAP rendszer adatküldést a más SAP rendszerekre, és az SAP rendszer is meg kell adni a átviteli vezérlő program átviteli profiljában. Ellenőrzi, hogy RFC-dokumentumokat és a tartomány átviteli könyvtár elérésének működni.

A rendszer a konfiguráció a szokásos módon a dokumentációjában leírt folytassa [változás- és a rendszer](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Útmutató:

* Ellenőrizze, hogy a helyszíni STM megfelelően van konfigurálva.
* Győződjön meg arról, hogy az átviteli tartományvezérlő állomásneve megoldhatók a virtuális gépet, az Azure és fordítva visa.
* Hívás, tranzakciót STM -> egyéb konfigurációs -> rendszer közé tartozik a tartományban.
* Erősítse meg a kapcsolat a helyszíni TMS a rendszerben.
* Átviteli útvonalon, csoportok és rétegek konfigurálása a szokásos módon.

A pont-pont csatlakoztatott létesítmények közötti forgatókönyv a helyszíni és az Azure közötti késés továbbra is lehet jelentős. Ha azt hajtsa végre az objektumoknak az üzemi környezetben fejlesztési és tesztelési rendszerek szállítására sorozatát vagy gondoljon átvitelek vagy támogatási csomag alkalmazása a különböző rendszerek kapcsolatos, akkor vegye figyelembe, hogy a függő központi átviteli könyvtár helye nagy késleltetésű olvasása vagy adatok írása a központi átviteli könyvtárban rendszereivel fog történni. A helyzet hasonlít SAP fekvő konfigurációk ahol a különböző rendszerek keresztül rendelkező jelentős távolságát az adatközpontok különböző adatközpontokban van elosztva.

Ahhoz, hogy az ilyen késés kerülő és a gyors olvasása vagy írása, vagy a transport directory beállíthatja a munkahelyi rendszert két STM átviteli tartományok (egy a helyszíni és egyet a rendszer az Azure-ban és a szállítási tartományok hivatkozás. Ellenőrizze az ebben a dokumentációban, amelyből megtudhatja, a fogalom a SAP TMS mögött ezeket az alapelveket: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/ 38dd924eb711d182bf0000e829fbfe/FRAMESET.htm>.

Útmutató:

* (A helyszíni és az Azure) helyekre átviteli tartomány beállítása STM tranzakció használatával <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* A tartományok hivatkozásra tartomány hivatkozást, és erősítse meg a hivatkozást a két tartomány között.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* A csatolt rendszer a konfiguráció terjesztése.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC forgalom Azure-ban és a helyszíni (létesítmények közötti) található SAP példányai között
RFC forgalom közötti rendszerek, amelyek a helyszíni és az Azure-ban működéséhez szükséges. Egy kapcsolat tranzakció SM59 beállítása a forrásrendszerben, ahol meg kell határoznia az RFC-kapcsolat a célrendszeren felé. A konfigurációs hasonlít az RFC-kapcsolat szokásos beállításait.

Feltételezzük, hogy a létesítmények közötti forgatókönyv esetén a virtuális gépek, amelyek beépített SAP rendszerek futnak, igénylő kommunikálnak egymással megegyező tartományban található. Ezért egy SAP rendszerek közötti RFC-kapcsolat beállítása nem különböznek a beállítási lépéseket és a bemeneti adatokat a helyszíni forgatókönyvekben.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Az Azure-ban, vagy fordítva található SAP példányokból elérése során "local" fileshares
Az Azure-ban található SAP-példányok tartoznak a vállalat helyszíni fájlmegosztások eléréséhez szükséges. Emellett a helyszíni SAP példányok kell hozzáfér a fájlmegosztásokhoz, amelyek az Azure-ban találhatók. A fájlmegosztások, konfigurálnia kell az engedélyeket és a helyi rendszer megosztási beállítások engedélyezéséhez. Győződjön meg arról, hogy a portok a VPN- vagy ExpressRoute kapcsolattal Azure és az Adatközpont között.

## <a name="supportability"></a>Támogatási lehetőségek
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Az Azure felügyeleti megoldás az SAP
Ahhoz, hogy a figyelő kritikus kritikus SAP rendszerek Azure a figyelési eszközök SAPOSCOL vagy az Azure virtuális gép szolgáltatásgazda egy Azure Figyelőbővítmény keresztül adatokat lekérni az SAP SAP Állomásügynök SAP. Óta az SAP igényeket erősen korlátozott számú SAP-alkalmazások, Microsoft lezárását nem általános implementálja a kért funkciót az Azure, de hagyja az ügyfelek számára a szükséges figyelési összetevőkkel és konfigurációk telepítése a virtuális Az Azure-ban futó gépek. Azonban a figyelési összetevők telepítési és életciklus kezelése főként automatizált lesz az Azure-ban.

#### <a name="solution-design"></a>Megoldásterv
A megoldás használatával engedélyezi a SAP fejlesztett Azure Virtuálisgép-ügynök és a bővítmény keretrendszer architektúrájának alapul. Az Azure Virtuálisgép-ügynök és a bővítmény keretrendszer lényege, engedélyezi az Azure Virtuálisgép-bővítmény katalógusában egy virtuális Gépen belül elérhető szoftverek alkalmazás(ok) telepítését. Az elve mögött a fogalom lényege engedélyezi (ilyen esetekben az Azure Figyelőbővítmény az SAP), speciális funkciókat a virtuális gépek telepítése és a szoftver központi telepítéskor.

2014. február, mert az "Azure Virtuálisgép-ügynök", amely lehetővé teszi az adott Azure Virtuálisgép-bővítmények a virtuális Gépen belül kezelésének van be a nézetmodellbe Windows virtuális gépek alapértelmezés szerint a virtuális gép létrehozása az Azure portálon. SUSE vagy a Red Hat Linux virtuális gép ügynök már része az Azure Piactéri lemezképhez. Abban az esetben, ha egy Linux virtuális gép a helyszíni Azure volna feltöltése a Virtuálisgép-ügynök manuális telepítve van.

Az alapvető építőelemeket, amelyekből az SAP keresi a figyelés megoldást az Azure-ban, ez például:

![A Microsoft Azure bővítmény összetevők][planning-guide-figure-2400]

Ahogy a fenti blokk ábrán is látható, az SAP figyelési megoldás egyik részét a Azure Virtuálisgép-lemezkép és Azure bővítmény gyűjteménye, amely Azure műveletei által kezelt globálisan replikált tára van tárolt. A feladata a közös SAP/MS munkacsoport SAP Azure műveletek közzététele az Azure Figyelőbővítmény az SAP új verzióit használható Azure végrehajtásáról működik. Az Azure Figyelőbővítmény az SAP a szükséges információkat a Microsoft Azure Diagnostics (ÜVEGVATTA) kiterjesztés vagy a Linux Azure Diagnostics (LAD) fogja használni.

Amikor telepít egy új Windows virtuális Gépet, az "Azure Virtuálisgép-ügynök" automatikusan szerepel-e a virtuális Gépet. Ez az ügynök függvény, koordinálhatja a be- és konfigurációs az Azure-bővítmények SAP NetWeaver rendszerek figyeléséhez. Linux virtuális gépek az Azure Virtuálisgép-ügynök már része az Azure piactér operációsrendszer-lemezképek.

Van azonban továbbra is szükséges az ügyfél által végrehajtandó minden lépést. Ez az a engedélyezése és a teljesítménynaplózás konfigurációját. A folyamat az konfigurációval kapcsolatos egy PowerShell-parancsfájl vagy a CLI parancs alapján automatikus. A PowerShell parancsfájl letölthető a Microsoft Azure Script Center leírtak szerint a [telepítési útmutató][deployment-guide].

Az Azure felügyeleti megoldás az SAP általános architektúrája néz ki:

![Az Azure felügyeleti megoldás az SAP NetWeaver][planning-guide-figure-2500]

**A pontos útmutató és részletes lépéseit a PowerShell-parancsmagjaival vagy a CLI-parancs segítségével központi telepítések során, kövesse az utasításokat, a megadott a [telepítési útmutató][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integráció az Azure található SAP példányának SAProuter
Azure-beli SAP példányok kell lennie, valamint SAProuter érhető el.

![SAP-útválasztó hálózati kapcsolat][planning-guide-figure-2600]

Egy SAProuter lehetővé teszi, hogy a TCP/IP-kommunikáció, ha nincs közvetlen IP-kapcsolat részt vevő rendszerek között. Ez lehetővé teszi az az előnye, hogy a végpont a kommunikációs partnerek között nincs kapcsolat szükséges hálózati szintű. A SAProuter alapértelmezés szerint 3299 portot figyel.
SAP példányok csatlakozhatnak egy SAProuter keresztül kell a kapcsolódási kísérlet a SAProuter karakterláncot és a gazdagép nevét meg kell adni.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Amennyiben a fókusz a dokumentum SAP NetWeaver általában vagy az SAP NetWeaver ABAP a verem. Ez kis területen figyelembe kell venni bizonyos az SAP Java verem találhatók. A legfontosabb kizárólag alapú SAP NetWeaver Java-alkalmazások egyike az SAP vállalati portálon. Más SAP NetWeaver alapú alkalmazások, például SAP-PI és SAP megoldás kezelő használata az SAP NetWeaver ABAP és a Java-verem. Ezért alapértékekkel szükség van, valamint a SAP NetWeaver Java verem kapcsolatos különleges szempontok figyelembe venni.

### <a name="sap-enterprise-portal"></a>SAP vállalati portál
A telepítő egy SAP portál egy Azure virtuális gép nem eltér a helyi telepítés a központi telepítése a létesítmények közötti forgatókönyv. A DNS-ben a helyszínen történik, mert a port beállítása az egyes példányok szerint konfigurált helyszíni végezhető. A javaslatok és korlátozások, amennyiben a jelen dokumentumban ismertetett érvényes olyan alkalmazások, mint az SAP vállalati portál vagy a SAP NetWeaver Java verem általában.

![Elérhetőségi SAP-portál][planning-guide-figure-2700]

Egy speciális környezet-forgatókönyv szerint egyes ügyfelek a közvetlen módon a SAP vállalati portál kapcsolódik az internethez, míg a virtuális gép gazdagép csatlakozik a vállalati hálózati helyek a VPN-alagúton vagy ExpressRoute keresztül. Ilyen esetben meg kell győződjön meg arról, hogy adott portok nyitva és nem blokkolja tűzfal vagy a hálózati biztonsági csoport. Azonos beállítás alkalmazható, ha azt szeretné, csak felhőalapú esetén helyszíni SAP Java-példány az csatlakozni kell.

A kezdeti portál URI nem HTTP (s):`<Portalserver`>: Ha a port 50000 plusz (Systemnumber x 100) alkotta 5XX00/irj. Az alapértelmezett portál URI az SAP rendszer 00 `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. További részletekért tekintse meg rendelkezik egy <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Végpont-konfiguráció][planning-guide-figure-2800]

Ha szeretné az URL-címet és/vagy a portok az SAP vállalati portál testreszabása, ellenőrizze a jelen dokumentáció:

* [Módosítsa a portál URL-címe](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Alapértelmezett portszámok, Portal portszámok módosítása](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

<a name="7cf991a1-badd-40a9-944e-7baae842a058"></a>
## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Magas rendelkezésre állású (HA) és a katasztrófa utáni helyreállítás (DR) az Azure virtuális gépeken futó SAP NetWeaver
### <a name="definition-of-terminologies"></a>Terminológiát meghatározása
A kifejezés **magas rendelkezésre ÁLLÁS** általában kapcsolódik, amely minimálisra csökkenti az informatikai szolgáltatások keresztül IT szolgáltatások üzleti folytonosság megadásával technológiák redundáns, hibatűrő vagy feladatátvételi védett összetevők belül a **azonos** adatközpont. Ebben az esetben egy Azure-régión belül.

**Vészhelyreállítás (DR)** is célzott IT szolgáltatások megszűnésének minimalizálja a és a helyreállítás azonban keresztben **különböző** adatközpontokban, amelyek általában több száz kilométer távolságra található. A mi esetünkben általában különböző Azure-régiókat geopolitikai régión belül, vagy Ön által létrehozott, az ügyfél között.

### <a name="overview-of-high-availability"></a>A magas rendelkezésre állás – Áttekintés
Azt is külön SAP magas rendelkezésre állás az Azure-ban két részre vonatkozó vitafórum:

* **Azure-infrastruktúra magas rendelkezésre állású**, pl. (VM) számítási, hálózati, tárolási stb és SAP rendelkezésre állásának növelése előnye GAZDAGÉPFÜRTÖK.
* **SAP alkalmazás magas rendelkezésre állású**, például a magas rendelkezésre ÁLLÁSÚ az SAP szoftverösszetevőket:
  * SAP alkalmazáskiszolgálók
  * SAP ASC/SCS példány
  * Adatbázis-kiszolgálót

és hogyan azt kombinálható az Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ.

SAP a magas rendelkezésre állás, az Azure-ban van néhány különbség az SAP magas rendelkezésre állás a helyi fizikai vagy virtuális környezetben képest. Elérhető a következő dokumentum ismerteti a szabványos SAP magas rendelkezésre állású konfiguráció Windows rendszeren virtualizált környezetben: <http://scn.sap.com/docs/DOC-44415>. Nincs sapinst integrált SAP-magas rendelkezésre ÁLLÁSÚ konfiguráció Linux például Windows létezik. Vonatkozó SAP magas rendelkezésre ÁLLÁSÚ Linux helyszíni további információkat itt találja: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure-infrastruktúra magas rendelkezésre állás
Nincs nem single-VM SLA-t Azure virtuális gépeken most. Ha meg szeretne ismerkedni hogyan lehet, hogy meg egy virtuális rendelkezésre állását, például egyszerűen hozhat létre a különböző elérhető Azure SLA-k szorzatát: <https://azure.microsoft.com/support/legal/sla/>.

A számítás alapja havonta vagy 43200 perc 30 nap. Ezért 21,6 perc 0,05 % állásidő felel meg. A szokásos módon a különböző szolgáltatások rendelkezésre állásának szorzási fogja az alábbi módon:

(Rendelkezésre állási szolgáltatás #1/100) * (rendelkezésre állási szolgáltatás #2/100) * (rendelkezésre állási szolgáltatás #3/100) *...

Például:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 vagy egy 99.75 % rendelkezésre állását.

#### <a name="virtual-machine-vm-high-availability"></a>A virtuális gép (VM) magas rendelkezésre állás
Az Azure platform események, amelyek hatással lehetnek a virtuális gépek rendelkezésre állásának két típusa van: a tervezett karbantartást és a nem tervezett karbantartás.

* Tervezett karbantartási események a Microsoft által készített az alapul szolgáló Azure platform átfogó megbízhatóságát, teljesítményét és a platform infrastruktúra, amely a virtuális gépek futnak a biztonsági javítása érdekében rendszeres frissítések érhetők el.
* Nem tervezett karbantartási események fordulhat elő, ha a hardver- vagy a virtuális gép alapul szolgáló fizikai infrastruktúra valamilyen módon hibát jelzett. Ez lehet helyi hálózati hiba, a helyi lemezek meghibásodása, vagy egyéb állványszintű meghibásodások. Ilyen hiba lép fel, ha az Azure platformon automatikusan áttelepíti a virtuális gép a virtuális gépet egy megfelelő fizikai kiszolgálóhoz szolgáltató a nem megfelelő fizikai kiszolgálóról. Ilyen esetek ritkán lépnek fel, de ezek is okozhatják a virtuális gép újraindítását.

További részletek találhatók a jelen dokumentációban: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Az Azure Storage redundancia
Mindig a rendszer replikálja az adatokat a Microsoft Azure Storage-fiók tartósság és magas rendelkezésre állás, az Azure Storage SLA még átmeneti hardverhibák esetén állásuk értekezlet biztosítása

Mivel az Azure Storage alapértelmezés szerint megakadályozza az adatok 3 képek, RAID5 vagy több Azure lemezre RAID1 nem szükségesek.

További részletek megtalálhatók a cikkben: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Azure-infrastruktúra virtuális gép újraindítása "magasabb rendelkezésre állás biztosítása érdekében" SAP-alkalmazások használata
Ha úgy dönt, hogy nem funkcióinak, például a Windows Server feladatátvételi fürtszolgáltatási (WSFC), vagy egy SAP rendszer elleni védelme érdekében be a Linux egyenértékű (az utóbbi egy még nem támogatott az Azure-on SAP szoftver együtt), Azure virtuális gép újraindítása tervezett és nem tervezett leállás a fizikai kiszolgálók Azure-infrastruktúra és a teljes alapul szolgáló Azure platformon.

> [!NOTE]
> Fontos említse meg, hogy Azure virtuális gép újraindítása elsősorban védi a virtuális gépek és alkalmazásokat nem. Indítsa újra a virtuális gép nem biztosít magas rendelkezésre állás a SAP-alkalmazásokból, de az infrastruktúra rendelkezésre állási bizonyos szintű kínálnak, ezért közvetve "magas rendelkezésre állás érdekében" SAP rendszerek. A ideig tart egy gazdagép tervezett vagy nem tervezett leállás után indítsa újra a virtuális gép is van nem az SLA-t. Ez a metódus "a magas rendelkezésre állás" ezért nem alkalmas A SCS vagy az adatbázis-kezelő például SAP rendszer kritikus összetevői.
>
>

A magas rendelkezésre állás fontos infrastruktúra elem az tároló. Például Az Azure Storage SLA 99,9 % rendelkezésre állási. Ha egy telepíti a lemezzel rendelkező összes virtuális gép egy egyetlen Azure Storage-fiók esetén lehetséges Azure Storage elérhetetlensége, akkor kerülnek, Azure Storage-fiókhoz tartozó összes virtuális gépet, is minden SAP összetevőiről és virtuális gépek belül futtatott elérhetetlensége be.  

Ahelyett, hogy minden virtuális gép üzembe egy egyetlen Azure Storage-fiókot, használhatja a kijelölt tárhelycsomópont fiókokat az egyes virtuális gépek, és így általános virtuális gép és az SAP rendelkezésre állásának növeléséhez több független Azure Storage-fiókok használatával.

Egy minta-architektúrát egy SAP NetWeaver rendszer által használt Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ nézhet ki:

![Magas rendelkezésre ÁLLÁSÚ SAP "felső" rendelkezésre állásának eléréséhez Azure-infrastruktúra használatával][planning-guide-figure-2900]

SAP-összetevők kritikus azt érhető el a következő eddig:

* Magas rendelkezésre állású SAP Alkalmazáskiszolgáló (AS)

SAP alkalmazáskiszolgáló-példányok a redundáns összetevők. Minden egyes SAP, példány telepítve van a saját virtuális Gépet, amely futtatja a egy másik Azure hiba és a tartomány frissítése (lásd: fejezetek [tartalék tartományok] [ planning-guide-3.2.1] és [frissítési tartományok] [ planning-guide-3.2.2]). Ez biztosítja Azure rendelkezésre állási csoportokkal (című [Azure rendelkezésre állási készletek][planning-guide-3.2.3]). Egy Azure hiba vagy frissítéséhez tartományi lehetséges tervezett vagy nem tervezett elérhetetlensége fog okozhat a virtuális gépek a korlátozott számú hiányában az SAP-AS példányt.
Minden példány kerül a saját Azure Storage-fiók – lehetséges hiányában egy Azure Storage-fiók hiányában csak egy virtuális gép, akkor az SAP-AS az SAP példány. Azonban vegye figyelembe, hogy nincs maximális száma Azure Storage-fiókok egy Azure-előfizetéssel belül. A virtuális gép újraindítása után () SCS-példány automatikus indítás érdekében feltétlenül állítson be Autostart paraméter (A) SCS példány start profilban fejezetben ismertetett [használatával Autostart SAP-példányok][planning-guide-11.5].
Olvassa el is fejezet [magas rendelkezésre állású SAP alkalmazáskiszolgálók] [ planning-guide-11.4.1] további részleteket.

* *Magasabb* SAP rendelkezésre állását (A) SCS példány

Itt azt hasznosítani Azure VM indítsa újra a virtuális gép védelmét, SAP (A) SCS telepített példánya. Az Azure tervezett vagy nem tervezett leállás esetén kiszolgálója, virtuális gépek újraindul egy másik elérhető kiszolgálón. Amint azt korábban említettük, Azure virtuális gép újraindítása elsősorban védi a virtuális gépek és alkalmazásokat nem, ez esetben a (A) SCS-példányban. A virtuális gép újraindítása keresztül jelenleg lesz érhető el közvetve "magas rendelkezésre állás érdekében" SAP (A) SCS-példány. (A) SCS-példány automatikus indítás biztosításához a virtuális gép újraindítása után, feltétlenül állítson be automatikus indítását paraméter (A) SCS példány start profilban fejezetben ismertetett [használatával Autostart SAP-példányok][planning-guide-11.5]. Ez azt jelenti, hogy a (A) SCS hibaként egyetlen pont, (SPOF) egy virtuális gépen futó példány lesz a meghatározó tényező az egész SAP fekvő rendelkezésre állását.

* *Magasabb* rendelkezésre állási adatbázis-kezelő kiszolgáló

Itt a SAP (A) SCS példány használati eset hasonló, azt használata Azure VM indítsa újra a virtuális Géphez a telepített adatbázis-kezelő szoftver védelmére, és azt "magasabb rendelkezésre állásának eléréséhez" adatbázis-kezelő szoftver használatával indítsa újra a virtuális gép.
Egyetlen virtuális gépen futó DBMS egyben a SPOF, és a meghatározó tényező az egész SAP fekvő rendelkezésre állását.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP alkalmazás magas rendelkezésre állásra Azure IaaS
Teljes SAP rendszer magas rendelkezésre állás biztosítása érdekében, kritikus SAP rendszer összetevők, például redundáns SAP alkalmazáskiszolgálókat, védeni kell, és egyedi összetevőket (például egyetlen pont, hiba), például SAP (A) SCS-példány és az adatbázis-kezelő.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Az SAP alkalmazáskiszolgálók magas rendelkezésre állás
Az SAP-alkalmazás kiszolgálók/párbeszédpanel példányok nincs szükség egy adott magas rendelkezésre állású megoldás gondolniuk. Magas rendelkezésre állású egyszerűen valósul meg a redundancia és ezáltal rendelkező elegendő azokat a különböző virtuális gépek. Azok az összes kell helyezni az azonos Azure rendelkezésre állási csoport elkerülése érdekében, hogy a virtuális gépek tervezett karbantartás leállások során egy időben előfordulhat, hogy frissíthetők. Az alapvető funkciókat épít, különböző frissítési és tartalék tartományok belül egy Azure méretezési egység, amely már jelent fejezetben [frissítési tartományok][planning-guide-3.2.2]. Az Azure rendelkezésre állási készletek fejezetben bemutatott [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] ebben a dokumentumban.

Nincs hiba, és a frissítési tartományok, amelyek segítségével Azure rendelkezésre állási csoport egy Azure méretezési egység belül korlátlan számú van. Ez azt jelenti, hogy egy rendelkezésre állási készlethez megadott előbb vagy később az, hogy az azonos tartalék vagy frissítéséhez tartományi említi egynél több virtuális gép üzembe a virtuális gépek száma

Néhány SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépeken telepíti, és feltéve, hogy azt a kapott a következő 5 frissítési tartományok, a következő kép jelenik meg a végén. Rendelkezésre állási csoportok hiba és a frissítési tartományok maximális száma a jövőben esetleg módosító:

![Magas rendelkezésre ÁLLÁSÚ SAP alkalmazáskiszolgáló az Azure-ban][planning-guide-figure-3000]

További részletek találhatók a jelen dokumentációban: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Az SAP (A) SCS példányához, a Windows a magas rendelkezésre állás
Windows Server feladatátvételi fürt (WSFC) egy olyan gyakran használt megoldás az SAP (A) SCS példány védelmét. Azt is integrálva van egy "magas rendelkezésre ÁLLÁSÚ telepítés" formában sapinst. Ezen a ponton a időben történő nincs beállításához a szükséges Windows Server feladatátvételi fürtszolgáltatás ugyanúgy fordult elő a helyi funkciókat nyújt az Azure-infrastruktúra.

2016. január az Azure felhőalapú platform, a Windows operációs rendszer nem biztosít a két Azure virtuális gépek között megosztott lemezt a fürt megosztott kötetei használatának lehetőségét.

Egy érvényes megoldás, ha a 3. fél szoftver, amely megosztott kötet biztosítja, mivel a szinkron és átlátható lemez replikációt, amely integrálható a WSFC használatát. Ez a megközelítés azt jelenti, hogy csak az aktív fürtcsomóponton érhessék el az egyik lemez példány egy időben. Január 2016 a magas rendelkezésre ÁLLÁSÚ konfiguráció támogatott windowsos vendég operációs rendszer Azure virtuális gépeken, valamint a 3. fél szoftver SIOS DataKeeper SAP (A) SCS-példányhoz védelme érdekében.

A SIOS DataKeeper megoldás a azzal, hogy biztosítja a Windows feladatátvételi fürtöket egy megosztott lemez fürterőforrását:

* Egy további Azure virtuális merevlemez csatolva a virtuális gépek (VM), amelyek a Windows-fürt konfigurációjába mindegyikének
* Mindkét virtuális gép csomópontokon futó SIOS DataKeeper Cluster Edition
* SIOS DataKeeper Cluster Edition szinkron módon tükrözi a további virtuális merevlemez tartalmát úgy, hogy a forrás virtuális gépeknek további virtuális merevlemezre csatlakoztatott kötet csatlakoztatott kötet a cél virtuális gép.
* SIOS DataKeeper a forrás- és helyi kötetek absztrakt, és azok Windows feladatátvevő fürt, amely egyetlen megosztott lemezként bemutatásának.

Windows rendszerű feladatátvevő fürtök telepítésével SIOS Datakeeper és az SAP található összes részletes a [fürtszolgáltatás SAP ASC példányhoz használatával Windows Server feladatátvevő fürt SIOS DataKeeper rendelkező Azure] [ ha-guide-classic] találhatók meg.

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Az SAP (A) SCS példány Linux magas rendelkezésre állás
Től Dec 2015 nincs is egyenértékű megosztott lemezre WSFC Azure Linux virtuális gépekhez. 3. fél szoftverrel SIOS a Windows-alternatív megoldásokat a rendszer nem érvényesíti még az SAP futó Azure Linux.

#### <a name="high-availability-for-the-sap-database-instance"></a>Az SAP-adatbázispéldány magas rendelkezésre állás
A szokásos SAP adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ telepítése a két adatbázis-kezelő virtuális gépeken, ahol adatbázis-kezelő magas rendelkezésre állású funkció használható replikálja az adatokat az aktív DBMS példányból a második virtuális gép be a passzív adatbázis-kezelő példánya alapul.

Az adatbázis-kezelő magas rendelkezésre állású és vész helyreállítási funkciók általában, valamint a meghatározott adatbázis-kezelő ismerteti a [adatbázis-kezelő telepítési útmutatója][dbms-guide].

#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Végpontok közötti magas rendelkezésre állás a teljes SAP rendszerhez
Teljes SAP NetWeaver magas rendelkezésre ÁLLÁSÚ architektúrájának az Azure - két példa egy a Windows és Linux egyet.
A fogalmakat, ahogy az alábbi leírásban esetleg kicsit megsértik, amikor sok SAP-rendszerek telepítése és a telepített virtuális gépek száma túllépte a maximális száma a tárfiókok előfizetésenként. Ebben az esetben virtuális gépek virtuális merevlemezeket kell egy Tárfiókon belül használható együtt. Általában tennénk VHD-k az SAP alkalmazásréteg virtuális gépek különböző SAP rendszerek kombinálásával.  Azt is együtt a különböző VHD-k különböző adatbázis-kezelő virtuális gépek különböző SAP rendszerek egy Azure Storage-fiókot. Azure Storage-fiókok IOPS-korlátok vonatkoznak ezáltal tartva ( <https://azure.microsoft.com/documentation/articles/storage-scalability-targets> )

##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] Magas rendelkezésre ÁLLÁSÚ Windows rendszeren
![SAP NetWeaver alkalmazás magas rendelkezésre ÁLLÁSÚ architektúra Azure IaaS SQL-kiszolgálót][planning-guide-figure-3200]

A következő Azure szerkezet által infrastrukturális problémára gyakorolt hatásának minimalizálása érdekében és javítását üzemeltetéséhez használt az SAP NetWeaver rendszerhez:

* A teljes rendszer Azure (kötelező – ugyanazon a helyen futtatnia kell az adatbázis-kezelő réteg, (A) SCS példány és a teljes alkalmazás réteg) van telepítve.
* A teljes rendszer fut (kötelező) egy Azure-előfizetéssel belül.
* A teljes rendszert futtat egy Azure virtuális hálózaton belül (kötelező).
* A három rendelkezésre állási be a virtuális gépeket egy SAP rendszer való ugyanahhoz a virtuális hálózathoz tartozó virtuális gépek mellett is lehetőség.
* Adatbázis-kezelő példányok egy SAP rendszer futó összes virtuális gép van egy rendelkezésre állási csoportban. Feltételezzük, hogy van-e az adatbázis-kezelő példányok esetében a rendszer futtató óta natív adatbázis-kezelő magas rendelkezésre funkcióit használják, mint például az SQL Server AlwaysOn vagy Oracle Data Guard egynél több virtuális.
* Adatbázis-kezelő példányok futó összes virtuális gép saját tárfiókot használni. Adatbázis-kezelő adatainak és naplókönyvtárainak fájlok lesznek replikálva az egy tárfiókot szinkronizálja az adatokat az adatbázis-kezelő magas rendelkezésre állású funkciókat használja egy másik tárfiókhoz. Egy tárfiókot elérhetetlensége miatt hiányában egy SQL-Windows-fürt csomópontja, de nem a teljes SQL Server szolgáltatás.
* (A) SCS példányát egy SAP rendszert futtató összes virtuális gép van egy rendelkezésre állási csoportban. A virtuális gépek Windows Server feladatátvételi fürt (WSFC) (A) SCS példány védelmére van konfigurálva.
* (A) SCS példányok futó összes virtuális gép saját tárfiókot használni. (A) SCS példány fájlok és a SAP globális mappa lesznek replikálva az egy tárfiókot SIOS DataKeeper replikációt használ egy másik tárfiókhoz. Hiányában egy tárfiókot, akkor az egyik elérhetetlensége (A) SCS Windows fürtcsomópontra, de nem teljes (A) SCS szolgáltatás.
* Képviselő az SAP alkalmazásréteg-kiszolgáló virtuális gépek vannak egy harmadik rendelkezésre állási csoport.
* SAP alkalmazáskiszolgálók futó összes virtuális gép saját tárfiókot használni. Egy tárfiókot elérhetetlensége miatt hiányában egy SAP alkalmazáskiszolgáló, ha más SAP-AS továbbra is futni.

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] Magas rendelkezésre ÁLLÁSÚ Linux rendszeren
Az SAP magas rendelkezésre ÁLLÁSÚ Azure Linux architektúra tulajdonképpen ugyanaz, mint a Windows fent leírt módon. 2016. január van, ha a két korlátozások:

* csak a SAP ASE 16 jelenleg támogatott Azure-on Linux nélkül valamelyik ASE replikációs szolgáltatást.
* SAP (A) SCS magas rendelkezésre ÁLLÁSÚ megoldás nincs még az Azure-on Linux rendszeren támogatott

Ennek következtében 2016. január egy SAP-Linux-Azure rendszer nem azonos rendelkezésre állásának eléréséhez SAP-Windows-Azure rendszert miatt magas rendelkezésre ÁLLÁSÚ hiányzik a (A) SCS-példány és az Egypéldányos SAP ASE adatbázis.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Automatikus indítás SAP-példányok használata
SAP SAP példányok elindítani a virtuális Gépen belül az operációs rendszer elindítása után azonnal funkciót kínál. A pontos lépések az SAP Tudásbázis volt dokumentált [1909114] – hogyan elindítani az SAP-példányok automatikusan az automatikus indítási paraméter használatával. Azonban SAP nem javasolja a beállítás használata többé nincs vezérlő sorrendjében példány újraindul, mert egynél több virtuális gép feltételezve van hatással, vagy több példány futtatott virtuális gépenként. Feltéve, hogy egy SAP application server-példány a virtuális gépek és a kis-és egy virtuális végül első újraindítása a jellemző Azure forgatókönyv, az automatikus indítási nincs igazán fontos, és ez a paraméter hozzáadásával engedélyezhető:

    Autostart = 1

A start profilba SAP ABAP és/vagy a Java-példány.

> [!NOTE]
> Az automatikus indítási paraméter néhány downfalls is rendelkezhet. Részletesen, a paraméter egy SAP ABAP vagy Java példány elindítása a példány kapcsolódó Windows/Linux-szolgáltatás elindítása váltja. Biztosan Ez a helyzet, amikor az operációs rendszerek indul el. Azonban az SAP-szolgáltatások újraindítása is egy közös lépésként SAP szoftver életciklus-kezelési funkciók, például a SUM vagy más frissítéseket, vagy frissíti. Ezek a funkciók nem vár egy példányát az összes automatikusan újraindul. Ezért a Autostart paraméter le kell tiltani az ilyen feladatok futtatása előtt. Az automatikus indítási paraméternek is nem használandó vannak foglalva, ASC/SCS/CI például SAP-példányok.
>
>

Tekintse meg a további információhoz autostart az SAP-példányok itt:

* [SAP, valamint a Unix kiszolgáló indítása/leállítása indítása/leállítása](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Indítása és leállítása SAP NetWeaver felügyeleti ügynökök](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Indítsa el a HANA-adatbázisból automatikus engedélyezése](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Nagyobb 3 szintű SAP rendszerek
Magas rendelkezésre állású aspektusainak 3 szintű SAP konfigurációk kapott tárgyalt korábbi szakaszokban már. De mi a helyzet a rendszerek, ahol az adatbázis-kezelő kiszolgáló követelményei túl nagy ahhoz, azok található, Azure, de az SAP alkalmazásréteg is telepíthető, az Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>3 rétegű SAP konfigurációk helye
Az alkalmazás réteg leíró nem támogatja a helyszíni és az Azure közötti réteg saját magát vagy az alkalmazás és az adatbázis-kezelő. Egy SAP rendszer teljesen telepített helyi vagy az Azure-ban. Nem is támogatott rendelkeznek az alkalmazáskiszolgálókat, futtassa a helyszíni és más Azure-ban. Ez az ismertető kiindulópontjaként. Azt is nem támogatják az SAP-rendszer és az SAP server alkalmazásréteg két különböző Azure-régiókban telepített adatbázis-kezelő összetevők. Például USA középső RÉGIÓJA, USA nyugati régiója és SAP alkalmazásréteg az adatbázis-kezelő. Nem támogatja az ilyen konfigurációt oka, hogy a késés érzékenységi a SAP NetWeaver architektúra.

Azonban során az elmúlt évben center fejlesztett partnerek közös helyének az Azure-régiókat. Ezeket a helyeket közös gyakran történik a nagyon közel a fizikai az Azure data központok belül egy Azure-régiót. A rövid távolság és a kapcsolat az Azure ExpressRoute segítségével a közös elhelyezés eszközök eredményezhet, amely kisebb, mint 2ms késleltetése. Ilyen esetben az adatbázis-kezelő réteg (beleértve a tárolási SAN/NAS) található ilyen a közös elhelyezés és az SAP alkalmazásréteg az Azure-ban lehetőség. Től Dec 2015 jelenleg nem áll a telepítések hasonlóan. Azonban nem SAP alkalmazástelepítések rendelkező, különböző ügyfelektől már ilyen megközelítések használ.

### <a name="offline-backup-of-sap-systems"></a>Offline biztonsági másolat az SAP-rendszerek
Függ az SAP-konfiguráció (rétegének 2 vagy 3 szintű) van kiválasztva a biztonsági másolat szükség lehet. A tartalom a virtuális gép plusz maga az adatbázis biztonsági mentésére. A DBMS kapcsolódó biztonsági mentések várható adatbázis metódusával kell elvégezni. A különböző adatbázisokhoz tartozó részletes leírása megtalálható [DBMS útmutató][dbms-guide]. Másrészről a SAP adatok készíthető (beleértve az adatbázis tartalmát is) offline módon ebben a szakaszban leírtak szerint online és a következő szakaszban leírtak szerint.

Az offline biztonsági másolat alapvetően lenne szükség a virtuális Gépet az Azure portálon keresztül leállítására, és a virtuális alaplemez és az összes másolatát csatlakoztatott virtuális merevlemezek a virtuális gép. Ezzel a virtuális gép és a kapcsolódó lemezt idő kép ponttá megőrzik. Javasoljuk, hogy a "biztonsági mentések" átmásolja egy másik Azure Storage-fiókot. Ezért eljárása a fejezet [Azure Storage-fiókok között a lemezek másolásának] [ planning-guide-5.4.2] ebben a dokumentumban csak akkor vonatkozik.
A leállás mellett egy Azure-portál használatával is megteheti Powershell vagy a parancssori felületen keresztül itt: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Visszaállítás, az állapotban, amelyek az alap virtuális gép, valamint az alap virtuális gép eredeti lemezek törlése, és a csatlakoztatott virtuális merevlemezek, a mentett virtuális merevlemezek másolása az eredeti Tárfiók és a rendszer majd újratelepítésével.
Ez a cikk példáját mutatja be ezt a folyamatot, a Powershell parancsfájl módjáról: <http://www.westerndevs.com/azure-snapshots/>

Győződjön meg arról, hogy egy új SAP licenc telepítése, mert egy virtuális gép biztonsági mentése fent leírt módon restoing létrehoz egy új hardver kulcsot.

### <a name="online-backup-of-an-sap-system"></a>Az SAP rendszer online biztonsági mentés
Az adatbázis-kezelő a biztonsági mentés az adott adatbázis-kezelő módszerekkel leírtak szerint a [DBMS útmutató][dbms-guide].

Más virtuális gépek, a SAP rendszerben készíthető funkciójával Azure virtuális gépek biztonsági mentéséhez. Az Azure virtuális gép biztonsági mentési korai 2015 kapott bevezetett és eközben szabványos módszer biztonsági másolatot a teljes Azure-ban. Azure biztonsági mentés Azure-ban tárolja a biztonsági mentések, és lehetővé teszi, hogy a virtuális gépek visszaállítási újra.

> [!NOTE]
> Től Dec 2015 VM biztonsági mentéssel megőrzése nélkül SAP használt egyedi virtuális gép azonosítója licencelési. Ez azt jelenti, hogy a virtuális gép biztonsági másolat visszaállítása egy SAP új licenckulcs telepítése szükséges, mivel a visszaállított virtuális Gépet egy új virtuális Gépet, és nem helyettesíti a korábbi egy mentett kell tekinteni.
> 2016. január Azure virtuális gép biztonsági mentése nem támogatja a virtuális gépek üzembe helyezett Azure Resourc Manager még.
>
> ![Windows][Logo_Windows] Windows
>
> Elméletileg virtuális gép futtatási adatbázisok biztonsági másolat készíthető következetesen, valamint ha az adatbázis-kezelő rendszer támogatja a Windows VSS (kötet árnyékmásolata szolgáltatás <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx> ) például az SQL Server nem.
> Azonban figyelembe időpontban adatbázisok visszaállítása Azure virtuális gép biztonsági másolatok alapján, amelyek nem lehetséges. A javaslat ezért biztonsági mentéshez az adatbázisok adatbázis-kezelő funkciójú ahelyett, hogy az Azure virtuális gép biztonsági mentése
>
> Ismerkedjen meg az Azure virtuális gépek biztonsági mentéséhez indítsa el itt: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Egyéb lehetőségek a Microsoft Data Protection Manager telepítése egy Azure virtuális gép és egy Azure Backup szolgáltatás a biztonsági mentés/visszaállítás adatbázisokhoz kombinációjának használatára. További információk itt találhatók: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.  
>
> ![Linux][Logo_Linux] Linux
>
> Nincs a Windows VSS lévő Linux megfelelője. Ezért csak a fájlkonzisztens biztonsági mentések is lehetséges, de nem alkalmazáskonzisztens biztonsági mentés. Az SAP DBMS biztonsági mentést kell végezni az adatbázis-kezelő szolgáltatással. A fájlrendszer, amely tartalmazza az SAP kapcsolatos adatokat is menthető pl. használatával bont lásd itt: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Az éles SAP tájak vész-Helyreállítási helyként Azure
Mid 2014 óta különböző összetevőket Hyper-V, a System Center és az Azure-bővítmények engedélyezése az Azure használatát vész-Helyreállítási helyként helyszíni Hyper-v rendszerű virtuális gépek.

Egy blog, és részletesen leírja a megoldás központi telepítéséről itt dokumentált: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## <a name="summary"></a>Összefoglalás
A legfontosabb, a magas rendelkezésre állás SAP rendszerekhez az Azure-ban a következők:

* Ezen a ponton az időben a SAP hibaérzékeny pontok kialakulását nem védhető ugyanúgy lehet végezni helyszíni telepítések esetén. A hiba oka, hogy a megosztott lemez fürtök nem még hozható létre az Azure-ban a 3. fél szoftverek használata nélkül.
* A DBMS réteg kell használnia, amely nem megosztott lemez fürt technológia szükséges adatbázis-kezelő funkcióit. Részletek ismertetett a [DBMS útmutató][dbms-guide].
* Az Azure infrastruktúra vagy a gazdagép karbantartási tartalék tartományok belül előforduló problémákról a hatás minimalizálása érdekében ajánlott Azure rendelkezésre állási csoportok:
  * Javasoljuk, hogy egy rendelkezésre állási készlet az SAP alkalmazásréteg rendelkezik.
  * Javasoljuk, hogy egy külön rendelkezésre állási csoport a SAP DBMS réteg rendelkezik.
  * NEM ajánlott, az azonos rendelkezésre állási készletét, különböző SAP rendszert virtuális gépeinek alkalmazandó.
* Az SAP DBMS réteg alkalmazásában biztonsági mentést, ellenőrizze a [DBMS útmutató][dbms-guide].
* SAP párbeszédpanel példányok biztonsági másolatának szabálykészletében kevés mert általában gyorsabb egyszerű párbeszédpanel példányok újratelepíteni.
* Biztonsági mentés a virtuális Gépet, amely tartalmazza a globális könyvtár az SAP rendszer, és azt a másik példány a profilok értelme és kell végezhető el a Windows biztonsági másolat vagy pl. Linux bont. Mivel a Windows Server 2008 (R2) és a Windows Server 2012 (R2) közötti különbségeket, amelyek révén könnyebben biztonsági mentését, a korábbi Windows Server használatával feloldja, javasoljuk, Windows Server 2012 (R2) Windows vendég operációs rendszerként való futtatásra.
