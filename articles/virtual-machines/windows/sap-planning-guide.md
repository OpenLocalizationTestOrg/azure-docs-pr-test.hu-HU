---
title: "aaaSAP NetWeaver Azure virtuális gépeken – tervezése és megvalósítása |} Microsoft Docs"
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
ms.openlocfilehash: 53d63240760282b409f7e9412e8240689bcbcc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
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

A Microsoft Azure lehetővé teszi a vállalatok tooacquire számítási és tárolási erőforrásokat minimális időben hosszadalmas beszerzési ciklusok nélkül. Az Azure virtuális gépek lehetővé teszik a vállalatok toodeploy klasszikus alkalmazások, például SAP NetWeaver-alapú alkalmazások az Azure, és a megbízhatóság és rendelkezésre állás kiterjesztése anélkül, hogy további erőforrás érhető el a helyszínen. Az Azure virtuális gép szolgáltatások közötti kapcsolatot nyújthassanak, mely lehetővé teszi a vállalatok tooactively Azure virtuális gépek integrálja a helyi tartomány, a Magánfelhők és az SAP rendszer fekvő is támogatja.
Ez a dokumentum ismerteti a hello alapjai a Microsoft Azure virtuális gép és biztosít egy SAP NetWeaver telepítések az Azure-ban tervezési és megvalósítási szempontok segédlet és ilyen kell hello dokumentum tooread elindítása előtt SAP NetWeaver Azure tényleges példányai.
hello papír kiegészítés hello SAP dokumentáció és az SAP képviselő hello elsődleges erőforrások telepítése és a SAP szoftver központi telepítését az adott platformok.

## <a name="summary"></a>Összefoglalás
A felhőalapú informatika egy olyan széles körben használt kifejezés, amely egyre több fontos hello informatikai iparági belül van való kis vállalatok toolarge és multinacionális cég vállalatok fel.

A Microsoft Azure Cloud Services Platform széles skálája új lehetőségeket kínál, amelyek Microsoft hello. Most ügyfelek rendszer képes toorapidly kiépítése és deaktiválás rendelkezés alkalmazás hello felhőalapú szolgáltatásként, hogy ne korlátozott tootechnical vagy költségvetési korlátozások. Helyett befektetés időt és a hardver infrastruktúra, a vállalatok összpontosíthat hello alkalmazás, az üzleti folyamatokat és az előnye az ügyfelek és felhasználók.

A Microsoft Azure Virtual Machine Services szolgáltatással a Microsoft átfogó szolgáltatott infrastruktúra (IaaS) platformot kínál. Az Azure virtuális gépek támogatják az SAP NetWeaver-alapú alkalmazásokat (IaaS). E tanulmány ismerteti, hogyan tooplan és megvalósítása SAP NetWeaver-alapú alkalmazások a Microsoft Azure-ban hello platformként választott.

hello papír maga összpontosítanak két fő szempontjait:

* hello első rész ismerteti, két támogatott üzembe helyezési mintát SAP NetWeaver alapú alkalmazásokhoz az Azure-on. Azt is leírja az SAP üzemelő példányokkal figyelembe Azure általános kezelését.
* végrehajtási hello két különböző alkalmazási helyzetek hello első részében leírt hello második része részletesen ismerteti.

További források című fejezet [erőforrások] [ planning-guide-1.2] ebben a dokumentumban.

### <a name="definitions-upfront"></a>Definíciók előzetes megfizetése esetén
Hello a dokumentum a következő feltételek hello használjuk:

* IaaS: Szolgáltatott infrastruktúra.
* A PaaS: Platform szolgáltatásként.
* SaaS: Szolgáltatott szoftver.
* ARM: Az Azure Resource Manager
* SAP összetevő: az egyes SAP alkalmazások például ECC, a Sávszélesség, a megoldás Manager vagy a EP  SAP-összetevők a hagyományos ABAP vagy Java technológiák vagy egy nem NetWeaver alapú alkalmazás, például üzleti objektumok is alapulhat.
* SAP-környezetben: egy vagy több SAP összetevők logikailag egy csoportba tooperform például fejlesztési, a QAS, a képzési, a vész-Helyreállítási vagy a termelési üzleti függvényt.
* SAP fekvő: Utal toohello egész SAP eszközök az ügyfél informatikai fekvő. hello SAP fekvő minden üzemi és nem éles környezetben tartalmaz.
* SAP-verzió: hello DBMS réteg és kombinációja alkalmazásréteg például egy SAP ERP fejlesztőrendszer SAP BW gazdagépes tesztrendszer, SAP CRM éles rendszer, stb. Az Azure-ban a központi telepítése nem ezen két réteg között a helyszíni és az Azure támogatott toodivide. Ez azt jelenti, hogy egy SAP rendszer vagy a helyszínen telepített vagy telepítve van az Azure-ban. Telepíthet azonban hello különböző egy SAP fekvő Azure vagy a helyszíni rendszeren. Például sikerült hello SAP CRM fejlesztési és tesztelési az Azure-ban rendszerek központi telepítéséhez, de SAP CRM éles rendszer helyszíni hello.
* Csak felhőalapú telepítési: egy központi telepítést, amelyen keresztül a pont-pont nincs csatlakoztatva a hello Azure-előfizetés, vagy ExpressRoute kapcsolat toohello helyszíni hálózati infrastruktúra. Közös Azure dokumentációja az ilyen típusú központi telepítések egyaránt "Csak felhőalapú" telepítések leírása. Ezzel a módszerrel telepített virtuális gépek keresztül elért hello internet és egy nyilvános IP-címet és/vagy egy nyilvános DNS-nevet hozzárendelt toohello virtuális gépek Azure-ban. A Microsoft Windows hello a helyszíni Active Directory (AD), és a DNS nincs kibővítve az ilyen típusú központi telepítések tooAzure. Ezért hello virtuális gépek részei hello a helyszíni Active Directoryban. Is igaz Linux-megvalósítások esetében pl. OpenLDAP + Kerberos használatával.

> [!NOTE]
> Ebben a dokumentumban csak felhőalapú telepítések definiált teljes SAP tájak kizárólag az Azure Active Directory kiterjesztés nélkül futnak / OpenLDAP vagy a nyilvános felhőbe helyszíni névfeloldás. Csak felhőalapú konfigurációk nem támogatottak a termelési SAP rendszerek vagy konfigurációk, ahol az SAP STM vagy más helyszíni erőforrásokat kell toobe használt Azure és a helyszínen található erőforrások üzemeltetett SAP-rendszerek között.
>
>

* Létesítmények közötti: Egy olyan virtuális gépek esetén az Azure-előfizetéssel, amely rendelkezik pont-pont, többhelyes üzembe helyezett tooan vagy hello helyszíni datacenter(s) és az Azure között ExpressRoute kapcsolat forgatókönyvet ismertet. Közös Azure dokumentációja, az ilyen típusú központi telepítések egyaránt létesítmények közötti forgatókönyv leírása. hello OK hello kapcsolat érték tooextend a helyi tartomány, a helyszíni Active Directoryban / OpenLDAP és a helyszíni DNS az Azure. hello helyszíni fekvő kiterjesztett toohello Azure eszközök hello előfizetés van. Ha ezt a bővítményt, hello virtuális gépek hello a helyi tartomány része lehet. Tartományi felhasználók hello helyszíni tartomány hello kiszolgálókat érhetnek el, és szolgáltatásokat futtathatja a virtuális gépek (például adatbázis-kezelő szolgáltatás). Telepített virtuális gépek a helyszíni és az Azure telepített virtuális gépek közötti kommunikációt és a névfeloldás lehetőség. Ez a legtöbb SAP eszközök toobe telepített várhatóan hello forgatókönyv.  Lásd: [ez] [ vpn-gateway-cross-premises-options] cikk és [ez] [ vpn-gateway-site-to-site-create] további információt.

> [!NOTE]
> Létesítmények közötti telepítések SAP rendszert futtató Azure virtuális gépek esetén a helyszíni-tartománybeli tagsággal SAP rendszerek éles SAP rendszerek támogatottak. Létesítmények közötti konfigurációk támogatottak a kijelzők telepítése, vagy végezze el az SAP tájak az Azure. Az Azure-ban futó hello teljes SAP fekvő szükség van arra, azokat a virtuális gépek helyszíni tartományban és REKLÁMOK részévé / OpenLDAP. Hello dokumentáció korábbi verzióiban megtartásról kapcsolatos hibrid-IT-forgatókönyvek, ahol hello kifejezés "Hibrid" hello valójában a létesítmények közötti hálózati kapcsolatot a helyszíni és az Azure közötti feltört eszköz. Továbbá, hello tényt, hogy az Azure virtuális gépek hello hello részét képezik a helyszíni Active Directory / OpenLDAP.
>
>

Néhány Microsoft dokumentációjában létesítmények közötti forgatókönyv különösen az adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ konfiguráció egy kicsit másképp ismerteti. Hello a kis-és hello SAP kapcsolódó dokumentumokat, hello létesítmények közötti forgatókönyv éppen forrni kezd toohaving le egy pont-pont vagy titkos (ExpressRoute) kapcsolatot, valamint hello tény, hogy hello SAP fekvő elosztása a helyszíni és az Azure között.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Erőforrások
hello következő kiegészítő útmutatók érhetők el a hello témakör Azure SAP telepítések:

* [SAP NetWeaver az Azure virtuális gépek (VM) – tervezési és megvalósítási útmutató (Ez a dokumentum)][planning-guide]
* [SAP NetWeaver az Azure virtuális gépek (VM) – telepítési útmutató][deployment-guide]
* [SAP NetWeaver az Azure virtuális gépek (VM) – adatbázis-kezelő telepítési útmutatója][dbms-guide]
* [SAP NetWeaver az Azure virtuális gépek (VM) – magas rendelkezésre állású telepítési útmutatója][ha-guide]

> [!IMPORTANT]
> Ahol csak lehetséges egy SAP telepítési útmutató utaló hivatkozás toohello használatos (hivatkozás InstGuide-01, lásd: <http://service.sap.com/instguides>). Ha ismét toohello Előfeltételek és a telepítési folyamat során hello SAP NetWeaver telepítési útmutatók kell lennie, gondosan olvassa el, mivel ez a dokumentum csak meghatározott feladatok SAP NetWeaver számítógépekhez a Microsoft Azure virtuális gép tartalmazza.
>
>

a következő SAP megjegyzések hello kapcsolódó toohello a témakör az Azure SAP:

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

Olvassa el is hello [Állapotváltozás Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux tartalmazó összes SAP-jegyzetet.

Általános alapértelmezett korlátozások és az Azure-előfizetések maximális korlátai található [Ez a cikk][azure-subscription-service-limits-subscription]

## <a name="possible-scenarios"></a>A következő eljárások
SAP gyakran előforduló hello legtöbb létfontosságú alkalmazások belül vállalatok számára rendelkezésre álló. hello architektúra és az alkalmazások műveletek többnyire nagyon bonyolult, és győződjön meg arról, hogy megfelel a követelményeknek, a rendelkezésre állás és teljesítmény fontos.

Így a vállalat rendelkezik toothink gondosan arról, hogy mely alkalmazások futtathatja a kiválasztott hello felhőszolgáltatóként független nyilvános felhő környezetben.

SAP NetWeaver telepítéséhez lehetséges rendszertípusok-alapú alkalmazások nyilvános felhőben környezetekben alábbiakban találhatók:

1. Közepes méretű éles rendszerek esetén.
2. Fejlesztési rendszerek
3. Tesztelési rendszerek
4. Prototípus rendszerek
5. Tanulási / bemutató rendszerek

A sorrend toosuccessfully rendszerek központi telepítéséhez SAP Azure IaaS vagy infrastruktúra-szolgáltatási általában, fontos toounderstand hello jelentős és közötti különbségek hello hagyományos outsourcers vagy a szolgáltatók által kínált infrastruktúra-szolgáltatási ajánlatok. Mivel hello hagyományos szolgáltató vagy outsourcer fog támogató infrastruktúra (hálózati, tárolási és a kiszolgáló típusa) toohello munkaterhelés egy ügyfél szeretne toohost, ehelyett hello ügyfél felelősségi toochoose hello jobb munkaterhelés IaaS telepítésekhez.

Első lépésként szükséges a következő elemek tooverify hello:

* hello SAP támogatott Azure VM típusai
* hello SAP támogatott termékek/kiadásokban az Azure-on
* hello támogatott operációs rendszer és az adatbázis-kezelő kiadásokban hello adott SAP kiadásai az Azure-ban
* Különböző Azure-SKU által biztosított SAP átviteli sebesség

hello toothese kérdésekre adott válaszok tudja olvasni az SAP Megjegyzés [1928533].

A második lépésben az Azure erőforrás- és sávszélesség korlátozások képest toobe tooactual hálózatierőforrás-fogyasztás helyszíni rendszerek kell. Ügyfelek kell toobe ismeri a hello különböző képességeket, ezért az SAP területén hello támogatott Azure típusok hello:

* Virtuális gép különböző típusú a CPU és memória-erőforrásokat és
* Virtuális gép különböző típusú IOPS sávszélesség és
* Virtuális gép különböző típusú hálózati képességeit.

Az adatok legnagyobb található [Itt][virtual-machines-sizes]

Ne feledje, hogy a fenti hello hivatkozás szerepel korlátok hello felső határa. Nem jelenti azt, hogy hello kapcsolatküszöbeit hello erőforrásokat, például IOPS biztosítható, hogy minden esetben. hello kivételek, ha olyan hello CPU és memória-erőforrások a kiválasztott Virtuálisgép-típussá. A SAP által támogatott hello VM típusú hello CPU és memória-erőforrások idő hello VM tartozó fenntartott, így elérhető bármely pontján.

hello Microsoft Azure platformon IaaS más platformokon, például a egy több-bérlős platformja. Ez azt jelenti, hogy a tárolási, hálózati és egyéb erőforrások megosztott bérlők között. Intelligens sávszélesség-szabályozás és a kvóta logikai használt tooprevent egy bérlő más bérlőket (zajos szomszédos) hello teljesítményét érintő drasztikus módon. Bár az Azure záma tookeep eltérésekre észlelt sávszélesség alacsony, magas megosztott platformokon a logikai toointroduce nagyobb eltérésekre általában erőforrás/sávszélesség rendelkezésre állási, mint az ügyfelek a sok vannak a helyszíni telepítések használt tooin. Ennek köszönhetően Ön is szembesülhet perces toominute sávszélesség tanúsítványinformációit toonetworking vagy a tárolási i/o (hello kötet, valamint a késés) a különböző szintjét. hello valószínűsége, hogy egy SAP rendszer Azure szabályos-nál nagyobb eltérésekre hibába egy helyszíni rendszerben kell figyelembe venni toobe.

Utolsó lépése tooevaluate rendelkezésre állási követelményeinek. Ez akkor fordulhat elő, az, hogy az alapul szolgáló Azure-infrastruktúra hello tooget frissítése, valamint hello gazdagépeken futó virtuális gépek toobe újraindítása szükséges. Ezekben az esetekben meg azokon a gazdagépeken futó virtuális gépeket szeretné kell leállításra és újraindításra is. ilyen karbantartás ütemezése hello munkaidejében kiegészítő egy adott régióban történik, de viszonylag nagy hello lehetséges ablak néhány óra során, ami egy újraindul. Különböző technológiákkal belül hello Azure platformon, amely konfigurált toomitigate néhány vagy összes hello ilyen frissítések hatással van. A jövőbeli fejlesztések hello Azure platformon, mivel az adatbázis-kezelő és az SAP-alkalmazás számára tervezett toominimize hello hatása ilyen újraindul.

Ahhoz, toosuccessfully telepíteni egy SAP Azure rendszerbe, hello helyszíni SAP rendszer(ek) operációs rendszer, adatbázis, SAP-alkalmazásokból kell szerepelnie(ük) hello SAP Azure támogatási mátrix, elférjen belül hello erőforrások hello Azure-infrastruktúra biztosít, és amely is rendelkezésre állási SLA-k a Microsoft Azure kínál hello működik. Ezekhez a rendszerekhez meghatározott van szüksége az alábbi két helyzetben hello egyik toodecide.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Csak felhőalapú - hello a függőségek nélkül az Azure virtuális gépek telepítéséhez a helyi felhasználói hálózati
![Az SAP bemutató vagy Azure szolgáltatásba telepített képzési forgatókönyv egyetlen VM][planning-guide-figure-100]

Ez a forgatókönyv része jellemzően oktatási anyag vagy bemutató rendszerek, ahol telepítve vannak minden hello összetevői SAP, és nem SAP szoftver egy virtuális belül. Éles SAP rendszerek nem támogatottak ebben a telepítési forgatókönyvben. Általában ez a forgatókönyv megfelel a követelményeknek hello:

* hello virtuális hello nyilvános hálózaton keresztül érhetők el. Közvetlen kapcsolattal hello virtuális gépek toohello belül futó hello alkalmazások a helyi hálózati vagy a tulajdonos hello demonstrációt vagy oktatási anyag tartalmát hello vállalat vagy hello vevő nincs szükség.
* Hello oktatási anyag vagy bemutató forgatókönyvet képviselő több virtuális gépek esetén a hálózati kommunikációt és a névfeloldás kell toowork hello virtuális gépek között. De hello beállítása a virtuális gépek közötti kommunikáció kell, hogy a virtuális gépek több készleteinek egymás mellett beavatkozás nélkül telepíthető elkülönített toobe.  
* Internetkapcsolattal az Azure-ban üzemeltetett virtuális gépek toohello hello végfelhasználói tooremote bejelentkezési szükség. Attól függően, hogy hello vendég operációs rendszer, Terminal Services/távoli asztali szolgáltatások vagy VNC ssh használt tooaccess hello VM tooeither hello képzési feladatok teljesítése vagy hello bemutatók végrehajtani. Ha az SAP 3200, 3300 & 3600 részleg például portok is elérhetővé tehető hello SAP alkalmazáspéldány elérhető bármely internethez csatlakoztatott asztalról.
* hello SAP rendszer(ek) (és a különálló esetén csak a felhasználói hozzáférés nyilvános internetkapcsolatra van szükség, és nem szükséges egy kapcsolat tooother az Azure virtuális gépek Azure VM(s)) jelentik.
* SAPGUI és a böngésző vannak telepítve, és közvetlenül a virtuális gép hello.
* A gyors alaphelyzetbe állítani a virtuális gép toohello eredeti állapot és új központi telepítését az eredeti állapotban ismét szükség.
* Hello esetben bemutató és képzési forgatókönyvek, amelyek több virtuális gép, egy Active Directory rendszer megvalósítani / OpenLDAP és/vagy a DNS szolgáltatásra szükség az egyes virtuális gépek.

![A virtuális gép egy bemutató vagy egy Azure-Felhőszolgáltatásban képzési forgatókönyv képviselő csoport][planning-guide-figure-200]

Fontos szem előtt, hogy a virtuális gép van hello minden hello tookeep beállítja párhuzamosan telepíteni kell toobe ahol hello VM nevét az egyes hello beállítása vannak hello azonos.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>létesítmények közötti - központi telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény abból a teljes mértékben történő integrálását hello a helyszíni hálózat
![A pont-pont kapcsolatban (létesítmények közötti) VPN][planning-guide-figure-300]

Ebben a forgatókönyvben a létesítmények közötti forgatókönyv számos lehetséges telepítési mintákat. Legegyszerűbb leírása egyes részei hello SAP fekvő a helyszíni és a SAP fekvő hello Azure-on futó lehet. Kell, hogy a végfelhasználók számára átlátszó hello SAP összetevők része az Azure-on futnak tény hello minden szempontját. Ezért hello SAP átviteli javítási rendszer (STM), a RFC kommunikációs, a nyomtatás, a biztonsági (például egyszeri bejelentkezés), stb. Azure-on futó hello SAP rendszerekhez zökkenőmentesen működjön. De hello létesítmények közötti forgatókönyv is egy olyan forgatókönyvet, ahol hello ügyfél tartomány Azure-ban futó hello teljes SAP fekvő és DNS kiterjeszthetőek az Azure ismerteti.

> [!NOTE]
> Ez a hello környezet-forgatókönyv, amely hatékony SAP rendszereket futtató támogatott.
>
>

Olvasási [Ez a cikk] [ vpn-gateway-create-site-to-site-rm-powershell] további információt a tooconnect a helyszíni hálózati tooMicrosoft Azure

> [!IMPORTANT]
> Azt is van szó létesítmények közötti forgatókönyv Azure és a helyszíni felhasználói telepítés között, azt keresi, az egész SAP rendszerek hello részletességű. Forgatókönyvek, amelyek *nem támogatott* létesítmények közötti forgatókönyv esetén:
>
> * Különböző központi telepítési módszer különböző rétegeinek SAP-alkalmazásokból fut. Például hello DBMS réteg a helyszínen, de hello SAP alkalmazásréteg futó Azure virtuális gépeken, vagy fordítva telepített virtuális gépek.
> * Azure-ban és egy helyszíni SAP réteg néhány összetevőt. Például a felosztás hello SAP alkalmazásréteg-hez és az Azure virtuális gépek közötti példányai.
> * SAP példányát egy olyan rendszert több Azure-régiókat felett futó virtuális gépeket eloszlásáról nem támogatott.
>
> hello e korlátozások oka, hogy egy SAP rendszerben, különösen hello alkalmazáspéldányok és az SAP rendszert hello DBMS réteg között nagy teljesítményű nagyon alacsony késleltetésű hálózathoz hello követelménye.
>
>

### <a name="supported-os-and-database-releases"></a>Támogatott operációs rendszer és adatbázis-kiadások
* Microsoft server szoftver esetében az Azure virtuális gép szolgáltatásokat, ez a cikk szerepel a támogatott: <http://support.microsoft.com/kb/2721672>.
* Támogatott operációs rendszer kiadások, adatbázis rendszer verziókban támogatott Azure virtuális gép szolgáltatások SAP szoftver együtt vannak dokumentálva SAP Megjegyzés [1928533].
* SAP-alkalmazásokból és -verziókat támogatja a virtuális gép Azure-szolgáltatásokra ismertetett SAP Megjegyzés [1928533].
* Csak 64 bites bináris támogatott toorun önmagukban SAP forgatókönyvek esetén a Vendég virtuális gépek Azure-ban. Ez azt is jelenti, hogy csak a 64 bites SAP-alkalmazásokból és az adatbázisok támogatottak.

## <a name="microsoft-azure-virtual-machine-services"></a>Virtuális gép Microsoft Azure-szolgáltatások
hello Microsoft Azure platform egy internet-skálázási felhőalapú szolgáltatások platform üzemeltetett, és adatközpontok Microsoft adatok üzemeltetni. hello platform hello Microsoft Azure virtuális gép szolgáltatások (infrastruktúra-szolgáltatás, vagy IaaS) és a gazdag platformok egy Platformszolgáltatási képességek egy készletét tartalmazza.

hello Azure platformon csökkenti a kezdeti technológia hello szükségességét, és infrastruktúra a későbbi szoftvervásárlások. Egyszerűbbé teszi az karbantartása és működő alkalmazások igény szerinti számítási és tárolási toohost megadásával, méretezhető és a webes alkalmazás és a csatlakoztatott alkalmazások kezelése. Infrastruktúra-kezelés a automatizált egy platform, amely a magas rendelkezésre állás és dinamikus méretezés toomatch igényei árképzési használatalapú fizetési modell hello kapcsolóval.

![A Microsoft Azure virtuális gép szolgáltatások elhelyezéséhez][planning-guide-figure-400]

Az Azure virtuális gép Services Microsoft van így már toodeploy egyéni kiszolgálói lemezképek tooAzure (lásd a 4. ábra) IaaS-példányként. hello Azure virtuális gépek a Hyper-V virtuális merevlemezek (VHD) alapuló, és képes toorun különböző operációs rendszer, a vendég operációs rendszer.

Működési szempontból az Azure virtuális gép szolgáltatást kínál hasonló hello a helyszínen telepített virtuális gépek során lép. Hello jelentős előnye, hogy nincs szükség van, azonban tooprocure, felügyelheti és kezelheti a hello infrastruktúra. A fejlesztők és rendszergazdák jogköre teljes hello operációsrendszer-lemezkép a virtuális gépekről. A rendszergazdák távolról jelentkezhet be azokat a virtuális gépek tooperform karbantartási és a feladatokat, valamint a szoftverfrissítések központi telepítésének feladatai hibaelhárítási. A legutóbb toodeployment a hello csak korlátozások vonatkoznak a hello méretű és az Azure virtuális gépek képességeit. Ezek nem lehet a finom konfigurációban részletes, mivel ezt megteheti a helyszínen. A Virtuálisgép-típusokon kombinációját jelentik választási lehetőség van:

* Vcpu, száma
* Memória
* Csatolt, VHD-k száma
* Hálózati és tárolási sávszélesség.

hello mérete és a különböző virtuális gépek különböző méretű korlátozások érhető el egy olyan táblázatában látható [Ez a cikk][virtual-machines-sizes]

Mivel különböző családok vagy a virtuális gépek sorozatát fog vegye figyelembe. Től Dec 2015 különböztetheti meg egymástól a következő virtuális gépek családok hello:

* VM a0-A7 csomag típusok: csak az SAP hitelesít. Első Virtuálisgép-sorozat Azure IaaS bevezetett kapott.
* VM a8-A11 csomag típusok: nagy teljesítményű számítástechnikai példányok. Különböző jobb végrehajtása futtatásának gazdagépek, mint más A-sorozatú virtuális gépek számítási.
* D sorozatú Virtuálisgép-típusokon: A0-A7-nál nagyobb végrehajtása. Az SAP hitelesített összes hello Virtuálisgép-típusokon.
* DS sorozatnak Virtuálisgép-típusokon: D sorozatú ugyanazon állomások használják, de tudja tooconnect tooAzure prémium szintű Storage (című [prémium szintű Azure Storage] [ planning-guide-3.3.2] a jelen dokumentum). Újra nem minden virtuális gép esetében az SAP hitelesít.
* G-sorozat Virtuálisgép-típusokon: felső memóriaterület Virtuálisgép-típusokon.
* GS sorozatnak Virtuálisgép-típusokon: G-sorozat hasonló, de beleértve hello beállítás toouse prémium szintű Azure Storage (című [prémium szintű Azure Storage] [ planning-guide-3.3.2] a jelen dokumentum). Adatbázis-kiszolgálóként GS sorozatnak virtuális gépek használata esetén kötelező toouse prémium szintű Storage DB adatok és a tranzakciós naplófájlok

Azt tapasztalhatja hello azonos Processzor- és konfigurációk másik Virtuálisgép-sorozat. Mindazonáltal, ha meg hello átviteli teljesítményt hello másik adatsorozathoz azok jelentősen eltérhetnek kívüli virtuális gépeken. Annak ellenére, hogy hello azonos Processzor- és konfigurációs. Oka az, hogy hello alapjául szolgáló kiszolgáló gazdahardverre: hello bemutatása hello eltérő típusú virtuális gép volt-e a különböző átviteli jellemzőit.  Általában hello különbség látható teljesítménye szempontjából is megjelenik hello hello ára különböző virtuális gépek.

Vegye figyelembe, hogy lehet-e minden más Virtuálisgép-sorozat érhető el a hello Azure-régiókat (az Azure-régiókat lásd a következő fejezet). Ügyeljen arra is, hogy nem minden virtuális gép vagy Virtuálisgép-sorozat hitelesített SAP.

> [!IMPORTANT]
> SAP NetWeaver-alapú alkalmazások hello használatát, a Virtuálisgép-típusokon és konfigurációk csak hello részhalmazát felsorolt SAP Megjegyzés [1928533] támogatottak.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-régiók
Microsoft lehetővé teszi a virtuális gépek toodeploy az úgynevezett "Azure-régiók". Egy Azure-régiót lehet egy vagy több olyan adatközpontokban közel található. A legtöbb hello geopolitikai régiók a hello world Microsoft van legalább két Azure-régió. Például az Európai "Észak-Európa" és "Nyugat-Európában" egy az Azure-régióban van. Ilyen geopolitikai régión belül két Azure-régiókat elválasztott elég jelentős távolság, hogy a természetes vagy technikai katasztrófák nem befolyásolják a hello mindkét Azure-régiókat geopolitikai ugyanabban a régióban. Microsoft folyamatosan globálisan fejleszt kimenő geopolitikai különböző régiókban új Azure-régiókat, mert a frissítésétől Dec 2015 elérte a már közzétett további régiókban 20 Azure régiók hello számát, és ezek a területek hello száma folyamatosan növekvő. Ügyfélként SAP rendszerek telepíthetők területekre összes ezeket, beleértve a két hello Azure-régiókat Kínában. Aktuális toodate be Azure-régiók kapcsolatos információkat lásd: ezen a webhelyen: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>a Microsoft Azure virtuális gép koncepció hello
A Microsoft Azure kínál az infrastruktúra, a szolgáltató (IaaS) megoldás toohost virtuális gépek hasonló funkciókkal rendelkező helyszíni virtualizálási megoldásaként. Áll képes toocreate származó virtuális gépek belül hello Azure portál, PowerShell vagy a parancssori felület, amely is kínál a központi telepítési és felügyeleti képességek.

Az Azure Resource Manager lehetővé teszi tooprovision olyan deklaratív sablonok segítségével az alkalmazások. Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt. Hello használata azonos sablon toorepeatedly az alkalmazást, minden szakasza hello alkalmazás-életciklus helyezi üzembe.

ARM-sablonokkal kapcsolatos további információkat talál itt:

* [Központi telepítése és virtuális gépek kezelése az Azure Resource Manager-sablonok és hello Azure parancssori felület használatával][virtual-machines-linux-cli-deploy-templates]
* [Virtuális gépeket Azure Resource Manager és a PowerShell használatával][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

Egy másik érdekes szolgáltatása hello képességét toocreate képek a virtuális gépekről, amely tooprepare bizonyos tárházak találhatók, amelynek tudja tooquickly elő a követelményeknek megfelelő virtuálisgép-példányok telepítése lehetővé teszi.

A virtuális gépekről lemezképek létrehozásával kapcsolatos további információért megtalálhatók [Ez a cikk (Windows)] [ virtual-machines-windows-capture-image] vagy a [Ez a cikk (Linux)] [ virtual-machines-linux-capture-image].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Tartalék tartományok
Tartalék tartományok határoz meg a hiba, szorosan kapcsolódó toohello fizikai infrastruktúra adatközpontok, és amíg a fizikai panel vagy állvány lehet tekinteni a tartalék tartomány található fizikai egység, nincs közvetlen egy az egyhez típusú megfeleltetés hello két között.

Több virtuális gép egy SAP rendszer a Microsoft Azure virtuális gép szolgáltatás részeként történő telepítésekor befolyásolhatja hello Azure Fabric Controller toodeploy az alkalmazás különböző tartalék tartományok, ezáltal követelményeinek hello hello A Microsoft Azure SLA-t. Azonban hello tartalék tartományok terjesztése egy Azure méretezési egységhez (akár több százszor is a számítási csomópontok vagy a tárolási csomópontokat és a hálózatkezelés gyűjteményét) keresztül vagy a virtuális gépek tooa hello hozzárendelése adott tartalék tartomány valami keresztül, amely nem rendelkezik közvetlen vezérlő. Rendelés toodirect hello Azure-hálót vezérlő toodeploy keresztül különböző tartalék tartomány virtuális gépek halmaza kell tooassign egy Azure rendelkezésre állási csoport toohello virtuális gépek központi telepítéskor. Azure rendelkezésre állási csoportokra vonatkozó további információkért lásd: fejezet [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] ebben a dokumentumban.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Frissítési tartományok
Frissítési tartományt határoz meg, amelyek segítenek az SAP rendszerben, alkotó SAP-példány a több virtuális gép fut, a virtuális gépek frissítésének toodetermine logikai egység. Frissítés esetén a Microsoft Azure végighalad hello a frissítési tartományok egyenként frissítésének folyamatát. Virtuális gépek terjesszen a központi telepítéskor keresztül különböző frissítési tartományok megvédheti a SAP rendszer részben a várható állásidő. A sorrend tooforce Azure toodeploy hello virtuális gépek különböző frissítési tartományok eloszlatva SAP a rendszer, a központi telepítéskor az egyes virtuális gépek tooset a specifikus attribútummal van szüksége. Hasonló tooFault tartományok, egy Azure méretezési egységhez több frissítési tartományt van osztva. Rendelés toodirect hello Azure-hálót vezérlő toodeploy keresztül különböző frissítési tartományok virtuális gépek halmaza kell tooassign egy Azure rendelkezésre állási csoport toohello virtuális gépek központi telepítéskor. Azure rendelkezésre állási csoportokra vonatkozó további információkért lásd: fejezet [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] alatt.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Az Azure rendelkezésre állási csoportok
Azure virtuális gépeken belül egy Azure rendelkezésre állási csoport különböző tartalék és frissítési tartományok hello Azure Fabric Controller lesznek kiosztva. hello hello terjesztési különböző tartalék és frissítési tartományok célja az SAP-rendszer nem minden virtuális gép leállítása hello kis-és infrastruktúra karbantartása vagy egy belül egy tartalék tartomány tooprevent. Alapértelmezés szerint virtuális gépek részei egy rendelkezésre állási csoportban. hello tulajdonság részvételét a virtuális gépek rendelkezésre állási csoport a központi telepítéskor vagy későbbi egy újrakonfigurálása és a virtuális gépek ismételt telepítése által definiált.

olvassa el a rendelkezésre állási csoportokra vonatkoznak, tooFault és a frissítési tartományok úgy Azure rendelkezésre állási készletek és hello toounderstand hello fogalma [Ez a cikk][virtual-machines-manage-availability]

a json-sablon használatával ARM toodefine rendelkezésre állási készletek lásd: [rest-api specifikáció hello](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) , és keressen az "availability".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Tárolás: A Microsoft Azure Storage és adatlemezek
Microsoft Azure virtuális gépek hasznosítani különböző típusú tárolókat. Azure virtuális gép szolgáltatások végrehajtási SAP esetén fontos toounderstand hello különbségek tárolási két fő típusok között:

* Nem állandó, ideiglenes tárolási.
* Az állandó tároló.

nem állandó tároló hello futó virtuális gépek közvetlenül csatlakoztatott toohello, és a hello számítási csomópontok maguk – hello helyi Egypéldányos tárolás (ideiglenes tároló) található. hello mérete hello hello hello telepítési indításakor kiválasztott virtuális gép méretétől függ. A tárolási típus: "volatile", és ezért a hello lemez inicializálva van-e a virtuálisgép-példány újraindítását követően. Hello lapozófájl hello operációs rendszer általában ezen ideiglenes a lemezen található.

- - -
> ![Windows][Logo_Windows] Windows
>
> Windows virtuális gépeken a hello ideiglenes meghajtó csatlakoztatva van, a központilag telepített virtuális gépek a D:\ meghajtóra.
>
> ![Linux][Logo_Linux] Linux
>
> A Linux virtuális gépeken /mnt/resource vagy /mnt van csatlakoztatva. További részletek itt:
>
> * [Hogyan tooAttach egy adatlemez tooa Linux virtuális gép][virtual-machines-linux-how-to-attach-disk]
> * <http://blogs.msdn.com/b/mast/Archive/2013/12/07/Understanding-the-Temporary-drive-on-Windows-Azure-Virtual-machines.aspx>
>
>

- - -
hello valódi meghajtó "volatile", mert első maga hello gazdagép-kiszolgálón. Ha hello virtuális gép áthelyezését egy újbóli üzembe helyezése (pl. esedékes toomaintenance hello állomás vagy leállítása és újraindítása) hello meghajtó hello tartalom elvész. Emiatt nincs egy beállítás toostore ezen a meghajtón fontos adatokról. az ilyen típusú tárolási használt eszköz típusa hello nagyon különböző teljesítményt nyújt, amely 2015 júniusában frissítésétől tűnik a másik Virtuálisgép-sorozat eltérnek:

* A5-A7: Erősen korlátozott teljesítményét. Nem ajánlott a semmit túl az oldal fájlja
* A8-A11: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* D sorozatú: Majd ezer IOPS nagyon jó teljesítmény jellemzőit és > 1GB/s átviteli sebesség.
* DS-méretek: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* G-sorozat: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* GS sorozatnak: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.

A fenti utasítások alkalmazzák a SAP hitelesített toohello Virtuálisgép-típusokon. használja ki az egyes adatbázis-kezelő szolgáltatásai által jogosultak hello Virtuálisgép-sorozat kiváló IOPS és átviteli sebesség. Tekintse meg a hello [adatbázis-kezelő telepítési útmutatója] [ dbms-guide] további részleteket.

A Microsoft Azure tárolás megőrzött tárolási és hello tipikus szintű védelmet és San-alapú tárolón látható redundanciát biztosít. Azure Storage alapján lemezek a virtuális merevlemez (VHD) hello Azure Storage szolgáltatásban található. hello helyi operációsrendszer-lemez (Windows C:\, Linux / (/ dev/sda1)) tárolt hello Azure Storage és a további kötetek/lemezek csatlakoztatott toohello VM lekérése tárolt, túl.

Lehetséges tooupload egy meglévő VHD-t a helyszíni vagy hozzon létre üres néhányat a meglévők közül az Azure-ban, és csatlakoztassa a virtuális gépek toodeployed. Ezen a virtuális merevlemezek Azure lemezként hivatkozott.

Miután létrehozott vagy a virtuális merevlemez feltöltése az Azure Storage lehetséges toomount, és csatolja azokat meglévő virtuális gép és a meglévő (nem) VHD toocopy tooan.

Ezen a virtuális merevlemezek megmaradnak, adatokat és változásokkal azokat a rendszer biztonságos, amikor újraindul, és újra létrehozni a virtuálisgép-példányt. Akkor is, ha a példány törlését a VHD-k biztonságban és is újra kell telepíteni, vagy nem operációsrendszer lemezek esetén lehet csatlakoztatott tooother virtuális gépek.

Belül hello Azure Storage különböző redundancia szintjének hálózati konfigurálható:

* Minimális választható szintje "helyi redundanciát", amely egyenértékű toothree-replika hello hello adatainak ugyanabban az adatközpontban egy Azure-régióhoz (című [Azure-régiókat][planning-guide-3.1]).
* Zóna redundáns tárolás három hello lemezképeket fogja eloszlatva belül különböző adatközpontokban, amelyek egy Azure-régióban hello.
* Alapértelmezett redundanciájának szintje földrajzi redundancia céljából, ami aszinkron módon replikál egy másik Azure-régióban lévő hello hello adatokat egy másik 3 képeket a hello tartalom geopolitikai ugyanabban a régióban.

Is láthatja, hogy ez a cikk tanúsítványinformációit toohello különböző redundancia beállításai fölött hello tábla: <https://azure.microsoft.com/pricing/details/storage/>

További információ a tanúsítványinformációit tooAzure tárolási itt található:

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/Site-Recovery>
* <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-Disk-Encryption-for-Linux-and-Windows-Virtual-machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Az Azure standard szintű tárolót
Az Azure szabványos BLOB storage volt hello típusú tárterület érhető el, ha Azure IaaS jelent. Nem voltak / egyetlen virtuális merevlemez IOPS kvóta. Tapasztalt késést azonos SAN/NAS-eszközökön általában telepített csúcskategóriás SAP rendszerekhez helyben tárolt osztályt hello volt. Ettől függetlenül az Azure standard szintű tárolást bizonyult elegendő-e több száz hello SAP rendszerek időközben telepítve az Azure-ban.

Az Azure standard szintű tárolást hello tényleges tárolt adatokat, storage-tranzakció, a kimenő adatátviteli és a kiválasztott redundancia opció hello mennyisége alapján számítjuk fel. Sok virtuális merevlemezek hello legfeljebb 1 TB méretű is hozható létre, de mindaddig, amíg azok továbbra is üres használata díjmentes. Ha ezután töltse ki egy virtuális Merevlemezt 100GB, fizetnie kell 100GB tárolására, nem pedig a VHD készült hello névleges méret hello.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Prémium szintű Azure Storage
A 2015. áprilisi Microsoft bevezette a prémium szintű Azure Storage. Prémium szintű Storage-ban készült bevezetett hello cél tooprovide:

* Jobb késleltetéséről.
* Nagyobb átviteli sebesség.
* A i/o-késés kevesebb variancia.

Erre a célra sok módosítást jelentek meg, melyik hello két legfontosabb vannak:

* SSD-lemezek hello Azure Storage-csomópontok használatát
* Egy új olvasási gyorsítótár által támogatott hello egy Azure számítási csomópont helyi SSD

Ha nem változtatta képességek tooStandard tárolási ellentétes hello lemez (vagy VHD), hello méretétől függ prémium szintű Storage jelenleg rendelkezik 3 másik lemezt kategóriák előtt hello – gyakori kérdések a szakasz a cikk végén hello látható: <https:/ /Azure.microsoft.com/pricing/details/Storage/>

Láthatja, hogy IOPS-/ VHD- és lemez átviteli/VHD hello mérete kategória hello lemezek függ

Prémium szintű Storage esetében hello költség alapja nincs hello tényleges adatmennyiség ilyen virtuális merevlemezek, de ilyen virtuális merevlemez, független hello adatmennyiség hello hello VHD tárolt kategóriáját hello mérete a tárolt.

Virtuális merevlemezek a prémium szintű Storage, amely nem közvetlen leképezése látható hello mérete kategóriákba hozhat létre. Erre akkor lehet hello eset, különösen akkor, ha a standard szintű tárolót a prémium szintű tároló virtuális merevlemezek másolását. Ebben az esetben a leképezési toohello következő legnagyobb prémium szintű Storage lemez használata történik.

Felhívjuk a figyelmét arra, hogy csak bizonyos Virtuálisgép-sorozat a prémium szintű Azure Storage hello kihasználhassa. Től Dec 2015-öt ezek a hello DS - és GS-méretek. hello DS sorozatnak van alapvetően hello azonos D sorozatú hello kivétellel, hogy a DS sorozatnak rendelkezik hello képességét toomount prémium szintű Storage-alapú virtuális gépek, továbbá az Azure standard szintű tárolást tooVHDs. Ugyanaz a G-sorozat képest tooGS-adatsorozathoz érvénytelen.

Vannak-e kikérése hello DS sorozatnak virtuális gépeinek hello részét [Ez a cikk] [ virtual-machines-sizes] akkor is fog vegye figyelembe, hogy nincsenek adatok mennyiségi korlátozásai tooPremium tárolási VHD-k a hello granularitási hello VM szint. Különböző DS-méretek és GS sorozatnak virtuális gépeken is korlátozásokat tanúsítványinformációit toohello számú virtuális merevlemezeket lehet csatlakoztatni. Ezek a korlátozások, valamint a fent említett hello cikkben szerepelnek. De lényegében azt jelenti, hogy 32 x P30 lemezek/VHD-k tooa pl. csatlakoztatása virtuális DS14 nem kaphat 32 x hello P30 lemez maximális átviteli sebesség. Ehelyett hello maximális átviteli sebesség a virtuális gép szinten hello cikkben leírtak szerint korlátozza az adatátvitelt.

Prémium szintű Storage további információk itt találhatók: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Azure Storage-tárfiókok
Szolgáltatások és az Azure virtuális gépeken való telepítésekor virtuális merevlemezek és a Virtuálisgép-lemezképek központi telepítését az Azure Storage-fiókok nevezett egységekben kell beállítani. Az Azure központi telepítésének tervezésekor toocarefully kell fontolja meg az Azure hello korlátozásait. Hello egyik oldalon Storage-fiókok korlátozott számú Azure előfizetésenként van. Bár minden Azure Storage-fiók nagyszámú VHD-fájlokat képes tárolni, nincs-e a rögzített korlát hello a teljes iops-értéket a Tárfiók. SAP virtuális gépek több száz jelentős IO hívások létrehozása DBMS rendszerekkel való telepítése esetén ajánlott toodistribute magas IOPS DBMS VMs több Azure Storage-fiókok között. Gondot kell fordítani a nem tooexceed hello aktuális korlát az Azure Storage-fiókok előfizetésenként. Tárolót, mert az SAP rendszer hello adatbázis központi telepítésének fontos része, a fogalom tárgyalja részletesen hello már hivatkozik a [adatbázis-kezelő telepítési útmutatója][dbms-guide].

Azure Storage-fiókokról további információ található a [Ez a cikk][storage-scalability-targets]. A cikk elolvasása még lesz megvalósítható hello korlátozások Azure standard szintű Storage-fiókok és a prémium szintű Storage-fiókok közötti különbségek vannak. Fő különbségek hello ilyen egy Tárfiókon belül tárolt adatok mennyiségét. Standard szintű tárolót a hello kötet egy nagyobb, mint a prémium szintű Storage nagyságrenddel. A hello más ügyféloldali hello standard szintű Tárfiók súlyosan korlátozott iops-érték (lásd a "Teljes kérelmek gyakorisága" oszlop), mivel hello Azure prémium szintű Tárfiók nincs ilyen korlátozás van érvényben. Ismertetjük részletek és eltérések eredmények hello telepítések SAP rendszerek, különösen a hello DBMS kiszolgálókon ismertetésekor.

A Tárfiókon belül van hello lehetőségét toocreate különböző tárolók hello célra történő szervezése és különböző virtuális merevlemezek kategorizálását. Ezek a tárolók rendszerint használt tooe.g. a különböző virtuális gépek virtuális merevlemezek külön. Nincsenek nem teljesítményre gyakorolt hatása, csak egy tárolót vagy több tároló alatt egy Azure Storage-fiók használatával.

Azure-ban egy virtuális merevlemez neve követi hello elnevezési kapcsolat, amelyet a tooprovide hello Azure-ban virtuális merevlemez egyedi nevet a következő:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

A fenti említett hello karakterláncból kell toouniquely azonosítása hello Azure Storage a tárolt virtuális merevlemez.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>A Microsoft Azure hálózatkezelés
A Microsoft Azure biztosít a hálózati infrastruktúra hello leképezési összes forgatókönyvek, amelyek azt szeretnénk, amellyel toorealize SAP szoftverrel. hello funkciók a következők:

* A hozzáférést a hello közvetlenül a virtuális gépek, a Windows Terminálszolgáltatások vagy ssh/VNC toohello kívül
* Hozzáférés tooservices és az adott alkalmazások belül hello virtuális gépek által használt portok
* Belső kommunikációs és a névfeloldás között egy központilag telepített Azure virtuális gépek csoportján
* Létesítmények közötti kapcsolat egy ügyfél a helyszíni hálózat és hello Azure-hálózat között
* Több Azure-régióban vagy az adatközpontok Azure helyek közötti kapcsolat

További információk itt találhatók: <https://azure.microsoft.com/documentation/services/virtual-network/>

Nincsenek sok különböző lehetőségek tooconfigure neve és IP-megoldás az Azure-ban. Ebben a dokumentumban csak felhőalapú forgatókönyvek az Azure DNS-sel (az ezzel szemben toodefining egy saját DNS-szolgáltatás) hello alapértelmezett támaszkodnak. Egy új Azure DNS szolgáltatást, amely helyett a saját DNS-kiszolgáló beállításához használható is van. További információ található [Ez a cikk] [ virtual-networks-manage-dns-in-vnet] és a [ezen a lapon](https://azure.microsoft.com/services/dns/).

A helyszíni hello azt a megbízható függő hello tényt a létesítmények közötti forgatókönyvek AD/OpenLDAP/DNS-megtörtént-e VPN- vagy titkos kapcsolat tooAzure keresztül. Az egyes forgatókönyvek szerint itt dokumentált lehetőségektől, előfordulhat, hogy telepítette az Azure AD/OpenLDAP replikájának szükséges toohave.

Mivel a hálózat és névfeloldás hello adatbázis egy SAP rendszer központi telepítésének fontos része, a fogalom hello részletesen tárgyalt [adatbázis-kezelő telepítési útmutatója][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure virtuális hálózatok
Egy Azure virtuális hálózat épület hello címtartomány hello Azure DHCP-funkciók által kiosztott magánhálózati IP-címeket adhat meg. A létesítmények közötti forgatókönyv hello IP-címtartomány definiált továbbra is oszt ki DHCP használatával az Azure-ban. Tartomány névfeloldás azonban helyszíni (feltéve, hogy, hogy hello virtuális gépek egy helyszíni tartomány része) történik, és ezért oldhatja címek különböző Azure-Felhőszolgáltatások túl.

[comment]: <> (Továbbra is szükséges MSSedusch? Teendők eredetileg egy Azure virtuális hálózatra kötött tooan Affinitáscsoport volt. Vele egy virtuális hálózatot az Azure-ban a kapott a következő korlátozott toohello Azure méretezési egység, hogy hello Affinitáscsoport lett rendelve. Hello end ezt jelentette hello virtuális hálózati volt korlátozott toohello erőforrások hello Azure méretezési egység érhető el. Ez megváltozott azóta, és most is kiterjesztheti az Azure virtuális hálózatok közötti egynél több Azure méretezési egység. Azonban, amely megköveteli, hogy az Azure virtuális hálózatok legyenek ** nem ** társított Affinitáscsoportok többé a létrehozás időpontjában. A Microsoft már említettük, hogy a ellentétes toorecommendations évente ezelőtt történt, akkor ** nem kihasználhatja az Azure-Affinitáscsoportok többé **. További információkért lásd: < https://azure.microsoft.com/blog/regional-virtual-networks/>)

Minden virtuális gép Azure igények toobe tooa virtuális hálózati kapcsolat.

További részletek találhatók [Ez a cikk] [ resource-groups-networking] és a [ezen a lapon](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd teendők található olyan cikk, amelyet a program hello OpenLDAP témakör + ARM;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Alapértelmezés szerint a virtuális gépek telepítése után hello virtuális hálózat a konfiguráció nem módosítható. hello TCP/IP-beállítások toohello Azure DHCP-kiszolgáló kell hagyni. Alapértelmezett viselkedés a dinamikus IP-hozzárendelés.
>
>

hello MAC-cím hello virtuális hálózati kártya például módosíthatja után újra méretezés és hello Windows vagy Linux vendég operációs rendszer hello új hálózati kártya felveszi, és ebben az esetben automatikusan használja, DHCP-tooassign hello IP-cím és DNS címek.

##### <a name="static-ip-assignment"></a>Statikus IP-hozzárendelés
Rögzített lehetséges tooassign vagy fenntartott IP-címek tooVMs egy Azure virtuális hálózaton belül. Egy nagyszerű lehetőséget tooleverage hello virtuális gépeket futtató Azure virtuális hálózatban megnyitja ezt a funkciót, ha szükséges, vagy bizonyos esetekben szükséges. hello IP-hozzárendelés egész virtuális gép független hello virtuális gép fut-e, vagy -leállítás hello hello megléte érvényes marad. Ennek eredményeképpen kell tootake virtuális gépek (fut, és a leállított virtuális Gépeken) teljes számát figyelembe hello hello az IP-címek a virtuális hálózati hello meghatározásakor. hello IP-cím megmarad, hello virtuális gép és a hálózati illesztő nem törlik, vagy amíg hello IP-cím hozzárendelése újra deszerializálni lekérdezi. A részletes információkat lásd: [Ez a cikk][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Több hálózati adapter virtuális gépenként
Egy Azure virtuális gépen a több virtuális hálózati adapterrel (vNIC) adhat meg. Hello képességét toohave több vNICs el lehet indítani tooset fel a hálózati forgalom elkülönítése, ahol például ügyfélforgalmat áthalad egy vNIC és háttér forgalmat egy második virtuális hálózati keresztül történik. A különböző korlátozás VM hello típusú függ vNICs toohello számának tekintetében. Ezek a cikkek megtalálhatók pontos részleteit, funkcióit és korlátozások:

* [Több hálózati adapterrel rendelkező virtuális gép létrehozása][virtual-networks-multiple-nics]
* [Sablon használatával több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-template]
* [PowerShell-lel több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-ps]
* [Hello Azure parancssori felület használatával több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Hely-hely kapcsolatot
Létesítmények közötti Azure virtuális gépek és a helyszíni átlátható és állandó VPN-kapcsolat csatolva. Várt toobecome hello leggyakoribb SAP telepítési minta az Azure-ban. hello feltételezése, hogy a műveleti eljárások és a folyamatok SAP osztályt az Azure-ban transzparens módon kell működnie. Ez azt jelenti, hogy meg kell kell tudni tooprint kívül ezek a rendszerek, valamint SAP átviteli felügyeleti rendszer (TMS) tootransport módosítja a fejlesztői rendszerhez telepített helyszíni Azure tooa teszt rendszerben rendszer hello használata. Pont-pont körül további dokumentációjában található [Ez a cikk][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-alagút eszköz
A rendezés toocreate webhelyek kapcsolatot (a helyszíni adatok center tooAzure adatközpont), tooeither kell beszerzése és konfigurálja a VPN-eszköz vagy az Útválasztás és távelérés szolgáltatás (RRAS) operációs rendszerben megjelent a Windows Server szoftver összetevőjeként 2012.

* [Virtuális hálózat létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal][vpn-gateway-create-site-to-site-rm-powershell]
* [VPN-eszközökről a webhelyek közötti VPN átjáró kapcsolatok][vpn-gateway-about-vpn-devices]
* [VPN Gateway – gyakori kérdések][vpn-gateway-vpn-faq]

![A helyszíni és az Azure közötti pont-pont kapcsolat][planning-guide-figure-600]

hello a fenti ábrán látható, két Azure-előfizetések használatra fenntartva az IP-cím résztartomány rendelkezik az Azure virtuális hálózatairól. hello hello kapcsolat a helyszíni hálózati tooAzure létrejön a VPN-en keresztül.

#### <a name="point-to-site-vpn"></a>Pont – hely típusú VPN
Pont – hely típusú VPN minden ügyfél gép tooconnect a saját virtuális Magánhálózatokkal az Azure van szükség. Hello SAP forgatókönyvek azt láthatjuk pont-hely kapcsolatot nincs gyakorlati. Ezért semmilyen további hivatkozások közül toopoint-site VPN-kapcsolatot.

[comment]: <> (MSSedusch – További információk itt találhatók)
[comment]: <> (MShermannd TODO hivatkozás már nem érvényes; Azonban ennek ellenére nem támogatott ARM - lásd a következő hivatkozásra:)
[comment]: <> (MSSedusch--< http://msdn.microsoft.com/library/azure/dn133798.aspx>.)
[comment]: <> (MShermannd TODO pont tooSite ARM még nem támogatott)
[comment]: <> (MSSedusch--< https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### <a name="multi-site-vpn"></a>Többhelyes VPN
Az Azure ma is kínál hello lehetőségét toocreate többhelyes VPN-kapcsolatot egy Azure-előfizetéséhez. Egy-egy előfizetéshez korábban korlátozott tooone pont-pont VPN-kapcsolatot. Ez a korlátozás tűnnek el egyetlen előfizetéssel többhelyes VPN-kapcsolatokkal. Így több lehetséges tooleverage, mint a létesítmények közötti konfigurációk keresztül az adott előfizetést egy Azure-régióban.

További dokumentációjában talál [Ez a cikk][vpn-gateway-create-site-to-site-rm-powershell]

[comment]: <> (MShermannd TODO ARM dok csatolás nem található)

#### <a name="vnet-toovnet-connection"></a>Virtuális hálózat tooVNet kapcsolat
Többhelyes VPN-kapcsolattal, kell tooconfigure hello régió külön Azure virtuális hálózat. Azonban nagyon gyakran hello szoftverösszetevőket hello különböző régiókban kell egymással kommunikáló hello követelmény, hogy. Ideális esetben ez a kommunikáció nem irányítható egy Azure-régióban tooon helyszíni és ott toohello más Azure-régióban. tooshortcut, az Azure kínál hello lehetőségét tooconfigure egy Azure virtuális hálózat az Azure virtuális hálózat egy másik régióban található egy régió tooanother kapcsolatot. Ez a funkció neve VNet – VNet-kapcsolatot. Ez a funkció a további részletek itt találhatók: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-tooazure--expressroute"></a>Magánhálózati kapcsolat tooAzure – ExpressRoute
Microsoft Azure ExpressRoute lehetővé teszi, hogy az Azure-adatközpont és a helyszíni infrastruktúrát vagy hello az ügyfél között vagy a közös elhelyezés környezetben magánhálózati kapcsolatok hello létrehozását. ExpressRoute felajánlott különböző MPLS (csomagkapcsolt) VPN-szolgáltatókkal vagy az egyéb hálózati szolgáltatók. Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. ExpressRoute kapcsolatok ajánlatot magasabb biztonsági szint, több párhuzamos kapcsolatok, a gyorsabb sebesség és a kisebb késések fordulnak elő, mint a szokásos kapcsolatok további megbízhatóság hello interneten keresztül.

További részleteket a Azure ExpressRoute- és ajánlatok itt található:

* <https://Azure.microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-faqs/>

Expressroute több Azure-előfizetések egy ExpressRoute-kapcsolatcsoportot keresztül lehetővé teszi a itt leírtak szerint

* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-linkvnet-ARM/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-Circuit-ARM/>

#### <a name="forced-tunneling-in-case-of-cross-premises"></a>A kényszerített bújtatás esetén a létesítmények közötti
A virtuális gépekhez való csatlakozás helyszíni tartományok, webhelyek, pont-hely vagy ExpressRoute kell toomake, hogy a hello internetes proxybeállítások vannak első telepítve a virtuális gépek is összes hello felhasználója számára. Alapértelmezés szerint ezeket a virtuális gépek vagy felhasználók egy böngészőben tooaccess hello interneten keresztül hello vállalati proxy nem sikerült, de rögtön az Azure toohello keresztül csatlakozni a futó szoftvereket internet. De még akkor is, hello proxybeállítást nem a 100 %-os toodirect hello forgalom hello vállalati proxyn keresztül történő felelőssége a szoftverek és -szolgáltatások toocheck hello proxy óta. Ha hello virtuális gép futó szoftver, amely nem végez, illetve a rendszergazda kezeli a hello-beállítások, internetes forgalom toohello is kell újra detoured Azure toohello interneten keresztül közvetlenül.

Ennek rendelés tooavoid, konfigurálhatja a kényszerített bújtatás a helyszíni és az Azure közötti pont-pont kapcsolatban. hello hello kényszerített bújtatás szolgáltatás részletes ismertetése közzétett Itt <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/>

Kényszerített bújtatás az ExpressRoute engedélyezve van az ügyfelek egy alapértelmezett útvonalat hirdet keresztül hello ExpressRoute BGP társviszony-létesítési munkameneteket.

#### <a name="summary-of-azure-networking"></a>Az Azure hálózatkezelés összegzése
Ez a fejezet Azure hálózati számos fontos pontok tartalmazott. Itt található egy Összegzés hello fő pontok:

* Az Azure virtuális hálózatok lehetővé teszi, hogy a tooset tooyour saját igényeinek megfelelően hello hálózat
* Egy Azure virtuális hálózatot lehet tooassign kihasználhatók IP-cím tartományokhoz tooVMs vagy rendeljen rögzített IP-címek tooVMs
* a pont-pont vagy a pont – hely kapcsolat van szüksége egy Azure virtuális hálózatra toocreate először fel tooset
* Miután telepítette a virtuális gép már nem lehetséges toochange hello virtuális hálózati hozzárendelt toohello méretű VM

### <a name="quotas-in-azure-virtual-machine-services"></a>A virtuális gép az Azure-szolgáltatások kvóták
Igazolnia kell toobe törléséhez kapcsolatos hello tény hello tárolási és hálózati infrastruktúra megosztott hello Azure-infrastruktúra számos szolgáltatáshoz futó virtuális gépek között. És hasonlóan hello ügyfél saját adatközpontok, néhány hello infrastruktúrához kapcsolódó erőforrások túlzott kiosztása hely tooa fok. a Microsoft Azure Platform hello lemez, Processzor, hálózati és egyéb kvóták toolimit hello erőforrás-felhasználás és toopreserve következetes és determinisztikus teljesítmény használja.  hello különböző virtuális gép (A5, A6 stb.) típusokhoz különböző kvótái lemezek, a CPU-, memória- és hálózati hello száma.

> [!NOTE]
> SAP által támogatott hello VM típusok a CPU és memória-erőforrások előre lefoglalt hello állomás csomópontján. Ez azt jelenti, hogy hello virtuális gép van telepítve, hello gazdagépen hello erőforrások állnak rendelkezésre hello Virtuálisgép-típussá által definiált konfigurációjának kialakításához.
>
>

Ha tervezési és méretezéssel SAP Azure megoldásokról hello minden virtuális gép méretét kvótái figyelembe kell venni.  ismerteti a hello VM kvóták [Itt][virtual-machines-sizes].

hello kvóták leírt hello elméleti maximális értékeket jelölik.  virtuális merevlemez iops-értéket hello legfeljebb kis IOS (8 KB-os) érhető el, de valószínűleg nem lehet elérni a nagy IOs (1Mb).  egyetlen virtuális merevlemezek hello granularitása megvalósul hello IOPS-korláttal.

A nyers döntési fa toodecide, Azure virtuális gép szolgáltatások és platformképességei illeszkedik az SAP rendszer vagy egy meglévő rendszer toobe rendszerben rendelés toodeploy hello Azure, másként konfigurálva kell hello döntési fa alábbi használhatók:

![Döntési fa toodecide képességét toodeploy SAP, az Azure-on][planning-guide-figure-700]

**1. lépés**: hello legfontosabb adatokat toostart rendelkező egy adott rendszerhez SAP hello SAP követelmény. hello SAP követelményeket kell toobe rétegekben hello DBMS és hello SAP alkalmazás részét, még akkor is, ha hello SAP rendszer már van telepítve a 2-réteg konfigurációjához a helyszíni. A meglévő rendszerek esetében SAP kapcsolódó toohello a hardver gyakran meghatározhatók vagy becsült hello meglévő SAP-referenciaalapok alapján. hello eredmények itt található: <http://global.sap.com/campaigns/benchmark/index.epx>.
Az újonnan telepített SAP rendszerek kell megtettünk mindent keresztül egy méretezési feladat, amely kell meghatározni, hogy a rendszer hello hello SAP követelményeinek.
Lásd még: Ebben a blogban és az SAP méretezési csatolt dokumentumot Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**2. lépés**: A meglévő rendszerek hello i/o-kötet, és i/o-műveletek másodpercenkénti száma hello DBMS kiszolgálón kell értékelni. Újonnan tervezett rendszerek esetén is méretezése a gyakorlatban hello új rendszer hello hello i/o-követelményeinek hello DBMS ügyféloldali a nyers ötleteket adjon. Ha nem ismeri, végül van szüksége a koncepció igazolása tooconduct.

**3. lépés**: hasonlítsa össze hello SAP követelmény hello adatbázis-kezelő kiszolgáló hello SAP hello VM különböző Azure biztosít. hello adatokat hello Azure virtuális gép különböző SAP SAP Megjegyzés ismertetett [1928533]. hello fókusz kell az adatbázis-kezelő VM hello először mert hello Adatbázisréteg, amely egy terjessze ki a központi telepítések többsége hello SAP NetWeaver rendszer hello réteg. Ezzel szemben hello SAP alkalmazásréteg kiterjeszthető. Ha egy hello SAP támogatja Azure Virtuálisgép-típusokon biztosíthat szükséges hello SAP, tervezett hello SAP rendszer hello munkaterhelés nem futtatható, az Azure-on. Szükség vagy toodeploy hello rendszer helyszíni, illetve toochange hello munkaterhelés kötet hello rendszer szükséges.

**4. lépés**: dokumentált [Itt][virtual-machines-sizes], Azure akár tárolási Standard vagy prémium szintű Storage érvénybe lépteti az IOPS kvótát egy virtuális merevlemez független. Virtuálisgép-típussá hello függ VHD, amelyek lehet csatlakoztatni hello száma függ. Ennek eredményeképpen kiszámításához a maximális iops-érték, amely elérhető az összes hello eltérő típusú virtuális gép. Hello adatbázis fájlelrendezés függ, virtuális merevlemezek toobecome egy kötet hello vendég operációs rendszer is paritásos. Azonban ha hello aktuális IOPS telepített SAP-rendszer meghaladják hello legnagyobb virtuális gép típusát és az Azure-e egyetlen alkalommal toocompensate több memória számított hello korlátai által megszabott, hello SAP rendszer hello munkaterhelését befolyásolhatja, súlyosan. Ebben az esetben kattintson az a pont, ahol nem kell telepít hello rendszert az Azure-on.

**5. lépés**: különösen SAP rendszereket telepítette a helyszíni 2 szintű konfigurációk, valószínűleg hello, hogy hello rendszer módosítania kell toobe konfigurált Azure 3 szintű konfiguráció. Ebben a lépésben kell toocheck összetevő hello SAP alkalmazás rétegben, amely nem terjeszthető ki, és amely nem fér el hello CPU és memória-erőforrások hello különböző Azure virtuális gép típusok ajánlat van-e. Ha valóban ilyen összetevő, hello SAP rendszer és a munkaterhelés nem telepíthető az Azure. De ha akkor is kibővíthető hello SAP alkalmazás-összetevők a több Azure virtuális gépeken, hello rendszer is telepíthető az Azure.

**6. lépés**: hello adatbázis-kezelő és az SAP réteg alkalmazásösszetevői Azure virtuális gépeken is futtatható, ha hello beállításokat kell-e toobe definiált a következőkre vonatkozóan:

* Az Azure virtuális gépek száma
* Virtuálisgép-típusokon hello egyéni összetevők
* Az adatbázis-kezelő VM tooprovide VHD-k száma elég IOPS

## <a name="managing-azure-assets"></a>Az Azure-eszközök kezelése
### <a name="azure-portal"></a>Azure Portal
hello Azure Portal három felületek toomanage Azure Virtuálisgép-telepítések egyike. hello alapvető felügyeleti feladatokat, például a lemezképeket, a virtuális gépek telepítése végezhető el hello Azure portálon. Ezenkívül hello létrehozása Tárfiókokat, virtuális hálózatok és egyéb Azure összetevők egyaránt feladatok hello Azure Portal jól kezelhető. Azonban funkciók például a virtuális merevlemezek feltöltését a helyszíni tooAzure vagy az Azure virtuális merevlemez másolása az ehhez szükséges, harmadik féltől származó eszközök vagy a PowerShell vagy a parancssori felületen keresztül felügyeleti feladatokat.

![A Microsoft Azure portál – a virtuális gépek – áttekintés][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Felügyelet és a konfigurációs feladatok hello virtuálisgép-példányt is előfordulhatnak a hello Azure portálon belül.

Újraindítás és a virtuális gép leállítása mellett is csatolása, leválasztani és hello virtuálisgép-példányt, toocapture hello példány lemezkép előkészítése az adatlemezek létrehozása és hello virtuálisgép-példányt hello méretének beállítása.

hello Azure Portal toodeploy alapvető funkciókat biztosít, és konfigurálja a virtuális gépek és sok más Azure-szolgáltatásokkal. Azonban nem az összes rendelkezésre álló funkciók hello Azure Portal vonatkozik. Hello Azure portál nincs lehetőség tooperform feladatokat, például:

* Virtuális merevlemezek tooAzure feltöltése
* Virtuális gépek másolása

[comment]: <> (MShermannd TODO Mi a helyzet automation szolgáltatás SAP virtuális gépekhez?)
[comment]: <> (Több virtuális gépek, operációs rendszer időközben lehetséges MSSedusch telepítése)
[comment]: <> (Az automatizálás vonatkozó központi telepítési típussal is MSSedusch nem hello Azure-portálon lehetséges. Feladatokat, mint a több virtuális gépek parancsfájlalapú telepítés nincs lehetőség hello Azure portálon keresztül.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>A Microsoft Azure PowerShell-parancsmagok segítségével kezelése
A Windows PowerShell telepítése az Azure-ban rendszerek nagyobb mennyiségű ügyfél széles körben elfogadott sokoldalú és bővíthető keretrendszer. A PowerShell-parancsmagok egy asztali, a laptop vagy a dedikált felügyeleti állomás hello telepítés után hello PowerShell-parancsmagok távolról futtatható.

hello folyamat tooenable hello használati Azure PowerShell-parancsmagok helyi asztali vagy hordozható, és hogyan a felhasználást a hello tooconfigure hello Azure előfizetés(ek) leírtak [Ez a cikk][powershell-install-configure].

További részletes lépéseket tooinstall, frissítése és konfigurálása hello Azure PowerShell-parancsmagok is megtalálhatók [hello telepítési útmutató ezen fejezete][deployment-guide-4.1].

Felhasználói élmény eddig portja, hogy PowerShell (PS): alapértékekkel hello hatékonyabb eszköz toodeploy virtuális gépek és toocreate egyéni virtuális gépek hello telepítési lépéseit. PS parancsmagok toosupplement felügyeleti feladatokat az Azure portál hello, vagy a PS-parancsmagok is használ az összes Azure-beli SAP példányok hello ügyfelek használ kizárólag toomanage a telepítések az Azure-ban. Mivel hello Azure egyes parancsmagokat megosztás hello hello azonos elnevezési egyezmény Windows 2000-nél több kapcsolódó parancsmagok, célszerű egy egyszerű feladathoz tartozó Windows rendszergazdák tooleverage e parancsmagok.

Itt a következő példa: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd Teendőlista új CLI parancs tesztelésekor írják le.)
Hello Azure Figyelőbővítmény az SAP telepítését (című [Azure figyelésére szolgáló megoldás az SAP] [ planning-guide-9.1] ebben a dokumentumban) csak akkor PowerShell vagy a parancssori felületen keresztül lehetséges. Ezért azt kötelező toosetup és konfigurálja a PowerShell vagy a CLI üzembe helyezésekor és felügyelete az Azure-ban az SAP NetWeaver rendszer.  

Azure több funkciót biztosít, új PS-parancsmagok fog toobe hozzáadott hello parancsmagok frissítést igényel. Ezért teszi logika toocheck hello Azure letöltési hely legalább egyszer hello hónap <https://azure.microsoft.com/downloads/> hello parancsmagok új verziója. hello új verziója hello régebbi verzióra csak lesz telepítve.

Az Azure listáját általános kapcsolódó PowerShell-parancsok be a négyzetet: <https://msdn.microsoft.com/library/azure/dn708514.aspx>.

### <a name="management-via-microsoft-azure-cli-commands"></a>A Microsoft Azure parancssori felület parancsait keresztül kezelése
Szeretné, hogy toomanage Azure Linux felhasználók erőforrások Powershell beállítást nem lehet. A Microsoft Azure CLI alternatívájaként kínál.
hello Azure parancssori felület számos nyílt forráskódú, platformok közötti parancsok végzett munka hello Azure platformra. az Azure parancssori felület hello több hello hello Azure-portálon található ugyanezeket a funkciókat tartalmazza.

További információ a telepítéshez, konfiguráláshoz és hogyan toouse parancssori felület parancsai tooaccomplish Azure feladatok:

* [Hello Azure parancssori felület telepítése][xplat-cli]
* [Központi telepítése és virtuális gépek kezelése az Azure Resource Manager-sablonok és hello Azure parancssori felület használatával][virtual-machines-linux-cli-deploy-templates]
* [Hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager használatára][xplat-cli-azure-resource-manager]

Olvassa el is fejezet [Linux virtuális gépek Azure CLI] [ deployment-guide-4.5.2] a hello [telepítési útmutató] [ planning-guide] a hogyan toouse Azure CLI toodeploy hello Azure Az SAP monitorozási bővítményt.

## <a name="different-ways-toodeploy-vms-for-sap-in-azure"></a>Az SAP, az Azure virtuális gépek különböző módokon toodeploy
Ez a fejezet megtanulhatja hello különböző módokon toodeploy egy Azure-ban. Ez a fejezet további előkészítést eljárásokat, valamint a virtuális merevlemezek és az Azure virtuális gépek kezelésének ismertetnek.

### <a name="deployment-of-vms-for-sap"></a>Az SAP virtuális gépek telepítése
Microsoft Azure toodeploy virtuális gépek több lehetőséget kínál, és a kapcsolódó lemezek. Ezért ez a beállítás nagyon fontos toounderstand hello különbségek mivel hello virtuális gépek előkészített eltérőek lehetnek attól függően, hogy az üzembe helyezési módszer hello. Most elindítjuk általában egy pillantást a következő forgatókönyvek hello:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>A virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemez
A helyszíni tooAzure SAP rendszert toomove tervezi. Ez virtuális Merevlemezt, amely tartalmazza az operációs rendszer hello hello feltöltésével végezhető, SAP bináris fájljait és adatbázis-kezelő bináris fájljait és hello adatokat tartalmazó virtuális merevlemezeket hello hello és hello DBMS tooAzure naplófájlok. Ezzel szemben túl[az alábbi #2. forgatókönyv][planning-guide-5.1.2], mindig hello állomásnév, SAP SID és SAP felhasználói fiókok a hello Azure virtuális Gépen, azok hello a helyszíni környezetben volt konfigurálva. Ezért normalizálása üdvözlő kép, nem szükséges. Tekintse meg a fejezetek [előkészítése a virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemezzel] [ planning-guide-5.2.1] a jelen dokumentum előkészítő lépések helyszíni és az nem általánosított virtuális gépek és VHD feltöltése tooAzure. Olvassa el a fejezet [forgatókönyv 3: a helyszíni SAP Azure nem általánosított virtuális Merevlemezt használ a virtuális gépek áthelyezése] [ deployment-guide-3.4] a hello [telepítési útmutató] [ deployment-guide]egy lemezképet, az Azure-ban történő telepítésének részletes leírást.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Virtuális gép és egy ügyfél konkrét lemezkép központi telepítése
Miatt az operációs rendszer vagy az adatbázis-kezelő verziója toospecific javítás követelményeinek megadott hello képek hello Azure piactér előfordulhat, hogy nem felelnek meg az igényeinek. Ezért szükség lehet a saját "private" az operációs rendszer vagy adatbázis-kezelő Virtuálisgép-lemezkép többször később is telepíthető, amely virtuális gépek toocreate. tooprepare duplikálva lettek-e ilyen egy "private" lemezképet, a következő elemek hello figyelembe veendő toobe rendelkezik:


[comment]: <> (MSSedusch > Itt további részletek:)
[comment]: <> (MShermannd TODO első hivatkozás a klasszikus modellt tárgya. Nem található az Azure dokumentum-cikk)
[comment]: <> (MSSedusch >< https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>)
[comment]: <> (MSSedusch >< http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
- - -
> ![Windows][Logo_Windows] Windows
>
> beállítások (például a Windows biztonsági AZONOSÍTÓK és állomásnév) kell azért/általánosítva hello a Windows hello a helyszíni virtuális gép keresztül hello sysprep parancsot.
>
>
> ![Linux][Logo_Linux] Linux
>
> Kövesse az alábbi cikkeiben hello lépéseket [SUSE] [ virtual-machines-linux-create-upload-vhd-suse] vagy [Red Hat] [ virtual-machines-linux-redhat-create-upload-vhd] tooprepare egy virtuális merevlemez toobe tooAzure feltöltve.
>
>

- - -
Ha már telepítette a helyszíni virtuális gépre (különösen a 2-réteg rendszerek) SAP tartalom, módosíthatja hello SAP rendszerbeállítások hello telepítése Azure virtuális gép hello hello példány keresztül SAP Szoftverellátáshoz hello által támogatott eljárás átnevezése után Manager (SAP Megjegyzés [1619720]). Lásd: fejezetek [előkészítése virtuális gép és egy ügyfél konkrét kép telepítése az SAP] [ planning-guide-5.2.2] és [helyszíni tooAzure virtuális merevlemez feltöltése] [ planning-guide-5.3.2]a jelen dokumentum előkészítő lépések helyszíni és az egy általánosított virtuális gép tooAzure feltöltése. Olvassa el a fejezet [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP] [ deployment-guide-3.3] a hello [telepítési útmutató] [ deployment-guide] a lépésenkénti útmutató ilyen lemezkép az Azure-ban telepítése.

#### <a name="deploying-a-vm-out-of-hello-azure-marketplace"></a>A virtuális gépek kívül hello Azure piactér telepítése
Azt szeretné, hogy a Microsoft toouse, vagy a 3. fél megadott hello Azure piactér toodeploy a Virtuálisgép-lemezkép a virtuális Gépet. Miután telepítette a virtuális Gépet az Azure-ban, akkor hajtsa végre a hello azonos irányelvek és eszközök tooinstall hello SAP szoftver és/vagy az adatbázis-kezelő a virtuális Gépen belül, mint a helyszíni környezetben. Részletes leírás a telepítés, ellenőrizze a fejezet [1. forgatókönyv: központi telepítése egy virtuális gép kívül hello Azure piactérről az SAP] [ deployment-guide-3.2] hello a [telepítési útmutató] [ deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Az Azure SAP rendelkező virtuális gépek előkészítése
Az Azure virtuális gépek feltöltés előtt kell toomake meg arról, hogy hello virtuális gépek és VHD-k bizonyos követelmények teljesítéséhez. Használt hello telepítési módszertől függően kisebb különbségek vannak.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>A virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemezzel előkészítése
Gyakori telepítési módja, hogy egy meglévő virtuális Gépre, amely a helyszíni tooAzure SAP rendszerrel toomove. Hogy a virtuális gép és a virtuális gép csak kell futtatni az Azure használatával hello SAP rendszer hello hello azonos állomásnév, és valószínűleg hello azonos SAP SID azonosítója. Ebben az esetben hello vendég operációs rendszer a virtuális gép kell nem általánosítva több-telepítéshez. Ha hello a helyszíni hálózat kapott kiterjeszthetőek az Azure (című [létesítmények közötti - központi telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény abból a teljes mértékben történő integrálását hello a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban), még akkor is, majd hello azonos tartományi fiókokat használhat hello VM belül azok helyszíni előtt használt.

A saját Azure VM lemez előkészítésekor követelmények a következők:

* Eredetileg hello VHD-t tartalmazó hello operációs rendszer maximális mérete 127GB csak rendelkezhetnek. Ez a korlátozás kapott szüntetni 2015. márciusi hello végét. Most hello hello operációs rendszert tartalmazó virtuális merevlemez lehet mérete too1TB be minden más Azure-tárterület tárolt VHD-t is.

[comment]: <> (MShermannd TODO toocheck is, ha a parancssori felület is alakítja át a toostatic)
* A rögzített VHD-formátumban hello toobe szükséges. Dinamikus VHD vagy VHDx formátumú virtuális merevlemezeket még nem támogatottak az Azure-on. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Virtuális merevlemezek, amelyek csatlakoztatott toohello virtuális gép, és az Azure toohello VM kell toobe egy rögzített méretű VHD-formátumban, valamint újra legyen csatlakoztatva. hello azonos hello operációsrendszer-lemez mérete érvényes toodata lemezeket is. Virtuális merevlemezek mérete 1 TB-os lehet. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Adjon hozzá egy másik helyi fiók, amely használatával a Microsoft támogatási vagy amely rendelhető környezetben a szolgáltatások és alkalmazások toorun amíg hello virtuális gép telepítve van és jobban megfelelő felhasználók rendszergazdai jogosultságokkal is használható.
* Hello eset egy kizárólag felhőalapú környezet-forgatókönyv használatával (című [csak felhőalapú - hello a függőségek nélkül az Azure virtuális gépek telepítéséhez a helyi felhasználói hálózati] [ planning-guide-2.1] ezen dokumentálja) együtt a telepítési módszerrel, tartományi fiókok nem működik, ha hello Azure lemez a rendszer az Azure-ban. Ez különösen igaz, a fiókok, amelyek használt toorun szolgáltatásokat, mint a hello DBMS vagy SAP-alkalmazást. Ezért kell tooreplace ilyen tartományi fiókokat a virtuális gép helyi fiókokkal, és törölje a hello helyszíni tartományi fiókokat a virtuális gép hello. A helyszíni tartományi felhasználók tartása hello VM-lemezképben darabolása nem okoz problémát telepítésekor hello VM a hello létesítmények közötti forgatókönyv fejezetben leírtak [létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény, hogy a teljesen integrálva hello a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban.
* Ha tartományi fiókokat, az adatbázis-kezelő bejelentkezések használt, vagy felhasználó hello rendszer a helyi és virtuális gépek futtatásakor kizárólag felhőalapú forgatókönyvekben telepített toobe tudhatja, hello tartományi felhasználók kell toobe törölve. Arról, hogy helyi rendszergazda hello plusz egy másik virtuális gép helyi felhasználói meg van adva a bejelentkezési/felhasználó hello DBMS rendszergazdaként történő toomake van szüksége.
* Hozzáadhat más helyi fiókok azokat az adott környezet hello szükség lehet.

- - -
> ![Windows][Logo_Windows] Windows
>
> Ebben a forgatókönyvben nem általánosítása (sysprep) a virtuális gép hello szükséges tooupload és hello Azure VM telepítése.
> Győződjön meg róla, hogy nincs D:\ meghajtót használja Set lemez automount csatlakoztatott lemezek fejezetben leírtak [beállítás automount a csatlakoztatott lemezekkel] [ planning-guide-5.5.3] ebben a dokumentumban.
>
> ![Linux][Logo_Linux] Linux
>
> Ebben a forgatókönyvben nem általánosítása (waagent-deprovision) hello VM szükséges tooupload és hello Azure VM telepítése.
> Győződjön meg arról, hogy az összes lemez csatlakoztatva van-e keresztül uuid és, hogy nem használja a/mnt és az erőforrások. Győződjön meg arról, hogy hello rendszertöltő bejegyzés is tükrözi hello uuid-alapú csatlakoztatási hello operációsrendszer-lemez.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Virtuális gép és egy ügyfél konkrét kép telepítése az SAP-előkészítése
Egy általánosított OS tartalmazó VHD-fájlokat is tárolódnak az Azure Storage-fiókok a tárolók. Vezérlőpultjának hello virtuális Merevlemezt a virtuális merevlemez a központi telepítési sablon fájlokban forrásaként, fejezetben leírtak szerint telepíthet egy új virtuális gép lemezképből ilyen virtuális merevlemez [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP] [ deployment-guide-3.3] a hello [Telepítési útmutató][deployment-guide].

Ha a saját Azure Virtuálisgép-lemezkép előkészítése követelményei vannak:

* Eredetileg hello VHD-t tartalmazó hello operációs rendszer maximális mérete 127GB csak rendelkezhetnek. Ez a korlátozás kapott szüntetni 2015. márciusi hello végét. Most hello hello operációs rendszert tartalmazó virtuális merevlemez lehet mérete too1TB be minden más Azure-tárterület tárolt VHD-t is.

[comment]: <> (MShermannd TODO toocheck is, ha a parancssori felület is alakítja át a toostatic)
* A rögzített VHD-formátumban hello toobe szükséges. Dinamikus VHD vagy VHDx formátumú virtuális merevlemezeket még nem támogatottak az Azure-on. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Virtuális merevlemezek, amelyek csatlakoztatott toohello virtuális gép, és az Azure toohello VM kell toobe egy rögzített méretű VHD-formátumban, valamint újra legyen csatlakoztatva. hello azonos hello operációsrendszer-lemez mérete érvényes toodata lemezeket is. Virtuális merevlemezek mérete 1 TB-os lehet. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Mivel az összes regisztrált felhasználók a virtuális gép hello nem szerepel egy kizárólag felhőalapú forgatókönyv Tartományfelhasználók hello (című [csak felhőalapú - hello a függőségek nélkül az Azure virtuális gépek telepítéséhez a helyi felhasználói hálózati] [ planning-guide-2.1] a jelen dokumentum), a szolgáltatások ilyen tartomány fiókok nem működik, ha hello lemezképét a rendszer az Azure-ban. Ez különösen igaz, a fiókok, amelyek használt toorun szolgáltatások, például az adatbázis-kezelő vagy SAP-alkalmazást. Ezért kell tooreplace ilyen tartományi fiókokat a virtuális gép helyi fiókokkal, és törölje a hello helyszíni tartományi fiókokat a virtuális gép hello. Megőrzi a helyszíni tartományi felhasználók található hello Virtuálisgép-lemezkép nem lehet hibát amikor a virtuális gép telepítve van az hello hello létesítmények közötti forgatókönyv fejezetben leírtak [létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény abból a teljes mértékben történő integrálását hello a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban.
* Vegyen fel egy másik helyi fiók probléma vizsgálatokat, vagy amelyek rendelhető környezetben a szolgáltatások és alkalmazások toorun amíg hello virtuális gép telepítve van a Microsoft támogatási szolgálatához, és jobban megfelelő felhasználók által is használt rendszergazdai jogosultságokkal is használható.
* A csak felhőalapú telepítések és, adatbázis-kezelő bejelentkezések használt tartományi fiókok vagy felhasználók futtatásakor a rendszer a helyi hello hello tartományi felhasználók törölni kell. Arról, hogy helyi rendszergazda hello plusz egy másik virtuális gép helyi felhasználói meg van adva egy hello DBMS rendszergazdai szerepkörrel bejelentkező/felhasználó toomake van szüksége.
* Hozzáadhat más helyi fiókok azokat az adott környezet hello szükség lehet.
* Hello rendszerkép tartalmazza az SAP NetWeaver egy telepítése, és hello állomásnév hello eredeti neve az Azure-telepítés hello hello ponton átnevezése valószínű, esetén ajánlott toocopy hello legújabb verziókat hello SAP szoftver kiépítési Manager DVD be hello sablont. Ez lehetővé teszi az tooeasily használata hello SAP megadott Átnevezés funkció tooadapt megváltozott hello állomásnév és/vagy módosítása hello SID-je hello hello SAP rendszerében telepített Virtuálisgép-lemezkép, amint egy új példányt elindult.

- - -
> ![Windows][Logo_Windows] Windows
>
> Győződjön meg róla, hogy nincs D:\ meghajtót használja Set lemez automount csatlakoztatott lemezek fejezetben leírtak [beállítás automount a csatlakoztatott lemezekkel] [ planning-guide-5.5.3] ebben a dokumentumban.
>
> ![Linux][Logo_Linux] Linux
>
> Győződjön meg arról, hogy az összes lemez csatlakoztatva van-e keresztül uuid és, hogy nem használja a/mnt és az erőforrások. Az operációs rendszer hello lemez ellenőrizze, hogy hello rendszertöltő bejegyzés is tükrözi az uuid-alapú csatlakoztatási hello.
>
>

- - -
* SAP grafikus felhasználói felület (a felügyeleti és beállítására céljából) a sablon előre telepítve.
* Egyéb szoftverek szükséges toorun hello virtuális gépek sikeresen a létesítmények közötti forgatókönyv telepíthető, amíg ez a szoftver használható hello nevezze át a virtuális gép hello.

Ha hello virtuális gép készen áll a testreszabásra elég általános, és végül független a fiókok/felhasználók számára nem érhető el a hello toobe céloz Azure üzembe helyezési forgatókönyv, hello utolsó előkészítő lépésében ilyen lemezkép normalizálása végzik.

##### <a name="generalizing-a-vm"></a>A virtuális gépek normalizálása
- - -
[comment]: <> (MShermannd TODO rendelkezik toofind jobb cikkekhez / d kapcsolatos normalizálása hello ARM a virtuális gépek)
> ![Windows][Logo_Windows] Windows
>
> hello utolsó lépése a virtuális gép tooa rendszergazdaként toolog. Nyisson meg egy Windows parancsablakot "rendszergazdaként. Nyissa meg too...\windows\system32\sysprep, majd hajtsa végre a sysprep.exe.
> Egy kis ablakban jelenik meg. Fontos toocheck hello "Generalize" lehetőség (hello alapértelmezett érték a nem ellenőrzött), és módosítsa a hello kikapcsolás beállítás az alapértelmezett "Újraindítás" too'Shutdown ". Ez az eljárás azt feltételezi, hogy hello sysprep folyamat végrehajtott történő helyszíni hello a virtuális gépek vendég operációs rendszer.
> Ha tooperform hello eljárás Azure-ban már futó virtuális gépen, lépésekkel hello ismertetett [Ez a cikk][virtual-machines-windows-capture-image].
>
> ![Linux][Logo_Linux] Linux
>
> [Hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként][virtual-machines-linux-capture-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-tooazure"></a>A helyszíni tooAzure közötti átviteléhez a virtuális gépek és VHD-k
Mivel a Virtuálisgép-lemezképet és lemezt tooAzure feltöltése nem lehetséges hello Azure portálon keresztül, kell toouse Azure PowerShell-parancsmagjaival vagy a parancssori felület. Egy másik lehetőség, hello eszköz "AzCopy" hello használatát. hello eszköz másolható VHD-k a helyszíni és az Azure közötti (kétirányú). Azt is másolhatja Azure-régiók közötti virtuális merevlemezeket. További részleteket [ebben a dokumentációban] [ storage-use-azcopy] a letöltésről és az AzCopy használata.

Harmadik alternatív lenne toouse különböző harmadik féltől származó készült grafikus eszközöket. Azonban győződjön meg arról, hogy ezek az eszközök támogatják a Lapblobokat Azure. Az az Azure oldalakra vonatkozó Blob toouse kell célokból tárolni (hello különbségek a dokumentum ismerteti: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). Azure által biztosított hello eszközöket is nagyon hatékonyak a tömörítés hello virtuális gépek és VHD, amelyek feltöltött toobe kell. Ez azért fontos, mert ez a hatékonyság a tömörítés csökkenti a hello ideje (ami függ ennek ellenére a hello feltöltés hivatkozás toohello hello helyszíni létesítmény és hello Azure-telepítés régió internet megcélzott). A valós azt feltételezi, hogy a virtuális gép vagy a virtuális merevlemez feltöltése adatokból Európai hely toohello USA-alapú Azure adatközpontjaiban fog tovább tart, mint feltöltése hello azonos virtuális gépek/VHD-k toohello Európai Azure adatközpontjaiban.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>A helyszíni tooAzure virtuális merevlemez feltöltése
egy meglévő virtuális vagy virtuális merevlemez hello helyszíni hálózati egy virtuális Gépet, vagy VHD-t kell toomeet hello követelmények fejezetben felsorolt tooupload [előkészítése a virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemezzel] [ planning-guide-5.2.1] ebben a dokumentumban.

Egy virtuális Gépet nem kell toobe általánosítva van, és hello állapotát és után hello leállítás helyszíni ügyféloldali rendelkezik alakzat fel kell tölteni. hello esetében is igaz további VHD, amelyek nem tartalmaznak minden operációs rendszer.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Virtuális merevlemez feltöltése, majd elérhetővé tétele az Azure-lemez
Ebben az esetben azt szeretné, hogy egy virtuális Merevlemezt, vagy egy, az operációs rendszer anélkül tooupload és tooa VM adatok lemezként csatlakoztatni vagy használni az operációsrendszer-lemez. Ez az folyamat

**PowerShell**

* Bejelentkezési tooyour előfizetés *Login-AzureRmAccount*
* Állítsa be a hello előfizetést, a környezetben a *Set-AzureRmContext* és előfizetés-azonosító paramétert vagy SubscriptionName - <https://msdn.microsoft.com/library/mt619263.aspx>
* Töltse fel a virtuális merevlemez hello *Add-AzureRmVhd* tooan Azure Storage-fiók – lásd: <https://msdn.microsoft.com/library/mt603554.aspx>
* Set hello az operációs rendszer lemezén található egy új virtuális gép konfigurációs toohello VHD-t a *Set-AzureRmVMOSDisk* -lásd <https://msdn.microsoft.com/library/mt603746.aspx>
* Hozzon létre egy új virtuális Gépet a Virtuálisgép-konfiguráció hello rendelkező *New-AzureRmVM* -lásd <https://msdn.microsoft.com/library/mt603754.aspx>
* Adatok lemez tooa hozzáadása az új virtuális gép *Add-AzureRmVMDataDisk* -lásd <https://msdn.microsoft.com/library/mt603673.aspx>

**Azure CLI**

* Kapcsoló tooAzure erőforrás-kezelő módban *azure config mód arm*
* Bejelentkezési tooyour előfizetés *azure bejelentkezési*
* Válassza ki az előfizetés *azure-fiók beállítása`<subscription name or id`>*
* Töltse fel a virtuális merevlemez hello *az azure storage-blob feltöltése* -lásd: [az Azure Storage Azure CLI használata hello][storage-azure-cli]
* Hozzon létre egy új virtuális Gépet hello VHD feltöltése az operációs rendszer lemezeként megadása *azure virtuális gép létrehozása* és paraméter -d
* Adatok lemez tooa hozzáadása az új virtuális gép *méretű lemez csatolása és új*

**Sablon**

* Hello Powershell vagy Azure CLI VHD feltöltése
* Hello VM telepítsen egy JSON-sablon hivatkozik hello VHD, ahogy az az [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>A Virtuálisgép-lemezkép központi telepítése
egy meglévő virtuális vagy virtuális merevlemez hello helyszíni hálózatot a rendelés toouse az Azure virtuális gép képként egy virtuális Gépet vagy virtuális Merevlemezt kell toomeet hello követelményeknek fejezetben felsorolt tooupload [virtuális gép és egy ügyfél konkrét kép telepítése az SAP-előkészítése] [ planning-guide-5.2.2] ebben a dokumentumban.

* Használjon *sysprep* Windows vagy *waagent-deprovision* lévő Linux toogeneralize a VM - lásd: [hogyan toocapture egy Windows rendszerű virtuális gép hello erőforrás-kezelő telepítési modell] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] vagy [hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként][virtual-machines-linux-capture-image-capture]
* Bejelentkezési tooyour előfizetés *Login-AzureRmAccount*
* Állítsa be a hello előfizetést, a környezetben a *Set-AzureRmContext* és előfizetés-azonosító paramétert vagy SubscriptionName - <https://msdn.microsoft.com/library/mt619263.aspx>
* Töltse fel a virtuális merevlemez hello *Add-AzureRmVhd* tooan Azure Storage-fiók – lásd: <https://msdn.microsoft.com/library/mt603554.aspx>
* Set hello az operációs rendszer lemezén található egy új virtuális gép konfigurációs toohello VHD-t a *Set-AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -lásd <https://msdn.microsoft.com/library/mt603746.aspx>
* Hozzon létre egy új virtuális Gépet a Virtuálisgép-konfiguráció hello rendelkező *New-AzureRmVM* -lásd <https://msdn.microsoft.com/library/mt603754.aspx>

**Azure CLI**

* Használjon *sysprep* Windows vagy *waagent-deprovision* lévő Linux toogeneralize a VM - lásd: [hogyan toocapture egy Windows rendszerű virtuális gép hello erőforrás-kezelő telepítési modell] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] vagy [hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként] [ virtual-machines-linux-capture-image-capture] Linux
* Kapcsoló tooAzure erőforrás-kezelő módban *azure config mód arm*
* Bejelentkezési tooyour előfizetés *azure bejelentkezési*
* Válassza ki az előfizetés *azure-fiók beállítása`<subscription name or id`>*
* Töltse fel a virtuális merevlemez hello *az azure storage-blob feltöltése* -lásd: [az Azure Storage Azure CLI használata hello][storage-azure-cli]
* Hozzon létre egy új virtuális Gépet hello VHD feltöltése az operációs rendszer lemezeként megadása *azure virtuális gép létrehozása* és paraméter -K

**Sablon**

* Használjon *sysprep* Windows vagy *waagent-deprovision* lévő Linux toogeneralize a VM - lásd: [hogyan toocapture egy Windows rendszerű virtuális gép hello erőforrás-kezelő telepítési modell] [ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] vagy [hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként] [ virtual-machines-linux-capture-image-capture] Linux
* Hello Powershell vagy Azure CLI VHD feltöltése
* Hello VM telepítsen egy JSON-sablon hivatkozik hello kép VHD, ahogy az az [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-tooon-premises"></a>Virtuális merevlemezek tooon helyszíni letöltése
Azure-infrastruktúra szolgáltatásként nincs egy egyirányú utcaneve csak a képes tooupload VHD-k és az SAP rendszerek alatt. SAP áthelyezheti az Azure-ból rendszerek vissza az hello helyszíni világon is.

Hello letöltési hello időszakban hello VHD-k nem lehet aktív. Akkor is, ha a virtuális merevlemezeket, amelyek csatlakoztatott tooVMs letöltését hello VM toobe leállítási van szüksége. Ha csak egy új rendszer telepítéséhez használt tooset majd kellene lenniük a helyszíni és elfogadható, hogy a hello hello idő alatt letöltése és telepítése új hello hello rendszer, amely a rendszer az Azure-ban hello továbbra is lehet működési tartalom toodownload hello adatbázis , nem sikerült egy hosszú állásidő elkerülheti, ha a tömörített adatbázis biztonsági mentést egy VHD-be, és csak az, hogy a virtuális merevlemez is letöltésük helyett az operációs rendszer alap virtuális gép hello letöltése.

#### <a name="powershell"></a>PowerShell
Miután hello SAP rendszer le van állítva és hello virtuális gép leáll, használhatja hello PowerShell parancsmag mentés-AzureRmVhd hello helyszíni cél toodownload hello VHD lemezek toohello helyszíni globális biztonsági másolatot. Ahhoz, toodo, telepíteni kell, amely hello virtuális Merevlemezt, amely az "Tároló rész" hello található URL-címe hello hello Azure Portal (kell toonavigate toohello Tárfiókot és hello tároló hello virtuális merevlemez létrehozásának helyét), és meg kell tooknow, ahol hello VHD-t kell lennie másolja.

Kihasználhatja a hello parancs majd meghatározásával egyszerűen hello paraméter SourceUri hello URL-címet hello VHD toodownload és hello LocalFilePath mint hello (beleértve a neve) VHD hello fizikai helyét. hello parancs nézhet:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

Hello mentés-AzureRmVhd parancsmag további részletekért ellenőrizze Itt <https://msdn.microsoft.com/library/mt622705.aspx>.

#### <a name="cli"></a>parancssori felület
Miután hello SAP rendszer le van állítva és hello virtuális gép leáll, használhatja hello Azure CLI parancs az azure storage blob letöltési hello helyszíni cél toodownload hello VHD lemezek toohello helyszíni globális biztonsági másolatot. Ahhoz, toodo, telepíteni kell, amely hello nevét és a virtuális Merevlemezt, amely a "Tároló rész" hello hello hello tároló hello Azure Portal (kell toonavigate toohello Tárfiókot és hello tároló hello virtuális merevlemez létrehozásának helyét), és meg kell tooknow ahol virtuális merevlemez hello kell átmásolni.

Kihasználhatja a hello parancs majd egyszerűen hello paraméterek blob és megadásával hello VHD toodownload és hello cél tároló, fizikai célhelye hello hello virtuális merevlemez (beleértve a nevét). hello parancs nézhet:

```
azure storage blob download --blob <name of hello VHD toodownload> --container <container of hello VHD toodownload> --account-name <storage account name of hello VHD toodownload> --account-key <storage account key> --destination <destination of hello VHD toodownload>
```

### <a name="transferring-vms-and-vhds-within-azure"></a>Átadó virtuális gépek és virtuális merevlemezek Azure-ban
#### <a name="copying-sap-systems-within-azure"></a>Az Azure SAP rendszerek másolása
Egy SAP rendszert vagy akár egy SAP alkalmazásréteg támogató dedikált adatbázis-kezelő kiszolgálóra lesz valószínűleg közé több VHD, amelyek vagy az operációs rendszer hello hello bináris fájljait tartalmazza, vagy adatokat hello és naplófájl hello SAP-adatbázis (oka) t. Hello Azure funkcióit átmásolni a virtuális merevlemezeket, sem hello Azure funkcióit mentése a virtuális merevlemezek toodisk rendelkezik egy szinkronizálási mechanizmus, amely pillanatkép több virtuális merevlemezzel szinkron módon történik. Ezért hello hello állapotának másolta, vagy mentett virtuális merevlemezeket, még akkor is, ha azok csatlakoztatva vannak elleni hello ugyanazon VM eltérő lenne. Ez azt jelenti, hogy hello konkrét esetben másik adatforráshoz és szereplő logfile(s) hello különböző virtuális merevlemezek, a hello végén adatbázis hello lenne inkonzisztens.

**Megkötését: Rendelés toocopy vagy mentse a VHD-k egy SAP rendszerkonfiguráció toostop hello SAP rendszerre van szüksége, valamint kell részét képező tooshut hello le központilag telepített virtuális gép. Csak akkor másolhatja, akkor vagy letöltési hello készlete virtuális merevlemezek tooeither hello SAP rendszer másolatának létrehozása az Azure vagy a helyszíni.**

Adatlemezek egy Azure Storage-fiókot a VHD-fájlokban tárolódnak, és közvetlenül tooa virtuális géphez csatolása vagy képként használni. Ebben az esetben hello VHD-t az másolt tooanother hely, beeing toohello virtuális géphez csatolni. az Azure-ban hello VHD-fájl teljes neve hello Azure belül egyedinek kell lennie. Amint azt korábban már említettük, a hello neve: milyen a következőhöz hasonló háromrészes név:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### <a name="powershell"></a>PowerShell
Ahogy az Azure PowerShell-parancsmagok toocopy virtuális merevlemez használható [Ez a cikk][storage-powershell-guide-full-copy-vhd].

##### <a name="cli"></a>parancssori felület
Is használhatja az Azure parancssori felület toocopy virtuális merevlemez, ahogy az [Ez a cikk][storage-azure-cli-copy-blobs]

##### <a name="azure-storage-tools"></a>Az Azure Storage-eszközök
* <http://azurestorageexplorer.Codeplex.com/releases/View/125870>

Nincsenek is professional kiadása Azure Tártallózók, amely itt található:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

hello a virtuális merevlemez magát egy tárfiókon belül példány egy folyamatot, amely csupán néhány másodpercet (hasonló tooSAN hardver pillanatképeinek Lusta és az írás) vesz igénybe. Miután hello VHD fájlról csatolhatók tooa virtuális gép vagy egy kép tooattach, másolja át a hello VHD toovirtual gépek használatát.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path toovhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of hello vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path tooimage vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### <a name="cli"></a>parancssori felület
```
azure config mode arm

# attach a vhd tooa vm
azure vm disk attach <resource group name> <vm name> <path toovhd>

# attach a copy of hello vhd tooa vm
# this scenario is currently not possible with Azure CLI. A workaround is toomanually copy hello vhd toohello destination.
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Azure Storage-fiókok között a lemezek másolása
Ez a feladat nem hajtható végre hello Azure portálon. Ise Azure PowerShell parancsmagok, az Azure parancssori felület vagy harmadik féltől származó tárolási böngésző is. PowerShell-parancsmagok hello, vagy a parancssori felület parancsait hozhat létre és kezelheti a bináris objektumok, például hello képességét tooasynchronously másolási BLOB Storage-fiókok és régiók belül hello Azure-előfizetés között.

##### <a name="powershell"></a>PowerShell
Másolás a VHD-k között előfizetések esetében is lehetséges. További információk [Ez a cikk][storage-powershell-guide-full-copy-vhd].

hello alapszintű hello PS parancsmag logika áramló így néz ki:

* Hozzon létre egy hello forrás tárfiókhoz a tárfiók környezetét *New-AzureStorageContext* -lásd <https://msdn.microsoft.com/library/dn806380.aspx>
* Hozzon létre egy hello cél tárfiókhoz a tárfiók környezetét *New-AzureStorageContext* -lásd <https://msdn.microsoft.com/library/dn806380.aspx>
* Indítsa el a hello másolása

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Az ismétlődő hello másolási hello állapotának ellenőrzése

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Hello új virtuális merevlemez tooa virtuális gép csatlakoztatása a fent leírt módon.

További példák: [Ez a cikk][storage-powershell-guide-full-copy-vhd]

##### <a name="cli"></a>parancssori felület
* Indítsa el a hello másolása

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Ellenőrizze a hello állapotát, ha hello az ismétlődő másolása

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Hello új virtuális merevlemez tooa virtuális gép csatlakoztatása a fent leírt módon.

További példák: [Ez a cikk][storage-azure-cli-copy-blobs]

### <a name="disk-handling"></a>Lemez kezelése
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>VM-/ VHD-szerkezet SAP központi telepítések
Ideális esetben hello hello struktúra, a virtuális gépek kezelésére, illetve hello tartozó virtuális merevlemezek nagyon egyszerű kell lennie. A helyszíni telepítésekre az ügyfelek szerkezetének kialakítása a kiszolgáló telepítése az sokféleképpen ki.

* Egy Alaplemez, amely tartalmazza az operációs rendszer hello és minden hello bináris fájljait és/vagy az adatbázis-kezelő SAP hello. 2015. márciusi, mert a VHD-nál korábbi korlátozások, korlátozott helyett too1TB fel lehet too127GB.
* Egy vagy több virtuális merevlemezzel, amely tartalmazza a hello DBMS naplófájl hello SAP adatbázis és adatbázis-kezelő ideiglenes tárterület hello hello naplófájlt (ha az adatbázis-kezelő hello támogatja ezt). Ha hello adatbázis napló IOPS követelményei magas, kell toostripe több virtuális merevlemezzel rendelés tooreach hello IOPS kötet szükséges.
* Virtuális merevlemezek tartalmazó egy vagy két adatbázis hello SAP-adatbázis és hello DBMS ideiglenes adatfájlok is (ha az adatbázis-kezelő hello támogatja ezt) száma.

![Az Azure infrastruktúra-szolgáltatási virtuális gép SAP referencia-konfiguráció][planning-guide-figure-1300]

[comment]: <> (MShermannd TODO Linux struktúra írják le.)

- - -
> ![Windows][Logo_Windows] Windows
>
> Sok ügyfél az imént látott konfigurációkat, például SAP- és adatbázis-kezelő bináris fájlok nincsenek telepítve a hello c:\ meghajtó, ahová hello az operációs rendszer telepítve van. Ennek számos oka volt, de azt vissza toohello legfelső szintű frissülő, általában hello meghajtók kicsi volt, és az operációs rendszer frissítései szükséges további terület éve 10 – 15. Mindkét feltétel nem vonatkoznak ezek a napok túl gyakran többé. Ma hello c:\ meghajtó nagy méretű lemezek vagy a virtuális gépek rendelhetők. Az egyszerű a struktúrában rendelés tookeep telepítések esetén is ajánlott toofollow a következő telepítési minta SAP NetWeaver rendszerekhez az Azure-ban
>
> hello Windows operációs rendszer lapozófájl szabad hello D: meghajtó (nem állandó lemez)
>
> ![Linux][Logo_Linux] Linux
>
> Helyezze hello Linux swapfile /mnt/mnt és az erőforrások a Linux leírtak [Ez a cikk][virtual-machines-linux-agent-user-guide]. hello lapozófájl konfigurálható a Linux-ügynök /etc/waagent.conf hello hello konfigurációs fájlban. Adja hozzá, vagy módosítsa a beállításokat a következő hello:
>
>

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

toorestart szüksége tooactivate hello módosításokat, a Linux-ügynök hello

```
sudo service waagent restart
```

Olvassa el az SAP Megjegyzés [1597355] kapcsolatban további részleteket a hello ajánlott a lapozófájl mérete

- - -
hello DBMS az adatfájlok és hello Azure a tároló típusa szerinti üzemelteti a virtuális merevlemezek tárolására használt virtuális merevlemezeket hello száma hello IOPS követelmények és hello késés szükséges kell meghatározni. Ismerteti a pontos kvóták [Ez a cikk][virtual-machines-sizes]

SAP telepítések keresztül hello utolsó 2 év élménye tanított velünk során néhány tapasztalatokat, amely mint összegezhető:

* IOPS forgalom toodifferent adatfájlok nincs mindig azonos óta a meglévő ügyfél rendszerek lehet, hogy rendelkezik másképp méretű adatok fájlok, a SAP adatbázis(ok) képviselő hello. Ennek eredményeképpen névszolgáltatások kimenő toobe jobban használatával több virtuális merevlemezek tooplace hello adatok fájlokat logikai egységek faragottnak ki azokat a RAID-konfigurációt. Nem voltak olyan helyzetekben, különösen a Azure standard szintű tárolást, ahol az IOPS-ráta találati hello DBMS tranzakciónapló szemben egyetlen virtuális hello kvóta. Prémium szintű Storage ilyen forgatókönyvek hello használata javasolt, vagy másik lehetőségként a Standard tárolási csak egy virtuális merevlemezt egy szoftverek RAID összesítése.

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
* Prémium szintű Storage jelentős jobb teljesítményt, különösen a kritikus tranzakciós napló írása láthatók. SAP forgatókönyvek, amelyek például a teljesítmény várható toodeliver éles esetén ajánlott, amelyek kihasználhatják a prémium szintű Azure Storage Virtuálisgép-sorozat toouse.

Ne feledje, hogy virtuális Merevlemezt, amely tartalmazza az operációs rendszer hello hello és, azt javasoljuk, hello SAP bináris fájljait és hello adatbázis (alap virtuális gép) is, már nem korlátozott too127GB. Most rendelkezhet mérete too1TB fel. Ez legyen elég lemezterület tookeep összes szükséges fájlt, így például SAP kötegelt feladatnaplóit hello.

A további vonatkozó javaslatokat és adatbázis-kezelő virtuális gépeken, kifejezetten a további részletekért tekintse meg a hello [adatbázis-kezelő telepítési útmutatója][dbms-guide]

#### <a name="disk-handling"></a>Lemez kezelése
A legtöbb esetben toocreate további lemezek rendelés toodeploy hello SAP-adatbázisban történő hello VM kell. A fejezet a VHD-k számára hello használata megtartásról [VM-/ VHD-szerkezet SAP központi telepítések] [ planning-guide-5.5.1] ebben a dokumentumban. hello Azure portál lehetővé teszi, hogy a tooattach és leválasztani a lemezeket, amennyiben egy alap virtuális Gépre van telepítve. hello lemezek csak csatlakoztatott vagy leválasztott amikor hello VM működik-e és fut, valamint ha le van állítva. Lemez csatlakoztatása, hello Azure Portal kínál tooattach üres lemez vagy egy meglévő lemez, amely ezen a ponton a időben nem csatlakoztatott tooanother virtuális gép.

**Megjegyzés:**: virtuális merevlemezek csak lehet csatolt tooone virtuális gép egy adott időpontban.

![Csatlakoztassa / lemezeken Azure standard szintű tárolóval leválasztása][planning-guide-figure-1400]

Új és üres virtuális merevlemez (amely a Tárfiókon, hello alap virtuális gép van hello létrehozott) vagy akkor tooselect egy meglévő VHD-t, amely korábban feltöltött, és most már csatlakoztatott toohello VM toocreate kíván-e szüksége toodecide.

**FONTOS**: akkor **nem** állomás gyorsítótárazását Azure standard szintű tárolóval toouse szeretné. Hello állomás gyorsítótár beállítása None hello alapértelmezett hagyja. Prémium szintű Azure Storage kell engedélyeznie, olvasási gyorsítótárazás Ha hello i/o-jellemző többnyire olvasható például tipikus i/o-forgalom adatbázis adatok fájlokra vonatkozóan. Adatbázis-tranzakciós naplófájl esetén gyorsítótárazása használata ajánlott.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Hogyan tooattach az adatok lemezre a hello Azure-portálon][virtual-machines-windows-attach-disk-portal]
>
> Lemezek vannak csatolva hozzá, ha van szüksége a toolog hello VM tooopen hello Windows lemezkezelő be. Ha automount nincs engedélyezve a fejezet ajánlott [beállítás automount a csatlakoztatott lemezekkel][planning-guide-5.5.3], hello újonnan csatolt kötet kell toobe online venni, illetve az inicializálása.
>
> ![Linux][Logo_Linux] Linux
>
> Lemezek vannak csatolva hozzá, ha a toolog igénylő hello virtuális gép, és hello lemezek inicializálása a [Ez a cikk][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Ha új lemez hello üres lemez, szükség van, valamint a tooformat hello lemezre. Formázásához, különösen a DBMS adatok és a naplófájlok hello hello az operációs rendszer nélküli telepítések esetében azonos javaslatok DBMS alkalmazni.

Említettek szerint fejezetben [Microsoft Azure virtuális gép koncepció hello][planning-guide-3.2], egy Azure Storage-fiók nem rendelkezik végtelen erőforrásokkal tekintetében i/o-kötet, iops-érték és az adatok kötet. Adatbázis-kezelő virtuális gépek általában leginkább által érintett. Ajánlott toouse egy külön Tárfiókot az egyes virtuális gépek előfordulhat, ha i/o kötet néhány nagy virtuális gépek toodeploy rendelés toostay belül hello hello Azure Storage-fiók kötet található. Ellenkező esetben szüksége toosee hogyan anélkül, hogy elérte-e minden egyetlen Tárfiók hello legfeljebb különböző Storage-fiókok közötti virtuális gépeken is el tudnak osztani. További részletekért hello esik szó [adatbázis-kezelő telepítési útmutatója][dbms-guide]. Ezek a korlátozások is tiszta SAP alkalmazás kiszolgáló virtuális gépek vagy más virtuális gépek, amelyek idővel előfordulhat, hogy további virtuális merevlemezek figyelembe kell venni.

Egy másik témakör, amely a Storage-fiókok szükséges, hogy a Tárfiók a virtuális merevlemezek hello georeplikált kap. A georeplikáció engedélyezve van-e hello Tárfiók szint és nem a virtuális gép szintjének hello. Ha a georeplikáció engedélyezve van, a Tárfiók szeretné replikálni azokat egy másik Azure-adatközpont belül hello belül hello VHD-k hello ugyanabban a régióban. Mielőtt eldönti, ez, gondolja hello korlátozása a következő:

Azure georeplikáció helyileg működik-e az egyes virtuális merevlemez virtuális gépen, és nem replikálja hello IOs időrendi sorrendben egy virtuális gép több virtuális merevlemezzel között. Ezért hello VHD-t, hogy jelöli hello alap virtuális gép, valamint minden további csatlakoztatott virtuális merevlemezek toohello VM egymástól független replikálódnak. Ez azt jelenti, hogy különböző virtuális merevlemezek hello hello változásai között nincs szinkronizálás van. hello tényt, hogy hello IOs replikálódnak függetlenül hello megadott sorrendben jelennek meg azok írt azt jelenti, hogy a georeplikáció nem a hozzájuk tartozó adatbázisok elosztva a több virtuális merevlemezzel rendelkező adatbázis-kiszolgálók számára. Továbbá toohello DBMS, az is előfordulhat, más alkalmazások, ahol írási figyelmen kívül hagyott folyamatok kezelhetők a különböző virtuális merevlemezek és a módosítások sorrendje fontos tookeep hello adatok. Ha ez a követelmény, az Azure-ban a georeplikáció nem engedélyezni kell. E szükséges, vagy szeretné georeplikáció állítja be a virtuális gépek, de nem egy másik készlet függ, már rendezheti az virtuális gépek és azok kapcsolódó virtuális merevlemezek be különböző Storage-fiókok, amelyek rendelkeznek a georeplikáció engedélyezve vagy letiltva.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>A csatlakoztatott lemezek automount beállítása
- - -
> ![Windows][Logo_Windows] Windows
>
> Virtuális gépek, amelyek a saját lemezképek vagy lemezek jönnek létre, a szükséges toocheck, és esetleg hello automount paraméter. A paraméter lehetővé teszi a virtuális gép hello után újraindítás vagy az Azure toomount hello újratelepítés csatolt/csatlakoztatott meghajtók újra automatikusan.
> hello paraméter hello Azure piactér a Microsoft által biztosított hello képek van beállítva.
>
> Rendelés tooset hello automount ellenőrizze hello parancssori végrehajtható diskpart.exe itt hello dokumentációját:
>
> * [A DiskPart parancssori kapcsolók](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> hello Windows parancssori ablakot rendszergazdaként kell megnyitni.
>
> Lemezek vannak csatolva hozzá, ha van szüksége a toolog hello VM tooopen hello Windows lemezkezelő be. Ha automount nincs engedélyezve a fejezet ajánlott [beállítás automount a csatlakoztatott lemezekkel][planning-guide-5.5.3], hello újonnan csatolt kötet > toobe online venni és inicializálva kell.
>
> ![Linux][Logo_Linux] Linux
>
> Szüksége van a tooinitialize újonnan csatolt üres lemez a [Ez a cikk][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Meg kell tooadd új lemezek toohello /etc/fstab is.
>
>

- - -
### <a name="final-deployment"></a>Végső telepítés
A hello végső telepítés és a pontos lépések, különösen az tanúsítványinformációit toohello telepítésének SAP kiterjesztett figyelést, tekintse meg az toohello [telepítési útmutató][deployment-guide].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Azure virtuális gépeken futó SAP rendszerek elérése
Csak felhőalapú forgatókönyvek esetén szükség lehet tooconnect toothose SAP rendszerek között hello SAP grafikus felhasználói felülettel nyilvános internethez. Ezekben az esetekben hello következő eljárásokat kell alkalmazni toobe.

A következőket ismertetjük hello dokumentum hello más fő forgatókönyv, létesítmények közötti telepítések esetén, amelyek a pont-pont (VPN-alagút) vagy Azure ExpressRoute-kapcsolatot hello a helyszíni és az Azure rendszerek közötti összekötő tooSAP rendszerek.

### <a name="remote-access-toosap-systems"></a>Távoli hozzáférés tooSAP rendszerek
Az Azure Resource Manager nincsenek alapértelmezett végpontok többé például hello volt a klasszikus modellt. Egy Azure ARM VM összes portjai nyitva, amíg:

1. Nincs hálózati biztonsági csoport hello alhálózati vagy hello hálózati adapter van definiálva. Hálózati forgalom tooAzure virtuális gépek úgynevezett "hálózati biztonsági csoportok" via védve legyenek. További információ: [Mi az a hálózati biztonsági csoport (NSG)?][virtual-networks-nsg]
2. Nincsenek Azure Load Balancer hello hálózati adapter van definiálva.   

Lásd: hello architektúra különbség klasszikus modellt és ARM leírtak [Ez a cikk][virtual-machines-azure-resource-manager-architecture].

#### <a name="configuration-of-hello-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Hello csak felhőalapú forgatókönyvhöz SAP rendszer és az SAP grafikus felhasználói Felülettel kapcsolat konfigurációja
Tekintse meg az ebben a cikkben, amely részletek toothis témakör ismerteti: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx>

#### <a name="changing-firewall-settings-within-vm"></a>Virtuális Gépen belül a tűzfal beállításainak módosítása
Előfordulhat, hogy a virtuális gépek tooallow szükséges tooconfigure hello tűzfal bejövő forgalom tooyour SAP rendszer.

- - -
> ![Windows][Logo_Windows] Windows
>
> Hello egy Azure telepített virtuális Gépen belül a Windows tűzfal alapértelmezés szerint be van kapcsolva. Most kell tooallow hello SAP Port toobe megnyitni, ellenkező esetben hello SAP grafikus felhasználói felület nem lesz képes tooconnect.
> toodo ezt:
>
> * Vezérlőpult\rendszer és biztonság\windows tűzfal too'Advanced beállításainak megnyitásához.
> * Most kattintson a jobb gombbal a bejövő szabályok és a kiválasztott új-szabály.
> * Hello a következő varázsló döntött, hogy toocreate egy új "Port" szabály.
> * Hello hello varázsló a következő lépésben hagyja hello beállítást TCP, és írja be a hello portszámát írja tooopen. Mivel az SAP-példány azonosítója 00, azt 3200 vett igénybe. Ha a példány egy másik példány a számot, korábban hello példányszámának alapján meghatározott hello portot kell megnyitni.
> * Hello következő része hello varázsló kell tooleave hello elem "Engedélyezése kapcsolat" a be van jelölve.
> * A következő lépésben hello hello varázsló kell toodefine e szabály hello tartományi, privát és nyilvános hálózat. Állítsa be, ha szükséges tooyour van szüksége. Azonban csatlakozó SAP grafikus felhasználói Felülettel rendelkező hello kívül hello nyilvános hálózaton keresztül, meg kell toohave hello szabályt toohello nyilvános hálózat.
> * Hello utolsó lépésként hello varázsló toogive hello kell szabály nevét, és mentse a hello szabály billentyűkombináció lenyomásával a "Befejezés"
>
> hello szabály azonnal érvénybe lép.
>
> ![Port szabályát leíró definíció beolvasása][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> hello Linux képek hello Azure piactér hello iptables tűzfal nem engedélyezi alapértelmezés szerint, és hello kapcsolat tooyour SAP rendszer kell működnie. Ha iptables vagy más tűzfal engedélyezve, tekintse meg a iptables toohello dokumentációját, vagy használt hello tűzfal tooallow bejövő TCP-forgalom túl port 32xx (ahol xx az SAP rendszeróra hello rendszer száma).
>
>

- - -
#### <a name="security-recommendations"></a>Biztonsági javaslatok
hello SAP grafikus felhasználói Felülettel csatlakozás azonnal tooany hello SAP-példányok (port 32xx) futnak, de először hello megnyitva port toohello SAP üzenet kiszolgáló folyamat (port 36xx) keresztül csatlakozik. A múltbeli hello nagyon ugyanazt a portot hello használták hello üzenet kiszolgáló hello belső kommunikációs toohello alkalmazás példányok. tooprevent helyszíni alkalmazáskiszolgálók véletlenül kommunikálni az Azure hello belső message kiszolgáló kommunikációs portok módosíthatók. Lehetőleg toochange hello belső kommunikációs hello SAP üzenet kiszolgáló és az alkalmazás példányok tooa másik portszámot a rendelkezik a helyszíni rendszerekre, például egy fejlesztési projekt tesztelési klónja lett klónozása rendszerek között stb. Ezt megteheti hello alapértelmezett profil paraméterrel:

> rdisp/msserv_internal
>
>

Ahogy: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm>

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>SAP-példányok csak felhőalapú telepítés fogalmak
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Az SAP NetWeaver bemutató/képzési forgatókönyv egyetlen VM
![Egyetlen VM SAP bemutató rendszereket futtató hello az azonos virtuális gép nevét, egymástól el vannak különítve az Azure Cloud Services csomag][planning-guide-figure-1700]

Ebben a forgatókönyvben (lásd a fejezet [csak felhőalapú] [ planning-guide-2.1] a jelen dokumentum) azt webkiszolgálókból egy tipikus képzési/bemutató rendszer forgatókönyv, ahol hello teljes képzési/bemutató forgatókönyvet magában foglal egy virtuális. Feltételezzük, hogy hello telepítési Virtuálisgép-lemezkép sablonok keresztül történik. Azt is feltételezzük, hogy ezek bemutató/oktatási anyag virtuális gépek kell toobe hello hello rendelkező virtuális gépek telepítik, hogy többszöröse azonos nevet.

hello feltételezi, létrehozott egy Virtuálisgép-lemezkép fejezet egyes szakaszokban ismertetett módon [előkészítése virtuális gépek Azure-beli SAP] [ planning-guide-5.2] ebben a dokumentumban.

az események tooimplement hello forgatókönyvek hello feladatütemezési így néz ki:

[comment]: <> (MShermannd TODO rendelkezik tooprovide ARM minta / leírása a json-sablon + problémával kapcsolatos ARM virtuális hálózaton belül egyedi virtuális gép neve)   
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

* Új virtuális hálózat létrehozása minden képzési/bemutató fekvő tooenable hello használatáról hello azonos állomásnévvel és IP-címek. hello virtuális hálózatot a hálózati biztonsági csoport, amely csak forgalom tooport 3389-es tooenable távoli asztal eléréséhez és 22-es portot az SSH védi.

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Hozzon létre egy új nyilvános IP-címet, amely használt tooaccess hello a virtuális gép hello internet

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Hozzon létre egy új hálózati illesztő hello virtuális géphez

```powershell
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip
```

* Hozzon létre egy virtuális gépet. Hello csak felhőalapú forgatókönyvben minden virtuális gép lesz hello ugyanazzal a névvel. hello SAP SID-je hello virtuális gépek példánya lesz SAP NetWeaver hello azonos is. Belül hello Azure-erőforráscsoportot, hello VM hello nevét kell toobe egyedi, de különböző Azure erőforráscsoportok futtatható virtuális gépek a hello ugyanazzal a névvel. alapértelmezett "Rendszergazda" fiók a Windows hello vagy Linux rendszeren a "Gyökér" érvénytelen. Ezért egy másik rendszergazda felhasználónevet szükséges toobe definiált és jelszóval. hello VM hello mérete definiált toobe kell.

```powershell
#####
# Create a new virtual machine with an official image from hello Azure Marketplace
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
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
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path tooVHD that contains hello OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path tooVHD that contains hello OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Lehetősége van további lemezek hozzáadása, és állítsa vissza a szükséges tartalom. Vegye figyelembe, hogy az összes blob nevének (URL-címek toohello blobok) Azure belül egyedieknek kell lenniük.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### <a name="cli"></a>parancssori felület
a következő példakód hello Linux rendszeren használható. A Windows, vagy használjon PowerShell fent leírt módon vagy igazítja hello példa toouse % rgName %$rgName helyett, majd hello környezeti változót hello Windows paranccsal *beállítása*.

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

* Új virtuális hálózat létrehozása minden képzési/bemutató fekvő tooenable hello használatáról hello azonos állomásnévvel és IP-címek. hello virtuális hálózatot a hálózati biztonsági csoport, amely csak forgalom tooport 3389-es tooenable távoli asztal eléréséhez és 22-es portot az SSH védi.

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Hozzon létre egy új nyilvános IP-címet, amely használt tooaccess hello a virtuális gép hello internet

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Hozzon létre egy új hálózati illesztő hello virtuális géphez

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet
```

* Hozzon létre egy virtuális gépet. Hello csak felhőalapú forgatókönyvben minden virtuális gép lesz hello ugyanazzal a névvel. hello SAP SID-je hello virtuális gépek példánya lesz SAP NetWeaver hello azonos is. Belül hello Azure-erőforráscsoportot, hello VM hello nevét kell toobe egyedi, de különböző Azure erőforráscsoportok futtatható virtuális gépek a hello ugyanazzal a névvel. alapértelmezett "Rendszergazda" fiók a Windows hello vagy Linux rendszeren a "Gyökér" érvénytelen. Ezért egy másik rendszergazda felhasználónevet szükséges toobe definiált és jelszóval. hello VM hello mérete definiált toobe kell.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path tooimage vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path tooimage vhd> --disable-boot-diagnostics
```

* Lehetősége van további lemezek hozzáadása, és állítsa vissza a szükséges tartalom. Vegye figyelembe, hogy az összes blob nevének (URL-címek toohello blobok) Azure belül egyedieknek kell lenniük.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### <a name="template"></a>Sablon
A github tárházából hello azure-gyors üzembe helyezés-sablonok hello minta sablonok is használhatja.

* [Egyszerű Linux virtuális gép](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Egyszerű Windows virtuális gép](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [A kép VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-toocommunicate-within-azure"></a>Virtuális gépek Azure-ban toocommunicate igénylő készletének megvalósítását
A csak felhőalapú forgatókönyv, a képzési és bemutató célokra jellemző forgatókönyv, ahol hello hello bemutató/képzési forgatókönyv képviselő szoftver nyúlik át több virtuális gép. hello különböző összetevői telepítve hello különböző virtuális gépek kell toocommunicate egymással. Ebben az esetben ebben a forgatókönyvben nem a helyi hálózati kommunikáció, vagy a létesítmények közötti forgatókönyv van szükség.

Ebben a forgatókönyvben az fejezetben ismertetett hello telepítési kiterjesztése [rendelkező SAP NetWeaver bemutató/képzési forgatókönyv egyetlen virtuális] [ planning-guide-7.1] ebben a dokumentumban. Ebben az esetben több virtuális gép megkapja tooan meglévő erőforráscsoportot. Hello a következő példa hello képzési fekvő egy SAP ASC/SCS virtuális Gépre, a virtuális gép fut egy adatbázis-kezelő és az SAP Application Server-példány VM áll.

Mielőtt Ez a forgatókönyv szerint gyakorolt hello forgatókönyvben előtt alapvető beállításokkal kapcsolatos toothink van szüksége.

#### <a name="resource-group-and-virtual-machine-naming"></a>Erőforráscsoport és a virtuális gép elnevezése
Az összes erőforráscsoport-nevek egyedinek kell lennie. A saját elnevezési sémát az erőforrások, például a fejlesztés `<rg-name`>-utótagot.

hello virtuális gép neve toobe egyedi hello erőforráscsoporton belül van.

#### <a name="setup-network-for-communication-between-hello-different-vms"></a>A hálózat beállítása közötti kommunikációhoz hello különböző virtuális gépek
![Egy Azure virtuális hálózaton belüli virtuális gépek halmaza][planning-guide-figure-1900]

ütközésének a klónok hello az azonos képzési/bemutató tájak tooprevent minden fekvő toocreate egy Azure virtuális hálózatra van szüksége. Azure DNS-név feloldása biztosítja, vagy beállíthatja, hogy a saját DNS-kiszolgáló Azure (nem Microsofttól további toobe) kívül. Ebben a forgatókönyvben azt a saját DNS konfigurálása nem. Az összes virtuális gép egy Azure virtuális hálózaton belüli kommunikáció segítségével hostnames engedélyezve lesz.

hello tooseparate képzési vagy bemutató tájak virtuális hálózatok és nem csak erőforrás csoportok lehet okok miatt:

* hello SAP fekvő igényeinek beállított saját AD OpenLDAP és hello tájak mindegyikének a tartománykiszolgálóhoz igények toobe része.  
* hello SAP fekvő beállítása összetevők rendelkezik rögzített IP-címekkel rendelkező, hogy szükség toowork.

További információt az Azure virtuális hálózatok, és hogyan toodefine őket itt található: [Ez a cikk][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP virtuális gépek telepítése a vállalati hálózati kapcsolat hibája (létesítmények közötti)
Egy SAP fekvő futtatja, és szeretné, hogy toodivide hello telepítése operációs rendszer nélküli csúcskategóriás DBMS kiszolgálók között, helyszíni virtualizált környezetekben alkalmazás rétegek és kisebb 2-réteg SAP rendszerek és az Azure infrastruktúra-szolgáltatási konfigurálva. hello alap feltételezése, SAP rendszerek egy SAP fekvő belül kell-e egymással, és számos egyéb szoftverösszetevők, hello vállalati, függetlenül a telepítési képernyőn az üzembe helyezett toocommunicate. Is kell a Kapcsolódás a SAP grafikus felhasználói felületének vagy egyéb felületek hello végfelhasználói hello telepítési űrlap által bevezetett nincs különbség. Csak akkor kell teljesíteni ezeket a feltételeket, amikor tudunk hello a helyszíni Active Directory/OpenLDAP, valamint a DNS-szolgáltatások kiterjesztett toohello Azure hely-a-site/több-site kapcsolatot vagy a magánhálózati kapcsolatok, például Azure ExpressRoute-rendszerekhez.

A sorrend tooget több Azure SAP hello megvalósítási részleteit a háttérben, javasoljuk, tooread fejezet [fogalmak a Cloud-Only telepítési SAP-példányok] [ planning-guide-7] ebben a dokumentumban, amelyből megtudhatja, Néhány hello alapjai hoz létre Azure, és hogyan ezeket kell használni az SAP-alkalmazások az Azure-ban.

### <a name="scenario-of-a-sap-landscape"></a>Egy SAP fekvő forgatókönyvet
hello létesítmények közötti forgatókönyv nagyjából jelentheti például az alábbi hello grafikus:

![A helyszíni és az Azure eszközök közötti pont-pont kapcsolat][planning-guide-figure-2100]

hello fenti forgatókönyv bemutatja egy olyan forgatókönyvet, ahol hello a helyszíni AD/OpenLDAP és DNS ki van bővítve tooAzure. Hello helyszíni oldalon egy bizonyos IP-címtartomány egy Azure-előfizetés van fenntartva. hello IP-címtartomány hello Azure oldalán az Azure Virtual Network tooan rendeli.

#### <a name="security-considerations"></a>Biztonsági szempontok
hello minimális vonatkozó követelmény akkor biztonságos kommunikációs protokollok hello használata, mint például a hozzáférés toohello Azure SSL/TLS a böngésző-hozzáférés vagy VPN-alapú kapcsolatok rendszer szolgáltatások. hello feltételezése, hogy a vállalatok nagyon másként kezeli-e hello VPN-kapcsolat a vállalati hálózat és az Azure között. Egyes vállalatok blankly sérülékennyé minden hello port. Néhány más vállalatokkal, érdemes toobe nagyon pontos portokat a szükséges tooopen stb.

A tipikus SAP az alábbi hello táblázatban kommunikációs portok vannak felsorolva. Alapvetően elegendő tooopen hello SAP átjáró port.

| Szolgáltatás | Port neve | Példa `<nn`> = 01 | Alapértelmezett tartomány (min, max) | Megjegyzés |
| --- | --- | --- | --- | --- |
| A kézbesítő |sapdp`<nn>` lásd: * |3201 |3200 – 3299 |SAP kézbesítő SAP grafikus felhasználói felület a Windows és a Java által használt |
| Üzenet kiszolgáló |sapms`<sid`> tekintse meg ** |3600 |szabad sapms`<anySID`> |SID SAP-rendszer-ID = |
| Átjáró |sapgw`<nn`> lásd: * |3301 |Ingyenes |SAP-átjáróhoz, CPIC és RFC-kommunikációhoz használt |
| SAP-útválasztó |sapdp99 |3299 |Ingyenes |Csak CI (központi példány) szolgáltatásnevek /etc/services tooan tetszőleges érték elvégezhető a telepítés után. |

*) nn = SAP-példányok száma

*) sid SAP-rendszer-ID =

További információk a különböző SAP termékek szükséges portok vagy szolgáltatásokat SAP termékek szerint itt található <http://scn.sap.com/docs/DOC-17124>.
Ez a dokumentum a hello VPN-eszköz adott SAP termékeket és forgatókönyvek szükséges portok dedikált képes tooopen kell lennie.

Egyéb biztonsági intézkedések, amikor a virtuális gépek telepítését ilyen esetben lehet toocreate egy [hálózati biztonsági csoport] [ virtual-networks-nsg] toodefine hozzáférési szabályok.

### <a name="dealing-with-different-virtual-machine-series"></a>A virtuális gép másik Adatsorozathoz foglalkozó
Elmúlt 12 hónap során hello Microsoft hozzáadott számos további Virtuálisgép-típusokon, amelyek eltérnek vagy Vcpu, memória számát, vagy fontosabb hardveren futó. Nem minden virtuális gépek támogatottak SAP (lásd a támogatott Virtuálisgép-típusokon SAP feljegyzés [1928533]). Néhány virtuális gépek futtasson másik gazdagépet hardver generációt. A gazdagép hardver generációja hello granularitási, egy Azure-Bővítőegysége első telepített. Azt jelenti, hogy olyan esetek is felmerülhetnek, ahol hello másik Virtuálisgép-méretek választotta nem futtatható a hello azonos Bővítőegysége. Rendelkezésre állási csoport hello képességét toospan Méretezőegységnek alapú különböző hardver van korlátozva.  Például Ha azt szeretné, hogy toorun hello DBMS A5-A11 virtuális gépeken, és hello SAP alkalmazásréteg G sorozatú virtuális gépeken, lehetővé válik a kényszerített toodeploy egyetlen SAP rendszer vagy más SAP rendszerek belül különböző rendelkezésre állási készletek.

#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Nyomtatás a helyi hálózati SAP példányból az Azure-ban
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>A létesítmények közötti forgatókönyv TCP/IP felett nyomtatás
Ugyanaz, mint a vállalati hálózathoz, feltéve, hogy a VPN-helyek alagút vagy ExpressRoute-kapcsolat létrejött rendelkező általános hello beállítása a helyszíni TCP/IP-alapú hálózati nyomtatók egy Azure virtuális gép.

- - -
> ![Windows][Logo_Windows] Windows
>
> toodo ezt:
>
> * Egyes hálózati nyomtatók származnak, így könnyen tooset a nyomtató telepítése egy Azure virtuális gép konfigurációs varázsló segítségével. Ha nincs varázsló szoftver a nyomtató hello "manual" tooset hello nyomtató telepítése nem módosul egy új TCP/IP nyomtatóportot toocreate volt terjesztve.
> * Nyissa meg a Vezérlőpult -> eszközök és nyomtatók -> Nyomtató hozzáadása
> * A Hozzáadás gombra a számítógéphez egy TCP/IP-címét vagy állomásnevét használata
> * Írja be a hello hello nyomtató IP-címét
> * Standard 9100 nyomtatóportot
> * Szükség esetén telepítse kézzel az hello megfelelő nyomtató-illesztőprogramot.
>
> ![Linux][Logo_Linux] Linux
>
> * például a Windows csak kövesse hello szokásos eljárás tooinstall hálózati nyomtató
> * Kövesse a hello nyilvános Linux útmutatói [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) vagy [Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) hogyan tooadd nyomtató.
>
>

- - -
![Hálózati nyomtatás][planning-guide-figure-2200]

##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Gazdagép-alapú nyomtató (megosztott nyomtató) az SMB protokollt a létesítmények közötti forgatókönyv
A nyomtatók állomásalapú nincsenek hálózati-kompatibilis úgy lett kialakítva. De állomásalapú nyomtató a hálózaton található számítógépek között megosztható, mindaddig, amíg hello nyomtató csatlakoztatott tooa üzemkész a számítógépen. Csatlakozás a vállalati hálózati helyek vagy ExpressRoute és helyi nyomtató megosztása. hello SMB protokoll helyett DNS NetBIOS név szolgáltatásként használja. hello NetBIOS állomásnév különbözhetnek hello DNS-állomásnevet. hello normál esetben hello NetBIOS állomásnév és a DNS-állomásnevet hello azonosak. hello DNS-tartomány nem célszerű a hello NetBIOS-névteret. Ennek megfelelően hello hello DNS-állomásnevet álló teljesen minősített DNS-állomásnevet és a DNS-tartomány nem használható a hello NetBIOS-névteret.

hello nyomtató megosztási azonosít egy egyedi nevet a hello hálózati:

* Állomásnév hello SMB-állomás (mindig szükséges).
* Hello megosztás (mindig szükséges) nevét.
* Ha a nyomtató megosztási nem hello hello tartomány nevét SAP rendszer azonos tartományban.
* A felhasználónév és jelszó is előfordulhat, nem szükséges tooaccess hello nyomtató megosztási.

Útmutató:

- - -
> ![Windows][Logo_Windows] Windows
>
> Helyi nyomtató megosztása.
> Nyissa meg hello Windows Explorer és a megosztási név hello hello nyomtató típusú hello Azure virtuális Gépen.
> A nyomtató telepítővarázsló végigvezeti hello telepítési folyamat.
>
> ![Linux][Logo_Linux] Linux
>
> Néhány példa a hálózati nyomtatók konfigurálása a Linux vagy többek között a fejezet kapcsolatos dokumentáció a Linux nyomtatásra vonatkozó. Hello működni fog az Azure Linux virtuális gép virtuális gép hello is ugyanúgy VPN része:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba) _Share_or_Windows_Share>
> * RHEL <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>
>
>

- - -
##### <a name="usb-printer-printer-forwarding"></a>USB-nyomtató (nyomtató továbbítás)
Az Azure hello lehetősége hello távoli asztali szolgáltatások tooprovide felhasználók hello hozzáférési tootheir helyi nyomtató eszközök távoli munkamenetben nem érhető el.

- - -
> ![Windows][Logo_Windows] Windows
>
> Nyomtatás a Windows további információkat itt talál: <http://technet.microsoft.com/library/jj590748.aspx>.
>
>

- - -
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Az SAP integrálását javítása és a létesítmények közötti (TMS) rendszer Azure rendszerek
SAP változás- és átviteli rendszer (TMS) kell konfigurálni toobe tooexport hello, és importálja az átviteli kérelem hello tükrében megfigyelhető rendszerekből. Feltételezzük, hogy hello fejlesztési példányok az SAP rendszer (fejlesztés) találhatók az Azure-ban, mivel hello minőségi megbízhatósági (QA) és a hatékony rendszerek (PRD) helyszíni. Továbbá feltételezzük, hogy van-e a központi átviteli könyvtár.

##### <a name="configuring-hello-transport-domain"></a>Átviteli tartomány hello konfigurálása
Konfigurálja az átviteli tartomány átviteli tartományvezérlő hello leírtak szerint kijelölt hello rendszeren [konfigurálása hello átviteli tartományvezérlő](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). A rendszer felhasználói TMSADM jön létre és hello szükséges RFC cél jön létre. Ha bejelöli a RFC kapcsolatok hello tranzakció SM59 használatával. Állomásnév-feloldási engedélyezni kell a átviteli tartományon keresztül.

Útmutató:

* A mi esetünkben döntöttünk hello helyszíni QAS rendszer hello CTS tartományvezérlő lesz. Tranzakció STM hívja. hello TMS párbeszédpanel jelenik meg. A szállítási tartomány konfigurálása párbeszédpanel. (Ez a párbeszédpanel csak akkor jelenik meg, ha még nincs konfigurálva az átviteli tartomány.)
* Győződjön meg arról, hogy automatikusan létrehozott hello felhasználói TMSADM engedélyezve van (SM59 -> ABAP kapcsolat -> TMSADM@E61.DOMAIN_E61 -> Részletek -> Utilities(M) engedélyezési teszt ->). hello lap kezdőképernyőjének tranzakció STM jelenítsen meg, hogy az SAP rendszer most hello tartományvezérlőjeként hello átviteli tartomány Itt látható módon működik:

![Tranzakció STM hello tartományvezérlőn indítási képernyő][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-hello-transport-domain"></a>SAP rendszerek hello átviteli tartomány beleértve
az SAP-rendszerek például átviteli tartományban hello sorozatát a következőképpen néz ki:

* A fejlesztői hello a rendszer az Azure-ban nyissa meg toohello rendszer (000 ügyfél), és hívja meg tranzakció STM. Egyéb konfigurációs lehetőségek közül választhat hello párbeszédpanel, és folytassa a rendszer közé tartozik a tartományban. Adja meg a célként megadott gazdagép hello tartományvezérlő ([SAP rendszerek beleértve hello átviteli tartomány](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). hello rendszer mostantól várakozási toobe hello átviteli tartomány része.
* Biztonsági okokból így már rendelkezik toogo hátsó toohello tartomány a tartományvezérlő tooconfirm a kérést. Válassza ki a rendszer áttekintése és jóváhagyása hello várakozási rendszer. Erősítse meg hello kérdés-hello konfigurációs lesznek kiosztva.

Az SAP rendszer hello átviteli tartományban más SAP rendszerek most tartalmaz hello összes hello szükséges információkat. Hello: ugyanaz, hello új SAP rendszer adatküldést tooall hello más SAP rendszerekkel, és hello SAP rendszer hello átviteli profil hello átviteli vezérlő program megadott hello cím eltöltött idő Ellenőrizze, hogy RFC-dokumentumokat és a hozzáférés toohello átviteli könyvtár hello tartomány működik.

A szokásos módon hello dokumentációjában leírt továbbra is hello konfigurációval, a rendszer [változás- és a rendszer](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Útmutató:

* Ellenőrizze, hogy a helyszíni STM megfelelően van konfigurálva.
* Ellenőrizze, hogy hello átviteli tartományvezérlő állomásnevét hello megoldhatók a virtuális gép visa Azure és fordítva.
* Hívás, tranzakciót STM -> egyéb konfigurációs -> rendszer közé tartozik a tartományban.
* A helyi TMS rendszeren hello hello kapcsolat ellenőrzéséhez.
* Átviteli útvonalon, csoportok és rétegek konfigurálása a szokásos módon.

A pont-pont csatlakoztatott létesítmények közötti forgatókönyv a helyszíni és az Azure közötti hello késés továbbra is lehet jelentős. Ha azt hajtsa végre az objektumok keresztüli fejlesztési és tesztelési rendszerek tooproduction szállítására hello sorozatát, vagy átvitelek alkalmazása gondolja át, vagy támogatási csomagokat toohello különböző rendszerek, akkor vegye figyelembe, hogy a függő központi átviteli hello hello helye könyvtár, hello rendszereivel fog történni nagy késleltetésű olvasása vagy adatok írása hello központi átviteli könyvtárban. hello helyzet akkor hasonló tooSAP fekvő konfigurációk, ahol hello különböző rendszerek keresztül hello adatközpontok jelentős távolsága a különböző adatközpontokban van elosztva.

Az ilyen késés körül toowork rendezés és gyors írásakor vagy olvasásakor tooor hello átviteli könyvtár, két STM átviteli tartományok (egy a helyszíni., a másik tartományban Azure és a hivatkozás hello átviteli hello rendszerekkel beállíthatja a munkahelyi hello rendszert Ellenőrizze az ebben a dokumentációban, amelyből megtudhatja, a fogalom a hello SAP TMS mögött hello alapelvek: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/ 38dd924eb711d182bf0000e829fbfe/FRAMESET.htm>.

Útmutató:

* (A helyszíni és az Azure) helyekre átviteli tartomány beállítása STM tranzakció használatával <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Hivatkozás hello tartományok tartománnyal rendelkező hivatkozásra, és erősítse meg a hello hivatkozás hello két tartomány között.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Hello konfigurációs kapcsolódó toohello rendszer terjesztése.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC forgalom Azure-ban és a helyszíni (létesítmények közötti) található SAP példányai között
RFC forgalom közötti rendszerek, amelyek a helyszíni és az Azure-ban toowork kell. kapcsolat toosetup hívja tranzakció SM59 a forrásrendszerben toodefine kell az RFC-kapcsolat hello cél rendszer felé. hello konfigurálása hasonló toohello szabványos telepítése az RFC-kapcsolat számára.

Feltételezzük, hogy hello létesítmények közötti forgatókönyv, mely SAP rendszerek futnak egymással toocommunicate igénylő szerepelnek hello virtuális gépek hello ugyanabban a tartományban. Ezért hello egy SAP rendszerek közötti RFC-kapcsolat beállítása nem eltérnek a hello beállítási lépéseket és helyszíni forgatókönyvekben bemeneti adatok.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Az Azure-ban, vagy fordítva található SAP példányokból elérése során "local" fileshares
Az Azure-ban található SAP példányok kell tooaccess hello vállalati helyi tartoznak fájlmegosztások. Ezenkívül a helyszíni SAP példányok kell az Azure-ban található fájlmegosztások tooaccess. tooenable hello fájlmegosztások hello engedéllyel és megosztási beállítások hello helyi rendszeren kell konfigurálnia. Győződjön meg arról, hogy tooopen hello portok hello VPN vagy az Azure és az Adatközpont között ExpressRoute kapcsolat.

## <a name="supportability"></a>Támogatási lehetőségek
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Az Azure felügyeleti megoldás az SAP
Rendelés tooenable hello figyelési kritikus kritikus SAP rendszerek Azure hello SAP a Hálózatfigyelő eszközök SAPOSCOL vagy SAP a gazdagép ügynöke hello Azure virtuális gép szolgáltatásgazda egy Azure Figyelőbővítmény az SAP keresztül adatokat beolvasni. Óta hello iránti igények kielégítése érdekében SAP által meghatározott tooSAP alkalmazások, a Microsoft nem toogenerically megvalósítása hello szükséges funkciókat Azure úgy döntött, de hagyja a mezőt az ügyfelek toodeploy hello szükséges figyelési összetevők és konfigurációk tootheir Az Azure-ban futó virtuális gépek. Azonban központi telepítési és életciklusának kezelését a figyelési összetevők hello többnyire automatizált lesz az Azure-ban.

#### <a name="solution-design"></a>Megoldásterv
hello megoldást fejlesztett tooenable hello architektúra Azure Virtuálisgép-ügynök és a bővítmény keretrendszer SAP figyelési alapul. hello hello Azure Virtuálisgép-ügynök és a bővítmény keretrendszer lényege hello Azure Virtuálisgép-bővítmény gyűjtemény egy virtuális Gépen belül a rendelkezésre álló szoftver alkalmazás(ok) tooallow telepítését. hello elve (ilyen esetekben hello Azure Figyelőbővítmény az SAP) tooallow lényege mögött a fogalom, hello központi telepítését a központi telepítéskor a virtuális gép és hello szoftverhez konfigurációs speciális funkciókat.

2014. február óta hello "Azure Virtuálisgép-ügynök", amely lehetővé teszi, hogy adott Azure Virtuálisgép-bővítmények hello VM van be a nézetmodellbe, a Windows virtuális gépek alapértelmezés szerint a virtuális gép létrehozása a hello Azure portálon belül kezelése. SUSE vagy a Red Hat Linux hello esetén Virtuálisgép-ügynök már része az Azure Piactéri lemezképhez. Abban az esetben, ha a Linux virtuális gép egy volna feltöltése a helyszíni tooAzure hello Virtuálisgép-ügynök van toobe manuálisan telepíteni.

hello alapvető építőelemeket, amelyekből hello figyelés megoldás az Azure-ban, az SAP néz ki:

![A Microsoft Azure bővítmény összetevők][planning-guide-figure-2400]

Ahogy fent hello blokk ábrán is látható, hello az SAP felügyeleti megoldás egyik részét képező hello Azure Virtuálisgép-lemezkép, és amely globálisan replikált tára Azure műveletei által felügyelt Azure-bővítmény katalógus tárolja. Feladata hello hello hello Azure műveletek toopublish új verzióihoz hello Azure Figyelőbővítmény az SAP SAP toowork Azure végrehajtásának dolgozik közös SAP/MS munkacsoport. Az Azure Figyelőbővítmény az SAP hello Microsoft Azure Diagnostics (ÜVEGVATTA) kiterjesztés vagy a Linux Azure Diagnostics (LAD) tooget hello szükséges információt fogja használni.

Amikor telepít egy új Windows virtuális Gépet, "Azure Virtuálisgép-ügynök" hello automatikusan szerepel-e virtuális gép hello. hello az ügynök függvény betöltése toocoordinate hello és hello Azure-bővítményekkel konfigurációjának SAP NetWeaver rendszerek figyeléséhez. Linux virtuális gépek hello Azure Virtuálisgép-ügynök része már hello Azure piactér operációsrendszer-lemezképek.

Van azonban továbbra is szükséges toobe hello ügyfél által végrehajtott minden lépést. Ez az hello engedélyezését, illetve hello teljesítményadat-gyűjtési beállításait. hello folyamattal kapcsolatos, a PowerShell-parancsfájl vagy a CLI parancs automatizált toohello "konfiguráció". hello PowerShell-parancsprogram letölthető a Microsoft Azure Script Center hello hello leírtak [telepítési útmutató][deployment-guide].

hello átfogó architektúrája hello Azure felügyeleti megoldás az SAP néz ki:

![Az Azure felügyeleti megoldás az SAP NetWeaver][planning-guide-figure-2500]

**Hello pontosan hogyan-tooand részletes leírást a PowerShell-parancsmagok vagy a CLI parancs használatával központi telepítések során, az utasítások szerint hello hello megadott [telepítési útmutató][deployment-guide].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integráció az Azure található SAP példányának SAProuter
Azure-beli SAP példányok kell toobe SAProuter elérhető is.

![SAP-útválasztó hálózati kapcsolat][planning-guide-figure-2600]

Egy SAProuter lehetővé teszi, hogy a hello TCP/IP-kommunikáció, ha nincs közvetlen IP-kapcsolat részt vevő rendszerek között. Ez lehetővé teszi a hello kommunikációs partnerek között nincs végpontok közötti kapcsolat szükség a hálózati szintű hello kihasználásához. hello SAProuter alapértelmezés szerint 3299 portot figyel.
tooconnect SAP-példányok keresztül egy SAProuter toogive hello SAProuter karakterlánc és állomásnévvel rendelkező bármely kísérlet tooconnect van szükség.

## <a name="sap-netweaver-as-java"></a>SAP NetWeaver AS Java
Hello dokumentum eddigi hello fókusz SAP NetWeaver általában vagy hello SAP NetWeaver ABAP verem. Ez kis területen figyelembe kell venni bizonyos hello SAP Java verem találhatók. Hello legfontosabb kizárólag-alapú alkalmazások SAP NetWeaver Java egyik hello SAP vállalati portálon. Más SAP NetWeaver alapú alkalmazások, például SAP-PI és SAP megoldás Manager hello SAP NetWeaver ABAP és Java verem használható. Ezért biztosan nem egy szükséges tooconsider különleges szempontok kapcsolódó toohello SAP NetWeaver Java verem is.

### <a name="sap-enterprise-portal"></a>SAP vállalati portál
hello beállítása egy SAP portál egy Azure virtuális gép nem eltér a helyi telepítés központi telepítése a létesítmények közötti forgatókönyv. Hello DNS helyszíni végezhető el, mert egyes példányok hello hello portbeállítások is lehet végrehajtani, mert konfigurált helyszíni. hello javaslatok és, amennyiben a jelen dokumentumban ismertetett korlátozások vonatkoznak az alkalmazás, például SAP vállalati portál vagy hello SAP NetWeaver Java verem általában.

![Elérhetőségi SAP-portál][planning-guide-figure-2700]

Egy speciális környezet-forgatókönyv szerint egyes ügyfelek hello hello SAP vállalati portál toohello Internet közvetlen elérhetővé tegyék addig, amíg hello virtuálisgép-gazdagépre csatlakoztatott toohello vállalati hálózati helyek a VPN-alagúton vagy ExpressRoute keresztül. Ilyen esetben van toomake meg arról, hogy adott portok nyitva, és nem blokkolja tűzfal vagy a hálózati biztonsági csoport. hello azonos mechanika kellene lépnek érvénybe, ha azt szeretné, hogy tooconnect tooan SAP Java példány egy kizárólag felhőalapú forgatókönyvben helyszíni toobe.

hello kezdeti portál URI http (s):`<Portalserver`>: 5XX00/irj ahol hello port alkot 50000 plusz (Systemnumber x 100). hello alapértelmezett portál URI az SAP 00 rendszer `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. További részletekért tekintse meg rendelkezik egy <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Végpont-konfiguráció][planning-guide-figure-2800]

Ha azt szeretné, toocustomize hello URL-címet és/vagy a portok az SAP vállalati portál, ellenőrizze a jelen dokumentáció:

* [Módosítsa a portál URL-címe](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Alapértelmezett portszámok, Portal portszámok módosítása](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

<a name="7cf991a1-badd-40a9-944e-7baae842a058"></a>
## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Magas rendelkezésre állású (HA) és a katasztrófa utáni helyreállítás (DR) az Azure virtuális gépeken futó SAP NetWeaver
### <a name="definition-of-terminologies"></a>Terminológiát meghatározása
hello kifejezés **magas rendelkezésre ÁLLÁS** általában kapcsolódó tooa értéke technológia, amely minimálisra csökkenti az informatikai szolgáltatások keresztül IT szolgáltatások üzleti folytonosság megadásával redundáns, hibatűrő vagy feladatátvételi védett összetevők hello belül **azonos** adatközpont. Ebben az esetben egy Azure-régión belül.

**Vészhelyreállítás (DR)** is célzott IT szolgáltatások megszűnésének minimalizálja a és a helyreállítás azonban keresztben **különböző** adatközpontokban, amelyek általában több száz kilométer távolságra található. A mi esetünkben általában hello belül különböző Azure-régiók közötti geopolitikai ugyanabban a régióban vagy ügyfélként az Ön által létrehozott.

### <a name="overview-of-high-availability"></a>A magas rendelkezésre állás – Áttekintés
Azt is külön hello vitafórum SAP magas rendelkezésre állás az Azure-ban két részre kapcsolatban:

* **Azure-infrastruktúra magas rendelkezésre állású**, pl. (VM) számítási, hálózati, tárolási stb és SAP rendelkezésre állásának növelése előnye GAZDAGÉPFÜRTÖK.
* **SAP alkalmazás magas rendelkezésre állású**, például a magas rendelkezésre ÁLLÁSÚ az SAP szoftverösszetevőket:
  * SAP alkalmazáskiszolgálók
  * SAP ASC/SCS példány
  * Adatbázis-kiszolgálót

és hogyan azt kombinálható az Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ.

SAP a magas rendelkezésre állás, az Azure-ban van néhány eltérést tooSAP magas rendelkezésre állás a helyi fizikai vagy virtuális környezetben. hello SAP a következő dokumentum ismerteti Windows rendszeren virtualizált környezetben szabványos SAP magas rendelkezésre állású konfiguráció: <http://scn.sap.com/docs/DOC-44415>. Nincs sapinst integrált SAP-magas rendelkezésre ÁLLÁSÚ konfiguráció Linux például Windows létezik. Vonatkozó SAP magas rendelkezésre ÁLLÁSÚ Linux helyszíni további információkat itt találja: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure-infrastruktúra magas rendelkezésre állás
Nincs nem single-VM SLA-t Azure virtuális gépeken most. egy ötletet tooget hogyan egy virtuális hello rendelkezésre állását nézhet ki például egyszerűen hozhat létre hello szorzatát hello különböző elérhető Azure SLA-k: <https://azure.microsoft.com/support/legal/sla/>.

hello hello számítási alapja havonta vagy 43200 perc 30 nap. Ezért 0,05 % állásidő megfelel-e too21.6 perc. Szokásos módon hello hello különböző szolgáltatások rendelkezésre állásának a rendszer a következő módon hello szorozza meg:

(Rendelkezésre állási szolgáltatás #1/100) * (rendelkezésre állási szolgáltatás #2/100) * (rendelkezésre állási szolgáltatás #3/100) *...

Például:

(99,95/100) * (99,9/100) * (99,9/100) = 0.9975 vagy egy 99.75 % rendelkezésre állását.

#### <a name="virtual-machine-vm-high-availability"></a>A virtuális gép (VM) magas rendelkezésre állás
Az Azure platform események, amelyek hatással lehetnek a virtuális gépek rendelkezésre állását hello két típusa van: a tervezett karbantartást és a nem tervezett karbantartás.

* Az alapul szolgáló Azure platformon tooimprove Microsoft toohello által végzett rendszeres frissítések érhetők el tervezett karbantartási események teljes megbízhatóság, teljesítmény és a virtuális gépeken futó hello platform infrastruktúra biztonságát.
* Nem tervezett karbantartási események fordulhat elő, amikor valamilyen módon hello hardver- vagy a virtuális gép alapul szolgáló fizikai infrastruktúra hibát jelzett. Ez lehet helyi hálózati hiba, a helyi lemezek meghibásodása, vagy egyéb állványszintű meghibásodások. Ilyen hiba lép fel, amikor hello Azure platformon automatikusan át a virtuális gép hello sérült fizikai tároló kiszolgáló a virtuális gép tooa megfelelő fizikai kiszolgálón. Ezek az események ritkán fordul elő, de a virtuális gép tooreboot okozhat.

További részletek találhatók a jelen dokumentációban: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Az Azure Storage redundancia
hello a Microsoft Azure Storage-fiók adatai mindig replikált tooensure tartósság és magas rendelkezésre állású, teljesíti-hello Azure Storage szolgáltatásiszint-szerződés még átmeneti hardverhibák esetén hello felületét a

Mivel az Azure Storage alapértelmezés szerint megakadályozza a hello adatok 3 képek, RAID5 vagy több Azure lemezre RAID1 nem szükségesek.

További részletek megtalálhatók a cikkben: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-tooachieve-higher-availability-of-sap-applications"></a>Azure infrastruktúra virtuális gép újraindítása tooAchieve "Magasabb rendelkezésre állását" SAP-alkalmazásokból használata
Ha úgy dönt, hogy nem toouse funkciók például a Windows Server feladatátvételi fürtszolgáltatási (WSFC), vagy egy egyenértékű Linux (hello ez utóbbi egy még nem támogatott az Azure-on SAP szoftver együtt), Azure virtuális gép újraindítása túlterhelt tooprotect egy SAP rendszert a tervezett és nem tervezett leállás hello Azure fizikai kiszolgálói infrastruktúra és a teljes alapul szolgáló Azure platformon.

> [!NOTE]
> Fontos, hogy Azure virtuális gép újraindítása elsősorban védi a virtuális gépek és alkalmazásokat nem toomention. Indítsa újra a virtuális gép nem biztosít magas rendelkezésre állás a SAP-alkalmazásokból, de az infrastruktúra rendelkezésre állási bizonyos szintű kínálnak, ezért közvetve "magas rendelkezésre állás érdekében" SAP rendszerek. Hello ideig fog tartani, amíg toorestart egy virtuális Gépet egy gazdagépen tervezett vagy nem tervezett leállás után is van nem az SLA-t. Ez a metódus "a magas rendelkezésre állás" ezért nem alkalmas A SCS vagy az adatbázis-kezelő például SAP rendszer kritikus összetevői.
>
>

A magas rendelkezésre állás fontos infrastruktúra elem az tároló. Például Az Azure Storage SLA 99,9 % rendelkezésre állási. Ha egy telepíti a lemezzel rendelkező összes virtuális gép egy egyetlen Azure Storage-fiók esetén lehetséges Azure Storage elérhetetlensége, akkor kerülnek, Azure Storage-fiókhoz tartozó összes virtuális gépet, is minden SAP összetevőiről és virtuális gépek belül futtatott elérhetetlensége be.  

Ahelyett, hogy minden virtuális gép üzembe egy egyetlen Azure Storage-fiókot, használhatja a kijelölt tárhelycsomópont fiókokat az egyes virtuális gépek, és így általános virtuális gép és az SAP rendelkezésre állásának növeléséhez több független Azure Storage-fiókok használatával.

Egy minta-architektúrát egy SAP NetWeaver rendszer által használt Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ nézhet ki:

![Magas rendelkezésre ÁLLÁSÚ tooachieve SAP alkalmazás "felső" rendelkezésre állási Azure-infrastruktúra használatával][planning-guide-figure-2900]

SAP-összetevők kritikus azt elért hello, amennyiben a következő:

* Magas rendelkezésre állású SAP Alkalmazáskiszolgáló (AS)

SAP alkalmazáskiszolgáló-példányok a redundáns összetevők. Minden egyes SAP, példány telepítve van a saját virtuális Gépet, amely futtatja a egy másik Azure hiba és a tartomány frissítése (lásd: fejezetek [tartalék tartományok] [ planning-guide-3.2.1] és [frissítési tartományok] [ planning-guide-3.2.2]). Ez biztosítja Azure rendelkezésre állási csoportokkal (című [Azure rendelkezésre állási készletek][planning-guide-3.2.3]). Egy Azure hiba vagy frissítéséhez tartományi lehetséges tervezett vagy nem tervezett elérhetetlensége fog okozhat a virtuális gépek a korlátozott számú hiányában az SAP-AS példányt.
Minden példány kerül a saját Azure Storage-fiók – lehetséges hiányában egy Azure Storage-fiók hiányában csak egy virtuális gép, akkor az SAP-AS az SAP példány. Azonban vegye figyelembe, hogy nincs maximális száma Azure Storage-fiókok egy Azure-előfizetéssel belül. Automatikus indítás tooensure (A) SCS példány után indítsa újra a virtuális gép hello, ellenőrizze, hogy tooset Autostart paraméter (A) SCS példányban fejezetben ismertetett profil elindításához [használatával Autostart SAP-példányok][planning-guide-11.5].
Olvassa el is fejezet [magas rendelkezésre állású SAP alkalmazáskiszolgálók] [ planning-guide-11.4.1] további részleteket.

* *Magasabb* SAP rendelkezésre állását (A) SCS példány

Itt azt hasznosítani tooprotect hello Azure VM indítsa újra a virtuális gép telepített SAP (A) SCS-példánnyal. A hello kis-és az Azure tervezett vagy nem tervezett leállás kiszolgálója, virtuális gépek újraindul egy másik elérhető kiszolgálón. A korábban említett, elsősorban a virtuális gépek és alkalmazásokat nem Azure virtuális gép újraindítása védi, ebben az esetben hello (A) SCS példánya. Indítsa újra a virtuális gép hello keresztül fog eljut közvetve "magas rendelkezésre állás érdekében" SAP (A) SCS-példány. Automatikus indítás tooinsure (A) SCS példány után indítsa újra a virtuális gép hello, ellenőrizze, hogy tooset Autostart paraméter (A) SCS példányban fejezetben ismertetett profil elindításához [használatával Autostart SAP-példányok][planning-guide-11.5]. Ez azt jelenti, hogy hello (A) SCS példányát hibaként egyetlen pont, (SPOF) egy virtuális gépen futó lesz hello meghatározó tényező hello egész SAP fekvő hello rendelkezésre állását.

* *Magasabb* rendelkezésre állási adatbázis-kezelő kiszolgáló

Itt hasonló toohello SAP (A) SCS példány-és nagybetűhasználattal, a Microsoft Azure virtuális gép újraindítása tooprotect hello VM telepített adatbázis-kezelő szoftverrel használják, és azt "magasabb rendelkezésre állásának eléréséhez" adatbázis-kezelő szoftver használatával indítsa újra a virtuális gép.
Egyetlen virtuális gépen futó DBMS egyben a SPOF, és hello meghatározó tényező hello egész SAP fekvő hello rendelkezésre állását.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP alkalmazás magas rendelkezésre állásra Azure IaaS
tooachieve teljes SAP rendszer magas rendelkezésre állású, igazolnia kell tooprotect kritikus SAP rendszer összetevők, például redundáns SAP-alkalmazáskiszolgálókat, és egyedi összetevőket (például egyetlen pont, hiba), például SAP (A) SCS-példány és az adatbázis-kezelő.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Az SAP alkalmazáskiszolgálók magas rendelkezésre állás
Hello SAP alkalmazáspéldányok kiszolgálók/párbeszédpanel esetén nem szükséges toothink egy adott magas rendelkezésre állású megoldás. Magas rendelkezésre állású egyszerűen valósul meg a redundancia és ezáltal rendelkező elegendő azokat a különböző virtuális gépek. Azok az összes kell helyezni, amelyek a virtuális gépek hello azonos Azure rendelkezésre állási csoport tooavoid előfordulhat, hogy frissíteni hello hello karbantartási tervezett leállások során ugyanannyi időt vesz igénybe. hello alapvető funkciókat épít, különböző frissítési és tartalék tartományok belül egy Azure méretezési egység, amely már jelent fejezetben [frissítési tartományok][planning-guide-3.2.2]. Az Azure rendelkezésre állási készletek fejezetben bemutatott [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] ebben a dokumentumban.

Nincs hiba, és a frissítési tartományok, amelyek segítségével Azure rendelkezésre állási csoport egy Azure méretezési egység belül korlátlan számú van. Ez azt jelenti, hogy egy rendelkezésre állási készlethez megadott előbb vagy később hello tényt, hogy az azonos tartalék vagy frissítéséhez tartományi hello említi egynél több virtuális gép üzembe a virtuális gépek száma

A dedikált virtuális Gépeik néhány SAP-alkalmazáskiszolgáló telepítése példánya, és feltételezve, hogy 5 frissítési tartományok kapott azt, a következő kép hello hello végén jelenik-e. hello tényleges maximális hiba és a frissítési tartományok száma belül egy rendelkezésre állási csoportot a jövőbeli hello változhatnak:

![Magas rendelkezésre ÁLLÁSÚ SAP alkalmazáskiszolgáló az Azure-ban][planning-guide-figure-3000]

További részletek találhatók a jelen dokumentációban: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="high-availability-for-hello-sap-ascs-instance-on-windows"></a>A Windows hello SAP (A) SCS példányához magas rendelkezésre állás
Windows Server feladatátvételi fürt (WSFC)-e a gyakran használt megoldás tooprotect hello SAP (A) SCS példány. Azt is integrálva van egy "magas rendelkezésre ÁLLÁSÚ telepítés" formában sapinst. Ezen a ponton a időben történő hello Azure-infrastruktúra nem tud tooprovide hello funkció tooset hello szükséges Windows Server feladatátvevő fürt hello azonos módon elkészült a helyi másolatot.

2016. január hello Azure cloud platform hello Windows operációs rendszer nem biztosít két Azure virtuális gépek között megosztott lemezt a fürt megosztott kötetei használatával hello lehetőségét.

3. fél szoftver, amely megosztott kötet nyújt a szinkron és átlátható lemez replikációt, amely integrálható a Windows hello használata, ha egy érvényes megoldás. Ezt a módszert használja azt jelenti, hogy csak hello aktív fürtcsomópont képes tooaccess egy hello lemeze másolja egy időben. A magas rendelkezésre ÁLLÁSÚ. január 2016 konfigurációs támogatott tooprotect hello SAP (A) SCS példányához a Windows vendég operációs rendszer Azure virtuális gépeken, valamint a 3. fél szoftver SIOS DataKeeper.

hello SIOS DataKeeper megoldást biztosít egy megosztott lemez fürt erőforrás tooWindows feladatátvevő fürtök azzal, hogy:

* Egy további Azure virtuális merevlemez csatolva tooeach hello virtuális gépek (VM), amelyek Windows fürtkonfiguráció
* Mindkét virtuális gép csomópontokon futó SIOS DataKeeper Cluster Edition
* Virtuális merevlemez SIOS DataKeeper Cluster Edition szinkron módon tükrözi további hello hello tartalmát úgy, hogy a virtuális gépek tooadditional csatolt virtuális merevlemez forráskötet a cél virtuális gép csatlakoztatott kötet.
* SIOS DataKeeper hello forrás- és helyi kötetek absztrakt, és azok bemutatásának tooWindows feladatátvevő fürt, amely egyetlen megosztott lemezként.

Hogyan található összes részletes tooinstall SIOS Datakeeper és SAP a Windows feladatátvevő fürtben lévő hello [fürtszolgáltatás SAP ASC példányhoz használatával Windows Server feladatátvevő fürt SIOS DataKeeper rendelkező Azure] [ ha-guide-classic]találhatók meg.

#### <a name="high-availability-for-hello-sap-ascs-instance-on-linux"></a>Magas rendelkezésre állású hello SAP (A) SCS példány Linux rendszeren
Től Dec 2015 nincs is egyenértékű tooshared lemez WSFC Azure Linux virtuális gépekhez. 3. fél szoftverrel SIOS a Windows-alternatív megoldásokat a rendszer nem érvényesíti még az SAP futó Azure Linux.

#### <a name="high-availability-for-hello-sap-database-instance"></a>Hello SAP adatbázispéldány magas rendelkezésre állás
hello szokásos SAP adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ telepítés alapul, a két adatbázis-kezelő virtuális gépeken, ahol adatbázis-kezelő magas rendelkezésre állású funkció használható hello aktív DBMS példány toohello tooreplicate adatait második virtuális gép be a passzív adatbázis-kezelő példánya.

Ismerteti a magas rendelkezésre állási és vészhelyreállítási helyreállítási funkcióhoz tartozó adatbázis-kezelő általános, valamint az olyan konkrét célrendszerben hello [adatbázis-kezelő telepítési útmutatója][dbms-guide].

#### <a name="end-to-end-high-availability-for-hello-complete-sap-system"></a>Végpontok közötti magas rendelkezésre állás a teljes SAP rendszert hello
Teljes SAP NetWeaver magas rendelkezésre ÁLLÁSÚ architektúrájának az Azure - két példa egy a Windows és Linux egyet.
hello fogalmakat, ahogy az alábbi leírásban esetleg kicsit sérült esetén számos SAP rendszert telepít, és a telepített virtuális gépek hello száma túllépte hello maximális száma a tárfiókok előfizetésenként toobe. Ebben az esetben virtuális gépek virtuális merevlemezeket kell toobe együttesen egy Tárfiókon belül. Általában tennénk VHD-k az SAP alkalmazásréteg virtuális gépek különböző SAP rendszerek kombinálásával.  Azt is együtt a különböző VHD-k különböző adatbázis-kezelő virtuális gépek különböző SAP rendszerek egy Azure Storage-fiókot. Azure Storage-fiókok hello IOPS-korlátok vonatkoznak ezáltal tartva ( <https://azure.microsoft.com/documentation/articles/storage-scalability-targets> )

##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] Magas rendelkezésre ÁLLÁSÚ Windows rendszeren
![SAP NetWeaver alkalmazás magas rendelkezésre ÁLLÁSÚ architektúra Azure IaaS SQL-kiszolgálót][planning-guide-figure-3200]

a következő Azure szerkezetek hello hello SAP NetWeaver rendszer infrastrukturális problémára és javítását állomás toominimize hatás használják:

* a rendszer teljes hello Azure (kötelező – adatbázis-kezelő réteg, (A) SCS példány és a teljes alkalmazás réteg kell toorun a hello ugyanazon a helyen) van telepítve.
* hello teljes rendszer fut egy Azure-előfizetéssel (kötelező) belül.
* hello teljes rendszert futtat egy Azure virtuális hálózaton belül (kötelező).
* a három rendelkezésre állási készletek hello hello virtuális gépek egy SAP rendszer elkülönítése lehetőség toohello tartozó hello virtuális gépek mellett is ugyanazt a virtuális hálózatot.
* Adatbázis-kezelő példányok egy SAP rendszer futó összes virtuális gép van egy rendelkezésre állási csoportban. Feltételezzük, hogy van-e az adatbázis-kezelő példányok esetében a rendszer futtató óta natív adatbázis-kezelő magas rendelkezésre funkcióit használják, mint például az SQL Server AlwaysOn vagy Oracle Data Guard egynél több virtuális.
* Adatbázis-kezelő példányok futó összes virtuális gép saját tárfiókot használni. Adatbázis-kezelő adatainak és naplókönyvtárainak fájlok lesznek replikálva az egy tárolási fiók tooanother tárfiók hello adatok szinkronizálása az adatbázis-kezelő magas rendelkezésre állású funkciókat használja. Egy tárfiókot elérhetetlensége miatt hiányában egy SQL-Windows-fürt csomópontja, de nem hello teljes SQL Server szolgáltatás.
* (A) SCS példányát egy SAP rendszert futtató összes virtuális gép van egy rendelkezésre állási csoportban. A virtuális gépek Windows Server feladatátvételi fürt (WSFC) (A) tooprotect SCS példány van konfigurálva.
* (A) SCS példányok futó összes virtuális gép saját tárfiókot használni. (A) Egy tárolási fiók tooanother tárfiók SIOS DataKeeper replikációval SCS példány fájlok és a SAP globális mappa replikálódnak. Hiányában egy tárfiókot, akkor egy (A) elérhetetlensége SCS Windows fürtcsomópontra, de nem teljes hello (A) SCS szolgáltatás.
* Hello SAP alkalmazásréteg server képviselő hello virtuális gépek vannak egy harmadik rendelkezésre állási csoportban.
* SAP alkalmazáskiszolgálók futó összes hello virtuális gép saját tárfiókot használni. Egy tárfiókot hiányában egy SAP alkalmazáskiszolgáló, ha más SAP-AS továbbra is toorun elérhetetlensége miatt.

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] Magas rendelkezésre ÁLLÁSÚ Linux rendszeren
az SAP magas rendelkezésre ÁLLÁSÚ Azure Linux hello architektúra tulajdonképpen ugyanaz, mint a Windows, mint a fent leírt hello. 2016. január van, ha a két korlátozások:

* csak a SAP ASE 16 jelenleg támogatott Azure-on Linux nélkül valamelyik ASE replikációs szolgáltatást.
* SAP (A) SCS magas rendelkezésre ÁLLÁSÚ megoldás nincs még az Azure-on Linux rendszeren támogatott

Következtében a 2016. január egy SAP-Linux-Azure rendszer elérése nem ugyanabban a rendelkezésre állási SAP-Windows-Azure rendszert hello miatt hiányzik a magas rendelkezésre ÁLLÁSÚ hello (A) SCS példány és hello egypéldányos SAP ASE adatbázis.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Automatikus indítás SAP-példányok használata
SAP toostart SAP példányok kínált hello funkció hello hello OS belül hello virtuális gép elindítása után azonnal. hello pontos lépéseket a volt részletes ismertetését lásd a Tudásbázis következő cikke SAP [1909114] – hogyan toostart SAP-példányok automatikusan az automatikus indítási paraméter használatával. Azonban SAP van nem ajánló toouse hello beállítása többé nincs vezérlő hello sorrendben példány újraindul, mert egynél több virtuális gép van hatással, vagy több futtatott virtuális gépenként feltételezve. Feltéve, hogy a jellemző Azure forgatókönyv egy SAP application server-példány egyetlen virtuális gép újraindítása végül első virtuális gép és hello esetén, hello Autostart nincs igazán fontos, és ez a paraméter hozzáadásával engedélyezhető:

    Autostart = 1

A hello hello SAP ABAP és/vagy a Java-példány profil elindításához.

> [!NOTE]
> hello Autostart paraméter néhány downfalls is rendelkezhet. Részletesebben hello paraméter eseményindítók hello kapcsolatos Windows/Linux hello példány szolgáltatás hello start SAP ABAP vagy Java-példány. Hogy biztosan helyzet hello olyankor, amikor elindul a hello operációs rendszerek. Azonban az SAP-szolgáltatások újraindítása is egy közös lépésként SAP szoftver életciklus-kezelési funkciók, például a SUM vagy más frissítéseket, vagy frissíti. Ezek a funkciók nem vár egy példány toobe összes automatikusan újraindul. Ezért hello Autostart paraméter le kell tiltani az ilyen feladatok futtatása előtt. hello Autostart paraméternek is nem használható vannak foglalva, ASC/SCS/CI például SAP-példány.
>
>

Tekintse meg a további információhoz autostart az SAP-példányok itt:

* [SAP, valamint a Unix kiszolgáló indítása/leállítása indítása/leállítása](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Indítása és leállítása SAP NetWeaver felügyeleti ügynökök](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Hogyan tooenable automatikus indítsa el a HANA-adatbázisból](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Nagyobb 3 szintű SAP rendszerek
Magas rendelkezésre állású aspektusainak 3 szintű SAP konfigurációk kapott tárgyalt korábbi szakaszokban már. De mi a helyzet a rendszerek, ahol hello DBMS Kiszolgálókövetelmények túl nagyok toohave azt található az Azure-ban, de hello SAP alkalmazásréteg is telepíthető az Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>3 rétegű SAP konfigurációk helye
Már nem támogatott toosplit hello alkalmazás réteg saját magát vagy hello alkalmazás és a helyszíni és az Azure közötti adatbázis-kezelő réteg. Egy SAP rendszer teljesen telepített helyi vagy az Azure-ban. Nincs is támogatott toohave néhány hello alkalmazáskiszolgálók futtassa a helyszíni és más Azure-ban. Ez a kezdőpont hello Vitafórum hello. Azt is toohave hello adatbázis-kezelő összetevők egy SAP-rendszer nem támogatja, és a SAP server alkalmazásréteg két különböző Azure-régiókban telepített hello. Például USA középső RÉGIÓJA, USA nyugati régiója és SAP alkalmazásréteg az adatbázis-kezelő. Nem támogatja az ilyen konfigurációt hello SAP NetWeaver architektúra érzékenységének hello késés.

Azonban az elmúlt évben adatok hello folyamán center partnerek közös helyek tooAzure régiók fejlesztett ki. Ezeket a helyeket közös gyakran szerepelnek nagyon közel toohello fizikai az Azure data központok belül egy Azure-régiót. hello rövid távolság és az eszközök hello közös elhelyezés-ExpressRoute keresztül az Azure a kapcsolat a késési, amely kisebb, mint 2ms eredményezhet. Ilyen esetben toolocate hello DBMS réteg (beleértve a tárolási SAN/NAS) egy közös elhelyezés és hello SAP alkalmazásréteg az Azure-ban lehetőség. Től Dec 2015 jelenleg nem áll a telepítések hasonlóan. Azonban nem SAP alkalmazástelepítések rendelkező, különböző ügyfelektől már ilyen megközelítések használ.

### <a name="offline-backup-of-sap-systems"></a>Offline biztonsági másolat az SAP-rendszerek
Hello függ (rétegének 2 vagy 3 szintű) van kiválasztva az SAP-konfiguráció oka lehet egy kell toobackup. hello tartalom hello virtuális gépért plusz toohave hello adatbázis biztonsági másolatát. adatbázis-kezelő hello kapcsolatos biztonsági mentések várt toobe adatbázis-módszerekkel együtt történik. Hello a különböző adatbázisokhoz, részletes leírását megtalálhatók [DBMS útmutató][dbms-guide]. A hello ugyanakkor, hello SAP-adatok biztonsági másolat készíthető (hello adatbázis tartalmát is beleértve) offline módon ebben a szakaszban leírt módon vagy online hello a következő szakaszban leírtak szerint.

hello offline biztonsági másolat alapvetően van szüksége a virtuális gép hello hello Azure portálon keresztül leállítására, és hello alap virtuális lemez egy példányát, és az összes csatolt VHD-k toohello virtuális gép. Ezzel megőrzik ponttá a virtuális gép hello idő kép és a kapcsolódó lemezt. Ajánlott toocopy hello "biztonsági mentések" be egy másik Azure Storage-fiókot. Ezért az eljárást a következő fejezetben hello [Azure Storage-fiókok között a lemezek másolásának] [ planning-guide-5.4.2] ebben a dokumentumban csak akkor vonatkozik.
Módosításokon kívül a leállítási használatával hello hello egy Azure-portálon is megteheti Powershell vagy a parancssori felületen keresztül itt: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Visszaállítás, az állapotban, amelyek a hello törlése alap virtuális gép, valamint az eredeti lemezek hello hello kiinduló virtuális gép, és a csatlakoztatott virtuális merevlemezek, a Másolás hátsó hello mentett VHD-k toohello eredeti Tárfiókot, és azt, majd újratelepítésével hello rendszer.
Ez a cikk azt szemlélteti, hogyan tooscript ez feldolgozható Powershell: <http://www.westerndevs.com/azure-snapshots/>

Ellenőrizze, hogy meg arról, hogy tooinstall új SAP licencet óta restoing egy virtuális gép biztonsági mentése fent leírt módon létrehoz egy új hardver kulcsot.

### <a name="online-backup-of-an-sap-system"></a>Az SAP rendszer online biztonsági mentés
Az adatbázis-kezelő hello biztonsági mentés az adott adatbázis-kezelő módszerekkel hello leírtak [DBMS útmutató][dbms-guide].

A többi virtuális gépen belül hello SAP rendszer készíthető funkciójával Azure virtuális gépek biztonsági mentéséhez. Az Azure virtuális gép biztonsági mentése korai 2015 kapott bevezetett és eközben egy szabványos metódus toobackup egy teljes Azure-ban. Azure biztonsági mentés hello biztonsági mentések Azure-ban tárolja, és lehetővé teszi, hogy a virtuális gépek visszaállítási újra.

> [!NOTE]
> Től Dec 2015 VM biztonsági mentéssel megőrzése nélkül hello egyedi virtuális gép azonosítója SAP használt licencelési. Ez azt jelenti, hogy a virtuális gép biztonsági másolat visszaállítása szükséges telepítése egy új SAP licenckulcs as hello vissza VM tekinthető toobe egy új virtuális Gépet, és nem helyettesíti a korábbi egy mentett.
> 2016. január Azure virtuális gép biztonsági mentése nem támogatja a virtuális gépek üzembe helyezett Azure Resourc Manager még.
>
> ![Windows][Logo_Windows] Windows
>
> Elméletileg virtuális gép futtatási adatbázisok biztonsági másolat készíthető következetesen, valamint ha hello adatbázis-kezelő rendszerek által támogatott Windows VSS hello (kötet árnyékmásolata szolgáltatás <https://msdn.microsoft.com/library/windows/desktop/bb968832 (v=VS.85).aspx megfelelő > ) mint pl. az SQL Server.
> Azonban figyelembe időpontban adatbázisok visszaállítása Azure virtuális gép biztonsági másolatok alapján, amelyek nem lehetséges. A javaslat ezért tooperform biztonsági másolatokat az adatbázisok adatbázis-kezelő funkciójú ahelyett, hogy az Azure virtuális gép biztonsági mentése
>
> Azure virtuális gépek biztonsági mentéséhez ismernie tooget indítsa el a itt: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Egyéb lehetőségek toouse Microsoft Data Protection Manager együttes telepítve egy Azure virtuális gép és egy Azure Backup szolgáltatás biztonsági mentési/visszaállítási adatbázisokhoz. További információk itt találhatók: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.  
>
> ![Linux][Logo_Linux] Linux
>
> Nincs egyenértékű tooWindows lévő Linux VSS van. Ezért csak a fájlkonzisztens biztonsági mentések is lehetséges, de nem alkalmazáskonzisztens biztonsági mentés. hello SAP DBMS biztonsági mentést kell végezni az adatbázis-kezelő szolgáltatással. hello fájlrendszer, köztük a hello SAP kapcsolatos adatokat is menthető pl. használatával bont lásd itt: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Az éles SAP tájak vész-Helyreállítási helyként Azure
Mid 2014 óta bővítmények toovarious összetevőket Hyper-V, a System Center és az Azure hello használatának engedélyezése Azure vész-Helyreállítási helyként helyszíni Hyper-v rendszerű virtuális gépekhez.

Egy blog, és részletesen leírja, hogyan toodeploy ehhez a megoldáshoz az itt dokumentált lehetőségektől: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## <a name="summary"></a>Összefoglalás
hello legfontosabb pontjait magas rendelkezésre állású SAP rendszerekhez az Azure-ban a következők:

* Ezen a ponton az időben, hello SAP hibaérzékeny pontok kialakulását nem védhető pontosan hello ugyanaz, mint a helyszíni telepítések esetén is elvégezhető. hello oka, hogy a megosztott lemezfürtöket nem még hozható létre az Azure-ban 3. fél szoftver hello használata nélkül.
* Hello DBMS réteg van szüksége, amely nem függ a megosztott lemez fürt technológia toouse adatbázis-kezelő funkcióit. Részletek ismertetett hello [DBMS útmutató][dbms-guide].
* toominimize hello hatás hello Azure infrastruktúra vagy a gazdagép karbantartási tartalék tartományok belül előforduló problémákról, az Azure rendelkezésre állási készletek kell használnia:
  * Ajánlott toohave egy rendelkezésre állási készlet hello SAP alkalmazásréteg.
  * Egy külön rendelkezésre állási készlet hello SAP DBMS réteg toohave ajánlott.
  * Virtuális gépek különböző SAP rendszerek azonos rendelkezésre állási készlet tooapply hello nem ajánlott.
* Biztonsági mentés céljából hello SAP DBMS réteg, tekintse meg az hello [DBMS útmutató][dbms-guide].
* SAP párbeszédpanel példányok biztonsági másolatának szabálykészletében kevés mert általában gyorsabb tooredeploy egyszerű párbeszédpanel példányok.
* Biztonsági mentés hello VM hello globális directory hello SAP rendszer és az azt tartalmazó összes hello-profil hello különböző példányok, célszerű és kell végezhető el a Windows biztonsági másolat vagy pl. Linux bont. Mivel a Windows Server 2008 (R2) és a Windows Server 2012 (R2), amelyek révén könnyebben toobackup közötti különbségek újabb hello használatával feloldja a Windows Server, azt javasoljuk, hogy a Windows Server 2012 (R2) Windows vendég operációs rendszerként toorun.
