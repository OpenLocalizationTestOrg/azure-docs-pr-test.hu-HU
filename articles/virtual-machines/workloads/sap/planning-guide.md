---
title: "Virtuális gépek aaaAzure tervezési és megvalósítási az SAP NetWeaver |} Microsoft Docs"
description: "Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d1e9fa13d847e4d8abb3c34fd352d1f4ece3717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-planning-and-implementation-for-sap-netweaver"></a>Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver
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

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription-limits

[dbms-guide]:dbms-guide.md
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
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

[deployment-guide]:deployment-guide.md
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
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
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

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

[planning-guide]:planning-guide.md  
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92

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
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/planning-monitoring-overview-2502.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-2901]:media/virtual-machines-shared-sap-planning-guide/2901-azure-ha-sap-ha-md.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-3201]:media/virtual-machines-shared-sap-planning-guide/3201-sap-ha-with-sql-md.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
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
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#step-2-capture-the-vm
[virtual-machines-windows-capture-image-resource-manager]:../../windows/capture-image.md
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:../../virtual-machines-windows-generalize-vhd.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-create-upload-vhd-oracle]:../../linux/oracle-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../linux/manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes-linux]:../../linux/sizes.md
[virtual-machines-sizes-windows]:../../windows/sizes.md
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
[virtual-networks-multiple-nics-windows]:../../windows/multiple-nics.md
[virtual-networks-multiple-nics-linux]:../../linux/multiple-nics.md
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
[capture-image-linux-step-2-create-vm-image]:../../linux/capture-image.md#step-2-create-vm-image

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

A Microsoft Azure lehetővé teszi a vállalatok tooacquire számítási és tárolási erőforrásokat minimális időben hosszadalmas beszerzési ciklusok nélkül. Az Azure virtuális gépek lehetővé teszik a vállalatok toodeploy klasszikus alkalmazások, például SAP NetWeaver-alapú alkalmazások az Azure, és a megbízhatóság és rendelkezésre állás kiterjesztése anélkül, hogy további erőforrás érhető el a helyszínen. Az Azure virtuális gép szolgáltatások közötti kapcsolatot nyújthassanak, mely lehetővé teszi a vállalatok tooactively Azure virtuális gépek integrálja a helyi tartomány, a Magánfelhők és az SAP rendszer fekvő is támogatja.
Ez a dokumentum ismerteti a hello alapjai a Microsoft Azure virtuális gép és biztosít egy SAP NetWeaver telepítések az Azure-ban tervezési és megvalósítási szempontok segédlet és ilyen kell hello dokumentum tooread elindítása előtt SAP NetWeaver Azure tényleges példányai.
hello papír kiegészítés hello SAP dokumentáció és az SAP, képviselő hello elsődleges erőforrások telepítése és a SAP szoftver központi telepítését az adott platformok.

## <a name="summary"></a>Összefoglalás
A felhőalapú informatika egy kifejezést gyakran használják, amely egyre több fontos hello informatikai iparági belül van való kis vállalatok toolarge és multinacionális cég vállalatok fel.

Microsoft Azure a hello Felhőplatform szolgáltatások a Microsofttól, mely egy új lehetőségek széles skáláját biztosítja. Most ügyfelek rendszer képes toorapidly kiépítése és deaktiválás rendelkezés alkalmazás hello felhőalapú szolgáltatásként, hogy ne korlátozott tootechnical vagy költségvetési korlátozások. Helyett befektetés időt és a hardver infrastruktúra, a vállalatok összpontosíthat hello alkalmazás, az üzleti folyamatokat és az előnye az ügyfelek és felhasználók.

A Microsoft Azure Virtual Machine Services szolgáltatással a Microsoft átfogó szolgáltatott infrastruktúra (IaaS) platformot kínál. Az Azure virtuális gépek támogatják az SAP NetWeaver-alapú alkalmazásokat (IaaS). E tanulmány ismerteti, hogyan tooplan és megvalósítása SAP NetWeaver-alapú alkalmazások a Microsoft Azure-ban hello platformként választott.

hello papír maga összpontosít két fő szempontjait:

* hello első része két támogatott üzembe helyezési mintát SAP NetWeaver alapú alkalmazásokhoz az Azure-on írja le. Azt is bemutatja az SAP üzemelő példányokkal figyelembe Azure általános kezelését.
* hello második része adatokat hello két különböző alkalmazási helyzetek hello első részében leírt megvalósítása.

További források című fejezet [erőforrások] [ planning-guide-1.2] ebben a dokumentumban.

### <a name="definitions-upfront"></a>Definíciók előzetes megfizetése esetén
Hello a dokumentum a következő feltételek hello használjuk:

* Infrastruktúra-szolgáltatási: Szolgáltatott infrastruktúra
* A PaaS: Platformok
* SaaS: Szolgáltatott szoftver
* ARM: Az Azure Resource Manager
* SAP összetevő: egy egyedi SAP alkalmazás, például ECC, a Sávszélesség, a megoldás Manager vagy a EP  SAP-összetevők a hagyományos ABAP vagy Java technológiák vagy egy nem NetWeaver alapú alkalmazás, például üzleti objektumok is alapulhat.
* SAP-környezetben: egy vagy több SAP összetevők logikailag egy csoportba tooperform például fejlesztési, a QAS, a képzési, a vész-Helyreállítási vagy a éles üzleti függvényt.
* SAP fekvő: Utal toohello egész SAP eszközök az ügyfél informatikai fekvő. hello SAP fekvő minden üzemi és nem éles környezetben tartalmaz.
* SAP-verzió: hello DBMS réteg és kombinációja alkalmazás réteget, például egy SAP ERP fejlesztőrendszer, SAP BW gazdagépes tesztrendszer, SAP CRM éles rendszer stb... Azure-környezetekhez, hogy a rendszer nem támogatott toodivide egyes két réteg között a helyszíni és az Azure. Ez azt jelenti, hogy egy SAP rendszer vagy a helyszínen telepített vagy telepítve van az Azure-ban. Telepíthet azonban hello különböző egy SAP fekvő Azure vagy a helyszíni rendszeren. Például sikerült hello SAP CRM fejlesztési és tesztelési az Azure-ban rendszerek központi telepítéséhez, de SAP CRM éles rendszer helyszíni hello.
* Csak felhőalapú telepítési: egy központi telepítést, amelyen keresztül a pont-pont nincs csatlakoztatva a hello Azure-előfizetés, vagy ExpressRoute kapcsolat toohello helyszíni hálózati infrastruktúra. Közös Azure dokumentációja az ilyen típusú központi telepítések egyaránt "Csak felhőalapú" telepítések leírása. Ezzel a módszerrel telepített virtuális gépek keresztül elért hello internet és egy nyilvános IP-címet és/vagy egy nyilvános DNS-nevet hozzárendelt toohello virtuális gépek Azure-ban. A Microsoft Windows hello a helyszíni Active Directory (AD), és a DNS nincs kibővítve az ilyen típusú központi telepítések tooAzure. Ezért hello virtuális gépek részei hello a helyszíni Active Directoryban. Is igaz Linux-használ, például OpenLDAP + Kerberos hitelesítés megvalósításához.

> [!NOTE]
> Ebben a dokumentumban csak felhőalapú központi telepítés meghatározott teljes SAP tájak kizárólag az Azure Active Directory kiterjesztés nélkül futnak / OpenLDAP vagy a nyilvános felhőbe helyszíni névfeloldás. Csak felhőalapú konfigurációk nem támogatottak a termelési SAP rendszerek vagy konfigurációk, ahol az SAP STM vagy más helyszíni erőforrásokat kell toobe használt Azure és a helyszínen található erőforrások üzemeltetett SAP-rendszerek között.
>
>

* Létesítmények közötti: Egy olyan forgatókönyvet, hol a virtuális gépek az Azure-előfizetéssel, amely rendelkezik pont-pont, többhelyes vagy hello helyszíni datacenter(s) és az Azure között ExpressRoute kapcsolat telepített tooan ismertet. Közös Azure dokumentációja, az ilyen típusú központi telepítések egyaránt létesítmények közötti forgatókönyv leírása. hello hello kapcsolat oka, hogy a helyi DNS az Azure, a tooextend helyi tartományoknak és a helyszíni Active Directory/OpenLDAP. hello helyszíni fekvő kiterjesztett toohello Azure eszközök hello előfizetés van. Ha ezt a bővítményt, hello virtuális gépek hello a helyi tartomány része lehet. Tartományi felhasználók hello helyszíni tartomány hello kiszolgálókat érhetnek el, és szolgáltatásokat futtathatja a virtuális gépek (például adatbázis-kezelő szolgáltatás). Telepített virtuális gépek a helyszíni és az Azure telepített virtuális gépek közötti kommunikációt és a névfeloldás lehetőség. Ez a legtöbb SAP eszközök toobe telepített várhatóan hello forgatókönyv. További információkért lásd: [ez] [ vpn-gateway-cross-premises-options] cikk és [ez][vpn-gateway-site-to-site-create].

> [!NOTE]
> Létesítmények közötti telepítések SAP rendszert futtató Azure virtuális gépek esetén a helyszíni-tartománybeli tagsággal SAP rendszerek éles SAP rendszerek támogatottak. Létesítmények közötti konfigurációk támogatottak a kijelzők telepítése, vagy végezze el az SAP tájak az Azure. Az Azure-ban futó hello teljes SAP fekvő szükség van a helyszíni tartományi és ADS/OpenLDAP tartozó virtuális gépek. Hello dokumentáció korábbi verzióiban megtartásról kapcsolatos hibrid-IT-forgatókönyvek, ahol hello kifejezés "Hibrid" hello valójában a létesítmények közötti hálózati kapcsolatot a helyszíni és az Azure közötti feltört eszköz. Továbbá, hello tényt, hogy az Azure virtuális gépek hello hello részét képezik a helyszíni Active Directory / OpenLDAP.
>
>

Néhány Microsoft dokumentációjában létesítmények közötti forgatókönyv különösen az adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ konfiguráció egy kicsit másképp ismerteti. Az SAP hello dokumentumokat hello esetben hello létesítmények közötti forgatókönyv éppen forrni kezd toohaving le egy pont-pont vagy titkos (ExpressRoute) kapcsolatot, valamint hello tény, hogy hello SAP fekvő elosztása a helyszíni és az Azure között.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Erőforrások
hello következő kiegészítő útmutatók érhetők el a hello témakör Azure SAP telepítések:

* [Az Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver (Ez a dokumentum)][planning-guide]
* [SAP NetWeaver Azure virtuális gépek központi telepítését][deployment-guide]
* [Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]

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
| [2069760] |Oracle Linux 7.x SAP telepítése és frissítése |
| [1597355] |A Linux rendszerhez használt lapozóterület javaslat |

Hello is olvasható [Állapotváltozás Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux tartalmazó összes SAP-jegyzetet.

Általános alapértelmezett korlátozások és az Azure-előfizetések maximális korlátai található [Ez a cikk][azure-subscription-service-limits-subscription].

## <a name="possible-scenarios"></a>A következő eljárások
SAP gyakran előforduló hello legtöbb létfontosságú alkalmazások belül vállalatok számára rendelkezésre álló. hello architektúra és az alkalmazások műveletek többnyire nagyon bonyolult, és győződjön meg arról, hogy megfelel a követelményeknek, a rendelkezésre állás és teljesítmény fontos.

Így a vállalat rendelkezik toothink gondosan arról, hogy mely alkalmazások futtathatja a kiválasztott hello felhőszolgáltatóként független nyilvános felhő környezetben.

SAP NetWeaver telepítéséhez lehetséges rendszertípusok-alapú alkalmazások nyilvános felhőben környezetekben alábbiakban találhatók:

1. Közepes méretű éles rendszerek esetén.
2. Fejlesztési rendszerek
3. Tesztelési rendszerek
4. Prototípus rendszerek
5. Tanulási / bemutató rendszerek

A sorrend toosuccessfully rendszerek központi telepítéséhez SAP Azure IaaS vagy infrastruktúra-szolgáltatási általában, fontos toounderstand hello jelentős és közötti különbségek hello hagyományos outsourcers vagy a szolgáltatók által kínált infrastruktúra-szolgáltatási ajánlatok. Mivel hello hagyományos szolgáltató vagy outsourcer alkalmazkodik infrastruktúra (hálózati, tárolási és a kiszolgáló típusa) toohello munkaterhelés egy ügyfél szeretne toohost, ehelyett hello ügyfél felelősségi toochoose hello jobb munkaterhelés IaaS telepítésekhez.

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

Az adatok legnagyobb található [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows].

Ne feledje, hogy a fenti hello hivatkozás szerepel korlátok hello felső határa. Ez nem jelenti azt, hogy hello korlátairól hello erőforrásokat, például IOPS biztosítható, hogy minden esetben. hello kivételek, ha olyan hello CPU és memória-erőforrások a kiválasztott Virtuálisgép-típussá. A SAP által támogatott hello VM típusú hello CPU és memória-erőforrások idő hello VM tartozó fenntartott, így elérhető bármely pontján.

hello Microsoft Azure platformon IaaS más platformokon, például a egy több-bérlős platformja. Ez azt jelenti, hogy a tárolási, hálózati és egyéb erőforrások megosztott bérlők között. Intelligens sávszélesség-szabályozás és a kvóta logikai használt tooprevent egy bérlő más bérlőket (zajos szomszédos) hello teljesítményét érintő drasztikus módon. Bár a programot az Azure-ban próbálja tookeep eltérések észlelt sávszélesség az alacsony, magas megosztott platformokhoz általában toointroduce nagyobb eltérések az erőforrás/sávszélesség számos ügyfél használt tooin, mint a helyszíni telepítések. Ennek köszönhetően Ön is szembesülhet perces toominute sávszélesség hálózati vagy tárolási i/o (hello kötet, valamint a késés) vonatkozó különböző szintjét. hello valószínűsége, hogy az SAP rendszer Azure szabályos-nál nagyobb eltérésekre hibába egy helyszíni rendszerben kell figyelembe venni toobe.

Utolsó lépése tooevaluate rendelkezésre állási követelményeinek. Ez akkor fordulhat elő, az, hogy az alapul szolgáló Azure-infrastruktúra hello tooget frissítése, valamint hello gazdagépeken futó virtuális gépek toobe újraindítása szükséges. Ezekben az esetekben meg azokon a gazdagépeken futó virtuális gépeket szeretné kell leállításra és újraindításra is. ilyen karbantartás ütemezése hello munkaidejében kiegészítő egy adott régióban történik, de viszonylag nagy hello lehetséges ablak néhány óra során, ami egy újraindul. Különböző technológiákkal belül hello Azure platformon, amely konfigurált toomitigate néhány vagy összes hello ilyen frissítések hatással van. A jövőbeli fejlesztések hello Azure platformon, adatbázis-kezelő, és a SAP alkalmazás tervezett toominimize hello hatása ilyen újraindul.

Ahhoz, toosuccessfully SAP Azure alakzatot, hello helyszíni SAP rendszer(ek) operációs rendszer, adatbázis, vagy központi SAP-alkalmazásokból szerepelnie kell a hello SAP Azure támogatási mátrix hello erőforrások hello Azure-infrastruktúra illeszkedni biztosít, és amely rendelkezésre állási SLA-k a Microsoft Azure kínál hello együttműködik. Ezekhez a rendszerekhez meghatározott van szüksége az alábbi két helyzetben hello egyik toodecide.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Csak felhőalapú - hello a függőségek nélkül az Azure virtuális gépek telepítéséhez a helyi felhasználói hálózati
![Az SAP bemutató vagy Azure szolgáltatásba telepített képzési forgatókönyv egyetlen VM][planning-guide-figure-100]

Ez a forgatókönyv része jellemzően oktatási anyag vagy bemutató rendszerek, ahol telepítve vannak minden hello összetevői SAP, és nem SAP szoftver egy virtuális belül. Éles SAP rendszerek nem támogatottak ebben a telepítési forgatókönyvben. Általában ez a forgatókönyv megfelel a követelményeknek hello:

* hello virtuális hello nyilvános hálózaton keresztül érhetők el. Közvetlen kapcsolattal hello virtuális gépek toohello belül futó hello alkalmazások a helyi hálózati vagy a tulajdonos hello demonstrációt vagy oktatási anyag tartalmát hello vállalat vagy hello vevő nincs szükség.
* Hello oktatási anyag vagy bemutató forgatókönyvet képviselő több virtuális gépek esetén a hálózati kommunikációt és a névfeloldás kell toowork hello virtuális gépek között. De hello beállítása a virtuális gépek közötti kommunikáció kell, hogy a virtuális gépek több készleteinek egymás mellett beavatkozás nélkül telepíthető elkülönített toobe.  
* Internetkapcsolattal az Azure-ban üzemeltetett virtuális gépek toohello hello végfelhasználói tooremote bejelentkezési szükség. Attól függően, hogy hello vendég operációs rendszer, Terminal Services/távoli asztali szolgáltatások vagy VNC ssh használt tooaccess hello VM tooeither hello képzési feladatok teljesítése vagy hello bemutatók végrehajtani. Ha az SAP 3200, 3300 & 3600 részleg például portok is elérhetővé tehető hello SAP alkalmazáspéldány elérhető bármely internethez csatlakoztatott asztalról.
* hello SAP rendszer(ek) (és VM(s)) jelentik a önálló Azure, amely csak a végfelhasználói hozzáférése nyilvános internetkapcsolatra van szükség, és nem szükséges egy kapcsolat tooother az Azure virtuális gépek esetén.
* SAPGUI és a böngésző vannak telepítve, és közvetlenül a virtuális gép hello.
* A gyors alaphelyzetbe állítani a virtuális gép toohello eredeti állapot és új központi telepítését az eredeti állapotban ismét szükség.
* A bemutató és a betanítási esetek hello esetben több virtuális gép, egy Active Directory amely vannak megvalósítani / OpenLDAP és/vagy a DNS szolgáltatásra szükség az egyes virtuális gépek.

![A virtuális gép egy bemutató vagy egy Azure-Felhőszolgáltatásban képzési forgatókönyv képviselő csoport][planning-guide-figure-200]

Fontos szem előtt, hogy a virtuális gép van hello minden hello tookeep beállítja párhuzamosan telepíteni kell toobe ahol hello VM nevét az egyes hello beállítása vannak hello azonos.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Létesítmények közötti - központi telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény abból a teljes mértékben történő integrálását hello a helyszíni hálózat
![A pont-pont kapcsolatban (létesítmények közötti) VPN][planning-guide-figure-300]

Ebben a forgatókönyvben a létesítmények közötti forgatókönyv számos lehetséges telepítési mintákat. Legegyszerűbb leírása egyes részei hello SAP fekvő a helyszíni és a SAP fekvő hello Azure-on futó lehet. Kell, hogy a végfelhasználók számára átlátszó hello SAP összetevők része az Azure-on futnak tény hello minden szempontját. Ezért hello SAP átviteli javítási rendszer (STM), RFC kommunikációs nyomtatás, biztonsági (például SSO) és hasonló zökkenőmentesen működjön hello SAP rendszerekhez az Azure-on. De hello létesítmények közötti forgatókönyv is egy olyan forgatókönyvet, ahol hello ügyfél tartomány Azure-ban futó hello teljes SAP fekvő és DNS kiterjeszthetőek az Azure ismerteti.

> [!NOTE]
> Ez a hello környezet, amely hatékony SAP rendszereket futtató támogatott.
>
>

Olvasási [Ez a cikk] [ vpn-gateway-create-site-to-site-rm-powershell] további információt a tooconnect a helyszíni hálózati tooMicrosoft Azure

> [!IMPORTANT]
> Azt is van szó létesítmények közötti forgatókönyv Azure és a helyszíni felhasználói telepítés között, azt keresi, az egész SAP rendszerek hello részletességű. Forgatókönyvek, amelyek *nem támogatott* létesítmények közötti forgatókönyv esetén:
>
> * Különböző központi telepítési módszer különböző rétegeinek SAP-alkalmazásokból fut. Hello DBMS réteg a helyszínen, de hello SAP alkalmazásréteg például fordítva vagy Azure virtuális gépeken telepített virtuális gépek futnak.
> * Azure-ban és egy helyszíni SAP réteg néhány összetevőt. Például a felosztás hello SAP alkalmazásréteg-hez és az Azure virtuális gépek közötti példányai.
> * SAP példányát egy olyan rendszert több Azure-régiókat felett futó virtuális gépeket eloszlásáról nem támogatott.
>
> hello e korlátozások oka, hogy egy SAP rendszerben, különösen hello az alkalmazáspéldányok és az SAP rendszer hello DBMS réteg között nagy teljesítményű nagyon alacsony késleltetésű hálózathoz hello követelménye.
>
>

### <a name="supported-os-and-database-releases"></a>Támogatott operációs rendszer és adatbázis-kiadások
* Microsoft server szoftver esetében az Azure virtuális gép szolgáltatásokat, ez a cikk szerepel a támogatott: <http://support.microsoft.com/kb/2721672>.
* Támogatott operációs rendszer kiadások, adatbázis rendszer verziókban támogatott Azure virtuális gép szolgáltatások SAP szoftver együtt vannak dokumentálva SAP Megjegyzés [1928533].
* SAP-alkalmazásokból és -verziókat támogatja a virtuális gép Azure-szolgáltatásokra ismertetett SAP Megjegyzés [1928533].
* Csak 64 bites bináris támogatott toorun önmagukban SAP forgatókönyvek esetén a Vendég virtuális gépek Azure-ban. Ez azt is jelenti, hogy csak a 64 bites SAP-alkalmazásokból és az adatbázisok támogatottak.

## <a name="microsoft-azure-virtual-machine-services"></a>Virtuális gép Microsoft Azure-szolgáltatások
hello Microsoft Azure platform egy internet-skálázási felhőalapú szolgáltatások platform üzemeltetett, és adatközpontok Microsoft adatok üzemeltetni. hello platform hello Microsoft Azure virtuális gép szolgáltatások (infrastruktúra-szolgáltatás, vagy IaaS) és a gazdag platformok egy Platformszolgáltatási képességek egy készletét tartalmazza.

hello Azure platformon csökkenti a kezdeti technológia hello szükségességét, és infrastruktúra a későbbi szoftvervásárlások. Megkönnyíti a fenntartása és működő alkalmazások, adja meg a számítási és tárolási toohost igény szerinti, méretezhető, és kezelheti a webalkalmazás és az egymáshoz kapcsolódó alkalmazások. Infrastruktúra-kezelés a automatizált egy platform, amely a magas rendelkezésre állás és dinamikus méretezés toomatch igényei árképzési használatalapú fizetési modell hello kapcsolóval.

![A Microsoft Azure virtuális gép szolgáltatások elhelyezéséhez][planning-guide-figure-400]

Az Azure virtuális gép Services Microsoft van így már toodeploy egyéni kiszolgálói lemezképek tooAzure (lásd a 4. ábra) IaaS-példányként. hello Azure virtuális gépek a Hyper-V virtuális merevlemezek (VHD) alapuló, és képes toorun különböző operációs rendszer, a vendég operációs rendszer.

Működési szempontból az Azure virtuális gép szolgáltatást kínál hasonló hello a helyszínen telepített virtuális gépek során lép. Hello jelentős előnye, hogy nincs szükség van, azonban tooprocure, felügyelhetik és kezelhetik hello infrastruktúrát. A fejlesztők és rendszergazdák jogköre teljes hello operációsrendszer-lemezkép a virtuális gépekről. A rendszergazdák jelentkezhetnek be távolról toothose virtuális gépek tooperform karbantartási és a feladatokat, valamint a szoftverfrissítések központi telepítésének feladatai hibaelhárítási. A legutóbb toodeployment a hello csak korlátozások vonatkoznak a hello méretű és az Azure virtuális gépek képességeit. Ezek nem lehet a finom konfigurációban részletes, mivel ezt megteheti a helyszínen. A Virtuálisgép-típusokon kombinációját jelentik választási lehetőség van:

* Vcpu, száma
* Memória
* Csatolt, VHD-k száma
* Hálózati és tárolási sávszélesség.

hello mérete és a különböző virtuális gépek különböző méretű korlátozások érhető el egy olyan táblázatában látható [Ez a cikk (Linux)] [ virtual-machines-sizes-linux] és [Ez a cikk (Windows)] [ virtual-machines-sizes-windows].

Vegye figyelembe, hogy számos különböző családok vagy azokat a virtuális gépek. A következő virtuális gépek családok hello segítségével különböztetheti meg egymástól:

* VM a0-A7 csomag típusok: csak az SAP hitelesít. Első Virtuálisgép-sorozat Azure IaaS bevezetett kapott.
* VM a8-A11 csomag típusok: nagy teljesítményű számítástechnikai példányok. Különböző jobb végrehajtása futtatásának gazdagépek, mint más A-sorozatú virtuális gépek számítási.
* D/Dv2-sorozat Virtuálisgép-típusokon: A0-A7-nál nagyobb végrehajtása. Az SAP hitelesített összes hello Virtuálisgép-típusokon.
* DS/DSv2-méretek Virtuálisgép-típusokon: tooD/Dv2-sorozat hasonló, de azok képes tooconnect tooAzure prémium szintű Storage (című [prémium szintű Azure Storage] [ planning-guide-3.3.2] a jelen dokumentum). Újra nem minden virtuális gép esetében az SAP hitelesít.
* G-sorozat Virtuálisgép-típusokon: felső memóriaterület Virtuálisgép-típusokon.
* GS sorozatnak Virtuálisgép-típusokon: G-sorozat hasonló, de beleértve hello beállítás toouse prémium szintű Azure Storage (című [prémium szintű Azure Storage] [ planning-guide-3.3.2] a jelen dokumentum). Adatbázis-kiszolgálók GS sorozatnak virtuális gépeket használ, esetén kötelező toouse prémium szintű Storage DB adatok és a tranzakciós naplófájlok

Azt tapasztalhatja hello azonos Processzor- és konfigurációk másik Virtuálisgép-sorozat. Mindazonáltal, ha meg hello átviteli teljesítményt hello másik adatsorozathoz azok jelentősen eltérhetnek kívüli virtuális gépeken. Annak ellenére, hogy hello azonos Processzor- és konfigurációs. Oka az, hogy hello alapjául szolgáló kiszolgáló gazdahardverre: hello bemutatása hello eltérő típusú virtuális gép volt-e a különböző átviteli jellemzőit.  Általában hello különbség látható teljesítménye szempontjából is megjelenik hello hello ára különböző virtuális gépek.

Nem minden más Virtuálisgép-sorozat előfordulhat, hogy lehet érhető el a hello Azure-régiókat (az Azure-régiókat lásd a következő fejezet). Ügyeljen arra is, hogy nem minden virtuális gép vagy Virtuálisgép-sorozat hitelesített SAP.

> [!IMPORTANT]
> SAP NetWeaver-alapú alkalmazások hello használatát, a Virtuálisgép-típusokon és konfigurációk csak hello részhalmazát felsorolt SAP Megjegyzés [1928533] támogatottak.
>
>

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure-régiók
Microsoft lehetővé teszi a virtuális gépek toodeploy az úgynevezett "Azure-régiók". Egy Azure-régiót lehet egy vagy több olyan adatközpontokban közel található. A legtöbb hello geopolitikai régiók a hello world Microsoft van legalább két Azure-régió. Például az Európai nincs "Észak-Európa" és "Nyugat-Európában" egy az Azure-régióban. Ilyen geopolitikai régión belül két Azure-régiókat elválasztott elég jelentős távolság, hogy a természetes vagy technikai katasztrófák nem befolyásolják a hello mindkét Azure-régiókat geopolitikai ugyanabban a régióban. Microsoft folyamatosan globálisan fejleszt kimenő geopolitikai különböző régiókban új Azure-régiókat, mert a frissítésétől Dec 2015 elérte a már közzétett további régiókban 20 Azure régiók hello számát, és ezek a területek hello száma folyamatosan növekvő. Ügyfélként SAP rendszerek telepíthetők területekre összes ezeket, beleértve a két hello Azure-régiókat Kínában. Azure-régiók aktuális naprakész információkat lásd: a webhely: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>a Microsoft Azure virtuális gép koncepció hello
A Microsoft Azure kínál az infrastruktúra, a szolgáltató (IaaS) megoldás toohost virtuális gépek hasonló funkciókkal rendelkező helyszíni virtualizálási megoldásaként. Áll képes toocreate származó virtuális gépek belül hello Azure-portálon PowerShell vagy a parancssori felület, amely is kínál a központi telepítési és felügyeleti képességek.

Az Azure Resource Manager lehetővé teszi tooprovision olyan deklaratív sablonok segítségével az alkalmazások. Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt. Hello használata azonos sablon toorepeatedly az alkalmazást, minden szakasza hello alkalmazás-életciklus helyezi üzembe.

Resource Manager-sablonok használatával kapcsolatos további információkat talál itt:

* [Telepítését és kezelését a virtuális gépek Azure Resource Manager-sablonok és hello Azure parancssori felület használatával] [.. /.. / linux/create-ssh-secured-vm-from-template.md]
* [Virtuális gépeket Azure Resource Manager és a PowerShell használatával][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

Egy másik érdekes szolgáltatása hello képességét toocreate képek a virtuális gépekről, amellyel tooprepare bizonyos tárházak találhatók, amelynek tudja tooquickly elő központi telepítése virtuálisgép-példányok, amelyek megfelelnek az elvárásainak.

A virtuális gépekről lemezképek létrehozásával kapcsolatos további információért megtalálhatók [Ez a cikk (Linux)] [ virtual-machines-linux-capture-image-resource-manager] és [Ez a cikk (Windows)] [ virtual-machines-windows-capture-image-resource-manager].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Tartalék tartományok
Tartalék tartományok határoz meg a hiba, szorosan kapcsolódó toohello fizikai infrastruktúra adatközpontok, és amíg a fizikai panel vagy állvány lehet tekinteni a tartalék tartomány található fizikai egység, nincs közvetlen egy az egyhez típusú megfeleltetés hello két között.

Több virtuális gép egy SAP rendszer a Microsoft Azure virtuális gép szolgáltatás részeként történő telepítésekor befolyásolhatja hello Azure Fabric Controller toodeploy az alkalmazás különböző tartalék tartományok, ezáltal követelményeinek hello hello A Microsoft Azure SLA-t. Azonban hello tartalék tartományok terjesztése egy Azure méretezési egységhez (akár több százszor is a számítási csomópontok vagy a tárolási csomópontokat és a hálózatkezelés gyűjteményét) keresztül vagy a virtuális gépek tooa hello hozzárendelése adott tartalék tartomány valami keresztül, amely nem rendelkezik közvetlen vezérlő. Rendelés toodirect hello Azure-hálót vezérlő toodeploy keresztül különböző tartalék tartomány virtuális gépek halmaza kell tooassign egy Azure rendelkezésre állási csoport toohello virtuális gépek központi telepítéskor. Azure rendelkezésre állási csoportokra vonatkozó további információkért lásd: fejezet [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] ebben a dokumentumban.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Frissítési tartományok
Frissítési tartományt határoz meg egy logikai egységet, amelyek segítenek az SAP rendszerben, alkotó SAP-példány a több virtuális gép fut, a virtuális gépek frissítésének toodetermine. Frissítés esetén a Microsoft Azure végighalad hello a frissítési tartományok egyenként frissítésének folyamatát. Virtuális gépek központi telepítéskor a különböző frissítési tartományok keresztül terjednek, megvédheti a SAP rendszer részben a várható állásidő. A sorrend tooforce Azure toodeploy hello virtuális gépek különböző frissítési tartományok eloszlatva SAP a rendszer, a központi telepítéskor az egyes virtuális gépek tooset a specifikus attribútummal van szüksége. Hasonló tooFault tartományok, egy Azure méretezési egységhez több frissítési tartományt van osztva. Rendelés toodirect hello Azure-hálót vezérlő toodeploy keresztül különböző frissítési tartományok virtuális gépek halmaza kell tooassign egy Azure rendelkezésre állási csoport toohello virtuális gépek központi telepítéskor. Azure rendelkezésre állási csoportokra vonatkozó további információkért lásd: fejezet [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] alatt.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Az Azure rendelkezésre állási csoportok
Azure virtuális gépeken belül egy Azure rendelkezésre állási csoport hello Azure Fabric Controller különböző tartalék és frissítési tartományok terjeszti. hello hello terjesztési különböző tartalék és frissítési tartományok célja az SAP-rendszer nem minden virtuális gép leállítása hello kis-és infrastruktúra karbantartása vagy egy belül egy tartalék tartomány tooprevent. Alapértelmezés szerint virtuális gépek részei egy rendelkezésre állási csoportban. hello tulajdonság részvételét a virtuális gépek rendelkezésre állási csoport a központi telepítéskor vagy későbbi egy újrakonfigurálása és a virtuális gépek ismételt telepítése által definiált.

olvassa el a rendelkezésre állási csoportokra vonatkoznak, tooFault és a frissítési tartományok úgy Azure rendelkezésre állási készletek és hello toounderstand hello fogalma [Ez a cikk][virtual-machines-manage-availability]

a json-sablon használatával ARM toodefine rendelkezésre állási készletek lásd: [rest-api specifikáció hello](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) , és keressen az "availability".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Tárolás: A Microsoft Azure Storage és adatlemezek
Microsoft Azure virtuális gépek hasznosítani különböző típusú tárolókat. SAP megvalósítás a virtuális gép Azure-szolgáltatásokra, esetén fontos toounderstand hello különbségek tárolási két fő típusok között:

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
> * <https://docs.microsoft.com/Azure/Storage/Storage-About-Disks-and-vhds-Linux#Temporary-Disk>
>
>

- - -
hello valódi meghajtó "volatile", mert első maga hello gazdagép-kiszolgálón. Ha hello virtuális gép áthelyezését egy újbóli üzembe helyezése (például esedékes toomaintenance hello állomás vagy leállítása és újraindítása) hello meghajtó hello tartalom elvész. Emiatt nincs egy beállítás toostore ezen a meghajtón fontos adatokról. az ilyen típusú tárolási használt eszköz típusa hello nagyon különböző teljesítményt nyújt, amely 2015 júniusában frissítésétől tűnik a másik Virtuálisgép-sorozat eltérnek:

* A5-A7: Erősen korlátozott teljesítményét. Nem ajánlott a semmit túl az oldal fájlja
* A8-A11: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* D sorozatú: Majd ezer IOPS nagyon jó teljesítmény jellemzőit és > 1GB/s átviteli sebesség.
* DS-méretek: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* G-sorozat: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.
* GS sorozatnak: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség.

A fenti utasítások alkalmazzák a SAP hitelesített toohello Virtuálisgép-típusokon. használja ki az egyes adatbázis-kezelő szolgáltatásai által jogosultak hello Virtuálisgép-sorozat kiváló IOPS és átviteli sebesség. További információkért lásd: hello [adatbázis-kezelő telepítési útmutatója][dbms-guide].

A Microsoft Azure tárolás megőrzött tárolási és hello tipikus szintű védelmet és San-alapú tárolón látható redundanciát biztosít. Azure Storage alapján lemezek a virtuális merevlemez (VHD) hello Azure Storage szolgáltatásban található. hello helyi operációsrendszer-lemez (Windows C:\, Linux/dev/sda1) tárolt hello Azure Storage és a további kötetek/lemezek csatlakoztatott toohello VM lekérése tárolt, túl.

Lehetséges tooupload egy meglévő VHD-t a helyszíni vagy hozzon létre üres néhányat a meglévők közül az Azure-ban, és csatlakoztassa a virtuális gépek toodeployed.

Miután létrehozott vagy a virtuális merevlemez feltöltése az Azure Storage lehetséges toomount, és csatolja azokat meglévő virtuális gép és a meglévő (nem) VHD toocopy tooan.

Ezen a virtuális merevlemezek megmaradnak, adatokat és változásokkal azokat a rendszer biztonságos, amikor újraindul, és újra létrehozni a virtuálisgép-példányt. Akkor is, ha a példány törlését a VHD-k biztonságban és is újra kell telepíteni, vagy nem operációsrendszer lemezek esetén lehet csatlakoztatott tooother virtuális gépek.

Belül hello Azure Storage különböző redundancia szintjének hálózati konfigurálható:

* Minimális választható szintje "helyi redundanciát", amely egyenértékű toothree-replika hello hello adatainak ugyanabban az adatközpontban egy Azure-régióhoz (című [Azure-régiókat][planning-guide-3.1]).
* Zóna redundáns tárolás, mely férgek hello három képek keresztül belül különböző adatközpontokban hello egy Azure-régióban.
* Alapértelmezett redundanciájának szintje földrajzi redundancia céljából, ami aszinkron módon replikál hello tartalma a másik három képek hello adatok be egy másik Azure azon régióját, lévő hello geopolitikai ugyanabban a régióban.

Is láthatja, hogy ez a cikk vonatkozó hello különböző redundancia fölött hello tábla: <https://azure.microsoft.com/pricing/details/storage/>

Azure Storage-ról további információt itt található:

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/Site-Recovery>
* <https://docs.microsoft.com/REST/API/storageservices/Understanding-Block-Blobs--append-Blobs--and-Page-Blobs>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-Disk-Encryption-for-Linux-and-Windows-Virtual-machines-Public-Preview.aspx>

#### <a name="azure-standard-storage"></a>Az Azure standard szintű tárolót
Azure szabványos állt hello a tároló típusa szerinti elérhető Azure IaaS kiadásakor. Nem voltak IOPS kvóta egyetlen lemezenként. Tapasztalt késést azonos SAN/NAS-eszközökön általában telepített csúcskategóriás SAP rendszerekhez helyben tárolt osztályt hello volt. Ettől függetlenül az Azure standard szintű tárolást bizonyult elegendő-e több száz hello SAP rendszerek időközben telepítve az Azure-ban.

A szabványos Azure Storage-fiókok tárolt lemezek van szó, hello tényleges tárolt adatokat, hello mennyisége storage-tranzakció, a kimenő adatátviteli és a választott redundancia beállítás alapján. Sok: hello legfeljebb 1 TB méretű is létrehozható, de mindaddig, amíg azok továbbra is üres használata díjmentes. Ha ezután töltse ki egy virtuális Merevlemezt 100GB, 100GB tárolására, nem pedig a VHD készült hello névleges méret hello van szó.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Prémium szintű Azure Storage
A 2015. áprilisi Microsoft bevezette a prémium szintű Azure Storage. Prémium szintű Storage-ban készült bevezetett hello cél tooprovide:

* Jobb késleltetéséről.
* Nagyobb átviteli sebesség.
* A i/o-késés kevesebb variancia.

Erre a célra sok módosítások jelentek meg, mely hello két legfontosabb vannak:

* SSD-lemezek hello Azure Storage-csomópontok használatát
* Egy új olvasási gyorsítótár által támogatott hello egy Azure számítási csomópont helyi SSD

Ha nem változtatta képességek ellentétes tooStandard tárolási hello lemez (vagy VHD), hello méretétől függ prémium szintű Storage jelenleg rendelkezik három különböző lemez kategóriák, ez a cikk látható: <https://azure.microsoft.com/ árképzési/részletei / / nem felügyelt-tárolólemezek />

Láthatja, hogy IOPS/lemez és a lemez átviteli/lemez hello mérete kategória hello lemezek függ

Prémium szintű Storage esetében hello költség alapja nincs hello tényleges adatmennyiség ilyen lemezek, de ilyen lemez hello adatmennyiség hello hello lemezen tárolt független kategóriáját hello mérete a tárolt.

Lemez a prémium szintű Storage, amely nem közvetlen leképezése látható hello mérete kategóriákba hozható létre. Erre akkor lehet hello eset, különösen akkor, ha a lemezek másolásának szabványos tárolásból a prémium szintű tároló. Ebben az esetben a leképezési toohello következő legnagyobb prémium szintű Storage lemez használata történik.

Vegye figyelembe, hogy csak bizonyos Virtuálisgép-sorozat a prémium szintű Azure Storage hello kihasználhassa. Től Dec 2015-öt ezek a hello DS - és GS-méretek. hello DS sorozatnak van alapvetően hello azonos D sorozatú hello kivétellel, hogy a DS sorozatnak rendelkezik hello képességét toomount prémium szintű Storage-alapú virtuális gépek, továbbá az Azure standard szintű tárolást toodisks. Ugyanaz a G-sorozat képest tooGS-adatsorozathoz érvénytelen.

Vannak-e kikérése hello DS sorozatnak virtuális gépeinek hello részét [Ez a cikk (Linux)] [ virtual-machines-sizes-linux] és [Ez a cikk (Windows)][virtual-machines-sizes-windows], még megvalósítható amelyek nincsenek adatok mennyiségi korlátozásai tooPremium tárolólemezeket hello granularitása hello VM szint. Különböző DS-méretek és GS sorozatnak virtuális gépeken is különböző korlátozásokkal rendelkezik lehet csatlakoztatni adatok lemezek számát illetően toohello. Ezek a korlátozások, valamint a fent említett hello cikkben szerepelnek. De lényegében azt jelenti, hogy, például csatlakoztatása 32 x P30 lemezek tooa virtuális DS14 nem kaphat 32 x hello P30 lemez maximális átviteli sebesség. Ehelyett hello maximális átviteli sebesség a virtuális gép szinten dokumentált módon hello cikk korlátok adatátvitelt.

Prémium szintű Storage további információk itt találhatók: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="c55b2c6e-3ca1-4476-be16-16c81927550f"></a>Felügyelt lemezek
Felügyelt lemezek olyan új erőforrást az Azure Resource Manager az Azure Storage-fiókok tárolt VHD-k helyett használható. Felügyelt lemezek automatikusan igazodni hello azok hello virtuális gép rendelkezésre állási csoport csatolt tooand ezért növekedése hello rendelkezésre állását a virtuális gép és a hello hello virtuális gépen futó szolgáltatásokat. További információkért olvassa el a hello [a cikk áttekintése](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

Tooyou használata felügyelt lemezes, azt javasoljuk, mert egyszerűsítése érdekében a hello üzembe helyezési és a virtuális gépek felügyeletét.
SAP jelenleg csak prémium felügyelt lemezeit támogatja. További információkért olvassa el az SAP Megjegyzés [1928533].

#### <a name="azure-storage-accounts"></a>Azure Storage-tárfiókok
Szolgáltatások és az Azure virtuális gépeken való telepítése virtuális merevlemezek és a Virtuálisgép-lemezképek központi telepítése Azure Storage-fiókok nevezett egységekben rendszerezheti. Az Azure központi telepítésének tervezésekor toocarefully kell fontolja meg az Azure hello korlátozásait. Hello egyik oldalon Storage-fiókok korlátozott számú Azure előfizetésenként van. Bár minden Azure Storage-fiók nagyszámú VHD-fájlokat képes tárolni, nincs-e a rögzített korlát hello a teljes iops-értéket a Tárfiók. SAP virtuális gépek több száz jelentős IO hívások létrehozása DBMS rendszerekkel való telepítése esetén ajánlott toodistribute magas IOPS DBMS VMs több Azure Storage-fiókok között. Gondot kell fordítani a nem tooexceed hello aktuális korlát az Azure Storage-fiókok előfizetésenként. Tárolót, mert az SAP rendszer hello adatbázis központi telepítésének fontos része, a fogalom tárgyalja részletesen hello már hivatkozik a [adatbázis-kezelő telepítési útmutatója][dbms-guide].

Azure Storage-fiókokról további információ található a [Ez a cikk][storage-scalability-targets]. A cikk elolvasása, vegye figyelembe, hogy hello korlátozások Azure standard szintű Storage-fiókok és a prémium szintű Storage-fiókok közötti különbségek vannak. Fő különbségek hello ilyen egy Tárfiókon belül tárolt adatok mennyiségét. Standard szintű tárolót a hello kötet egy nagyobb, mint a prémium szintű Storage nagyságrenddel. Hello a másik oldalon, Standard szintű Tárfiók hello súlyosan korlátozott iops-érték (lásd a "Teljes kérelmek gyakorisága" oszlop), mivel hello Azure prémium szintű Tárfiók nincs ilyen korlátozás van érvényben. Ismertetjük részletek és eltérések eredmények hello telepítések SAP rendszerek, különösen a hello DBMS kiszolgálókon ismertetésekor.

A Tárfiókon belül van hello lehetőségét toocreate különböző tárolók hello célra történő szervezése és különböző virtuális merevlemezek kategorizálását. Ezek a tárolók általában szolgálnak, például különálló virtuális merevlemezeket különböző virtuális gépek. Nincsenek nem teljesítményre gyakorolt hatása, csak egy tárolót vagy több tároló alatt egy Azure Storage-fiók használatával.

Azure-ban egy virtuális merevlemez neve követi hello elnevezési kapcsolat, amelyet a tooprovide hello Azure-ban virtuális merevlemez egyedi nevet a következő:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

A fenti említett hello karakterláncból kell toouniquely azonosítása hello Azure Storage a tárolt virtuális merevlemez.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>A Microsoft Azure hálózatkezelés
A Microsoft Azure biztosít egy hálózati infrastruktúra, amely lehetővé teszi azt szeretnénk, ha minden forgatókönyvben hello leképezése toorealize SAP szoftverrel. hello funkciók a következők:

* A hozzáférést a hello közvetlenül a virtuális gépek, a Windows Terminálszolgáltatások vagy ssh/VNC toohello kívül
* Hozzáférés tooservices és az adott alkalmazások belül hello virtuális gépek által használt portok
* Belső kommunikációs és a névfeloldás között egy központilag telepített Azure virtuális gépek csoportján
* Létesítmények közötti kapcsolat egy ügyfél a helyszíni hálózat és hello Azure-hálózat között
* Több Azure-régióban vagy az adatközpontok Azure helyek közötti kapcsolat

További információk itt találhatók: <https://azure.microsoft.com/documentation/services/virtual-network/>

Nincsenek számos különböző lehetőségek tooconfigure neve és IP-megoldás az Azure-ban. Ebben a dokumentumban csak felhőalapú forgatókönyvek az Azure DNS-sel (az ezzel szemben toodefining egy saját DNS-szolgáltatás) hello alapértelmezett támaszkodnak. Egy új Azure DNS-szolgáltatás, amely helyett a saját DNS-kiszolgáló beállításához használható is van. További információ található [Ez a cikk] [ virtual-networks-manage-dns-in-vnet] és a [ezen a lapon](https://azure.microsoft.com/services/dns/).

Létesítmények közötti forgatókönyvek esetén azt is hagyatkoznia hello tényt, hogy hello a helyszíni AD/OpenLDAP/DNS-megtörtént-e VPN- vagy titkos kapcsolat tooAzure keresztül. Az egyes forgatókönyvek szerint itt dokumentált lehetőségektől, előfordulhat, hogy telepítette az Azure AD/OpenLDAP replikájának szükséges toohave.

Mivel a hálózat és névfeloldás hello adatbázis egy SAP rendszer központi telepítésének fontos része, a fogalom hello részletesen tárgyalt [adatbázis-kezelő telepítési útmutatója][dbms-guide].

##### <a name="azure-virtual-networks"></a>Azure virtuális hálózatok
Egy Azure virtuális hálózat kiépítésével hello címtartomány hello Azure DHCP-funkciók által kiosztott magánhálózati IP-címeket adhat meg. A létesítmények közötti forgatókönyv hello IP-címtartomány definiált még hozzá van rendelve DHCP használatával az Azure-ban. Tartomány névfeloldás azonban helyszíni (feltéve, hogy, hogy hello virtuális gépek egy helyszíni tartomány része) történik, és ezért oldhatja címek különböző Azure-Felhőszolgáltatások túl.

Minden virtuális gép Azure igények toobe tooa virtuális hálózati kapcsolat.

További részletek találhatók [Ez a cikk] [ resource-groups-networking] és a [ezen a lapon](https://azure.microsoft.com/documentation/services/virtual-network/).

[comment]: <> (MShermannd teendők található olyan cikk, amelyet a program hello OpenLDAP témakör + ARM;)
[comment]: <> (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [!NOTE]
> Alapértelmezés szerint a virtuális gépek telepítése után hello virtuális hálózat a konfiguráció nem módosítható. hello TCP/IP-beállítások toohello Azure DHCP-kiszolgáló kell hagyni. Alapértelmezett viselkedés a dinamikus IP-hozzárendelés.
>
>

hello hello virtuális hálózati kártya MAC-címet módosíthatja, például után átméretezési és a hello Windows vagy Linux vendég operációs rendszer szerzi be hello új hálózati kártya, és automatikusan használja ebben az esetben a DHCP-tooassign hello IP-cím és DNS címek.

##### <a name="static-ip-assignment"></a>Statikus IP-hozzárendelés
Rögzített lehetséges tooassign vagy fenntartott IP-címek tooVMs egy Azure virtuális hálózaton belül. Egy nagyszerű lehetőséget tooleverage hello virtuális gépeket futtató Azure virtuális hálózatban megnyitja ezt a funkciót, ha szükséges, vagy bizonyos esetekben szükséges. hello IP-hozzárendelés egész virtuális gép független hello virtuális gép fut-e, vagy -leállítás hello hello megléte érvényes marad. Ennek eredményeképpen kell tootake virtuális gépek (fut, és a leállított virtuális Gépeken) teljes számát figyelembe hello hello az IP-címek a virtuális hálózati hello meghatározásakor. hello IP-cím megmarad, hello virtuális gép és a hálózati illesztő nem törlik, vagy amíg hello IP-cím hozzárendelése újra deszerializálni lekérdezi. További információkért olvassa el a [Ez a cikk][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Több hálózati adapter virtuális gépenként
Egy Azure virtuális gépen a több virtuális hálózati adapterrel (vNIC) adhat meg. Hello képességét toohave több vNICs el lehet indítani tooset fel a hálózati forgalom elkülönítése, például ügyfélforgalmat áthalad egy vNIC és háttér forgalmat egy második virtuális hálózati keresztül történik. A különböző korlátozás VM hello típusú függ vNICs toohello számának tekintetében. Ezek a cikkek megtalálhatók pontos részleteit, funkcióit és korlátozások:

* [Több hálózati adapterrel rendelkező Windows virtuális gép létrehozása][virtual-networks-multiple-nics-windows]
* [Több hálózati adapterrel rendelkező Linux virtuális gép létrehozása][virtual-networks-multiple-nics-linux]
* [Sablon használatával több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-template]
* [PowerShell-lel több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-ps]
* [Hello Azure parancssori felület használatával több hálózati adapter virtuális gépek telepítése][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Hely-hely kapcsolatot
Létesítmények közötti Azure virtuális gépek és a helyszíni átlátható és állandó VPN-kapcsolat csatolva. Várt toobecome hello leggyakoribb SAP telepítési minta az Azure-ban. hello feltételezése, hogy a műveleti eljárások és a folyamatok SAP osztályt az Azure-ban transzparens módon kell működnie. Ez azt jelenti, hogy meg kell kell tudni tooprint kívül ezek a rendszerek, valamint SAP átviteli felügyeleti rendszer (TMS) tootransport módosításait fejlesztési rendszerről Azure tooa gazdagépes tesztrendszer telepített helyszíni hello használata. Pont-pont körül további dokumentációjában található [Ez a cikk][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>VPN-alagút eszköz
A rendezés toocreate webhelyek kapcsolatot (a helyszíni adatok center tooAzure adatközpont), tooeither kell beszerzése és konfigurálja a VPN-eszköz vagy az Útválasztás és távelérés szolgáltatás (RRAS) operációs rendszerben megjelent a Windows Server szoftver összetevőjeként 2012.

* [Virtuális hálózat létrehozása a PowerShell használatával pont-pont VPN-kapcsolattal][vpn-gateway-create-site-to-site-rm-powershell]
* [VPN-eszközökről a webhelyek közötti VPN átjáró kapcsolatok][vpn-gateway-about-vpn-devices]
* [VPN Gateway – gyakori kérdések][vpn-gateway-vpn-faq]

![A helyszíni és az Azure közötti pont-pont kapcsolat][planning-guide-figure-600]

hello a fenti ábrán látható, két Azure-előfizetések használatra fenntartva az IP-cím résztartomány rendelkezik az Azure virtuális hálózatairól. hello hello kapcsolat a helyszíni hálózati tooAzure létrejön a VPN-en keresztül.

#### <a name="point-to-site-vpn"></a>Pont – hely típusú VPN
Pont – hely típusú VPN minden ügyfél gép tooconnect a saját virtuális Magánhálózatokkal az Azure van szükség. Hello SAP forgatókönyvek azt láthatjuk pont-hely kapcsolatot nincs gyakorlati. Ezért semmilyen további hivatkozások adta toopoint-site VPN-kapcsolatot.

További információk itt találhatók
* [Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása az Azure portál használatával hello](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása PowerShell használatával](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

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
* Ha egy virtuális gép van telepítve, már nem lehetséges toochange hello virtuális hálózati hozzárendelt toohello méretű VM

### <a name="quotas-in-azure-virtual-machine-services"></a>A virtuális gép az Azure-szolgáltatások kvóták
Igazolnia kell toobe törléséhez kapcsolatos hello tény hello tárolási és hálózati infrastruktúra megosztott hello Azure-infrastruktúra számos szolgáltatáshoz futó virtuális gépek között. És hasonlóan hello ügyfél saját adatközpontok, néhány hello infrastruktúrához kapcsolódó erőforrások túlzott kiosztása hely tooa fok. a Microsoft Azure Platform hello lemez, Processzor, hálózati, és más kvóták toolimit hello erőforrás-felhasználás és toopreserve következetes és determinisztikus teljesítmény használja.  hello különböző Virtuálisgép-típusokon (A5, a6 méretű, stb.) eltérő kvóták hello lemezeinek száma miatt, CPU, RAM, és a hálózati.

> [!NOTE]
> SAP által támogatott hello VM típusok a CPU és memória-erőforrások előre lefoglalt hello állomás csomópontján. Ez azt jelenti, hogy ha hello virtuális gép van telepítve, hello gazdagép hello erőforrásainak elérhetők a Virtuálisgép-típussá hello által definiált.
>
>

Ha tervezési és méretezéssel SAP Azure megoldásokról hello minden virtuális gép méretét kvótái figyelembe kell venni. ismerteti a hello VM kvóták [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows].

hello kvóták leírt hello elméleti maximális értékeket jelölik.  hello legfeljebb iops-érték lemezenként kis IOS (8 KB-os) érhető el, de valószínűleg nem lehet elérni a nagy IOs (1Mb).  hello IOPS-korláttal megvalósul hello granularitási egyetlen lemezen.

A nyers döntési fa toodecide, Azure virtuális gép szolgáltatások és platformképességei illeszkedik az SAP rendszer vagy egy meglévő rendszer toobe rendszerben rendelés toodeploy hello Azure, másként konfigurálva kell hello döntési fa alábbi használhatók:

![Döntési fa toodecide képességét toodeploy SAP, az Azure-on][planning-guide-figure-700]

**1. lépés**: hello legfontosabb adatokat toostart rendelkező egy adott rendszerhez SAP hello SAP követelmény. hello SAP követelményeket kell toobe rétegekben hello DBMS és hello SAP alkalmazás részét, még akkor is, ha hello SAP rendszer már van telepítve a 2-réteg konfigurációjához a helyszíni. A meglévő rendszerek esetében SAP kapcsolódó toohello a hardver gyakran meghatározhatók vagy becsült hello meglévő SAP-referenciaalapok alapján. hello eredmények itt található: <http://global.sap.com/campaigns/benchmark/index.epx>.
Az újonnan telepített SAP rendszerek kell megtettünk mindent keresztül egy méretezési gyakorlatban kell meghatározni, hogy a rendszer hello hello SAP követelményeinek.
Lásd még: Ebben a blogban és az SAP méretezési csatolt dokumentumot Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**2. lépés**: A meglévő rendszerek hello i/o-kötet, és i/o-műveletek másodpercenkénti száma hello DBMS kiszolgálón kell értékelni. Újonnan tervezett rendszerek esetén is méretezése a gyakorlatban hello új rendszer hello hello i/o-követelményeinek hello DBMS ügyféloldali a nyers ötleteket adjon. Ha nem ismeri, végül van szüksége a koncepció igazolása tooconduct.

**3. lépés**: hasonlítsa össze hello SAP követelmény hello adatbázis-kezelő kiszolgáló hello SAP hello VM különböző Azure biztosít. hello adatokat hello Azure virtuális gép különböző SAP SAP Megjegyzés ismertetett [1928533]. hello fókusz kell az adatbázis-kezelő VM hello először mert hello Adatbázisréteg, amely egy terjessze ki a központi telepítések többsége hello SAP NetWeaver rendszer hello réteg. Ezzel szemben hello SAP alkalmazásréteg kiterjeszthető. Ha egy hello SAP támogatja Azure Virtuálisgép-típusokon biztosíthat szükséges hello SAP, tervezett hello SAP rendszer hello munkaterhelés nem futtatható, az Azure-on. Szükség vagy toodeploy hello rendszer helyszíni, illetve toochange hello munkaterhelés kötet hello rendszer szükséges.

**4. lépés**: dokumentált [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows], Azure érvénybe lépteti az IOPS kvóta lemezenként Standard szintű tárolást vagy a prémium szintű Storage akár független. Virtuálisgép-típussá hello függ, lehet csatlakoztatni adatlemezek száma hello függ. Ennek eredményeképpen kiszámításához a maximális iops-érték, amely elérhető az összes hello eltérő típusú virtuális gép. Hello adatbázis fájlelrendezés függ, lemezek toobecome egy kötet hello vendég operációs rendszer is paritásos. Azonban ha hello aktuális IOPS telepített SAP-rendszer meghaladják hello legnagyobb virtuális gép típusát és az Azure-e egyetlen alkalommal toocompensate több memória számított hello korlátai által megszabott, hello SAP rendszer hello munkaterhelését befolyásolhatja, súlyosan. Ebben az esetben kattintson az a pont, ahol nem kell telepít hello rendszert az Azure-on.

**5. lépés**: különösen SAP rendszerek, amelyek telepít a helyi 2 szintű konfigurációk, valószínűleg hello, hello rendszer toobe konfigurált Azure 3 szintű konfigurációban előfordulhat, hogy kell. Ebben a lépésben kell toocheck összetevő hello SAP alkalmazás rétegben, amely nem terjeszthető ki, és amely nem fér el hello CPU és memória-erőforrások hello különböző Azure virtuális gép típusok ajánlat van-e. Ha valóban ilyen összetevő, hello SAP rendszer és a munkaterhelés nem telepíthető az Azure. De ha több Azure virtuális gépeken történő lehet horizontálisan hello SAP alkalmazás-összetevők, hello rendszer is telepíthető az Azure.

**6. lépés**: hello adatbázis-kezelő és az SAP réteg alkalmazásösszetevői Azure virtuális gépeken is futtatható, ha hello beállításokat kell-e toobe definiált a következőkre vonatkozóan:

* Az Azure virtuális gépek száma
* Virtuálisgép-típusokon hello egyéni összetevők
* Az adatbázis-kezelő VM tooprovide VHD-k száma elég IOPS

## <a name="managing-azure-assets"></a>Az Azure-eszközök kezelése
### <a name="azure-portal"></a>Azure Portal
hello Azure-portálon a három felületek toomanage Azure Virtuálisgép-telepítések egyike. hello alapvető felügyeleti feladatokat, például a lemezképeket, a virtuális gépek telepítése végezhető el hello Azure-portálon. Ezenkívül hello Tárfiókokat, virtuális hálózatok létrehozása, és más Azure összetevői is feladatok hello Azure-portálon jól kezelhető. Azonban a funkciók például a virtuális merevlemezek feltöltését a helyszíni tooAzure vagy az Azure virtuális merevlemez másolása feladatokat, melyekhez szükséges külső gyártású eszközöknek vagy a PowerShell vagy a parancssori felületen keresztül felügyeleti is.

![A Microsoft Azure portál – a virtuális gépek – áttekintés][planning-guide-figure-800]

[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[comment]: <> (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Felügyelet és a konfigurációs feladatok hello virtuálisgép-példányt is előfordulhatnak a hello Azure-portálon belül.

Újraindítása és leállítása a virtuális gép mellett is csatolni, válassza le, és hello virtuálisgép-példányt, toocapture hello példány lemezkép előkészítése az adatlemezek létrehozása és hello virtuálisgép-példányt hello méretének beállítása.

hello Azure-portálon toodeploy alapvető funkciókat biztosít, és konfigurálja a virtuális gépek és sok más Azure-szolgáltatásokkal. Azonban nem az összes rendelkezésre álló funkciók hello Azure-portálon vonatkozik. A hello Azure-portálra már nem lehetséges tooperform feladatokat, például:

* Virtuális merevlemezek tooAzure feltöltése
* Virtuális gépek másolása

[comment]: <> (MShermannd TODO Mi a helyzet automation szolgáltatás SAP virtuális gépekhez?)
[comment]: <> (Több virtuális gépek, operációs rendszer időközben lehetséges MSSedusch telepítése)
[comment]: <> (Az automatizálás vonatkozó központi telepítési típussal is MSSedusch nem hello Azure-portálon lehetséges. Feladatokat, mint a több virtuális gépek parancsfájlalapú telepítés nincs lehetőség hello Azure-portálon keresztül.)

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>A Microsoft Azure PowerShell-parancsmagok segítségével kezelése
A Windows PowerShell telepítése az Azure-ban rendszerek nagyobb mennyiségű ügyfél széles körben elfogadott sokoldalú és bővíthető keretrendszer. A PowerShell-parancsmagok egy asztali, a laptop vagy a dedikált felügyeleti állomás hello telepítés után hello PowerShell-parancsmagok távolról futtatható.

hello folyamat tooenable hello használati Azure PowerShell-parancsmagok helyi asztali vagy hordozható, és hogyan a felhasználást a hello tooconfigure hello Azure előfizetés(ek) leírtak [Ez a cikk][powershell-install-configure].

Hogyan tooinstall, frissítése és konfigurálása hello Azure PowerShell-parancsmagok részletes lépéseket is megtalálhatók [hello telepítési útmutató ezen fejezete][deployment-guide-4.1].

Felhasználói élmény eddig portja, hogy PowerShell (PS): alapértékekkel hello hatékonyabb eszköz toodeploy virtuális gépek és toocreate egyéni virtuális gépek hello telepítési lépéseit. PS parancsmagok toosupplement felügyeleti feladatokat az Azure-portálon hello, vagy a PS-parancsmagok is használ az összes Azure-beli SAP példányok hello ügyfelek használ kizárólag toomanage a telepítések az Azure-ban. Mivel hello Azure-specifikus parancsmagok megosztás hello azonos elnevezési hello mint 2000-nél több Windows-kapcsolatos parancsmagok, célszerű egy egyszerű feladathoz tartozó Windows rendszergazdák tooleverage e parancsmagok.

Itt a következő példa: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[comment]: <> (MShermannd Teendőlista új CLI parancs tesztelésekor írják le.)
Hello Azure Figyelőbővítmény az SAP telepítését (című [Azure figyelésére szolgáló megoldás az SAP] [ planning-guide-9.1] ebben a dokumentumban) csak akkor PowerShell vagy a parancssori felületen keresztül lehetséges. Ezért azt kötelező tooset működik-e, és konfigurálja a PowerShell vagy a CLI üzembe helyezésekor és felügyelete az Azure-ban az SAP NetWeaver rendszer.  

Azure több funkciót biztosít, új PS-parancsmagok fog toobe hozzáadott hello parancsmagok frissítést igényel. Ezért teszi logika toocheck hello Azure letöltési hely legalább egyszer hello hónap <https://azure.microsoft.com/downloads/> hello parancsmagok új verziója. hello új verziója hello régebbi verzióra.

Az Azure-hoz kapcsolódó PowerShell listáját általános parancsok be a négyzetet: <https://docs.microsoft.com/powershell/azure/overview>.

### <a name="management-via-microsoft-azure-cli-commands"></a>A Microsoft Azure parancssori felület parancsait keresztül kezelése
Szeretné, hogy toomanage Azure Linux felhasználók erőforrások Powershell beállítást nem lehet. A Microsoft Azure CLI alternatívájaként kínál.
hello Azure parancssori felület számos nyílt forráskódú, platformok közötti parancsok végzett munka hello Azure platformra. az Azure parancssori felület hello több hello hello Azure-portálon található ugyanezeket a funkciókat tartalmazza.

További információ a telepítéshez, konfiguráláshoz és hogyan toouse parancssori felület parancsai tooaccomplish Azure feladatok:

* [Hello Azure parancssori felület telepítése][xplat-cli]
* [Telepítését és kezelését a virtuális gépek Azure Resource Manager-sablonok és hello Azure parancssori felület használatával] [.. /.. / linux/create-ssh-secured-vm-from-template.md]
* [Hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager használatára][xplat-cli-azure-resource-manager]

Fejezet olvassa [Linux virtuális gépek Azure CLI] [ deployment-guide-4.5.2] hello a [telepítési útmutató] [ planning-guide] a hogyan toouse Azure CLI toodeploy hello Azure figyelése Az SAP-kiterjesztés.

## <a name="different-ways-toodeploy-vms-for-sap-in-azure"></a>Az SAP, az Azure virtuális gépek különböző módokon toodeploy
Ez a fejezet elsajátíthatja hello különböző módokon toodeploy egy Azure-ban. Ez a fejezet további előkészítést eljárásokat, valamint a virtuális merevlemezek és az Azure virtuális gépek kezelésének ismertetnek.

### <a name="deployment-of-vms-for-sap"></a>Az SAP virtuális gépek telepítése
Microsoft Azure toodeploy virtuális gépek több lehetőséget kínál, és a kapcsolódó lemezek. Ezért ez a beállítás nagyon fontos toounderstand hello különbségek mivel hello virtuális gépek előkészített eltérőek lehetnek attól függően, hogy az üzembe helyezési módszer hello. Általában azt tekintse meg a következő forgatókönyvek hello:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>A virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemez
A helyszíni tooAzure SAP rendszert toomove tervezi. Ez hello virtuális Merevlemezt, amely tartalmazza az operációs rendszer hello feltöltésével végezhető, hello SAP bináris fájljait, és az adatbázis-kezelő bináris fájlokat, valamint virtuális merevlemezek hello fájlokkal hello adatokhoz és a naplófájlhoz az adatbázis-kezelő tooAzure hello. Ezzel szemben túl[az alábbi #2. forgatókönyv][planning-guide-5.1.2], mindig hello állomásnév, SAP SID és SAP felhasználói fiókot a hello Azure virtuális gép, azok hello a helyszíni környezetben volt konfigurálva. Ezért normalizálása üdvözlő kép, nem szükséges. Fejezetek lásd [előkészítése a virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemezzel] [ planning-guide-5.2.1] a jelen dokumentum előkészítő lépések helyszíni és az nem általánosított virtuális gépek és VHD tooAzure feltöltése. Fejezet elolvasása [3. forgatókönyv: egy virtuális gép áthelyezése a helyszíni Azure nem általánosított virtuális Merevlemezt használ SAP] [ deployment-guide-3.4] hello a [telepítési útmutató] [ deployment-guide] a részletes lépéseket egy lemezképet, az Azure-ban történő telepítésének.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Virtuális gép és egy ügyfél-specifikus lemezkép központi telepítése
Miatt az operációs rendszer vagy az adatbázis-kezelő verziója toospecific javítás követelményeinek megadott hello képek hello Azure piactér előfordulhat, hogy nem felelnek meg az igényeinek. Ezért szükség lehet a saját "private" az operációs rendszer vagy adatbázis-kezelő Virtuálisgép-lemezkép, amely ezután többször is telepíthető virtuális gépek toocreate. tooprepare duplikálva lettek-e ilyen egy "private" lemezképet, a következő elemek hello figyelembe veendő toobe rendelkezik:

- - -
> ![Windows][Logo_Windows] Windows
>
> További információt itt olvashat: <https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed> hello Windows-beállítások (például a Windows biztonsági AZONOSÍTÓK és állomásnév) kell lennie a helyszíni hello azért/általánosítva Virtuális gépről hello sysprep parancsot.
>
>
> ![Linux][Logo_Linux] Linux
>
> Hajtsa végre az alábbi cikkeiben hello lépéseket [SUSE][virtual-machines-linux-create-upload-vhd-suse], [Red Hat][virtual-machines-linux-redhat-create-upload-vhd], vagy [Oracle Linux] [ virtual-machines-linux-create-upload-vhd-oracle], egy virtuális merevlemez toobe tooprepare tooAzure feltöltve.
>
>

- - -
Ha már telepítette a helyszíni virtuális gépre (különösen a 2-réteg rendszerek) SAP tartalom, módosíthatja hello SAP rendszerbeállítások hello telepítése Azure virtuális gép hello hello példány keresztül SAP Szoftverellátáshoz hello által támogatott eljárás átnevezése után Manager (SAP Megjegyzés [1619720]). Lásd: fejezetek [specifikus képének a virtuális gépek telepítése az SAP-előkészítése] [ planning-guide-5.2.2] és [helyszíni tooAzure virtuális merevlemez feltöltése] [ planning-guide-5.3.2]a jelen dokumentum előkészítő lépések helyszíni és az egy általánosított virtuális gép tooAzure feltöltése. Fejezet elolvasása [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP] [ deployment-guide-3.3] a hello [telepítési útmutató] [ deployment-guide] történő telepítésének részletes lépései ilyen lemezkép az Azure-ban.

#### <a name="deploying-a-vm-out-of-hello-azure-marketplace"></a>A virtuális gépek kívül hello Azure piactér telepítése
Szeretné a Microsoft vagy harmadik fél toouse megadott hello Azure piactér toodeploy a Virtuálisgép-lemezkép a virtuális Gépet. Miután telepítette a virtuális Gépet az Azure-ban, akkor hajtsa végre a hello azonos irányelvek és eszközök tooinstall hello SAP szoftver és/vagy az adatbázis-kezelő a virtuális Gépen belül, mint a helyszíni környezetben. Részletes leírás a telepítés, ellenőrizze a fejezet [1. forgatókönyv: központi telepítése egy virtuális gép kívül hello Azure piactérről az SAP] [ deployment-guide-3.2] hello a [telepítési útmutató] [ deployment-guide].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Az Azure SAP rendelkező virtuális gépek előkészítése
Az Azure virtuális gépek feltöltés előtt kell toomake meg arról, hogy hello virtuális gépek és VHD-k bizonyos követelmények teljesítéséhez. Használt hello telepítési módszertől függően kisebb különbségek vannak.

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>A virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemezzel előkészítése
Gyakori telepítési módja, hogy egy meglévő virtuális Gépre, amely a helyszíni tooAzure SAP rendszerrel toomove. Hogy a virtuális gép és a virtuális gép csak kell futtatni az Azure használatával hello SAP rendszer hello hello azonos állomásnév, és valószínűleg hello azonos SAP SID azonosítója. Ebben az esetben hello vendég operációs rendszer a virtuális gép kell nem általánosítva több-telepítéshez. Ha hello a helyszíni hálózat kapott kiterjeszthetőek az Azure (című [létesítmények közötti - központi telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény abból a teljes mértékben történő integrálását hello a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban), még akkor is, majd hello azonos tartományi fiókokat használhat hello VM belül azok helyszíni előtt használt.

A saját Azure VM lemez előkészítésekor követelmények a következők:

* Eredetileg hello VHD-t tartalmazó hello operációs rendszer maximális mérete 127GB csak rendelkezhetnek. Ez a korlátozás kapott szüntetni 2015. márciusi hello végét. Most hello hello operációs rendszert tartalmazó virtuális merevlemez lehet mérete too1TB be minden más Azure-tárterület tárolt VHD-t is.
* A rögzített VHD-formátumban hello toobe szükséges. Dinamikus VHD vagy VHDx formátumú virtuális merevlemezeket még nem támogatottak az Azure-on. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Virtuális merevlemezek, amelyek csatlakoztatott toohello virtuális gép, és az Azure toohello VM kell toobe egy rögzített méretű VHD-formátumban, valamint újra legyen csatlakoztatva. Kérjük, olvassa el [Ez a cikk (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) és [Ez a cikk (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) méretkorlátait adatlemezek számára. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Adjon hozzá egy másik helyi fiók, amely használatával a Microsoft támogatási vagy amely rendelhető környezetben a szolgáltatások és alkalmazások toorun amíg hello virtuális gép telepítve van és jobban megfelelő felhasználók rendszergazdai jogosultságokkal is használható.
* Hello eset egy kizárólag felhőalapú környezet-forgatókönyv használatával (című [csak felhőalapú - hello a függőségek nélkül az Azure virtuális gépek telepítéséhez a helyi felhasználói hálózati] [ planning-guide-2.1] ezen dokumentálja) együtt a telepítési módszerrel, tartományi fiókok nem működik, ha hello Azure lemez a rendszer az Azure-ban. Ez különösen igaz, a fiókok, amelyek használt toorun szolgáltatásokat, mint a hello DBMS vagy SAP-alkalmazást. Ezért kell tooreplace ilyen tartományi fiókokat a virtuális gép helyi fiókokkal, és törölje a hello helyszíni tartományi fiókokat a virtuális gép hello. A helyszíni tartományi felhasználók tartása hello VM-lemezképben darabolása nem okoz problémát amikor a virtuális gép telepítve van az hello hello létesítmények közötti forgatókönyv fejezetben leírtak [létesítmények közötti - telepítését egy vagy több SAP-virtuális gép az Azure hello követelmény, hogy a teljesen integrálva hello a helyszíni hálózat] [ planning-guide-2.2] ebben a dokumentumban.
* Ha tartományi fiókokat, az adatbázis-kezelő bejelentkezések használt, vagy felhasználó hello rendszer a helyi és virtuális gépek futtatásakor kizárólag felhőalapú forgatókönyvekben telepített toobe tudhatja, hello tartományi felhasználók kell toobe törölve. Arról, hogy helyi rendszergazda hello plusz egy másik virtuális gép helyi felhasználói meg van adva a bejelentkezési/felhasználó hello DBMS rendszergazdaként történő toomake van szüksége.
* Hozzáadhat más helyi fiókok azokat az adott környezet hello szükség lehet.

- - -
> ![Windows][Logo_Windows] Windows
>
> Ebben a forgatókönyvben nem általánosítása (sysprep) a virtuális gép hello szükséges tooupload és hello Azure VM telepítése.
> Győződjön meg arról, hogy nem használatos a D:\ meghajtóra.
> Állítsa be a csatlakoztatott lemezek lemez automount fejezetben leírtak [beállítás automount a csatlakoztatott lemezekkel] [ planning-guide-5.5.3] ebben a dokumentumban.
>
> ![Linux][Logo_Linux] Linux
>
> Ebben a forgatókönyvben nem általánosítása (waagent-deprovision) hello VM szükséges tooupload és hello Azure VM telepítése.
> Győződjön meg arról, hogy az összes lemez csatlakoztatva van-e keresztül uuid és, hogy nem használja a/mnt és az erőforrások. Hello operációsrendszer-lemez győződjön meg arról, hogy hello rendszertöltő bejegyzés is tükrözi hello uuid-alapú csatlakoztatási.
>
>

- - -
#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Virtuális gép és egy ügyfél-specifikus kép telepítése az SAP-előkészítése
Egy általánosított OS tartalmazó VHD-fájlokat tároló Azure Storage-fiókok vagy kezelt lemezképek tárolódnak. A központi telepítési sablon fájlokban forrásaként hivatkozó hello VHD vagy felügyelt lemezképről, fejezetben leírtak segítségével telepíthet egy új virtuális Gépet egy lemezképből [2. forgatókönyv: központi telepítése a virtuális gép és egy egyéni lemezképet az SAP] [ deployment-guide-3.3]a hello [telepítési útmutató][deployment-guide].

Ha a saját Azure Virtuálisgép-lemezkép előkészítése követelményei vannak:

* Eredetileg hello VHD-t tartalmazó hello operációs rendszer maximális mérete 127GB csak rendelkezhetnek. Ez a korlátozás kapott szüntetni 2015. márciusi hello végét. Most hello hello operációs rendszert tartalmazó virtuális merevlemez lehet mérete too1TB be minden más Azure-tárterület tárolt VHD-t is.
* A rögzített VHD-formátumban hello toobe szükséges. Dinamikus VHD vagy VHDx formátumú virtuális merevlemezeket még nem támogatottak az Azure-on. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
* Virtuális merevlemezek, amelyek csatlakoztatott toohello virtuális gép, és az Azure toohello VM kell toobe egy rögzített méretű VHD-formátumban, valamint újra legyen csatlakoztatva. Kérjük, olvassa el [Ez a cikk (Linux)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-linux) és [Ez a cikk (Windows)](https://docs.microsoft.com/azure/storage/storage-about-disks-and-vhds-windows) méretkorlátait adatlemezek számára. Dinamikus virtuális merevlemezek lesz konvertált toostatic virtuális merevlemezek VHD hello PowerShell-parancsmagjaival vagy a CLI feltöltésekor
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
> Győződjön meg arról, hogy az összes lemez csatlakoztatva van-e keresztül uuid és, hogy nem használja a/mnt és az erőforrások. Hello operációsrendszer-lemez győződjön meg arról, hello rendszertöltő bejegyzés is tükrözi az uuid-alapú csatlakoztatási hello.
>
>

- - -
* SAP grafikus felhasználói felület (a felügyeleti és beállítására céljából) a sablon előre telepítve.
* Egyéb szoftverek szükséges toorun hello sikeresen létesítmények közötti forgatókönyv a virtuális gépek telepíthetők, mindaddig, amíg ez a szoftver használható hello nevezze át a virtuális gép hello.

Ha hello virtuális gép készen áll a testreszabásra elég általános, és végül független a fiókok/felhasználók számára nem érhető el a hello toobe céloz Azure üzembe helyezési forgatókönyv, hello utolsó előkészítő lépésében ilyen lemezkép normalizálása végzik.

##### <a name="generalizing-a-vm"></a>A virtuális gépek normalizálása
- - -
> ![Windows][Logo_Windows] Windows
>
> hello utolsó lépése a virtuális gép tooa rendszergazdaként toolog. Nyisson meg egy Windows parancsablakot "rendszergazdaként. Nyissa meg too%windir%\windows\system32\sysprep, majd hajtsa végre a sysprep.exe.
> Egy kis ablakban jelenik meg. Fontos toocheck hello "Generalize" lehetőség (hello alapértelmezett érték a nem ellenőrzött), és módosítsa a hello kikapcsolás beállítás az alapértelmezett "Újraindítás" too'Shutdown ". Ez az eljárás azt feltételezi, hogy hello sysprep folyamat végrehajtott történő helyszíni hello a virtuális gépek vendég operációs rendszer.
> Ha tooperform hello eljárás Azure-ban már futó virtuális gépen, lépésekkel hello ismertetett [Ez a cikk](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).
>
> ![Linux][Logo_Linux] Linux
>
> [Hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként][capture-image-linux-step-2-create-vm-image]
>
>

- - -
### <a name="transferring-vms-and-vhds-between-on-premises-tooazure"></a>A helyszíni tooAzure közötti átviteléhez a virtuális gépek és VHD-k
Mivel a Virtuálisgép-lemezképet és lemezt tooAzure feltöltése nem lehetséges hello Azure-portálon keresztül, kell toouse Azure PowerShell-parancsmagjaival vagy a parancssori felület. Egy másik lehetőség, hello eszköz "AzCopy" hello használatát. hello eszköz másolható VHD-k a helyszíni és az Azure közötti (kétirányú). Azt is másolhatja Azure-régiók közötti virtuális merevlemezeket. További részleteket [ebben a dokumentációban] [ storage-use-azcopy] a letöltésről és az AzCopy használata.

Harmadik alternatív lenne toouse különböző külső grafikus felhasználói Felülettel alapú eszközök. Azonban győződjön meg arról, hogy ezek az eszközök támogatják a Lapblobokat Azure. Az az Azure oldalakra vonatkozó Blob toouse kell célokból tárolni (hello különbségek a dokumentum ismerteti: <https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>). Azure által biztosított hello eszközöket is nagyon hatékonyak a tömörítés hello virtuális gépek és VHD, amelyek feltöltött toobe kell. Ez azért fontos, mert ez a hatékonyság a tömörítés csökkenti a hello ideje (ami függ ennek ellenére a hello feltöltés hivatkozás toohello hello helyszíni létesítmény és hello Azure-telepítés régió internet megcélzott). A valós azt feltételezi, hogy a virtuális gép vagy a virtuális merevlemez feltöltése az Európai hely toohello adatközpontok hosszabb, mint feltöltése érvénybe az Egyesült államokbeli az Azure data hello azonos virtuális gépek/VHD-k toohello Európai Azure adatközpontjaiban.

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>A helyszíni tooAzure virtuális merevlemez feltöltése
egy meglévő virtuális vagy virtuális merevlemez hello helyszíni hálózati egy virtuális Gépet, vagy VHD-t kell toomeet hello követelmények fejezetben felsorolt tooupload [előkészítése a virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemezzel] [ planning-guide-5.2.1] ebben a dokumentumban.

Egy virtuális Gépet nem kell toobe általánosítva van, és hello állapotát és után hello leállítás helyszíni ügyféloldali rendelkezik alakzat fel kell tölteni. hello esetében is igaz további VHD, amelyek nem tartalmaznak minden operációs rendszer.

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Virtuális merevlemez feltöltése, majd elérhetővé tétele az Azure-lemez
Ebben az esetben azt szeretné, hogy egy virtuális Merevlemezt, vagy egy, az operációs rendszer anélkül tooupload és tooa VM adatok lemezként csatlakoztatni vagy használni az operációsrendszer-lemez. Ez az folyamat

**PowerShell**

* Jelentkezzen be tooyour előfizetés *Login-AzureRmAccount*
* Állítsa be a hello előfizetést, a környezetben a *Set-AzureRmContext* és előfizetés-azonosító paramétert vagy SubscriptionName - <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Töltse fel a virtuális merevlemez hello *Add-AzureRmVhd* tooan Azure Storage-fiók – lásd: <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Választható) Hello VHD-t hozhat létre egy felügyelt lemezt a *New-AzureRmDisk* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk>
* Hello az operációs rendszer lemezén található egy új virtuális gép konfigurációs toohello VHD-t vagy a felügyelt lemezzel *Set-AzureRmVMOSDisk* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
* Hozzon létre egy új virtuális Gépet a Virtuálisgép-konfiguráció hello rendelkező *New-AzureRmVM* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>
* Adatok lemez tooa hozzáadása az új virtuális gép *Add-AzureRmVMDataDisk* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk>

**Azure CLI 2.0**

* Jelentkezzen be tooyour előfizetés *az bejelentkezés*
* Válassza ki az előfizetés *az fiók beállítása – előfizetés`<subscription name or id`>*
* Töltse fel a virtuális merevlemez hello *az tárolási blob feltöltése* -lásd: [az Azure Storage Azure CLI használata hello][storage-azure-cli]
* (Választható) Hello VHD-t hozhat létre egy felügyelt lemezt a *az lemez létrehozása* -https://docs.microsoft.com/cli/azure/disk#create lásd:
* Hozzon létre egy új virtuális Gépet hello feltöltött virtuális merevlemez vagy a kezelt lemez az operációs rendszer lemezeként megadása *az virtuális gép létrehozása* és paraméter *--csatolása operációsrendszer-lemez*
* Adatok lemez tooa hozzáadása az új virtuális gép *az méretű lemez csatolása* és paraméter *– új*

**Sablon**

* Hello Powershell vagy Azure CLI VHD feltöltése
* (Választható) Hozhat létre egy felügyelt lemezt hello VHD Powershell, az Azure parancssori felület vagy a hello Azure-portálon
* Hello VM telepítsen egy JSON-sablon hivatkozik hello VHD, ahogy az az [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json) vagy felügyelt lemezt használ, ahogy az [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-disk-md/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>A Virtuálisgép-lemezkép központi telepítése
egy meglévő virtuális vagy virtuális merevlemez hello helyszíni hálózatot a rendelés toouse az Azure virtuális gép képként egy virtuális Gépet vagy virtuális Merevlemezt kell toomeet hello követelményeknek fejezetben felsorolt tooupload [specifikus képének a virtuális gépek telepítése az SAPelőkészítése] [ planning-guide-5.2.2] ebben a dokumentumban.

* Használjon *sysprep* Windows vagy *waagent-deprovision* lévő Linux toogeneralize a VM - lásd: [Sysprep műszaki útmutatója](https://technet.microsoft.com/library/cc766049.aspx) Windows vagy [hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként] [ capture-image-linux-step-2-create-vm-image] Linux
* Jelentkezzen be tooyour előfizetés *Login-AzureRmAccount*
* Állítsa be a hello előfizetést, a környezetben a *Set-AzureRmContext* és előfizetés-azonosító paramétert vagy SubscriptionName - <https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext>
* Töltse fel a virtuális merevlemez hello *Add-AzureRmVhd* tooan Azure Storage-fiók – lásd: <https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd>
* (Választható) Hello VHD felügyelt lemezképet készíteni az *New-AzureRmImage* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage>
* Állítsa be az új Virtuálisgép-konfiguráció toothe hello operációsrendszer-lemez
  * Virtuális merevlemez *Set-AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk>
  * Felügyelt lemezképet *Set-AzureRmVMSourceImage* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage>
* Hozzon létre egy új virtuális Gépet a Virtuálisgép-konfiguráció hello rendelkező *New-AzureRmVM* -lásd <https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm>

**Azure CLI 2.0**

* Használjon *sysprep* Windows vagy *waagent-deprovision* lévő Linux toogeneralize a VM - lásd: [Sysprep műszaki útmutatója](https://technet.microsoft.com/library/cc766049.aspx) Windows vagy [hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként] [ capture-image-linux-step-2-create-vm-image] Linux
* Jelentkezzen be tooyour előfizetés *az bejelentkezés*
* Válassza ki az előfizetés *az fiók beállítása – előfizetés`<subscription name or id`>*
* Töltse fel a virtuális merevlemez hello *az tárolási blob feltöltése* -lásd: [az Azure Storage Azure CLI használata hello][storage-azure-cli]
* (Választható) Hello VHD felügyelt lemezképet készíteni az *az lemezkép létrehozása* -https://docs.microsoft.com/cli/azure/image#create lásd:
* Hozzon létre egy új virtuális Gépet hello feltöltött virtuális merevlemez vagy a felügyelt lemezképét az operációs rendszer lemezeként megadása *az virtuális gép létrehozása* és paraméter *– kép*

**Sablon**

* Használjon *sysprep* Windows vagy *waagent-deprovision* lévő Linux toogeneralize a VM - lásd: [Sysprep műszaki útmutatója](https://technet.microsoft.com/library/cc766049.aspx) Windows vagy [hogyan toocapture egy Linux virtuális gép toouse erőforrás-kezelő sablonként] [ capture-image-linux-step-2-create-vm-image] Linux
* Hello Powershell vagy Azure CLI VHD feltöltése
* (Választható) Felügyelt lemezképet készíteni hello VHD Powershell, az Azure parancssori felület vagy a hello Azure-portálon
* Hello VM telepítsen egy JSON-sablon hivatkozik hello kép VHD, ahogy az az [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/sap-2-tier-user-image/azuredeploy.json) vagy használatával hello felügyelt lemezképét, ahogy az [példa JSON sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-or-managed-disks-tooon-premises"></a>Virtuális merevlemezek vagy kezelt lemezek tooon helyszíni letöltése
Azure-infrastruktúra szolgáltatásként nincs egy egyirányú utcaneve csak a képes tooupload VHD-k és az SAP rendszerek alatt. SAP áthelyezheti az Azure-ból rendszerek vissza az hello helyszíni világon is.

Hello hello ideje alatt a letöltési hello VHD-k vagy kezelt lemezek nem lehet aktív. Akkor is, ha a letöltés lemezek, amelyek csatlakoztatott tooVMs hello a virtuális gép igényeinek toobe leállítása és felszabadítása. lehetséges. Ha csak egy új rendszer telepítéséhez használt tooset majd kellene lenniük a helyszíni és elfogadható, hogy a hello hello idő alatt letöltése és telepítése új hello hello rendszer, amely a rendszer az Azure-ban hello továbbra is lehet működési tartalom toodownload hello adatbázis , nem sikerült egy hosszú állásidő elkerülése érdekében végezzen egy tömörített adatbázis biztonsági mentése lemezre, és csak töltse le, hogy a lemez az operációs rendszer alap virtuális gép hello is letöltésük helyett.

#### <a name="powershell"></a>PowerShell

  * Felügyelt lemezes letöltése  
  Először tooget hozzáférés toohello az alapul szolgáló hello felügyelt lemezt a blob. Ezután másolja hello alapul szolgáló blob tooa új tárfiók és hello blob letölteni ezt a tárfiókot.

  ```powershell
  $access = Grant-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name> -Access Read -DurationInSecond 3600
  $key = (Get-AzureRmStorageAccountKey -ResourceGroupName <resource group> -Name <storage account name>)[0].Value
  $destContext = (New-AzureStorageContext -StorageAccountName <storage account name -StorageAccountKey $key)
  Start-AzureStorageBlobCopy -AbsoluteUri $access.AccessSAS -DestContainer <container name> -DestBlob <blob name> -DestContext $destContext
  # Wait for blob copy toofinish
  Get-AzureStorageBlobCopyState -Container <container name> -Blob <blob name> -Context $destContext
  Save-AzureRmVhd -SourceUri <blob in new storage account> -LocalFilePath <local file path> -StorageKey $key
  # Wait for download toofinish
  Revoke-AzureRmDiskAccess -ResourceGroupName <resource group> -DiskName <disk name>
  ```

  * Virtuális merevlemez letöltése  
  Miután hello SAP rendszer le van állítva és hello virtuális gép le van állítva, használhatja hello PowerShell parancsmag mentés-AzureRmVhd hello helyszíni cél toodownload hello VHD lemezek toohello helyszíni globális biztonsági másolatot. Rendelés toodo, telepíteni kell, amely virtuális Merevlemezt, amely a "Tároló rész" hello hello hello URL-CÍMÉT a hello Azure portal (kell toonavigate toohello Tárfiókot és hello tároló hello virtuális merevlemez létrehozásának helyét), és kell tooknow, ahol hello VHD-t kell lennie másolja.

  Kihasználhatja a hello parancs majd meghatározásával egyszerűen hello paraméter SourceUri hello URL-címet hello VHD toodownload és hello LocalFilePath mint hello (beleértve a neve) VHD hello fizikai helyét. hello parancs nézhet:

  ```powerhell
  Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
  ```

  Hello mentés-AzureRmVhd parancsmag további részletekért ellenőrizze Itt <https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvhd>.

#### <a name="cli-20"></a>CLI 2.0
  * Felügyelt lemezes letöltése  
  Először tooget hozzáférés toohello az alapul szolgáló hello felügyelt lemezt a blob. Ezután másolja hello alapul szolgáló blob tooa új tárfiók és hello blob letölteni ezt a tárfiókot.
  ```
  az disk grant-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --duration-in-seconds 3600
  az storage blob download --sas-token "<sas token>" --account-name <account name> --container-name <container name> --name <blob name> --file <local file>
  az disk revoke-access --ids "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>"
  ```

  * Virtuális merevlemez letöltése   
  Miután hello SAP rendszer le van állítva, és hello virtuális gép le van állítva, paranccsal hello Azure CLI _az azure storage-blob letöltési_ hello helyszíni cél toodownload hello VHD lemezek hátsó toohello helyszíni globális. Rendelés toodo, telepíteni kell, amely hello nevét és a virtuális Merevlemezt, amely a "Tároló rész" hello hello hello tároló hello az Azure portálon (kell toonavigate toohello Tárfiókot és hello tároló hello virtuális merevlemez létrehozásának helyét), és kell tooknow ahol virtuális merevlemez hello kell átmásolni.

  Kihasználhatja a hello parancs majd egyszerűen hello paraméterek blob és megadásával hello VHD toodownload és hello cél tároló, fizikai célhelye hello hello virtuális merevlemez (beleértve a nevét). hello parancs nézhet:

  ```
  az storage blob download --name <name of hello VHD toodownload> --container-name <container of hello VHD toodownload> --account-name <storage account name of hello VHD toodownload> --account-key <storage account key> --file <destination of hello VHD toodownload>
  ```

### <a name="transferring-vms-and-disks-within-azure"></a>Virtuális gépek és az Azure lemezek átvitele
#### <a name="copying-sap-systems-within-azure"></a>Az Azure SAP rendszerek másolása
Az SAP rendszer vagy akár egy SAP alkalmazásréteg támogató dedikált adatbázis-kezelő kiszolgálóra lesz valószínűleg közé több lemezt, vagy az operációs rendszer hello hello bináris fájljait tartalmazza, vagy adatokat hello és naplófájl (oka) t hello SAP adatbázis. A lemezek másolásának Azure funkció hello sem hello Azure funkcióit lemezek tooa helyi lemezre mentését szinkronizálási mechanizmussal rendelkezik, amely volna pillanatkép-párhuzamosan több lemez. Ezért hello hello állapotának másolja, vagy menti a lemezek, még akkor is, ha azok csatlakoztatva vannak elleni hello ugyanazon VM eltérő lenne. Ez azt jelenti, hogy hello konkrét esetben másik adatforráshoz és logfile(s) hello különböző lemezeken található hello adatbázis lévő hello lenne inkonzisztens.

**Megkötését: Rendelés toocopy vagy lemezek, amelyek részei egy SAP rendszerkonfiguráció toostop hello SAP rendszerre van szüksége, és is meg kell menteni a tooshut le hello telepített virtuális gép. Csak akkor másolja, vagy töltse le a lemezek tooeither hello készlete hello SAP rendszer másolatának létrehozása az Azure vagy a helyszíni.**

Az adatlemezek egy Azure Storage-fiókot a VHD-fájlokat képes tárolni, és közvetlenül csatlakoztatott tooa virtuális géphez, vagy képként használni. Ebben az esetben a hello VHD az másolt tooanother hely, mielőtt a virtuális géphez csatolt toohello. az Azure-ban hello VHD-fájl teljes neve hello Azure belül egyedinek kell lennie. Amint azt korábban már említettük, a hello neve: milyen a következőhöz hasonló háromrészes név:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Az adatlemezek kezelt lemezeken is lehet. Ebben az esetben hello kezelt lemez egy új kezelt lemezre, mielőtt a virtuális géphez csatolt toohello használt toocreate. hello kezelt lemez neve hello erőforráscsoporton belül egyedinek kell lennie.

##### <a name="powershell"></a>PowerShell
Ahogy az Azure PowerShell-parancsmagok toocopy virtuális merevlemez használható [Ez a cikk][storage-powershell-guide-full-copy-vhd]. ahogy az alábbi példa hello toocreate új kezelt lemez, új-AzureRmDiskConfig és a New-AzureRmDisk használja.

```powershell
$config = New-AzureRmDiskConfig -CreateOption Copy -SourceUri "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" -Location <location>
New-AzureRmDisk -ResourceGroupName <resource group name> -DiskName <disk name> -Disk $config
```

##### <a name="cli-20"></a>CLI 2.0
Is használhatja az Azure parancssori felület toocopy virtuális merevlemez, ahogy az [Ez a cikk][storage-azure-cli-copy-blobs]. toocreate egy új kezelt lemezt használjon *az lemez létrehozása* a hello a következő példában látható módon.

```
az disk create --source "/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/disks/<disk name>" --name <disk name> --resource-group <resource group name> --location <location>
```

##### <a name="azure-storage-tools"></a>Az Azure Storage-eszközök
* <http://storageexplorer.com/>

Azure Tártallózók Professional kiadása itt található:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer>

hello a virtuális merevlemez magát egy tárfiókon belül példány egy folyamatot, amely csupán néhány másodpercet (hasonló tooSAN hardver pillanatképeinek Lusta és az írás) vesz igénybe. Miután hello VHD fájlról csatolhatók tooa virtuális gép vagy egy kép tooattach, másolja át a hello VHD toovirtual gépek használatát.

##### <a name="powershell"></a>PowerShell
```powershell
# attach a vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path toovhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a managed disk tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -ManagedDiskId <managed disk id> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of hello vhd tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name <disk name> -VhdUri <new path of vhd> -SourceImageUri <path tooimage vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun, for example 0> -CreateOption fromImage
$vm | Update-AzureRmVM

# attach a copy of hello managed disk tooa vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$diskConfig = New-AzureRmDiskConfig -Location $vm.Location -CreateOption Copy -SourceUri <source managed disk id>
$disk = New-AzureRmDisk -DiskName <disk name> -Disk $diskConfig -ResourceGroupName <resource group name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Caching <caching option> -Lun <lun, for example 0> -CreateOption attach -ManagedDiskId $disk.Id
$vm | Update-AzureRmVM
```
##### <a name="cli-20"></a>CLI 2.0
```

# attach a vhd tooa vm
az vm unmanaged-disk attach --resource-group <resource group name> --vm-name <vm name> --vhd-uri <path toovhd>

# attach a managed disk tooa vm
az vm disk attach --resource-group <resource group name> --vm-name <vm name> --disk <managed disk id>

# attach a copy of hello vhd tooa vm
# this scenario is currently not possible with Azure CLI. A workaround is toomanually copy hello vhd toohello destination.

# attach a copy of a managed disk tooa vm
az disk create --name <new disk name> --resource-group <resource group name> --location <location of target virtual machine> --source <source managed disk id>
az vm disk attach --disk <new disk name or managed disk id> --resource-group <resource group name> --vm-name <vm name> --caching <caching option> --lun <lun, for example 0>
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Azure Storage-fiókok között a lemezek másolása
Ez a feladat nem hajtható végre hello Azure-portálon. Használhatja az Azure PowerShell-parancsmagok, Azure CLI és egy külső tároló böngészőt. PowerShell-parancsmagok hello, vagy a parancssori felület parancsait hozhat létre és kezelheti a bináris objektumok, például hello képességét tooasynchronously másolási BLOB Storage-fiókok és régiók belül hello Azure-előfizetés között.

##### <a name="powershell"></a>PowerShell
Virtuális merevlemezek előfizetések között is másolhatja. További információk [Ez a cikk][storage-powershell-guide-full-copy-vhd].

hello alapszintű hello PS parancsmag logika áramló így néz ki:

* Hozzon létre egy tárfiók környezetét a hello **forrás** tárfiók *New-AzureStorageContext* -lásd <https://msdn.microsoft.com/library/dn806380.aspx>
* Hozzon létre egy tárfiók környezetét a hello **cél** tárfiók *New-AzureStorageContext* -lásd <https://msdn.microsoft.com/library/dn806380.aspx>
* Indítsa el a hello másolása

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Az ismétlődő hello másolási hello állapotának ellenőrzése

```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Hello új virtuális merevlemez tooa virtuális gép csatlakoztatása a fent leírt módon.

További példák: [Ez a cikk][storage-powershell-guide-full-copy-vhd].

##### <a name="cli-20"></a>CLI 2.0
* Indítsa el a hello másolása

```
az storage blob copy start --source-blob <source blob name> --source-container <source container name> --source-account-name <source storage account name> --source-account-key <source storage account key> --destination-container <target container name> --destination-blob <target blob name> --account-name <target storage account name> --account-key <target storage account name>
```

* Ellenőrizze a hello állapotát, ha hello az ismétlődő másolása

```
az storage blob show --name <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```

* Hello új virtuális merevlemez tooa virtuális gép csatlakoztatása a fent leírt módon.

További példák: [Ez a cikk][storage-azure-cli-copy-blobs].

### <a name="disk-handling"></a>Lemez kezelése
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Virtuális gép/lemez-szerkezet SAP központi telepítések
Ideális esetben hello hello struktúra, a virtuális gépek kezelésére, és társított hello lemezek nagyon egyszerű kell lennie. A helyszíni telepítésekre az ügyfelek szerkezetének kialakítása a kiszolgáló telepítése az sokféleképpen ki.

* Egy alaplemez, amely tartalmazza az operációs rendszer hello és minden hello bináris fájljait és/vagy az adatbázis-kezelő SAP hello. 2015. márciusi, mert a lemez-nál korábbi korlátozások, korlátozott helyett too1TB fel lehet too127GB.
* Egy vagy több lemezt tartalmazó adatbázis-kezelő hello jelentkezzen hello SAP adatbázis fájl- és adatbázis-kezelő ideiglenes tárterület hello hello naplófájlt (ha az adatbázis-kezelő hello támogatja ezt). Ha a hello adatbázis napló IOPS-követelmények magas, toostripe több lemez szükséges rendelés tooreach hello IOPS kötet szükséges.
* Lemezt tartalmazó egy vagy két adatbázis hello SAP-adatbázis és hello DBMS ideiglenes adatfájlok is (ha az adatbázis-kezelő hello támogatja ezt) száma.

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
hello lemezeinek száma miatt hello DBMS az adatfájlok és hello Azure a tároló típusa szerinti ezeknek a lemezeknek üzemeltetett használt hello IOPS követelmények és hello késés szükséges kell meghatározni. Ismerteti a pontos kvóták [Ez a cikk (Linux)] [ virtual-machines-sizes-linux] és [Ez a cikk (Windows)][virtual-machines-sizes-windows].

SAP telepítések keresztül hello utolsó 2 év élménye tanított velünk során néhány tapasztalatokat, amely mint összegezhető:

* IOPS forgalom toodifferent adatfájlok nincs mindig azonos óta a meglévő ügyfél rendszerek lehet, hogy rendelkezik másképp méretű adatok fájlok, a SAP adatbázis(ok) képviselő hello. Ennek eredményeképpen névszolgáltatások kimenő toobe jobban használatával több lemezek tooplace hello adatok fájlokat logikai egységek faragottnak ki azokat a RAID-konfigurációt. Nem voltak olyan helyzetekben, különösen a Azure standard szintű tárolást, ahol az IOPS-ráta találati hello hello DBMS tranzakciónapló szemben egyetlen lemezt beállított kvótát. Ilyen esetekben a prémium szintű Storage hello használata ajánlott, vagy egy szoftverek lemezek RAID azt is megteheti összesítése több standard szintű tárolót.

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

Ne feledje, hogy hello lemez, amely tartalmazza az operációs rendszer hello és, azt javasoljuk, hello SAP bináris fájljait és hello adatbázis (alap virtuális gép) is, már nem korlátozott too127GB. Most rendelkezhet mérete too1TB fel. Ez elég lemezterület tookeep összes hello, beleértve a szükséges fájl például kell, köteg feladatnaplóit SAP.

A további vonatkozó javaslatokat és adatbázis-kezelő virtuális gépeken, kifejezetten a további részletekért tekintse meg a hello [adatbázis-kezelő telepítési útmutatója][dbms-guide]

#### <a name="disk-handling"></a>Lemez kezelése
A legtöbb esetben toocreate további lemezek rendelés toodeploy hello SAP-adatbázisban történő hello VM kell. A fejezet lemezek számát megtartásról hello használata [virtuális gép/lemez-szerkezet SAP központi telepítések] [ planning-guide-5.5.1] ebben a dokumentumban. hello Azure-portál lehetővé teszi, hogy a tooattach és leválasztani a lemezeket, amennyiben egy alap virtuális Gépre van telepítve. hello lemezek csak csatlakoztatott vagy leválasztott amikor hello VM működik-e és fut, valamint ha le van állítva. Lemez csatlakoztatása, hello Azure-portálon kínál tooattach üres lemez vagy egy meglévő lemez, amely ezen a ponton a időben nem csatlakoztatott tooanother virtuális gép.

**Megjegyzés:**: lemezek csak lehet csatolt tooone virtuális gép egy adott időpontban.

![Csatlakoztassa / lemezeken Azure standard szintű tárolóval leválasztása][planning-guide-figure-1400]

Az új virtuális gépek hello a telepítés során eldöntheti, hogy szeretné, hogy toouse kezelt lemezek, vagy helyezze a lemezek Azure Storage-fiókok. Ha azt szeretné, hogy toouse prémium szintű Storage, kezelt lemezek használatát javasoljuk.

A következő toodecide kell, hogy azt szeretné, hogy egy új és üres lemez toocreate, vagy hogy szeretné-e tooselect meglévő lemez, amely korábban feltöltött, és most már csatlakoztatott toohello VM.

**FONTOS**: akkor **nem** állomás gyorsítótárazását Azure standard szintű tárolóval toouse szeretné. Hello állomás gyorsítótár beállítása None hello alapértelmezett hagyja. Prémium szintű Azure Storage kell engedélyeznie, olvasási gyorsítótárazás Ha hello i/o-jellemző többnyire olvasható például tipikus i/o-forgalom adatbázis adatok fájlokra vonatkozóan. Adatbázis-tranzakciós naplófájl esetén gyorsítótárazása használata ajánlott.

- - -
> ![Windows][Logo_Windows] Windows
>
> [Hogyan tooattach az adatok lemezre a hello Azure-portálon][virtual-machines-linux-attach-disk-portal]
>
> Lemezek vannak csatolva hozzá, ha van szüksége a toohello VM tooopen hello Windows lemezkezelő toolog. Ha automount nincs engedélyezve a fejezet ajánlott [beállítás automount a csatlakoztatott lemezekkel][planning-guide-5.5.3], hello újonnan csatolt kötet kell toobe online venni, illetve az inicializálása.
>
> ![Linux][Logo_Linux] Linux
>
> Lemezek vannak csatolva hozzá, ha a virtuális gép toohello toolog kell, és hello lemezek inicializálása a [Ez a cikk][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]
>
>

- - -
Ha új lemez hello üres lemez, szükség van, valamint a tooformat hello lemezre. Formázásához, különösen a DBMS adatok és a naplófájlok hello hello az operációs rendszer nélküli telepítések esetében azonos javaslatok DBMS alkalmazni.

Említettek szerint fejezetben [Microsoft Azure virtuális gép koncepció hello][planning-guide-3.2], egy Azure Storage-fiók nem rendelkezik végtelen erőforrásokkal tekintetében i/o-kötet, iops-érték és az adatok kötet. Adatbázis-kezelő virtuális gépek általában leginkább által érintett. Ajánlott toouse egy külön Tárfiókot az egyes virtuális gépek előfordulhat, ha i/o kötet néhány nagy virtuális gépek toodeploy rendelés toostay belül hello hello Azure Storage-fiók kötet található. Ellenkező esetben szüksége toosee hogyan anélkül, hogy elérte-e minden egyetlen Tárfiók hello legfeljebb különböző Storage-fiókok közötti virtuális gépeken is el tudnak osztani. További részletekért hello esik szó [adatbázis-kezelő telepítési útmutatója][dbms-guide]. Ezek a korlátozások is tiszta SAP alkalmazás kiszolgáló virtuális gépek vagy más virtuális gépek, amelyek idővel előfordulhat, hogy további virtuális merevlemezek figyelembe kell venni. Ezen korlátozás alól kivételt képez felügyelt lemezt használni. Ha azt tervezi, toouse prémium szintű Storage, azt javasoljuk, kezelt lemezre.

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
> * [A DiskPart parancssori kapcsolók](https://technet.microsoft.com/library/bb490893.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
>
> hello Windows parancssori ablakot rendszergazdaként kell megnyitni.
>
> Lemezek vannak csatolva hozzá, ha van szüksége a toohello VM tooopen hello Windows lemezkezelő toolog. Ha automount nincs engedélyezve a fejezet ajánlott [beállítás automount a csatlakoztatott lemezekkel][planning-guide-5.5.3], hello újonnan csatolt kötet > toobe online venni és inicializálva kell.
>
> ![Linux][Logo_Linux] Linux
>
> Szüksége van a tooinitialize egy újonnan csatolt üres lemez a [Ez a cikk][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
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
> * Hello utolsó lépésként hello varázsló hello szabály és menthet billentyűkombináció lenyomásával a "Befejezés"
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

Ahogy [hello SAP üzenet kiszolgáló biztonsági beállításait](https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm)

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>SAP-példányok csak felhőalapú telepítés fogalmak
### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Az SAP NetWeaver bemutató/képzési forgatókönyv egyetlen VM
![Egyetlen VM SAP bemutató rendszereket futtató hello az azonos virtuális gép nevét, egymástól el vannak különítve az Azure Cloud Services csomag][planning-guide-figure-1700]

Ebben a forgatókönyvben (lásd a fejezet [csak felhőalapú] [ planning-guide-2.1] a jelen dokumentum) végrehajtási azt egy tipikus képzési/bemutató rendszer forgatókönyv, ahol hello teljes képzési/bemutató forgatókönyvet tartalmazza egyetlen virtuális gépen. Feltételezzük, hogy hello telepítési Virtuálisgép-lemezkép sablonok keresztül történik. Azt is feltételezzük, hogy ezek bemutató/oktatási anyag virtuális gépek kell toobe hello hello rendelkező virtuális gépek telepítik, hogy többszöröse azonos nevet.

hello feltételezi, létrehozott egy Virtuálisgép-lemezkép fejezet egyes szakaszokban ismertetett módon [előkészítése virtuális gépek Azure-beli SAP] [ planning-guide-5.2] ebben a dokumentumban.

az események tooimplement hello forgatókönyvek hello feladatütemezési így néz ki:

##### <a name="powershell"></a>PowerShell
* Hozzon létre egy új erőforráscsoportot minden képzési/bemutató fekvő

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```
* Hozzon létre egy új tárfiókot, ha nem szeretné, hogy toouse kezelt lemezek

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
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES-SAP" -Skus "12-SP1" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "Oracle" -Offer "Oracle-Linux" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
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

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a Managed Disk Image
#####
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -Id <Id of Managed Disk Image>
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Set-AzureRmVMBootDiagnostics -Disable -VM $vmconfig
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Lehetősége van további lemezek hozzáadása, és állítsa vissza a szükséges tartalom. Vegye figyelembe, hogy az összes blob nevének (URL-címek toohello blobok) Azure belül egyedieknek kell lenniük.

```powershell
# Optional: Attach additional VHD data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM

# Optional: Attach additional Managed Disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -DiskSizeInGB 1023 -CreateOption empty -Lun 0 | Update-AzureRmVM
```

##### <a name="cli"></a>parancssori felület
a következő példakód hello Linux rendszeren használható. A Windows, vagy használjon PowerShell fent leírt módon vagy igazítja hello példa toouse % rgName %$rgName helyett, majd hello környezeti változót hello Windows paranccsal *beállítása*.

* Hozzon létre egy új erőforráscsoportot minden képzési/bemutató fekvő

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
az group create --name $rgName --location "North Europe"
```

* Új tárfiók létrehozása

```
az storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku Standard_LRS --name $rgNameLower
```

* Új virtuális hálózat létrehozása minden képzési/bemutató fekvő tooenable hello használatáról hello azonos állomásnévvel és IP-címek. hello virtuális hálózatot a hálózati biztonsági csoport, amely csak forgalom tooport 3389-es tooenable távoli asztal eléréséhez és 22-es portot az SSH védi.

```
az network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
az network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

az network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
az network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group SAPERPDemoNSG
```

* Hozzon létre egy új nyilvános IP-címet, amely használt tooaccess hello a virtuális gép hello internet

```
az network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --dns-name $rgNameLower --allocation-method Dynamic
```

* Hozzon létre egy új hálózati illesztő hello virtuális géphez

```
az network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-address SAPERPDemoPIP --subnet Subnet1 --vnet-name SAPERPDemoVNet
```

* Hozzon létre egy virtuális gépet. Hello csak felhőalapú forgatókönyvben minden virtuális gép lesz hello ugyanazzal a névvel. hello SAP SID-je hello virtuális gépek példánya lesz SAP NetWeaver hello azonos is. Belül hello Azure-erőforráscsoportot, hello VM hello nevét kell toobe egyedi, de különböző Azure erőforráscsoportok futtatható virtuális gépek a hello ugyanazzal a névvel. alapértelmezett "Rendszergazda" fiók a Windows hello vagy Linux rendszeren a "Gyökér" érvénytelen. Ezért egy másik rendszergazda felhasználónevet szükséges toobe definiált és jelszóval. hello VM hello mérete definiált toobe kell.

```
#####
# Create virtual machines using storage accounts
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --authentication-type password

#####
# Create virtual machines using Managed Disks
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image SUSE:SLES-SAP:12-SP1:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image RedHat:RHEL:7.2:latest --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --image "Oracle:Oracle-Linux:7.2:latest" --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --authentication-type password
```

```
#####
# Create a new virtual machine with a VHD that contains hello private image that you want toouse
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path tooimage vhd>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --size Standard_D11 --use-unmanaged-disk --storage-account $rgNameLower --storage-container-name vhds --os-disk-name os --image <path tooimage vhd> --authentication-type password

#####
# Create a new virtual machine with a Managed Disk Image
#####
az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id>
#az vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nics SAPERPDemoNIC --admin-username <username> --admin-password <password> --size Standard_DS11_v2 --os-disk-name os --image <managed disk image id> --authentication-type password
```

* Lehetősége van további lemezek hozzáadása, és állítsa vissza a szükséges tartalom. Vegye figyelembe, hogy az összes blob nevének (URL-címek toohello blobok) Azure belül egyedieknek kell lenniük.

```
# Optional: Attach additional VHD data disks
az vm unmanaged-disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --vhd-uri https://$rgNameLower.blob.core.windows.net/vhds/data.vhd  --new

# Optional: Attach additional Managed Disks
az vm disk attach --resource-group $rgName --vm-name SAPERPDemo --size-gb 1023 --disk datadisk --new
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

#### <a name="set-up-network-for-communication-between-hello-different-vms"></a>Hálózat közötti kommunikáció beállítása hello különböző virtuális gépek
![Egy Azure virtuális hálózaton belüli virtuális gépek halmaza][planning-guide-figure-1900]

ütközésének a klónok hello az azonos képzési/bemutató tájak tooprevent minden fekvő toocreate egy Azure virtuális hálózatra van szüksége. Azure DNS-név feloldása biztosítja, vagy beállíthatja, hogy a saját DNS-kiszolgáló Azure (nem Microsofttól további toobe) kívül. Ebben a forgatókönyvben azt a saját DNS konfigurálása nem. Az összes virtuális gép egy Azure virtuális hálózaton belül kommunikációs segítségével hostnames engedélyezve lesz.

hello tooseparate képzési vagy bemutató tájak virtuális hálózatok és nem csak erőforrás csoportok lehet okok miatt:

* hello SAP fekvő igényeinek beállított saját AD OpenLDAP és hello tájak mindegyikének a tartománykiszolgálóhoz igények toobe része.  
* hello SAP fekvő beállítása összetevők rendelkezik rögzített IP-címekkel rendelkező, hogy szükség toowork.

További információt az Azure virtuális hálózatok, és hogyan toodefine őket itt található: [Ez a cikk][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>SAP virtuális gépek telepítése a vállalati hálózati kapcsolat hibája (létesítmények közötti)
Egy SAP fekvő futtatja, és szeretné, hogy toodivide hello telepítése operációs rendszer nélküli csúcskategóriás DBMS kiszolgálók között, helyszíni virtualizált környezetekben alkalmazás rétegek és kisebb 2-réteg SAP rendszerek és az Azure infrastruktúra-szolgáltatási konfigurálva. hello alap feltételezése, SAP rendszerek egy SAP fekvő belül kell-e egymással, és számos egyéb szoftverösszetevők, hello vállalati, függetlenül a telepítési képernyőn az üzembe helyezett toocommunicate. Is kell a Kapcsolódás a SAP grafikus felhasználói felületének vagy egyéb felületek hello végfelhasználói hello telepítési űrlap által bevezetett nincs különbség. Csak akkor kell teljesíteni ezeket a feltételeket, amikor tudunk hello a helyszíni Active Directory/OpenLDAP, valamint a DNS-szolgáltatások kiterjesztett toohello Azure hely-a-site/több-site kapcsolatot vagy a magánhálózati kapcsolatok, például Azure ExpressRoute-rendszerekhez.

A sorrend tooget több Azure SAP hello megvalósítási részleteit a háttérben, javasoljuk, tooread fejezet [fogalmak a Cloud-Only telepítési SAP-példányok] [ planning-guide-7] ebben a dokumentumban, amelyből megtudhatja, Néhány hello alapjai hoz létre Azure, és hogyan ezeket kell használni az SAP-alkalmazások az Azure-ban.

### <a name="scenario-of-an-sap-landscape"></a>Egy SAP fekvő forgatókönyvet
hello létesítmények közötti forgatókönyv nagyjából jelentheti például az alábbi hello grafikus:

![A helyszíni és az Azure eszközök közötti pont-pont kapcsolat][planning-guide-figure-2100]

hello fenti forgatókönyvben egy olyan forgatókönyvet, ahol a hello a helyszíni AD/OpenLDAP és a DNS bővítve lettek tooAzure. Hello helyszíni oldalon egy bizonyos IP-címtartomány egy Azure-előfizetés van fenntartva. hello IP-címtartomány hello Azure oldalán az Azure Virtual Network tooan rendeli.

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
Elmúlt 12 hónap során hello Microsoft hozzáadott számos további Virtuálisgép-típusokon, amelyek eltérnek vagy Vcpu, memória számát, vagy fontosabb hardveren futó. Nem minden virtuális gépek támogatottak SAP (lásd a támogatott Virtuálisgép-típusokon SAP feljegyzés [1928533]). Néhány virtuális gépek futtasson másik gazdagépet hardver generációt. A gazdagép hardver generációja hello granularitási, egy Azure-Bővítőegysége első telepített. Azt jelenti, hogy olyan esetek is felmerülhetnek, ahol hello másik Virtuálisgép-méretek választotta nem futtatható a hello azonos Bővítőegysége. Rendelkezésre állási csoport hello képességét toospan Méretezőegységnek alapú különböző hardver van korlátozva.  A példa, ha azt szeretné, hogy toorun hello DBMS A5-A11 virtuális gépeken és hello SAP alkalmazásréteg G sorozatú virtuális gépeken, lehetővé válik a kényszerített toodeploy egyetlen SAP rendszer vagy másik rendelkezésre állási készletek belül különböző SAP rendszert.

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
> * Kövesse a hello nyilvános Linux útmutatói [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) vagy [Red Hat és Oracle Linux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) hogyan tooadd nyomtató.
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
> A hello Azure virtuális Gépen nyissa meg a Windows Explorer hello és hello nyomtató típusú a hello megosztási név.
> A nyomtató telepítővarázsló végigvezeti hello telepítési folyamat.
>
> ![Linux][Logo_Linux] Linux
>
> Néhány példa a hálózati nyomtatók konfigurálása a Linux vagy többek között a fejezet kapcsolatos dokumentáció a Linux nyomtatásra vonatkozó. Hello működni fog az Azure Linux virtuális gép virtuális gép hello is ugyanúgy VPN része:
>
> * SLES <https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba) _Share_or_Windows_Share>
> * RHEL vagy Oracle Linux <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sec-Printer_Configuration.html#s1-printing-smb-printer>
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

* Hello fejlesztői rendszer az Azure-ban, a go toohello rendszer (000 ügyfél) és a tranzakció STM. Egyéb konfigurációs lehetőségek közül választhat hello párbeszédpanel, és folytassa a rendszer közé tartozik a tartományban. Adja meg a célként megadott gazdagép hello tartományvezérlő ([SAP rendszerek beleértve hello átviteli tartomány](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). hello rendszer mostantól várakozási toobe hello átviteli tartomány része.
* Biztonsági okokból így már rendelkezik toogo hátsó toohello tartomány a tartományvezérlő tooconfirm a kérést. Válassza ki a rendszer áttekintése és jóváhagyása hello várakozási rendszer. Erősítse meg hello kérdés-hello konfigurációs lesznek kiosztva.

Az SAP rendszer hello átviteli tartományban más SAP rendszerek most tartalmaz hello összes hello szükséges információkat. Hello: ugyanaz, hello új SAP rendszer adatküldést tooall hello más SAP rendszerekkel, és hello SAP rendszer hello átviteli profil hello átviteli vezérlő program megadott hello cím eltöltött idő Ellenőrizze, hogy RFC-dokumentumokat és a hozzáférés toohello átviteli könyvtár hello tartomány működik.

A szokásos módon hello dokumentációjában leírt továbbra is hello konfigurációval, a rendszer [változás- és a rendszer](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

Útmutató:

* Ellenőrizze, hogy a helyszíni STM megfelelően van konfigurálva.
* Ellenőrizze, hogy hello átviteli tartományvezérlő állomásnevét hello megoldhatók a virtuális gép visa Azure és fordítva.
* Hívás, tranzakciót STM -> egyéb konfigurációs -> rendszer közé tartozik a tartományban.
* A helyi TMS rendszeren hello hello kapcsolat ellenőrzéséhez.
* Átviteli útvonalon, csoportok és rétegek konfigurálása a szokásos módon.

A pont-pont csatlakoztatott létesítmények közötti forgatókönyv, a helyszíni és az Azure közötti hello késés továbbra is lehet jelentős. Ha azt hajtsa végre az objektumok keresztüli fejlesztési és tesztelési rendszerek tooproduction szállítására hello sorozatát, vagy átvitelek alkalmazása gondolja át, vagy támogatási csomagokat toohello különböző rendszerek, akkor vegye figyelembe, hogy a függő központi átviteli hello hello helye könyvtár, hello rendszereivel fog történni nagy késleltetésű olvasása vagy adatok írása hello központi átviteli könyvtárban. hello helyzet akkor hasonló tooSAP fekvő konfigurációk, ahol hello különböző rendszerek keresztül hello adatközpontok jelentős távolsága a különböző adatközpontokban van elosztva.

Az ilyen késés körül toowork rendezés és gyors írásakor vagy olvasásakor tooor hello átviteli könyvtár, Azure és a hivatkozás hello átviteli tartományok beállíthat két STM átviteli tartományok (egy a helyszíni., a másik hello rendszerrel működnek hello rendszert Ellenőrizze az ebben a dokumentációban, amelyből megtudhatja, a fogalom a hello SAP TMS mögött hello alapelvek: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/ 38dd924eb711d182bf0000e829fbfe/FRAMESET.htm>.

Útmutató:

* (A helyszíni és az Azure) helyekre átviteli tartomány beállítása STM tranzakció használatával <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Hivatkozás hello tartományok tartománnyal rendelkező hivatkozásra, és erősítse meg a hello hivatkozás hello két tartomány között.
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Hello konfigurációs kapcsolódó toohello rendszer terjesztése.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>RFC forgalom Azure-ban és a helyszíni (létesítmények közötti) található SAP példányai között
RFC forgalom közötti rendszerek, amelyek a helyszíni és az Azure-ban toowork kell. kapcsolat tooset hívás tranzakció SM59 a forrásrendszerben toodefine kell az RFC-kapcsolat hello cél rendszer felé. hello konfigurálása hasonló toohello szabványos telepítése az RFC-kapcsolat számára.

Feltételezzük, hogy hello létesítmények közötti forgatókönyv, mely SAP rendszerek futnak egymással toocommunicate igénylő szerepelnek hello virtuális gépek hello ugyanabban a tartományban. Ezért hello egy SAP rendszerek közötti RFC-kapcsolat beállítása nem eltérnek a hello beállítási lépéseket és helyszíni forgatókönyvekben bemeneti adatok.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Az Azure-ban, vagy fordítva található SAP példányokból elérése során "local" fileshares
Az Azure-ban található SAP példányok kell tooaccess hello vállalati helyi tartoznak fájlmegosztások. Ezenkívül a helyszíni SAP példányok kell az Azure-ban található fájlmegosztások tooaccess. tooenable hello fájlmegosztások hello engedéllyel és megosztási beállítások hello helyi rendszeren kell konfigurálnia. Győződjön meg arról, hogy tooopen hello portok hello VPN vagy az Azure és az Adatközpont között ExpressRoute kapcsolat.

## <a name="supportability"></a>Támogatási lehetőségek
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Az Azure felügyeleti megoldás az SAP
Rendelés tooenable hello figyelési kritikus kritikus SAP rendszerek Azure hello SAP a Hálózatfigyelő eszközök SAPOSCOL vagy SAP a gazdagép ügynöke hello Azure virtuális gép szolgáltatásgazda egy Azure Figyelőbővítmény az SAP keresztül adatokat beolvasni. Óta hello iránti igények kielégítése érdekében SAP által meghatározott tooSAP alkalmazások, a Microsoft nem toogenerically megvalósítása hello szükséges funkciókat Azure úgy döntött, de hagyja a mezőt az ügyfelek toodeploy hello szükséges figyelési összetevők és konfigurációk tootheir Az Azure-ban futó virtuális gépek. Azonban központi telepítési és életciklusának kezelését a figyelési összetevők hello többnyire automatizált lesz az Azure-ban.

#### <a name="solution-design"></a>Megoldásterv
hello megoldást fejlesztett tooenable hello architektúra Azure Virtuálisgép-ügynök és a bővítmény keretrendszer SAP figyelési alapul. hello hello Azure Virtuálisgép-ügynök és a bővítmény keretrendszer lényege hello Azure Virtuálisgép-bővítmény gyűjtemény egy virtuális Gépen belül a rendelkezésre álló szoftver alkalmazás(ok) tooallow telepítését. hello elve (ilyen esetekben hello Azure Figyelőbővítmény az SAP) tooallow lényege mögött a fogalom, hello központi telepítését a központi telepítéskor a virtuális gép és hello szoftverhez konfigurációs speciális funkciókat.

hello "Azure Virtuálisgép-ügynök", amely lehetővé teszi adott Azure Virtuálisgép-bővítmények hello VM van be a nézetmodellbe, a Windows virtuális gépek alapértelmezés szerint a virtuális gép létrehozása a hello Azure-portálon belül kezelését. SUSE, Red Hat vagy Oracle Linux esetén hello Virtuálisgép-ügynök már része az Azure Piactéri lemezképhez. Abban az esetben, ha a Linux virtuális gép egy volna feltöltése a helyszíni tooAzure hello Virtuálisgép-ügynök van toobe manuálisan telepíteni.

hello alapvető építőelemeket, amelyekből hello figyelés megoldás az Azure-ban, az SAP néz ki:

![A Microsoft Azure bővítmény összetevők][planning-guide-figure-2400]

Ahogy fent hello blokk ábrán is látható, hello az SAP felügyeleti megoldás egyik részét képező hello Azure Virtuálisgép-lemezkép, és amely globálisan replikált tára Azure műveletei által felügyelt Azure-bővítmény katalógus tárolja. Feladata hello hello hello Azure műveletek toopublish új verzióihoz hello Azure Figyelőbővítmény az SAP SAP toowork Azure végrehajtásának dolgozik közös SAP/MS munkacsoport.

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

hello kezdeti portál URI http (s):`<Portalserver`>: 5XX00/irj ahol hello port alkot 50000 plusz (Systemnumber x 100). hello alapértelmezett portál URI az SAP 00 rendszer `<dns name`>.`<azure region` >.Cloudapp.azure.com:PublicPort/irj. További részletekért rá egy pillantást <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>.

![Végpont-konfiguráció][planning-guide-figure-2800]

Ha azt szeretné, toocustomize hello URL-címet és/vagy a portok az SAP vállalati portál, ellenőrizze a jelen dokumentáció:

* [Módosítsa a portál URL-címe](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL)
* [Alapértelmezett portszámok, Portal portszámok módosítása](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers)

## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Magas rendelkezésre állású (HA) és a katasztrófa utáni helyreállítás (DR) az Azure virtuális gépeken futó SAP NetWeaver
### <a name="definition-of-terminologies"></a>Terminológiát meghatározása
hello kifejezés **magas rendelkezésre ÁLLÁS** általában kapcsolódó tooa értéke technológia, amely minimálisra csökkenti az informatikai szolgáltatások keresztül IT szolgáltatások üzleti folytonosság megadásával redundáns, hibatűrő vagy feladatátvételi védett összetevők hello belül **azonos** adatközpont. Ebben az esetben egy Azure-régión belül.

**Vészhelyreállítás (DR)** is célzott IT szolgáltatások megszűnésének minimalizálja a és a helyreállítás azonban keresztben **különböző** adatközpontokban, amelyek általában több száz kilométer távolságra található. A mi esetünkben általában hello belül különböző Azure-régiók közötti geopolitikai ugyanabban a régióban vagy ügyfélként az Ön által létrehozott.

### <a name="overview-of-high-availability"></a>A magas rendelkezésre állás – Áttekintés
Azt is külön hello vitafórum SAP magas rendelkezésre állás az Azure-ban két részre kapcsolatban:

* **Azure-infrastruktúra magas rendelkezésre állású**, például magas rendelkezésre ÁLLÁSÚ (VM) számítási, hálózati, tárolási stb és az SAP rendelkezésre állásának növelése előnyeit.
* **SAP alkalmazás magas rendelkezésre állású**, például a magas rendelkezésre ÁLLÁSÚ az SAP szoftverösszetevőket:
  * SAP alkalmazáskiszolgálók
  * SAP ASC/SCS példány
  * Adatbázis-kiszolgálót

és hogyan azt kombinálható az Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ.

SAP a magas rendelkezésre állás, az Azure-ban van néhány eltérést tooSAP magas rendelkezésre állás a helyi fizikai vagy virtuális környezetben. hello SAP a következő dokumentum ismerteti Windows rendszeren virtualizált környezetben szabványos SAP magas rendelkezésre állású konfiguráció: <http://scn.sap.com/docs/DOC-44415>. Nincs sapinst integrált SAP-magas rendelkezésre ÁLLÁSÚ konfiguráció Linux például Windows létezik. Vonatkozó SAP magas rendelkezésre ÁLLÁSÚ Linux helyszíni további információkat itt találja: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Azure-infrastruktúra magas rendelkezésre állás
Jelenleg egy 99,9 %-os single-VM szolgáltatásiszint-szerződés. egy ötletet tooget hogyan egy virtuális hello rendelkezésre állását nézhet ki például egyszerűen hozhat létre hello szorzatát hello különböző elérhető Azure SLA-k: <https://azure.microsoft.com/support/legal/sla/>.

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
hello a Microsoft Azure Storage-fiók adatai mindig replikált tooensure tartósság és magas rendelkezésre állás érdekében hello Azure Storage szolgáltatásiszint-szerződés még átmeneti hardverhibák esetén hello felületét az értekezlet.

Mivel az Azure Storage alapértelmezés szerint megakadályozza a hello adatok három képek, RAID5 vagy több Azure lemezre RAID1 nem szükségesek.

További részletek megtalálhatók a cikkben: <http://azure.microsoft.com/documentation/articles/storage-redundancy/>

#### <a name="utilizing-azure-infrastructure-vm-restart-tooachieve-higher-availability-of-sap-applications"></a>Azure infrastruktúra virtuális gép újraindítása tooAchieve "Magasabb rendelkezésre állását" SAP-alkalmazásokból használata
Ha úgy dönt, hogy nem toouse funkciók, például a Windows Server feladatátvételi fürtszolgáltatási (WSFC) vagy a Linux támasztja (jelenleg csak a SLES 12 rendszert és az annál magasabb támogatott), Azure virtuális gép újraindítása túlterhelt tooprotect az SAP rendszer hello a tervezett és nem tervezett leállások ellen Fizikai kiszolgálók Azure-infrastruktúra és a teljes alapul szolgáló Azure platformon.

> [!NOTE]
> Fontos, hogy Azure virtuális gép újraindítása elsősorban védi a virtuális gépek és alkalmazásokat nem toomention. Indítsa újra a virtuális gép nem biztosít magas rendelkezésre állás a SAP-alkalmazásokból, de az infrastruktúra rendelkezésre állási bizonyos szintű kínálnak, ezért közvetve "magas rendelkezésre állás érdekében" SAP rendszerek. Hello ideig fog tartani, amíg toorestart egy virtuális Gépet egy gazdagépen tervezett vagy nem tervezett leállás után is van nem az SLA-t. Ez a metódus "a magas rendelkezésre állás" ezért nem alkalmas A SCS vagy az adatbázis-kezelő például SAP a rendszer kritikus összetevői.
>
>

A magas rendelkezésre állás fontos infrastruktúra elem az tároló. Például az Azure Storage szolgáltatásiszint-szerződés 99,9 % rendelkezésre állási. Ha egy telepíti a lemezzel rendelkező összes virtuális gép egy egyetlen Azure Storage-fiók esetén lehetséges Azure Storage elérhetetlensége, akkor kerülnek, Azure Storage-fiókhoz tartozó összes virtuális gépet, is minden SAP összetevőiről és virtuális gépek belül futtatott elérhetetlensége be.  

Ahelyett, hogy minden virtuális gép üzembe egy egyetlen Azure Storage-fiókot, használhatja a kijelölt tárhelycsomópont fiókokat az egyes virtuális gépek, és így általános virtuális gép és az SAP rendelkezésre állásának növeléséhez több független Azure Storage-fiókok használatával.

Azure-lemezeket felügyelt automatikusan kerülnek hello tartalék tartomány hello virtuális gép vannak csatolva. Ha két virtuális gép rendelkezésre állási és felügyelt lemezt használ, terjesztése hello felügyelt lemezek különböző tartalék tartományok, valamint a hello platform kezeli. Ha azt tervezi, toouse prémium szintű Storage, erősen ajánlott lemezek kezelése, valamint használatával.

Az SAP NetWeaver rendszer által használt Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ és a storage-fiókok egy mintaarchitektúrája nézhet ki:

![Magas rendelkezésre ÁLLÁSÚ tooachieve SAP alkalmazás "felső" rendelkezésre állási Azure-infrastruktúra használatával][planning-guide-figure-2900]

Egy Azure-infrastruktúra magas rendelkezésre ÁLLÁSÚ és felügyelt lemezeket használó SAP NetWeaver a rendszer mintaarchitektúrája nézhet ki:

![Magas rendelkezésre ÁLLÁSÚ tooachieve SAP alkalmazás "felső" rendelkezésre állási Azure-infrastruktúra használatával][planning-guide-figure-2901]

SAP-összetevők kritikus azt elért hello, amennyiben a következő:

* Magas rendelkezésre állású SAP Alkalmazáskiszolgáló (AS)

  SAP alkalmazáskiszolgáló-példányok a redundáns összetevők. Minden egyes SAP, példány telepítve van a saját virtuális Gépet, amely futtatja a egy másik Azure hiba és a tartomány frissítése (lásd: fejezetek [tartalék tartományok] [ planning-guide-3.2.1] és [frissítési tartományok] [ planning-guide-3.2.2]). Ez biztosítja Azure rendelkezésre állási csoportokkal (című [Azure rendelkezésre állási készletek][planning-guide-3.2.3]). Egy Azure hiba vagy frissítéséhez tartományi lehetséges tervezett vagy nem tervezett elérhetetlensége fog okozhat a virtuális gépek a korlátozott számú hiányában az SAP-AS példányt.

  Minden példány kerül a saját Azure Storage-fiók – lehetséges hiányában egy Azure Storage-fiók hiányában csak egy virtuális gép, akkor az SAP-AS az SAP példány. Azonban vegye figyelembe, hogy nincs maximális száma Azure Storage-fiókok egy Azure-előfizetéssel belül. Automatikus indítás tooensure (A) SCS példány után hello virtuális gép újraindul, ellenőrizze, hogy tooset hello Autostart paraméter (A) SCS példányban fejezetben ismertetett profil elindításához [használatával Autostart SAP-példányok] [ planning-guide-11.5].
  Olvassa el is fejezet [magas rendelkezésre állású SAP alkalmazáskiszolgálók] [ planning-guide-11.4.1] további részleteket.

  Akkor is, ha a lemez felügyelt használata esetén a lemezek tárolódnak az Azure-Tárfiók, és egy tárolási leállás az esemény nem érhető el.

* *Magasabb* SAP rendelkezésre állását (A) SCS példány

  Itt azt hasznosítani tooprotect hello Azure VM indítsa újra a virtuális gép telepített SAP (A) SCS-példánnyal. A hello kis-és az Azure tervezett vagy nem tervezett leállás kiszolgálója, virtuális gépek újraindul egy másik elérhető kiszolgálón. A korábban említett, elsősorban a virtuális gépek és alkalmazásokat nem Azure virtuális gép újraindítása védi, ebben az esetben hello (A) SCS példánya. Indítsa újra a virtuális gép hello keresztül fog eljut közvetve "magas rendelkezésre állás érdekében" SAP (A) SCS-példány. Automatikus indítás tooinsure (A) SCS példány után indítsa újra a virtuális gép hello, ellenőrizze, hogy tooset Autostart paraméter (A) SCS példányban fejezetben ismertetett profil elindításához [használatával Autostart SAP-példányok][planning-guide-11.5]. Ez azt jelenti, hogy hello (A) SCS példányát hibaként egyetlen pont, (SPOF) egy virtuális gépen futó lesz hello meghatározó tényező hello egész SAP fekvő hello rendelkezésre állását.

* *Magasabb* rendelkezésre állási adatbázis-kezelő kiszolgáló

  Itt hasonló toohello SAP (A) SCS példány-és nagybetűhasználattal, a Microsoft Azure virtuális gép újraindítása tooprotect hello VM telepített adatbázis-kezelő szoftverrel használják, és azt "magasabb rendelkezésre állásának eléréséhez" adatbázis-kezelő szoftver használatával indítsa újra a virtuális gép.
  Egyetlen virtuális gépen futó DBMS egyben a SPOF, és hello meghatározó tényező hello egész SAP fekvő hello rendelkezésre állását.

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP alkalmazás magas rendelkezésre állásra Azure IaaS
tooachieve teljes SAP rendszer magas rendelkezésre állású, igazolnia kell a tooprotect összes fontos SAP rendszer összetevők, például redundáns SAP alkalmazáskiszolgálók, és egyedi összetevők (például egyetlen pont, hiba) például SAP (A) SCS-példány és az adatbázis-kezelő.

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Az SAP alkalmazáskiszolgálók magas rendelkezésre állás
Hello SAP alkalmazáspéldányok kiszolgálók/párbeszédpanel esetén nem szükséges toothink egy adott magas rendelkezésre állású megoldás. Magas rendelkezésre állású egyszerűen valósul meg a redundancia és ezáltal rendelkező elegendő azokat a különböző virtuális gépek. Azok az összes kell helyezni, amelyek a virtuális gépek hello azonos Azure rendelkezésre állási csoport tooavoid előfordulhat, hogy frissíteni hello hello karbantartási tervezett leállások során ugyanannyi időt vesz igénybe. hello alapvető funkciókat épít, különböző frissítési és tartalék tartományok belül egy Azure méretezési egység, amely már jelent fejezetben [frissítési tartományok][planning-guide-3.2.2]. Az Azure rendelkezésre állási készletek fejezetben bemutatott [Azure rendelkezésre állási készletek] [ planning-guide-3.2.3] ebben a dokumentumban.

Nincs hiba, és a frissítési tartományok, amelyek segítségével Azure rendelkezésre állási csoport egy Azure méretezési egység belül korlátlan számú van. Ez azt jelenti, hogy a virtuális gépek száma üzembe egy rendelkezésre állási csoportban, előbb vagy később egynél több virtuális gép fejeződik be a hello azonos tartalék vagy tartomány frissítése.

A dedikált virtuális Gépeik néhány SAP-alkalmazáskiszolgáló telepítése példánya, és feltételezve, hogy azt kapott öt frissítési tartományok, a következő kép hello hello végén jelenik-e. hello tényleges maximális hiba és a frissítési tartományok száma belül egy rendelkezésre állási csoportot a jövőbeli hello változhatnak:

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

Hogyan található összes részletes tooinstall SIOS DataKeeper és SAP a Windows feladatátvevő fürtben lévő hello [fürtszolgáltatás SAP ASC példányhoz használatával Windows Server feladatátvevő fürt SIOS DataKeeper rendelkező Azure] [ ha-guide-classic]találhatók meg.

#### <a name="high-availability-for-hello-sap-ascs-instance-on-linux"></a>Magas rendelkezésre állású hello SAP (A) SCS példány Linux rendszeren
Től Dec 2015 nincs is egyenértékű tooshared lemez WSFC Azure Linux virtuális gépekhez. 3. fél szoftverrel SIOS a Windows-alternatív megoldásokat a rendszer nem érvényesíti még az SAP futó Azure Linux.

#### <a name="high-availability-for-hello-sap-database-instance"></a>Hello SAP adatbázispéldány magas rendelkezésre állás
hello szokásos SAP adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ telepítés alapul, a két adatbázis-kezelő virtuális gépeken, ahol adatbázis-kezelő magas rendelkezésre állású funkció használható hello aktív DBMS példány toohello tooreplicate adatait második virtuális gép be a passzív adatbázis-kezelő példánya.

Ismerteti a magas rendelkezésre állási és vészhelyreállítási helyreállítási funkcióhoz tartozó adatbázis-kezelő általános, valamint az olyan konkrét célrendszerben hello [adatbázis-kezelő telepítési útmutatója][dbms-guide].

#### <a name="end-to-end-high-availability-for-hello-complete-sap-system"></a>Végpontok közötti magas rendelkezésre állás a teljes SAP rendszert hello
Teljes SAP NetWeaver magas rendelkezésre ÁLLÁSÚ architektúrájának az Azure - két példa egy a Windows és Linux egyet.

Nem felügyelt csak lemezek: hello fogalmakat, ahogy az alábbi leírásban esetleg kicsit sérült esetén számos SAP rendszert telepít, és a telepített virtuális gépek hello száma túllépte hello maximális száma a tárfiókok előfizetésenként toobe. Ebben az esetben virtuális gépek virtuális merevlemezeket kell toobe együttesen egy Tárfiókon belül. Általában tennénk VHD-k az SAP alkalmazásréteg virtuális gépek különböző SAP rendszerek kombinálásával.  Azt is együtt a különböző VHD-k különböző adatbázis-kezelő virtuális gépek különböző SAP rendszerek egy Azure Storage-fiókot. Azure Storage-fiókok hello IOPS-korlátok vonatkoznak ezáltal tartva (<https://azure.microsoft.com/documentation/articles/storage-scalability-targets>)


##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] Magas rendelkezésre ÁLLÁSÚ Windows rendszeren
![SAP NetWeaver alkalmazás magas rendelkezésre ÁLLÁSÚ architektúra Azure IaaS SQL-kiszolgálót][planning-guide-figure-3200]

a következő Azure szerkezetek hello hello SAP NetWeaver rendszer infrastrukturális problémára és javítását állomás toominimize hatás használják:

* a rendszer teljes hello Azure (kötelező – adatbázis-kezelő réteg, (A) SCS példány és a teljes alkalmazás réteg kell toorun a hello ugyanazon a helyen) van telepítve.
* hello teljes rendszer fut egy Azure-előfizetéssel (kötelező) belül.
* hello teljes rendszert futtat egy Azure virtuális hálózaton belül (kötelező).
* a három rendelkezésre állási készletek hello hello virtuális gépek egy SAP rendszer elkülönítése lehetőség toohello tartozó hello virtuális gépek mellett is ugyanazt a virtuális hálózatot.
* Adatbázis-kezelő példányok egy SAP rendszer futó összes virtuális gép van egy rendelkezésre állási csoportban. Feltételezzük, hogy van-e az adatbázis-kezelő példányok esetében a rendszer futtató óta natív adatbázis-kezelő magas rendelkezésre funkcióit használják, mint például az SQL Server AlwaysOn vagy Oracle Data Guard egynél több virtuális.
* Adatbázis-kezelő példányok futó összes virtuális gép saját tárfiókot használni. Adatbázis-kezelő adatainak és naplókönyvtárainak fájlok lesznek replikálva az egy tárolási fiók tooanother tárfiók hello adatok szinkronizálása az adatbázis-kezelő magas rendelkezésre állású funkciókat használja. Egy tárfiókot elérhetetlensége miatt hiányában egy SQL-Windows-fürt csomópontja, de nem hello teljes SQL Server szolgáltatás.
* (A) SCS példányát egy SAP rendszert futtató összes virtuális gép van egy rendelkezésre állási csoportban. A Windows Server feladatátvételi fürt (WSFC) belül e virtuális gépek tooprotect hello (A) SCS példány van konfigurálva.
* (A) SCS példányok futó összes virtuális gép saját tárfiókot használni. (A) Egy tárolási fiók tooanother tárfiók SIOS DataKeeper replikációval SCS példány fájlok és a SAP globális mappa replikálódnak. Hiányában egy tárfiókot, akkor egy (A) elérhetetlensége SCS Windows fürtcsomópontra, de nem teljes hello (A) SCS szolgáltatás.
* Hello SAP alkalmazásréteg server képviselő hello virtuális gépek vannak egy harmadik rendelkezésre állási csoportban.
* SAP alkalmazáskiszolgálók futó összes hello virtuális gép saját tárfiókot használni. Egy tárfiókot hiányában egy SAP alkalmazáskiszolgáló, ha más SAP-AS továbbra is toorun elérhetetlensége miatt.

hello következő azonos fekvő felügyelt lemezekkel illusztrált hello. ábra.

![SAP NetWeaver alkalmazás magas rendelkezésre ÁLLÁSÚ architektúra Azure IaaS SQL-kiszolgálót][planning-guide-figure-3201]

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] Magas rendelkezésre ÁLLÁSÚ Linux rendszeren
az SAP magas rendelkezésre ÁLLÁSÚ Azure Linux hello architektúra tulajdonképpen ugyanaz, mint a Windows, mint a fent leírt hello. 2016. január jelenleg nincs SAP (A) SCS magas rendelkezésre ÁLLÁSÚ megoldás még az Azure-on Linux rendszeren támogatott

Következtében 2016 januárjának az SAP-Linux-Azure rendszer elérése nem egy SAP-Windows-Azure rendszer azonos rendelkezésre állási hello miatt hello (A) SCS példány hiányzik a magas rendelkezésre ÁLLÁSÚ és hello egypéldányos SAP ASE adatbázis.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Automatikus indítás SAP-példányok használata
SAP toostart SAP példányok kínált hello funkció hello hello OS belül hello virtuális gép elindítása után azonnal. hello pontos lépéseket a volt részletes ismertetését lásd a Tudásbázis következő cikke SAP [1909114]. Azonban SAP van nem ajánló toouse hello beállítása többé nincs vezérlő hello sorrendben példány újraindul, mert egynél több virtuális gép van hatással, vagy több futtatott virtuális gépenként feltételezve. Feltéve, hogy a jellemző Azure forgatókönyv egy SAP application server-példány egyetlen virtuális gép újraindítása végül első virtuális gép és hello esetén, hello Autostart nincs igazán fontos, és ez a paraméter hozzáadásával engedélyezhető:

    Autostart = 1

A hello hello SAP ABAP és/vagy a Java-példány profil elindításához.

> [!NOTE]
> hello Autostart paraméter néhány downfalls is rendelkezhet. Részletesebben hello paraméter eseményindítók hello kapcsolatos Windows/Linux hello példány szolgáltatás hello SAP ABAP vagy Java példány elindítása. Hogy biztosan helyzet hello olyankor, amikor elindul a hello operációs rendszer. Azonban az SAP-szolgáltatások újraindítása is egy közös lépésként SAP szoftver életciklus-kezelési funkciók, például a SUM vagy más frissítéseket, vagy frissíti. Ezek a funkciók nem vár egy példány toobe összes automatikusan újraindul. Ezért hello Autostart paraméter le kell tiltani az ilyen feladatok futtatása előtt. hello Autostart paraméternek is nem használható vannak foglalva, ASC/SCS/CI például SAP-példány.
>
>

Tekintse meg a további információhoz autostart az SAP-példányok itt:

* [SAP, valamint a Unix kiszolgáló indítása/leállítása indítása/leállítása](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Indítása és leállítása SAP NetWeaver felügyeleti ügynökök](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Hogyan tooenable automatikus indítsa el a HANA-adatbázisból](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)

### <a name="larger-3-tier-sap-systems"></a>Nagyobb 3 szintű SAP rendszerek
Magas rendelkezésre állású aspektusainak 3 szintű SAP konfigurációk kapott tárgyalt korábbi szakaszokban már. De mi a helyzet a rendszerek, ahol hello DBMS Kiszolgálókövetelmények túl nagyok toohave azt található az Azure-ban, de hello SAP alkalmazásréteg is telepíthető az Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>3 rétegű SAP konfigurációk helye
Már nem támogatott toosplit hello alkalmazás réteg saját magát vagy hello alkalmazás és a helyszíni és az Azure közötti adatbázis-kezelő réteg. Az SAP-rendszer teljesen telepített helyi vagy az Azure-ban. Nincs is támogatott toohave néhány hello alkalmazáskiszolgálók futtassa a helyszíni és más Azure-ban. Ez a kezdőpont hello Vitafórum hello. Azt is toohave hello adatbázis-kezelő összetevők SAP a rendszer nem támogatja, és a SAP server alkalmazásréteg két különböző Azure-régiókban telepített hello. USA nyugati régiója és SAP alkalmazás rétegben Államok középső például DBMS. Nem támogatja az ilyen konfigurációt hello SAP NetWeaver architektúra érzékenységének hello késés.

Azonban az elmúlt évben adatok hello folyamán center partnerek közös helyek tooAzure régiók fejlesztett ki. Ezeket a helyeket közös gyakran szerepelnek nagyon közel toohello fizikai az Azure data központok belül egy Azure-régiót. hello rövid távolság és az eszközök hello közös elhelyezés-ExpressRoute keresztül az Azure a kapcsolat a késési, amely kisebb, mint 2ms eredményezhet. Ilyen esetben toolocate hello DBMS réteg (beleértve a tárolási SAN/NAS) egy közös elhelyezés és hello SAP alkalmazásréteg az Azure-ban lehetőség. Től Dec 2015 jelenleg nem áll a telepítések hasonlóan. Azonban nem SAP alkalmazástelepítések rendelkező, különböző ügyfelektől már ilyen megközelítések használ.

### <a name="offline-backup-of-sap-systems"></a>Offline biztonsági másolat az SAP-rendszerek
Hello függ (rétegének 2 vagy 3 szintű) van kiválasztva az SAP-konfiguráció oka lehet egy szükséges tooback fel. hello tartalom hello virtuális gépért plusz toohave hello adatbázis biztonsági másolatát. hello DBMS kapcsolatos biztonsági mentések várt toobe adatbázis-módszerekkel együtt történik. Hello a különböző adatbázisokhoz, részletes leírását megtalálhatók [DBMS útmutató][dbms-guide]. A hello ugyanakkor, hello SAP-adatok biztonsági másolat készíthető (hello adatbázis tartalmát is beleértve) offline módon ebben a szakaszban leírt módon vagy online hello a következő szakaszban leírtak szerint.

hello offline biztonsági másolat alapvetően igényelnének hello hello Azure-portálon keresztül a virtuális gép leállítására és hello alap virtuális lemez és az összes csatlakoztatott lemezek toohello virtuális gép másolatát. Ezzel megőrzik ponttá a virtuális gép hello idő kép és a kapcsolódó lemezt. Ajánlott toocopy hello "biztonsági mentések" be egy másik Azure Storage-fiókot. Ezért az eljárást a következő fejezetben hello [Azure Storage-fiókok között a lemezek másolásának] [ planning-guide-5.4.2] ebben a dokumentumban csak akkor vonatkozik.
Hello leállítási hello Azure használata mellett egy portál is megteheti Powershell vagy a parancssori felületen keresztül itt: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Visszaállítás, az állapotban, amelyek hello törlésével alap virtuális gép is, az eredeti lemezek hello hello kiinduló virtuális gép és csatlakoztatott lemezek másolása vissza hello mentett lemezek toohello eredeti Tárfiók vagy erőforrás csoport felügyelt lemezek és hello rendszer majd újratelepítésével.
Ez a cikk azt szemlélteti, hogyan tooscript ez feldolgozható Powershell: <http://www.westerndevs.com/azure-snapshots/>

Ellenőrizze, hogy meg arról, hogy tooinstall új SAP licencet, mivel a virtuális gép biztonsági mentését, fent leírt módon állítja vissza létrehoz egy új hardver kulcsot.

### <a name="online-backup-of-an-sap-system"></a>Az SAP rendszer online biztonsági mentés
Biztonsági másolatot az adatbázis-kezelő történik az adatbázis-kezelő-specifikus módszerekkel hello leírtak hello [DBMS útmutató][dbms-guide].

A többi virtuális gépen belül hello SAP rendszer készíthető funkciójával Azure virtuális gépek biztonsági mentéséhez. Az Azure virtuális gépek biztonsági mentéséhez korai 2015 kapott bevezetett és eközben egy szabványos metódus tooback teljes virtuális gépet az Azure-ban. Azure biztonsági mentés hello biztonsági mentések Azure-ban tárolja, és lehetővé teszi, hogy a virtuális gépek visszaállítási újra.

> [!NOTE]
> Től Dec 2015 VM biztonsági mentéssel megőrzése nélkül hello egyedi virtuális gép azonosítója SAP használt licencelési. Ez azt jelenti, hogy a virtuális gép biztonsági másolat visszaállítása szükséges telepítése egy új SAP licenckulcs as hello vissza VM tekinthető toobe egy új virtuális Gépet, és nem helyettesíti a korábbi egy mentett.
>
> ![Windows][Logo_Windows] Windows
>
> Elméletileg, virtuális gépek, amelyek futtatási adatbázisok biztonsági másolat készíthető következetesen, valamint ha hello adatbázis-kezelő rendszer támogatja a Windows VSS hello (kötet árnyékmásolata szolgáltatás <https://msdn.microsoft.com/library/windows/desktop/bb968832 (v=VS.85).aspx megfelelő >), mint például az SQL létezik.
> Azonban figyelembe időpontban adatbázisok visszaállítása Azure virtuális gép biztonsági másolatok alapján, amelyek nem lehetséges. A javaslat ezért ahelyett, hogy az Azure virtuális gép biztonsági mentése az adatbázis-kezelő funkció adatbázisokkal tooperform biztonsági másolatait.
>
> Azure virtuális gépek biztonsági mentéséhez ismernie tooget indítsa el a itt: <https://docs.microsoft.com/azure/backup/backup-azure-vms>.
>
> Egyéb lehetőségek toouse Microsoft Data Protection Manager együttes telepítve egy Azure virtuális gép és egy Azure Backup szolgáltatás biztonsági mentési/visszaállítási adatbázisokhoz. További információk itt találhatók: <https://docs.microsoft.com/azure/backup/backup-azure-dpm-introduction>.  
>
> ![Linux][Logo_Linux] Linux
>
> Nincs egyenértékű tooWindows lévő Linux VSS van. Ezért csak a fájlkonzisztens biztonsági mentések is lehetséges, de nem alkalmazáskonzisztens biztonsági mentés. hello SAP DBMS biztonsági mentést kell végezni az adatbázis-kezelő szolgáltatással. hello hello SAP kapcsolatos adatokat tartalmazó fájlrendszer menthetők, például használatával bont lásd itt: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>
>
>

### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Az éles SAP tájak vész-Helyreállítási helyként Azure
Mid 2014 óta bővítmények toovarious összetevőket Hyper-V, a System Center és az Azure hello használatának engedélyezése Azure vész-Helyreállítási helyként helyszíni Hyper-v rendszerű virtuális gépekhez.

Egy blog, és részletesen leírja, hogyan toodeploy ehhez a megoldáshoz az itt dokumentált lehetőségektől: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>.

## <a name="summary"></a>Összefoglalás
hello legfontosabb pontjait magas rendelkezésre állású SAP rendszerekhez az Azure-ban a következők:

* Ezen a ponton az időben, hello SAP hibaérzékeny pontok kialakulását nem védhető pontosan hello ugyanaz, mint a helyszíni telepítések esetén is elvégezhető. hello oka, hogy a megosztott lemezfürtöket nem még hozható létre az Azure-ban 3. fél szoftver hello használata nélkül.
* Hello DBMS réteg van szüksége, amely nem függ a megosztott lemez fürt technológia toouse adatbázis-kezelő funkcióit. Részletek ismertetett hello [DBMS útmutató][dbms-guide].
* toominimize hello hatás hello Azure infrastruktúra vagy a gazdagép karbantartási tartalék tartományok belül előforduló problémákról, az Azure rendelkezésre állási készletek kell használnia:
  * Ajánlott toohave egy rendelkezésre állási készlet hello SAP alkalmazásréteg.
  * Egy külön rendelkezésre állási készlet hello SAP DBMS réteg toohave ajánlott.
  * Virtuális gépek különböző SAP rendszerek azonos rendelkezésre állási készlet tooapply hello nem ajánlott.
  * Ajánlott toouse Premium felügyelt lemezek.
* Biztonsági mentés céljából hello SAP DBMS réteg, tekintse meg az hello [DBMS útmutató][dbms-guide].
* SAP párbeszédpanel példányok biztonsági másolatának szabálykészletében kevés mert általában gyorsabb tooredeploy egyszerű párbeszédpanel példányok.
* Biztonsági mentés hello VM hello globális directory hello SAP rendszer és az azt tartalmazó összes hello-profil hello különböző példányok, célszerű és kell végezhető el a Windows biztonsági másolat vagy, például a Linux bont. Mivel a Windows Server 2008 (R2) és a Windows Server 2012 (R2), amelyek révén könnyebben tooback be hello segítségével közötti különbségek újabb Windows Server kiadásai, azt javasoljuk, hogy a Windows Server 2012 (R2) Windows vendég operációs rendszerként toorun.
