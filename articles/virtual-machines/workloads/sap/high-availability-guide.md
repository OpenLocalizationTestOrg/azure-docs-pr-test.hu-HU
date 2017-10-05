---
title: "Az Azure virtuális gépek magas rendelkezésre állás a SAP NetWeaver |} Microsoft Docs"
description: "Magas rendelkezésre állású útmutatója SAP NetWeaver Azure virtuális gépeken"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/07/2016
ms.author: goraco
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 65236f527b62b4990b062fb6a54ce13b3c182e93
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a><span data-ttu-id="2b2f4-103">Magas rendelkezésre állás a SAP NetWeaver Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="2b2f4-103">High availability for SAP NetWeaver on Azure VMs</span></span>

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

[sap-installation-guides]:http://service.sap.com/instguides

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:https://docs.microsoft.com/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md
[dbms-guide-2.1]:../../virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f
[dbms-guide-2.2]:../../virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91
[dbms-guide-2.3]:../../virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152
[dbms-guide-2]:../../virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64
[dbms-guide-3]:../../virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3
[dbms-guide-5.5.1]:../../virtual-machines-windows-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268
[dbms-guide-5.5.2]:../../virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b
[dbms-guide-5.6]:../../virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8
[dbms-guide-5.8]:../../virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30
[dbms-guide-5]:../../virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737
[dbms-guide-8.4.1]:../../virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573
[dbms-guide-8.4.2]:../../virtual-machines-windows-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d
[dbms-guide-8.4.3]:../../virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c
[dbms-guide-8.4.4]:../../virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818
[dbms-guide-900-sap-cache-server-on-premises]:../../virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:../../virtual-machines-windows-sap-deployment-guide.md
[deployment-guide-2.2]:../../virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94
[deployment-guide-3.1.2]:../../virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab
[deployment-guide-3.2]:../../virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281
[deployment-guide-3.3]:../../virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2
[deployment-guide-3.4]:../../virtual-machines-windows-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1
[deployment-guide-3]:../../virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[deployment-guide-4.1]:../../virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7
[deployment-guide-4.2]:../../virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e
[deployment-guide-4.3]:../../virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc
[deployment-guide-4.4.2]:../../virtual-machines-windows-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542
[deployment-guide-4.4]:../../virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d
[deployment-guide-4.5.1]:../../virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4
[deployment-guide-4.5.2]:../../virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f
[deployment-guide-4.5]:../../virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca
[deployment-guide-5.1]:../../virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2
[deployment-guide-5.2]:../../virtual-machines-windows-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:../../virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8

[deployment-guide-configure-monitoring-scenario-1]:../../virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b
[deployment-guide-configure-proxy]:../../virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:../../virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:../../virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:../../virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:../../virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:../../virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:../../virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:../../virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:../../virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:../../virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../../../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../../../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:../../virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:../../virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:../../virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:../../virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:high-availability-guide.md

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
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:high-availability-guide.md
[sap-ha-guide-1]:high-availability-guide.md#217c5479-5595-4cd8-870d-15ab00d4f84c
[sap-ha-guide-2]:high-availability-guide.md#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-3]:high-availability-guide.md#42156640c6-01cf-45a9-b225-4baa678b24f1
[sap-ha-guide-3.1]:high-availability-guide.md#f76af273-1993-4d83-b12d-65deeae23686
[sap-ha-guide-3.2]:high-availability-guide.md#3e85fbe0-84b1-4892-87af-d9b65ff91860
[sap-ha-guide-4]:high-availability-guide.md#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-4.1]:high-availability-guide.md#1a3c5408-b168-46d6-99f5-4219ad1b1ff2
[sap-ha-guide-5]:high-availability-guide.md#fdfee875-6e66-483a-a343-14bbaee33275
[sap-ha-guide-5.1]:high-availability-guide.md#be21cf3e-fb01-402b-9955-54fbecf66592
[sap-ha-guide-5.2]:high-availability-guide.md#ff7a9a06-2bc5-4b20-860a-46cdb44669cd
[sap-ha-guide-6]:high-availability-guide.md#2ddba413-a7f5-4e4e-9a51-87908879c10a
[sap-ha-guide-6.1]:high-availability-guide.md#1a464091-922b-48d7-9d08-7cecf757f341
[sap-ha-guide-6.2]:high-availability-guide.md#44641e18-a94e-431f-95ff-303ab65e0bcb
[sap-ha-guide-7]:high-availability-guide.md#2e3fec50-241e-441b-8708-0b1864f66dfa
[sap-ha-guide-7.1]:high-availability-guide.md#93faa747-907e-440a-b00a-1ae0a89b1c0e
[sap-ha-guide-7.2]:high-availability-guide.md#f559c285-ee68-4eec-add1-f60fe7b978db
[sap-ha-guide-7.2.1]:high-availability-guide.md#b5b1fd0b-1db4-4d49-9162-de07a0132a51
[sap-ha-guide-7.3]:high-availability-guide.md#ddd878a0-9c2f-4b8e-8968-26ce60be1027
[sap-ha-guide-7.4]:high-availability-guide.md#045252ed-0277-4fc8-8f46-c5a29694a816
[sap-ha-guide-8]:high-availability-guide.md#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:high-availability-guide.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.2]:high-availability-guide.md#7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310
[sap-ha-guide-8.3]:high-availability-guide.md#47d5300a-a830-41d4-83dd-1a0d1ffdbe6a
[sap-ha-guide-8.4]:high-availability-guide.md#b22d7b3b-4343-40ff-a319-097e13f62f9e
[sap-ha-guide-8.5]:high-availability-guide.md#9fbd43c0-5850-4965-9726-2a921d85d73f
[sap-ha-guide-8.6]:high-availability-guide.md#84c019fe-8c58-4dac-9e54-173efd4b2c30
[sap-ha-guide-8.7]:high-availability-guide.md#7a8f3e9b-0624-4051-9e41-b73fff816a9e
[sap-ha-guide-8.8]:high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.9]:high-availability-guide.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.10]:high-availability-guide.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c
[sap-ha-guide-8.11]:high-availability-guide.md#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:high-availability-guide.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:high-availability-guide.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.2]:high-availability-guide.md#e49a4529-50c9-4dcf-bde7-15a0c21d21ca
[sap-ha-guide-8.12.2.1]:high-availability-guide.md#06260b30-d697-4c4d-b1c9-d22c0bd64855
[sap-ha-guide-8.12.2.2]:high-availability-guide.md#4c08c387-78a0-46b1-9d27-b497b08cac3d
[sap-ha-guide-8.12.3]:high-availability-guide.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:high-availability-guide.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:high-availability-guide.md#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-ha-guide-9.1.2]:high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0
[sap-ha-guide-9.1.3]:high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556
[sap-ha-guide-9.1.4]:high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761
[sap-ha-guide-9.1.5]:high-availability-guide.md#4498c707-86c0-4cde-9c69-058a7ab8c3ac
[sap-ha-guide-9.2]:high-availability-guide.md#85d78414-b21d-4097-92b6-34d8bcb724b7
[sap-ha-guide-9.3]:high-availability-guide.md#8a276e16-f507-4071-b829-cdc0a4d36748
[sap-ha-guide-9.4]:high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a
[sap-ha-guide-9.5]:high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5
[sap-ha-guide-9.6]:high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772
[sap-ha-guide-10]:high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9
[sap-ha-guide-10.1]:high-availability-guide.md#65fdef0f-9f94-41f9-b314-ea45bbfea445
[sap-ha-guide-10.2]:high-availability-guide.md#5e959fa9-8fcd-49e5-a12c-37f6ba07b916
[sap-ha-guide-10.3]:high-availability-guide.md#755a6b93-0099-4533-9f6d-5c9a613878b5

[sap-ha-multi-sid-guide]:high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../../../azure-resource-manager/resource-group-overview.md
[resource-groups-networking]:../../../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
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
[virtual-machines-windows-attach-disk-portal]:../../virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../../../azure-resource-manager/resource-group-overview.md
[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md
[virtual-machines-linux-agent-user-guide]:../../linux/agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:../../linux/agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:../../linux/capture-image.md
[virtual-machines-linux-capture-image-capture]:../../linux/capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:../../virtual-machines-windows-capture-image.md
[virtual-machines-windows-capture-image-capture]:../../virtual-machines-windows-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-lvm]:../../linux/configure-lvm.md
[virtual-machines-linux-configure-raid]:../../linux/configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:../../virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:../../linux/suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:../../linux/redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:../../linux/add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:../../linux/add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:../../linux/quick-create-cli.md
[virtual-machines-linux-update-agent]:../../linux/update-agent.md
[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:../../virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:../../virtual-machines-windows-sizes.md
[virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md
[virtual-machines-windows-portal-sql-alwayson-int-listener]:../../windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md
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
[vpn-gateway-site-to-site-create]:../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../../../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../../../cli-install-nodejs.md
[xplat-cli-azure-resource-manager]:../../../xplat-cli-azure-resource-manager.md


<span data-ttu-id="2b2f4-109">Az Azure virtuális gépek egy a megoldás olyan szervezeteknek, amelyek szükség van a számítási, tárolási és hálózati erőforrásokat, minimális időben, valamint hosszú beszerzési ciklusok nélkül.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-109">Azure Virtual Machines is the solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="2b2f4-110">Azure virtuális gépek például SAP NetWeaver alapú ABAP, Java és egy ABAP + Java verem klasszikus alkalmazások központi telepítéséhez használhatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-110">You can use Azure Virtual Machines to deploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="2b2f4-111">Megbízhatóság és rendelkezésre állás további helyszíni erőforrások nélkül kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="2b2f4-112">Az Azure virtuális gépek közötti kapcsolatot nyújthassanak, támogatja, így a Azure Virtual Machines integrálása a szervezete helyszíni tartományok, magánfelhőket és SAP rendszer fekvő.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="2b2f4-113">Ebben a cikkben azt a lépéseket, amelyek tudja telepíteni a magas rendelkezésre állású SAP rendszerek az Azure-ban Azure Resource Manager telepítési modellel fedik le.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-113">In this article, we cover the steps that you can take to deploy high-availability SAP systems in Azure by using the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="2b2f4-114">A Microsoft végigvezetik Önt a nagyobb feladatok:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="2b2f4-115">Keresse meg a megfelelő SAP megjegyzések és a telepítési útmutatók, szerepel a [erőforrások] [ sap-ha-guide-2] szakasz.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-115">Find the right SAP Notes and installation guides, listed in the [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="2b2f4-116">Ez a cikk kiegészíti az SAP-dokumentáció és az SAP megjegyzések, amelyek az elsődleges erőforrások, amelyek segítségével telepítse, és az adott platformon SAP szoftver központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-116">This article complements SAP installation documentation and SAP Notes, which are the primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="2b2f4-117">További tudnivalók az Azure Resource Manager telepítési modell és az Azure klasszikus üzembe helyezési modellel közötti különbségeket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-117">Learn the differences between the Azure Resource Manager deployment model and the Azure classic deployment model.</span></span>
* <span data-ttu-id="2b2f4-118">További információk a Windows Server feladatátvételi fürtszolgáltatási kvórummódok, így kiválaszthatja a modellt az Azure-telepítés számára megfelelő.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-118">Learn about Windows Server Failover Clustering quorum modes, so you can select the model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="2b2f4-119">További információk a Windows Server feladatátvételi fürtszolgáltatási megosztott tár az Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="2b2f4-120">Ismerje meg, hogyan védheti az egyetlen ponton-az-hibát jelentő összetevők például a fejlett üzleti alkalmazások fejlesztői (ABAP) SAP központi szolgáltatások (ASC) / SAP központi szolgáltatások (SCS) és az adatbázis-kezelő rendszerek (DBMS) és a redundáns összetevők, például SAP-alkalmazás Kiszolgáló, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-120">Learn how to help protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="2b2f4-121">Kövesse a részletes példa egy telepítési és a konfiguráció egy magas rendelkezésre állású SAP rendszer Windows Server feladatátvételi fürtszolgáltatási fürtben az Azure-ban Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="2b2f4-122">Ismerje meg a Windows Server feladatátvételi fürtszolgáltatási Azure, de amelyek nem szükségesek helyszíni telepítés használatához szükséges további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-122">Learn about additional steps required to use Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="2b2f4-123">Egyszerűbbé teheti a üzembe helyezését és konfigurálását, ebben a cikkben az SAP háromrétegű magas rendelkezésre állású Resource Manager-sablonok használjuk.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-123">To simplify deployment and configuration, in this article, we use the SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="2b2f4-124">A sablonok automatizálni, amelyekre szüksége van a magas rendelkezésre állású SAP rendszert a teljes infrastruktúra telepítését.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-124">The templates automate deployment of the entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="2b2f4-125">Az infrastruktúra SAP alkalmazás teljesítmény szabványos (SAP) méretezése a SAP rendszert is támogatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-125">The infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="2b2f4-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2b2f4-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="2b2f4-127">Mielőtt elkezdené, győződjön meg arról, hogy teljesülnek-e az előfeltételeket, amelyeket a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-127">Before you start, make sure that you meet the prerequisites that are described in the following sections.</span></span> <span data-ttu-id="2b2f4-128">Emellett ügyeljen arra, hogy a felsorolt összes erőforrást ellenőrizze a [erőforrások] [ sap-ha-guide-2] szakasz.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-128">Also, be sure to check all resources listed in the [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="2b2f4-129">Ebben a cikkben az Azure Resource Manager sablonok használjuk [háromrétegű SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span></span> <span data-ttu-id="2b2f4-130">Sablonok hasznos áttekintéséért lásd: [SAP Azure Resource Manager-sablonok](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="2b2f4-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Erőforrások</span><span class="sxs-lookup"><span data-stu-id="2b2f4-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="2b2f4-132">Ezek a cikkek SAP üzemelő példányok Azure terjed ki:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="2b2f4-133">[Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="2b2f4-134">[SAP NetWeaver Azure virtuális gépek központi telepítését][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="2b2f4-135">[Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="2b2f4-136">[Az Azure virtuális gépek magas rendelkezésre állás a SAP NetWeaver (Ez az útmutató)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-137">Amikor csak lehetséges – ad a hivatkozás a hivatkozó SAP telepítési útmutatójában (lásd a [SAP telepítési útmutatók][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-137">Whenever possible, we give you a link to the referring SAP installation guide (see the [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="2b2f4-138">Az Előfeltételek és a telepítési folyamatra vonatkozó információ célszerű gondosan olvassa el az SAP NetWeaver telepítési útmutatók.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-138">For prerequisites and information about the installation process, it's a good idea to read the SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="2b2f4-139">Ez a cikk ismerteti a SAP NetWeaver-alapú rendszereken használható az Azure virtuális gépek csak meghatározott feladatok.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="2b2f4-140">Ezek a megjegyzések SAP SAP, az Azure-ban témakör kapcsolódnak:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-140">These SAP Notes are related to the topic of SAP in Azure:</span></span>

| <span data-ttu-id="2b2f4-141">Megjegyzés száma</span><span class="sxs-lookup"><span data-stu-id="2b2f4-141">Note number</span></span> | <span data-ttu-id="2b2f4-142">Cím</span><span class="sxs-lookup"><span data-stu-id="2b2f4-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="2b2f4-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-143">[1928533]</span></span> |<span data-ttu-id="2b2f4-144">Az Azure-on SAP-alkalmazásokból: támogatott termékek és méretezése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="2b2f4-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-145">[2015553]</span></span> |<span data-ttu-id="2b2f4-146">A Microsoft Azure SAP: Előfeltételek támogatja</span><span class="sxs-lookup"><span data-stu-id="2b2f4-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="2b2f4-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-147">[1999351]</span></span> |<span data-ttu-id="2b2f4-148">SAP Azure figyelésének továbbfejlesztett</span><span class="sxs-lookup"><span data-stu-id="2b2f4-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="2b2f4-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-149">[2178632]</span></span> |<span data-ttu-id="2b2f4-150">Kulcs figyelési metrikákat az SAP, a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="2b2f4-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="2b2f4-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-151">[1999351]</span></span> |<span data-ttu-id="2b2f4-152">A Windows virtualizálási: fokozott figyelése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="2b2f4-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-153">[2243692]</span></span> |<span data-ttu-id="2b2f4-154">A prémium szintű Azure SSD-tárolón SAP DBMS példány használata</span><span class="sxs-lookup"><span data-stu-id="2b2f4-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="2b2f4-155">További információ a [korlátozások az Azure-előfizetések][azure-subscription-service-limits-subscription], többek között az általános alapértelmezett korlátozások és a maximális korlátozások.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-155">Learn more about the [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="2b2f4-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Magas rendelkezésre állású SAP az Azure Resource Manager és az Azure klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="2b2f4-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. the Azure classic deployment model</span></span>
<span data-ttu-id="2b2f4-157">Az Azure Resource Manager és az Azure klasszikus üzembe helyezési modellek szerepelnek eltérő a következő területeken:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-157">The Azure Resource Manager and Azure classic deployment models are different in the following areas:</span></span>

- <span data-ttu-id="2b2f4-158">Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="2b2f4-158">Resource groups</span></span>
- <span data-ttu-id="2b2f4-159">Az Azure erőforráscsoport Azure belső terheléselosztási terheléselosztó függőség</span><span class="sxs-lookup"><span data-stu-id="2b2f4-159">Azure internal load balancer dependency on the Azure resource group</span></span>
- <span data-ttu-id="2b2f4-160">SAP multi-SID forgatókönyvek támogatása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="2b2f4-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="2b2f4-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="2b2f4-162">Az Azure Resource Manager erőforráscsoportokat használhatja az alkalmazás-erőforrásokat az Azure-előfizetése kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-162">In Azure Resource Manager, you can use resource groups to manage all the application resources in your Azure subscription.</span></span> <span data-ttu-id="2b2f4-163">Integrált megközelítést, egy erőforráscsoportban található összes erőforrást rendelkezik az azonos életciklusát.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-163">An integrated approach, in a resource group, all resources have the same life cycle.</span></span> <span data-ttu-id="2b2f4-164">Például minden erőforrás létrehozása egy időben, és egyszerre kell, akkor azok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-164">For example, all resources are created at the same time and they are deleted at the same time.</span></span> <span data-ttu-id="2b2f4-165">További információk az [erőforráscsoportokról](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="2b2f4-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Az Azure erőforráscsoport Azure belső terheléselosztási terheléselosztó függőség</span><span class="sxs-lookup"><span data-stu-id="2b2f4-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on the Azure resource group</span></span>

<span data-ttu-id="2b2f4-167">Az Azure klasszikus üzembe helyezési modellel az Azure belső terheléselosztó (Azure terheléselosztó szolgáltatás) és a felhőalapú szolgáltatás csoportja közötti függőség van.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-167">In the Azure classic deployment model, there is a dependency between the Azure internal load balancer (Azure Load Balancer service) and the cloud service group.</span></span> <span data-ttu-id="2b2f4-168">Minden belső terheléselosztónak egy felhőalapú szolgáltatás csoportot kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-168">Every internal load balancer needs one cloud service group.</span></span>

<span data-ttu-id="2b2f4-169">Az Azure erőforrás-kezelőben, nem kell használni az Azure Load Balancer egy Azure erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-169">In Azure Resource Manager, you don't need an Azure resource group to use Azure Load Balancer.</span></span> <span data-ttu-id="2b2f4-170">A környezet nem egyszerűbb és rugalmasabb.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-170">The environment is simpler and more flexible.</span></span>

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="2b2f4-171">SAP multi-SID forgatókönyvek támogatása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-171">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="2b2f4-172">Az Azure erőforrás-kezelőben, akkor több példányt is telepíthet SAP rendszer azonosítója (SID) ASC/SCS egy fürt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-172">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="2b2f4-173">Minden Azure belső terheléselosztóhoz több IP-cím támogatásának köszönhetően multi-SID-példányok is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-173">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="2b2f4-174">Az Azure klasszikus üzembe helyezési modellel használatához kövesse az ismertetett eljárások [SAP NetWeaver az Azure-ban: az Azure-ban SIOS DataKeeper segítségével a Windows Server feladatátvételi fürtszolgáltatási SAP ASC/SCS fürtszolgáltatási példányok](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-174">To use the Azure classic deployment model, follow the procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b2f4-175">Javasoljuk, hogy az SAP-telepítések az Azure Resource Manager központi telepítési modellt használja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-175">We strongly recommend that you use the Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="2b2f4-176">Nem érhetők el a klasszikus üzembe helyezési modellel számos előnyt kínál.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-176">It offers many benefits that are not available in the classic deployment model.</span></span> <span data-ttu-id="2b2f4-177">További tudnivalók az Azure [üzembe helyezési modellel][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-177">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="2b2f4-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server feladatátvételi fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2b2f4-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="2b2f4-179">Windows Server feladatátvételi fürtszolgáltatási az alapja az egy magas rendelkezésre állású SAP ASC/SCS telepítése és a Windows DBMS.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-179">Windows Server Failover Clustering is the foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="2b2f4-180">A feladatátvevő fürt egy 1 + n különálló kiszolgálók csoportja (csomópontok), amelyek együttműködése az alkalmazások és szolgáltatások rendelkezésre állásának javítása.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-180">A failover cluster is a group of 1+n independent servers (nodes) that work together to increase the availability of applications and services.</span></span> <span data-ttu-id="2b2f4-181">Ha egy csomópont meghibásodik, Windows Server feladatátvételi fürtszolgáltatási számítja ki a biztosításához az alkalmazások és szolgáltatások kifogástalan fürt megőrzésével előforduló hibák száma.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-181">If a node failure occurs, Windows Server Failover Clustering calculates the number of failures that can occur while maintaining a healthy cluster to provide applications and services.</span></span> <span data-ttu-id="2b2f4-182">Választhat a különböző kvórummódok feladatátvételi fürtszolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-182">You can choose from different quorum modes to achieve failover clustering.</span></span>

### <span data-ttu-id="2b2f4-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Kvórummódok</span><span class="sxs-lookup"><span data-stu-id="2b2f4-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="2b2f4-184">Windows Server feladatátvételi fürtszolgáltatási használatakor négy kvórummódok közül választhat:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-184">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="2b2f4-185">**Csomóponttöbbség**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-185">**Node Majority**.</span></span> <span data-ttu-id="2b2f4-186">A fürt mindegyik csomópontján szavazati is.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-186">Each node of the cluster can vote.</span></span> <span data-ttu-id="2b2f4-187">A fürt működik csak a szavazatot, többségét Ez azt jelenti, hogy a több mint fele a szavazatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-187">The cluster functions only with a majority of votes, that is, with more than half the votes.</span></span> <span data-ttu-id="2b2f4-188">Azt javasoljuk, hogy a csomópontok páratlan számú fürtöknél ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-188">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="2b2f4-189">Például egy hét csomópontos fürtben három csomópont sikertelen lehet, és a fürt stills többsége éri el, és továbbra is fut.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-189">For example, three nodes in a seven-node cluster can fail, and the cluster stills achieves a majority and continues to run.</span></span>  
* <span data-ttu-id="2b2f4-190">**Csomópont- és lemeztöbbség**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-190">**Node and Disk Majority**.</span></span> <span data-ttu-id="2b2f4-191">Minden csomópont- és a fürttároló egy kijelölt lemezt (a tanúsító lemez) is szavazásra, mikor érhetők el, és a kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-191">Each node and a designated disk (a disk witness) in the cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="2b2f4-192">A fürt működik csak a többsége a szavazatot, ez azt jelenti, hogy a több mint fele a szavazatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-192">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="2b2f4-193">Ez a mód környezetben fürt páros számú csomópont teljesen logikus.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-193">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="2b2f4-194">Ha a csomópontok fele és a lemez online állapotban, a fürt megfelelő állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-194">If half the nodes and the disk are online, the cluster remains in a healthy state.</span></span>
* <span data-ttu-id="2b2f4-195">**Csomópont- és fájlmegosztás többsége**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-195">**Node and File Share Majority**.</span></span> <span data-ttu-id="2b2f4-196">Minden csomópont és a kijelölt fájlmegosztást (a tanúsító fájlmegosztás), amelyet a rendszergazda létrehoz is szavazásra, függetlenül a csomópontok és a fájlmegosztás elérhetők-e, és a kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-196">Each node plus a designated file share (a file share witness) that the administrator creates can vote, regardless of whether the nodes and file share are available and in communication.</span></span> <span data-ttu-id="2b2f4-197">A fürt működik csak a többsége a szavazatot, ez azt jelenti, hogy a több mint fele a szavazatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-197">The cluster functions only with a majority of the votes, that is, with more than half the votes.</span></span> <span data-ttu-id="2b2f4-198">Ez a mód környezetben fürt páros számú csomópont teljesen logikus.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-198">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="2b2f4-199">A csomópont- és lemeztöbbség mód hasonló, de helyett a tanúsító lemez a tanúsító fájlmegosztás használja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-199">It's similar to the Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="2b2f4-200">Ebben a módban könnyű, de ha a fájlmegosztás maga nem magas rendelkezésre állású, azt válnak a hibaérzékeny pontok kialakulását.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-200">This mode is easy to implement, but if the file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="2b2f4-201">**Nincs többség: Csak lemez**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-201">**No Majority: Disk Only**.</span></span> <span data-ttu-id="2b2f4-202">A fürt rendelkezik egy kvórum, ha egy csomópont elérhető, és a fürttároló egy meghatározott lemezével kommunikál.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-202">The cluster has a quorum if one node is available and in communication with a specific disk in the cluster storage.</span></span> <span data-ttu-id="2b2f4-203">Csak a csomópontok is, hogy a lemez kommunikál csatlakozhat a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-203">Only the nodes that also are in communication with that disk can join the cluster.</span></span> <span data-ttu-id="2b2f4-204">Azt javasoljuk, hogy nem használja ezt a módot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-204">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="2b2f4-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server feladatátvételi fürtszolgáltatás a helyszíni</span><span class="sxs-lookup"><span data-stu-id="2b2f4-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="2b2f4-206">1. ábra mutatja a két csomópontból álló fürtben.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-206">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="2b2f4-207">Ha a hálózati kapcsolatot a csomópontok sikertelen, és mind fel csomópontok marad és futtatását, egy kvórumlemez vagy a fájl határozza meg, melyik csomópontján továbbra is a fürt alkalmazások és szolgáltatások biztosításához.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-207">If the network connection between the nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue to provide the cluster's applications and services.</span></span> <span data-ttu-id="2b2f4-208">A csomópont a kvórum lemez vagy fájlmegosztás eléréséhez használt a csomópont, amely biztosítja, hogy a szolgáltatások továbbra is.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-208">The node that has access to the quorum disk or file share is the node that ensures that services continue.</span></span>

<span data-ttu-id="2b2f4-209">Ez a példa egy két csomópontot tartalmazó fürtben, mert a csomópont- és fájlmegosztástöbbség kvórummód használjuk.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-209">Because this example uses a two-node cluster, we use the Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="2b2f4-210">A csomópont- és lemeztöbbség is beállítás érvényes.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-210">The Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="2b2f4-211">Éles környezetben azt javasoljuk, hogy egy kvórumlemez használjon.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-211">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="2b2f4-212">Hálózati és tárolási rendszer technológia segítségével magas rendelkezésre állásúvá tenni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-212">You can use network and storage system technology to make it highly available.</span></span>

![1. ábra: Példa egy Windows Server feladatátvételi fürtszolgáltatás konfigurációs az SAP ASC/SCS az Azure-ban][sap-ha-guide-figure-1000]

<span data-ttu-id="2b2f4-214">_**1. ábra:** egy Windows Server feladatátvételi fürtszolgáltatás konfigurációs az SAP ASC/SCS az Azure-ban – példa_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-214">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="2b2f4-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Megosztott tároló</span><span class="sxs-lookup"><span data-stu-id="2b2f4-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="2b2f4-216">1. ábra is mutatja egy két csomópontos megosztott tárolófürt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-216">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="2b2f4-217">Egy helyi megosztott tárolási fürt a fürt összes csomópontjának észleli a megosztott tároló.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-217">In an on-premises shared storage cluster, all nodes in the cluster detect shared storage.</span></span> <span data-ttu-id="2b2f4-218">Zárolási mechanizmus sérülése védi az adatokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-218">A locking mechanism protects the data from corruption.</span></span> <span data-ttu-id="2b2f4-219">Minden csomópont észleli, ha egy másik csomópont meghibásodik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-219">All nodes can detect if another node fails.</span></span> <span data-ttu-id="2b2f4-220">Ha egy csomópont meghibásodik, a megmaradó csomópontra megkapja a tulajdonjogot, a tárolási erőforrások és szolgáltatások rendelkezésre állását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-220">If one node fails, the remaining node takes ownership of the storage resources and ensures the availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-221">Például a magas rendelkezésre állás az egyes adatbázis-kezelő alkalmazások, egy megosztott lemez nem kell az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-221">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="2b2f4-222">SQL Server Always On DBMS adatainak és naplókönyvtárainak fájlokat replikálja a helyi lemez egy fürtcsomópont a helyi lemez egy másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-222">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="2b2f4-223">A Windows-fürt konfigurációs ebben az esetben egy megosztott lemez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-223">In that case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="2b2f4-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Hálózati és a névfeloldás</span><span class="sxs-lookup"><span data-stu-id="2b2f4-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="2b2f4-225">Ügyfélszámítógépek számára a fürt egy virtuális IP-címet és egy virtuális nevet a DNS-kiszolgáló által biztosított keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-225">Client computers reach the cluster over a virtual IP address and a virtual host name that the DNS server provides.</span></span> <span data-ttu-id="2b2f4-226">A helyszíni csomópontok és a DNS-kiszolgáló képes kezelni a több IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-226">The on-premises nodes and the DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="2b2f4-227">A hagyományos telepítés, a két vagy több hálózati kapcsolatot használja:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-227">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="2b2f4-228">A tárolási dedikált kapcsolat</span><span class="sxs-lookup"><span data-stu-id="2b2f4-228">A dedicated connection to the storage</span></span>
* <span data-ttu-id="2b2f4-229">A szívverés egy fürt belső hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="2b2f4-229">A cluster-internal network connection for the heartbeat</span></span>
* <span data-ttu-id="2b2f4-230">Az ügyfelek kapcsolódik a fürthöz használt nyilvános hálózaton</span><span class="sxs-lookup"><span data-stu-id="2b2f4-230">A public network that clients use to connect to the cluster</span></span>

## <span data-ttu-id="2b2f4-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server feladatátvételi fürtszolgáltatás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="2b2f4-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="2b2f4-232">Operációs rendszer nélküli vagy saját felhőben történő alkalmazáshoz képest, Azure virtuális gépek konfigurálása a Windows Server feladatátvételi fürtszolgáltatás további lépéseket igényel.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-232">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="2b2f4-233">A megosztott fürtlemez összeállításakor több IP-címek és az SAP ASC/SCS példány virtuális állomásnevek be kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-233">When you build a shared cluster disk, you need to set several IP addresses and virtual host names for the SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="2b2f4-234">Ez a cikk arról lesz szó főbb fogalmakat és az Azure-ban az SAP központi szolgáltatások magas rendelkezésre állású fürt létrehozásához szükséges további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-234">In this article, we discuss key concepts and the additional steps required to build an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="2b2f4-235">Megmutatjuk, hogyan állíthat be a külső eszköz SIOS DataKeeper és konfigurálása az Azure belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-235">We show you how to set up the third-party tool SIOS DataKeeper, and how to configure the Azure internal load balancer.</span></span> <span data-ttu-id="2b2f4-236">Ezek az eszközök segítségével hozzon létre egy Windows feladatátvevő fürt egy tanúsító fájlmegosztást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-236">You can use these tools to create a Windows failover cluster with a file share witness in Azure.</span></span>

![2. ábra: A Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban egy megosztott lemez nélkül][sap-ha-guide-figure-1001]

<span data-ttu-id="2b2f4-238">_**2. ábra:** Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban egy megosztott lemez nélkül_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-238">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="2b2f4-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>SIOS DataKeeper megosztott lemezt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="2b2f4-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="2b2f4-240">A magas rendelkezésre állású SAP ASC/SCS példányához megosztott tárolót kell fürtbe.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-240">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="2b2f4-241">2016 szeptemberétől Azure segítségével hozzon létre egy megosztott tárolófürt megosztott tárhelyet nem kínál.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-241">As of September 2016, Azure doesn't offer shared storage that you can use to create a shared storage cluster.</span></span> <span data-ttu-id="2b2f4-242">Harmadik féltől származó szoftverek SIOS DataKeeper Cluster Edition hozhat létre tükrözött tárolási funkciókat biztosító, amely a fürt megosztott tárolási szimulálja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-242">You can use third-party software SIOS DataKeeper Cluster Edition to create a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="2b2f4-243">A SIOS megoldást biztosít a valós idejű szinkron replikálása.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-243">The SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="2b2f4-244">Ez az, hogy hogyan hozhat létre a fürt egy megosztott lemez erőforrás:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-244">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="2b2f4-245">Egy másik Azure virtuális merevlemezt (VHD) csatolni az egyes virtuális gépek (VM) Windows fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-245">Attach an additional Azure virtual hard disk (VHD) to each of the virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="2b2f4-246">Futtassa a SIOS DataKeeper Cluster Edition mindkét virtuális gép csomóponton.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-246">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="2b2f4-247">Konfigurálása SIOS DataKeeper Cluster Edition, hogy azt a tartalmat a további csatolt virtuális merevlemez kötetének a forrás virtuális gép a cél virtuális gép további csatolt virtuális merevlemez méretével tükrözi.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-247">Configure SIOS DataKeeper Cluster Edition so that it mirrors the content of the additional VHD attached volume from the source virtual machine to the additional VHD attached volume of the target virtual machine.</span></span> <span data-ttu-id="2b2f4-248">SIOS DataKeeper kivonatolja a forrás- és helyi köteteken, és majd megadja azokat a Windows Server feladatátvételi fürtszolgáltatási mint egy megosztott lemez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-248">SIOS DataKeeper abstracts the source and target local volumes, and then presents them to Windows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="2b2f4-249">További információk [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-249">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![3. ábra: A Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban SIOS DataKeeper][sap-ha-guide-figure-1002]

<span data-ttu-id="2b2f4-251">_**3. ábra:** Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-251">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-252">Nincs szükség a megosztott lemez néhány DBMS termékekkel, például az SQL Server magas rendelkezésre állásra.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-252">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="2b2f4-253">SQL Server Always On DBMS adatainak és naplókönyvtárainak fájlokat replikálja a helyi lemez egy fürtcsomópont a helyi lemez egy másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-253">SQL Server Always On replicates DBMS data and log files from the local disk of one cluster node to the local disk of another cluster node.</span></span> <span data-ttu-id="2b2f4-254">A Windows-fürt konfigurációs ebben az esetben egy megosztott lemez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-254">In this case, the Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="2b2f4-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>A névfeloldás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="2b2f4-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="2b2f4-256">Az Azure felhőalapú platform nem teszi lehetővé a virtuális IP-címek, például csak fix IP-címek konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-256">The Azure cloud platform doesn't offer the option to configure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="2b2f4-257">Hozzon létre egy virtuális IP-címet a felhőben fürterőforrás elérni egy másik megoldásra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-257">You need an alternative solution to set up a virtual IP address to reach the cluster resource in the cloud.</span></span>
<span data-ttu-id="2b2f4-258">Azure az Azure terheléselosztó szolgáltatás belső terheléselosztót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-258">Azure has an internal load balancer in the Azure Load Balancer service.</span></span> <span data-ttu-id="2b2f4-259">A belső terheléselosztó rendelkező ügyfelek elérni a fürt keresztül a virtuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-259">With the internal load balancer, clients reach the cluster over the cluster virtual IP address.</span></span>
<span data-ttu-id="2b2f4-260">Kell telepítenie a belső terheléselosztó, amely tartalmazza a fürt csomópontjai az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-260">You need to deploy the internal load balancer in the resource group that contains the cluster nodes.</span></span> <span data-ttu-id="2b2f4-261">Ezt követően konfigurálja a belső terheléselosztó portok továbbítási szabályok a mintavételi minden szükséges portot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-261">Then, configure all necessary port forwarding rules with the probe ports of the internal load balancer.</span></span>
<span data-ttu-id="2b2f4-262">Az ügyfelek kapcsolódhatnak az virtuális állomás név.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-262">The clients can connect via the virtual host name.</span></span> <span data-ttu-id="2b2f4-263">A DNS-kiszolgáló oldja fel a fürt IP-címét, és a belső terheléselosztó leírók port továbbítja a fürt aktív csomópontja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-263">The DNS server resolves the cluster IP address, and the internal load balancer handles port forwarding to the active node of the cluster.</span></span>

## <span data-ttu-id="2b2f4-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver magas rendelkezésre állás érdekében az Azure infrastruktúra,--szolgáltatás (IaaS)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="2b2f4-265">Eléréséhez SAP alkalmazás magas rendelkezésre állású, többek között az SAP szoftverösszetevőket, a következő összetevők védelmére kell:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-265">To achieve SAP application high availability, such as for SAP software components, you need to protect the following components:</span></span>

* <span data-ttu-id="2b2f4-266">SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-266">SAP Application Server instance</span></span>
* <span data-ttu-id="2b2f4-267">SAP ASC/SCS példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-267">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="2b2f4-268">Adatbázis-kezelő kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2b2f4-268">DBMS server</span></span>

<span data-ttu-id="2b2f4-269">Tudnivalók az SAP-összetevők a magas rendelkezésre állású forgatókönyvek védelméről további információkért lásd: [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver](planning-guide.md).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-269">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver](planning-guide.md).</span></span>

### <span data-ttu-id="2b2f4-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Magas rendelkezésre állású SAP-alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2b2f4-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="2b2f4-271">Általában nem szükséges egy adott magas rendelkezésre állású megoldás az SAP-kiszolgáló és a párbeszédpanel-példányok.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-271">You usually don't need a specific high-availability solution for the SAP Application Server and dialog instances.</span></span> <span data-ttu-id="2b2f4-272">Magas rendelkezésre állású redundancia által érhetők el, és másik példányát az Azure virtuális gépek több párbeszédpanel példánya konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-272">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="2b2f4-273">Két példányban Azure virtuális gépek telepítése legalább két SAP alkalmazáspéldányok kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-273">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![4. ábra: Magas rendelkezésre állású SAP-alkalmazáskiszolgáló][sap-ha-guide-figure-2000]

<span data-ttu-id="2b2f4-275">_**4. ábra:** magas rendelkezésre állású SAP-alkalmazáskiszolgáló_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-275">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="2b2f4-276">Az azonos Azure rendelkezésre állási állomás SAP Application Server-példány beállítása minden virtuális gépnek kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-276">You must place all virtual machines that host SAP Application Server instances in the same Azure availability set.</span></span> <span data-ttu-id="2b2f4-277">Az Azure rendelkezésre állási csoportok biztosítja, hogy:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-277">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="2b2f4-278">Összes virtuális gépet az azonos frissítési tartomány részét képezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-278">All virtual machines are part of the same upgrade domain.</span></span> <span data-ttu-id="2b2f4-279">Frissítési tartományokhoz, például gondoskodik arról, hogy a virtuális gépek tervezett karbantartás leállások során egy időben nem frissíti.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-279">An upgrade domain, for example, makes sure that the virtual machines aren't updated at the same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="2b2f4-280">Összes virtuális gépet az azonos tartalék tartományban részét képezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-280">All virtual machines are part of the same fault domain.</span></span> <span data-ttu-id="2b2f4-281">A tartalék tartomány például gondoskodik arról, hogy telepít virtuális gépeket, hogy nincs hibaérzékeny pontok kialakulását hatással van az összes virtuális gép rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-281">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects the availability of all virtual machines.</span></span>

<span data-ttu-id="2b2f4-282">További tudnivalók a [virtuális gépek rendelkezésre állásának kezelése][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-282">Learn more about how to [manage the availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="2b2f4-283">Mivel az Azure storage-fiók egy potenciális hibaérzékeny pontok kialakulását, fontos, hogy legalább két az Azure storage-fiókot, legalább két virtuális gép szétosztva.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-283">Because the Azure storage account is a potential single point of failure, it's important to have at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="2b2f4-284">Az ideális beállítás a lemezeket, az egyes virtuális gépek egy SAP párbeszédpanel példányát futtató volna helyezhető üzembe egy másik tárolási fiókot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-284">In an ideal setup, the disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="2b2f4-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Magas rendelkezésre állású SAP ASC/SCS példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="2b2f4-286">5. ábra egy példát a magas rendelkezésre állású SAP ASC/SCS példánya.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-286">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![5. ábra: Magas rendelkezésre állású SAP ASC/SCS példány][sap-ha-guide-figure-2001]

<span data-ttu-id="2b2f4-288">_**5. ábra:** magas rendelkezésre állású SAP ASC/SCS példány_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-288">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="2b2f4-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC/SCS példány magas rendelkezésre állását a Windows Server feladatátvételi fürtszolgáltatási az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="2b2f4-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="2b2f4-290">Operációs rendszer nélküli vagy saját felhőben történő alkalmazáshoz képest, Azure virtuális gépek konfigurálása a Windows Server feladatátvételi fürtszolgáltatás további lépéseket igényel.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-290">Compared to bare metal or private cloud deployments, Azure Virtual Machines requires additional steps to configure Windows Server Failover Clustering.</span></span> <span data-ttu-id="2b2f4-291">A Windows feladatátvevő fürtöt hozza létre, szüksége megosztott fürtlemezre mutat, több IP-címek, több virtuális állomásnevek és az Azure belső terheléselosztót a fürtszolgáltatás egy SAP ASC/SCS példányát.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-291">To build a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="2b2f4-292">Ez a cikk későbbi részében részletesebben azt tárgyalják.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-292">We discuss this in more detail later in the article.</span></span>

![6. ábra: A Windows Server feladatátvételi fürtszolgáltatási SIOS DataKeeper használatával az Azure-ban SAP ASC/SCS konfigurációhoz][sap-ha-guide-figure-1002]

<span data-ttu-id="2b2f4-294">_**6. ábra:** Windows Server feladatátvételi fürtszolgáltatási az Azure-ban SIOS DataKeeper SAP ASC/SCS konfigurációhoz_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-294">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="2b2f4-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Magas rendelkezésre állású DBMS példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="2b2f4-296">Az adatbázis-kezelő a rendszer egyetlen pont, forduljon egy SAP rendszerben.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-296">The DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="2b2f4-297">Azt a magas rendelkezésre állású megoldás használatával védeni kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-297">You need to protect it by using a high-availability solution.</span></span> <span data-ttu-id="2b2f4-298">7. ábra mutatja egy SQL Server Always On magas rendelkezésre állású megoldás az Azure-ban, a Windows Server feladatátvételi fürtszolgáltatási és az Azure belső terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-298">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and the Azure internal load balancer.</span></span> <span data-ttu-id="2b2f4-299">SQL Server Always On DBMS adatainak és naplókönyvtárainak fájlokat replikálja a saját adatbázis-kezelő replikáció használatával.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-299">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="2b2f4-300">Ebben az esetben, nem kell a fürt megosztott lemezt, amely egyszerűbbé teszi az összes beállítást.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-300">In this case, you don't need cluster shared disks, which simplifies the entire setup.</span></span>

![7. ábra: Példa egy magas rendelkezésre állású SAP DBMS, az SQL Server Always On][sap-ha-guide-figure-2003]

<span data-ttu-id="2b2f4-302">_**7. ábra:** egy magas rendelkezésre állású SAP DBMS, az SQL Server Always On – példa_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-302">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="2b2f4-303">SQL Server az Azure-ban az Azure Resource Manager telepítési modell segítségével fürtszolgáltatással kapcsolatos további információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-303">For more information about clustering SQL Server in Azure by using the Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="2b2f4-304">[Always On rendelkezésre állási csoport konfigurálásához Azure virtuális gépek manuálisan erőforrás-kezelő használatával][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-304">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="2b2f4-305">[Egy Azure belső terheléselosztót egy Always On rendelkezésre állási csoport konfigurálása az Azure-ban][virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="2b2f4-305">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="2b2f4-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Végpontok közötti magas rendelkezésre állású környezetekben</span><span class="sxs-lookup"><span data-stu-id="2b2f4-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="2b2f4-307">Üzembe helyezési forgatókönyv segítségével 1. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-307">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="2b2f4-308">8. ábrán egy SAP NetWeaver magas rendelkezésre állású architektúra példáját mutatja be az Azure-ban **egy** SAP rendszer.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-308">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="2b2f4-309">Ebben a forgatókönyvben be van állítva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-309">This scenario is set up as follows:</span></span>

- <span data-ttu-id="2b2f4-310">Az SAP ASC/SCS-példány egy kijelölt fürt használható.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-310">One dedicated cluster is used for the SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="2b2f4-311">Az adatbázis-kezelő példány egy kijelölt fürt használható.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-311">One dedicated cluster is used for the DBMS instance.</span></span>
- <span data-ttu-id="2b2f4-312">SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-312">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![8. ábra: SAP magas rendelkezésre állású architekturális sablon 1, a kijelölt fürt ASC/SCS és adatbázis-kezelő][sap-ha-guide-figure-2004]

<span data-ttu-id="2b2f4-314">_**8. ábra:** SAP architekturális sablon 1 magas rendelkezésre állású, dedikált fürtök ASC/SCS és adatbázis-kezelő_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-314">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="2b2f4-315">Üzembe helyezési forgatókönyv architekturális sablon 2 használatával</span><span class="sxs-lookup"><span data-stu-id="2b2f4-315">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="2b2f4-316">9. ábra egy SAP NetWeaver magas rendelkezésre állású architektúra példáját mutatja be az Azure-ban **egy** SAP rendszer.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-316">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="2b2f4-317">Ebben a forgatókönyvben be van állítva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-317">This scenario is set up as follows:</span></span>

- <span data-ttu-id="2b2f4-318">Egy kijelölt fürt használt **mindkét** az SAP ASC/SCS példány és az adatbázis-kezelő.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-318">One dedicated cluster is used for **both** the SAP ASCS/SCS instance and the DBMS.</span></span>
- <span data-ttu-id="2b2f4-319">SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-319">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![9. ábra: SAP magas rendelkezésre állású architekturális sablon 2, a dedikált fürtökben ASC/SCS és egy dedikált adatbázis-kezelő fürt][sap-ha-guide-figure-2005]

<span data-ttu-id="2b2f4-321">_**9. ábra:** SAP magas rendelkezésre állású architekturális sablon 2, a dedikált fürtökben ASC/SCS és egy dedikált adatbázis-kezelő fürt_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-321">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="2b2f4-322">Üzembe helyezési forgatókönyv a 3. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-322">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="2b2f4-323">10. ábrán látható egy példa egy SAP NetWeaver magas rendelkezésre állású architektúra az Azure-ban **két** rendszerek, a SAP &lt;SID1&gt; és &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-323">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="2b2f4-324">Ebben a forgatókönyvben be van állítva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-324">This scenario is set up as follows:</span></span>

- <span data-ttu-id="2b2f4-325">Egy kijelölt fürt használt **mindkét** az SAP ASC/SCS SID1 példány *és* az SAP ASC/SCS SID2 példány (egy fürt).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-325">One dedicated cluster is used for **both** the SAP ASCS/SCS SID1 instance *and* the SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="2b2f4-326">Egy kijelölt fürt DBMS SID1 szolgál, és egy másik kijelölt fürt használt adatbázis-kezelő SID2 (fürtök).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-326">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="2b2f4-327">Az SAP rendszer SID1 SAP alkalmazáskiszolgáló példányainak rendelkezik saját dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-327">SAP Application Server instances for the SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="2b2f4-328">Az SAP rendszer SID2 SAP alkalmazáskiszolgáló példányainak rendelkezik saját dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-328">SAP Application Server instances for the SAP system SID2 have their own dedicated VMs.</span></span>

![10. ábra: SAP magas rendelkezésre állású architekturális sablon 3, egy dedikált fürttel különböző ASC/SCS-példányok][sap-ha-guide-figure-6003]

<span data-ttu-id="2b2f4-330">_**10. ábra:** SAP magas rendelkezésre állású architekturális sablon 3, egy dedikált fürttel különböző ASC/SCS-példányok_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-330">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="2b2f4-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Az infrastruktúra előkészítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare the infrastructure</span></span>

### <a name="prepare-the-infrastructure-for-architectural-template-1"></a><span data-ttu-id="2b2f4-332">Az infrastruktúra előkészítése architekturális sablon 1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-332">Prepare the infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="2b2f4-333">Az SAP Azure Resource Manager-sablonok segítségével egyszerűsítheti a szükséges erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-333">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="2b2f4-334">A háromrétegű sablonok az Azure Resource Manager is támogatja a magas rendelkezésre állású forgatókönyvek, például architekturális sablon az 1., amely két fürttel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-334">The three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="2b2f4-335">Minden fürthöz egy SAP hibaérzékeny pontot SAP ASC/SCS és adatbázis-kezelő.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-335">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="2b2f4-336">Itt található, ahol kaphat a példaforgatókönyv azt ismertetik, ez a cikk az Azure Resource Manager-sablonok:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-336">Here's where you can get Azure Resource Manager templates for the example scenario we describe in this article:</span></span>

* [<span data-ttu-id="2b2f4-337">Kép: Azure piactér</span><span class="sxs-lookup"><span data-stu-id="2b2f4-337">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="2b2f4-338">Kép: egyéni</span><span class="sxs-lookup"><span data-stu-id="2b2f4-338">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

<span data-ttu-id="2b2f4-339">Az infrastruktúra előkészítése architekturális sablon 1:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-339">To prepare the infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="2b2f4-340">Az Azure portálon a a **paraméterek** panelen, a a **SYSTEMAVAILABILITY** mezőben válassza **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-340">In the Azure portal, on the **Parameters** blade, in the **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![11. ábra: Beállítása SAP magas rendelkezésre állású Azure Resource Manager-paraméterek][sap-ha-guide-figure-3000]

<span data-ttu-id="2b2f4-342">_**11. ábra:** beállítása SAP magas rendelkezésre állású Azure Resource Manager-paraméterek_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-342">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="2b2f4-343">A sablonok létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-343">The templates create:</span></span>

  * <span data-ttu-id="2b2f4-344">**Virtuális gépek**:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-344">**Virtual machines**:</span></span>
    * <span data-ttu-id="2b2f4-345">SAP alkalmazáskiszolgáló virtuális gépek: <*SAPSystemSID*> - di - <*száma*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-345">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="2b2f4-346">A fürt virtuális gépek ASC/SCS: <*SAPSystemSID*> - ASC - <*száma*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-346">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="2b2f4-347">Adatbázis-kezelő fürt: <*SAPSystemSID*> - db - <*száma*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-347">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="2b2f4-348">**Hálózati kártya tartozó összes virtuális géphez társított IP-címekkel rendelkező**:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-348">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="2b2f4-349"><*SAPSystemSID*> - nic - di - <*száma*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-349"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="2b2f4-350"><*SAPSystemSID*> - nic - ASC - <*száma*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-350"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="2b2f4-351"><*SAPSystemSID*> - nic - db - <*száma*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-351"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="2b2f4-352">**Az Azure storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="2b2f4-352">**Azure storage accounts**</span></span>

  * <span data-ttu-id="2b2f4-353">**Rendelkezésre állási csoportok** esetében:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-353">**Availability groups** for:</span></span>
    * <span data-ttu-id="2b2f4-354">SAP alkalmazáskiszolgáló virtuális gépek: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="2b2f4-354">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="2b2f4-355">SAP ASC/SCS cluster virtuális gépek: <*SAPSystemSID*> - avset - ASC</span><span class="sxs-lookup"><span data-stu-id="2b2f4-355">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="2b2f4-356">Adatbázis-kezelő fürt virtuális gépek: <*SAPSystemSID*> - avset - adatbázis</span><span class="sxs-lookup"><span data-stu-id="2b2f4-356">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="2b2f4-357">**Az Azure belső terheléselosztó**:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-357">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="2b2f4-358">Az összes port a ASC/SCS példányhoz, és az IP-cím <*SAPSystemSID*> - lb - ASC</span><span class="sxs-lookup"><span data-stu-id="2b2f4-358">With all ports for the ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="2b2f4-359">Az összes port az SQL Server adatbázis-kezelő és az IP-cím <*SAPSystemSID*> - lb - adatbázis</span><span class="sxs-lookup"><span data-stu-id="2b2f4-359">With all ports for the SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="2b2f4-360">**Hálózati biztonsági csoport**: <*SAPSystemSID*> - nsg - ASC-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-360">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="2b2f4-361">Egy nyitott külső Remote Desktop Protocol (RDP) port az a <*SAPSystemSID*> virtuálisgép - ASC - 0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-361">With an open external Remote Desktop Protocol (RDP) port to the <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-362">Az összes IP-címek a hálózati kártyák és az Azure belső terheléselosztók **dinamikus** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-362">All IP addresses of the network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="2b2f4-363">Módosítani őket **statikus** IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-363">Change them to **static** IP addresses.</span></span> <span data-ttu-id="2b2f4-364">Azt ismerteti, hogyan ehhez a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-364">We describe how to do this later in the article.</span></span>
>
>

### <span data-ttu-id="2b2f4-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>A vállalati hálózati kapcsolat hibája (létesítmények közötti) éles környezetben használandó virtuális gépek telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) to use in production</span></span>
<span data-ttu-id="2b2f4-366">Éles rendszerek esetén SAP, az Azure virtuális gépek telepítése a [vállalati hálózati kapcsolat (létesítmények közötti)] [ planning-guide-2.2] Azure hely-hely típusú VPN vagy Azure ExpressRoute segítségével.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-366">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-367">Az Azure Virtual Network-példányt is használhat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-367">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="2b2f4-368">A virtuális hálózat és alhálózat már létrehozott és előkészítése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-368">The virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="2b2f4-369">Az Azure portálon a a **paraméterek** panelen, a a **NEWOREXISTINGSUBNET** mezőben válassza **meglévő**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-369">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="2b2f4-370">Az a **SUBNETID** mezőben adja meg az elkészített Azure hálózati SubnetID, ha azt tervezi, hogy az Azure virtuális gépek telepítése a teljes karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-370">In the **SUBNETID** box, add the full string of your prepared Azure network SubnetID where you plan to deploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="2b2f4-371">Ahhoz, hogy az összes Azure-hálózat alhálózatok listája, a PowerShell parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-371">To get a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="2b2f4-372">A **azonosító** mezőben látható a **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-372">The **ID** field shows the **SUBNETID**.</span></span>
4. <span data-ttu-id="2b2f4-373">Az összes listájának **SUBNETID** értékek, futtassa a PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-373">To get a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="2b2f4-374">A **SUBNETID** dolgozunk:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-374">The **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="2b2f4-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>A tesztelési és bemutató csak felhőalapú SAP-példányok telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="2b2f4-376">Egy kizárólag felhőalapú üzembe helyezési modellben a magas rendelkezésre állású SAP rendszereket telepítheti központilag.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-376">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="2b2f4-377">Ez a központi telepítési típus elsősorban akkor hasznos, bemutató és tesztelési használatából adódó.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-377">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="2b2f4-378">Nem alkalmas a termelési használati eseteket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-378">It's not suited for production use cases.</span></span>

- <span data-ttu-id="2b2f4-379">Az Azure portálon a a **paraméterek** panelen, a a **NEWOREXISTINGSUBNET** mezőben válassza **új**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-379">In the Azure portal, on the **Parameters** blade, in the **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="2b2f4-380">Hagyja a **SUBNETID** mező üres.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-380">Leave the **SUBNETID** field empty.</span></span>

  <span data-ttu-id="2b2f4-381">Az SAP Azure Resource Manager sablon automatikusan létrehozza az Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-381">The SAP Azure Resource Manager template automatically creates the Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-382">Szükség legalább egy dedikált virtuális gép üzembe helyezéséhez az Active Directory és a DNS az Azure Virtual Network ugyanezen példányában.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-382">You also need to deploy at least one dedicated virtual machine for Active Directory and DNS in the same Azure Virtual Network instance.</span></span> <span data-ttu-id="2b2f4-383">A sablon nem hoz létre a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-383">The template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-the-infrastructure-for-architectural-template-2"></a><span data-ttu-id="2b2f4-384">Az infrastruktúra előkészítése a 2. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-384">Prepare the infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="2b2f4-385">A SAP Azure Resource Manager sablon segítségével leegyszerűsíti az SAP architekturális sablon 2 álló szükséges infrastruktúra központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-385">You can use this Azure Resource Manager template for SAP to help simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="2b2f4-386">Itt található, ahol kaphat Azure Resource Manager-sablonok a központi telepítési forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-386">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="2b2f4-387">Kép: Azure piactér</span><span class="sxs-lookup"><span data-stu-id="2b2f4-387">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="2b2f4-388">Kép: egyéni</span><span class="sxs-lookup"><span data-stu-id="2b2f4-388">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-the-infrastructure-for-architectural-template-3"></a><span data-ttu-id="2b2f4-389">Az infrastruktúra előkészítése a 3. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-389">Prepare the infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="2b2f4-390">Készítse elő az infrastruktúrát, és konfigurálja az SAP **multi-SID**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-390">You can prepare the infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="2b2f4-391">Például hozzáadhat egy SAP ASC/SCS másodpéldányt be egy *meglévő* fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-391">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="2b2f4-392">További információkért lásd: [konfigurálása egy SAP ASC/SCS másodpéldányt be egy meglévő fürt konfigurációját, és hozzon létre egy SAP multi-SID-konfigurációját az Azure Resource Manager][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-392">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration to create an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="2b2f4-393">Ha szeretne létrehozni egy új multi-SID-fürtöt, használhatja a multi-SID [gyorsindítási sablonok a Githubon](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-393">If you want to create a new multi-SID cluster, you can use the multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="2b2f4-394">Hozzon létre egy új multi-SID-fürtöt, üzembe helyezheti a következő három sablonokat kell:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-394">To create a new multi-SID cluster, you need to deploy the following three templates:</span></span>

* [<span data-ttu-id="2b2f4-395">Asc/SCS sablon</span><span class="sxs-lookup"><span data-stu-id="2b2f4-395">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="2b2f4-396">Adatbázis-sablon</span><span class="sxs-lookup"><span data-stu-id="2b2f4-396">Database template</span></span>](#database-template)
* [<span data-ttu-id="2b2f4-397">Alkalmazássablon-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="2b2f4-397">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="2b2f4-398">A következő szakaszok további információkat a sablonok és a paraméterek meg kell adnia a sablonokban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-398">The following sections have more details about the templates and the parameters you need to provide in the templates.</span></span>

#### <span data-ttu-id="2b2f4-399"><a name="ASCS-SCS-template"></a>Asc/SCS sablon</span><span class="sxs-lookup"><span data-stu-id="2b2f4-399"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="2b2f4-400">A ASC/SCS sablon segítségével több ASC/SCS-példányt futtató Windows Server feladatátvevő fürt létrehozása két virtuális gépek telepíti.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-400">The ASCS/SCS template deploys two virtual machines that you can use to create a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="2b2f4-401">A ASC/SCS multi-SID-sablon beállítása az a [ASC/SCS multi-SID sablon][sap-templates-3-tier-multisid-xscs-marketplace-image], adja meg a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-401">To set up the ASCS/SCS multi-SID template, in the [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image], enter values for the following parameters:</span></span>

  - <span data-ttu-id="2b2f4-402">**Erőforrás-előtag**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-402">**Resource Prefix**.</span></span>  <span data-ttu-id="2b2f4-403">Állítsa be az erőforrás előtagja, amelynek segítségével a telepítés során létrehozott összes erőforrás előtag.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-403">Set the resource prefix, which is used to prefix all resources that are created during the deployment.</span></span> <span data-ttu-id="2b2f4-404">Mivel az erőforrások csak egy SAP-rendszer nem tartoznak, az előtag, az erőforrás nincs egy SAP rendszer biztonsági azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-404">Because the resources do not belong to only one SAP system, the prefix of the resource is not the SID of one SAP system.</span></span>  <span data-ttu-id="2b2f4-405">Az előtag között kell lennie **3-6 karakter**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-405">The prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="2b2f4-406">**A verem típus**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-406">**Stack Type**.</span></span> <span data-ttu-id="2b2f4-407">Válassza ki a verem az SAP rendszer típusa.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-407">Select the stack type of the SAP system.</span></span> <span data-ttu-id="2b2f4-408">A verem típusától függően a Azure Load Balancer (ABAP vagy csak a Java) egy vagy két (ABAP + Java) magánhálózati IP-címek SAP rendszerenként rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-408">Depending on the stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="2b2f4-409">**Operációs rendszer típusa**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-409">**OS Type**.</span></span> <span data-ttu-id="2b2f4-410">Válassza ki a virtuális gépek operációs rendszerének.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-410">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="2b2f4-411">**SAP rendszer száma**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-411">**SAP System Count**.</span></span> <span data-ttu-id="2b2f4-412">Válassza ki a fürt telepítendő SAP rendszerek számát.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-412">Select the number of SAP systems you want to install in this cluster.</span></span>
  -  <span data-ttu-id="2b2f4-413">**Rendelkezésre állására**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-413">**System Availability**.</span></span> <span data-ttu-id="2b2f4-414">Válassza ki **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-414">Select **HA**.</span></span>
  -  <span data-ttu-id="2b2f4-415">**Rendszergazda felhasználónevét és a rendszergazdai jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-415">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="2b2f4-416">Hozzon létre egy új felhasználót, amely segítségével jelentkezzen be a gépet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-416">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="2b2f4-417">**Új vagy meglévő alhálózati**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-417">**New Or Existing Subnet**.</span></span> <span data-ttu-id="2b2f4-418">Állítsa be, hogy egy új virtuális hálózat és alhálózat kell létrehozni, vagy használjon egy létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-418">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="2b2f4-419">Ha már van egy virtuális hálózatot, amely a helyszíni hálózathoz csatlakozik, válassza ki a **meglévő**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-419">If you already have a virtual network that is connected to your on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="2b2f4-420">**Alhálózati azonosító**. Állítsa az azonosító az alhálózat, amelyhez a virtuális gépek kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-420">**Subnet Id**. Set the ID of the subnet to which the virtual machines should be connected.</span></span> <span data-ttu-id="2b2f4-421">Jelölje ki az alhálózatot, a virtuális magánhálózati (VPN) vagy ExpressRoute virtuális hálózat a virtuális gép és a helyszíni hálózathoz csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-421">Select the subnet of your virtual private network (VPN) or ExpressRoute virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="2b2f4-422">Az azonosító általában néz ki:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-422">The ID usually looks like this:</span></span>

   <span data-ttu-id="2b2f4-423">/Subscriptions/ <*előfizetés-azonosító*> /resourceGroups/ <*erőforráscsoport-név*> /providers/Microsoft.Network/virtualNetworks/ <*virtuálishálózat-név*> /subnets/ <*alhálózat neve*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-423">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="2b2f4-424">A sablon egy Azure Load Balancer példány, amely támogatja a több SAP rendszert telepíti.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-424">The template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="2b2f4-425">A ASC példányok példányszámának 00, 10, 20 beállítást...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-425">The ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="2b2f4-426">A SCS példányok példányszámának 01, 11, 21 beállítást...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-426">The SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="2b2f4-427">A ASC sorba helyezni replikációs Server (SSZON) (csak Linux) példányok példányszámának 02, 12, 22 beállítást...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-427">The ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="2b2f4-428">A SCS SSZON (csak Linux) példányok példányszámának 03., 13, 23 beállítást...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-428">The SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="2b2f4-429">A load balancer tartalmaz 1 (Linux 2) VIP(s), a ASC/SCS 1 x-VIP és a SSZON (csak Linux esetén) 1 x-VIP.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-429">The load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="2b2f4-430">Az alábbi lista tartalmazza az összes terheléselosztási szabályok (ahol x egy SAP rendszer, például 1, 2, 3... száma):</span><span class="sxs-lookup"><span data-stu-id="2b2f4-430">The following list contains all load balancing rules (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="2b2f4-431">Minden SAP rendszerhez a Windows-specifikus portokat: 445-ös, 5985</span><span class="sxs-lookup"><span data-stu-id="2b2f4-431">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="2b2f4-432">Asc portok (x0 száma): 32 x 0, 36 x 0, 39 x 0, 81-es x 0, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="2b2f4-432">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="2b2f4-433">SCS portok (x1 száma): 32 x 1, 33 x 1, 39 x 1, 81-es x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="2b2f4-433">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="2b2f4-434">Linux (példányszámának x2) portok ASC SSZON: 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="2b2f4-434">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="2b2f4-435">Linux (példányszámának x3) portok SCS SSZON: 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="2b2f4-435">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="2b2f4-436">A terheléselosztó a következő (Ha x az a szám az SAP-rendszer, például 1, 2, 3...) mintavételi portok használatára van konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-436">The load balancer is configured to use the following probe ports (where x is the number of the SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="2b2f4-437">Asc/SCS belső terheléselosztó mintavételi portot betölteni: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-437">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="2b2f4-438">SSZON belső terheléselosztó mintavételi portot (csak Linux) betöltése: 621 x 2</span><span class="sxs-lookup"><span data-stu-id="2b2f4-438">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="2b2f4-439"><a name="database-template"></a>Adatbázis-sablon</span><span class="sxs-lookup"><span data-stu-id="2b2f4-439"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="2b2f4-440">Az adatbázis-sablon telepít egy vagy két virtuális gépek, amelyek egy SAP rendszer relációs adatbázis-kezelő rendszerének (RDBMS) telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-440">The database template deploys one or two virtual machines that you can use to install the relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="2b2f4-441">Például ha telepít egy ASC/SCS sablon öt SAP rendszerekhez, akkor kell telepíteni a sablon ötször.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-441">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="2b2f4-442">Az adatbázis-multi-SID sablon beállítása a [adatbázis multi-SID sablon][sap-templates-3-tier-multisid-db-marketplace-image], adja meg a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-442">To set up the database multi-SID template, in the [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="2b2f4-443">**SAP rendszerazonosító**. Adja meg a telepíteni kívánt SAP rendszer SAP rendszer Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-443">**Sap System Id**. Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="2b2f4-444">Az azonosító az üzembe helyezett erőforrások előtagjaként lesz használható.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-444">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="2b2f4-445">**Operációs rendszer típusa**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-445">**Os Type**.</span></span> <span data-ttu-id="2b2f4-446">Válassza ki a virtuális gépek operációs rendszerének.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-446">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="2b2f4-447">**DbType**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-447">**Dbtype**.</span></span> <span data-ttu-id="2b2f4-448">Válassza ki az adatbázist, a fürtön telepíteni kívánt típusát.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-448">Select the type of the database you want to install on the cluster.</span></span> <span data-ttu-id="2b2f4-449">Válassza ki **SQL** Ha telepíti a Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-449">Select **SQL** if you want to install Microsoft SQL Server.</span></span> <span data-ttu-id="2b2f4-450">Válassza ki **HANA** Ha azt tervezi, hogy SAP HANA telepítse a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-450">Select **HANA** if you plan to install SAP HANA on the virtual machines.</span></span> <span data-ttu-id="2b2f4-451">Ügyeljen arra, hogy válassza ki a megfelelő operációs rendszer típusa: válassza ki **Windows** SQL, és válasszon egy Linux terjesztési Hana.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-451">Make sure to select the correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="2b2f4-452">A kijelölt adatbázis támogatja az Azure terheléselosztó, amely kapcsolódik a virtuális gépek lesz konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-452">The Azure Load Balancer that is connected to the virtual machines will be configured to support the selected database type:</span></span>
    * <span data-ttu-id="2b2f4-453">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-453">**SQL**.</span></span> <span data-ttu-id="2b2f4-454">A load balancer fog terheléselosztásához 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-454">The load balancer will load-balance port 1433.</span></span> <span data-ttu-id="2b2f4-455">Győződjön meg arról, hogy ezen a porton az SQL Server Always On beállítás.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-455">Make sure to use this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="2b2f4-456">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-456">**HANA**.</span></span> <span data-ttu-id="2b2f4-457">A load balancer fog terheléselosztásához 35015 és 35017 portot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-457">The load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="2b2f4-458">Ügyeljen arra, hogy telepítse SAP HANA példányszámának **50**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-458">Make sure to install SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="2b2f4-459">A load balancer 62550 mintavételi portot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-459">The load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="2b2f4-460">**SAP mérete**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-460">**Sap System Size**.</span></span> <span data-ttu-id="2b2f4-461">Adjon meg az új rendszer nyújtják SAP.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-461">Set the number of SAPS the new system will provide.</span></span> <span data-ttu-id="2b2f4-462">Ha nem tudja, a rendszer hány SAP, kérje meg a SAP technológia Partner vagy a rendszer integráló.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-462">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="2b2f4-463">**Rendelkezésre állására**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-463">**System Availability**.</span></span> <span data-ttu-id="2b2f4-464">Válassza ki **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-464">Select **HA**.</span></span>
  -  <span data-ttu-id="2b2f4-465">**Rendszergazda felhasználónevét és a rendszergazdai jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-465">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="2b2f4-466">Hozzon létre egy új felhasználót, amely segítségével jelentkezzen be a gépet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-466">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="2b2f4-467">**Alhálózati azonosító**. Adja meg az alhálózat ASC/SCS-sablon a telepítés során használt azonosító, vagy az alhálózat létrehozott azonosítója a ASC/SCS sablon központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-467">**Subnet Id**. Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="2b2f4-468"><a name="application-servers-template"></a>Alkalmazássablon-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="2b2f4-468"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="2b2f4-469">A kiszolgálók alkalmazássablon SAP alkalmazáskiszolgáló-példányok egy SAP-rendszerhez használható két vagy több virtuális gépek telepíti.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-469">The application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="2b2f4-470">Például ha telepít egy ASC/SCS sablon öt SAP rendszerekhez, akkor kell telepíteni a sablon ötször.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-470">For example, if you deploy an ASCS/SCS template for five SAP systems, you need to deploy this template five times.</span></span>

<span data-ttu-id="2b2f4-471">Az alkalmazás kiszolgálók multi-SID sablon beállítása a [kiszolgálók multi-SID alkalmazássablon][sap-templates-3-tier-multisid-apps-marketplace-image], adja meg a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-471">To set up the application servers multi-SID template, in the [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image], enter values for the following parameters:</span></span>

  -  <span data-ttu-id="2b2f4-472">**SAP rendszerazonosító**. Adja meg a telepíteni kívánt SAP rendszer SAP rendszer Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-472">**Sap System Id**. Enter the SAP system ID of the SAP system you want to install.</span></span> <span data-ttu-id="2b2f4-473">Az azonosító az üzembe helyezett erőforrások előtagjaként lesz használható.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-473">The ID will be used as a prefix for the resources that are deployed.</span></span>
  -  <span data-ttu-id="2b2f4-474">**Operációs rendszer típusa**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-474">**Os Type**.</span></span> <span data-ttu-id="2b2f4-475">Válassza ki a virtuális gépek operációs rendszerének.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-475">Select the operating system of the virtual machines.</span></span>
  -  <span data-ttu-id="2b2f4-476">**SAP mérete**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-476">**Sap System Size**.</span></span> <span data-ttu-id="2b2f4-477">Az új rendszer nyújtják SAP száma.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-477">The number of SAPS the new system will provide.</span></span> <span data-ttu-id="2b2f4-478">Ha nem tudja, a rendszer hány SAP, kérje meg a SAP technológia Partner vagy a rendszer integráló.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-478">If you are not sure how many SAPS the system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="2b2f4-479">**Rendelkezésre állására**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-479">**System Availability**.</span></span> <span data-ttu-id="2b2f4-480">Válassza ki **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-480">Select **HA**.</span></span>
  -  <span data-ttu-id="2b2f4-481">**Rendszergazda felhasználónevét és a rendszergazdai jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-481">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="2b2f4-482">Hozzon létre egy új felhasználót, amely segítségével jelentkezzen be a gépet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-482">Create a new user that can be used to sign in to the machine.</span></span>
  -  <span data-ttu-id="2b2f4-483">**Alhálózati azonosító**. Adja meg az alhálózat ASC/SCS-sablon a telepítés során használt azonosító, vagy az alhálózat létrehozott azonosítója a ASC/SCS sablon központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-483">**Subnet Id**. Enter the ID of the subnet that you used during the deployment of the ASCS/SCS template, or the ID of the subnet that was created as part of the ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="2b2f4-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure-beli virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="2b2f4-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="2b2f4-485">A fenti példában az Azure virtuális hálózat címtartománya 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-485">In our example, the address space of the Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="2b2f4-486">Egy alhálózat neve van **alhálózati**, a 10.0.0.0/24 címtartományt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-486">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="2b2f4-487">A virtuális hálózaton található virtuális gépek és a belső terheléselosztók vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-487">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b2f4-488">Nem módosítja a vendég operációs rendszerében a hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-488">Don't make any changes to the network settings inside the guest operating system.</span></span> <span data-ttu-id="2b2f4-489">Ez magában foglalja az IP-címek, a DNS-kiszolgálók és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-489">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="2b2f4-490">A hálózati beállítások konfigurálása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-490">Configure all your network settings in Azure.</span></span> <span data-ttu-id="2b2f4-491">A Dynamic Host Configuration Protocol (DHCP) szolgáltatás tölti ki a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-491">The Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="2b2f4-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS-IP-címek</span><span class="sxs-lookup"><span data-stu-id="2b2f4-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="2b2f4-493">Állítsa be a szükséges DNS-IP-címek, tegye a következőket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-493">To set the required DNS IP addresses, do the following steps.</span></span>

1.  <span data-ttu-id="2b2f4-494">Az Azure portálon a a **DNS-kiszolgálók** panelen ellenőrizze, hogy a virtuális hálózat **DNS-kiszolgálók** beállítás **egyéni DNS**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-494">In the Azure portal, on the **DNS servers** blade, make sure that your virtual network **DNS servers** option is set to **Custom DNS**.</span></span>
2.  <span data-ttu-id="2b2f4-495">Válassza ki a beállítások alapján a hálózati rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-495">Select your settings based on the type of network you have.</span></span> <span data-ttu-id="2b2f4-496">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-496">For more information, see the following resources:</span></span>
    * <span data-ttu-id="2b2f4-497">[Vállalati hálózati kapcsolat (létesítmények közötti)][planning-guide-2.2]: hozzáadása a helyi DNS-kiszolgálók IP-címét.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-497">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add the IP addresses of the on-premises DNS servers.</span></span>  
    <span data-ttu-id="2b2f4-498">Kiterjesztheti a helyi DNS-kiszolgálók az Azure-ban futó virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-498">You can extend on-premises DNS servers to the virtual machines that are running in Azure.</span></span> <span data-ttu-id="2b2f4-499">A forgatókönyv adhat hozzá a DNS szolgáltatást futtató Azure virtuális gépek IP-címét.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-499">In that scenario, you can add the IP addresses of the Azure virtual machines on which you run the DNS service.</span></span>
    * <span data-ttu-id="2b2f4-500">[Csak felhőalapú telepítési][planning-guide-2.1]: helyezze üzembe a virtuális hálózati példányt a DNS-kiszolgáló látja, hogy egy további virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-500">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in the same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="2b2f4-501">Adja hozzá a DNS-szolgáltatás futtatásához beállítása az Azure virtuális gépek IP-címét.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-501">Add the IP addresses of the Azure virtual machines that you've set up to run DNS service.</span></span>

    ![12. ábra: DNS-kiszolgálók konfigurálása az Azure-beli virtuális hálózathoz][sap-ha-guide-figure-3001]

    <span data-ttu-id="2b2f4-503">_**12. ábra:** DNS konfigurálása az Azure Virtual Network kiszolgálók_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-503">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="2b2f4-504">Ha módosítja a DNS-kiszolgálók IP-címét, a módosítás alkalmazásához, és az új DNS-kiszolgálók propagálása Azure virtuális gépek újraindítására szeretné.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-504">If you change the IP addresses of the DNS servers, you need to restart the Azure virtual machines to apply the change and propagate the new DNS servers.</span></span>
  >
  >

<span data-ttu-id="2b2f4-505">A fenti példában a DNS-szolgáltatás telepítve és konfigurálva van a Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-505">In our example, the DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="2b2f4-506">Virtuálisgép-szerepkör</span><span class="sxs-lookup"><span data-stu-id="2b2f4-506">Virtual machine role</span></span> | <span data-ttu-id="2b2f4-507">Virtuális gép állomásneve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-507">Virtual machine host name</span></span> | <span data-ttu-id="2b2f4-508">Hálózati kártya neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-508">Network card name</span></span> | <span data-ttu-id="2b2f4-509">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="2b2f4-509">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2b2f4-510">Első DNS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2b2f4-510">First DNS server</span></span> |<span data-ttu-id="2b2f4-511">domcontr-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-511">domcontr-0</span></span> |<span data-ttu-id="2b2f4-512">PR1-nic-domcontr-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-512">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="2b2f4-513">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="2b2f4-513">10.0.0.10</span></span> |
| <span data-ttu-id="2b2f4-514">Második DNS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2b2f4-514">Second DNS server</span></span> |<span data-ttu-id="2b2f4-515">domcontr-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-515">domcontr-1</span></span> |<span data-ttu-id="2b2f4-516">PR1-nic-domcontr-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-516">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="2b2f4-517">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="2b2f4-517">10.0.0.11</span></span> |

### <span data-ttu-id="2b2f4-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Állomásnév és a statikus IP-címet a SAP ASC/SCS fürtözött példány és az adatbázis-kezelő fürtözött példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for the SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="2b2f4-519">A helyszíni telepítéshez szüksége ezek fenntartott állomásneve és IP-címek:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-519">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="2b2f4-520">Virtuális állomás neve szerepkör</span><span class="sxs-lookup"><span data-stu-id="2b2f4-520">Virtual host name role</span></span> | <span data-ttu-id="2b2f4-521">Virtuális állomás neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-521">Virtual host name</span></span> | <span data-ttu-id="2b2f4-522">Virtuális statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="2b2f4-522">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2b2f4-523">SAP ASC/SCS első fürt virtuális host name (kezelő)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-523">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="2b2f4-524">PR1-ASC-vir</span><span class="sxs-lookup"><span data-stu-id="2b2f4-524">pr1-ascs-vir</span></span> |<span data-ttu-id="2b2f4-525">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="2b2f4-525">10.0.0.42</span></span> |
| <span data-ttu-id="2b2f4-526">SAP ASC/SCS példány virtuális állomás neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-526">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="2b2f4-527">PR1-ASC-sap</span><span class="sxs-lookup"><span data-stu-id="2b2f4-527">pr1-ascs-sap</span></span> |<span data-ttu-id="2b2f4-528">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="2b2f4-528">10.0.0.43</span></span> |
| <span data-ttu-id="2b2f4-529">SAP DBMS második fürt virtuális állomásnevet (kezelő)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-529">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="2b2f4-530">PR1-dbms-vir</span><span class="sxs-lookup"><span data-stu-id="2b2f4-530">pr1-dbms-vir</span></span> |<span data-ttu-id="2b2f4-531">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="2b2f4-531">10.0.0.32</span></span> |

<span data-ttu-id="2b2f4-532">A fürt létrehozásakor hozzon létre a virtuális állomásnevek **pr1-ASC-vir** és **pr1-dbms-vir** és a társított IP-címek, amely egyrészt a fürt kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-532">When you create the cluster, create the virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and the associated IP addresses that manage the cluster itself.</span></span> <span data-ttu-id="2b2f4-533">Ezzel kapcsolatos további információkért lásd: [fürtcsomópontok fürtkonfiguráció gyűjtése][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-533">For information about how to do this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="2b2f4-534">A másik két virtuális állomásnevek, manuálisan is létrehozhat **pr1-ASC-sap** és **pr1-adatbázis-kezelő – sap**, és a társított IP-címek, a DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-534">You can manually create the other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and the associated IP addresses, on the DNS server.</span></span> <span data-ttu-id="2b2f4-535">A fürtözött SAP ASC/SCS-példány és a fürtözött adatbázis-kezelő példány használja ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-535">The clustered SAP ASCS/SCS instance and the clustered DBMS instance use these resources.</span></span> <span data-ttu-id="2b2f4-536">Ezzel kapcsolatos további információkért lásd: [hozzon létre egy virtuális nevet egy fürtözött SAP ASC/SCS példány][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-536">For information about how to do this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="2b2f4-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Állítsa be a statikus IP-címeket az SAP virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="2b2f4-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for the SAP virtual machines</span></span>
<span data-ttu-id="2b2f4-538">Miután telepítette a virtuális gépeket a fürtben használni, az összes virtuális gép statikus IP-címek be kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-538">After you deploy the virtual machines to use in your cluster, you need to set static IP addresses for all virtual machines.</span></span> <span data-ttu-id="2b2f4-539">Ehhez az Azure virtuális hálózat konfigurálása, és nem a vendég operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-539">Do this in the Azure Virtual Network configuration, and not in the guest operating system.</span></span>

1.  <span data-ttu-id="2b2f4-540">Válassza ki az Azure-portálon **erőforráscsoport** > **hálózati kártya** > **beállítások** > **IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-540">In the Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="2b2f4-541">Az a **IP-címek** panel alatt **hozzárendelés**, jelölje be **statikus**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-541">On the **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="2b2f4-542">Az a **IP-cím** mezőbe írja be a használni kívánt IP-cím.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-542">In the **IP address** box, enter the IP address that you want to use.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2b2f4-543">Ha módosítja a hálózati kártya IP-címét, indítsa újra az Azure virtuális gépeken, a módosítás alkalmazására van szükség.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-543">If you change the IP address of the network card, you need to restart the Azure virtual machines to apply the change.</span></span>  
  >
  >

  ![13. ábra: Állítsa be a statikus IP-címek a hálózati kártyát. az egyes virtuális gépek][sap-ha-guide-figure-3002]

  <span data-ttu-id="2b2f4-545">_**13. ábra:** statikus IP-címek a hálózati kártyát. az egyes virtuális gépek beállítása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-545">_**Figure 13:** Set static IP addresses for the network card of each virtual machine_</span></span>

  <span data-ttu-id="2b2f4-546">Ismételje meg ezt a lépést minden hálózati interfészen, hogy van, az összes virtuális gép, beleértve az Active Directory és a DNS szolgáltatás használni kívánt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-546">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want to use for your Active Directory/DNS service.</span></span>

<span data-ttu-id="2b2f4-547">A jelen példában vezetünk be a virtuális gépek és a statikus IP-címek:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-547">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="2b2f4-548">Virtuálisgép-szerepkör</span><span class="sxs-lookup"><span data-stu-id="2b2f4-548">Virtual machine role</span></span> | <span data-ttu-id="2b2f4-549">Virtuális gép állomásneve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-549">Virtual machine host name</span></span> | <span data-ttu-id="2b2f4-550">Hálózati kártya neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-550">Network card name</span></span> | <span data-ttu-id="2b2f4-551">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="2b2f4-551">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2b2f4-552">Első SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-552">First SAP Application Server instance</span></span> |<span data-ttu-id="2b2f4-553">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-553">pr1-di-0</span></span> |<span data-ttu-id="2b2f4-554">PR1-nic-di-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-554">pr1-nic-di-0</span></span> |<span data-ttu-id="2b2f4-555">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="2b2f4-555">10.0.0.50</span></span> |
| <span data-ttu-id="2b2f4-556">Második SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-556">Second SAP Application Server instance</span></span> |<span data-ttu-id="2b2f4-557">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-557">pr1-di-1</span></span> |<span data-ttu-id="2b2f4-558">PR1-nic-di-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-558">pr1-nic-di-1</span></span> |<span data-ttu-id="2b2f4-559">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="2b2f4-559">10.0.0.51</span></span> |
| <span data-ttu-id="2b2f4-560">...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-560">...</span></span> |<span data-ttu-id="2b2f4-561">...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-561">...</span></span> |<span data-ttu-id="2b2f4-562">...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-562">...</span></span> |<span data-ttu-id="2b2f4-563">...</span><span class="sxs-lookup"><span data-stu-id="2b2f4-563">...</span></span> |
| <span data-ttu-id="2b2f4-564">Utolsó SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-564">Last SAP Application Server instance</span></span> |<span data-ttu-id="2b2f4-565">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="2b2f4-565">pr1-di-5</span></span> |<span data-ttu-id="2b2f4-566">PR1-nic-di-5</span><span class="sxs-lookup"><span data-stu-id="2b2f4-566">pr1-nic-di-5</span></span> |<span data-ttu-id="2b2f4-567">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="2b2f4-567">10.0.0.55</span></span> |
| <span data-ttu-id="2b2f4-568">Első fürtcsomópontra ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-568">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="2b2f4-569">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-569">pr1-ascs-0</span></span> |<span data-ttu-id="2b2f4-570">PR1-nic-ASC-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-570">pr1-nic-ascs-0</span></span> |<span data-ttu-id="2b2f4-571">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="2b2f4-571">10.0.0.40</span></span> |
| <span data-ttu-id="2b2f4-572">Második fürtcsomópont ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-572">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="2b2f4-573">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-573">pr1-ascs-1</span></span> |<span data-ttu-id="2b2f4-574">PR1-nic-ASC-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-574">pr1-nic-ascs-1</span></span> |<span data-ttu-id="2b2f4-575">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="2b2f4-575">10.0.0.41</span></span> |
| <span data-ttu-id="2b2f4-576">Adatbázis-kezelő példány első fürtcsomópontra</span><span class="sxs-lookup"><span data-stu-id="2b2f4-576">First cluster node for DBMS instance</span></span> |<span data-ttu-id="2b2f4-577">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-577">pr1-db-0</span></span> |<span data-ttu-id="2b2f4-578">PR1-nic-db-0</span><span class="sxs-lookup"><span data-stu-id="2b2f4-578">pr1-nic-db-0</span></span> |<span data-ttu-id="2b2f4-579">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="2b2f4-579">10.0.0.30</span></span> |
| <span data-ttu-id="2b2f4-580">Adatbázis-kezelő példány második fürtcsomópont</span><span class="sxs-lookup"><span data-stu-id="2b2f4-580">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="2b2f4-581">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-581">pr1-db-1</span></span> |<span data-ttu-id="2b2f4-582">PR1-nic-db-1</span><span class="sxs-lookup"><span data-stu-id="2b2f4-582">pr1-nic-db-1</span></span> |<span data-ttu-id="2b2f4-583">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="2b2f4-583">10.0.0.31</span></span> |

### <span data-ttu-id="2b2f4-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Egy statikus IP-cím beállítása az Azure belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="2b2f4-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for the Azure internal load balancer</span></span>

<span data-ttu-id="2b2f4-585">Az SAP Azure Resource Manager sablonnal hoz létre Azure belső terheléselosztót a SAP ASC/SCS példány és az adatbázis-kezelő fürtön használt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-585">The SAP Azure Resource Manager template creates an Azure internal load balancer that is used for the SAP ASCS/SCS instance cluster and the DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b2f4-586">A virtuális gazdagép neve a SAP ASC/SCS IP-címe megegyezik a SAP ASC/SCS belső terheléselosztó IP-címe: **pr1-lb-ASC**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-586">The IP address of the virtual host name of the SAP ASCS/SCS is the same as the IP address of the SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="2b2f4-587">A DBMS neve virtuális IP-címe megegyezik az IP-cím, az adatbázis-kezelő belső terheléselosztó: **pr1-lb-dbms**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-587">The IP address of the virtual name of the DBMS is the same as the IP address of the DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="2b2f4-588">Egy statikus IP-cím beállítása az Azure belső terheléselosztóhoz:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-588">To set a static IP address for the Azure internal load balancer:</span></span>

1.  <span data-ttu-id="2b2f4-589">A kezdeti telepítés beállítja a belső terheléselosztó IP-cím **dinamikus**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-589">The initial deployment sets the internal load balancer IP address to **Dynamic**.</span></span> <span data-ttu-id="2b2f4-590">Az Azure portálon a a **IP-címek** panel alatt **hozzárendelés**, jelölje be **statikus**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-590">In the Azure portal, on the **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="2b2f4-591">Állítsa be az IP-cím, a belső terheléselosztó **pr1-lb-ASC** virtuális állomásnevének az SAP ASC/SCS példány IP-címre.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-591">Set the IP address of the internal load balancer **pr1-lb-ascs** to the IP address of the virtual host name of the SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="2b2f4-592">Állítsa be az IP-cím, a belső terheléselosztó **pr1-lb-dbms** virtuális állomásnevének az adatbázis-kezelő példány IP-címre.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-592">Set the IP address of the internal load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

  ![14. ábra: Az SAP ASC/SCS példány belső terheléselosztót beállítani statikus IP-címek][sap-ha-guide-figure-3003]

  <span data-ttu-id="2b2f4-594">_**14. ábra:** SAP ASC/SCS-példány a belső terheléselosztó statikus IP-címek beállítása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-594">_**Figure 14:** Set static IP addresses for the internal load balancer for the SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="2b2f4-595">Ebben a példában két Azure belső terheléselosztók a statikus IP-címmel rendelkező vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-595">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="2b2f4-596">Az Azure belső terheléselosztási szerepköréhez</span><span class="sxs-lookup"><span data-stu-id="2b2f4-596">Azure internal load balancer role</span></span> | <span data-ttu-id="2b2f4-597">Az Azure belső terheléselosztó neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-597">Azure internal load balancer name</span></span> | <span data-ttu-id="2b2f4-598">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="2b2f4-598">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2b2f4-599">SAP ASC/SCS példány belső terheléselosztót</span><span class="sxs-lookup"><span data-stu-id="2b2f4-599">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="2b2f4-600">PR1-lb-ASC</span><span class="sxs-lookup"><span data-stu-id="2b2f4-600">pr1-lb-ascs</span></span> |<span data-ttu-id="2b2f4-601">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="2b2f4-601">10.0.0.43</span></span> |
| <span data-ttu-id="2b2f4-602">SAP DBMS belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2b2f4-602">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="2b2f4-603">PR1-lb-adatbázis-kezelő</span><span class="sxs-lookup"><span data-stu-id="2b2f4-603">pr1-lb-dbms</span></span> |<span data-ttu-id="2b2f4-604">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="2b2f4-604">10.0.0.33</span></span> |


### <span data-ttu-id="2b2f4-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Alapértelmezett ASC/SCS terheléselosztási szabályok az Azure belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="2b2f4-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="2b2f4-606">A SAP Azure Resource Manager-sablon létrehozza a portok:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-606">The SAP Azure Resource Manager template creates the ports you need:</span></span>
* <span data-ttu-id="2b2f4-607">Az alapértelmezett példányszámának, ABAP ASC példány **00**</span><span class="sxs-lookup"><span data-stu-id="2b2f4-607">An ABAP ASCS instance, with the default instance number **00**</span></span>
* <span data-ttu-id="2b2f4-608">A Java SCS példány, az alapértelmezett példányszámának **01**</span><span class="sxs-lookup"><span data-stu-id="2b2f4-608">A Java SCS instance, with the default instance number **01**</span></span>

<span data-ttu-id="2b2f4-609">Ha a SAP ASC/SCS példányát telepíti, az alapértelmezett példányszámának kell használnia **00** ABAP ASC-példány és az alapértelmezett példányszámának **01** a Java SCS-példány.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-609">When you install your SAP ASCS/SCS instance, you must use the default instance number **00** for your ABAP ASCS instance and the default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="2b2f4-610">Ezután hozzon létre a szükséges belső terheléselosztási végpontok a SAP NetWeaver portok.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-610">Next, create required internal load balancing endpoints for the SAP NetWeaver ports.</span></span>

<span data-ttu-id="2b2f4-611">Szükséges belső terheléselosztási végpontok, először hozzon létre a terheléselosztást a SAP NetWeaver ABAP ASC portok végpontok:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-611">To create required internal load balancing endpoints, first, create these load balancing endpoints for the SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="2b2f4-612">Szolgáltatás/terheléselosztási szabály neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-612">Service/load balancing rule name</span></span> | <span data-ttu-id="2b2f4-613">Alapértelmezett portszámok</span><span class="sxs-lookup"><span data-stu-id="2b2f4-613">Default port numbers</span></span> | <span data-ttu-id="2b2f4-614">Konkrét portok (példányszámának 00 példány ASC) (SSZON 10)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-614">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2b2f4-615">Sorba helyezni Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-615">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="2b2f4-616">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-616">32<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-617">3200</span><span class="sxs-lookup"><span data-stu-id="2b2f4-617">3200</span></span> |
| <span data-ttu-id="2b2f4-618">ABAP üzenet Server / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-618">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="2b2f4-619">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-619">36<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-620">3600</span><span class="sxs-lookup"><span data-stu-id="2b2f4-620">3600</span></span> |
| <span data-ttu-id="2b2f4-621">Belső ABAP üzenet / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-621">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="2b2f4-622">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-622">39<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-623">3900</span><span class="sxs-lookup"><span data-stu-id="2b2f4-623">3900</span></span> |
| <span data-ttu-id="2b2f4-624">A kiszolgáló HTTP-üzenet / *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-624">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="2b2f4-625">81-es <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-625">81<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-626">8100</span><span class="sxs-lookup"><span data-stu-id="2b2f4-626">8100</span></span> |
| <span data-ttu-id="2b2f4-627">SAP Start ASC a HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="2b2f4-628">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="2b2f4-628">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="2b2f4-629">50013</span><span class="sxs-lookup"><span data-stu-id="2b2f4-629">50013</span></span> |
| <span data-ttu-id="2b2f4-630">SAP Start szolgáltatás ASC HTTPS / *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="2b2f4-631">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="2b2f4-631">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="2b2f4-632">50014</span><span class="sxs-lookup"><span data-stu-id="2b2f4-632">50014</span></span> |
| <span data-ttu-id="2b2f4-633">Sorba helyezni replikációs / *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-633">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="2b2f4-634">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="2b2f4-634">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="2b2f4-635">50016</span><span class="sxs-lookup"><span data-stu-id="2b2f4-635">50016</span></span> |
| <span data-ttu-id="2b2f4-636">SAP Start SSZON HTTP-kérelmekre *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-636">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="2b2f4-637">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="2b2f4-637">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="2b2f4-638">51013</span><span class="sxs-lookup"><span data-stu-id="2b2f4-638">51013</span></span> |
| <span data-ttu-id="2b2f4-639">SAP Start SSZON HTTP-kérelmekre *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-639">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="2b2f4-640">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="2b2f4-640">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="2b2f4-641">51014</span><span class="sxs-lookup"><span data-stu-id="2b2f4-641">51014</span></span> |
| <span data-ttu-id="2b2f4-642">Erőforrás-kezelő Win *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-642">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="2b2f4-643">5985</span><span class="sxs-lookup"><span data-stu-id="2b2f4-643">5985</span></span> |
| <span data-ttu-id="2b2f4-644">Fájlmegosztás *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-644">File Share *Lbrule445*</span></span> | |<span data-ttu-id="2b2f4-645">445</span><span class="sxs-lookup"><span data-stu-id="2b2f4-645">445</span></span> |

<span data-ttu-id="2b2f4-646">_**1. táblázat:** portszámokat SAP NetWeaver ABAP ASC példánya_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-646">_**Table 1:** Port numbers of the SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="2b2f4-647">Ezután hozzon létre a terheléselosztást a SAP NetWeaver Java SCS portok végpontok:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-647">Then, create these load balancing endpoints for the SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="2b2f4-648">Szolgáltatás/terheléselosztási szabály neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-648">Service/load balancing rule name</span></span> | <span data-ttu-id="2b2f4-649">Alapértelmezett portszámok</span><span class="sxs-lookup"><span data-stu-id="2b2f4-649">Default port numbers</span></span> | <span data-ttu-id="2b2f4-650">Konkrét portok (SCS példány példányszámának 01) (SSZON 11)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-650">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2b2f4-651">Sorba helyezni Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-651">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="2b2f4-652">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-652">32<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-653">3201</span><span class="sxs-lookup"><span data-stu-id="2b2f4-653">3201</span></span> |
| <span data-ttu-id="2b2f4-654">Átjárókiszolgáló / *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-654">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="2b2f4-655">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-655">33<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-656">3301</span><span class="sxs-lookup"><span data-stu-id="2b2f4-656">3301</span></span> |
| <span data-ttu-id="2b2f4-657">Java-üzenet Server / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-657">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="2b2f4-658">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-658">39<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-659">3901</span><span class="sxs-lookup"><span data-stu-id="2b2f4-659">3901</span></span> |
| <span data-ttu-id="2b2f4-660">A kiszolgáló HTTP-üzenet / *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-660">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="2b2f4-661">81-es <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="2b2f4-661">81<*InstanceNumber*></span></span> |<span data-ttu-id="2b2f4-662">8101</span><span class="sxs-lookup"><span data-stu-id="2b2f4-662">8101</span></span> |
| <span data-ttu-id="2b2f4-663">SAP Start SCS a HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-663">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="2b2f4-664">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="2b2f4-664">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="2b2f4-665">50113</span><span class="sxs-lookup"><span data-stu-id="2b2f4-665">50113</span></span> |
| <span data-ttu-id="2b2f4-666">SAP Start szolgáltatás SCS HTTPS / *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="2b2f4-667">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="2b2f4-667">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="2b2f4-668">50114</span><span class="sxs-lookup"><span data-stu-id="2b2f4-668">50114</span></span> |
| <span data-ttu-id="2b2f4-669">Sorba helyezni replikációs / *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-669">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="2b2f4-670">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="2b2f4-670">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="2b2f4-671">50116</span><span class="sxs-lookup"><span data-stu-id="2b2f4-671">50116</span></span> |
| <span data-ttu-id="2b2f4-672">SAP Start SSZON HTTP-kérelmekre *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-672">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="2b2f4-673">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="2b2f4-673">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="2b2f4-674">51113</span><span class="sxs-lookup"><span data-stu-id="2b2f4-674">51113</span></span> |
| <span data-ttu-id="2b2f4-675">SAP Start SSZON HTTP-kérelmekre *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-675">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="2b2f4-676">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="2b2f4-676">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="2b2f4-677">51114</span><span class="sxs-lookup"><span data-stu-id="2b2f4-677">51114</span></span> |
| <span data-ttu-id="2b2f4-678">Erőforrás-kezelő Win *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-678">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="2b2f4-679">5985</span><span class="sxs-lookup"><span data-stu-id="2b2f4-679">5985</span></span> |
| <span data-ttu-id="2b2f4-680">Fájlmegosztás *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="2b2f4-680">File Share *Lbrule445*</span></span> | |<span data-ttu-id="2b2f4-681">445</span><span class="sxs-lookup"><span data-stu-id="2b2f4-681">445</span></span> |

<span data-ttu-id="2b2f4-682">_**2. táblázat:** portszámot az SAP NetWeaver Java SCS-példányok_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-682">_**Table 2:** Port numbers of the SAP NetWeaver Java SCS instances_</span></span>

![15. ábra: Az alapértelmezett ASC/SCS betöltése az Azure belső terheléselosztóhoz tartozó terheléselosztási szabályok][sap-ha-guide-figure-3004]

<span data-ttu-id="2b2f4-684">_**15. ábra:** alapértelmezett ASC/SCS terheléselosztási szabályok az Azure belső terheléselosztóhoz_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-684">_**Figure 15:** Default ASCS/SCS load balancing rules for the Azure internal load balancer_</span></span>

<span data-ttu-id="2b2f4-685">A terheléselosztó IP-cím beállítása **pr1-lb-dbms** virtuális állomásnevének az adatbázis-kezelő példány IP-címre.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-685">Set the IP address of the load balancer **pr1-lb-dbms** to the IP address of the virtual host name of the DBMS instance.</span></span>

### <span data-ttu-id="2b2f4-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Módosítsa a ASC/SCS alapértelmezett terheléselosztási szabályok az Azure belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="2b2f4-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change the ASCS/SCS default load balancing rules for the Azure internal load balancer</span></span>

<span data-ttu-id="2b2f4-687">Ha a SAP ASC vagy SCS példányok eltérő számú használni kívánt, módosítania kell a nevek és értékek a portok alapértelmezett értékekhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-687">If you want to use different numbers for the SAP ASCS or SCS instances, you must change the names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="2b2f4-688">Válassza ki az Azure-portálon  **<* SID*> - lb - ASC betöltése terheléselosztó ** > **terheléselosztási szabályok betöltése**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-688">In the Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="2b2f4-689">Minden terheléselosztási szabályok, amelyek az SAP ASC vagy SCS példányhoz tartozik ezek az értékek módosítása:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-689">For all load balancing rules that belong to the SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="2b2f4-690">Név</span><span class="sxs-lookup"><span data-stu-id="2b2f4-690">Name</span></span>
  * <span data-ttu-id="2b2f4-691">Port</span><span class="sxs-lookup"><span data-stu-id="2b2f4-691">Port</span></span>
  * <span data-ttu-id="2b2f4-692">Háttér-port</span><span class="sxs-lookup"><span data-stu-id="2b2f4-692">Back-end port</span></span>

  <span data-ttu-id="2b2f4-693">Például ha szeretné módosítani az alapértelmezett ASC példányszámának a 00 és 31, szeretné hajtsa végre a módosításokat minden porthoz az 1.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-693">For example, if you want to change the default ASCS instance number from 00 to 31, you need to make the changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="2b2f4-694">Példa port frissítési *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-694">Here's an example of an update for port *lbrule3200*.</span></span>

  ![16. ábra: Módosítsa a ASC/SCS alapértelmezett terheléselosztási szabályok az Azure belső terheléselosztóhoz][sap-ha-guide-figure-3005]

  <span data-ttu-id="2b2f4-696">_**16. ábra:** ASC/SCS alapértelmezett terheléselosztási az Azure belső terheléselosztóhoz tartozó szabályok módosítása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-696">_**Figure 16:** Change the ASCS/SCS default load balancing rules for the Azure internal load balancer_</span></span>

### <span data-ttu-id="2b2f4-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuális gépek felvételét a tartományba</span><span class="sxs-lookup"><span data-stu-id="2b2f4-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines to the domain</span></span>

<span data-ttu-id="2b2f4-698">Miután egy statikus IP-címet rendel a virtuális gépek, a virtuális gépek felvételét a tartományba.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-698">After you assign a static IP address to the virtual machines, add the virtual machines to the domain.</span></span>

![17. ábra: A virtuális gépek hozzáadása a tartományhoz][sap-ha-guide-figure-3006]

<span data-ttu-id="2b2f4-700">_**17. ábra:** felvesz egy virtuális gépet egy tartományhoz_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-700">_**Figure 17:** Add a virtual machine to a domain_</span></span>

### <span data-ttu-id="2b2f4-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Adja hozzá a beállításjegyzék-bejegyzések az SAP ASC/SCS példány mindkét fürtcsomóponton</span><span class="sxs-lookup"><span data-stu-id="2b2f4-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of the SAP ASCS/SCS instance</span></span>

<span data-ttu-id="2b2f4-702">Az Azure terheléselosztó a belső terheléselosztók, hogy a kapcsolatok bezárása, amikor a kapcsolat üresjáratban a megadott ideig (üresjárati időkorlátot) időben rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-702">Azure Load Balancer has an internal load balancer that closes connections when the connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="2b2f4-703">SAP munkafolyamatok párbeszédpanel példányok nyitott kapcsolatok az SAP sorba, amint az első sorba helyezni/created küldje kérelem küldésének kell feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-703">SAP work processes in dialog instances open connections to the SAP enqueue process as soon as the first enqueue/dequeue request needs to be sent.</span></span> <span data-ttu-id="2b2f4-704">Ezek a kapcsolatok általában marad a meghatározott a munkahelyi folyamat, vagy a sorba helyezni folyamat újraindítja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-704">These connections usually remain established until the work process or the enqueue process restarts.</span></span> <span data-ttu-id="2b2f4-705">Azonban ha a kapcsolat üresjáratban a beállított időn belül, az Azure belső terheléselosztó bezárja a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-705">However, if the connection is idle for a set period of time, the Azure internal load balancer closes the connections.</span></span> <span data-ttu-id="2b2f4-706">Ez nem probléma, mert a SAP munkahelyi folyamat újra létrehozza a kapcsolatot, a várólistára helyezés folyamatban, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-706">This isn't a problem because the SAP work process reestablishes the connection to the enqueue process if it no longer exists.</span></span> <span data-ttu-id="2b2f4-707">Ezek a tevékenységek SAP folyamatok fejlesztői nyomait vannak dokumentálva, de ezeket a nyomkövetéseket a felesleges tartalmat nagy mennyiségű hoznak létre.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-707">These activities are documented in the developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="2b2f4-708">Jó ötlet módosítása a TCP/IP `KeepAliveTime` és `KeepAliveInterval` mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-708">It's a good idea to change the TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="2b2f4-709">Ezek a változások, a TCP/IP-paraméterek SAP profil paraméterekkel, a cikk későbbi részében leírt össze.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-709">Combine these changes in the TCP/IP parameters with SAP profile parameters, described later in the article.</span></span>

<span data-ttu-id="2b2f4-710">Mindkét fürtcsomópont az SAP ASC/SCS példány beállításjegyzék-bejegyzések hozzáadásához először adja hozzá a Windows beállításjegyzék-bejegyzések mindkét Windows fürtcsomópontokon az SAP ASC/SCS:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-710">To add registry entries on both cluster nodes of the SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="2b2f4-711">Elérési út</span><span class="sxs-lookup"><span data-stu-id="2b2f4-711">Path</span></span> | <span data-ttu-id="2b2f4-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="2b2f4-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="2b2f4-713">Változó neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-713">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="2b2f4-714">Változó típusa</span><span class="sxs-lookup"><span data-stu-id="2b2f4-714">Variable type</span></span> |<span data-ttu-id="2b2f4-715">REG_DWORD (decimális)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-715">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="2b2f4-716">Érték</span><span class="sxs-lookup"><span data-stu-id="2b2f4-716">Value</span></span> |<span data-ttu-id="2b2f4-717">120000</span><span class="sxs-lookup"><span data-stu-id="2b2f4-717">120000</span></span> |
| <span data-ttu-id="2b2f4-718">Dokumentáció csatolása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-718">Link to documentation</span></span> |[<span data-ttu-id="2b2f4-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="2b2f4-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="2b2f4-720">_**3. táblázat:** módosítsa az első TCP/IP-paraméter_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-720">_**Table 3:** Change the first TCP/IP parameter_</span></span>

<span data-ttu-id="2b2f4-721">Ezt követően adja hozzá a Windows beállításjegyzék-bejegyzések mindkét Windows fürtcsomópontokon az SAP ASC/SCS:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-721">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="2b2f4-722">Elérési út</span><span class="sxs-lookup"><span data-stu-id="2b2f4-722">Path</span></span> | <span data-ttu-id="2b2f4-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="2b2f4-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="2b2f4-724">Változó neve</span><span class="sxs-lookup"><span data-stu-id="2b2f4-724">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="2b2f4-725">Változó típusa</span><span class="sxs-lookup"><span data-stu-id="2b2f4-725">Variable type</span></span> |<span data-ttu-id="2b2f4-726">REG_DWORD (decimális)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-726">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="2b2f4-727">Érték</span><span class="sxs-lookup"><span data-stu-id="2b2f4-727">Value</span></span> |<span data-ttu-id="2b2f4-728">120000</span><span class="sxs-lookup"><span data-stu-id="2b2f4-728">120000</span></span> |
| <span data-ttu-id="2b2f4-729">Dokumentáció csatolása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-729">Link to documentation</span></span> |[<span data-ttu-id="2b2f4-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="2b2f4-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="2b2f4-731">_**4. táblázat:** módosítása a második TCP/IP-paraméter_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-731">_**Table 4:** Change the second TCP/IP parameter_</span></span>

<span data-ttu-id="2b2f4-732">**A módosítások alkalmazásához, indítsa újra a mindkét fürtcsomópontot**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-732">**To apply the changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="2b2f4-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Egy SAP ASC/SCS példánya számára a Windows Server feladatátvételi fürtszolgáltatási fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="2b2f4-734">A Windows Server feladatátvételi fürtszolgáltatási fürt egy SAP ASC/SCS-példány beállítása magában foglalja a ezeket a feladatokat:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-734">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="2b2f4-735">Fürtözött konfigurációban a fürt csomópontjai gyűjtése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-735">Collecting the cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="2b2f4-736">A fürt tanúsító fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-736">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="2b2f4-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Fürtözött konfigurációban a fürt csomópontjai gyűjtése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect the cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="2b2f4-738">A szerepkör hozzáadása és szolgáltatások varázsló vegye fel a Feladatátvételi fürtszolgáltatást mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-738">In the Add Role and Features Wizard, add failover clustering to both cluster nodes.</span></span>
2.  <span data-ttu-id="2b2f4-739">A feladatátvevő fürt beállítása a Feladatátvevőfürt-kezelő használatával.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-739">Set up the failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="2b2f4-740">A Feladatátvevőfürt-kezelőben válasszon **fürt létrehozása**, és adja meg csak az első fürt, a csomópont A. neve A második csomópont ne adjon hozzá még; a második csomópont egy későbbi lépésben fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-740">In Failover Cluster Manager, select **Create Cluster**, and then add only the name of the first cluster, node A. Do not add the second node yet; you'll add the second node in a later step.</span></span>

  ![18. ábra: A kiszolgáló vagy a virtuális gép nevét, az első fürtcsomópont hozzáadása][sap-ha-guide-figure-3007]

  <span data-ttu-id="2b2f4-742">_**18. ábra:** adja hozzá az első fürtcsomópontra a kiszolgáló vagy a virtuális gép neve_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-742">_**Figure 18:** Add the server or virtual machine name of the first cluster node_</span></span>

3.  <span data-ttu-id="2b2f4-743">Adja meg a fürt hálózati neve (virtuális állomás neve).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-743">Enter the network name (virtual host name) of the cluster.</span></span>

  ![19. ábra: Adja meg a fürt neve][sap-ha-guide-figure-3008]

  <span data-ttu-id="2b2f4-745">_**19. ábra:** adja meg a fürt neve_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-745">_**Figure 19:** Enter the cluster name_</span></span>

4.  <span data-ttu-id="2b2f4-746">Miután létrehozta a fürthöz, futtassa a fürtellenőrzési tesztet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-746">After you've created the cluster, run a cluster validation test.</span></span>

  ![20. ábra: A fürt érvényesítése-ellenőrzés futtatása][sap-ha-guide-figure-3009]

  <span data-ttu-id="2b2f4-748">_**20. ábra:** a fürt érvényesítése-ellenőrzés futtatása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-748">_**Figure 20:** Run the cluster validation check_</span></span>

  <span data-ttu-id="2b2f4-749">Ezen a ponton a folyamat lemezek kapcsolatos figyelmeztetéseket figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-749">You can ignore any warnings about disks at this point in the process.</span></span> <span data-ttu-id="2b2f4-750">Egy tanúsító fájlmegosztást és a SIOS megosztott lemezek később fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-750">You'll add a file share witness and the SIOS shared disks later.</span></span> <span data-ttu-id="2b2f4-751">Ezen a ponton nem kell foglalkoznia a kvórum.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-751">At this stage, you don't need to worry about having a quorum.</span></span>

  ![21. ábra: Kvórumlemez nem található][sap-ha-guide-figure-3010]

  <span data-ttu-id="2b2f4-753">_**21. ábra:** kvórumlemez nem található_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-753">_**Figure 21:** No quorum disk is found_</span></span>

  ![22. ábra: Core fürterőforrás kell egy új IP-cím][sap-ha-guide-figure-3011]

  <span data-ttu-id="2b2f4-755">_**22. ábra:** Core fürterőforrás kell egy új IP-cím_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-755">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="2b2f4-756">A core fürtszolgáltatás az IP-címének módosítása.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-756">Change the IP address of the core cluster service.</span></span> <span data-ttu-id="2b2f4-757">A fürt nem indítható el, amíg nem módosítja az IP-cím a core fürtszolgáltatás, mert a kiszolgáló IP-címét a virtuális gép csomópontok egyikére.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-757">The cluster can't start until you change the IP address of the core cluster service, because the IP address of the server points to one of the virtual machine nodes.</span></span> <span data-ttu-id="2b2f4-758">Az ehhez a **tulajdonságok** a core fürtszolgáltatás IP-erőforrás oldalán.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-758">Do this on the **Properties** page of the core cluster service's IP resource.</span></span>

  <span data-ttu-id="2b2f4-759">Például IP-címet kell (a példánkban **10.0.0.42**) a fürt virtuális állomás neve **pr1-ASC-vir**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-759">For example, we need to assign an IP address (in our example, **10.0.0.42**) for the cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![23. ábra: A Tulajdonságok párbeszédpanelen az IP-címének módosítása][sap-ha-guide-figure-3012]

  <span data-ttu-id="2b2f4-761">_**23. ábra:** a a **tulajdonságok** párbeszédpanel változtassa meg az IP-cím_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-761">_**Figure 23:** In the **Properties** dialog box, change the IP address_</span></span>

  ![24. ábra: Az a fürt számára fenntartott IP-cím hozzárendelése][sap-ha-guide-figure-3013]

  <span data-ttu-id="2b2f4-763">_**24. ábra:** a fürt számára fenntartott IP-cím hozzárendelése_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-763">_**Figure 24:** Assign the IP address that is reserved for the cluster_</span></span>

6.  <span data-ttu-id="2b2f4-764">A fürt virtuális állomásnevet online állapotba.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-764">Bring the cluster virtual host name online.</span></span>

  ![25. ábra: Core a fürtszolgáltatás működik-e és fut, és a megfelelő IP-cím][sap-ha-guide-figure-3014]

  <span data-ttu-id="2b2f4-766">_**25. ábra:** core a fürtszolgáltatás működik-e és fut, és a megfelelő IP-cím_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-766">_**Figure 25:** Cluster core service is up and running, and with the correct IP address_</span></span>

7.  <span data-ttu-id="2b2f4-767">Adja hozzá a második fürtcsomópontokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-767">Add the second cluster node.</span></span>

  <span data-ttu-id="2b2f4-768">Most, hogy a core fürtszolgáltatás működik és elérhető, a második fürtcsomópont is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-768">Now that the core cluster service is up and running, you can add the second cluster node.</span></span>

  ![26. ábra: A második csomópont hozzáadása][sap-ha-guide-figure-3015]

  <span data-ttu-id="2b2f4-770">_**26. ábra:** második csomópont hozzáadása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-770">_**Figure 26:** Add the second cluster node_</span></span>

8.  <span data-ttu-id="2b2f4-771">Adjon meg egy nevet, a második csomópont gazda esetében.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-771">Enter a name for the second cluster node host.</span></span>

  ![27. ábra: Adja meg a második fürt csomópont állomásnév][sap-ha-guide-figure-3016]

  <span data-ttu-id="2b2f4-773">_**27. ábra:** adja meg a második fürt csomópont állomásnév_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-773">_**Figure 27:** Enter the second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="2b2f4-774">Ügyeljen arra, hogy a **minden megfelelő tároló felvétele a fürt** jelölőnégyzet **nem** kijelölt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-774">Be sure that the **Add all eligible storage to the cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![28. ábra: Jelölje be a jelölőnégyzetet][sap-ha-guide-figure-3017]

  <span data-ttu-id="2b2f4-776">_**28. ábra:** tegye **nem** jelölje be a jelölőnégyzetet_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-776">_**Figure 28:** Do **not** select the check box_</span></span>

  <span data-ttu-id="2b2f4-777">Kvórum és lemezek kapcsolatos figyelmeztetések figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-777">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="2b2f4-778">Akkor lesz a kvórum és megosztja a lemez később, a [telepítése SIOS DataKeeper Cluster Edition SAP ASC/SCS megosztás olyan fürtlemez esetében][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-778">You'll set the quorum and share the disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![29. ábra: A lemez kvórumával kapcsolatos figyelmeztetések mellőzése][sap-ha-guide-figure-3018]

  <span data-ttu-id="2b2f4-780">_**29. ábra:** a lemez kvórumával kapcsolatos figyelmeztetések mellőzése_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-780">_**Figure 29:** Ignore warnings about the disk quorum_</span></span>


#### <span data-ttu-id="2b2f4-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>A fürt tanúsító fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="2b2f4-782">Ezeket a feladatokat a fürt tanúsító fájlmegosztás beállítása foglal magában:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-782">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="2b2f4-783">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-783">Creating a file share</span></span>
- <span data-ttu-id="2b2f4-784">A fájl megosztási tanúsító Kvórum beállítása a Feladatátvevőfürt-kezelőben</span><span class="sxs-lookup"><span data-stu-id="2b2f4-784">Setting the file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="2b2f4-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="2b2f4-786">Válassza ki a tanúsító fájlmegosztást egy kvórumlemez helyett.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-786">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="2b2f4-787">SIOS DataKeeper támogatja ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-787">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="2b2f4-788">A cikkben szereplő példákban a tanúsító fájlmegosztás az Azure-ban futó Active Directory és a DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-788">In the examples in this article, the file share witness is on the Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="2b2f4-789">A tanúsító fájlmegosztás neve **domcontr-0**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-789">The file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="2b2f4-790">Volna konfigurálta az Azure-ba (telephelyek közötti VPN vagy Azure ExpressRoute) keresztül VPN-kapcsolat, mert az Active Directory és a DNS szolgáltatás a helyi, és nem megfelelő futtatni egy tanúsító ossza meg.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-790">Because you would have configured a VPN connection to Azure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable to run a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2b2f4-791">Ha az Active Directory és a DNS szolgáltatás csak a helyszíni fut, ne állítson be a tanúsító fájlmegosztás fut-e a helyszíni Active Directory és a DNS a Windows operációs rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-791">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on the Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="2b2f4-792">Előfordulhat, hogy fut az Azure Active Directory és a DNS a helyszíni és a fürtcsomópontok közötti hálózati késés túl nagy, és a csatlakozási problémák miatt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-792">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="2b2f4-793">Ne felejtse el a tanúsító fájlmegosztás beállítása megközelíti a fürtcsomópont futtató Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-793">Be sure to configure the file share witness on an Azure virtual machine that is running close to the cluster node.</span></span>  
  >
  >

  <span data-ttu-id="2b2f4-794">A kvórum meghajtó legalább 1024 MB szabad területre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-794">The quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="2b2f4-795">Azt javasoljuk, hogy a 2048 MB szabad lemezterületet a kvórum meghajtót.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-795">We recommend 2,048 MB of free space for the quorum drive.</span></span>

2.  <span data-ttu-id="2b2f4-796">Adja hozzá a fürtnévobjektum.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-796">Add the cluster name object.</span></span>

  ![30. ábra: A megosztást a fürtnévobjektum vonatkozó engedélyek hozzárendelése][sap-ha-guide-figure-3019]

  <span data-ttu-id="2b2f4-798">_**30. ábra:** a megosztást a fürtnévobjektum vonatkozó engedélyek hozzárendelése_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-798">_**Figure 30:** Assign the permissions on the share for the cluster name object_</span></span>

  <span data-ttu-id="2b2f4-799">Ne feledje, hogy az engedélyek tartalmazza-e az adatok módosítása a megosztást a fürtnévobjektum-kezelő szolgáltatóként (a példánkban **pr1-ASC-vir$**).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-799">Be sure that the permissions include the authority to change data in the share for the cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="2b2f4-800">A fürtnévobjektum felvenni a listára, válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-800">To add the cluster name object to the list, select **Add**.</span></span> <span data-ttu-id="2b2f4-801">Módosítsa a szűrőt, hogy ellenőrizze a számítógép-objektumok kívül 31. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-801">Change the filter to check for computer objects, in addition to those shown in Figure 31.</span></span>

  ![31. ábra: Változás az Objektumtípusok számítógép felvétele][sap-ha-guide-figure-3020]

  <span data-ttu-id="2b2f4-803">_**31. ábra:** számítógépek felvenni objektumtípusok módosítása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-803">_**Figure 31:** Change the Object Types to include computers_</span></span>

  ![32. ábra: Jelölje be a számítógépek][sap-ha-guide-figure-3021]

  <span data-ttu-id="2b2f4-805">_**32. ábra:** válassza ki a **számítógépek** jelölőnégyzetet_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-805">_**Figure 32:** Select the **Computers** check box_</span></span>

4.  <span data-ttu-id="2b2f4-806">Adja meg a fürtnévobjektum, ahogy az ábra 31.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-806">Enter the cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="2b2f4-807">A bejegyzést már létrejött, mert az engedélyek módosíthatja, ahogy az ábra 30.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-807">Because the record has already been created, you can change the permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="2b2f4-808">Válassza ki a **biztonsági** a megosztást, és majd lapján részletesebb a fürtnévobjektum engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-808">Select the **Security** tab of the share, and then set more detailed permissions for the cluster name object.</span></span>

  ![33. ábra: A fájl megosztási kvórum a fürt neve objektum biztonsági attribútumainak beállítása][sap-ha-guide-figure-3022]

  <span data-ttu-id="2b2f4-810">_**33. ábra:** a a fájl megosztási kvórum a fürt neve objektum biztonsági attribútumainak beállítása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-810">_**Figure 33:** Set the security attributes for the cluster name object on the file share quorum_</span></span>

##### <span data-ttu-id="2b2f4-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Állítsa be a fájl megosztási tanúsító kvórum a Feladatátvevőfürt-kezelőben</span><span class="sxs-lookup"><span data-stu-id="2b2f4-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set the file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="2b2f4-812">Nyissa meg a fürtkvórum beállítása varázsló konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-812">Open the Configure Quorum Setting Wizard.</span></span>

  ![34. ábra: A konfigurálás fürtkvórum beállítása varázsló indítása][sap-ha-guide-figure-3023]

  <span data-ttu-id="2b2f4-814">_**34. ábra:** a konfigurálás fürtkvórum beállítása varázsló indítása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-814">_**Figure 34:** Start the Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="2b2f4-815">Az a **kvórumkonfiguráció kiválasztása** lapon, hogy melyik **a kvórum tanúsítójának kijelölése**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-815">On the **Select Quorum Configuration** page, select **Select the quorum witness**.</span></span>

  ![35. ábra: Választhat kvórumkonfigurációinak][sap-ha-guide-figure-3024]

  <span data-ttu-id="2b2f4-817">_**35. ábra:** választhat kvórumkonfigurációinak_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-817">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="2b2f4-818">Az a **kvórum Tanúsítójának kijelölése** lapon, hogy melyik **konfigurálása egy tanúsító fájlmegosztást**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-818">On the **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![36. ábra: Válassza ki a tanúsító fájlmegosztás][sap-ha-guide-figure-3025]

  <span data-ttu-id="2b2f4-820">_**36. ábra:** válassza ki a tanúsító fájlmegosztás_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-820">_**Figure 36:** Select the file share witness_</span></span>

4.  <span data-ttu-id="2b2f4-821">Adja meg az UNC elérési útnak a fájlmegosztásra (a példánkban \\domcontr-0\FSW).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-821">Enter the UNC path to the file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="2b2f4-822">A módosításokat végezhet listájának megtekintéséhez válasszon **következő**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-822">To see a list of the changes you can make, select **Next**.</span></span>

  ![37. ábra: A fájlmegosztási helyet a tanúsító fájlmegosztás meghatározása][sap-ha-guide-figure-3026]

  <span data-ttu-id="2b2f4-824">_**37. ábra:** határozza meg a fájlmegosztás helyét a tanúsító fájlmegosztás_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-824">_**Figure 37:** Define the file share location for the witness share_</span></span>

5.  <span data-ttu-id="2b2f4-825">Válassza ki a megfelelő módosításokat, majd válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-825">Select the changes you want, and then select **Next**.</span></span> <span data-ttu-id="2b2f4-826">Sikeresen konfigurálja újra a fürtkonfiguráció ahogy az ábra 38 kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-826">You need to successfully reconfigure the cluster configuration as shown in Figure 38.</span></span>  

  ![38. ábra: Megerősítése, hogy a fürt már újra konfigurálni][sap-ha-guide-figure-3027]

  <span data-ttu-id="2b2f4-828">_**38. ábra:** , hogy a fürt már újra konfigurálni megerősítése_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-828">_**Figure 38:** Confirmation that you've reconfigured the cluster_</span></span>

<span data-ttu-id="2b2f4-829">A Windows feladatátvevő fürt sikeres telepítését követően módosítások szükség lehet néhány küszöbértékek való igazításának lehetősége feladatátvételi észlelési feltételeket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-829">After installing the Windows Failover Cluster successfully, changes need to be made to some thresholds to adapt failover detection to conditions in Azure.</span></span> <span data-ttu-id="2b2f4-830">Módosítani kell a paraméterei dokumentálva vannak ebben a blogban: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-830">The parameters to be changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="2b2f4-831">Feltételezve, hogy a két virtuális gépekre, amelyek a Windows-fürt konfigurációs építése ASC/SCS ugyanazon az alhálózaton találhatók, a következő paramétereket kell módosítani ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-831">Assuming that your two VMs that build the Windows Cluster Configuration for ASCS/SCS are in the same SubNet, the following parameters need to be changed to these values:</span></span>
- <span data-ttu-id="2b2f4-832">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="2b2f4-832">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="2b2f4-833">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="2b2f4-833">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="2b2f4-834">Ezeket a beállításokat felhasználók tesztelése és megadott kell lennie, elég rugalmas az egyik oldalon a helyes biztonsági sérülése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-834">These settings were tested with customers and provided a good compromise to be resilient enough on the one side.</span></span> <span data-ttu-id="2b2f4-835">Másfelől ezeket a beállításokat volt biztosítása gyors elég feladatátvételi valós hibaüzenet feltételekben az SAP-szoftver vagy a csomópont vagy Virtuálisgép-hiba.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-835">On the other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="2b2f4-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Az SAP ASC/SCS fürtlemez-megosztás SIOS DataKeeper Cluster Edition telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="2b2f4-837">Most már rendelkezik egy működő Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-837">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="2b2f4-838">De egy SAP ASC/SCS példányát telepítenie kell egy megosztott lemez erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-838">But, to install an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="2b2f4-839">Nem hozható létre a szükséges megosztott lemez erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-839">You cannot create the shared disk resources you need in Azure.</span></span> <span data-ttu-id="2b2f4-840">SIOS DataKeeper Cluster Edition egy olyan külső megoldás, megosztott lemez erőforrások létrehozására használhatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-840">SIOS DataKeeper Cluster Edition is a third-party solution you can use to create shared disk resources.</span></span>

<span data-ttu-id="2b2f4-841">Ezeket a feladatokat az SAP ASC/SCS megosztás fürtlemezt SIOS DataKeeper Cluster Edition telepítését foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-841">Installing SIOS DataKeeper Cluster Edition for the SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="2b2f4-842">A .NET-keretrendszer 3.5-ös hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-842">Adding the .NET Framework 3.5</span></span>
- <span data-ttu-id="2b2f4-843">SIOS DataKeeper telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-843">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="2b2f4-844">SIOS DataKeeper beállítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-844">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="2b2f4-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>Adja hozzá a .NET-keretrendszer 3.5-ös verzióját</span><span class="sxs-lookup"><span data-stu-id="2b2f4-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add the .NET Framework 3.5</span></span>
<span data-ttu-id="2b2f4-846">A Microsoft .NET-keretrendszer 3.5-ös verzióját nem automatikusan aktiválva, vagy Windows Server 2012 R2 rendszeren telepítve.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-846">The Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="2b2f4-847">Mivel SIOS DataKeeper szüksége van a .NET-keretrendszer telepíthető DataKeeper minden csomóponton, telepítenie kell a .NET-keretrendszer 3.5 az összes virtuális gépet a fürt a vendég operációs rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-847">Because SIOS DataKeeper requires the .NET Framework to be on all nodes that you install DataKeeper on, you must install the .NET Framework 3.5 on the guest operating system of all virtual machines in the cluster.</span></span>

<span data-ttu-id="2b2f4-848">A .NET-keretrendszer 3.5 hozzáadandó két módja van:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-848">There are two ways to add the .NET Framework 3.5:</span></span>

- <span data-ttu-id="2b2f4-849">A szerepkörök hozzáadása és szolgáltatások varázsló használata a Windows ahogy az ábra 39.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-849">Use the Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![39. ábra: A .NET-keretrendszer 3.5 telepítése a szerepkörök hozzáadása és szolgáltatások varázsló segítségével][sap-ha-guide-figure-3028]

  <span data-ttu-id="2b2f4-851">_**39. ábra:** a .NET-keretrendszer 3.5 telepítése a szerepkörök hozzáadása és szolgáltatások varázsló_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-851">_**Figure 39:** Install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

  ![40. ábra: Telepítés folyamatjelző, amikor telepíti a .NET-keretrendszer 3.5-szerepkörök hozzáadása és szolgáltatások varázsló segítségével][sap-ha-guide-figure-3029]

  <span data-ttu-id="2b2f4-853">_**40. ábra:** telepítés folyamatjelző, amikor telepíti a .NET-keretrendszer 3.5-szerepkörök hozzáadása és szolgáltatások varázsló segítségével_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-853">_**Figure 40:** Installation progress bar when you install the .NET Framework 3.5 by using the Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="2b2f4-854">A parancssori eszköz a dism.exe használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-854">Use the command-line tool dism.exe.</span></span> <span data-ttu-id="2b2f4-855">Az ilyen típusú telepítést kell a Windows-telepítési adathordozón lévő SxS könyvtár eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-855">For this type of installation, you need to access the SxS directory on the Windows installation media.</span></span> <span data-ttu-id="2b2f4-856">Egy rendszergazda jogú parancssort írja be:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-856">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="2b2f4-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="2b2f4-858">A fürt minden csomópontja SIOS DataKeeper Cluster Edition telepítését.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-858">Install SIOS DataKeeper Cluster Edition on each node in the cluster.</span></span> <span data-ttu-id="2b2f4-859">Hozzon létre virtuális megosztott tárolási SIOS DataKeeper hozzon létre egy szinkronizált tükrözött, és ezután szimulálni a fürt megosztott tárhelye.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-859">To create virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="2b2f4-860">A SIOS szoftver telepítése előtt hozza létre a tartományi felhasználót **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-860">Before you install the SIOS software, create the domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-861">Adja hozzá a **DataKeeperSvc** felhasználót, hogy a **helyi rendszergazdai** csoport mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-861">Add the **DataKeeperSvc** user to the **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="2b2f4-862">SIOS DataKeeper telepítése:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-862">To install SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="2b2f4-863">Telepíti a SIOS mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-863">Install the SIOS software on both cluster nodes.</span></span>

  ![SIOS telepítő][sap-ha-guide-figure-3030]

  ![. Ábra 41: A SIOS DataKeeper telepítés első oldalán][sap-ha-guide-figure-3031]

  <span data-ttu-id="2b2f4-866">_**41. ábra:** SIOS DataKeeper telepítésének első oldalára_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-866">_**Figure 41:** First page of the SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="2b2f4-867">A megjelenő ábra 42 párbeszédpanelen válassza ki **Igen**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-867">In the dialog box shown in Figure 42, select **Yes**.</span></span>

  ![42. ábra: DataKeeper arról tájékoztatja, hogy a szolgáltatás le lesz tiltva][sap-ha-guide-figure-3032]

  <span data-ttu-id="2b2f4-869">_**42. ábra:** DataKeeper arról tájékoztatja, hogy a szolgáltatás le lesz tiltva_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-869">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="2b2f4-870">A párbeszédpanelen megjelenő ábra 43, azt javasoljuk, hogy kiválassza **tartomány vagy a kiszolgáló fiók**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-870">In the dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![43. ábra: A SIOS DataKeeper felhasználó kiválasztása][sap-ha-guide-figure-3033]

  <span data-ttu-id="2b2f4-872">_**43. ábra:** SIOS DataKeeper a felhasználó kiválasztása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-872">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="2b2f4-873">Adja meg a tartományi fiók felhasználói nevét és a jelszavak SIOS DataKeeper létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-873">Enter the domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![44. ábra: Adja meg a tartományi felhasználónevet és jelszót a SIOS DataKeeper telepítés][sap-ha-guide-figure-3034]

  <span data-ttu-id="2b2f4-875">_**44. ábra:** adja meg a tartományi felhasználónevet és jelszót a SIOS DataKeeper telepítés_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-875">_**Figure 44:** Enter the domain user name and password for the SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="2b2f4-876">Telepítse a SIOS DataKeeper példány licenckulcs, ahogy az ábra 45.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-876">Install the license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![45. ábra: Adja meg a SIOS DataKeeper licenckulcs][sap-ha-guide-figure-3035]

  <span data-ttu-id="2b2f4-878">_**45. ábra:** adja meg a SIOS DataKeeper licenckulcs_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-878">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="2b2f4-879">Amikor a rendszer kéri, indítsa újra a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-879">When prompted, restart the virtual machine.</span></span>

#### <span data-ttu-id="2b2f4-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper beállítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="2b2f4-881">Miután mindkét csomópont SIOS DataKeeper telepítette, akkor indítsa el a konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-881">After you install SIOS DataKeeper on both nodes, you need to start the configuration.</span></span> <span data-ttu-id="2b2f4-882">A konfigurációs célja van csatolva a virtuális gépek mindegyikének további virtuális merevlemezeknek között szinkron replikálása.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-882">The goal of the configuration is to have synchronous data replication between the additional VHDs attached to each of the virtual machines.</span></span>

1.  <span data-ttu-id="2b2f4-883">Indítsa el a DataKeeper kezelési és konfigurációs eszközt, és válassza **Connect kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-883">Start the DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="2b2f4-884">(Az ábrán 46, ez a beállítás van körben piros.)</span><span class="sxs-lookup"><span data-stu-id="2b2f4-884">(In Figure 46, this option is circled in red.)</span></span>

  ![46. ábra: SIOS DataKeeper kezelési és konfigurációs eszköz][sap-ha-guide-figure-3036]

  <span data-ttu-id="2b2f4-886">_**46. ábra:** SIOS DataKeeper felügyelete és konfigurálása eszköz_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-886">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="2b2f4-887">Adja meg a nevét vagy IP-címét az első csomópontot a felügyeleti és konfigurációs eszköz számára, és egy második lépésben, a második csomópont kapcsolódnia kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-887">Enter the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and, in a second step, the second node.</span></span>

  ![47. ábra: Helyezze be a nevet, vagy a TCP/IP-cím, az első csomópontot a felügyeleti és a konfigurációs eszközt kell csatlakoztatja, és egy második lépésben, a második csomópont][sap-ha-guide-figure-3037]

  <span data-ttu-id="2b2f4-889">_**47. ábra:** helyezze be a nevet vagy az első csomópontot a felügyeleti és a konfigurációs eszközt kell csatlakoztatja, és egy második lépésben, a második csomópont TCP/IP-címe_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-889">_**Figure 47:** Insert the name or TCP/IP address of the first node the Management and Configuration tool should connect to, and in a second step, the second node_</span></span>

3.  <span data-ttu-id="2b2f4-890">Hozzon létre a replikációs feladatot, a két csomópont között.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-890">Create the replication job between the two nodes.</span></span>

  ![48. ábra: A replikáció feladat létrehozása][sap-ha-guide-figure-3038]

  <span data-ttu-id="2b2f4-892">_**48. ábra:** replikációs feladat létrehozása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-892">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="2b2f4-893">A varázsló végigvezeti egy fájlreplikálási feladat létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-893">A wizard guides you through the process of creating a replication job.</span></span>
4.  <span data-ttu-id="2b2f4-894">Adja meg a nevét, az TCP/IP-cím és a a forráscsomópont lemezkötetet.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-894">Define the name, TCP/IP address, and disk volume of the source node.</span></span>

  ![49. ábra: Adja meg a nevét, a replikációs feladat][sap-ha-guide-figure-3039]

  <span data-ttu-id="2b2f4-896">_**49. ábra:** adja meg a nevét, a replikációs feladat_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-896">_**Figure 49:** Define the name of the replication job_</span></span>

  ![50. ábra: A csomópont, amely az aktuális adatforrás-csomópontnak kell lennie az alap adatok meghatározásához][sap-ha-guide-figure-3040]

  <span data-ttu-id="2b2f4-898">_**50. ábra:** a csomóponthoz, amely az aktuális adatforrás-csomópontnak kell lennie az alap adatok meghatározásához_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-898">_**Figure 50:** Define the base data for the node, which should be the current source node_</span></span>

5.  <span data-ttu-id="2b2f4-899">Adja meg a nevét, az TCP/IP-cím és a célcsomóponton lemezkötetének.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-899">Define the name, TCP/IP address, and disk volume of the target node.</span></span>

  ![51. ábra: A csomópont, hogy az aktuális célcsomóponttal alkalmazandó alap adatok meghatározásához][sap-ha-guide-figure-3041]

  <span data-ttu-id="2b2f4-901">_**51. ábra:** határozza meg a kell lennie az aktuális célcsomóponttal csomópont adatait_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-901">_**Figure 51:** Define the base data for the node, which should be the current target node_</span></span>

6.  <span data-ttu-id="2b2f4-902">A tömörítési algoritmust határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-902">Define the compression algorithms.</span></span> <span data-ttu-id="2b2f4-903">Ebben a példában azt javasoljuk, hogy a tömörítés a replikálási adatfolyamból.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-903">In our example, we recommend that you compress the replication stream.</span></span> <span data-ttu-id="2b2f4-904">Különösen abban az esetben az újraszinkronizálás a tömörítés a replikációs adatfolyam jelentősen csökkenti az újraszinkronizálás idő.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-904">Especially in resynchronization situations, the compression of the replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="2b2f4-905">Vegye figyelembe, hogy a tömörítés a Processzor és memória szempontjából erőforrásokat egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-905">Note that compression uses the CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="2b2f4-906">A tömörítési arány növekszik, ezért nem használt CPU-erőforrások mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-906">As the compression rate increases, so does the volume of CPU resources used.</span></span> <span data-ttu-id="2b2f4-907">Is módosíthatja ezt a beállítást később.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-907">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="2b2f4-908">Egy másik beállítást kell ellenőrizni, hogy a replikáció aszinkron vagy szinkron módon történik-e.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-908">Another setting you need to check is whether the replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="2b2f4-909">*Ha a SAP ASC/SCS konfigurációk, használnia kell a szinkron replikáció*.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-909">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![52. ábra: A replikáció adatainak megadása][sap-ha-guide-figure-3042]

  <span data-ttu-id="2b2f4-911">_**52. ábra:** replikációs adatainak megadása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-911">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="2b2f4-912">Határozza meg, hogy a kötet a replikációs feladat által replikált kell magát egy Windows Server feladatátvételi fürtszolgáltatási fürtkonfiguráció, mint egy megosztott lemez számára.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-912">Define whether the volume that is replicated by the replication job should be represented to a Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="2b2f4-913">Az SAP ASC/SCS konfigurációs kiválasztása **Igen** , hogy a Windows fürtszolgáltatás látja a replikált kötet egy megosztott lemezt, amely egy fürt kötetként használhat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-913">For the SAP ASCS/SCS configuration, select **Yes** so that the Windows cluster sees the replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![53. ábra: Válassza az Igen, a replikált kötet beállítása a fürt kötetként][sap-ha-guide-figure-3043]

  <span data-ttu-id="2b2f4-915">_**53. ábra:** válasszon **Igen** a replikált kötet beállítása a fürt kötetként_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-915">_**Figure 53:** Select **Yes** to set the replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="2b2f4-916">A kötet létrehozása után a DataKeeper felügyelete és konfigurálása eszköz mutatja, hogy a replikációs feladat aktív.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-916">After the volume is created, the DataKeeper Management and Configuration tool shows that the replication job is active.</span></span>

  ![Ábra 54: DataKeeper szinkron tükrözés az SAP ASC/SCS megosztás lemez jelenleg aktív][sap-ha-guide-figure-3044]

  <span data-ttu-id="2b2f4-918">_**Ábra 54:** DataKeeper szinkron tükrözés az SAP ASC/SCS a lemez megosztása a jelenleg aktív_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-918">_**Figure 54:** DataKeeper synchronous mirroring for the SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="2b2f4-919">Feladatátvevőfürt-kezelő most már a lemez DataKeeper lemezként jeleníti meg, ahogy az ábra 55.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-919">Failover Cluster Manager now shows the disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![55. ábra: A Feladatátvevőfürt-kezelő a lemez DataKeeper replikált jeleníti meg][sap-ha-guide-figure-3045]

  <span data-ttu-id="2b2f4-921">_**Ábra 55:** Feladatátvevőfürt-kezelő jeleníti meg a lemezt, a replikált DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-921">_**Figure 55:** Failover Cluster Manager shows the disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="2b2f4-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Az SAP NetWeaver rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install the SAP NetWeaver system</span></span>

<span data-ttu-id="2b2f4-923">Az adatbázis-kezelő a telepítő azt nem ismertetik, mert beállítások eltérők lehetnek, attól függően, hogy az adatbázis-kezelő rendszert használ.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-923">We won’t describe the DBMS setup because setups vary depending on the DBMS system you use.</span></span> <span data-ttu-id="2b2f4-924">Azonban feltételezzük, hogy az adatbázis-kezelő magas rendelkezésre állású kérdéseket a különböző DBMS forgalmazójával támogatja az Azure-funkciókkal rendelkező tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-924">However, we assume that high-availability concerns with the DBMS are addressed with the functionalities the different DBMS vendors support for Azure.</span></span> <span data-ttu-id="2b2f4-925">Például mindig bekapcsolva vagy adatbázis-tükrözést az SQL Server és Oracle Data Guard az Oracle-adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-925">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="2b2f4-926">A forgatókönyvben a cikkben használjuk, azt nem további védelem bekapcsolása a az adatbázis-kezelő.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-926">In the scenario we use in this article, we didn't add more protection to the DBMS.</span></span>

<span data-ttu-id="2b2f4-927">Nincsenek semmilyen külön odafigyelést különböző adatbázis-kezelő szolgáltatásokat fürtözött SAP ASC/SCS konfigurálása az Azure-ban az ilyen kommunikál.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-927">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-928">A telepítési eljárásokat az SAP NetWeaver ABAP rendszerek, Java, és a ABAP + Java rendszerek csaknem azonosak.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-928">The installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="2b2f4-929">A legfontosabb különbség, hogy rendelkezik-e az SAP ABAP rendszer ASC egy példánya.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-929">The most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="2b2f4-930">Az SAP Java rendszer egy SCS példány van.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-930">The SAP Java system has one SCS instance.</span></span> <span data-ttu-id="2b2f4-931">Az SAP ABAP + Java rendszer rendelkezik egy ASC példány és egy SCS példány fut a Microsoft feladatátvevő fürt csoporton belül.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-931">The SAP ABAP+Java system has one ASCS instance and one SCS instance running in the same Microsoft failover cluster group.</span></span> <span data-ttu-id="2b2f4-932">Összes telepítési különbséget minden SAP NetWeaver telepítési verem explicit módon szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-932">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="2b2f4-933">Feltételezzük, hogy minden egyéb részeinek megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-933">You can assume that all other parts are the same.</span></span>  
>
>

### <span data-ttu-id="2b2f4-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Telepítse a SAP, ha a magas rendelkezésre állású ASC/SCS példánya</span><span class="sxs-lookup"><span data-stu-id="2b2f4-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b2f4-935">Győződjön meg arról, hogy nem helyezhető el a lapozófájl DataKeeper tükrözött köteteken.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-935">Be sure not to place your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="2b2f4-936">DataKeeper nem támogatja a tükrözött kötetek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-936">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="2b2f4-937">A lapozófájl az ideiglenes meghajtón D egy Azure virtuális gép, az alapértelmezett hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-937">You can leave your page file on the temporary drive D of an Azure virtual machine, which is the default.</span></span> <span data-ttu-id="2b2f4-938">Ha még nincs hiba, a Windows lapozófájlja áthelyezése az Azure virtuális gép D meghajtó.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-938">If it's not already there, move the Windows page file to drive D of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="2b2f4-939">Ezeket a feladatokat, ha a magas rendelkezésre állású ASC/SCS példánya SAP telepítése foglal magában:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-939">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="2b2f4-940">Egy virtuális nevet az SAP ASC/SCS fürtözött példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-940">Creating a virtual host name for the clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="2b2f4-941">Az SAP első fürtcsomópontra telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-941">Installing the SAP first cluster node</span></span>
- <span data-ttu-id="2b2f4-942">Az SAP-profilnak ASC/SCS-példány módosítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-942">Modifying the SAP profile of the ASCS/SCS instance</span></span>
- <span data-ttu-id="2b2f4-943">A mintavétel a port hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-943">Adding a probe port</span></span>
- <span data-ttu-id="2b2f4-944">A Windows tűzfal mintavételi port megnyitása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-944">Opening the Windows firewall probe port</span></span>

#### <span data-ttu-id="2b2f4-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Hozzon létre egy virtuális nevet az SAP ASC/SCS fürtözött példány</span><span class="sxs-lookup"><span data-stu-id="2b2f4-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for the clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="2b2f4-946">A Windows DNS-kezelőben hozzon létre egy DNS-bejegyzést a ASC/SCS példányának virtuális állomás neve.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-946">In the Windows DNS manager, create a DNS entry for the virtual host name of the ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="2b2f4-947">Az IP-cím, amikor hozzárendeli a ASC/SCS példányának virtuális állomás neve lehet ugyanaz, mint az Azure Load Balancer rendelt IP-cím (**<*SID*> - lb - ASC **).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-947">The IP address that you assign to the virtual host name of the ASCS/SCS instance must be the same as the IP address that you assigned to Azure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="2b2f4-948">Az SAP ASC/SCS állomásnév virtuális IP-címe (**pr1-ASC-sap**) ugyanaz, mint az IP-cím az Azure Load Balancer (**pr1-lb-ASC**).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-948">The IP address of the virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is the same as the IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![56. ábra: A DNS-bejegyzést a SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím megadása][sap-ha-guide-figure-3046]

  <span data-ttu-id="2b2f4-950">_**Ábra 56:** a DNS-bejegyzést a SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím megadása_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-950">_**Figure 56:** Define the DNS entry for the SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="2b2f4-951">A virtuális állomásneve rendelt IP-cím megadásához válassza ki a **DNS-kezelő** > **tartomány**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-951">To define the IP address assigned to the virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![57. ábra: Új virtuális nevét és TCP/IP-cím SAP ASC/SCS fürtnek megfelelő konfiguráció][sap-ha-guide-figure-3047]

  <span data-ttu-id="2b2f4-953">_**57. ábra:** új virtuális nevét és a TCP/IP-címtartományok SAP ASC/SCS fürtnek megfelelő konfiguráció_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-953">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="2b2f4-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Az SAP első fürtcsomópontra telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install the SAP first cluster node</span></span>

1.  <span data-ttu-id="2b2f4-955">Az első fürt csomópont lehetőség fürtcsomóponton A. végrehajtása Ha például a **pr1-ASC-0** állomás.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-955">Execute the first cluster node option on cluster node A. For example, on the **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="2b2f4-956">Az alapértelmezett portok az Azure belső terheléselosztóhoz, jelölje be:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-956">To keep the default ports for the Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="2b2f4-957">**ABAP rendszer**: **ASC** szám példány **00**</span><span class="sxs-lookup"><span data-stu-id="2b2f4-957">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="2b2f4-958">**Java-rendszer**: **SCS** szám példány **01**</span><span class="sxs-lookup"><span data-stu-id="2b2f4-958">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="2b2f4-959">**ABAP + Java rendszer**: **ASC** szám példány **00** és **SCS** szám példány **01**</span><span class="sxs-lookup"><span data-stu-id="2b2f4-959">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="2b2f4-960">A ABAP ASC példányhoz, és a Java SCS példány 01 példány 00 eltérő számú portok használatához először módosítani szeretné az Azure belső alapértelmezett terheléselosztási szabályok, leírt [ASC/SCS alapértelmezett terheléselosztási szabályok módosítása az Azure belső terheléselosztó][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-960">To use instance numbers other than 00 for the ABAP ASCS instance and 01 for the Java SCS instance, first you need to change the Azure internal load balancer default load balancing rules, described in [Change the ASCS/SCS default load balancing rules for the Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="2b2f4-961">A következő néhány feladatot a szabványos SAP-dokumentáció nem ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-961">The next few tasks aren't described in the standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-962">Az SAP-dokumentáció ASC/SCS fürt első csomópontjára telepítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-962">The SAP installation documentation describes how to install the first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="2b2f4-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Az SAP-profil ASC/SCS-példány módosítása</span><span class="sxs-lookup"><span data-stu-id="2b2f4-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify the SAP profile of the ASCS/SCS instance</span></span>

<span data-ttu-id="2b2f4-964">Akkor hozzon létre egy új profil paraméter.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-964">You need to add a new profile parameter.</span></span> <span data-ttu-id="2b2f4-965">A profil paraméter megakadályozza, hogy az SAP-munkafolyamatok és a sorba helyezni kiszolgáló közötti kapcsolat bezárása, ha túl sokáig üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-965">The profile parameter prevents connections between SAP work processes and the enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="2b2f4-966">Jelenleg a probléma esetén említett [beállításjegyzék-bejegyzések hozzáadása az SAP ASC/SCS példány mindkét fürtcsomóponton][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="2b2f4-966">We mentioned the problem scenario in [Add registry entries on both cluster nodes of the SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="2b2f4-967">A szakaszt azt is vezette be két módosítások néhány alapvető TCP/IP-kapcsolat paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-967">In that section, we also introduced two changes to some basic TCP/IP connection parameters.</span></span> <span data-ttu-id="2b2f4-968">Egy második lépésben be kell állítani a sorba helyezni kiszolgáló küld egy `keep_alive` jelezze, hogy a kapcsolatok nem elérte az Azure belső elosztott terhelésű üresjárati küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-968">In a second step, you need to set the enqueue server to send a `keep_alive` signal so that the connections don't hit the Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="2b2f4-969">Az SAP-profil ASC/SCS-példány módosítása:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-969">To modify the SAP profile of the ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="2b2f4-970">Ez a profil paraméter hozzáadása az SAP ASC/SCS példány profilhoz:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-970">Add this profile parameter to the SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="2b2f4-971">A fenti példában az elérési út:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-971">In our example, the path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="2b2f4-972">Például, hogy a SAP SCS példány profil és a megfelelő elérési út:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-972">For example, to the SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="2b2f4-973">A módosítások alkalmazásához, indítsa újra az SAP ASC /SCS példányt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-973">To apply the changes, restart the SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="2b2f4-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Adjon hozzá egy mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="2b2f4-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="2b2f4-975">A belső terheléselosztó mintavételi funkció segítségével a teljes fürt konfigurációs Azure terheléselosztó dolgozni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-975">Use the internal load balancer's probe functionality to make the entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="2b2f4-976">Az Azure belső terheléselosztó általában a bejövő terhelés részt vevő virtuális gépek között egyenlően osztja el.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-976">The Azure internal load balancer usually distributes the incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="2b2f4-977">Azonban ez nem fog működni az egyes fürtkonfigurációk mert csak egy példány aktív.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-977">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="2b2f4-978">A többi példány passzív, és a munkaterhelés nem tudja elfogadni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-978">The other instance is passive and can’t accept any of the workload.</span></span> <span data-ttu-id="2b2f4-979">A mintavételi funkció segít, amikor az Azure belső terheléselosztó munkahelyi csak egy aktív példány számára.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-979">A probe functionality helps when the Azure internal load balancer assigns work only to an active instance.</span></span> <span data-ttu-id="2b2f4-980">A mintavétel-szolgáltatásával a belső terheléselosztó előfordulások aktív, és ezután célpéldányának csak a munkaterheléssel képes észlelni.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-980">With the probe functionality, the internal load balancer can detect which instances are active, and then target only the instance with the workload.</span></span>

<span data-ttu-id="2b2f4-981">A mintavételi portot hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="2b2f4-981">To add a probe port:</span></span>

1.  <span data-ttu-id="2b2f4-982">Ellenőrizze az aktuális **ProbePort** beállítása a következő PowerShell-parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-982">Check the current **ProbePort** setting by running the following PowerShell command.</span></span> <span data-ttu-id="2b2f4-983">A fürtkonfiguráció le a virtuális gépek egyik végrehajtást.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-983">Execute it from within one of the virtual machines in the cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="2b2f4-984">A mintavételi portot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-984">Define a probe port.</span></span> <span data-ttu-id="2b2f4-985">Az alapértelmezett mintavételi portszám **0**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-985">The default probe port number is **0**.</span></span> <span data-ttu-id="2b2f4-986">A jelen példában használjuk a mintavételi portot **62000**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-986">In our example, we use probe port **62000**.</span></span>

  ![58. ábra: A fürt konfigurációs mintavételi portot pedig 0 alapértelmezés szerint][sap-ha-guide-figure-3048]

  <span data-ttu-id="2b2f4-988">_**58. ábra:** fürt konfigurációs mintavételi törölve: 0_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-988">_**Figure 58:** The default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="2b2f4-989">A portszám SAP Azure Resource Manager-sablonok van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-989">The port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="2b2f4-990">A port számát a PowerShellben rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-990">You can assign the port number in PowerShell.</span></span>

  <span data-ttu-id="2b2f4-991">Egy új ProbePort értéket az a  **SAP <*SID*> IP ** fürterőforrás, futtassa a következő PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-991">To set a new ProbePort value for the **SAP <*SID*> IP** cluster resource, run the following PowerShell script.</span></span> <span data-ttu-id="2b2f4-992">A környezet PowerShell változói frissítése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-992">Update the PowerShell variables for your environment.</span></span> <span data-ttu-id="2b2f4-993">A parancsfájl futtatása után a rendszer kérni fogja újraindítja a SAP fürtcsoport a változások életbe lépjenek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-993">After the script runs, you'll be prompted to restart the SAP cluster group to activate the changes.</span></span>

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  Clear-Host
  $SAPClusterRoleName = "SAP $SAPSID"
  $SAPIPresourceName = "SAP $SAPSID IP"
  $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
  $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
  $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
  $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
  $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
  $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
  $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

  $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

  Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

  Write-Host
  Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

  if($ActivateChanges -eq "yes"){
  Write-Host
  Write-Host "Activating changes..." -ForegroundColor Cyan

  Write-Host
  write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
  Stop-ClusterResource -Name $SAPIPresourceName
  sleep 5

  Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
  Start-ClusterGroup -Name $SAPClusterRoleName

  Write-Host "New ProbePort parameter is active." -ForegroundColor Green
  Write-Host

  Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
  Write-Host
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
  }else
  {
  Write-Host "Changes are not activated."
  }
  ```

  <span data-ttu-id="2b2f4-994">Miután kapcsolása a  **SAP <*SID*> ** fürtön a szerepkör hálózatra, ellenőrizze, hogy **ProbePort** az új értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-994">After you bring the **SAP <*SID*>** cluster role online, verify that **ProbePort** is set to the new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![59. ábra: Miután beállította az új érték mintavétel-a fürt port][sap-ha-guide-figure-3049]

  <span data-ttu-id="2b2f4-996">_**59. ábra:** az új érték beállítása után, mintavételi modulja a fürt port_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-996">_**Figure 59:** Probe the cluster port after you set the new value_</span></span>

#### <span data-ttu-id="2b2f4-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Nyissa meg a Windows tűzfal mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="2b2f4-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open the Windows firewall probe port</span></span>

<span data-ttu-id="2b2f4-998">Nyissa meg a Windows tűzfal mintavételi portot mindkét fürtcsomóponton kell.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-998">You need to open a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="2b2f4-999">A következő parancsfájl segítségével nyissa meg a Windows tűzfal mintavételi portot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-999">Use the following script to open a Windows firewall probe port.</span></span> <span data-ttu-id="2b2f4-1000">A környezet PowerShell változói frissítése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1000">Update the PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="2b2f4-1001">A **ProbePort** értéke **62000**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1001">The **ProbePort** is set to **62000**.</span></span> <span data-ttu-id="2b2f4-1002">Most már hozzáférhet a fájlmegosztáshoz  **\\\ascsha-clsap\sapmnt** a többi, például mert a **ascsha-dbas**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1002">Now you can access the file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="2b2f4-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Az adatbázis-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install the database instance</span></span>

<span data-ttu-id="2b2f4-1004">Az adatbázispéldány fölött telepítéséhez kövesse a SAP dokumentáció ismertetett folyamatot.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1004">To install the database instance, follow the process described in the SAP installation documentation.</span></span>

### <span data-ttu-id="2b2f4-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>A második fürt csomópont telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install the second cluster node</span></span>

<span data-ttu-id="2b2f4-1006">A második fürt telepítéséhez kövesse az SAP telepítési útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1006">To install the second cluster, follow the steps in the SAP installation guide.</span></span>

### <span data-ttu-id="2b2f4-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Módosítsa a SAP SSZON Windows szolgáltatáspéldány indítási típusa</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change the start type of the SAP ERS Windows service instance</span></span>

<span data-ttu-id="2b2f4-1008">A SAP SSZON Windows szolgáltatás indítási típusának módosítása **automatikus (Késleltetett indítás)** mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1008">Change the start type of the SAP ERS Windows service to **Automatic (Delayed Start)** on both cluster nodes.</span></span>

![60. ábra: Az SAP SSZON példány szolgáltatás típusának módosítása késleltetett automatikusra][sap-ha-guide-figure-3050]

<span data-ttu-id="2b2f4-1010">_**60. ábra:** az SAP SSZON példányt késleltetett automatikus módosíthatja a szolgáltatás típusa_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1010">_**Figure 60:** Change the service type for the SAP ERS instance to delayed automatic_</span></span>

### <span data-ttu-id="2b2f4-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Az SAP elsődleges alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install the SAP Primary Application Server</span></span>

<span data-ttu-id="2b2f4-1012">Az elsődleges alkalmazás kiszolgáló (szolgáltatói)-példány telepítése <*SID*> - di - 0-fiókjához kijelölt üzemeltetéséhez a szolgáltatói CÍMEK a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1012">Install the Primary Application Server (PAS) instance <*SID*>-di-0 on the virtual machine that you've designated to host the PAS.</span></span> <span data-ttu-id="2b2f4-1013">Nincsenek függőségek a Azure vagy DataKeeper-specifikus beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1013">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="2b2f4-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Az SAP további alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install the SAP Additional Application Server</span></span>

<span data-ttu-id="2b2f4-1015">Telepítse az SAP további Application Server (AAS) üzemeltetésére SAP Application Server-példány már a kijelölt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1015">Install an SAP Additional Application Server (AAS) on all the virtual machines that you've designated to host an SAP Application Server instance.</span></span> <span data-ttu-id="2b2f4-1016">Például a <*SID*> - di - 1 a <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1016">For example, on <*SID*>-di-1 to <*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="2b2f4-1017">Ez befejezi a magas rendelkezésre állású SAP NetWeaver rendszer telepítését.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1017">This finishes the installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="2b2f4-1018">A következő folytatásához feladatátvételi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1018">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="2b2f4-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Az SAP ASC/SCS példány feladatátvétel és SIOS replikációs tesztelése</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test the SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="2b2f4-1020">Akkor is könnyen a tesztelése egy SAP ASC/SCS-példány feladatátvevő és SIOS lemez replikációs Feladatátvevőfürt-kezelő és a SIOS DataKeeper kezelési és konfigurációs eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1020">It's easy to test and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and the SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="2b2f4-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>A fürtcsomóponton SAP ASC/SCS-példány fut.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="2b2f4-1022">A **SAP PR1** fürtcsoport fürtcsomóponton A. fut. Például a **pr1-ASC-0**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1022">The **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="2b2f4-1023">Rendelje hozzá a megosztott lemezmeghajtó S, amely része a a **SAP PR1** fürtcsoportot, és az ASC/SCS-példány használja, a fürt csomópont azonosítójához.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1023">Assign the shared disk drive S, which is part of the **SAP PR1** cluster group, and which the ASCS/SCS instance uses, to cluster node A.</span></span>

![61. ábra: Feladatátvevőfürt-kezelő: < SID > a SAP fürtcsoport A csomóponton fut.][sap-ha-guide-figure-5000]

<span data-ttu-id="2b2f4-1025">_**61. ábra:** Feladatátvevőfürt-kezelő: az SAP <*SID*> fürtcsoport A csomóponton fut._</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1025">_**Figure 61:** Failover Cluster Manager: The SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="2b2f4-1026">A SIOS DataKeeper felügyelete és konfigurálása eszköz megtekintheti, hogy a megosztott lemez adatainak rendszer szinkron módon replikálja a csomóponton A forrás-kötet meghajtó S a kötet célmeghajtó S fürtcsomópontra a b kiszolgálóra. Például, hogy a rendszer replikálja a **pr1-ASC-0 [10.0.0.40]** való **pr1-ASC-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1026">In the SIOS DataKeeper Management and Configuration tool, you can see that the shared disk data is synchronously replicated from the source volume drive S on cluster node A to the target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** to **pr1-ascs-1 [10.0.0.41]**.</span></span>

![62. ábra: SIOS DataKeeper replikálja a helyi kötet fürtcsomópontról A B-fürt csomópontjának][sap-ha-guide-figure-5001]

<span data-ttu-id="2b2f4-1028">_**62. ábra:** SIOS DataKeeper replikálja a helyi kötet fürtcsomópontról A B-fürt csomópontjának_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1028">_**Figure 62:** In SIOS DataKeeper, replicate the local volume from cluster node A to cluster node B_</span></span>

### <span data-ttu-id="2b2f4-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>B csomópont-csomópont A feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A to node B</span></span>

1.  <span data-ttu-id="2b2f4-1030">Válasszon egyet ezek közül a SAP a feladatátvétel kezdeményezése <*SID*> fürtcsoport fürtcsomópontról A b-fürt csomópontjának</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1030">Choose one of these options to initiate a failover of the SAP <*SID*> cluster group from cluster node A to cluster node B:</span></span>
  - <span data-ttu-id="2b2f4-1031">Feladatátvevőfürt-kezelővel</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1031">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="2b2f4-1032">Feladatátvevő fürt PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1032">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="2b2f4-1033">Indítsa újra a fürt csomópont A a Windows vendég operációs rendszerben (a SAP, az automatikus feladatátvételt kezdeményez <*SID*> fürtcsoport csomópontból A csomópont B).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1033">Restart cluster node A within the Windows guest operating system (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
3.  <span data-ttu-id="2b2f4-1034">Indítsa újra a fürt csomópont A Azure-portálról (egy, a SAP automatikus feladatátvételt kezdeményez <*SID*> fürtcsoport csomópontból A csomópont B).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1034">Restart cluster node A from the Azure portal (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>  
4.  <span data-ttu-id="2b2f4-1035">Indítsa újra a fürt csomópont A Azure PowerShell használatával (ez elindít egy automatikus feladatátvétel, a SAP <*SID*> fürtcsoport csomópontból A csomópont B).</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1035">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of the SAP <*SID*> cluster group from node A to node B).</span></span>

  <span data-ttu-id="2b2f4-1036">Feladatátvétel után a SAP <*SID*> fürtcsoport fürtcsomóponton b fut. Futó például **pr1-ASC-1**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1036">After failover, the SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![63. ábra: A Feladatátvevőfürt-kezelő, a SAP < SID > fürtcsoport van fürtcsomóponton futó B][sap-ha-guide-figure-5002]

  <span data-ttu-id="2b2f4-1038">_**Ábra 63**: A Feladatátvevőfürt-kezelő, a SAP <*SID*> fürtcsoport fürtcsomóponton B fut._</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1038">_**Figure 63**: In Failover Cluster Manager, the SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="2b2f4-1039">A megosztott lemez már csatlakoztatva van a fürt csomópont b SIOS DataKeeper replikálódik adatok forrás kötet meghajtóról S fürtcsomóponton B célmeghajtó kötet, S fürtcsomóponton A. A replikáló például **pr1-ASC-1 [10.0.0.41]** való **pr1-ASC-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1039">The shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B to target volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** to **pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Ábra 64: SIOS DataKeeper replikálja a helyi kötet fürtcsomópontból B csomópont A fürt][sap-ha-guide-figure-5003]

  <span data-ttu-id="2b2f4-1041">_**Ábra 64:** SIOS DataKeeper replikálja a helyi kötet fürtcsomópontból B csomópont A fürt_</span><span class="sxs-lookup"><span data-stu-id="2b2f4-1041">_**Figure 64:** SIOS DataKeeper replicates the local volume from cluster node B to cluster node A_</span></span>
