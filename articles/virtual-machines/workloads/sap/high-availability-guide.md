---
title: "Virtuális gépek magas rendelkezésre állás a SAP NetWeaver aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 927409830065573248a43427eb382448ca07b6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a><span data-ttu-id="ce898-103">Magas rendelkezésre állás a SAP NetWeaver Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="ce898-103">High availability for SAP NetWeaver on Azure VMs</span></span>

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


<span data-ttu-id="ce898-109">Az Azure virtuális gépek egy hello megoldás olyan szervezeteknek, amelyek szükség van a számítási, tárolási és hálózati erőforrásokat, minimális időben, valamint hosszú beszerzési ciklusok nélkül.</span><span class="sxs-lookup"><span data-stu-id="ce898-109">Azure Virtual Machines is hello solution for organizations that need compute, storage, and network resources, in minimal time, and without lengthy procurement cycles.</span></span> <span data-ttu-id="ce898-110">Azure virtuális gépek toodeploy klasszikus alkalmazások, például SAP NetWeaver alapú ABAP, Java és egy ABAP + Java verem használható.</span><span class="sxs-lookup"><span data-stu-id="ce898-110">You can use Azure Virtual Machines toodeploy classic applications like SAP NetWeaver-based ABAP, Java, and an ABAP+Java stack.</span></span> <span data-ttu-id="ce898-111">Megbízhatóság és rendelkezésre állás további helyszíni erőforrások nélkül kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="ce898-111">Extend reliability and availability without additional on-premises resources.</span></span> <span data-ttu-id="ce898-112">Az Azure virtuális gépek közötti kapcsolatot nyújthassanak, támogatja, így a Azure Virtual Machines integrálása a szervezete helyszíni tartományok, magánfelhőket és SAP rendszer fekvő.</span><span class="sxs-lookup"><span data-stu-id="ce898-112">Azure Virtual Machines supports cross-premises connectivity, so you can integrate Azure Virtual Machines into your organization's on-premises domains, private clouds, and SAP system landscape.</span></span>

<span data-ttu-id="ce898-113">Ez a cikk azt lépésének hello, hogy elvégezhető toodeploy magas rendelkezésre állású SAP rendszerek az Azure-ban hello Azure Resource Manager telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="ce898-113">In this article, we cover hello steps that you can take toodeploy high-availability SAP systems in Azure by using hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="ce898-114">A Microsoft végigvezetik Önt a nagyobb feladatok:</span><span class="sxs-lookup"><span data-stu-id="ce898-114">We walk you through these major tasks:</span></span>

* <span data-ttu-id="ce898-115">Megkeresi a hello hello felsorolt megfelelő SAP megjegyzések és a telepítési útmutatók [erőforrások] [ sap-ha-guide-2] szakasz.</span><span class="sxs-lookup"><span data-stu-id="ce898-115">Find hello right SAP Notes and installation guides, listed in hello [Resources][sap-ha-guide-2] section.</span></span> <span data-ttu-id="ce898-116">Ez a cikk kiegészíti az SAP-dokumentáció és az SAP megjegyzések, amelyek hello elsődleges erőforrásokat, amelyek segítségével telepítse, és az adott platformon SAP szoftver központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="ce898-116">This article complements SAP installation documentation and SAP Notes, which are hello primary resources that can help you install and deploy SAP software on specific platforms.</span></span>
* <span data-ttu-id="ce898-117">Ismerje meg, hogy hello különbségei hello Azure Resource Manager üzembe helyezési modellben és hello Azure klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="ce898-117">Learn hello differences between hello Azure Resource Manager deployment model and hello Azure classic deployment model.</span></span>
* <span data-ttu-id="ce898-118">További információk a Windows Server feladatátvételi fürtszolgáltatási kvórummódok, kiválaszthatja az Azure-telepítés számára megfelelő hello modellt.</span><span class="sxs-lookup"><span data-stu-id="ce898-118">Learn about Windows Server Failover Clustering quorum modes, so you can select hello model that is right for your Azure deployment.</span></span>
* <span data-ttu-id="ce898-119">További információk a Windows Server feladatátvételi fürtszolgáltatási megosztott tár az Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ce898-119">Learn about Windows Server Failover Clustering shared storage in Azure services.</span></span>
* <span data-ttu-id="ce898-120">Ismerje meg, hogyan toohelp megvédeni egyetlen ponton-az-hibát jelentő összetevők például a fejlett üzleti alkalmazások fejlesztői (ABAP) SAP központi szolgáltatások (ASC) / SAP központi szolgáltatások (SCS) és az adatbázis-kezelő rendszerek (DBMS) és a redundáns összetevők, például SAP Alkalmazáskiszolgáló, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce898-120">Learn how toohelp protect single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server, in Azure.</span></span>
* <span data-ttu-id="ce898-121">Kövesse a részletes példa egy telepítési és a konfiguráció egy magas rendelkezésre állású SAP rendszer Windows Server feladatátvételi fürtszolgáltatási fürtben az Azure-ban Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="ce898-121">Follow a step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure by using Azure Resource Manager.</span></span>
* <span data-ttu-id="ce898-122">További tudnivalók a toouse szükséges további lépéseket a Windows Server feladatátvételi fürtszolgáltatási az Azure-ban, de amelyek esetén nem szükséges egy helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="ce898-122">Learn about additional steps required toouse Windows Server Failover Clustering in Azure, but which are not needed in an on-premises deployment.</span></span>

<span data-ttu-id="ce898-123">toosimplify üzembe helyezését és konfigurálását, ebben a cikkben használjuk hello SAP háromrétegű magas rendelkezésre állású Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="ce898-123">toosimplify deployment and configuration, in this article, we use hello SAP three-tier high-availability Resource Manager templates.</span></span> <span data-ttu-id="ce898-124">hello sablonok automatizálni hello teljes infrastruktúra, amelyekre szüksége van a magas rendelkezésre állású SAP-rendszer telepítését.</span><span class="sxs-lookup"><span data-stu-id="ce898-124">hello templates automate deployment of hello entire infrastructure that you need for a high-availability SAP system.</span></span> <span data-ttu-id="ce898-125">hello infrastruktúra SAP alkalmazás teljesítmény szabványos (SAP) méretezése a SAP rendszert is támogatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-125">hello infrastructure also supports SAP Application Performance Standard (SAPS) sizing of your SAP system.</span></span>

## <span data-ttu-id="ce898-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ce898-126"><a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a> Prerequisites</span></span>
<span data-ttu-id="ce898-127">Mielőtt elkezdené, győződjön meg arról, hogy teljesülnek-e a következő részekben hello ismertetett hello előfeltételek.</span><span class="sxs-lookup"><span data-stu-id="ce898-127">Before you start, make sure that you meet hello prerequisites that are described in hello following sections.</span></span> <span data-ttu-id="ce898-128">Is a hello felsorolt összes erőforrást meg arról, hogy toocheck [erőforrások] [ sap-ha-guide-2] szakasz.</span><span class="sxs-lookup"><span data-stu-id="ce898-128">Also, be sure toocheck all resources listed in hello [Resources][sap-ha-guide-2] section.</span></span>

<span data-ttu-id="ce898-129">Ebben a cikkben az Azure Resource Manager sablonok használjuk [háromrétegű SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span><span class="sxs-lookup"><span data-stu-id="ce898-129">In this article, we use Azure Resource Manager templates for [three-tier SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/).</span></span> <span data-ttu-id="ce898-130">Sablonok hasznos áttekintéséért lásd: [SAP Azure Resource Manager-sablonok](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span><span class="sxs-lookup"><span data-stu-id="ce898-130">For a helpful overview of templates, see [SAP Azure Resource Manager templates](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).</span></span>

## <span data-ttu-id="ce898-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Erőforrások</span><span class="sxs-lookup"><span data-stu-id="ce898-131"><a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a> Resources</span></span>
<span data-ttu-id="ce898-132">Ezek a cikkek SAP üzemelő példányok Azure terjed ki:</span><span class="sxs-lookup"><span data-stu-id="ce898-132">These articles cover SAP deployments in Azure:</span></span>

* <span data-ttu-id="ce898-133">[Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="ce898-133">[Azure Virtual Machines planning and implementation for SAP NetWeaver][planning-guide]</span></span>
* <span data-ttu-id="ce898-134">[SAP NetWeaver Azure virtuális gépek központi telepítését][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="ce898-134">[Azure Virtual Machines deployment for SAP NetWeaver][deployment-guide]</span></span>
* <span data-ttu-id="ce898-135">[Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="ce898-135">[Azure Virtual Machines DBMS deployment for SAP NetWeaver][dbms-guide]</span></span>
* <span data-ttu-id="ce898-136">[Az Azure virtuális gépek magas rendelkezésre állás a SAP NetWeaver (Ez az útmutató)][sap-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="ce898-136">[Azure Virtual Machines high availability for SAP NetWeaver (this guide)][sap-ha-guide]</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-137">Amikor csak lehetséges, amelyben tudatjuk a felhasználókkal, egy hivatkozás toohello utaló SAP telepítési útmutató (lásd: hello [SAP telepítési útmutatók][sap-installation-guides]).</span><span class="sxs-lookup"><span data-stu-id="ce898-137">Whenever possible, we give you a link toohello referring SAP installation guide (see hello [SAP installation guides][sap-installation-guides]).</span></span> <span data-ttu-id="ce898-138">Az Előfeltételek és hello telepítésének folyamatát, az a jó ötlet tooread hello SAP NetWeaver telepítési útmutató gondosan kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="ce898-138">For prerequisites and information about hello installation process, it's a good idea tooread hello SAP NetWeaver installation guides carefully.</span></span> <span data-ttu-id="ce898-139">Ez a cikk ismerteti a SAP NetWeaver-alapú rendszereken használható az Azure virtuális gépek csak meghatározott feladatok.</span><span class="sxs-lookup"><span data-stu-id="ce898-139">This article covers only specific tasks for SAP NetWeaver-based systems that you can use with Azure Virtual Machines.</span></span>
>
>

<span data-ttu-id="ce898-140">Ezek SAP azonban kapcsolódó toohello a témakör az SAP, az Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="ce898-140">These SAP Notes are related toohello topic of SAP in Azure:</span></span>

| <span data-ttu-id="ce898-141">Megjegyzés száma</span><span class="sxs-lookup"><span data-stu-id="ce898-141">Note number</span></span> | <span data-ttu-id="ce898-142">Cím</span><span class="sxs-lookup"><span data-stu-id="ce898-142">Title</span></span> |
| --- | --- |
| <span data-ttu-id="ce898-143">[1928533]</span><span class="sxs-lookup"><span data-stu-id="ce898-143">[1928533]</span></span> |<span data-ttu-id="ce898-144">Az Azure-on SAP-alkalmazásokból: támogatott termékek és méretezése</span><span class="sxs-lookup"><span data-stu-id="ce898-144">SAP Applications on Azure: Supported Products and Sizing</span></span> |
| <span data-ttu-id="ce898-145">[2015553]</span><span class="sxs-lookup"><span data-stu-id="ce898-145">[2015553]</span></span> |<span data-ttu-id="ce898-146">A Microsoft Azure SAP: Előfeltételek támogatja</span><span class="sxs-lookup"><span data-stu-id="ce898-146">SAP on Microsoft Azure: Support Prerequisites</span></span> |
| <span data-ttu-id="ce898-147">[1999351]</span><span class="sxs-lookup"><span data-stu-id="ce898-147">[1999351]</span></span> |<span data-ttu-id="ce898-148">SAP Azure figyelésének továbbfejlesztett</span><span class="sxs-lookup"><span data-stu-id="ce898-148">Enhanced Azure Monitoring for SAP</span></span> |
| <span data-ttu-id="ce898-149">[2178632]</span><span class="sxs-lookup"><span data-stu-id="ce898-149">[2178632]</span></span> |<span data-ttu-id="ce898-150">Kulcs figyelési metrikákat az SAP, a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="ce898-150">Key Monitoring Metrics for SAP on Microsoft Azure</span></span> |
| <span data-ttu-id="ce898-151">[1999351]</span><span class="sxs-lookup"><span data-stu-id="ce898-151">[1999351]</span></span> |<span data-ttu-id="ce898-152">A Windows virtualizálási: fokozott figyelése</span><span class="sxs-lookup"><span data-stu-id="ce898-152">Virtualization on Windows: Enhanced Monitoring</span></span> |
| <span data-ttu-id="ce898-153">[2243692]</span><span class="sxs-lookup"><span data-stu-id="ce898-153">[2243692]</span></span> |<span data-ttu-id="ce898-154">A prémium szintű Azure SSD-tárolón SAP DBMS példány használata</span><span class="sxs-lookup"><span data-stu-id="ce898-154">Use of Azure Premium SSD Storage for SAP DBMS Instance</span></span> |

<span data-ttu-id="ce898-155">További tudnivalók hello [korlátozások az Azure-előfizetések][azure-subscription-service-limits-subscription], többek között az általános alapértelmezett korlátozások és a maximális korlátozások.</span><span class="sxs-lookup"><span data-stu-id="ce898-155">Learn more about hello [limitations of Azure subscriptions][azure-subscription-service-limits-subscription], including general default limitations and maximum limitations.</span></span>

## <span data-ttu-id="ce898-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Magas rendelkezésre állású SAP az Azure Resource Manager és a hello Azure klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="ce898-156"><a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>High-availability SAP with Azure Resource Manager vs. hello Azure classic deployment model</span></span>
<span data-ttu-id="ce898-157">eltérő módon a következő területeken hello jelennek meg hello Azure Resource Manager és az Azure klasszikus üzembe helyezési modellek:</span><span class="sxs-lookup"><span data-stu-id="ce898-157">hello Azure Resource Manager and Azure classic deployment models are different in hello following areas:</span></span>

- <span data-ttu-id="ce898-158">Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="ce898-158">Resource groups</span></span>
- <span data-ttu-id="ce898-159">Azure belső terheléselosztó függőség hello Azure erőforráscsoport betöltése</span><span class="sxs-lookup"><span data-stu-id="ce898-159">Azure internal load balancer dependency on hello Azure resource group</span></span>
- <span data-ttu-id="ce898-160">SAP multi-SID forgatókönyvek támogatása</span><span class="sxs-lookup"><span data-stu-id="ce898-160">Support for SAP multi-SID scenarios</span></span>

### <span data-ttu-id="ce898-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Erőforráscsoportok</span><span class="sxs-lookup"><span data-stu-id="ce898-161"><a name="f76af273-1993-4d83-b12d-65deeae23686"></a> Resource groups</span></span>
<span data-ttu-id="ce898-162">Az Azure Resource Manager segítségével is erőforrás csoportok toomanage hello összes alkalmazás-erőforrásokat az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ce898-162">In Azure Resource Manager, you can use resource groups toomanage all hello application resources in your Azure subscription.</span></span> <span data-ttu-id="ce898-163">Integrált megközelítést, egy erőforráscsoportban található összes erőforrást rendelkezik hello azonos életciklusát.</span><span class="sxs-lookup"><span data-stu-id="ce898-163">An integrated approach, in a resource group, all resources have hello same life cycle.</span></span> <span data-ttu-id="ce898-164">Például minden erőforrások jönnek létre: hello azonos idő és azok törlődnek hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ce898-164">For example, all resources are created at hello same time and they are deleted at hello same time.</span></span> <span data-ttu-id="ce898-165">További információk az [erőforráscsoportokról](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="ce898-165">Learn more about [resource groups](../../../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>

### <span data-ttu-id="ce898-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure belső terheléselosztó függőség hello Azure erőforráscsoport betöltése</span><span class="sxs-lookup"><span data-stu-id="ce898-166"><a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a> Azure internal load balancer dependency on hello Azure resource group</span></span>

<span data-ttu-id="ce898-167">Hello Azure klasszikus üzembe helyezési modellel hello Azure belső terheléselosztó (Azure terheléselosztó szolgáltatás) és hello felhőalapú szolgáltatás csoportja közötti függőség van.</span><span class="sxs-lookup"><span data-stu-id="ce898-167">In hello Azure classic deployment model, there is a dependency between hello Azure internal load balancer (Azure Load Balancer service) and hello cloud service group.</span></span> <span data-ttu-id="ce898-168">Minden belső terheléselosztónak egy felhőalapú szolgáltatás csoportot kell.</span><span class="sxs-lookup"><span data-stu-id="ce898-168">Every internal load balancer needs one cloud service group.</span></span>

<span data-ttu-id="ce898-169">Az Azure erőforrás-kezelőben, nem kell egy Azure-erőforrás csoport toouse Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="ce898-169">In Azure Resource Manager, you don't need an Azure resource group toouse Azure Load Balancer.</span></span> <span data-ttu-id="ce898-170">egyszerűbb és rugalmasabb hello környezete.</span><span class="sxs-lookup"><span data-stu-id="ce898-170">hello environment is simpler and more flexible.</span></span>

### <a name="support-for-sap-multi-sid-scenarios"></a><span data-ttu-id="ce898-171">SAP multi-SID forgatókönyvek támogatása</span><span class="sxs-lookup"><span data-stu-id="ce898-171">Support for SAP multi-SID scenarios</span></span>

<span data-ttu-id="ce898-172">Az Azure erőforrás-kezelőben, akkor több példányt is telepíthet SAP rendszer azonosítója (SID) ASC/SCS egy fürt.</span><span class="sxs-lookup"><span data-stu-id="ce898-172">In Azure Resource Manager, you can install multiple SAP system identifier (SID) ASCS/SCS instances in one cluster.</span></span> <span data-ttu-id="ce898-173">Minden Azure belső terheléselosztóhoz több IP-cím támogatásának köszönhetően multi-SID-példányok is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="ce898-173">Multi-SID instances are possible because of support for multiple IP addresses for each Azure internal load balancer.</span></span>

<span data-ttu-id="ce898-174">toouse hello Azure klasszikus üzembe helyezési modellel, kövesse az ismertetett eljárások hello [SAP NetWeaver az Azure-ban: az Azure-ban SIOS DataKeeper segítségével a Windows Server feladatátvételi fürtszolgáltatási SAP ASC/SCS fürtszolgáltatási példányok](http://go.microsoft.com/fwlink/?LinkId=613056).</span><span class="sxs-lookup"><span data-stu-id="ce898-174">toouse hello Azure classic deployment model, follow hello procedures described in [SAP NetWeaver in Azure: Clustering SAP ASCS/SCS instances by using Windows Server Failover Clustering in Azure with SIOS DataKeeper](http://go.microsoft.com/fwlink/?LinkId=613056).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ce898-175">Javasoljuk, hogy a SAP telepítések használjon hello Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="ce898-175">We strongly recommend that you use hello Azure Resource Manager deployment model for your SAP installations.</span></span> <span data-ttu-id="ce898-176">Nem érhetők el hello klasszikus üzembe helyezési modellel számos előnyt kínál.</span><span class="sxs-lookup"><span data-stu-id="ce898-176">It offers many benefits that are not available in hello classic deployment model.</span></span> <span data-ttu-id="ce898-177">További tudnivalók az Azure [üzembe helyezési modellel][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span><span class="sxs-lookup"><span data-stu-id="ce898-177">Learn more about Azure [deployment models][virtual-machines-azure-resource-manager-architecture-benefits-arm].</span></span>   
>
>

## <span data-ttu-id="ce898-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server feladatátvételi fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ce898-178"><a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a> Windows Server Failover Clustering</span></span>
<span data-ttu-id="ce898-179">Az egy magas rendelkezésre állású SAP ASC/SCS telepítése és az adatbázis-kezelő, a Windows hello foundation Windows Server feladatátvételi fürtszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ce898-179">Windows Server Failover Clustering is hello foundation of a high-availability SAP ASCS/SCS installation and DBMS in Windows.</span></span>

<span data-ttu-id="ce898-180">A feladatátvevő fürt egy 1 + n különálló kiszolgálók csoportja (csomópontok), amelyek együttműködése az alkalmazások és szolgáltatások tooincrease hello rendelkezésre állási.</span><span class="sxs-lookup"><span data-stu-id="ce898-180">A failover cluster is a group of 1+n independent servers (nodes) that work together tooincrease hello availability of applications and services.</span></span> <span data-ttu-id="ce898-181">Csomópont hiba lép fel, ha a Windows Server feladatátvételi fürtszolgáltatási kiszámítja-e hibák merülnek fel a megfelelő megőrzésével hello számát a fürt tooprovide alkalmazásokhoz és szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ce898-181">If a node failure occurs, Windows Server Failover Clustering calculates hello number of failures that can occur while maintaining a healthy cluster tooprovide applications and services.</span></span> <span data-ttu-id="ce898-182">Választhat másik kvórum módok tooachieve Feladatátvételi fürtszolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="ce898-182">You can choose from different quorum modes tooachieve failover clustering.</span></span>

### <span data-ttu-id="ce898-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Kvórummódok</span><span class="sxs-lookup"><span data-stu-id="ce898-183"><a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a> Quorum modes</span></span>
<span data-ttu-id="ce898-184">Windows Server feladatátvételi fürtszolgáltatási használatakor négy kvórummódok közül választhat:</span><span class="sxs-lookup"><span data-stu-id="ce898-184">You can choose from four quorum modes when you use Windows Server Failover Clustering:</span></span>

* <span data-ttu-id="ce898-185">**Csomóponttöbbség**.</span><span class="sxs-lookup"><span data-stu-id="ce898-185">**Node Majority**.</span></span> <span data-ttu-id="ce898-186">Hello fürt mindegyik csomópontján szavazati is.</span><span class="sxs-lookup"><span data-stu-id="ce898-186">Each node of hello cluster can vote.</span></span> <span data-ttu-id="ce898-187">hello Ez azt jelenti, hogy a fürt működését csak a szavazatot, többsége a több mint fele hello szavazatot.</span><span class="sxs-lookup"><span data-stu-id="ce898-187">hello cluster functions only with a majority of votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="ce898-188">Azt javasoljuk, hogy a csomópontok páratlan számú fürtöknél ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="ce898-188">We recommend this option for clusters that have an uneven number of nodes.</span></span> <span data-ttu-id="ce898-189">Például egy hét csomópontos fürtben három csomópont sikertelen lehet, és hello fürt stills többsége éri el, és továbbra is toorun.</span><span class="sxs-lookup"><span data-stu-id="ce898-189">For example, three nodes in a seven-node cluster can fail, and hello cluster stills achieves a majority and continues toorun.</span></span>  
* <span data-ttu-id="ce898-190">**Csomópont- és lemeztöbbség**.</span><span class="sxs-lookup"><span data-stu-id="ce898-190">**Node and Disk Majority**.</span></span> <span data-ttu-id="ce898-191">Minden csomópont- és a kijelölt (a tanúsító lemez) hello fürttároló is szavazásra, mikor érhetők el, és a kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="ce898-191">Each node and a designated disk (a disk witness) in hello cluster storage can vote when they are available and in communication.</span></span> <span data-ttu-id="ce898-192">hello fürt működését csak hello többsége a szavazatot, ez azt jelenti, hogy a több mint fele hello szavazatot.</span><span class="sxs-lookup"><span data-stu-id="ce898-192">hello cluster functions only with a majority of hello votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="ce898-193">Ez a mód környezetben fürt páros számú csomópont teljesen logikus.</span><span class="sxs-lookup"><span data-stu-id="ce898-193">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="ce898-194">Ha a csomópontok fele hello és hello lemez online állapotban, hello fürt kifogástalan állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="ce898-194">If half hello nodes and hello disk are online, hello cluster remains in a healthy state.</span></span>
* <span data-ttu-id="ce898-195">**Csomópont- és fájlmegosztás többsége**.</span><span class="sxs-lookup"><span data-stu-id="ce898-195">**Node and File Share Majority**.</span></span> <span data-ttu-id="ce898-196">Minden csomópont plusz a kijelölt fájlmegosztást (a tanúsító fájlmegosztás), amelyet a rendszergazda hello hoz létre szavazásra, függetlenül hello csomópontok és a fájlmegosztás elérhetők-e, és a kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="ce898-196">Each node plus a designated file share (a file share witness) that hello administrator creates can vote, regardless of whether hello nodes and file share are available and in communication.</span></span> <span data-ttu-id="ce898-197">hello fürt működését csak hello többsége a szavazatot, ez azt jelenti, hogy a több mint fele hello szavazatot.</span><span class="sxs-lookup"><span data-stu-id="ce898-197">hello cluster functions only with a majority of hello votes, that is, with more than half hello votes.</span></span> <span data-ttu-id="ce898-198">Ez a mód környezetben fürt páros számú csomópont teljesen logikus.</span><span class="sxs-lookup"><span data-stu-id="ce898-198">This mode makes sense in a cluster environment with an even number of nodes.</span></span> <span data-ttu-id="ce898-199">Hasonló toohello csomópont- és lemeztöbbség módban, de a tanúsító lemez helyett egy tanúsító fájlmegosztást használja.</span><span class="sxs-lookup"><span data-stu-id="ce898-199">It's similar toohello Node and Disk Majority mode, but it uses a witness file share instead of a witness disk.</span></span> <span data-ttu-id="ce898-200">Ebben a módban egyszerűen tooimplement, de ha hello fájlmegosztás maga nem magas rendelkezésre állású, azt válnak a hibaérzékeny pontok kialakulását.</span><span class="sxs-lookup"><span data-stu-id="ce898-200">This mode is easy tooimplement, but if hello file share itself is not highly available, it might become a single point of failure.</span></span>
* <span data-ttu-id="ce898-201">**Nincs többség: Csak lemez**.</span><span class="sxs-lookup"><span data-stu-id="ce898-201">**No Majority: Disk Only**.</span></span> <span data-ttu-id="ce898-202">hello fürt rendelkezik egy kvórum, ha egy csomópont elérhető, és hello fürttároló egy meghatározott lemezével kommunikál.</span><span class="sxs-lookup"><span data-stu-id="ce898-202">hello cluster has a quorum if one node is available and in communication with a specific disk in hello cluster storage.</span></span> <span data-ttu-id="ce898-203">Csak hello csomópontok is, hogy a lemez kommunikál hello fürt csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="ce898-203">Only hello nodes that also are in communication with that disk can join hello cluster.</span></span> <span data-ttu-id="ce898-204">Azt javasoljuk, hogy nem használja ezt a módot.</span><span class="sxs-lookup"><span data-stu-id="ce898-204">We recommend that you do not use this mode.</span></span>
 

## <span data-ttu-id="ce898-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server feladatátvételi fürtszolgáltatás a helyszíni</span><span class="sxs-lookup"><span data-stu-id="ce898-205"><a name="fdfee875-6e66-483a-a343-14bbaee33275"></a> Windows Server Failover Clustering on-premises</span></span>
<span data-ttu-id="ce898-206">1. ábra mutatja a két csomópontból álló fürtben.</span><span class="sxs-lookup"><span data-stu-id="ce898-206">Figure 1 shows a cluster of two nodes.</span></span> <span data-ttu-id="ce898-207">Ha hello hello csomópontok közötti hálózati kapcsolat hibája esetén, és mindkét csomópont mentése marad, és fut, a kvórum lemez vagy fájlmegosztás meghatározza, hogy melyik csomópont továbbra is tooprovide hello fürt alkalmazásokhoz és szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ce898-207">If hello network connection between hello nodes fails and both nodes stay up and running, a quorum disk or file share determines which node will continue tooprovide hello cluster's applications and services.</span></span> <span data-ttu-id="ce898-208">hello csomópont, amelynek hozzáférési toohello kvórumlemez vagy fájlmegosztási hello csomópont, amely biztosítja, hogy a szolgáltatások továbbra is.</span><span class="sxs-lookup"><span data-stu-id="ce898-208">hello node that has access toohello quorum disk or file share is hello node that ensures that services continue.</span></span>

<span data-ttu-id="ce898-209">Ez a példa egy két csomópontot tartalmazó fürtben, mert hello csomópont- és fájlmegosztástöbbség kvórummód használjuk.</span><span class="sxs-lookup"><span data-stu-id="ce898-209">Because this example uses a two-node cluster, we use hello Node and File Share Majority quorum mode.</span></span> <span data-ttu-id="ce898-210">hello csomópont- és lemeztöbbség is beállítás érvényes.</span><span class="sxs-lookup"><span data-stu-id="ce898-210">hello Node and Disk Majority also is a valid option.</span></span> <span data-ttu-id="ce898-211">Éles környezetben azt javasoljuk, hogy egy kvórumlemez használjon.</span><span class="sxs-lookup"><span data-stu-id="ce898-211">In a production environment, we recommend that you use a quorum disk.</span></span> <span data-ttu-id="ce898-212">Hálózati és tárolási rendszer technológia toomake használhatja azt magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="ce898-212">You can use network and storage system technology toomake it highly available.</span></span>

![1. ábra: Példa egy Windows Server feladatátvételi fürtszolgáltatás konfigurációs az SAP ASC/SCS az Azure-ban][sap-ha-guide-figure-1000]

<span data-ttu-id="ce898-214">_**1. ábra:** egy Windows Server feladatátvételi fürtszolgáltatás konfigurációs az SAP ASC/SCS az Azure-ban – példa_</span><span class="sxs-lookup"><span data-stu-id="ce898-214">_**Figure 1:** Example of a Windows Server Failover Clustering configuration for SAP ASCS/SCS in Azure_</span></span>

### <span data-ttu-id="ce898-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Megosztott tároló</span><span class="sxs-lookup"><span data-stu-id="ce898-215"><a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a> Shared storage</span></span>
<span data-ttu-id="ce898-216">1. ábra is mutatja egy két csomópontos megosztott tárolófürt.</span><span class="sxs-lookup"><span data-stu-id="ce898-216">Figure 1 also shows a two-node shared storage cluster.</span></span> <span data-ttu-id="ce898-217">Egy helyi megosztott tárolási fürt hello fürt összes csomópontjának észleli a megosztott tároló.</span><span class="sxs-lookup"><span data-stu-id="ce898-217">In an on-premises shared storage cluster, all nodes in hello cluster detect shared storage.</span></span> <span data-ttu-id="ce898-218">Zárolási mechanizmus hello adatok sérülés elleni védelmét.</span><span class="sxs-lookup"><span data-stu-id="ce898-218">A locking mechanism protects hello data from corruption.</span></span> <span data-ttu-id="ce898-219">Minden csomópont észleli, ha egy másik csomópont meghibásodik.</span><span class="sxs-lookup"><span data-stu-id="ce898-219">All nodes can detect if another node fails.</span></span> <span data-ttu-id="ce898-220">Ha egy csomópont meghibásodik, hello megmaradó csomópontra megkapja a tulajdonjogot hello tárolási erőforrások és szolgáltatások hello rendelkezésre állását biztosítja.</span><span class="sxs-lookup"><span data-stu-id="ce898-220">If one node fails, hello remaining node takes ownership of hello storage resources and ensures hello availability of services.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-221">Például a magas rendelkezésre állás az egyes adatbázis-kezelő alkalmazások, egy megosztott lemez nem kell az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ce898-221">You don't need shared disks for high availability with some DBMS applications, like with SQL Server.</span></span> <span data-ttu-id="ce898-222">SQL Server Always On fájlokat replikálja a DBMS adatainak és naplókönyvtárainak hello helyi lemez egy fürt csomópont toohello helyi lemez egy másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="ce898-222">SQL Server Always On replicates DBMS data and log files from hello local disk of one cluster node toohello local disk of another cluster node.</span></span> <span data-ttu-id="ce898-223">Hello Windows fürtkonfiguráció ebben az esetben egy megosztott lemez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ce898-223">In that case, hello Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="ce898-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Hálózati és a névfeloldás</span><span class="sxs-lookup"><span data-stu-id="ce898-224"><a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a> Networking and name resolution</span></span>
<span data-ttu-id="ce898-225">Ügyfélszámítógépek elérni hello fürt egy virtuális IP-címet és egy virtuális nevet adott hello DNS-kiszolgáló biztosítja.</span><span class="sxs-lookup"><span data-stu-id="ce898-225">Client computers reach hello cluster over a virtual IP address and a virtual host name that hello DNS server provides.</span></span> <span data-ttu-id="ce898-226">hello helyszíni csomópontok és hello DNS-kiszolgáló kezelni tud a több IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ce898-226">hello on-premises nodes and hello DNS server can handle multiple IP addresses.</span></span>

<span data-ttu-id="ce898-227">A hagyományos telepítés, a két vagy több hálózati kapcsolatot használja:</span><span class="sxs-lookup"><span data-stu-id="ce898-227">In a typical setup, you use two or more network connections:</span></span>

* <span data-ttu-id="ce898-228">Egy dedikált kapcsolat toohello tároló</span><span class="sxs-lookup"><span data-stu-id="ce898-228">A dedicated connection toohello storage</span></span>
* <span data-ttu-id="ce898-229">Fürt belső hálózati kapcsolatot a hello szívverés</span><span class="sxs-lookup"><span data-stu-id="ce898-229">A cluster-internal network connection for hello heartbeat</span></span>
* <span data-ttu-id="ce898-230">Egy nyilvános hálózat, hogy az ügyfelek tooconnect toohello fürt használatára</span><span class="sxs-lookup"><span data-stu-id="ce898-230">A public network that clients use tooconnect toohello cluster</span></span>

## <span data-ttu-id="ce898-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server feladatátvételi fürtszolgáltatás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ce898-231"><a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a> Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="ce898-232">Képest toobare nélküli vagy saját felhőben történő alkalmazáshoz, Azure virtuális gépek Windows Server feladatátvételi fürtszolgáltatás további lépéseket tooconfigure igényel.</span><span class="sxs-lookup"><span data-stu-id="ce898-232">Compared toobare metal or private cloud deployments, Azure Virtual Machines requires additional steps tooconfigure Windows Server Failover Clustering.</span></span> <span data-ttu-id="ce898-233">Egy megosztott fürtlemez összeállításakor szüksége több IP-címek és virtuális állomás neve tooset hello SAP ASC/SCS példány.</span><span class="sxs-lookup"><span data-stu-id="ce898-233">When you build a shared cluster disk, you need tooset several IP addresses and virtual host names for hello SAP ASCS/SCS instance.</span></span>

<span data-ttu-id="ce898-234">Ebben a cikkben azt ismertetik az alapfogalmakat és hello további lépéseket szükséges toobuild egy SAP központi szolgáltatások magas rendelkezésre állású fürthöz az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce898-234">In this article, we discuss key concepts and hello additional steps required toobuild an SAP high-availability central services cluster in Azure.</span></span> <span data-ttu-id="ce898-235">Megmutatjuk, hogyan tooset hello külső eszköz SIOS DataKeeper fel, és hogyan tooconfigure hello Azure belső terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="ce898-235">We show you how tooset up hello third-party tool SIOS DataKeeper, and how tooconfigure hello Azure internal load balancer.</span></span> <span data-ttu-id="ce898-236">Ezen eszközök toocreate Windows feladatátvevő fürt egy tanúsító fájlmegosztást az Azure-ban használható.</span><span class="sxs-lookup"><span data-stu-id="ce898-236">You can use these tools toocreate a Windows failover cluster with a file share witness in Azure.</span></span>

![2. ábra: A Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban egy megosztott lemez nélkül][sap-ha-guide-figure-1001]

<span data-ttu-id="ce898-238">_**2. ábra:** Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban egy megosztott lemez nélkül_</span><span class="sxs-lookup"><span data-stu-id="ce898-238">_**Figure 2:** Windows Server Failover Clustering configuration in Azure without a shared disk_</span></span>

### <span data-ttu-id="ce898-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>SIOS DataKeeper megosztott lemezt az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ce898-239"><a name="1a464091-922b-48d7-9d08-7cecf757f341"></a> Shared disk in Azure with SIOS DataKeeper</span></span>
<span data-ttu-id="ce898-240">A magas rendelkezésre állású SAP ASC/SCS példányához megosztott tárolót kell fürtbe.</span><span class="sxs-lookup"><span data-stu-id="ce898-240">You need cluster shared storage for a high-availability SAP ASCS/SCS instance.</span></span> <span data-ttu-id="ce898-241">2016 szeptemberétől Azure nem munkaajánlat megosztott tároló, amely szerint a megosztott tárolófürt toocreate is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-241">As of September 2016, Azure doesn't offer shared storage that you can use toocreate a shared storage cluster.</span></span> <span data-ttu-id="ce898-242">Harmadik féltől származó szoftverek SIOS DataKeeper Cluster Edition toocreate tükrözött tárolási funkciókat biztosító, amely a fürt megosztott tárolási szimulálja is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-242">You can use third-party software SIOS DataKeeper Cluster Edition toocreate a mirrored storage that simulates cluster shared storage.</span></span> <span data-ttu-id="ce898-243">hello SIOS megoldást biztosít a valós idejű szinkron replikálása.</span><span class="sxs-lookup"><span data-stu-id="ce898-243">hello SIOS solution provides real-time synchronous data replication.</span></span> <span data-ttu-id="ce898-244">Ez az, hogy hogyan hozhat létre a fürt egy megosztott lemez erőforrás:</span><span class="sxs-lookup"><span data-stu-id="ce898-244">This is how you can create a shared disk resource for a cluster:</span></span>

1. <span data-ttu-id="ce898-245">Rendeljen egy további Azure virtuális merevlemez (VHD) tooeach hello virtuális gépek (VM) a Windows fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ce898-245">Attach an additional Azure virtual hard disk (VHD) tooeach of hello virtual machines (VMs) in a Windows cluster configuration.</span></span>
2. <span data-ttu-id="ce898-246">Futtassa a SIOS DataKeeper Cluster Edition mindkét virtuális gép csomóponton.</span><span class="sxs-lookup"><span data-stu-id="ce898-246">Run SIOS DataKeeper Cluster Edition on both virtual machine nodes.</span></span>
3. <span data-ttu-id="ce898-247">Konfigurálása SIOS DataKeeper Cluster Edition, így azt tükrözi hello tartalom hello további csatolt virtuális merevlemez kötetének kötetről hello forrás virtuális gép toohello további csatolt virtuális merevlemez hello cél virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ce898-247">Configure SIOS DataKeeper Cluster Edition so that it mirrors hello content of hello additional VHD attached volume from hello source virtual machine toohello additional VHD attached volume of hello target virtual machine.</span></span> <span data-ttu-id="ce898-248">SIOS DataKeeper kivonatolja hello forrás- és helyi köteteken, és adja őket az tooWindows Server feladatátvételi fürtszolgáltatási egy megosztott lemezként jeleníti.</span><span class="sxs-lookup"><span data-stu-id="ce898-248">SIOS DataKeeper abstracts hello source and target local volumes, and then presents them tooWindows Server Failover Clustering as one shared disk.</span></span>

<span data-ttu-id="ce898-249">További információk [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span><span class="sxs-lookup"><span data-stu-id="ce898-249">Get more information about [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).</span></span>

![3. ábra: A Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban SIOS DataKeeper][sap-ha-guide-figure-1002]

<span data-ttu-id="ce898-251">_**3. ábra:** Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban SIOS DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="ce898-251">_**Figure 3:** Windows Server Failover Clustering configuration in Azure with SIOS DataKeeper_</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-252">Nincs szükség a megosztott lemez néhány DBMS termékekkel, például az SQL Server magas rendelkezésre állásra.</span><span class="sxs-lookup"><span data-stu-id="ce898-252">You don't need shared disks for high availability with some DBMS products, like SQL Server.</span></span> <span data-ttu-id="ce898-253">SQL Server Always On fájlokat replikálja a DBMS adatainak és naplókönyvtárainak hello helyi lemez egy fürt csomópont toohello helyi lemez egy másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="ce898-253">SQL Server Always On replicates DBMS data and log files from hello local disk of one cluster node toohello local disk of another cluster node.</span></span> <span data-ttu-id="ce898-254">Hello Windows fürtkonfiguráció ebben az esetben egy megosztott lemez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ce898-254">In this case, hello Windows cluster configuration doesn't need a shared disk.</span></span>
>
>

### <span data-ttu-id="ce898-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>A névfeloldás az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ce898-255"><a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a> Name resolution in Azure</span></span>
<span data-ttu-id="ce898-256">hello Azure cloud platform hello beállítás tooconfigure virtuális IP-címek, például csak fix IP-címek nem kínál.</span><span class="sxs-lookup"><span data-stu-id="ce898-256">hello Azure cloud platform doesn't offer hello option tooconfigure virtual IP addresses, such as floating IP addresses.</span></span> <span data-ttu-id="ce898-257">Az alternatív megoldások tooset be egy virtuális IP-cím tooreach hello fürt erőforrás hello felhőben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-257">You need an alternative solution tooset up a virtual IP address tooreach hello cluster resource in hello cloud.</span></span>
<span data-ttu-id="ce898-258">Azure hello Azure terheléselosztó szolgáltatás egy belső terheléselosztóhoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="ce898-258">Azure has an internal load balancer in hello Azure Load Balancer service.</span></span> <span data-ttu-id="ce898-259">Hello belső terheléselosztóhoz, az ügyfelek elérni hello fürt keresztül hello virtuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ce898-259">With hello internal load balancer, clients reach hello cluster over hello cluster virtual IP address.</span></span>
<span data-ttu-id="ce898-260">Toodeploy hello belső terheléselosztó, amely tartalmazza a hello fürtcsomópontok hello erőforráscsoportban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-260">You need toodeploy hello internal load balancer in hello resource group that contains hello cluster nodes.</span></span> <span data-ttu-id="ce898-261">Ezt követően konfigurálja a szabályok továbbítási hello mintavételi hello belső terheléselosztó portok az összes szükséges portot.</span><span class="sxs-lookup"><span data-stu-id="ce898-261">Then, configure all necessary port forwarding rules with hello probe ports of hello internal load balancer.</span></span>
<span data-ttu-id="ce898-262">hello az ügyfélszámítógépek csatlakozhatnak keresztül hello virtuális állomás neve.</span><span class="sxs-lookup"><span data-stu-id="ce898-262">hello clients can connect via hello virtual host name.</span></span> <span data-ttu-id="ce898-263">hello DNS-kiszolgáló oldja fel a hello IP-címet, és hello belső terheléselosztó leírók port továbbítási toohello hello fürtjének aktív csomópontja.</span><span class="sxs-lookup"><span data-stu-id="ce898-263">hello DNS server resolves hello cluster IP address, and hello internal load balancer handles port forwarding toohello active node of hello cluster.</span></span>

## <span data-ttu-id="ce898-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver magas rendelkezésre állás érdekében az Azure infrastruktúra,--szolgáltatás (IaaS)</span><span class="sxs-lookup"><span data-stu-id="ce898-264"><a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a> SAP NetWeaver high availability in Azure Infrastructure-as-a-Service (IaaS)</span></span>
<span data-ttu-id="ce898-265">tooachieve alkalmazás magas rendelkezésre állású, SAP, például SAP szoftver összetevők, a következő összetevők tooprotect hello kell:</span><span class="sxs-lookup"><span data-stu-id="ce898-265">tooachieve SAP application high availability, such as for SAP software components, you need tooprotect hello following components:</span></span>

* <span data-ttu-id="ce898-266">SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="ce898-266">SAP Application Server instance</span></span>
* <span data-ttu-id="ce898-267">SAP ASC/SCS példány</span><span class="sxs-lookup"><span data-stu-id="ce898-267">SAP ASCS/SCS instance</span></span>
* <span data-ttu-id="ce898-268">Adatbázis-kezelő kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ce898-268">DBMS server</span></span>

<span data-ttu-id="ce898-269">Tudnivalók az SAP-összetevők a magas rendelkezésre állású forgatókönyvek védelméről további információkért lásd: [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver](planning-guide.md).</span><span class="sxs-lookup"><span data-stu-id="ce898-269">For more information about protecting SAP components in high-availability scenarios, see [Azure Virtual Machines planning and implementation for SAP NetWeaver](planning-guide.md).</span></span>

### <span data-ttu-id="ce898-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Magas rendelkezésre állású SAP-alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ce898-270"><a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a> High-availability SAP Application Server</span></span>
<span data-ttu-id="ce898-271">Általában nem szükséges egy adott magas rendelkezésre állású megoldás hello SAP-kiszolgáló és a párbeszédpanel-példányok.</span><span class="sxs-lookup"><span data-stu-id="ce898-271">You usually don't need a specific high-availability solution for hello SAP Application Server and dialog instances.</span></span> <span data-ttu-id="ce898-272">Magas rendelkezésre állású redundancia által érhetők el, és másik példányát az Azure virtuális gépek több párbeszédpanel példánya konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-272">You achieve high availability by redundancy, and you'll configure multiple dialog instances in different instances of Azure Virtual Machines.</span></span> <span data-ttu-id="ce898-273">Két példányban Azure virtuális gépek telepítése legalább két SAP alkalmazáspéldányok kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ce898-273">You should have at least two SAP application instances installed in two instances of Azure Virtual Machines.</span></span>

![4. ábra: Magas rendelkezésre állású SAP-alkalmazáskiszolgáló][sap-ha-guide-figure-2000]

<span data-ttu-id="ce898-275">_**4. ábra:** magas rendelkezésre állású SAP-alkalmazáskiszolgáló_</span><span class="sxs-lookup"><span data-stu-id="ce898-275">_**Figure 4:** High-availability SAP Application Server_</span></span>

<span data-ttu-id="ce898-276">El kell helyezni, hogy a gazdagép-SAP Application Server példánya hello azonos Azure rendelkezésre állási csoport összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ce898-276">You must place all virtual machines that host SAP Application Server instances in hello same Azure availability set.</span></span> <span data-ttu-id="ce898-277">Az Azure rendelkezésre állási csoportok biztosítja, hogy:</span><span class="sxs-lookup"><span data-stu-id="ce898-277">An Azure availability set ensures that:</span></span>

* <span data-ttu-id="ce898-278">Az összes virtuális gép hello részét képező azonos frissítési tartományban.</span><span class="sxs-lookup"><span data-stu-id="ce898-278">All virtual machines are part of hello same upgrade domain.</span></span> <span data-ttu-id="ce898-279">Frissítési tartományokhoz, például gondoskodik arról, hogy hello virtuális gépek nem frissített hello karbantartási tervezett leállások során ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ce898-279">An upgrade domain, for example, makes sure that hello virtual machines aren't updated at hello same time during planned maintenance downtime.</span></span>
* <span data-ttu-id="ce898-280">Az összes virtuális gép hello részét képező azonos tartalék tartományban.</span><span class="sxs-lookup"><span data-stu-id="ce898-280">All virtual machines are part of hello same fault domain.</span></span> <span data-ttu-id="ce898-281">A tartalék tartomány például gondoskodik arról, hogy telepít virtuális gépeket, hogy nincs hibaérzékeny pontok kialakulását hatással van az összes virtuális gép hello rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="ce898-281">A fault domain, for example, makes sure that virtual machines are deployed so that no single point of failure affects hello availability of all virtual machines.</span></span>

<span data-ttu-id="ce898-282">További tudnivalók túl[hello virtuális gépek rendelkezésre állásának kezelése][virtual-machines-manage-availability].</span><span class="sxs-lookup"><span data-stu-id="ce898-282">Learn more about how too[manage hello availability of virtual machines][virtual-machines-manage-availability].</span></span>

<span data-ttu-id="ce898-283">Mivel hello Azure storage-fiók egy potenciális hibaérzékeny pontok kialakulását, fontos toohave is legalább két Azure storage-fiókok, legalább két virtuális gép szétosztva.</span><span class="sxs-lookup"><span data-stu-id="ce898-283">Because hello Azure storage account is a potential single point of failure, it's important toohave at least two Azure storage accounts, in which at least two virtual machines are distributed.</span></span> <span data-ttu-id="ce898-284">Az ideális beállítás az egyes virtuális gépek egy SAP párbeszédpanel példányát futtató hello lemezek során eltérő tárfiók volna telepíthetők.</span><span class="sxs-lookup"><span data-stu-id="ce898-284">In an ideal setup, hello disks of each virtual machine that is running an SAP dialog instance would be deployed in a different storage account.</span></span>

### <span data-ttu-id="ce898-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Magas rendelkezésre állású SAP ASC/SCS példány</span><span class="sxs-lookup"><span data-stu-id="ce898-285"><a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a> High-availability SAP ASCS/SCS instance</span></span>
<span data-ttu-id="ce898-286">5. ábra egy példát a magas rendelkezésre állású SAP ASC/SCS példánya.</span><span class="sxs-lookup"><span data-stu-id="ce898-286">Figure 5 is an example of a high-availability SAP ASCS/SCS instance.</span></span>

![5. ábra: Magas rendelkezésre állású SAP ASC/SCS példány][sap-ha-guide-figure-2001]

<span data-ttu-id="ce898-288">_**5. ábra:** magas rendelkezésre állású SAP ASC/SCS példány_</span><span class="sxs-lookup"><span data-stu-id="ce898-288">_**Figure 5:** High-availability SAP ASCS/SCS instance_</span></span>

#### <span data-ttu-id="ce898-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC/SCS példány magas rendelkezésre állását a Windows Server feladatátvételi fürtszolgáltatási az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ce898-289"><a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a> SAP ASCS/SCS instance high availability with Windows Server Failover Clustering in Azure</span></span>
<span data-ttu-id="ce898-290">Képest toobare nélküli vagy saját felhőben történő alkalmazáshoz, Azure virtuális gépek Windows Server feladatátvételi fürtszolgáltatás további lépéseket tooconfigure igényel.</span><span class="sxs-lookup"><span data-stu-id="ce898-290">Compared toobare metal or private cloud deployments, Azure Virtual Machines requires additional steps tooconfigure Windows Server Failover Clustering.</span></span> <span data-ttu-id="ce898-291">toobuild Windows feladatátvevő fürt, szüksége megosztott fürtlemezre mutat, több IP-címek, több virtuális állomásnevek és az Azure belső terheléselosztót a fürtszolgáltatás egy SAP ASC/SCS példányát.</span><span class="sxs-lookup"><span data-stu-id="ce898-291">toobuild a Windows failover cluster, you need a shared cluster disk, several IP addresses, several virtual host names, and an Azure internal load balancer for clustering an SAP ASCS/SCS instance.</span></span> <span data-ttu-id="ce898-292">A Microsoft hello cikk későbbi részében részletesebben ismertetik el.</span><span class="sxs-lookup"><span data-stu-id="ce898-292">We discuss this in more detail later in hello article.</span></span>

![6. ábra: A Windows Server feladatátvételi fürtszolgáltatási SIOS DataKeeper használatával az Azure-ban SAP ASC/SCS konfigurációhoz][sap-ha-guide-figure-1002]

<span data-ttu-id="ce898-294">_**6. ábra:** Windows Server feladatátvételi fürtszolgáltatási az Azure-ban SIOS DataKeeper SAP ASC/SCS konfigurációhoz_</span><span class="sxs-lookup"><span data-stu-id="ce898-294">_**Figure 6:** Windows Server Failover Clustering for an SAP ASCS/SCS configuration in Azure with SIOS DataKeeper_</span></span>

### <span data-ttu-id="ce898-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Magas rendelkezésre állású DBMS példány</span><span class="sxs-lookup"><span data-stu-id="ce898-295"><a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>High-availability DBMS instance</span></span>
<span data-ttu-id="ce898-296">hello adatbázis-kezelő a rendszer egyetlen pont, forduljon egy SAP rendszerben.</span><span class="sxs-lookup"><span data-stu-id="ce898-296">hello DBMS also is a single point of contact in an SAP system.</span></span> <span data-ttu-id="ce898-297">Tooprotect kell azt a magas rendelkezésre állású megoldás segítségével.</span><span class="sxs-lookup"><span data-stu-id="ce898-297">You need tooprotect it by using a high-availability solution.</span></span> <span data-ttu-id="ce898-298">7. ábra mutatja egy SQL Server Always On magas rendelkezésre állású megoldás az Azure-ban, a Windows Server feladatátvételi fürtszolgáltatási és hello Azure belső terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="ce898-298">Figure 7 shows a SQL Server Always On high-availability solution in Azure, with Windows Server Failover Clustering and hello Azure internal load balancer.</span></span> <span data-ttu-id="ce898-299">SQL Server Always On DBMS adatainak és naplókönyvtárainak fájlokat replikálja a saját adatbázis-kezelő replikáció használatával.</span><span class="sxs-lookup"><span data-stu-id="ce898-299">SQL Server Always On replicates DBMS data and log files by using its own DBMS replication.</span></span> <span data-ttu-id="ce898-300">Ebben az esetben akkor nem kell a fürt megosztott lemezeket, egyszerűbbé teszi a teljes hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="ce898-300">In this case, you don't need cluster shared disks, which simplifies hello entire setup.</span></span>

![7. ábra: Példa egy magas rendelkezésre állású SAP DBMS, az SQL Server Always On][sap-ha-guide-figure-2003]

<span data-ttu-id="ce898-302">_**7. ábra:** egy magas rendelkezésre állású SAP DBMS, az SQL Server Always On – példa_</span><span class="sxs-lookup"><span data-stu-id="ce898-302">_**Figure 7:** Example of a high-availability SAP DBMS, with SQL Server Always On_</span></span>

<span data-ttu-id="ce898-303">Hello Azure Resource Manager telepítési modell segítségével az Azure SQL Server fürtszolgáltatással kapcsolatos további információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="ce898-303">For more information about clustering SQL Server in Azure by using hello Azure Resource Manager deployment model, see these articles:</span></span>

* <span data-ttu-id="ce898-304">[Always On rendelkezésre állási csoport konfigurálásához Azure virtuális gépek manuálisan erőforrás-kezelő használatával][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span><span class="sxs-lookup"><span data-stu-id="ce898-304">[Configure Always On availability group in Azure Virtual Machines manually by using Resource Manager][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]</span></span>
* <span data-ttu-id="ce898-305">[Egy Azure belső terheléselosztót egy Always On rendelkezésre állási csoport konfigurálása az Azure-ban][virtual-machines-windows-portal-sql-alwayson-int-listener]</span><span class="sxs-lookup"><span data-stu-id="ce898-305">[Configure an Azure internal load balancer for an Always On availability group in Azure][virtual-machines-windows-portal-sql-alwayson-int-listener]</span></span>

## <span data-ttu-id="ce898-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Végpontok közötti magas rendelkezésre állású környezetekben</span><span class="sxs-lookup"><span data-stu-id="ce898-306"><a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a> End-to-end high-availability deployment scenarios</span></span>

### <a name="deployment-scenario-using-architectural-template-1"></a><span data-ttu-id="ce898-307">Üzembe helyezési forgatókönyv segítségével 1. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="ce898-307">Deployment scenario using Architectural Template 1</span></span>

<span data-ttu-id="ce898-308">8. ábrán egy SAP NetWeaver magas rendelkezésre állású architektúra példáját mutatja be az Azure-ban **egy** SAP rendszer.</span><span class="sxs-lookup"><span data-stu-id="ce898-308">Figure 8 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="ce898-309">Ebben a forgatókönyvben be van állítva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ce898-309">This scenario is set up as follows:</span></span>

- <span data-ttu-id="ce898-310">Egy kijelölt fürt hello SAP ASC/SCS példányát használja.</span><span class="sxs-lookup"><span data-stu-id="ce898-310">One dedicated cluster is used for hello SAP ASCS/SCS instance.</span></span>
- <span data-ttu-id="ce898-311">Egy kijelölt fürt hello DBMS példányát használja.</span><span class="sxs-lookup"><span data-stu-id="ce898-311">One dedicated cluster is used for hello DBMS instance.</span></span>
- <span data-ttu-id="ce898-312">SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="ce898-312">SAP Application Server instances are deployed in their own dedicated VMs.</span></span>

![8. ábra: SAP magas rendelkezésre állású architekturális sablon 1, a kijelölt fürt ASC/SCS és adatbázis-kezelő][sap-ha-guide-figure-2004]

<span data-ttu-id="ce898-314">_**8. ábra:** SAP architekturális sablon 1 magas rendelkezésre állású, dedikált fürtök ASC/SCS és adatbázis-kezelő_</span><span class="sxs-lookup"><span data-stu-id="ce898-314">_**Figure 8:** SAP high-availability Architectural Template 1, dedicated clusters for ASCS/SCS and DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-2"></a><span data-ttu-id="ce898-315">Üzembe helyezési forgatókönyv architekturális sablon 2 használatával</span><span class="sxs-lookup"><span data-stu-id="ce898-315">Deployment scenario using Architectural Template 2</span></span>

<span data-ttu-id="ce898-316">9. ábra egy SAP NetWeaver magas rendelkezésre állású architektúra példáját mutatja be az Azure-ban **egy** SAP rendszer.</span><span class="sxs-lookup"><span data-stu-id="ce898-316">Figure 9 shows an example of an SAP NetWeaver high-availability architecture in Azure for **one** SAP system.</span></span> <span data-ttu-id="ce898-317">Ebben a forgatókönyvben be van állítva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ce898-317">This scenario is set up as follows:</span></span>

- <span data-ttu-id="ce898-318">Egy kijelölt fürt használt **mindkét** hello SAP ASC/SCS példányt, és az adatbázis-kezelő hello.</span><span class="sxs-lookup"><span data-stu-id="ce898-318">One dedicated cluster is used for **both** hello SAP ASCS/SCS instance and hello DBMS.</span></span>
- <span data-ttu-id="ce898-319">SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="ce898-319">SAP Application Server instances are deployed in own dedicated VMs.</span></span>

![9. ábra: SAP magas rendelkezésre állású architekturális sablon 2, a dedikált fürtökben ASC/SCS és egy dedikált adatbázis-kezelő fürt][sap-ha-guide-figure-2005]

<span data-ttu-id="ce898-321">_**9. ábra:** SAP magas rendelkezésre állású architekturális sablon 2, a dedikált fürtökben ASC/SCS és egy dedikált adatbázis-kezelő fürt_</span><span class="sxs-lookup"><span data-stu-id="ce898-321">_**Figure 9:** SAP high-availability Architectural Template 2, with a dedicated cluster for ASCS/SCS and a dedicated cluster for DBMS_</span></span>

### <a name="deployment-scenario-using-architectural-template-3"></a><span data-ttu-id="ce898-322">Üzembe helyezési forgatókönyv a 3. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="ce898-322">Deployment scenario using Architectural Template 3</span></span>

<span data-ttu-id="ce898-323">10. ábrán látható egy példa egy SAP NetWeaver magas rendelkezésre állású architektúra az Azure-ban **két** rendszerek, a SAP &lt;SID1&gt; és &lt;SID2&gt;.</span><span class="sxs-lookup"><span data-stu-id="ce898-323">Figure 10 shows an example of an SAP NetWeaver high-availability architecture in Azure for **two** SAP systems, with &lt;SID1&gt; and &lt;SID2&gt;.</span></span> <span data-ttu-id="ce898-324">Ebben a forgatókönyvben be van állítva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ce898-324">This scenario is set up as follows:</span></span>

- <span data-ttu-id="ce898-325">Egy kijelölt fürt használt **mindkét** hello SAP ASC/SCS SID1 példány *és* hello SAP ASC/SCS SID2 példány (egy fürt).</span><span class="sxs-lookup"><span data-stu-id="ce898-325">One dedicated cluster is used for **both** hello SAP ASCS/SCS SID1 instance *and* hello SAP ASCS/SCS SID2 instance (one cluster).</span></span>
- <span data-ttu-id="ce898-326">Egy kijelölt fürt DBMS SID1 szolgál, és egy másik kijelölt fürt használt adatbázis-kezelő SID2 (fürtök).</span><span class="sxs-lookup"><span data-stu-id="ce898-326">One dedicated cluster is used for DBMS SID1, and another dedicated cluster is used for DBMS SID2 (two clusters).</span></span>
- <span data-ttu-id="ce898-327">SAP alkalmazáskiszolgáló példányainak hello SAP rendszer SID1 rendelkezik saját dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ce898-327">SAP Application Server instances for hello SAP system SID1 have their own dedicated VMs.</span></span>
- <span data-ttu-id="ce898-328">SAP alkalmazáskiszolgáló példányainak hello SAP rendszer SID2 rendelkezik saját dedikált virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ce898-328">SAP Application Server instances for hello SAP system SID2 have their own dedicated VMs.</span></span>

![10. ábra: SAP magas rendelkezésre állású architekturális sablon 3, egy dedikált fürttel különböző ASC/SCS-példányok][sap-ha-guide-figure-6003]

<span data-ttu-id="ce898-330">_**10. ábra:** SAP magas rendelkezésre állású architekturális sablon 3, egy dedikált fürttel különböző ASC/SCS-példányok_</span><span class="sxs-lookup"><span data-stu-id="ce898-330">_**Figure 10:** SAP high-availability Architectural Template 3, with a dedicated cluster for different ASCS/SCS instances_</span></span>

## <span data-ttu-id="ce898-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Hello infrastruktúra előkészítése</span><span class="sxs-lookup"><span data-stu-id="ce898-331"><a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a> Prepare hello infrastructure</span></span>

### <a name="prepare-hello-infrastructure-for-architectural-template-1"></a><span data-ttu-id="ce898-332">1. sablon architekturális. hello infrastruktúra előkészítése</span><span class="sxs-lookup"><span data-stu-id="ce898-332">Prepare hello infrastructure for Architectural Template 1</span></span>
<span data-ttu-id="ce898-333">Az SAP Azure Resource Manager-sablonok segítségével egyszerűsítheti a szükséges erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ce898-333">Azure Resource Manager templates for SAP help simplify deployment of required resources.</span></span>

<span data-ttu-id="ce898-334">hello háromrétegű sablonok az Azure Resource Manager is támogatja a magas rendelkezésre állású forgatókönyvek, például architekturális sablon az 1., amely két fürttel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ce898-334">hello three-tier templates in Azure Resource Manager also support high-availability scenarios, such as in Architectural Template 1, which has two clusters.</span></span> <span data-ttu-id="ce898-335">Minden fürthöz egy SAP hibaérzékeny pontot SAP ASC/SCS és adatbázis-kezelő.</span><span class="sxs-lookup"><span data-stu-id="ce898-335">Each cluster is an SAP single point of failure for SAP ASCS/SCS and DBMS.</span></span>

<span data-ttu-id="ce898-336">Itt található, ahol kaphat Azure Resource Manager-sablonok azt írják le, a cikkben hello. példa:</span><span class="sxs-lookup"><span data-stu-id="ce898-336">Here's where you can get Azure Resource Manager templates for hello example scenario we describe in this article:</span></span>

* [<span data-ttu-id="ce898-337">Kép: Azure piactér</span><span class="sxs-lookup"><span data-stu-id="ce898-337">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [<span data-ttu-id="ce898-338">Kép: egyéni</span><span class="sxs-lookup"><span data-stu-id="ce898-338">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

<span data-ttu-id="ce898-339">hello infrastruktúra tooprepare architekturális sablon 1:</span><span class="sxs-lookup"><span data-stu-id="ce898-339">tooprepare hello infrastructure for Architectural Template 1:</span></span>

- <span data-ttu-id="ce898-340">Az Azure portál, a hello hello **paraméterek** paneljén, hello **SYSTEMAVAILABILITY** mezőben válassza **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="ce898-340">In hello Azure portal, on hello **Parameters** blade, in hello **SYSTEMAVAILABILITY** box, select **HA**.</span></span>

  ![11. ábra: Beállítása SAP magas rendelkezésre állású Azure Resource Manager-paraméterek][sap-ha-guide-figure-3000]

<span data-ttu-id="ce898-342">_**11. ábra:** beállítása SAP magas rendelkezésre állású Azure Resource Manager-paraméterek_</span><span class="sxs-lookup"><span data-stu-id="ce898-342">_**Figure 11:** Set SAP high-availability Azure Resource Manager parameters_</span></span>


  <span data-ttu-id="ce898-343">hello sablonok létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ce898-343">hello templates create:</span></span>

  * <span data-ttu-id="ce898-344">**Virtuális gépek**:</span><span class="sxs-lookup"><span data-stu-id="ce898-344">**Virtual machines**:</span></span>
    * <span data-ttu-id="ce898-345">SAP alkalmazáskiszolgáló virtuális gépek: <*SAPSystemSID*> - di - <*száma*></span><span class="sxs-lookup"><span data-stu-id="ce898-345">SAP Application Server virtual machines: <*SAPSystemSID*>-di-<*Number*></span></span>
    * <span data-ttu-id="ce898-346">A fürt virtuális gépek ASC/SCS: <*SAPSystemSID*> - ASC - <*száma*></span><span class="sxs-lookup"><span data-stu-id="ce898-346">ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-ascs-<*Number*></span></span>
    * <span data-ttu-id="ce898-347">Adatbázis-kezelő fürt: <*SAPSystemSID*> - db - <*száma*></span><span class="sxs-lookup"><span data-stu-id="ce898-347">DBMS cluster: <*SAPSystemSID*>-db-<*Number*></span></span>

  * <span data-ttu-id="ce898-348">**Hálózati kártya tartozó összes virtuális géphez társított IP-címekkel rendelkező**:</span><span class="sxs-lookup"><span data-stu-id="ce898-348">**Network cards for all virtual machines, with associated IP addresses**:</span></span>
    * <span data-ttu-id="ce898-349"><*SAPSystemSID*> - nic - di - <*száma*></span><span class="sxs-lookup"><span data-stu-id="ce898-349"><*SAPSystemSID*>-nic-di-<*Number*></span></span>
    * <span data-ttu-id="ce898-350"><*SAPSystemSID*> - nic - ASC - <*száma*></span><span class="sxs-lookup"><span data-stu-id="ce898-350"><*SAPSystemSID*>-nic-ascs-<*Number*></span></span>
    * <span data-ttu-id="ce898-351"><*SAPSystemSID*> - nic - db - <*száma*></span><span class="sxs-lookup"><span data-stu-id="ce898-351"><*SAPSystemSID*>-nic-db-<*Number*></span></span>

  * <span data-ttu-id="ce898-352">**Az Azure storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="ce898-352">**Azure storage accounts**</span></span>

  * <span data-ttu-id="ce898-353">**Rendelkezésre állási csoportok** esetében:</span><span class="sxs-lookup"><span data-stu-id="ce898-353">**Availability groups** for:</span></span>
    * <span data-ttu-id="ce898-354">SAP alkalmazáskiszolgáló virtuális gépek: <*SAPSystemSID*> - avset - di</span><span class="sxs-lookup"><span data-stu-id="ce898-354">SAP Application Server virtual machines: <*SAPSystemSID*>-avset-di</span></span>
    * <span data-ttu-id="ce898-355">SAP ASC/SCS cluster virtuális gépek: <*SAPSystemSID*> - avset - ASC</span><span class="sxs-lookup"><span data-stu-id="ce898-355">SAP ASCS/SCS cluster virtual machines: <*SAPSystemSID*>-avset-ascs</span></span>
    * <span data-ttu-id="ce898-356">Adatbázis-kezelő fürt virtuális gépek: <*SAPSystemSID*> - avset - adatbázis</span><span class="sxs-lookup"><span data-stu-id="ce898-356">DBMS cluster virtual machines: <*SAPSystemSID*>-avset-db</span></span>

  * <span data-ttu-id="ce898-357">**Az Azure belső terheléselosztó**:</span><span class="sxs-lookup"><span data-stu-id="ce898-357">**Azure internal load balancer**:</span></span>
    * <span data-ttu-id="ce898-358">Az összes port hello ASC/SCS példányhoz, és az IP-cím <*SAPSystemSID*> - lb - ASC</span><span class="sxs-lookup"><span data-stu-id="ce898-358">With all ports for hello ASCS/SCS instance and IP address <*SAPSystemSID*>-lb-ascs</span></span>
    * <span data-ttu-id="ce898-359">Az összes port hello SQL Server adatbázis-kezelő és az IP-cím <*SAPSystemSID*> - lb - adatbázis</span><span class="sxs-lookup"><span data-stu-id="ce898-359">With all ports for hello SQL Server DBMS and IP address <*SAPSystemSID*>-lb-db</span></span>

  * <span data-ttu-id="ce898-360">**Hálózati biztonsági csoport**: <*SAPSystemSID*> - nsg - ASC-0</span><span class="sxs-lookup"><span data-stu-id="ce898-360">**Network security group**: <*SAPSystemSID*>-nsg-ascs-0</span></span>  
    * <span data-ttu-id="ce898-361">Egy nyitott külső Remote Desktop Protocol (RDP) port toohello a <*SAPSystemSID*> virtuálisgép - ASC - 0</span><span class="sxs-lookup"><span data-stu-id="ce898-361">With an open external Remote Desktop Protocol (RDP) port toohello <*SAPSystemSID*>-ascs-0 virtual machine</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-362">Az összes IP-címet oszt hello hálózati kártyák és az Azure belső terheléselosztók **dinamikus** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ce898-362">All IP addresses of hello network cards and Azure internal load balancers are **dynamic** by default.</span></span> <span data-ttu-id="ce898-363">Túl módosításukra**statikus** IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="ce898-363">Change them too**static** IP addresses.</span></span> <span data-ttu-id="ce898-364">Azt ismerteti, hogy hogyan toodo ez hello cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="ce898-364">We describe how toodo this later in hello article.</span></span>
>
>

### <span data-ttu-id="ce898-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Virtuális gépek telepítése a vállalati hálózati kapcsolat (létesítmények közötti) toouse éles környezetben</span><span class="sxs-lookup"><span data-stu-id="ce898-365"><a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a> Deploy virtual machines with corporate network connectivity (cross-premises) toouse in production</span></span>
<span data-ttu-id="ce898-366">Éles rendszerek esetén SAP, az Azure virtuális gépek telepítése a [vállalati hálózati kapcsolat (létesítmények közötti)] [ planning-guide-2.2] Azure hely-hely típusú VPN vagy Azure ExpressRoute segítségével.</span><span class="sxs-lookup"><span data-stu-id="ce898-366">For production SAP systems, deploy Azure virtual machines with [corporate network connectivity (cross-premises)][planning-guide-2.2] by using Azure Site-to-Site VPN or Azure ExpressRoute.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-367">Az Azure Virtual Network-példányt is használhat.</span><span class="sxs-lookup"><span data-stu-id="ce898-367">You can use your Azure Virtual Network instance.</span></span> <span data-ttu-id="ce898-368">hello virtuális hálózat és alhálózat már létrehozott és előkészítése.</span><span class="sxs-lookup"><span data-stu-id="ce898-368">hello virtual network and subnet have already been created and prepared.</span></span>
>
>

1.  <span data-ttu-id="ce898-369">Az Azure portál, a hello hello **paraméterek** paneljén, hello **NEWOREXISTINGSUBNET** mezőben válassza **meglévő**.</span><span class="sxs-lookup"><span data-stu-id="ce898-369">In hello Azure portal, on hello **Parameters** blade, in hello **NEWOREXISTINGSUBNET** box, select **existing**.</span></span>
2.  <span data-ttu-id="ce898-370">A hello **SUBNETID** mezőben adja meg az elkészített Azure hálózati tervezett toodeploy az Azure virtuális gépek SubnetID hello teljes karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ce898-370">In hello **SUBNETID** box, add hello full string of your prepared Azure network SubnetID where you plan toodeploy your Azure virtual machines.</span></span>
3.  <span data-ttu-id="ce898-371">az összes Azure-hálózat alhálózatok listájának tooget futtassa a PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="ce898-371">tooget a list of all Azure network subnets, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  <span data-ttu-id="ce898-372">Hello **azonosító** mezőben látható hello **SUBNETID**.</span><span class="sxs-lookup"><span data-stu-id="ce898-372">hello **ID** field shows hello **SUBNETID**.</span></span>
4. <span data-ttu-id="ce898-373">az összes tooget **SUBNETID** értékek, futtassa a PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="ce898-373">tooget a list of all **SUBNETID** values, run this PowerShell command:</span></span>

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  <span data-ttu-id="ce898-374">Hello **SUBNETID** dolgozunk:</span><span class="sxs-lookup"><span data-stu-id="ce898-374">hello **SUBNETID** looks like this:</span></span>

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <span data-ttu-id="ce898-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>A tesztelési és bemutató csak felhőalapú SAP-példányok telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-375"><a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a> Deploy cloud-only SAP instances for test and demo</span></span>
<span data-ttu-id="ce898-376">Egy kizárólag felhőalapú üzembe helyezési modellben a magas rendelkezésre állású SAP rendszereket telepítheti központilag.</span><span class="sxs-lookup"><span data-stu-id="ce898-376">You can deploy your high-availability SAP system in a cloud-only deployment model.</span></span> <span data-ttu-id="ce898-377">Ez a központi telepítési típus elsősorban akkor hasznos, bemutató és tesztelési használatából adódó.</span><span class="sxs-lookup"><span data-stu-id="ce898-377">This kind of deployment primarily is useful for demo and test use cases.</span></span> <span data-ttu-id="ce898-378">Nem alkalmas a termelési használati eseteket.</span><span class="sxs-lookup"><span data-stu-id="ce898-378">It's not suited for production use cases.</span></span>

- <span data-ttu-id="ce898-379">Az Azure portál, a hello hello **paraméterek** paneljén, hello **NEWOREXISTINGSUBNET** mezőben válassza **új**.</span><span class="sxs-lookup"><span data-stu-id="ce898-379">In hello Azure portal, on hello **Parameters** blade, in hello **NEWOREXISTINGSUBNET** box, select **new**.</span></span> <span data-ttu-id="ce898-380">Hagyja hello **SUBNETID** mező üres.</span><span class="sxs-lookup"><span data-stu-id="ce898-380">Leave hello **SUBNETID** field empty.</span></span>

  <span data-ttu-id="ce898-381">hello SAP Azure Resource Manager sablon automatikusan létrehozza a hello Azure virtuális hálózat és alhálózat.</span><span class="sxs-lookup"><span data-stu-id="ce898-381">hello SAP Azure Resource Manager template automatically creates hello Azure virtual network and subnet.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-382">Toodeploy is szükség van legalább egy virtuális gép dedikált az Active Directory és DNS-ét hello ugyanazon Azure-beli virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="ce898-382">You also need toodeploy at least one dedicated virtual machine for Active Directory and DNS in hello same Azure Virtual Network instance.</span></span> <span data-ttu-id="ce898-383">hello sablon nem hoz létre a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ce898-383">hello template doesn't create these virtual machines.</span></span>
>
>


### <a name="prepare-hello-infrastructure-for-architectural-template-2"></a><span data-ttu-id="ce898-384">Hello infrastruktúra előkészítése a 2. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="ce898-384">Prepare hello infrastructure for Architectural Template 2</span></span>

<span data-ttu-id="ce898-385">Az Azure Resource Manager sablon használhatja az SAP toohelp egyszerűsítése SAP architekturális sablon 2 álló szükséges infrastruktúra központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="ce898-385">You can use this Azure Resource Manager template for SAP toohelp simplify deployment of required infrastructure resources for SAP Architectural Template 2.</span></span>

<span data-ttu-id="ce898-386">Itt található, ahol kaphat Azure Resource Manager-sablonok a központi telepítési forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="ce898-386">Here's where you can get Azure Resource Manager templates for this deployment scenario:</span></span>

* [<span data-ttu-id="ce898-387">Kép: Azure piactér</span><span class="sxs-lookup"><span data-stu-id="ce898-387">Azure Marketplace image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [<span data-ttu-id="ce898-388">Kép: egyéni</span><span class="sxs-lookup"><span data-stu-id="ce898-388">Custom image</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-hello-infrastructure-for-architectural-template-3"></a><span data-ttu-id="ce898-389">Hello infrastruktúra előkészítése a 3. sablon architekturális.</span><span class="sxs-lookup"><span data-stu-id="ce898-389">Prepare hello infrastructure for Architectural Template 3</span></span>

<span data-ttu-id="ce898-390">Hello infrastruktúra előkészítése, és konfigurálja az SAP **multi-SID**.</span><span class="sxs-lookup"><span data-stu-id="ce898-390">You can prepare hello infrastructure and configure SAP for **multi-SID**.</span></span> <span data-ttu-id="ce898-391">Például hozzáadhat egy SAP ASC/SCS másodpéldányt be egy *meglévő* fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ce898-391">For example, you can add an additional SAP ASCS/SCS instance into an *existing* cluster configuration.</span></span> <span data-ttu-id="ce898-392">További információkért lásd: [konfigurálása egy SAP ASC/SCS másodpéldányt be egy meglévő fürt konfigurációs toocreate egy SAP multi-SID-konfigurációját az Azure Resource Manager][sap-ha-multi-sid-guide].</span><span class="sxs-lookup"><span data-stu-id="ce898-392">For more information, see [Configure an additional SAP ASCS/SCS instance into an existing cluster configuration toocreate an SAP multi-SID configuration in Azure Resource Manager][sap-ha-multi-sid-guide].</span></span>

<span data-ttu-id="ce898-393">Ha azt szeretné, hogy egy új multi-SID-fürt toocreate, használhatja a hello multi-SID [gyorsindítási sablonok a Githubon](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="ce898-393">If you want toocreate a new multi-SID cluster, you can use hello multi-SID [quickstart templates on GitHub](https://github.com/Azure/azure-quickstart-templates).</span></span>
<span data-ttu-id="ce898-394">toocreate multi-SID új fürt, a következő három sablonok toodeploy hello kell:</span><span class="sxs-lookup"><span data-stu-id="ce898-394">toocreate a new multi-SID cluster, you need toodeploy hello following three templates:</span></span>

* [<span data-ttu-id="ce898-395">Asc/SCS sablon</span><span class="sxs-lookup"><span data-stu-id="ce898-395">ASCS/SCS template</span></span>](#ASCS-SCS-template)
* [<span data-ttu-id="ce898-396">Adatbázis-sablon</span><span class="sxs-lookup"><span data-stu-id="ce898-396">Database template</span></span>](#database-template)
* [<span data-ttu-id="ce898-397">Alkalmazássablon-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="ce898-397">Application servers template</span></span>](#application-servers-template)

<span data-ttu-id="ce898-398">hello alábbiakban rendelkezik hello sablonok és hello paraméterek hello sablonok tooprovide van szüksége további információt.</span><span class="sxs-lookup"><span data-stu-id="ce898-398">hello following sections have more details about hello templates and hello parameters you need tooprovide in hello templates.</span></span>

#### <span data-ttu-id="ce898-399"><a name="ASCS-SCS-template"></a>Asc/SCS sablon</span><span class="sxs-lookup"><span data-stu-id="ce898-399"><a name="ASCS-SCS-template"></a> ASCS/SCS template</span></span>

<span data-ttu-id="ce898-400">hello ASC/SCS sablon használható a Windows Server feladatátvevő fürt, amelyen több ASC/SCS példány toocreate két virtuális gép telepítését.</span><span class="sxs-lookup"><span data-stu-id="ce898-400">hello ASCS/SCS template deploys two virtual machines that you can use toocreate a Windows Server failover cluster that hosts multiple ASCS/SCS instances.</span></span>

<span data-ttu-id="ce898-401">hello ASC/SCS multi-SID-sablon hello mentése tooset [ASC/SCS multi-SID sablon][sap-templates-3-tier-multisid-xscs-marketplace-image], adja meg a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="ce898-401">tooset up hello ASCS/SCS multi-SID template, in hello [ASCS/SCS multi-SID template][sap-templates-3-tier-multisid-xscs-marketplace-image], enter values for hello following parameters:</span></span>

  - <span data-ttu-id="ce898-402">**Erőforrás-előtag**.</span><span class="sxs-lookup"><span data-stu-id="ce898-402">**Resource Prefix**.</span></span>  <span data-ttu-id="ce898-403">Állítsa be a hello erőforrás előtag használt tooprefix hello központi telepítése során létrehozott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ce898-403">Set hello resource prefix, which is used tooprefix all resources that are created during hello deployment.</span></span> <span data-ttu-id="ce898-404">Hello erőforrások nem tartoznak tooonly egy SAP rendszer, mert hello előtag hello erőforrás nincs hello egy SAP rendszer biztonsági azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ce898-404">Because hello resources do not belong tooonly one SAP system, hello prefix of hello resource is not hello SID of one SAP system.</span></span>  <span data-ttu-id="ce898-405">hello előtagnak közé kell esnie **3-6 karakter**.</span><span class="sxs-lookup"><span data-stu-id="ce898-405">hello prefix must be between **three and six characters**.</span></span>
  - <span data-ttu-id="ce898-406">**A verem típus**.</span><span class="sxs-lookup"><span data-stu-id="ce898-406">**Stack Type**.</span></span> <span data-ttu-id="ce898-407">Válassza ki a hello SAP rendszer hello verem típusát.</span><span class="sxs-lookup"><span data-stu-id="ce898-407">Select hello stack type of hello SAP system.</span></span> <span data-ttu-id="ce898-408">Hello verem típusától függően a Azure Load Balancer egy (ABAP vagy csak a Java), vagy két (ABAP + Java) magánhálózati IP-címek SAP rendszerenként rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ce898-408">Depending on hello stack type, Azure Load Balancer has one (ABAP or Java only) or two (ABAP+Java) private IP addresses per SAP system.</span></span>
  -  <span data-ttu-id="ce898-409">**Operációs rendszer típusa**.</span><span class="sxs-lookup"><span data-stu-id="ce898-409">**OS Type**.</span></span> <span data-ttu-id="ce898-410">Válassza ki a hello virtuális gép operációs rendszerének hello.</span><span class="sxs-lookup"><span data-stu-id="ce898-410">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="ce898-411">**SAP rendszer száma**.</span><span class="sxs-lookup"><span data-stu-id="ce898-411">**SAP System Count**.</span></span> <span data-ttu-id="ce898-412">Válassza ki a hello szám SAP rendszerek kívánt tooinstall ennek a fürtnek.</span><span class="sxs-lookup"><span data-stu-id="ce898-412">Select hello number of SAP systems you want tooinstall in this cluster.</span></span>
  -  <span data-ttu-id="ce898-413">**Rendelkezésre állására**.</span><span class="sxs-lookup"><span data-stu-id="ce898-413">**System Availability**.</span></span> <span data-ttu-id="ce898-414">Válassza ki **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="ce898-414">Select **HA**.</span></span>
  -  <span data-ttu-id="ce898-415">**Rendszergazda felhasználónevét és a rendszergazdai jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ce898-415">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="ce898-416">Hozzon létre egy új felhasználót, amely használt toosign toohello gépen.</span><span class="sxs-lookup"><span data-stu-id="ce898-416">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="ce898-417">**Új vagy meglévő alhálózati**.</span><span class="sxs-lookup"><span data-stu-id="ce898-417">**New Or Existing Subnet**.</span></span> <span data-ttu-id="ce898-418">Állítsa be, hogy egy új virtuális hálózat és alhálózat kell létrehozni, vagy használjon egy létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ce898-418">Set whether a new virtual network and subnet should be created, or an existing subnet should be used.</span></span> <span data-ttu-id="ce898-419">Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válassza ki a **meglévő**.</span><span class="sxs-lookup"><span data-stu-id="ce898-419">If you already have a virtual network that is connected tooyour on-premises network, select **existing**.</span></span>
  -  <span data-ttu-id="ce898-420">**Alhálózati azonosító**. Hello azonosítója hello alhálózati toowhich hello virtuális gépek kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="ce898-420">**Subnet Id**. Set hello ID of hello subnet toowhich hello virtual machines should be connected.</span></span> <span data-ttu-id="ce898-421">Válassza ki a virtuális magánhálózati (VPN) vagy ExpressRoute virtuális hálózat tooconnect hello virtuális gép tooyour a helyszíni hálózati hello alhálózati.</span><span class="sxs-lookup"><span data-stu-id="ce898-421">Select hello subnet of your virtual private network (VPN) or ExpressRoute virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="ce898-422">hello azonosítója általában néz ki:</span><span class="sxs-lookup"><span data-stu-id="ce898-422">hello ID usually looks like this:</span></span>

   <span data-ttu-id="ce898-423">/Subscriptions/ <*előfizetés-azonosító*> /resourceGroups/ <*erőforráscsoport-név*> /providers/Microsoft.Network/virtualNetworks/ <*virtuálishálózat-név*> /subnets/ <*alhálózat neve*></span><span class="sxs-lookup"><span data-stu-id="ce898-423">/subscriptions/<*subscription id*>/resourceGroups/<*resource group name*>/providers/Microsoft.Network/virtualNetworks/<*virtual network name*>/subnets/<*subnet name*></span></span>

<span data-ttu-id="ce898-424">hello sablon Azure Load Balancer egy példány, amely támogatja a több SAP rendszer telepíti.</span><span class="sxs-lookup"><span data-stu-id="ce898-424">hello template deploys one Azure Load Balancer instance, which supports multiple SAP systems.</span></span>

- <span data-ttu-id="ce898-425">hello ASC példányok példányszámának 00, 10, 20 beállítást...</span><span class="sxs-lookup"><span data-stu-id="ce898-425">hello ASCS instances are configured for instance number 00, 10, 20...</span></span>
- <span data-ttu-id="ce898-426">hello SCS példányok példányszámának 01, 11, 21 beállítást...</span><span class="sxs-lookup"><span data-stu-id="ce898-426">hello SCS instances are configured for instance number 01, 11, 21...</span></span>
- <span data-ttu-id="ce898-427">hello ASC sorba helyezni replikációs Server (SSZON) (csak Linux) példányok példányszámának 02, 12, 22 beállítást...</span><span class="sxs-lookup"><span data-stu-id="ce898-427">hello ASCS Enqueue Replication Server (ERS) (Linux only) instances are configured for instance number 02, 12, 22...</span></span>
- <span data-ttu-id="ce898-428">hello SCS SSZON (csak Linux) példányok példányszámának 03., 13, 23 beállítást...</span><span class="sxs-lookup"><span data-stu-id="ce898-428">hello SCS ERS (Linux only) instances are configured for instance number 03, 13, 23...</span></span>

<span data-ttu-id="ce898-429">hello terheléselosztó tartalmazza (Linux 2) 1 VIP(s), a ASC/SCS 1 x-VIP és a SSZON (csak Linux esetén) 1 x-VIP.</span><span class="sxs-lookup"><span data-stu-id="ce898-429">hello load balancer contains 1 (2 for Linux) VIP(s), 1x VIP for ASCS/SCS and 1x VIP for ERS (Linux only).</span></span>

<span data-ttu-id="ce898-430">hello alábbi lista tartalmazza-e minden terheléselosztási szabályok (ahol x egy SAP rendszer, például 1, 2, 3... hello hello száma):</span><span class="sxs-lookup"><span data-stu-id="ce898-430">hello following list contains all load balancing rules (where x is hello number of hello SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="ce898-431">Minden SAP rendszerhez a Windows-specifikus portokat: 445-ös, 5985</span><span class="sxs-lookup"><span data-stu-id="ce898-431">Windows-specific ports for every SAP system: 445, 5985</span></span>
- <span data-ttu-id="ce898-432">Asc portok (x0 száma): 32 x 0, 36 x 0, 39 x 0, 81-es x 0, 5 x 013, 5 x 014, 5 x 016</span><span class="sxs-lookup"><span data-stu-id="ce898-432">ASCS ports (instance number x0): 32x0, 36x0, 39x0, 81x0, 5x013, 5x014, 5x016</span></span>
- <span data-ttu-id="ce898-433">SCS portok (x1 száma): 32 x 1, 33 x 1, 39 x 1, 81-es x 1, 5 x 113, 5 x 114, 5 x 116</span><span class="sxs-lookup"><span data-stu-id="ce898-433">SCS ports (instance number x1): 32x1, 33x1, 39x1, 81x1, 5x113, 5x114, 5x116</span></span>
- <span data-ttu-id="ce898-434">Linux (példányszámának x2) portok ASC SSZON: 33 x 2, 5 x 213, 5 x 214, 5 x 216</span><span class="sxs-lookup"><span data-stu-id="ce898-434">ASCS ERS ports on Linux (instance number x2): 33x2, 5x213, 5x214, 5x216</span></span>
- <span data-ttu-id="ce898-435">Linux (példányszámának x3) portok SCS SSZON: 33 x 3, 5 x 313, 5 x 314, 5 x 316</span><span class="sxs-lookup"><span data-stu-id="ce898-435">SCS ERS ports on Linux (instance number x3): 33x3, 5x313, 5x314, 5x316</span></span>

<span data-ttu-id="ce898-436">hello terheléselosztó konfigurált toouse hello mintavételi portot (ahol x egy SAP rendszer, például 1, 2, 3... hello hello száma) a következő:</span><span class="sxs-lookup"><span data-stu-id="ce898-436">hello load balancer is configured toouse hello following probe ports (where x is hello number of hello SAP system, for example, 1, 2, 3...):</span></span>
- <span data-ttu-id="ce898-437">Asc/SCS belső terheléselosztó mintavételi portot betölteni: 620 x 0</span><span class="sxs-lookup"><span data-stu-id="ce898-437">ASCS/SCS internal load balancer probe port: 620x0</span></span>
- <span data-ttu-id="ce898-438">SSZON belső terheléselosztó mintavételi portot (csak Linux) betöltése: 621 x 2</span><span class="sxs-lookup"><span data-stu-id="ce898-438">ERS internal load balancer probe port (Linux only): 621x2</span></span>

#### <span data-ttu-id="ce898-439"><a name="database-template"></a>Adatbázis-sablon</span><span class="sxs-lookup"><span data-stu-id="ce898-439"><a name="database-template"></a> Database template</span></span>

<span data-ttu-id="ce898-440">hello adatbázis sablon telepít egy vagy két virtuális gép használható tooinstall hello egy SAP rendszer relációs adatbázis-kezelő rendszerének (RDBMS).</span><span class="sxs-lookup"><span data-stu-id="ce898-440">hello database template deploys one or two virtual machines that you can use tooinstall hello relational database management system (RDBMS) for one SAP system.</span></span> <span data-ttu-id="ce898-441">Például öt SAP rendszerekhez ASC/SCS sablonná telepíti, ha szüksége toodeploy Ez a sablon ötször.</span><span class="sxs-lookup"><span data-stu-id="ce898-441">For example, if you deploy an ASCS/SCS template for five SAP systems, you need toodeploy this template five times.</span></span>

<span data-ttu-id="ce898-442">hello adatbázis multi-SID-sablon, hello mentése tooset [adatbázis multi-SID sablon][sap-templates-3-tier-multisid-db-marketplace-image], adja meg a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="ce898-442">tooset up hello database multi-SID template, in hello [database multi-SID template][sap-templates-3-tier-multisid-db-marketplace-image], enter values for hello following parameters:</span></span>

  -  <span data-ttu-id="ce898-443">**SAP rendszerazonosító**. Adja meg azt szeretné, hogy tooinstall SAP rendszer hello hello SAP rendszer Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ce898-443">**Sap System Id**. Enter hello SAP system ID of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="ce898-444">hello azonosító használandó előtagjaként hello telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="ce898-444">hello ID will be used as a prefix for hello resources that are deployed.</span></span>
  -  <span data-ttu-id="ce898-445">**Operációs rendszer típusa**.</span><span class="sxs-lookup"><span data-stu-id="ce898-445">**Os Type**.</span></span> <span data-ttu-id="ce898-446">Válassza ki a hello virtuális gép operációs rendszerének hello.</span><span class="sxs-lookup"><span data-stu-id="ce898-446">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="ce898-447">**DbType**.</span><span class="sxs-lookup"><span data-stu-id="ce898-447">**Dbtype**.</span></span> <span data-ttu-id="ce898-448">Hello típusának kiválasztása hello adatbázis kívánt tooinstall hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="ce898-448">Select hello type of hello database you want tooinstall on hello cluster.</span></span> <span data-ttu-id="ce898-449">Válassza ki **SQL** Ha azt szeretné, hogy a Microsoft SQL Server tooinstall.</span><span class="sxs-lookup"><span data-stu-id="ce898-449">Select **SQL** if you want tooinstall Microsoft SQL Server.</span></span> <span data-ttu-id="ce898-450">Válassza ki **HANA** tudás az SAP HANA tooinstall hello virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="ce898-450">Select **HANA** if you plan tooinstall SAP HANA on hello virtual machines.</span></span> <span data-ttu-id="ce898-451">Győződjön meg arról, hogy tooselect hello megfelelő operációs rendszer típusa: válassza ki **Windows** SQL, és válasszon egy Linux terjesztési Hana.</span><span class="sxs-lookup"><span data-stu-id="ce898-451">Make sure tooselect hello correct operating system type: select **Windows** for SQL, and select a Linux distribution for HANA.</span></span> <span data-ttu-id="ce898-452">hello Azure Load Balancer, amely virtuális gépek lesznek csatlakoztatott toohello beállított toosupport kijelölt hello adatbázis típus:</span><span class="sxs-lookup"><span data-stu-id="ce898-452">hello Azure Load Balancer that is connected toohello virtual machines will be configured toosupport hello selected database type:</span></span>
    * <span data-ttu-id="ce898-453">**SQL**.</span><span class="sxs-lookup"><span data-stu-id="ce898-453">**SQL**.</span></span> <span data-ttu-id="ce898-454">hello terheléselosztó fog terheléselosztásához 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="ce898-454">hello load balancer will load-balance port 1433.</span></span> <span data-ttu-id="ce898-455">Győződjön meg arról, hogy toouse ezt a portot, az SQL Server Always On beállítás.</span><span class="sxs-lookup"><span data-stu-id="ce898-455">Make sure toouse this port for your SQL Server Always On setup.</span></span>
    * <span data-ttu-id="ce898-456">**HANA**.</span><span class="sxs-lookup"><span data-stu-id="ce898-456">**HANA**.</span></span> <span data-ttu-id="ce898-457">hello terheléselosztó fog terheléselosztásához 35015 és 35017 portot.</span><span class="sxs-lookup"><span data-stu-id="ce898-457">hello load balancer will load-balance ports 35015 and 35017.</span></span> <span data-ttu-id="ce898-458">Győződjön meg arról, hogy tooinstall SAP HANA rendelkező példányszámának **50**.</span><span class="sxs-lookup"><span data-stu-id="ce898-458">Make sure tooinstall SAP HANA with instance number **50**.</span></span>
    <span data-ttu-id="ce898-459">hello terheléselosztó 62550 mintavételi portot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="ce898-459">hello load balancer will use probe port 62550.</span></span>
  -  <span data-ttu-id="ce898-460">**SAP mérete**.</span><span class="sxs-lookup"><span data-stu-id="ce898-460">**Sap System Size**.</span></span> <span data-ttu-id="ce898-461">SAP hello új rendszer hello számának beállítása ad meg.</span><span class="sxs-lookup"><span data-stu-id="ce898-461">Set hello number of SAPS hello new system will provide.</span></span> <span data-ttu-id="ce898-462">Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.</span><span class="sxs-lookup"><span data-stu-id="ce898-462">If you are not sure how many SAPS hello system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="ce898-463">**Rendelkezésre állására**.</span><span class="sxs-lookup"><span data-stu-id="ce898-463">**System Availability**.</span></span> <span data-ttu-id="ce898-464">Válassza ki **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="ce898-464">Select **HA**.</span></span>
  -  <span data-ttu-id="ce898-465">**Rendszergazda felhasználónevét és a rendszergazdai jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ce898-465">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="ce898-466">Hozzon létre egy új felhasználót, amely használt toosign toohello gépen.</span><span class="sxs-lookup"><span data-stu-id="ce898-466">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="ce898-467">**Alhálózati azonosító**. Adja meg a hello hello alhálózat hello hello ASC/SCS sablon telepítése során használt, vagy hello azonosítója hello alhálózat létrehozott hello ASC/SCS sablon telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="ce898-467">**Subnet Id**. Enter hello ID of hello subnet that you used during hello deployment of hello ASCS/SCS template, or hello ID of hello subnet that was created as part of hello ASCS/SCS template deployment.</span></span>

#### <span data-ttu-id="ce898-468"><a name="application-servers-template"></a>Alkalmazássablon-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="ce898-468"><a name="application-servers-template"></a> Application servers template</span></span>

<span data-ttu-id="ce898-469">hello kiszolgálók alkalmazássablon SAP alkalmazáskiszolgáló-példányok egy SAP-rendszerhez használható két vagy több virtuális gépek telepíti.</span><span class="sxs-lookup"><span data-stu-id="ce898-469">hello application servers template deploys two or more virtual machines that can be used as SAP Application Server instances for one SAP system.</span></span> <span data-ttu-id="ce898-470">Például öt SAP rendszerekhez ASC/SCS sablonná telepíti, ha szüksége toodeploy Ez a sablon ötször.</span><span class="sxs-lookup"><span data-stu-id="ce898-470">For example, if you deploy an ASCS/SCS template for five SAP systems, you need toodeploy this template five times.</span></span>

<span data-ttu-id="ce898-471">hello alkalmazás kiszolgálók multi-SID-sablon, hello mentése tooset [kiszolgálók multi-SID alkalmazássablon][sap-templates-3-tier-multisid-apps-marketplace-image], adja meg a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="ce898-471">tooset up hello application servers multi-SID template, in hello [application servers multi-SID template][sap-templates-3-tier-multisid-apps-marketplace-image], enter values for hello following parameters:</span></span>

  -  <span data-ttu-id="ce898-472">**SAP rendszerazonosító**. Adja meg azt szeretné, hogy tooinstall SAP rendszer hello hello SAP rendszer Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ce898-472">**Sap System Id**. Enter hello SAP system ID of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="ce898-473">hello azonosító használandó előtagjaként hello telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="ce898-473">hello ID will be used as a prefix for hello resources that are deployed.</span></span>
  -  <span data-ttu-id="ce898-474">**Operációs rendszer típusa**.</span><span class="sxs-lookup"><span data-stu-id="ce898-474">**Os Type**.</span></span> <span data-ttu-id="ce898-475">Válassza ki a hello virtuális gép operációs rendszerének hello.</span><span class="sxs-lookup"><span data-stu-id="ce898-475">Select hello operating system of hello virtual machines.</span></span>
  -  <span data-ttu-id="ce898-476">**SAP mérete**.</span><span class="sxs-lookup"><span data-stu-id="ce898-476">**Sap System Size**.</span></span> <span data-ttu-id="ce898-477">SAP hello új rendszer hello száma ad meg.</span><span class="sxs-lookup"><span data-stu-id="ce898-477">hello number of SAPS hello new system will provide.</span></span> <span data-ttu-id="ce898-478">Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.</span><span class="sxs-lookup"><span data-stu-id="ce898-478">If you are not sure how many SAPS hello system will require, ask your SAP Technology Partner or System Integrator.</span></span>
  -  <span data-ttu-id="ce898-479">**Rendelkezésre állására**.</span><span class="sxs-lookup"><span data-stu-id="ce898-479">**System Availability**.</span></span> <span data-ttu-id="ce898-480">Válassza ki **magas rendelkezésre ÁLLÁSÚ**.</span><span class="sxs-lookup"><span data-stu-id="ce898-480">Select **HA**.</span></span>
  -  <span data-ttu-id="ce898-481">**Rendszergazda felhasználónevét és a rendszergazdai jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ce898-481">**Admin Username and Admin Password**.</span></span> <span data-ttu-id="ce898-482">Hozzon létre egy új felhasználót, amely használt toosign toohello gépen.</span><span class="sxs-lookup"><span data-stu-id="ce898-482">Create a new user that can be used toosign in toohello machine.</span></span>
  -  <span data-ttu-id="ce898-483">**Alhálózati azonosító**. Adja meg a hello hello alhálózat hello hello ASC/SCS sablon telepítése során használt, vagy hello azonosítója hello alhálózat létrehozott hello ASC/SCS sablon telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="ce898-483">**Subnet Id**. Enter hello ID of hello subnet that you used during hello deployment of hello ASCS/SCS template, or hello ID of hello subnet that was created as part of hello ASCS/SCS template deployment.</span></span>


### <span data-ttu-id="ce898-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure-beli virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="ce898-484"><a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a> Azure virtual network</span></span>
<span data-ttu-id="ce898-485">A jelen példában hello hello Azure-beli virtuális hálózat címtartománya 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="ce898-485">In our example, hello address space of hello Azure virtual network is 10.0.0.0/16.</span></span> <span data-ttu-id="ce898-486">Egy alhálózat neve van **alhálózati**, a 10.0.0.0/24 címtartományt.</span><span class="sxs-lookup"><span data-stu-id="ce898-486">There is one subnet called **Subnet**, with an address range of 10.0.0.0/24.</span></span> <span data-ttu-id="ce898-487">A virtuális hálózaton található virtuális gépek és a belső terheléselosztók vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="ce898-487">All virtual machines and internal load balancers are deployed in this virtual network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ce898-488">Nem módosításokat belül hello vendég operációs rendszer toohello hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ce898-488">Don't make any changes toohello network settings inside hello guest operating system.</span></span> <span data-ttu-id="ce898-489">Ez magában foglalja az IP-címek, a DNS-kiszolgálók és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ce898-489">This includes IP addresses, DNS servers, and subnet.</span></span> <span data-ttu-id="ce898-490">A hálózati beállítások konfigurálása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce898-490">Configure all your network settings in Azure.</span></span> <span data-ttu-id="ce898-491">hello Dynamic Host Configuration Protocol (DHCP) szolgáltatás tölti ki a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ce898-491">hello Dynamic Host Configuration Protocol (DHCP) service propagates your settings.</span></span>
>
>

### <span data-ttu-id="ce898-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS-IP-címek</span><span class="sxs-lookup"><span data-stu-id="ce898-492"><a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a> DNS IP addresses</span></span>

<span data-ttu-id="ce898-493">tooset hello szükséges DNS-IP-címek, hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ce898-493">tooset hello required DNS IP addresses, do hello following steps.</span></span>

1.  <span data-ttu-id="ce898-494">Az Azure portál, a hello hello **DNS-kiszolgálók** panelen ellenőrizze, hogy a virtuális hálózat **DNS-kiszolgálók** beállítás értéke túl**egyéni DNS**.</span><span class="sxs-lookup"><span data-stu-id="ce898-494">In hello Azure portal, on hello **DNS servers** blade, make sure that your virtual network **DNS servers** option is set too**Custom DNS**.</span></span>
2.  <span data-ttu-id="ce898-495">Válassza ki a hello alapján rendelkezik hálózati beállításait.</span><span class="sxs-lookup"><span data-stu-id="ce898-495">Select your settings based on hello type of network you have.</span></span> <span data-ttu-id="ce898-496">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="ce898-496">For more information, see hello following resources:</span></span>
    * <span data-ttu-id="ce898-497">[Vállalati hálózati kapcsolat (létesítmények közötti)][planning-guide-2.2]: hello IP-címek hello a helyi DNS-kiszolgálók hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="ce898-497">[Corporate network connectivity (cross-premises)][planning-guide-2.2]: Add hello IP addresses of hello on-premises DNS servers.</span></span>  
    <span data-ttu-id="ce898-498">Kibővítheti a helyszíni DNS kiszolgálók toohello virtuális gépek Azure-ban futó.</span><span class="sxs-lookup"><span data-stu-id="ce898-498">You can extend on-premises DNS servers toohello virtual machines that are running in Azure.</span></span> <span data-ttu-id="ce898-499">A forgatókönyv, hozzáadhat hello IP-címei hello Azure hello DNS szolgáltatást futtató virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ce898-499">In that scenario, you can add hello IP addresses of hello Azure virtual machines on which you run hello DNS service.</span></span>
    * <span data-ttu-id="ce898-500">[Csak felhőalapú telepítési][planning-guide-2.1]: hello tartozó további virtuális gép telepítése virtuális hálózati példányt, amely egy DNS-kiszolgálóként szolgál.</span><span class="sxs-lookup"><span data-stu-id="ce898-500">[Cloud-only deployment][planning-guide-2.1]: Deploy an additional virtual machine in hello same Virtual Network instance that serves as a DNS server.</span></span> <span data-ttu-id="ce898-501">Adja hozzá az IP-címei hello hello Azure virtuális gépekhez, amelyek toorun DNS-szolgáltatás beállítása.</span><span class="sxs-lookup"><span data-stu-id="ce898-501">Add hello IP addresses of hello Azure virtual machines that you've set up toorun DNS service.</span></span>

    ![12. ábra: DNS-kiszolgálók konfigurálása az Azure-beli virtuális hálózathoz][sap-ha-guide-figure-3001]

    <span data-ttu-id="ce898-503">_**12. ábra:** DNS konfigurálása az Azure Virtual Network kiszolgálók_</span><span class="sxs-lookup"><span data-stu-id="ce898-503">_**Figure 12:** Configure DNS servers for Azure Virtual Network_</span></span>

  > [!NOTE]
  > <span data-ttu-id="ce898-504">Hello IP-címek hello DNS-kiszolgálók módosítja, ha szüksége van-e toorestart hello Azure virtuális gépek tooapply hello módosítása és terjesztése hello új DNS-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="ce898-504">If you change hello IP addresses of hello DNS servers, you need toorestart hello Azure virtual machines tooapply hello change and propagate hello new DNS servers.</span></span>
  >
  >

<span data-ttu-id="ce898-505">A fenti példában hello DNS-szolgáltatás telepítve és konfigurálva van a Windows virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="ce898-505">In our example, hello DNS service is installed and configured on these Windows virtual machines:</span></span>

| <span data-ttu-id="ce898-506">Virtuálisgép-szerepkör</span><span class="sxs-lookup"><span data-stu-id="ce898-506">Virtual machine role</span></span> | <span data-ttu-id="ce898-507">Virtuális gép állomásneve</span><span class="sxs-lookup"><span data-stu-id="ce898-507">Virtual machine host name</span></span> | <span data-ttu-id="ce898-508">Hálózati kártya neve</span><span class="sxs-lookup"><span data-stu-id="ce898-508">Network card name</span></span> | <span data-ttu-id="ce898-509">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="ce898-509">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ce898-510">Első DNS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ce898-510">First DNS server</span></span> |<span data-ttu-id="ce898-511">domcontr-0</span><span class="sxs-lookup"><span data-stu-id="ce898-511">domcontr-0</span></span> |<span data-ttu-id="ce898-512">PR1-nic-domcontr-0</span><span class="sxs-lookup"><span data-stu-id="ce898-512">pr1-nic-domcontr-0</span></span> |<span data-ttu-id="ce898-513">10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="ce898-513">10.0.0.10</span></span> |
| <span data-ttu-id="ce898-514">Második DNS-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ce898-514">Second DNS server</span></span> |<span data-ttu-id="ce898-515">domcontr-1</span><span class="sxs-lookup"><span data-stu-id="ce898-515">domcontr-1</span></span> |<span data-ttu-id="ce898-516">PR1-nic-domcontr-1</span><span class="sxs-lookup"><span data-stu-id="ce898-516">pr1-nic-domcontr-1</span></span> |<span data-ttu-id="ce898-517">10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="ce898-517">10.0.0.11</span></span> |

### <span data-ttu-id="ce898-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Állomásnév és a statikus IP-címek hello SAP ASC/SCS fürtözött példány és az adatbázis-kezelő fürtözött példány</span><span class="sxs-lookup"><span data-stu-id="ce898-518"><a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a> Host names and static IP addresses for hello SAP ASCS/SCS clustered instance and DBMS clustered instance</span></span>

<span data-ttu-id="ce898-519">A helyszíni telepítéshez szüksége ezek fenntartott állomásneve és IP-címek:</span><span class="sxs-lookup"><span data-stu-id="ce898-519">For on-premises deployment, you need these reserved host names and IP addresses:</span></span>

| <span data-ttu-id="ce898-520">Virtuális állomás neve szerepkör</span><span class="sxs-lookup"><span data-stu-id="ce898-520">Virtual host name role</span></span> | <span data-ttu-id="ce898-521">Virtuális állomás neve</span><span class="sxs-lookup"><span data-stu-id="ce898-521">Virtual host name</span></span> | <span data-ttu-id="ce898-522">Virtuális statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="ce898-522">Virtual static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce898-523">SAP ASC/SCS első fürt virtuális host name (kezelő)</span><span class="sxs-lookup"><span data-stu-id="ce898-523">SAP ASCS/SCS first cluster virtual host name (for cluster management)</span></span> |<span data-ttu-id="ce898-524">PR1-ASC-vir</span><span class="sxs-lookup"><span data-stu-id="ce898-524">pr1-ascs-vir</span></span> |<span data-ttu-id="ce898-525">10.0.0.42</span><span class="sxs-lookup"><span data-stu-id="ce898-525">10.0.0.42</span></span> |
| <span data-ttu-id="ce898-526">SAP ASC/SCS példány virtuális állomás neve</span><span class="sxs-lookup"><span data-stu-id="ce898-526">SAP ASCS/SCS instance virtual host name</span></span> |<span data-ttu-id="ce898-527">PR1-ASC-sap</span><span class="sxs-lookup"><span data-stu-id="ce898-527">pr1-ascs-sap</span></span> |<span data-ttu-id="ce898-528">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="ce898-528">10.0.0.43</span></span> |
| <span data-ttu-id="ce898-529">SAP DBMS második fürt virtuális állomásnevet (kezelő)</span><span class="sxs-lookup"><span data-stu-id="ce898-529">SAP DBMS second cluster virtual host name (cluster management)</span></span> |<span data-ttu-id="ce898-530">PR1-dbms-vir</span><span class="sxs-lookup"><span data-stu-id="ce898-530">pr1-dbms-vir</span></span> |<span data-ttu-id="ce898-531">10.0.0.32</span><span class="sxs-lookup"><span data-stu-id="ce898-531">10.0.0.32</span></span> |

<span data-ttu-id="ce898-532">Hello fürt létrehozásakor létrehozása hello virtuális állomásnevek **pr1-ASC-vir** és **pr1-dbms-vir** és hello társított IP-címek, maga hello-fürt kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="ce898-532">When you create hello cluster, create hello virtual host names **pr1-ascs-vir** and **pr1-dbms-vir** and hello associated IP addresses that manage hello cluster itself.</span></span> <span data-ttu-id="ce898-533">További információ toodo a, lásd: [fürtcsomópontok fürtkonfiguráció gyűjtése][sap-ha-guide-8.12.1].</span><span class="sxs-lookup"><span data-stu-id="ce898-533">For information about how toodo this, see [Collect cluster nodes in a cluster configuration][sap-ha-guide-8.12.1].</span></span>

<span data-ttu-id="ce898-534">Manuálisan is létrehozhat hello más két virtuális állomásnevek **pr1-ASC-sap** és **pr1-adatbázis-kezelő – sap**, és hello társított IP-címmel, hello DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ce898-534">You can manually create hello other two virtual host names, **pr1-ascs-sap** and **pr1-dbms-sap**, and hello associated IP addresses, on hello DNS server.</span></span> <span data-ttu-id="ce898-535">hello fürtözött SAP ASC/SCS-példány és hello fürtözött adatbázis-kezelő példány használja ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ce898-535">hello clustered SAP ASCS/SCS instance and hello clustered DBMS instance use these resources.</span></span> <span data-ttu-id="ce898-536">További információ toodo a, lásd: [hozzon létre egy virtuális nevet egy fürtözött SAP ASC/SCS példány][sap-ha-guide-9.1.1].</span><span class="sxs-lookup"><span data-stu-id="ce898-536">For information about how toodo this, see [Create a virtual host name for a clustered SAP ASCS/SCS instance][sap-ha-guide-9.1.1].</span></span>

### <span data-ttu-id="ce898-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Állítsa be a statikus IP-címek hello SAP virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="ce898-537"><a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a> Set static IP addresses for hello SAP virtual machines</span></span>
<span data-ttu-id="ce898-538">Után hello virtuális gépek toouse a fürt központi telepítése, az összes virtuális gép szükséges tooset statikus IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="ce898-538">After you deploy hello virtual machines toouse in your cluster, you need tooset static IP addresses for all virtual machines.</span></span> <span data-ttu-id="ce898-539">Ehhez hello Azure virtuális hálózat konfigurálása, és nem hello vendég operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="ce898-539">Do this in hello Azure Virtual Network configuration, and not in hello guest operating system.</span></span>

1.  <span data-ttu-id="ce898-540">Hello Azure-portálon, válassza ki **erőforráscsoport** > **hálózati kártya** > **beállítások** > **IP-cím** .</span><span class="sxs-lookup"><span data-stu-id="ce898-540">In hello Azure portal, select **Resource Group** > **Network Card** > **Settings** > **IP Address**.</span></span>
2.  <span data-ttu-id="ce898-541">A hello **IP-címek** panel alatt **hozzárendelés**, jelölje be **statikus**.</span><span class="sxs-lookup"><span data-stu-id="ce898-541">On hello **IP addresses** blade, under **Assignment**, select **Static**.</span></span> <span data-ttu-id="ce898-542">A hello **IP-cím** mezőben adja meg, hogy szeretné-e toouse hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ce898-542">In hello **IP address** box, enter hello IP address that you want toouse.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ce898-543">Hello IP-cím hello hálózati kártya módosítja, ha szüksége van-e toorestart hello Azure virtuális gépek tooapply hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="ce898-543">If you change hello IP address of hello network card, you need toorestart hello Azure virtual machines tooapply hello change.</span></span>  
  >
  >

  ![13. ábra: Állítsa be a statikus IP-címeket az egyes virtuális gépek hello hálózati kártya][sap-ha-guide-figure-3002]

  <span data-ttu-id="ce898-545">_**13. ábra:** hello hálózati kártya minden virtuális gép statikus IP-címek beállítása_</span><span class="sxs-lookup"><span data-stu-id="ce898-545">_**Figure 13:** Set static IP addresses for hello network card of each virtual machine_</span></span>

  <span data-ttu-id="ce898-546">Ismételje meg ezt a lépést minden hálózati interfészen esetén ez azt jelenti, hogy az összes virtuális gép, beleértve a virtuális gépek toouse szeretné az Active Directory és a DNS szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ce898-546">Repeat this step for all network interfaces, that is, for all virtual machines, including virtual machines that you want toouse for your Active Directory/DNS service.</span></span>

<span data-ttu-id="ce898-547">A jelen példában vezetünk be a virtuális gépek és a statikus IP-címek:</span><span class="sxs-lookup"><span data-stu-id="ce898-547">In our example, we have these virtual machines and static IP addresses:</span></span>

| <span data-ttu-id="ce898-548">Virtuálisgép-szerepkör</span><span class="sxs-lookup"><span data-stu-id="ce898-548">Virtual machine role</span></span> | <span data-ttu-id="ce898-549">Virtuális gép állomásneve</span><span class="sxs-lookup"><span data-stu-id="ce898-549">Virtual machine host name</span></span> | <span data-ttu-id="ce898-550">Hálózati kártya neve</span><span class="sxs-lookup"><span data-stu-id="ce898-550">Network card name</span></span> | <span data-ttu-id="ce898-551">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="ce898-551">Static IP address</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ce898-552">Első SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="ce898-552">First SAP Application Server instance</span></span> |<span data-ttu-id="ce898-553">PR1-di-0</span><span class="sxs-lookup"><span data-stu-id="ce898-553">pr1-di-0</span></span> |<span data-ttu-id="ce898-554">PR1-nic-di-0</span><span class="sxs-lookup"><span data-stu-id="ce898-554">pr1-nic-di-0</span></span> |<span data-ttu-id="ce898-555">10.0.0.50</span><span class="sxs-lookup"><span data-stu-id="ce898-555">10.0.0.50</span></span> |
| <span data-ttu-id="ce898-556">Második SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="ce898-556">Second SAP Application Server instance</span></span> |<span data-ttu-id="ce898-557">PR1-di-1</span><span class="sxs-lookup"><span data-stu-id="ce898-557">pr1-di-1</span></span> |<span data-ttu-id="ce898-558">PR1-nic-di-1</span><span class="sxs-lookup"><span data-stu-id="ce898-558">pr1-nic-di-1</span></span> |<span data-ttu-id="ce898-559">10.0.0.51</span><span class="sxs-lookup"><span data-stu-id="ce898-559">10.0.0.51</span></span> |
| <span data-ttu-id="ce898-560">...</span><span class="sxs-lookup"><span data-stu-id="ce898-560">...</span></span> |<span data-ttu-id="ce898-561">...</span><span class="sxs-lookup"><span data-stu-id="ce898-561">...</span></span> |<span data-ttu-id="ce898-562">...</span><span class="sxs-lookup"><span data-stu-id="ce898-562">...</span></span> |<span data-ttu-id="ce898-563">...</span><span class="sxs-lookup"><span data-stu-id="ce898-563">...</span></span> |
| <span data-ttu-id="ce898-564">Utolsó SAP Application Server-példány</span><span class="sxs-lookup"><span data-stu-id="ce898-564">Last SAP Application Server instance</span></span> |<span data-ttu-id="ce898-565">PR1-di-5</span><span class="sxs-lookup"><span data-stu-id="ce898-565">pr1-di-5</span></span> |<span data-ttu-id="ce898-566">PR1-nic-di-5</span><span class="sxs-lookup"><span data-stu-id="ce898-566">pr1-nic-di-5</span></span> |<span data-ttu-id="ce898-567">10.0.0.55</span><span class="sxs-lookup"><span data-stu-id="ce898-567">10.0.0.55</span></span> |
| <span data-ttu-id="ce898-568">Első fürtcsomópontra ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="ce898-568">First cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="ce898-569">PR1-ASC-0</span><span class="sxs-lookup"><span data-stu-id="ce898-569">pr1-ascs-0</span></span> |<span data-ttu-id="ce898-570">PR1-nic-ASC-0</span><span class="sxs-lookup"><span data-stu-id="ce898-570">pr1-nic-ascs-0</span></span> |<span data-ttu-id="ce898-571">10.0.0.40</span><span class="sxs-lookup"><span data-stu-id="ce898-571">10.0.0.40</span></span> |
| <span data-ttu-id="ce898-572">Második fürtcsomópont ASC/SCS-példány</span><span class="sxs-lookup"><span data-stu-id="ce898-572">Second cluster node for ASCS/SCS instance</span></span> |<span data-ttu-id="ce898-573">PR1-ASC-1</span><span class="sxs-lookup"><span data-stu-id="ce898-573">pr1-ascs-1</span></span> |<span data-ttu-id="ce898-574">PR1-nic-ASC-1</span><span class="sxs-lookup"><span data-stu-id="ce898-574">pr1-nic-ascs-1</span></span> |<span data-ttu-id="ce898-575">10.0.0.41</span><span class="sxs-lookup"><span data-stu-id="ce898-575">10.0.0.41</span></span> |
| <span data-ttu-id="ce898-576">Adatbázis-kezelő példány első fürtcsomópontra</span><span class="sxs-lookup"><span data-stu-id="ce898-576">First cluster node for DBMS instance</span></span> |<span data-ttu-id="ce898-577">PR1-db-0</span><span class="sxs-lookup"><span data-stu-id="ce898-577">pr1-db-0</span></span> |<span data-ttu-id="ce898-578">PR1-nic-db-0</span><span class="sxs-lookup"><span data-stu-id="ce898-578">pr1-nic-db-0</span></span> |<span data-ttu-id="ce898-579">10.0.0.30</span><span class="sxs-lookup"><span data-stu-id="ce898-579">10.0.0.30</span></span> |
| <span data-ttu-id="ce898-580">Adatbázis-kezelő példány második fürtcsomópont</span><span class="sxs-lookup"><span data-stu-id="ce898-580">Second cluster node for DBMS instance</span></span> |<span data-ttu-id="ce898-581">PR1-db-1</span><span class="sxs-lookup"><span data-stu-id="ce898-581">pr1-db-1</span></span> |<span data-ttu-id="ce898-582">PR1-nic-db-1</span><span class="sxs-lookup"><span data-stu-id="ce898-582">pr1-nic-db-1</span></span> |<span data-ttu-id="ce898-583">10.0.0.31</span><span class="sxs-lookup"><span data-stu-id="ce898-583">10.0.0.31</span></span> |

### <span data-ttu-id="ce898-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Egy statikus IP-cím beállítása hello Azure belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="ce898-584"><a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a> Set a static IP address for hello Azure internal load balancer</span></span>

<span data-ttu-id="ce898-585">hello SAP Azure Resource Manager sablonnal hoz létre az Azure belső terheléselosztó hello SAP ASC/SCS példány és hello DBMS fürtön használt.</span><span class="sxs-lookup"><span data-stu-id="ce898-585">hello SAP Azure Resource Manager template creates an Azure internal load balancer that is used for hello SAP ASCS/SCS instance cluster and hello DBMS cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ce898-586">hello hello virtuális állomásnevét hello SAP ASC/SCS az IP-címe hello azonos hello IP-címet az SAP ASC/SCS belső terheléselosztó hello: **pr1-lb-ASC**.</span><span class="sxs-lookup"><span data-stu-id="ce898-586">hello IP address of hello virtual host name of hello SAP ASCS/SCS is hello same as hello IP address of hello SAP ASCS/SCS internal load balancer: **pr1-lb-ascs**.</span></span>
> <span data-ttu-id="ce898-587">hello hello virtuális nevét az adatbázis-kezelő rendszer hello IP-címe hello ugyanazt az adatbázis-kezelő belső terheléselosztó hello hello IP-címként: **pr1-lb-dbms**.</span><span class="sxs-lookup"><span data-stu-id="ce898-587">hello IP address of hello virtual name of hello DBMS is hello same as hello IP address of hello DBMS internal load balancer: **pr1-lb-dbms**.</span></span>
>
>

<span data-ttu-id="ce898-588">tooset egy statikus IP-címet hello Azure belső terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="ce898-588">tooset a static IP address for hello Azure internal load balancer:</span></span>

1.  <span data-ttu-id="ce898-589">hello kezdeti telepítési beállítja hello belső terheléselosztó IP-cím túl**dinamikus**.</span><span class="sxs-lookup"><span data-stu-id="ce898-589">hello initial deployment sets hello internal load balancer IP address too**Dynamic**.</span></span> <span data-ttu-id="ce898-590">Az Azure portál, a hello hello **IP-címek** panel alatt **hozzárendelés**, jelölje be **statikus**.</span><span class="sxs-lookup"><span data-stu-id="ce898-590">In hello Azure portal, on hello **IP addresses** blade, under **Assignment**, select **Static**.</span></span>
2.  <span data-ttu-id="ce898-591">Belső terheléselosztó hello hello IP-cím beállítása **pr1-lb-ASC** hello virtuális állomásnevet hello SAP ASC/SCS példány toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ce898-591">Set hello IP address of hello internal load balancer **pr1-lb-ascs** toohello IP address of hello virtual host name of hello SAP ASCS/SCS instance.</span></span>
3.  <span data-ttu-id="ce898-592">Belső terheléselosztó hello hello IP-cím beállítása **pr1-lb-dbms** hello virtuális állomásnevet hello DBMS példány toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ce898-592">Set hello IP address of hello internal load balancer **pr1-lb-dbms** toohello IP address of hello virtual host name of hello DBMS instance.</span></span>

  ![14. ábra: Hello belső terheléselosztóhoz hello SAP ASC/SCS-példány beállítása a statikus IP-címek][sap-ha-guide-figure-3003]

  <span data-ttu-id="ce898-594">_**14. ábra:** statikus IP-címek hello belső terheléselosztóhoz hello SAP ASC/SCS-példány beállítása_</span><span class="sxs-lookup"><span data-stu-id="ce898-594">_**Figure 14:** Set static IP addresses for hello internal load balancer for hello SAP ASCS/SCS instance_</span></span>

<span data-ttu-id="ce898-595">Ebben a példában két Azure belső terheléselosztók a statikus IP-címmel rendelkező vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="ce898-595">In our example, we have two Azure internal load balancers that have these static IP addresses:</span></span>

| <span data-ttu-id="ce898-596">Az Azure belső terheléselosztási szerepköréhez</span><span class="sxs-lookup"><span data-stu-id="ce898-596">Azure internal load balancer role</span></span> | <span data-ttu-id="ce898-597">Az Azure belső terheléselosztó neve</span><span class="sxs-lookup"><span data-stu-id="ce898-597">Azure internal load balancer name</span></span> | <span data-ttu-id="ce898-598">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="ce898-598">Static IP address</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce898-599">SAP ASC/SCS példány belső terheléselosztót</span><span class="sxs-lookup"><span data-stu-id="ce898-599">SAP ASCS/SCS instance internal load balancer</span></span> |<span data-ttu-id="ce898-600">PR1-lb-ASC</span><span class="sxs-lookup"><span data-stu-id="ce898-600">pr1-lb-ascs</span></span> |<span data-ttu-id="ce898-601">10.0.0.43</span><span class="sxs-lookup"><span data-stu-id="ce898-601">10.0.0.43</span></span> |
| <span data-ttu-id="ce898-602">SAP DBMS belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ce898-602">SAP DBMS internal load balancer</span></span> |<span data-ttu-id="ce898-603">PR1-lb-adatbázis-kezelő</span><span class="sxs-lookup"><span data-stu-id="ce898-603">pr1-lb-dbms</span></span> |<span data-ttu-id="ce898-604">10.0.0.33</span><span class="sxs-lookup"><span data-stu-id="ce898-604">10.0.0.33</span></span> |


### <span data-ttu-id="ce898-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Alapértelmezett ASC/SCS terheléselosztási szabályok hello Azure belső terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="ce898-605"><a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a> Default ASCS/SCS load balancing rules for hello Azure internal load balancer</span></span>

<span data-ttu-id="ce898-606">hello SAP Azure Resource Manager-sablon létrehozza a hello portok:</span><span class="sxs-lookup"><span data-stu-id="ce898-606">hello SAP Azure Resource Manager template creates hello ports you need:</span></span>
* <span data-ttu-id="ce898-607">ABAP ASC példány, hello alapértelmezett példányszámának **00**</span><span class="sxs-lookup"><span data-stu-id="ce898-607">An ABAP ASCS instance, with hello default instance number **00**</span></span>
* <span data-ttu-id="ce898-608">A Java SCS példány, hello alapértelmezett példányszámának **01**</span><span class="sxs-lookup"><span data-stu-id="ce898-608">A Java SCS instance, with hello default instance number **01**</span></span>

<span data-ttu-id="ce898-609">A SAP ASC/SCS példányát telepítésekor használnia kell a hello alapértelmezett példányszámának **00** a ABAP ASC példány és hello alapértelmezett példány számára vonatkozó **01** a Java SCS-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="ce898-609">When you install your SAP ASCS/SCS instance, you must use hello default instance number **00** for your ABAP ASCS instance and hello default instance number **01** for your Java SCS instance.</span></span>

<span data-ttu-id="ce898-610">Ezután hozzon létre a szükséges belső terheléselosztási végpontok hello SAP NetWeaver portok.</span><span class="sxs-lookup"><span data-stu-id="ce898-610">Next, create required internal load balancing endpoints for hello SAP NetWeaver ports.</span></span>

<span data-ttu-id="ce898-611">toocreate szükséges belső terheléselosztási végpontok, először hozzon létre a terheléselosztási végpontok hello SAP NetWeaver ABAP ASC portok:</span><span class="sxs-lookup"><span data-stu-id="ce898-611">toocreate required internal load balancing endpoints, first, create these load balancing endpoints for hello SAP NetWeaver ABAP ASCS ports:</span></span>

| <span data-ttu-id="ce898-612">Szolgáltatás/terheléselosztási szabály neve</span><span class="sxs-lookup"><span data-stu-id="ce898-612">Service/load balancing rule name</span></span> | <span data-ttu-id="ce898-613">Alapértelmezett portszámok</span><span class="sxs-lookup"><span data-stu-id="ce898-613">Default port numbers</span></span> | <span data-ttu-id="ce898-614">Konkrét portok (példányszámának 00 példány ASC) (SSZON 10)</span><span class="sxs-lookup"><span data-stu-id="ce898-614">Concrete ports for (ASCS instance with instance number 00) (ERS with 10)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce898-615">Sorba helyezni Server / *lbrule3200*</span><span class="sxs-lookup"><span data-stu-id="ce898-615">Enqueue Server / *lbrule3200*</span></span> |<span data-ttu-id="ce898-616">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-616">32<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-617">3200</span><span class="sxs-lookup"><span data-stu-id="ce898-617">3200</span></span> |
| <span data-ttu-id="ce898-618">ABAP üzenet Server / *lbrule3600*</span><span class="sxs-lookup"><span data-stu-id="ce898-618">ABAP Message Server / *lbrule3600*</span></span> |<span data-ttu-id="ce898-619">36 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-619">36<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-620">3600</span><span class="sxs-lookup"><span data-stu-id="ce898-620">3600</span></span> |
| <span data-ttu-id="ce898-621">Belső ABAP üzenet / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="ce898-621">Internal ABAP Message / *lbrule3900*</span></span> |<span data-ttu-id="ce898-622">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-622">39<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-623">3900</span><span class="sxs-lookup"><span data-stu-id="ce898-623">3900</span></span> |
| <span data-ttu-id="ce898-624">A kiszolgáló HTTP-üzenet / *Lbrule8100*</span><span class="sxs-lookup"><span data-stu-id="ce898-624">Message Server HTTP / *Lbrule8100*</span></span> |<span data-ttu-id="ce898-625">81-es <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-625">81<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-626">8100</span><span class="sxs-lookup"><span data-stu-id="ce898-626">8100</span></span> |
| <span data-ttu-id="ce898-627">SAP Start ASC a HTTP / *Lbrule50013*</span><span class="sxs-lookup"><span data-stu-id="ce898-627">SAP Start Service ASCS HTTP / *Lbrule50013*</span></span> |<span data-ttu-id="ce898-628">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="ce898-628">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="ce898-629">50013</span><span class="sxs-lookup"><span data-stu-id="ce898-629">50013</span></span> |
| <span data-ttu-id="ce898-630">SAP Start szolgáltatás ASC HTTPS / *Lbrule50014*</span><span class="sxs-lookup"><span data-stu-id="ce898-630">SAP Start Service ASCS HTTPS / *Lbrule50014*</span></span> |<span data-ttu-id="ce898-631">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="ce898-631">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="ce898-632">50014</span><span class="sxs-lookup"><span data-stu-id="ce898-632">50014</span></span> |
| <span data-ttu-id="ce898-633">Sorba helyezni replikációs / *Lbrule50016*</span><span class="sxs-lookup"><span data-stu-id="ce898-633">Enqueue Replication / *Lbrule50016*</span></span> |<span data-ttu-id="ce898-634">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="ce898-634">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="ce898-635">50016</span><span class="sxs-lookup"><span data-stu-id="ce898-635">50016</span></span> |
| <span data-ttu-id="ce898-636">SAP Start SSZON HTTP-kérelmekre *Lbrule51013*</span><span class="sxs-lookup"><span data-stu-id="ce898-636">SAP Start Service ERS HTTP *Lbrule51013*</span></span> |<span data-ttu-id="ce898-637">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="ce898-637">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="ce898-638">51013</span><span class="sxs-lookup"><span data-stu-id="ce898-638">51013</span></span> |
| <span data-ttu-id="ce898-639">SAP Start SSZON HTTP-kérelmekre *Lbrule51014*</span><span class="sxs-lookup"><span data-stu-id="ce898-639">SAP Start Service ERS HTTP *Lbrule51014*</span></span> |<span data-ttu-id="ce898-640">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="ce898-640">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="ce898-641">51014</span><span class="sxs-lookup"><span data-stu-id="ce898-641">51014</span></span> |
| <span data-ttu-id="ce898-642">Erőforrás-kezelő Win *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="ce898-642">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="ce898-643">5985</span><span class="sxs-lookup"><span data-stu-id="ce898-643">5985</span></span> |
| <span data-ttu-id="ce898-644">Fájlmegosztás *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="ce898-644">File Share *Lbrule445*</span></span> | |<span data-ttu-id="ce898-645">445</span><span class="sxs-lookup"><span data-stu-id="ce898-645">445</span></span> |

<span data-ttu-id="ce898-646">_**1. táblázat:** portszámokat hello SAP NetWeaver ABAP ASC példányok_</span><span class="sxs-lookup"><span data-stu-id="ce898-646">_**Table 1:** Port numbers of hello SAP NetWeaver ABAP ASCS instances_</span></span>

<span data-ttu-id="ce898-647">Ezután hozzon létre a terheléselosztási végpontok hello SAP NetWeaver Java SCS portok:</span><span class="sxs-lookup"><span data-stu-id="ce898-647">Then, create these load balancing endpoints for hello SAP NetWeaver Java SCS ports:</span></span>

| <span data-ttu-id="ce898-648">Szolgáltatás/terheléselosztási szabály neve</span><span class="sxs-lookup"><span data-stu-id="ce898-648">Service/load balancing rule name</span></span> | <span data-ttu-id="ce898-649">Alapértelmezett portszámok</span><span class="sxs-lookup"><span data-stu-id="ce898-649">Default port numbers</span></span> | <span data-ttu-id="ce898-650">Konkrét portok (SCS példány példányszámának 01) (SSZON 11)</span><span class="sxs-lookup"><span data-stu-id="ce898-650">Concrete ports for (SCS instance with instance number 01) (ERS with 11)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce898-651">Sorba helyezni Server / *lbrule3201*</span><span class="sxs-lookup"><span data-stu-id="ce898-651">Enqueue Server / *lbrule3201*</span></span> |<span data-ttu-id="ce898-652">32 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-652">32<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-653">3201</span><span class="sxs-lookup"><span data-stu-id="ce898-653">3201</span></span> |
| <span data-ttu-id="ce898-654">Átjárókiszolgáló / *lbrule3301*</span><span class="sxs-lookup"><span data-stu-id="ce898-654">Gateway Server / *lbrule3301*</span></span> |<span data-ttu-id="ce898-655">33 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-655">33<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-656">3301</span><span class="sxs-lookup"><span data-stu-id="ce898-656">3301</span></span> |
| <span data-ttu-id="ce898-657">Java-üzenet Server / *lbrule3900*</span><span class="sxs-lookup"><span data-stu-id="ce898-657">Java Message Server / *lbrule3900*</span></span> |<span data-ttu-id="ce898-658">39 <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-658">39<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-659">3901</span><span class="sxs-lookup"><span data-stu-id="ce898-659">3901</span></span> |
| <span data-ttu-id="ce898-660">A kiszolgáló HTTP-üzenet / *Lbrule8101*</span><span class="sxs-lookup"><span data-stu-id="ce898-660">Message Server HTTP / *Lbrule8101*</span></span> |<span data-ttu-id="ce898-661">81-es <*InstanceNumber*></span><span class="sxs-lookup"><span data-stu-id="ce898-661">81<*InstanceNumber*></span></span> |<span data-ttu-id="ce898-662">8101</span><span class="sxs-lookup"><span data-stu-id="ce898-662">8101</span></span> |
| <span data-ttu-id="ce898-663">SAP Start SCS a HTTP / *Lbrule50113*</span><span class="sxs-lookup"><span data-stu-id="ce898-663">SAP Start Service SCS HTTP / *Lbrule50113*</span></span> |<span data-ttu-id="ce898-664">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="ce898-664">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="ce898-665">50113</span><span class="sxs-lookup"><span data-stu-id="ce898-665">50113</span></span> |
| <span data-ttu-id="ce898-666">SAP Start szolgáltatás SCS HTTPS / *Lbrule50114*</span><span class="sxs-lookup"><span data-stu-id="ce898-666">SAP Start Service SCS HTTPS / *Lbrule50114*</span></span> |<span data-ttu-id="ce898-667">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="ce898-667">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="ce898-668">50114</span><span class="sxs-lookup"><span data-stu-id="ce898-668">50114</span></span> |
| <span data-ttu-id="ce898-669">Sorba helyezni replikációs / *Lbrule50116*</span><span class="sxs-lookup"><span data-stu-id="ce898-669">Enqueue Replication / *Lbrule50116*</span></span> |<span data-ttu-id="ce898-670">5 <*InstanceNumber*> 16</span><span class="sxs-lookup"><span data-stu-id="ce898-670">5<*InstanceNumber*>16</span></span> |<span data-ttu-id="ce898-671">50116</span><span class="sxs-lookup"><span data-stu-id="ce898-671">50116</span></span> |
| <span data-ttu-id="ce898-672">SAP Start SSZON HTTP-kérelmekre *Lbrule51113*</span><span class="sxs-lookup"><span data-stu-id="ce898-672">SAP Start Service ERS HTTP *Lbrule51113*</span></span> |<span data-ttu-id="ce898-673">5 <*InstanceNumber*> 13</span><span class="sxs-lookup"><span data-stu-id="ce898-673">5<*InstanceNumber*>13</span></span> |<span data-ttu-id="ce898-674">51113</span><span class="sxs-lookup"><span data-stu-id="ce898-674">51113</span></span> |
| <span data-ttu-id="ce898-675">SAP Start SSZON HTTP-kérelmekre *Lbrule51114*</span><span class="sxs-lookup"><span data-stu-id="ce898-675">SAP Start Service ERS HTTP *Lbrule51114*</span></span> |<span data-ttu-id="ce898-676">5 <*InstanceNumber*> 14</span><span class="sxs-lookup"><span data-stu-id="ce898-676">5<*InstanceNumber*>14</span></span> |<span data-ttu-id="ce898-677">51114</span><span class="sxs-lookup"><span data-stu-id="ce898-677">51114</span></span> |
| <span data-ttu-id="ce898-678">Erőforrás-kezelő Win *Lbrule5985*</span><span class="sxs-lookup"><span data-stu-id="ce898-678">Win RM *Lbrule5985*</span></span> | |<span data-ttu-id="ce898-679">5985</span><span class="sxs-lookup"><span data-stu-id="ce898-679">5985</span></span> |
| <span data-ttu-id="ce898-680">Fájlmegosztás *Lbrule445*</span><span class="sxs-lookup"><span data-stu-id="ce898-680">File Share *Lbrule445*</span></span> | |<span data-ttu-id="ce898-681">445</span><span class="sxs-lookup"><span data-stu-id="ce898-681">445</span></span> |

<span data-ttu-id="ce898-682">_**2. táblázat:** portszámokat hello SAP NetWeaver Java SCS-példányok_</span><span class="sxs-lookup"><span data-stu-id="ce898-682">_**Table 2:** Port numbers of hello SAP NetWeaver Java SCS instances_</span></span>

![15. ábra: Alapértelmezett ASC/SCS terheléselosztási szabályok hello Azure belső terheléselosztóhoz][sap-ha-guide-figure-3004]

<span data-ttu-id="ce898-684">_**15. ábra:** alapértelmezett ASC/SCS terheléselosztási szabályok hello Azure belső terheléselosztóhoz_</span><span class="sxs-lookup"><span data-stu-id="ce898-684">_**Figure 15:** Default ASCS/SCS load balancing rules for hello Azure internal load balancer_</span></span>

<span data-ttu-id="ce898-685">Hello terheléselosztó hello IP-cím beállítása **pr1-lb-dbms** hello virtuális állomásnevet hello DBMS példány toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ce898-685">Set hello IP address of hello load balancer **pr1-lb-dbms** toohello IP address of hello virtual host name of hello DBMS instance.</span></span>

### <span data-ttu-id="ce898-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Hello ASC/SCS alapértelmezett terheléselosztási hello Azure belső terheléselosztóhoz tartozó szabályok módosítása</span><span class="sxs-lookup"><span data-stu-id="ce898-686"><a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a> Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer</span></span>

<span data-ttu-id="ce898-687">Ha toouse eltérő számú SAP-ASC hello vagy SCS példányok, meg kell változtatnia hello neveit és értékeit a portok alapértelmezett értékekhez.</span><span class="sxs-lookup"><span data-stu-id="ce898-687">If you want toouse different numbers for hello SAP ASCS or SCS instances, you must change hello names and values of their ports from default values.</span></span>

1.  <span data-ttu-id="ce898-688">Hello Azure-portálon, válassza ki  **<* SID*> - lb - ASC betöltése terheléselosztó ** > **terheléselosztási szabályok betöltése**.</span><span class="sxs-lookup"><span data-stu-id="ce898-688">In hello Azure portal, select **<*SID*>-lb-ascs load balancer** > **Load Balancing Rules**.</span></span>
2.  <span data-ttu-id="ce898-689">Minden terheléselosztási szabályok, amelyek toohello SAP ASC vagy SCS példány tartozik ezek az értékek módosítása:</span><span class="sxs-lookup"><span data-stu-id="ce898-689">For all load balancing rules that belong toohello SAP ASCS or SCS instance, change these values:</span></span>

  * <span data-ttu-id="ce898-690">Név</span><span class="sxs-lookup"><span data-stu-id="ce898-690">Name</span></span>
  * <span data-ttu-id="ce898-691">Port</span><span class="sxs-lookup"><span data-stu-id="ce898-691">Port</span></span>
  * <span data-ttu-id="ce898-692">Háttér-port</span><span class="sxs-lookup"><span data-stu-id="ce898-692">Back-end port</span></span>

  <span data-ttu-id="ce898-693">Például ha azt szeretné, hogy toochange hello alapértelmezett ASC példány szám 00 too31, kell toomake hello módosításokat minden porthoz az 1.</span><span class="sxs-lookup"><span data-stu-id="ce898-693">For example, if you want toochange hello default ASCS instance number from 00 too31, you need toomake hello changes for all ports listed in Table 1.</span></span>

  <span data-ttu-id="ce898-694">Példa port frissítési *lbrule3200*.</span><span class="sxs-lookup"><span data-stu-id="ce898-694">Here's an example of an update for port *lbrule3200*.</span></span>

  ![16. ábra: Hello ASC/SCS alapértelmezett terheléselosztási hello Azure belső terheléselosztóhoz tartozó szabályok módosítása][sap-ha-guide-figure-3005]

  <span data-ttu-id="ce898-696">_**16. ábra:** módosítás hello ASC/SCS alapértelmezett terheléselosztási szabályok hello Azure belső terheléselosztóhoz_</span><span class="sxs-lookup"><span data-stu-id="ce898-696">_**Figure 16:** Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer_</span></span>

### <span data-ttu-id="ce898-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuális gépek toohello tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce898-697"><a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a> Add Windows virtual machines toohello domain</span></span>

<span data-ttu-id="ce898-698">Miután hozzárendelt egy statikus IP cím toohello virtuális gépek, hello virtuális gépek toohello tartomány hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="ce898-698">After you assign a static IP address toohello virtual machines, add hello virtual machines toohello domain.</span></span>

![17. ábra: A virtuális gép tooa tartomány hozzáadása][sap-ha-guide-figure-3006]

<span data-ttu-id="ce898-700">_**17. ábra:** tartomány hozzáadása a virtuális gép tooa_</span><span class="sxs-lookup"><span data-stu-id="ce898-700">_**Figure 17:** Add a virtual machine tooa domain_</span></span>

### <span data-ttu-id="ce898-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Adja hozzá a beállításjegyzék-bejegyzések hello SAP ASC/SCS példány mindkét fürtcsomóponton</span><span class="sxs-lookup"><span data-stu-id="ce898-701"><a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a> Add registry entries on both cluster nodes of hello SAP ASCS/SCS instance</span></span>

<span data-ttu-id="ce898-702">Az Azure terheléselosztó, hogy a kapcsolatok bezárása, amikor hello kapcsolat üresjáratban a megadott ideig (üresjárati időkorlátot) idő belső terheléselosztót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ce898-702">Azure Load Balancer has an internal load balancer that closes connections when hello connections are idle for a set period of time (an idle timeout).</span></span> <span data-ttu-id="ce898-703">SAP munkafolyamatok párbeszédpanel példányok nyitott kapcsolatok toohello SAP sorba helyezni a, amint hello első sorba helyezni/created igények toobe küldött kérelmek feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="ce898-703">SAP work processes in dialog instances open connections toohello SAP enqueue process as soon as hello first enqueue/dequeue request needs toobe sent.</span></span> <span data-ttu-id="ce898-704">Ezek a kapcsolatok általában marad a meghatározott hello munkahelyi folyamat vagy hello sorba helyezni folyamat újraindítja.</span><span class="sxs-lookup"><span data-stu-id="ce898-704">These connections usually remain established until hello work process or hello enqueue process restarts.</span></span> <span data-ttu-id="ce898-705">Ha a beállított időn hello kapcsolat üresjáratban, hello Azure belső terheléselosztási terheléselosztó bezárása hello kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="ce898-705">However, if hello connection is idle for a set period of time, hello Azure internal load balancer closes hello connections.</span></span> <span data-ttu-id="ce898-706">Ez nem hiba, mert hello SAP munkahelyi folyamat helyreállítja a csatolást hello kapcsolódási toohello sorba helyezni folyamatot, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ce898-706">This isn't a problem because hello SAP work process reestablishes hello connection toohello enqueue process if it no longer exists.</span></span> <span data-ttu-id="ce898-707">Ezek a tevékenységek hello fejlesztői nyomkövetések SAP folyamatok vannak dokumentálva, de ezeket a nyomkövetéseket a felesleges tartalmat nagy mennyiségű hoznak létre.</span><span class="sxs-lookup"><span data-stu-id="ce898-707">These activities are documented in hello developer traces of SAP processes, but they create a large amount of extra content in those traces.</span></span> <span data-ttu-id="ce898-708">Egy jó ötlet toochange hello TCP/IP `KeepAliveTime` és `KeepAliveInterval` mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="ce898-708">It's a good idea toochange hello TCP/IP `KeepAliveTime` and `KeepAliveInterval` on both cluster nodes.</span></span> <span data-ttu-id="ce898-709">A módosításokat a hello TCP/IP-paraméterek SAP profil paraméterekkel, hello cikk későbbi részében leírt össze.</span><span class="sxs-lookup"><span data-stu-id="ce898-709">Combine these changes in hello TCP/IP parameters with SAP profile parameters, described later in hello article.</span></span>

<span data-ttu-id="ce898-710">beállításjegyzék-bejegyzések tooadd mindkét fürtcsomóponton hello SAP ASC/SCS példány, először adja hozzá a Windows beállításjegyzék-bejegyzések mindkét Windows fürtcsomópontokon az SAP ASC/SCS:</span><span class="sxs-lookup"><span data-stu-id="ce898-710">tooadd registry entries on both cluster nodes of hello SAP ASCS/SCS instance, first, add these Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="ce898-711">Elérési út</span><span class="sxs-lookup"><span data-stu-id="ce898-711">Path</span></span> | <span data-ttu-id="ce898-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="ce898-712">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="ce898-713">Változó neve</span><span class="sxs-lookup"><span data-stu-id="ce898-713">Variable name</span></span> |`KeepAliveTime` |
| <span data-ttu-id="ce898-714">Változó típusa</span><span class="sxs-lookup"><span data-stu-id="ce898-714">Variable type</span></span> |<span data-ttu-id="ce898-715">REG_DWORD (decimális)</span><span class="sxs-lookup"><span data-stu-id="ce898-715">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="ce898-716">Érték</span><span class="sxs-lookup"><span data-stu-id="ce898-716">Value</span></span> |<span data-ttu-id="ce898-717">120000</span><span class="sxs-lookup"><span data-stu-id="ce898-717">120000</span></span> |
| <span data-ttu-id="ce898-718">Hivatkozás toodocumentation</span><span class="sxs-lookup"><span data-stu-id="ce898-718">Link toodocumentation</span></span> |[<span data-ttu-id="ce898-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span><span class="sxs-lookup"><span data-stu-id="ce898-719">https://technet.microsoft.com/en-us/library/cc957549.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

<span data-ttu-id="ce898-720">_**3. táblázat:** módosítás hello első TCP/IP-paraméter_</span><span class="sxs-lookup"><span data-stu-id="ce898-720">_**Table 3:** Change hello first TCP/IP parameter_</span></span>

<span data-ttu-id="ce898-721">Ezt követően adja hozzá a Windows beállításjegyzék-bejegyzések mindkét Windows fürtcsomópontokon az SAP ASC/SCS:</span><span class="sxs-lookup"><span data-stu-id="ce898-721">Then, add this Windows registry entries on both Windows cluster nodes for SAP ASCS/SCS:</span></span>

| <span data-ttu-id="ce898-722">Elérési út</span><span class="sxs-lookup"><span data-stu-id="ce898-722">Path</span></span> | <span data-ttu-id="ce898-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span><span class="sxs-lookup"><span data-stu-id="ce898-723">HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters</span></span> |
| --- | --- |
| <span data-ttu-id="ce898-724">Változó neve</span><span class="sxs-lookup"><span data-stu-id="ce898-724">Variable name</span></span> |`KeepAliveInterval` |
| <span data-ttu-id="ce898-725">Változó típusa</span><span class="sxs-lookup"><span data-stu-id="ce898-725">Variable type</span></span> |<span data-ttu-id="ce898-726">REG_DWORD (decimális)</span><span class="sxs-lookup"><span data-stu-id="ce898-726">REG_DWORD (Decimal)</span></span> |
| <span data-ttu-id="ce898-727">Érték</span><span class="sxs-lookup"><span data-stu-id="ce898-727">Value</span></span> |<span data-ttu-id="ce898-728">120000</span><span class="sxs-lookup"><span data-stu-id="ce898-728">120000</span></span> |
| <span data-ttu-id="ce898-729">Hivatkozás toodocumentation</span><span class="sxs-lookup"><span data-stu-id="ce898-729">Link toodocumentation</span></span> |[<span data-ttu-id="ce898-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span><span class="sxs-lookup"><span data-stu-id="ce898-730">https://technet.microsoft.com/en-us/library/cc957548.aspx</span></span>](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

<span data-ttu-id="ce898-731">_**4. táblázat:** módosítás hello második TCP/IP-paraméter_</span><span class="sxs-lookup"><span data-stu-id="ce898-731">_**Table 4:** Change hello second TCP/IP parameter_</span></span>

<span data-ttu-id="ce898-732">**tooapply hello módosításokat, indítsa újra mindkét fürtcsomópontot**.</span><span class="sxs-lookup"><span data-stu-id="ce898-732">**tooapply hello changes, restart both cluster nodes**.</span></span>

### <span data-ttu-id="ce898-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Egy SAP ASC/SCS példánya számára a Windows Server feladatátvételi fürtszolgáltatási fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="ce898-733"><a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a> Set up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance</span></span>

<span data-ttu-id="ce898-734">A Windows Server feladatátvételi fürtszolgáltatási fürt egy SAP ASC/SCS-példány beállítása magában foglalja a ezeket a feladatokat:</span><span class="sxs-lookup"><span data-stu-id="ce898-734">Setting up a Windows Server Failover Clustering cluster for an SAP ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="ce898-735">Fürtkonfiguráció hello fürtcsomópontok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="ce898-735">Collecting hello cluster nodes in a cluster configuration</span></span>
- <span data-ttu-id="ce898-736">A fürt tanúsító fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="ce898-736">Configuring a cluster file share witness</span></span>

#### <span data-ttu-id="ce898-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Fürtkonfiguráció hello fürtcsomópontok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="ce898-737"><a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a> Collect hello cluster nodes in a cluster configuration</span></span>

1.  <span data-ttu-id="ce898-738">Hello szerepkör hozzáadása és szolgáltatások varázsló vegye fel a Feladatátvételi fürtszolgáltatás tooboth fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="ce898-738">In hello Add Role and Features Wizard, add failover clustering tooboth cluster nodes.</span></span>
2.  <span data-ttu-id="ce898-739">Hello feladatátvevő fürt beállítása a Feladatátvevőfürt-kezelő használatával.</span><span class="sxs-lookup"><span data-stu-id="ce898-739">Set up hello failover cluster by using Failover Cluster Manager.</span></span> <span data-ttu-id="ce898-740">A Feladatátvevőfürt-kezelőben válasszon **fürt létrehozása**, majd adja hozzá a hello első fürt, csomópont A. csak hello neve Nem hello második csomópont hozzáadása még; hello második csomópont egy későbbi lépésben fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="ce898-740">In Failover Cluster Manager, select **Create Cluster**, and then add only hello name of hello first cluster, node A. Do not add hello second node yet; you'll add hello second node in a later step.</span></span>

  ![18. ábra: Hello kiszolgáló vagy a virtuális gép neve hello első fürtcsomópont hozzáadása][sap-ha-guide-figure-3007]

  <span data-ttu-id="ce898-742">_**18. ábra:** Hozzáadás hello kiszolgáló vagy a virtuális gép hello első fürtcsomópont nevét_</span><span class="sxs-lookup"><span data-stu-id="ce898-742">_**Figure 18:** Add hello server or virtual machine name of hello first cluster node_</span></span>

3.  <span data-ttu-id="ce898-743">Adja meg a hello hello fürt hálózati neve (virtuális állomás neve).</span><span class="sxs-lookup"><span data-stu-id="ce898-743">Enter hello network name (virtual host name) of hello cluster.</span></span>

  ![19. ábra: Hello fürt nevét adja meg.][sap-ha-guide-figure-3008]

  <span data-ttu-id="ce898-745">_**19. ábra:** hello fürt nevének megadása_</span><span class="sxs-lookup"><span data-stu-id="ce898-745">_**Figure 19:** Enter hello cluster name_</span></span>

4.  <span data-ttu-id="ce898-746">Hello fürt létrehozása után futtassa a fürtellenőrzési tesztet.</span><span class="sxs-lookup"><span data-stu-id="ce898-746">After you've created hello cluster, run a cluster validation test.</span></span>

  ![20. ábra: Hello fürt érvényesítési-ellenőrzés futtatása][sap-ha-guide-figure-3009]

  <span data-ttu-id="ce898-748">_**20. ábra:** hello fürt eredetiség ellenőrzésének futtatása_</span><span class="sxs-lookup"><span data-stu-id="ce898-748">_**Figure 20:** Run hello cluster validation check_</span></span>

  <span data-ttu-id="ce898-749">Lemezek ezen a ponton hello folyamat kapcsolatos figyelmeztetéseket figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-749">You can ignore any warnings about disks at this point in hello process.</span></span> <span data-ttu-id="ce898-750">A tanúsító fájlmegosztás és hello SIOS megosztott lemezek később fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="ce898-750">You'll add a file share witness and hello SIOS shared disks later.</span></span> <span data-ttu-id="ce898-751">Ebben a szakaszban egy kvórum kapcsolatos tooworry nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ce898-751">At this stage, you don't need tooworry about having a quorum.</span></span>

  ![21. ábra: Kvórumlemez nem található][sap-ha-guide-figure-3010]

  <span data-ttu-id="ce898-753">_**21. ábra:** kvórumlemez nem található_</span><span class="sxs-lookup"><span data-stu-id="ce898-753">_**Figure 21:** No quorum disk is found_</span></span>

  ![22. ábra: Core fürterőforrás kell egy új IP-cím][sap-ha-guide-figure-3011]

  <span data-ttu-id="ce898-755">_**22. ábra:** Core fürterőforrás kell egy új IP-cím_</span><span class="sxs-lookup"><span data-stu-id="ce898-755">_**Figure 22:** Core cluster resource needs a new IP address_</span></span>

5.  <span data-ttu-id="ce898-756">Hello core fürtszolgáltatás hello IP-címének módosítása.</span><span class="sxs-lookup"><span data-stu-id="ce898-756">Change hello IP address of hello core cluster service.</span></span> <span data-ttu-id="ce898-757">hello fürt nem indítható el, amíg meg nem változtatja hello IP-cím hello core fürtszolgáltatás, mert hello kiszolgáló IP-címének hello tooone hello virtuális gép csomópontok.</span><span class="sxs-lookup"><span data-stu-id="ce898-757">hello cluster can't start until you change hello IP address of hello core cluster service, because hello IP address of hello server points tooone of hello virtual machine nodes.</span></span> <span data-ttu-id="ce898-758">Ehhez a hello **tulajdonságok** hello core fürt szolgáltatás IP-erőforrás oldalán.</span><span class="sxs-lookup"><span data-stu-id="ce898-758">Do this on hello **Properties** page of hello core cluster service's IP resource.</span></span>

  <span data-ttu-id="ce898-759">Például tooassign IP-címet kell (a példánkban **10.0.0.42**) hello fürt virtuális állomás neve **pr1-ASC-vir**.</span><span class="sxs-lookup"><span data-stu-id="ce898-759">For example, we need tooassign an IP address (in our example, **10.0.0.42**) for hello cluster virtual host name **pr1-ascs-vir**.</span></span>

  ![23. ábra: A hello tulajdonságai párbeszédpanelen hello IP-címének módosítása][sap-ha-guide-figure-3012]

  <span data-ttu-id="ce898-761">_**23. ábra:** a hello **tulajdonságok** párbeszédpanelen, a módosítás hello IP-cím_</span><span class="sxs-lookup"><span data-stu-id="ce898-761">_**Figure 23:** In hello **Properties** dialog box, change hello IP address_</span></span>

  ![24. ábra: Hello fürt fenntartott hello IP-cím hozzárendelése][sap-ha-guide-figure-3013]

  <span data-ttu-id="ce898-763">_**24. ábra:** hello fürt fenntartott hello IP-cím hozzárendelése_</span><span class="sxs-lookup"><span data-stu-id="ce898-763">_**Figure 24:** Assign hello IP address that is reserved for hello cluster_</span></span>

6.  <span data-ttu-id="ce898-764">Hello fürt virtuális állomás neve online állapotba.</span><span class="sxs-lookup"><span data-stu-id="ce898-764">Bring hello cluster virtual host name online.</span></span>

  ![25. ábra: Core a fürtszolgáltatás működik-e és fut, valamint hello javítsa ki az IP-cím][sap-ha-guide-figure-3014]

  <span data-ttu-id="ce898-766">_**25. ábra:** core a fürtszolgáltatás működik-e és fut, és a hello javítsa ki az IP-cím_</span><span class="sxs-lookup"><span data-stu-id="ce898-766">_**Figure 25:** Cluster core service is up and running, and with hello correct IP address_</span></span>

7.  <span data-ttu-id="ce898-767">Adja hozzá az hello második fürtcsomópontokat.</span><span class="sxs-lookup"><span data-stu-id="ce898-767">Add hello second cluster node.</span></span>

  <span data-ttu-id="ce898-768">Most, hogy hello core fürtszolgáltatás működik és elérhető, a második fürtcsomópont hello is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ce898-768">Now that hello core cluster service is up and running, you can add hello second cluster node.</span></span>

  ![26. ábra: Hello második fürt csomópont hozzáadása][sap-ha-guide-figure-3015]

  <span data-ttu-id="ce898-770">_**26. ábra:** Hozzáadás hello második fürtcsomópont_</span><span class="sxs-lookup"><span data-stu-id="ce898-770">_**Figure 26:** Add hello second cluster node_</span></span>

8.  <span data-ttu-id="ce898-771">Adjon meg egy nevet hello második csomópont gazda.</span><span class="sxs-lookup"><span data-stu-id="ce898-771">Enter a name for hello second cluster node host.</span></span>

  ![27. ábra: Adja meg a hello második fürtcsomópont gazdagép neve][sap-ha-guide-figure-3016]

  <span data-ttu-id="ce898-773">_**27. ábra:** hello második fürt csomópont gazdagép nevének megadása_</span><span class="sxs-lookup"><span data-stu-id="ce898-773">_**Figure 27:** Enter hello second cluster node host name_</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ce898-774">Győződjön meg arról, hogy hello **adja hozzá az összes megfelelő tárolót toohello fürtöt** jelölőnégyzet **nem** kijelölt.</span><span class="sxs-lookup"><span data-stu-id="ce898-774">Be sure that hello **Add all eligible storage toohello cluster** check box is **NOT** selected.</span></span>  
  >
  >

  ![28. ábra: Jelölje be az hello jelölőnégyzetet][sap-ha-guide-figure-3017]

  <span data-ttu-id="ce898-776">_**28. ábra:** tegye **nem** válasszon hello jelölőnégyzetet_</span><span class="sxs-lookup"><span data-stu-id="ce898-776">_**Figure 28:** Do **not** select hello check box_</span></span>

  <span data-ttu-id="ce898-777">Kvórum és lemezek kapcsolatos figyelmeztetések figyelmen kívül hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-777">You can ignore warnings about quorum and disks.</span></span> <span data-ttu-id="ce898-778">Hello kvórum és megosztási hello lemez később lesznek állítva, a [telepítése SIOS DataKeeper Cluster Edition SAP ASC/SCS megosztás olyan fürtlemez esetében][sap-ha-guide-8.12.3].</span><span class="sxs-lookup"><span data-stu-id="ce898-778">You'll set hello quorum and share hello disk later, as described in [Installing SIOS DataKeeper Cluster Edition for SAP ASCS/SCS cluster share disk][sap-ha-guide-8.12.3].</span></span>

  ![29. ábra: Hello lemez kvórumával kapcsolatos figyelmeztetések mellőzése][sap-ha-guide-figure-3018]

  <span data-ttu-id="ce898-780">_**29. ábra:** hello lemez kvórumával kapcsolatos figyelmeztetések mellőzése_</span><span class="sxs-lookup"><span data-stu-id="ce898-780">_**Figure 29:** Ignore warnings about hello disk quorum_</span></span>


#### <span data-ttu-id="ce898-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>A fürt tanúsító fájlmegosztás beállítása</span><span class="sxs-lookup"><span data-stu-id="ce898-781"><a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a> Configure a cluster file share witness</span></span>

<span data-ttu-id="ce898-782">Ezeket a feladatokat a fürt tanúsító fájlmegosztás beállítása foglal magában:</span><span class="sxs-lookup"><span data-stu-id="ce898-782">Configuring a cluster file share witness involves these tasks:</span></span>

- <span data-ttu-id="ce898-783">Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce898-783">Creating a file share</span></span>
- <span data-ttu-id="ce898-784">A beállítás hello fájl megosztási tanúsító kvórum a Feladatátvevőfürt-kezelőben</span><span class="sxs-lookup"><span data-stu-id="ce898-784">Setting hello file share witness quorum in Failover Cluster Manager</span></span>

##### <span data-ttu-id="ce898-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce898-785"><a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a> Create a file share</span></span>

1.  <span data-ttu-id="ce898-786">Válassza ki a tanúsító fájlmegosztást egy kvórumlemez helyett.</span><span class="sxs-lookup"><span data-stu-id="ce898-786">Select a file share witness instead of a quorum disk.</span></span> <span data-ttu-id="ce898-787">SIOS DataKeeper támogatja ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ce898-787">SIOS DataKeeper supports this option.</span></span>

  <span data-ttu-id="ce898-788">Az ebben a cikkben szereplő példák hello hello tanúsító fájlmegosztás van az Azure-ban futó hello Active Directory és a DNS-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ce898-788">In hello examples in this article, hello file share witness is on hello Active Directory/DNS server that is running in Azure.</span></span> <span data-ttu-id="ce898-789">hello tanúsító fájlmegosztás neve **domcontr-0**.</span><span class="sxs-lookup"><span data-stu-id="ce898-789">hello file share witness is called **domcontr-0**.</span></span> <span data-ttu-id="ce898-790">Volna már konfigurált egy VPN-kapcsolat tooAzure (keresztül telephelyek közötti VPN vagy Azure expressroute-ot), mert az Active Directory és a DNS szolgáltatás a helyi, és nem megfelelő toorun fájl megosztása tanúsító.</span><span class="sxs-lookup"><span data-stu-id="ce898-790">Because you would have configured a VPN connection tooAzure (via Site-to-Site VPN or Azure ExpressRoute), your Active Directory/DNS service is on-premises and isn't suitable toorun a file share witness.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ce898-791">Ha az Active Directory és a DNS szolgáltatás csak a helyszíni fut, ne állítson be a tanúsító fájlmegosztás hello Active Directory és a DNS a Windows operációs rendszeren, amely a helyszíni fut-e.</span><span class="sxs-lookup"><span data-stu-id="ce898-791">If your Active Directory/DNS service runs only on-premises, don't configure your file share witness on hello Active Directory/DNS Windows operating system that is running on-premises.</span></span> <span data-ttu-id="ce898-792">Előfordulhat, hogy fut az Azure Active Directory és a DNS a helyszíni és a fürtcsomópontok közötti hálózati késés túl nagy, és a csatlakozási problémák miatt.</span><span class="sxs-lookup"><span data-stu-id="ce898-792">Network latency between cluster nodes running in Azure and Active Directory/DNS on-premises might be too large and cause connectivity issues.</span></span> <span data-ttu-id="ce898-793">Lehet, hogy tooconfigure hello tanúsító fájlmegosztás Bezárás toohello fürtcsomóponton futó Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="ce898-793">Be sure tooconfigure hello file share witness on an Azure virtual machine that is running close toohello cluster node.</span></span>  
  >
  >

  <span data-ttu-id="ce898-794">hello kvórum meghajtó legalább 1024 MB szabad területre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-794">hello quorum drive needs at least 1,024 MB of free space.</span></span> <span data-ttu-id="ce898-795">Azt javasoljuk, hogy a 2048 MB szabad lemezterületet a hello kvórum meghajtóra.</span><span class="sxs-lookup"><span data-stu-id="ce898-795">We recommend 2,048 MB of free space for hello quorum drive.</span></span>

2.  <span data-ttu-id="ce898-796">Adja hozzá a hello fürtnévobjektum.</span><span class="sxs-lookup"><span data-stu-id="ce898-796">Add hello cluster name object.</span></span>

  ![30. ábra: Hello fürtnévobjektum hello megosztáson hello engedélyek hozzárendelése][sap-ha-guide-figure-3019]

  <span data-ttu-id="ce898-798">_**30. ábra:** hello fürtnévobjektum hello megosztáson hello engedélyek hozzárendelése_</span><span class="sxs-lookup"><span data-stu-id="ce898-798">_**Figure 30:** Assign hello permissions on hello share for hello cluster name object_</span></span>

  <span data-ttu-id="ce898-799">Ne feledje hello engedélyek hello fürtnévobjektum hello megosztást hello hatóság toochange adatok közé tartozik (a példánkban **pr1-ASC-vir$**).</span><span class="sxs-lookup"><span data-stu-id="ce898-799">Be sure that hello permissions include hello authority toochange data in hello share for hello cluster name object (in our example, **pr1-ascs-vir$**).</span></span>

3.  <span data-ttu-id="ce898-800">tooadd hello fürt neve objektum toohello listáról válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ce898-800">tooadd hello cluster name object toohello list, select **Add**.</span></span> <span data-ttu-id="ce898-801">Módosítsa a hello szűrő toocheck számítógép-objektumok hozzáadása toothose 31. ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="ce898-801">Change hello filter toocheck for computer objects, in addition toothose shown in Figure 31.</span></span>

  ![31. ábra: Változás hello objektumtípusok tooinclude számítógépek][sap-ha-guide-figure-3020]

  <span data-ttu-id="ce898-803">_**31. ábra:** hello objektumtípusok tooinclude számítógépek módosítása_</span><span class="sxs-lookup"><span data-stu-id="ce898-803">_**Figure 31:** Change hello Object Types tooinclude computers_</span></span>

  ![32. ábra: Jelölje be hello számítógépek][sap-ha-guide-figure-3021]

  <span data-ttu-id="ce898-805">_**32. ábra:** válassza hello **számítógépek** jelölőnégyzetet_</span><span class="sxs-lookup"><span data-stu-id="ce898-805">_**Figure 32:** Select hello **Computers** check box_</span></span>

4.  <span data-ttu-id="ce898-806">Adja meg a hello fürtnévobjektum 31. ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="ce898-806">Enter hello cluster name object as shown in Figure 31.</span></span> <span data-ttu-id="ce898-807">Hello rekord már létrejött, mert hello engedélyek, módosíthatja, ahogy az ábra 30.</span><span class="sxs-lookup"><span data-stu-id="ce898-807">Because hello record has already been created, you can change hello permissions, as shown in Figure 30.</span></span>

5.  <span data-ttu-id="ce898-808">Jelölje be hello **biztonsági** hello megosztást, és majd lapján részletesebb hello fürtnévobjektum engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="ce898-808">Select hello **Security** tab of hello share, and then set more detailed permissions for hello cluster name object.</span></span>

  ![33. ábra: Hello biztonsági attribútumainak hello fürtnévobjektum a hello fájl megosztási Kvórum beállítása.][sap-ha-guide-figure-3022]

  <span data-ttu-id="ce898-810">_**33. ábra:** hello biztonsági attribútumainak hello fürtnévobjektum a hello fájl megosztási Kvórum beállítása_</span><span class="sxs-lookup"><span data-stu-id="ce898-810">_**Figure 33:** Set hello security attributes for hello cluster name object on hello file share quorum_</span></span>

##### <span data-ttu-id="ce898-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Hello fájl megosztási tanúsító Kvórum beállítása a Feladatátvevőfürt-kezelőben</span><span class="sxs-lookup"><span data-stu-id="ce898-811"><a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a> Set hello file share witness quorum in Failover Cluster Manager</span></span>

1.  <span data-ttu-id="ce898-812">Nyissa meg a hello konfigurálása fürtkvórum beállítása varázslót.</span><span class="sxs-lookup"><span data-stu-id="ce898-812">Open hello Configure Quorum Setting Wizard.</span></span>

  ![34. ábra: Hello konfigurálása fürtkvórum beállítása varázsló indítása][sap-ha-guide-figure-3023]

  <span data-ttu-id="ce898-814">_**34. ábra:** Start hello konfigurálása fürtkvórum beállítása varázsló_</span><span class="sxs-lookup"><span data-stu-id="ce898-814">_**Figure 34:** Start hello Configure Cluster Quorum Setting Wizard_</span></span>

2.  <span data-ttu-id="ce898-815">A hello **kvórumkonfiguráció kiválasztása** lapon, hogy melyik **hello kvórum tanúsítójának kijelölése**.</span><span class="sxs-lookup"><span data-stu-id="ce898-815">On hello **Select Quorum Configuration** page, select **Select hello quorum witness**.</span></span>

  ![35. ábra: Választhat kvórumkonfigurációinak][sap-ha-guide-figure-3024]

  <span data-ttu-id="ce898-817">_**35. ábra:** választhat kvórumkonfigurációinak_</span><span class="sxs-lookup"><span data-stu-id="ce898-817">_**Figure 35:** Quorum configurations you can choose from_</span></span>

3.  <span data-ttu-id="ce898-818">A hello **kvórum Tanúsítójának kijelölése** lapon, hogy melyik **konfigurálása egy tanúsító fájlmegosztást**.</span><span class="sxs-lookup"><span data-stu-id="ce898-818">On hello **Select Quorum Witness** page, select **Configure a file share witness**.</span></span>

  ![36. ábra: Select hello tanúsító fájlmegosztás][sap-ha-guide-figure-3025]

  <span data-ttu-id="ce898-820">_**36. ábra:** hello tanúsító fájlmegosztás kiválasztása_</span><span class="sxs-lookup"><span data-stu-id="ce898-820">_**Figure 36:** Select hello file share witness_</span></span>

4.  <span data-ttu-id="ce898-821">Adja meg a hello UNC elérési út toohello fájlmegosztást (a példánkban \\domcontr-0\FSW).</span><span class="sxs-lookup"><span data-stu-id="ce898-821">Enter hello UNC path toohello file share (in our example, \\domcontr-0\FSW).</span></span> <span data-ttu-id="ce898-822">hello módosításokat végezhet, válassza ki a listáját toosee **következő**.</span><span class="sxs-lookup"><span data-stu-id="ce898-822">toosee a list of hello changes you can make, select **Next**.</span></span>

  ![37. ábra: Hello fájlmegosztási helyet hello tanúsító fájlmegosztás meghatározása][sap-ha-guide-figure-3026]

  <span data-ttu-id="ce898-824">_**37. ábra:** hello fájlmegosztási helyet hello tanúsító fájlmegosztás meghatározása_</span><span class="sxs-lookup"><span data-stu-id="ce898-824">_**Figure 37:** Define hello file share location for hello witness share_</span></span>

5.  <span data-ttu-id="ce898-825">Válassza ki a hello módosításokat, és válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="ce898-825">Select hello changes you want, and then select **Next**.</span></span> <span data-ttu-id="ce898-826">Toosuccessfully kell konfigurálnia a hello fürtkonfiguráció ahogy az ábra 38.</span><span class="sxs-lookup"><span data-stu-id="ce898-826">You need toosuccessfully reconfigure hello cluster configuration as shown in Figure 38.</span></span>  

  ![38. ábra: Megerősítése, hogy Ön már újrakonfigurálása hello fürt][sap-ha-guide-figure-3027]

  <span data-ttu-id="ce898-828">_**38. ábra:** , hogy Ön már újrakonfigurálása hello fürt megerősítése_</span><span class="sxs-lookup"><span data-stu-id="ce898-828">_**Figure 38:** Confirmation that you've reconfigured hello cluster_</span></span>

<span data-ttu-id="ce898-829">Windows feladatátvevő fürt hello sikeres telepítését követően a módosításokat kell toobe toosome küszöbértékek tooadapt feladatátvételi észlelési tooconditions tett az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce898-829">After installing hello Windows Failover Cluster successfully, changes need toobe made toosome thresholds tooadapt failover detection tooconditions in Azure.</span></span> <span data-ttu-id="ce898-830">hello megváltozott paraméterek toobe szerepelnek a blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/.</span><span class="sxs-lookup"><span data-stu-id="ce898-830">hello parameters toobe changed are documented in this blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/ .</span></span> <span data-ttu-id="ce898-831">Feltételezve, hogy a két virtuális gépekre, amelyek build hello fürtkonfiguráció Windows ASC/SCS a szerepelnek hello ugyanazon az alhálózaton, hello következő paramétereket kell módosítani toobe toothese értékeket:</span><span class="sxs-lookup"><span data-stu-id="ce898-831">Assuming that your two VMs that build hello Windows Cluster Configuration for ASCS/SCS are in hello same SubNet, hello following parameters need toobe changed toothese values:</span></span>
- <span data-ttu-id="ce898-832">SameSubNetDelay = 2</span><span class="sxs-lookup"><span data-stu-id="ce898-832">SameSubNetDelay = 2</span></span>
- <span data-ttu-id="ce898-833">SameSubNetThreshold = 15</span><span class="sxs-lookup"><span data-stu-id="ce898-833">SameSubNetThreshold = 15</span></span>

<span data-ttu-id="ce898-834">Ezeket a beállításokat felhasználók tesztelése és egy jó biztonsági sérülés toobe elég rugalmas megadott hello egy oldalán.</span><span class="sxs-lookup"><span data-stu-id="ce898-834">These settings were tested with customers and provided a good compromise toobe resilient enough on hello one side.</span></span> <span data-ttu-id="ce898-835">A hello ugyanakkor ezek a beállítások volt biztosítása gyors elég feladatátvételi valós hibaüzenet feltételekben az SAP-szoftver vagy a csomópont vagy Virtuálisgép-hiba.</span><span class="sxs-lookup"><span data-stu-id="ce898-835">On hello other hand those settings were providing fast enough failover in real error conditions on SAP software or node/VM failure.</span></span> 

### <span data-ttu-id="ce898-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Hello SAP ASC/SCS fürtlemez megosztás SIOS DataKeeper Cluster Edition telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-836"><a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a> Install SIOS DataKeeper Cluster Edition for hello SAP ASCS/SCS cluster share disk</span></span>

<span data-ttu-id="ce898-837">Most már rendelkezik egy működő Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce898-837">You now have a working Windows Server Failover Clustering configuration in Azure.</span></span> <span data-ttu-id="ce898-838">De tooinstall SAP ASC/SCS példánya egy megosztott lemez erőforrás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-838">But, tooinstall an SAP ASCS/SCS instance, you need a shared disk resource.</span></span> <span data-ttu-id="ce898-839">Nem hozható létre megosztott hello lemezerőforrásokat van szüksége az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ce898-839">You cannot create hello shared disk resources you need in Azure.</span></span> <span data-ttu-id="ce898-840">SIOS DataKeeper Cluster Edition egy olyan külső megoldás, megosztott toocreate lemezerőforrásokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-840">SIOS DataKeeper Cluster Edition is a third-party solution you can use toocreate shared disk resources.</span></span>

<span data-ttu-id="ce898-841">Az SAP ASC/SCS hello SIOS DataKeeper Cluster Edition telepítését a megosztás fürtlemez ezeket a feladatokat foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="ce898-841">Installing SIOS DataKeeper Cluster Edition for hello SAP ASCS/SCS cluster share disk involves these tasks:</span></span>

- <span data-ttu-id="ce898-842">.NET-keretrendszer 3.5 hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce898-842">Adding hello .NET Framework 3.5</span></span>
- <span data-ttu-id="ce898-843">SIOS DataKeeper telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-843">Installing SIOS DataKeeper</span></span>
- <span data-ttu-id="ce898-844">SIOS DataKeeper beállítása</span><span class="sxs-lookup"><span data-stu-id="ce898-844">Setting up SIOS DataKeeper</span></span>

#### <span data-ttu-id="ce898-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>.NET-keretrendszer 3.5 hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce898-845"><a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a> Add hello .NET Framework 3.5</span></span>
<span data-ttu-id="ce898-846">hello Microsoft .NET-keretrendszer 3.5-ös verzióját nem automatikusan aktiválva, vagy Windows Server 2012 R2 rendszeren telepítve.</span><span class="sxs-lookup"><span data-stu-id="ce898-846">hello Microsoft .NET Framework 3.5 isn't automatically activated or installed on Windows Server 2012 R2.</span></span> <span data-ttu-id="ce898-847">SIOS DataKeeper használatához hello .NET-keretrendszer toobe telepíthető DataKeeper minden csomóponton, telepítenie kell hello .NET-keretrendszer 3.5 hello vendég operációs rendszeren az hello fürt összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ce898-847">Because SIOS DataKeeper requires hello .NET Framework toobe on all nodes that you install DataKeeper on, you must install hello .NET Framework 3.5 on hello guest operating system of all virtual machines in hello cluster.</span></span>

<span data-ttu-id="ce898-848">Két módon tooadd hello .NET-keretrendszer 3.5 van:</span><span class="sxs-lookup"><span data-stu-id="ce898-848">There are two ways tooadd hello .NET Framework 3.5:</span></span>

- <span data-ttu-id="ce898-849">Hello-szerepkörök hozzáadása és szolgáltatások varázsló használata a Windows ahogy az ábra 39.</span><span class="sxs-lookup"><span data-stu-id="ce898-849">Use hello Add Roles and Features Wizard in Windows as shown in Figure 39.</span></span>

  ![39. ábra: Hello .NET-keretrendszer 3.5 telepítése hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével][sap-ha-guide-figure-3028]

  <span data-ttu-id="ce898-851">_**39. ábra:** telepítés hello .NET-keretrendszer 3.5 hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével_</span><span class="sxs-lookup"><span data-stu-id="ce898-851">_**Figure 39:** Install hello .NET Framework 3.5 by using hello Add Roles and Features Wizard_</span></span>

  ![40. ábra: Telepítés folyamatjelző hello .NET-keretrendszer 3.5 telepítése hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével][sap-ha-guide-figure-3029]

  <span data-ttu-id="ce898-853">_**40. ábra:** sáv hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével hello .NET-keretrendszer 3.5 telepítésekor a telepítési folyamat_</span><span class="sxs-lookup"><span data-stu-id="ce898-853">_**Figure 40:** Installation progress bar when you install hello .NET Framework 3.5 by using hello Add Roles and Features Wizard_</span></span>

- <span data-ttu-id="ce898-854">Hello parancssori eszköz a dism.exe használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="ce898-854">Use hello command-line tool dism.exe.</span></span> <span data-ttu-id="ce898-855">Az ilyen típusú telepítés tooaccess hello SxS könyvtár a telepítési adathordozó Windows hello kell.</span><span class="sxs-lookup"><span data-stu-id="ce898-855">For this type of installation, you need tooaccess hello SxS directory on hello Windows installation media.</span></span> <span data-ttu-id="ce898-856">Egy rendszergazda jogú parancssort írja be:</span><span class="sxs-lookup"><span data-stu-id="ce898-856">At an elevated command prompt, type:</span></span>

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <span data-ttu-id="ce898-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-857"><a name="dd41d5a2-8083-415b-9878-839652812102"></a> Install SIOS DataKeeper</span></span>

<span data-ttu-id="ce898-858">Telepítse a SIOS DataKeeper Cluster Edition hello fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="ce898-858">Install SIOS DataKeeper Cluster Edition on each node in hello cluster.</span></span> <span data-ttu-id="ce898-859">SIOS DataKeeper, a megosztott tároló virtuális toocreate hozzon létre egy szinkronizált tükrözött, és majd szimulálása a fürt megosztott tároló.</span><span class="sxs-lookup"><span data-stu-id="ce898-859">toocreate virtual shared storage with SIOS DataKeeper, create a synced mirror and then simulate cluster shared storage.</span></span>

<span data-ttu-id="ce898-860">Hello SIOS szoftver telepítése előtt hozzon létre hello tartományi felhasználó **DataKeeperSvc**.</span><span class="sxs-lookup"><span data-stu-id="ce898-860">Before you install hello SIOS software, create hello domain user **DataKeeperSvc**.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-861">Adja hozzá a hello **DataKeeperSvc** felhasználói toohello **helyi rendszergazdai** csoport mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="ce898-861">Add hello **DataKeeperSvc** user toohello **Local Administrator** group on both cluster nodes.</span></span>
>
>

<span data-ttu-id="ce898-862">tooinstall SIOS DataKeeper:</span><span class="sxs-lookup"><span data-stu-id="ce898-862">tooinstall SIOS DataKeeper:</span></span>

1.  <span data-ttu-id="ce898-863">Szoftvertelepítés hello SIOS mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="ce898-863">Install hello SIOS software on both cluster nodes.</span></span>

  ![SIOS telepítő][sap-ha-guide-figure-3030]

  ![. Ábra 41: Hello SIOS DataKeeper telepítési első oldalán][sap-ha-guide-figure-3031]

  <span data-ttu-id="ce898-866">_**41. ábra:** hello SIOS DataKeeper telepítési első oldalára_</span><span class="sxs-lookup"><span data-stu-id="ce898-866">_**Figure 41:** First page of hello SIOS DataKeeper installation_</span></span>

2.  <span data-ttu-id="ce898-867">A megjelenő ábra 42 hello párbeszédpanelen válassza ki a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="ce898-867">In hello dialog box shown in Figure 42, select **Yes**.</span></span>

  ![42. ábra: DataKeeper arról tájékoztatja, hogy a szolgáltatás le lesz tiltva][sap-ha-guide-figure-3032]

  <span data-ttu-id="ce898-869">_**42. ábra:** DataKeeper arról tájékoztatja, hogy a szolgáltatás le lesz tiltva_</span><span class="sxs-lookup"><span data-stu-id="ce898-869">_**Figure 42:** DataKeeper informs you that a service will be disabled_</span></span>

3.  <span data-ttu-id="ce898-870">Hello párbeszédpanelen megjelenő ábra 43, azt javasoljuk, hogy kiválassza **tartomány vagy a kiszolgáló fiók**.</span><span class="sxs-lookup"><span data-stu-id="ce898-870">In hello dialog box shown in Figure 43, we recommend that you select **Domain or Server account**.</span></span>

  ![43. ábra: A SIOS DataKeeper felhasználó kiválasztása][sap-ha-guide-figure-3033]

  <span data-ttu-id="ce898-872">_**43. ábra:** SIOS DataKeeper a felhasználó kiválasztása_</span><span class="sxs-lookup"><span data-stu-id="ce898-872">_**Figure 43:** User selection for SIOS DataKeeper_</span></span>

4.  <span data-ttu-id="ce898-873">Adja meg a hello tartományi fiók felhasználói nevét és SIOS DataKeeper létrehozott jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="ce898-873">Enter hello domain account user name and passwords that you created for SIOS DataKeeper.</span></span>

  ![44. ábra: Hello tartományi felhasználónév és jelszó megadása hello SIOS DataKeeper telepítése][sap-ha-guide-figure-3034]

  <span data-ttu-id="ce898-875">_**44. ábra:** hello SIOS DataKeeper telepítési hello tartományi felhasználónevet és jelszót adjon meg_</span><span class="sxs-lookup"><span data-stu-id="ce898-875">_**Figure 44:** Enter hello domain user name and password for hello SIOS DataKeeper installation_</span></span>

5.  <span data-ttu-id="ce898-876">Telepítse a SIOS DataKeeper példány, ahogy az ábra 45 hello Licenckulcs.</span><span class="sxs-lookup"><span data-stu-id="ce898-876">Install hello license key for your SIOS DataKeeper instance as shown in Figure 45.</span></span>

  ![45. ábra: Adja meg a SIOS DataKeeper licenckulcs][sap-ha-guide-figure-3035]

  <span data-ttu-id="ce898-878">_**45. ábra:** adja meg a SIOS DataKeeper licenckulcs_</span><span class="sxs-lookup"><span data-stu-id="ce898-878">_**Figure 45:** Enter your SIOS DataKeeper license key_</span></span>

6.  <span data-ttu-id="ce898-879">Amikor a rendszer kéri, indítsa újra a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ce898-879">When prompted, restart hello virtual machine.</span></span>

#### <span data-ttu-id="ce898-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper beállítása</span><span class="sxs-lookup"><span data-stu-id="ce898-880"><a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a> Set up SIOS DataKeeper</span></span>

<span data-ttu-id="ce898-881">Telepítése után SIOS DataKeeper mindkét csomóponton, toostart hello konfigurációs szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-881">After you install SIOS DataKeeper on both nodes, you need toostart hello configuration.</span></span> <span data-ttu-id="ce898-882">hello hello konfigurációs célja toohave szinkron replikálása hello további csatlakoztatott virtuális merevlemezek tooeach hello virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="ce898-882">hello goal of hello configuration is toohave synchronous data replication between hello additional VHDs attached tooeach of hello virtual machines.</span></span>

1.  <span data-ttu-id="ce898-883">Indítsa el a hello DataKeeper felügyeleti és a konfigurációs eszközt, és válassza ki **Connect kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="ce898-883">Start hello DataKeeper Management and Configuration tool, and then select **Connect Server**.</span></span> <span data-ttu-id="ce898-884">(Az ábrán 46, ez a beállítás van körben piros.)</span><span class="sxs-lookup"><span data-stu-id="ce898-884">(In Figure 46, this option is circled in red.)</span></span>

  ![46. ábra: SIOS DataKeeper kezelési és konfigurációs eszköz][sap-ha-guide-figure-3036]

  <span data-ttu-id="ce898-886">_**46. ábra:** SIOS DataKeeper felügyelete és konfigurálása eszköz_</span><span class="sxs-lookup"><span data-stu-id="ce898-886">_**Figure 46:** SIOS DataKeeper Management and Configuration tool_</span></span>

2.  <span data-ttu-id="ce898-887">Adja meg hello nevét, vagy a TCP/IP-cím hello első csomópont hello felügyelete és konfigurálása eszköz kell csatlakoztatja, és, a második lépésben hello második csomópontra.</span><span class="sxs-lookup"><span data-stu-id="ce898-887">Enter hello name or TCP/IP address of hello first node hello Management and Configuration tool should connect to, and, in a second step, hello second node.</span></span>

  ![47. ábra: Hello neve vagy a TCP/IP-cím hello első csomópont hello felügyelete és konfigurálása eszköz kell csatlakoztatja, és egy második lépésben hello második csomópont][sap-ha-guide-figure-3037]

  <span data-ttu-id="ce898-889">_**47. ábra:** hello neve vagy a TCP/IP-cím hello első csomópont hello felügyelete és konfigurálása eszköz kell csatlakoztatja, és egy második lépésben hello második csomópont_</span><span class="sxs-lookup"><span data-stu-id="ce898-889">_**Figure 47:** Insert hello name or TCP/IP address of hello first node hello Management and Configuration tool should connect to, and in a second step, hello second node_</span></span>

3.  <span data-ttu-id="ce898-890">Hozzon létre hello fájlreplikálási feladat hello két csomópont között.</span><span class="sxs-lookup"><span data-stu-id="ce898-890">Create hello replication job between hello two nodes.</span></span>

  ![48. ábra: A replikáció feladat létrehozása][sap-ha-guide-figure-3038]

  <span data-ttu-id="ce898-892">_**48. ábra:** replikációs feladat létrehozása_</span><span class="sxs-lookup"><span data-stu-id="ce898-892">_**Figure 48:** Create a replication job_</span></span>

  <span data-ttu-id="ce898-893">A varázsló végigvezeti egy fájlreplikálási feladat létrehozásának folyamatán hello.</span><span class="sxs-lookup"><span data-stu-id="ce898-893">A wizard guides you through hello process of creating a replication job.</span></span>
4.  <span data-ttu-id="ce898-894">Adja meg a hello nevét, az TCP/IP-cím és a hello forráscsomópont lemezköteten.</span><span class="sxs-lookup"><span data-stu-id="ce898-894">Define hello name, TCP/IP address, and disk volume of hello source node.</span></span>

  ![49. ábra: Hello replikációs feladat hello nevének meghatározása][sap-ha-guide-figure-3039]

  <span data-ttu-id="ce898-896">_**49. ábra:** Define hello hello replikációs feladat neve_</span><span class="sxs-lookup"><span data-stu-id="ce898-896">_**Figure 49:** Define hello name of hello replication job_</span></span>

  ![50. ábra: Hello alap adatok hello csomópont, hogy hello aktuális forráscsomópont alkalmazandó meghatározása][sap-ha-guide-figure-3040]

  <span data-ttu-id="ce898-898">_**50. ábra:** hello csomópont, hogy hello aktuális forráscsomópont alkalmazandó hello adatainak megadása_</span><span class="sxs-lookup"><span data-stu-id="ce898-898">_**Figure 50:** Define hello base data for hello node, which should be hello current source node_</span></span>

5.  <span data-ttu-id="ce898-899">Hello nevét, a TCP/IP-cím és a hello célcsomóponttal lemezkötetének megadása.</span><span class="sxs-lookup"><span data-stu-id="ce898-899">Define hello name, TCP/IP address, and disk volume of hello target node.</span></span>

  ![51. ábra: Hello alap adatok hello csomópont, hogy hello aktuális célcsomóponttal alkalmazandó meghatározása][sap-ha-guide-figure-3041]

  <span data-ttu-id="ce898-901">_**51. ábra:** hello csomópont, hogy hello aktuális célcsomóponttal alkalmazandó hello adatainak megadása_</span><span class="sxs-lookup"><span data-stu-id="ce898-901">_**Figure 51:** Define hello base data for hello node, which should be hello current target node_</span></span>

6.  <span data-ttu-id="ce898-902">Hello tömörítési algoritmust határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ce898-902">Define hello compression algorithms.</span></span> <span data-ttu-id="ce898-903">Ebben a példában azt javasoljuk, hogy a tömörítés hello replikálási adatfolyamból.</span><span class="sxs-lookup"><span data-stu-id="ce898-903">In our example, we recommend that you compress hello replication stream.</span></span> <span data-ttu-id="ce898-904">Különösen abban az esetben az újraszinkronizálás hello replikálási adatfolyamból hello tömörítésének jelentősen csökkenti az újraszinkronizálás idő.</span><span class="sxs-lookup"><span data-stu-id="ce898-904">Especially in resynchronization situations, hello compression of hello replication stream dramatically reduces resynchronization time.</span></span> <span data-ttu-id="ce898-905">Vegye figyelembe, hogy a tömörítési hello Processzor és memória szempontjából erőforrásokat egy virtuális gép használ.</span><span class="sxs-lookup"><span data-stu-id="ce898-905">Note that compression uses hello CPU and RAM resources of a virtual machine.</span></span> <span data-ttu-id="ce898-906">A hello tömörítési arány növekedése, hello használt CPU-erőforrások mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="ce898-906">As hello compression rate increases, so does hello volume of CPU resources used.</span></span> <span data-ttu-id="ce898-907">Is módosíthatja ezt a beállítást később.</span><span class="sxs-lookup"><span data-stu-id="ce898-907">You also can adjust this setting later.</span></span>

7.  <span data-ttu-id="ce898-908">Egy másik beállítás kell toocheck e hello replikáció aszinkron vagy szinkron módon történik-e.</span><span class="sxs-lookup"><span data-stu-id="ce898-908">Another setting you need toocheck is whether hello replication occurs asynchronously or synchronously.</span></span> <span data-ttu-id="ce898-909">*Ha a SAP ASC/SCS konfigurációk, használnia kell a szinkron replikáció*.</span><span class="sxs-lookup"><span data-stu-id="ce898-909">*When you protect SAP ASCS/SCS configurations, you must use synchronous replication*.</span></span>  

  ![52. ábra: A replikáció adatainak megadása][sap-ha-guide-figure-3042]

  <span data-ttu-id="ce898-911">_**52. ábra:** replikációs adatainak megadása_</span><span class="sxs-lookup"><span data-stu-id="ce898-911">_**Figure 52:** Define replication details_</span></span>

8.  <span data-ttu-id="ce898-912">Határozza meg, hogy legyen-e az hello kötet replikált hello replikációs feladat által képviselt tooa Windows Server feladatátvételi fürtszolgáltatási fürtkonfiguráció egy megosztott lemezt.</span><span class="sxs-lookup"><span data-stu-id="ce898-912">Define whether hello volume that is replicated by hello replication job should be represented tooa Windows Server Failover Clustering cluster configuration as a shared disk.</span></span> <span data-ttu-id="ce898-913">Hello SAP ASC/SCS konfigurációs, válassza a **Igen** , hogy a fürt látja hello Windows hello replikált a kötet olyan megosztott lemezzel, amelyet a fürt kötetként használhat.</span><span class="sxs-lookup"><span data-stu-id="ce898-913">For hello SAP ASCS/SCS configuration, select **Yes** so that hello Windows cluster sees hello replicated volume as a shared disk that it can use as a cluster volume.</span></span>

  ![53. ábra: Kattintson az Igen tooset hello replikált kötet egy fürtkötetként][sap-ha-guide-figure-3043]

  <span data-ttu-id="ce898-915">_**53. ábra:** válasszon **Igen** tooset hello egy fürt kötet replikálása_</span><span class="sxs-lookup"><span data-stu-id="ce898-915">_**Figure 53:** Select **Yes** tooset hello replicated volume as a cluster volume_</span></span>

  <span data-ttu-id="ce898-916">Hello kötet létrehozása után hello DataKeeper felügyelete és konfigurálása eszköz látható, hogy hello fájlreplikálási feladat aktív.</span><span class="sxs-lookup"><span data-stu-id="ce898-916">After hello volume is created, hello DataKeeper Management and Configuration tool shows that hello replication job is active.</span></span>

  ![Ábra 54: DataKeeper szinkron tükrözés hello SAP ASC/SCS megosztás lemez a jelenleg aktív][sap-ha-guide-figure-3044]

  <span data-ttu-id="ce898-918">_**Ábra 54:** DataKeeper szinkron tükrözés hello SAP ASC/SCS osztozhat lemezen a jelenleg aktív_</span><span class="sxs-lookup"><span data-stu-id="ce898-918">_**Figure 54:** DataKeeper synchronous mirroring for hello SAP ASCS/SCS share disk is active_</span></span>

  <span data-ttu-id="ce898-919">Ahogy az ábra 55 a Feladatátvevőfürt-kezelő most hello lemez DataKeeper lemezként jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ce898-919">Failover Cluster Manager now shows hello disk as a DataKeeper disk, as shown in Figure 55.</span></span>

  ![55. ábra: A Feladatátvevőfürt-kezelő hello lemez DataKeeper replikált jeleníti meg][sap-ha-guide-figure-3045]

  <span data-ttu-id="ce898-921">_**Ábra 55:** Feladatátvevőfürt-kezelő látható hello lemez adott replikált DataKeeper_</span><span class="sxs-lookup"><span data-stu-id="ce898-921">_**Figure 55:** Failover Cluster Manager shows hello disk that DataKeeper replicated_</span></span>

## <span data-ttu-id="ce898-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Hello SAP NetWeaver rendszer telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-922"><a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a> Install hello SAP NetWeaver system</span></span>

<span data-ttu-id="ce898-923">Mert beállítások változhatnak attól függően hello használt adatbázis-kezelő rendszer azt nem leírják hello DBMS beállítása.</span><span class="sxs-lookup"><span data-stu-id="ce898-923">We won’t describe hello DBMS setup because setups vary depending on hello DBMS system you use.</span></span> <span data-ttu-id="ce898-924">Azonban feltételezzük, hogy magas rendelkezésre állású kérdéseket hello DBMS hello különböző DBMS szállítóktól támogatja az Azure-hello funkciókkal rendelkező tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="ce898-924">However, we assume that high-availability concerns with hello DBMS are addressed with hello functionalities hello different DBMS vendors support for Azure.</span></span> <span data-ttu-id="ce898-925">Például mindig bekapcsolva vagy adatbázis-tükrözést az SQL Server és Oracle Data Guard az Oracle-adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="ce898-925">For example, Always On or database mirroring for SQL Server, and Oracle Data Guard for Oracle databases.</span></span> <span data-ttu-id="ce898-926">Ebben a cikkben használjuk hello esetben azt további védelmi toohello adatbázis-kezelő nem vett fel.</span><span class="sxs-lookup"><span data-stu-id="ce898-926">In hello scenario we use in this article, we didn't add more protection toohello DBMS.</span></span>

<span data-ttu-id="ce898-927">Nincsenek semmilyen külön odafigyelést különböző adatbázis-kezelő szolgáltatásokat fürtözött SAP ASC/SCS konfigurálása az Azure-ban az ilyen kommunikál.</span><span class="sxs-lookup"><span data-stu-id="ce898-927">There are no special considerations when different DBMS services interact with this kind of clustered SAP ASCS/SCS configuration in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-928">hello telepítési eljárásokat az SAP NetWeaver ABAP rendszerek, Java, és a ABAP + Java rendszerek csaknem azonosak.</span><span class="sxs-lookup"><span data-stu-id="ce898-928">hello installation procedures of SAP NetWeaver ABAP systems, Java systems, and ABAP+Java systems are almost identical.</span></span> <span data-ttu-id="ce898-929">hello legfontosabb különbség, hogy rendelkezik-e az SAP ABAP rendszer ASC egy példánya.</span><span class="sxs-lookup"><span data-stu-id="ce898-929">hello most significant difference is that an SAP ABAP system has one ASCS instance.</span></span> <span data-ttu-id="ce898-930">SAP Java rendszer hello egy SCS példány van.</span><span class="sxs-lookup"><span data-stu-id="ce898-930">hello SAP Java system has one SCS instance.</span></span> <span data-ttu-id="ce898-931">hello SAP ABAP + Java rendszer egy ASC példányával rendelkezik, és fut egy SCS példány hello ugyanazt a Microsoft feladatátvevő fürt csoport.</span><span class="sxs-lookup"><span data-stu-id="ce898-931">hello SAP ABAP+Java system has one ASCS instance and one SCS instance running in hello same Microsoft failover cluster group.</span></span> <span data-ttu-id="ce898-932">Összes telepítési különbséget minden SAP NetWeaver telepítési verem explicit módon szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="ce898-932">Any installation differences for each SAP NetWeaver installation stack are explicitly mentioned.</span></span> <span data-ttu-id="ce898-933">Feltételezzük, hogy minden egyéb részeinek vannak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="ce898-933">You can assume that all other parts are hello same.</span></span>  
>
>

### <span data-ttu-id="ce898-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Telepítse a SAP, ha a magas rendelkezésre állású ASC/SCS példánya</span><span class="sxs-lookup"><span data-stu-id="ce898-934"><a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a> Install SAP with a high-availability ASCS/SCS instance</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ce898-935">Győződjön meg a lap DataKeeper fájlja nem a tooplace tükrözött kötetek.</span><span class="sxs-lookup"><span data-stu-id="ce898-935">Be sure not tooplace your page file on DataKeeper mirrored volumes.</span></span> <span data-ttu-id="ce898-936">DataKeeper nem támogatja a tükrözött kötetek.</span><span class="sxs-lookup"><span data-stu-id="ce898-936">DataKeeper does not support mirrored volumes.</span></span> <span data-ttu-id="ce898-937">A lapozófájl hello ideiglenes D meghajtó egy Azure virtuális gép hello alapértelmezett hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="ce898-937">You can leave your page file on hello temporary drive D of an Azure virtual machine, which is hello default.</span></span> <span data-ttu-id="ce898-938">Ha még nem szerepel ott, helyezze át a hello Windows lap fájl toodrive D az Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ce898-938">If it's not already there, move hello Windows page file toodrive D of your Azure virtual machine.</span></span>
>
>

<span data-ttu-id="ce898-939">Ezeket a feladatokat, ha a magas rendelkezésre állású ASC/SCS példánya SAP telepítése foglal magában:</span><span class="sxs-lookup"><span data-stu-id="ce898-939">Installing SAP with a high-availability ASCS/SCS instance involves these tasks:</span></span>

- <span data-ttu-id="ce898-940">Virtuális állomásnevet fürtözött hello SAP ASC/SCS példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce898-940">Creating a virtual host name for hello clustered SAP ASCS/SCS instance</span></span>
- <span data-ttu-id="ce898-941">Hello SAP első fürtcsomópontra telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-941">Installing hello SAP first cluster node</span></span>
- <span data-ttu-id="ce898-942">Hello SAP profil hello ASC/SCS példány módosítása</span><span class="sxs-lookup"><span data-stu-id="ce898-942">Modifying hello SAP profile of hello ASCS/SCS instance</span></span>
- <span data-ttu-id="ce898-943">A mintavétel a port hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce898-943">Adding a probe port</span></span>
- <span data-ttu-id="ce898-944">Hello Windows tűzfal mintavételi port megnyitása</span><span class="sxs-lookup"><span data-stu-id="ce898-944">Opening hello Windows firewall probe port</span></span>

#### <span data-ttu-id="ce898-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Hozzon létre egy virtuális nevet fürtözött hello SAP ASC/SCS példány</span><span class="sxs-lookup"><span data-stu-id="ce898-945"><a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a> Create a virtual host name for hello clustered SAP ASCS/SCS instance</span></span>

1.  <span data-ttu-id="ce898-946">Hello Windows DNS-kezelőben hozzon létre egy DNS-bejegyzést hello ASC/SCS példányának hello virtuális állomás neve.</span><span class="sxs-lookup"><span data-stu-id="ce898-946">In hello Windows DNS manager, create a DNS entry for hello virtual host name of hello ASCS/SCS instance.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ce898-947">hello toohello virtuális állomásnevét ASC/SCS-példány neve lehet hello hozzárendelt IP-cím hello azonos tooAzure terheléselosztóhoz rendelt hello IP-címként (**<*SID*> - lb - ASC **).</span><span class="sxs-lookup"><span data-stu-id="ce898-947">hello IP address that you assign toohello virtual host name of hello ASCS/SCS instance must be hello same as hello IP address that you assigned tooAzure Load Balancer (**<*SID*>-lb-ascs**).</span></span>  
  >
  >

  <span data-ttu-id="ce898-948">IP-cím hello virtuális SAP ASC/SCS állomásnév hello (**pr1-ASC-sap**) van hello azonos Azure terheléselosztó hello IP-címként (**pr1-lb-ASC**).</span><span class="sxs-lookup"><span data-stu-id="ce898-948">hello IP address of hello virtual SAP ASCS/SCS host name (**pr1-ascs-sap**) is hello same as hello IP address of Azure Load Balancer (**pr1-lb-ascs**).</span></span>

  ![Ábra 56: Hello DNS-bejegyzés hello SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím megadása][sap-ha-guide-figure-3046]

  <span data-ttu-id="ce898-950">_**Ábra 56:** hello DNS-bejegyzés hello SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím megadása_</span><span class="sxs-lookup"><span data-stu-id="ce898-950">_**Figure 56:** Define hello DNS entry for hello SAP ASCS/SCS cluster virtual name and TCP/IP address_</span></span>

2.  <span data-ttu-id="ce898-951">toodefine hello IP-hozzárendelt toohello virtuális állomás nevét, jelölje be **DNS-kezelő** > **tartomány**.</span><span class="sxs-lookup"><span data-stu-id="ce898-951">toodefine hello IP address assigned toohello virtual host name, select **DNS Manager** > **Domain**.</span></span>

  ![57. ábra: Új virtuális nevét és TCP/IP-cím SAP ASC/SCS fürtnek megfelelő konfiguráció][sap-ha-guide-figure-3047]

  <span data-ttu-id="ce898-953">_**57. ábra:** új virtuális nevét és a TCP/IP-címtartományok SAP ASC/SCS fürtnek megfelelő konfiguráció_</span><span class="sxs-lookup"><span data-stu-id="ce898-953">_**Figure 57:** New virtual name and TCP/IP address for SAP ASCS/SCS cluster configuration_</span></span>

#### <span data-ttu-id="ce898-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Hello SAP első fürtcsomópontra telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-954"><a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a> Install hello SAP first cluster node</span></span>

1.  <span data-ttu-id="ce898-955">Hello első fürt csomópont lehetőség fürtcsomóponton A. végrehajtása Például a hello **pr1-ASC-0** állomás.</span><span class="sxs-lookup"><span data-stu-id="ce898-955">Execute hello first cluster node option on cluster node A. For example, on hello **pr1-ascs-0** host.</span></span>
2.  <span data-ttu-id="ce898-956">tookeep hello alapértelmezett portok hello Azure belső terheléselosztót, válasszon:</span><span class="sxs-lookup"><span data-stu-id="ce898-956">tookeep hello default ports for hello Azure internal load balancer, select:</span></span>

  * <span data-ttu-id="ce898-957">**ABAP rendszer**: **ASC** szám példány **00**</span><span class="sxs-lookup"><span data-stu-id="ce898-957">**ABAP system**: **ASCS** instance number **00**</span></span>
  * <span data-ttu-id="ce898-958">**Java-rendszer**: **SCS** szám példány **01**</span><span class="sxs-lookup"><span data-stu-id="ce898-958">**Java system**: **SCS** instance number **01**</span></span>
  * <span data-ttu-id="ce898-959">**ABAP + Java rendszer**: **ASC** szám példány **00** és **SCS** szám példány **01**</span><span class="sxs-lookup"><span data-stu-id="ce898-959">**ABAP+Java system**: **ASCS** instance number **00** and **SCS** instance number **01**</span></span>

  <span data-ttu-id="ce898-960">toouse példány számok eltérő 00 hello ABAP ASC a példány és hello Java SCS példány 01, először szüksége toochange hello Azure belső alapértelmezett terheléselosztási szabályok, leírt [módosítás hello ASC/SCS alapértelmezett betöltése terheléselosztási szabályok hello Azure belső terheléselosztóhoz][sap-ha-guide-8.9].</span><span class="sxs-lookup"><span data-stu-id="ce898-960">toouse instance numbers other than 00 for hello ABAP ASCS instance and 01 for hello Java SCS instance, first you need toochange hello Azure internal load balancer default load balancing rules, described in [Change hello ASCS/SCS default load balancing rules for hello Azure internal load balancer][sap-ha-guide-8.9].</span></span>

<span data-ttu-id="ce898-961">hello tovább néhány feladatot nem hello szabványos SAP telepítési dokumentációjában leírt.</span><span class="sxs-lookup"><span data-stu-id="ce898-961">hello next few tasks aren't described in hello standard SAP installation documentation.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-962">hello SAP dokumentáció ismerteti, hogyan tooinstall hello ASC/SCS fürt első csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="ce898-962">hello SAP installation documentation describes how tooinstall hello first ASCS/SCS cluster node.</span></span>
>
>

#### <span data-ttu-id="ce898-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Hello SAP profil hello ASC/SCS példány módosítása</span><span class="sxs-lookup"><span data-stu-id="ce898-963"><a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a> Modify hello SAP profile of hello ASCS/SCS instance</span></span>

<span data-ttu-id="ce898-964">Egy új profil paraméter tooadd van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-964">You need tooadd a new profile parameter.</span></span> <span data-ttu-id="ce898-965">hello-profil paraméter megakadályozza, hogy a SAP-munkafolyamatok és hello sorba helyezni kiszolgáló közötti kapcsolat bezárása, ha túl sokáig üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="ce898-965">hello profile parameter prevents connections between SAP work processes and hello enqueue server from closing when they are idle for too long.</span></span> <span data-ttu-id="ce898-966">A Microsoft hello probléma esetén említett [adja hozzá a beállításjegyzék-bejegyzések mindkét fürtcsomóponton hello SAP ASC/SCS példány][sap-ha-guide-8.11].</span><span class="sxs-lookup"><span data-stu-id="ce898-966">We mentioned hello problem scenario in [Add registry entries on both cluster nodes of hello SAP ASCS/SCS instance][sap-ha-guide-8.11].</span></span> <span data-ttu-id="ce898-967">A szakaszt azt is bevezette a két módosítások toosome Alapszintű TCP/IP kapcsolat paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ce898-967">In that section, we also introduced two changes toosome basic TCP/IP connection parameters.</span></span> <span data-ttu-id="ce898-968">A második lépésben tooset hello sorba helyezni server toosend szükség van egy `keep_alive` jelezze, hogy hello kapcsolatok nem találati hello Azure belső elosztott terhelésű üresjárati küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="ce898-968">In a second step, you need tooset hello enqueue server toosend a `keep_alive` signal so that hello connections don't hit hello Azure internal load balancer's idle threshold.</span></span>

<span data-ttu-id="ce898-969">toomodify hello SAP profil hello ASC/SCS példány:</span><span class="sxs-lookup"><span data-stu-id="ce898-969">toomodify hello SAP profile of hello ASCS/SCS instance:</span></span>

1.  <span data-ttu-id="ce898-970">A profil paraméter toohello SAP ASC/SCS példány profil hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ce898-970">Add this profile parameter toohello SAP ASCS/SCS instance profile:</span></span>

  ```
  enque/encni/set_so_keepalive = true
  ```
  <span data-ttu-id="ce898-971">A jelen példában hello elérési út:</span><span class="sxs-lookup"><span data-stu-id="ce898-971">In our example, hello path is:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  <span data-ttu-id="ce898-972">Például SAP SCS toohello példány profil és a megfelelő elérési út:</span><span class="sxs-lookup"><span data-stu-id="ce898-972">For example, toohello SAP SCS instance profile and corresponding path:</span></span>

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  <span data-ttu-id="ce898-973">tooapply hello módosításokat, indítsa újra a hello SAP ASC /SCS példányt.</span><span class="sxs-lookup"><span data-stu-id="ce898-973">tooapply hello changes, restart hello SAP ASCS /SCS instance.</span></span>

#### <span data-ttu-id="ce898-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Adjon hozzá egy mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="ce898-974"><a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a> Add a probe port</span></span>

<span data-ttu-id="ce898-975">Toomake hello fürtözési konfigurációs működnek hello belső elosztott terhelésű mintavételi használata Azure terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="ce898-975">Use hello internal load balancer's probe functionality toomake hello entire cluster configuration work with Azure Load Balancer.</span></span> <span data-ttu-id="ce898-976">hello Azure belső terheléselosztó általában hello bejövő munkaterhelés részt vevő virtuális gépek között egyenlően osztja el.</span><span class="sxs-lookup"><span data-stu-id="ce898-976">hello Azure internal load balancer usually distributes hello incoming workload equally between participating virtual machines.</span></span> <span data-ttu-id="ce898-977">Azonban ez nem fog működni az egyes fürtkonfigurációk mert csak egy példány aktív.</span><span class="sxs-lookup"><span data-stu-id="ce898-977">However, this won't work in some cluster configurations because only one instance is active.</span></span> <span data-ttu-id="ce898-978">hello más példány passzív, hello munkaterhelés nem tudja elfogadni.</span><span class="sxs-lookup"><span data-stu-id="ce898-978">hello other instance is passive and can’t accept any of hello workload.</span></span> <span data-ttu-id="ce898-979">A mintavétel funkció segítségével hello Azure belső load balancer rendel csak tooan aktív példány végzett munka során.</span><span class="sxs-lookup"><span data-stu-id="ce898-979">A probe functionality helps when hello Azure internal load balancer assigns work only tooan active instance.</span></span> <span data-ttu-id="ce898-980">Hello mintavételi funkciójú hello belső terheléselosztó előfordulások aktív, és majd cél hello munkaterhelés csak hello példány képes észlelni.</span><span class="sxs-lookup"><span data-stu-id="ce898-980">With hello probe functionality, hello internal load balancer can detect which instances are active, and then target only hello instance with hello workload.</span></span>

<span data-ttu-id="ce898-981">a mintavételi portot tooadd:</span><span class="sxs-lookup"><span data-stu-id="ce898-981">tooadd a probe port:</span></span>

1.  <span data-ttu-id="ce898-982">Ellenőrizze a hello aktuális **ProbePort** hello a következő PowerShell-parancs futtatásával beállítása.</span><span class="sxs-lookup"><span data-stu-id="ce898-982">Check hello current **ProbePort** setting by running hello following PowerShell command.</span></span> <span data-ttu-id="ce898-983">Végrehajtja a hello virtuális gépek egyik hello fürt konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="ce898-983">Execute it from within one of hello virtual machines in hello cluster configuration.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  <span data-ttu-id="ce898-984">A mintavételi portot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ce898-984">Define a probe port.</span></span> <span data-ttu-id="ce898-985">hello alapértelmezett mintavételi portszám **0**.</span><span class="sxs-lookup"><span data-stu-id="ce898-985">hello default probe port number is **0**.</span></span> <span data-ttu-id="ce898-986">A jelen példában használjuk a mintavételi portot **62000**.</span><span class="sxs-lookup"><span data-stu-id="ce898-986">In our example, we use probe port **62000**.</span></span>

  ![58. ábra: hello fürt konfigurációs mintavételi portot pedig 0 alapértelmezés szerint][sap-ha-guide-figure-3048]

  <span data-ttu-id="ce898-988">_**58. ábra:** hello alapértelmezett fürt konfigurációs mintavételi portot: 0_</span><span class="sxs-lookup"><span data-stu-id="ce898-988">_**Figure 58:** hello default cluster configuration probe port is 0_</span></span>

  <span data-ttu-id="ce898-989">hello portszámot az SAP Azure Resource Manager-sablonok van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="ce898-989">hello port number is defined in SAP Azure Resource Manager templates.</span></span> <span data-ttu-id="ce898-990">PowerShell hello portszámát rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="ce898-990">You can assign hello port number in PowerShell.</span></span>

  <span data-ttu-id="ce898-991">hello új ProbePort értéket tooset  **SAP <*SID*> IP ** fürterőforrás, futtassa a következő PowerShell-parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="ce898-991">tooset a new ProbePort value for hello **SAP <*SID*> IP** cluster resource, run hello following PowerShell script.</span></span> <span data-ttu-id="ce898-992">Hello PowerShell változók a környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="ce898-992">Update hello PowerShell variables for your environment.</span></span> <span data-ttu-id="ce898-993">Hello parancsfájl futtatása után lesz felszólító toorestart hello SAP fürt csoport tooactivate hello változásait.</span><span class="sxs-lookup"><span data-stu-id="ce898-993">After hello script runs, you'll be prompted toorestart hello SAP cluster group tooactivate hello changes.</span></span>

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

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
  Write-Host "Current probe port property of hello SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting hello new probe port property of hello SAP cluster resource '$SAPIPresourceName' too'$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want tootake restart SAP cluster role '$SAPClusterRoleName', tooactivate hello changes (yes/no)?"

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

  <span data-ttu-id="ce898-994">Miután hello kapcsolása  **SAP <*SID*> ** fürtön a szerepkör hálózatra, ellenőrizze, hogy **ProbePort** toohello új érték van beállítva.</span><span class="sxs-lookup"><span data-stu-id="ce898-994">After you bring hello **SAP <*SID*>** cluster role online, verify that **ProbePort** is set toohello new value.</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![59. ábra: Mintavételi hello fürt port hello új érték beállítása után][sap-ha-guide-figure-3049]

  <span data-ttu-id="ce898-996">_**59. ábra:** hello új érték beállítása után, mintavételi modulja hello fürt port_</span><span class="sxs-lookup"><span data-stu-id="ce898-996">_**Figure 59:** Probe hello cluster port after you set hello new value_</span></span>

#### <span data-ttu-id="ce898-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Nyissa meg a hello Windows tűzfal mintavételi portot</span><span class="sxs-lookup"><span data-stu-id="ce898-997"><a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a> Open hello Windows firewall probe port</span></span>

<span data-ttu-id="ce898-998">A Windows tűzfal mintavételi portot mindkét fürtcsomóponton tooopen van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ce898-998">You need tooopen a Windows firewall probe port on both cluster nodes.</span></span> <span data-ttu-id="ce898-999">A következő parancsfájl tooopen a Windows tűzfal mintavételi portot hello használata.</span><span class="sxs-lookup"><span data-stu-id="ce898-999">Use hello following script tooopen a Windows firewall probe port.</span></span> <span data-ttu-id="ce898-1000">Hello PowerShell változók a környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="ce898-1000">Update hello PowerShell variables for your environment.</span></span>

  ```PowerShell
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

<span data-ttu-id="ce898-1001">Hello **ProbePort** értéke túl**62000**.</span><span class="sxs-lookup"><span data-stu-id="ce898-1001">hello **ProbePort** is set too**62000**.</span></span> <span data-ttu-id="ce898-1002">Most már hozzáférhet a fájlmegosztáshoz hello  **\\\ascsha-clsap\sapmnt** a többi, például mert a **ascsha-dbas**.</span><span class="sxs-lookup"><span data-stu-id="ce898-1002">Now you can access hello file share **\\\ascsha-clsap\sapmnt** from other hosts, such as from **ascsha-dbas**.</span></span>

### <span data-ttu-id="ce898-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Hello adatbázis-példány telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-1003"><a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a> Install hello database instance</span></span>

<span data-ttu-id="ce898-1004">tooinstall hello adatbázis-példány, hajtsa végre az SAP-dokumentáció hello ismertetett hello folyamatot.</span><span class="sxs-lookup"><span data-stu-id="ce898-1004">tooinstall hello database instance, follow hello process described in hello SAP installation documentation.</span></span>

### <span data-ttu-id="ce898-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Hello második fürtcsomópont telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-1005"><a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a> Install hello second cluster node</span></span>

<span data-ttu-id="ce898-1006">tooinstall hello második fürt, hello SAP telepítési útmutató hello lépéseit kövesse.</span><span class="sxs-lookup"><span data-stu-id="ce898-1006">tooinstall hello second cluster, follow hello steps in hello SAP installation guide.</span></span>

### <span data-ttu-id="ce898-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Hello indítási típust hello SAP SSZON Windows szolgáltatáspéldány módosítása</span><span class="sxs-lookup"><span data-stu-id="ce898-1007"><a name="094bc895-31d4-4471-91cc-1513b64e406a"></a> Change hello start type of hello SAP ERS Windows service instance</span></span>

<span data-ttu-id="ce898-1008">Hello SAP SSZON Windows-szolgáltatás indítási típusát hello túl módosítása**automatikus (Késleltetett indítás)** mindkét fürtcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="ce898-1008">Change hello start type of hello SAP ERS Windows service too**Automatic (Delayed Start)** on both cluster nodes.</span></span>

![60. ábra: Módosítani hello SAP SSZON példány toodelayed automatikus hello szolgáltatás típusát][sap-ha-guide-figure-3050]

<span data-ttu-id="ce898-1010">_**60. ábra:** módosítani hello SAP SSZON példány toodelayed automatikus hello szolgáltatás típusát_</span><span class="sxs-lookup"><span data-stu-id="ce898-1010">_**Figure 60:** Change hello service type for hello SAP ERS instance toodelayed automatic_</span></span>

### <span data-ttu-id="ce898-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Hello SAP elsődleges kiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-1011"><a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a> Install hello SAP Primary Application Server</span></span>

<span data-ttu-id="ce898-1012">Hello elsődleges Application Server (szolgáltatói)-példány telepítése <*SID*> - di - 0 hello virtuális gépen, amely már a kijelölt toohost hello szolgáltatói CÍMEI.</span><span class="sxs-lookup"><span data-stu-id="ce898-1012">Install hello Primary Application Server (PAS) instance <*SID*>-di-0 on hello virtual machine that you've designated toohost hello PAS.</span></span> <span data-ttu-id="ce898-1013">Nincsenek függőségek a Azure vagy DataKeeper-specifikus beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ce898-1013">There are no dependencies on Azure or DataKeeper-specific settings.</span></span>

### <span data-ttu-id="ce898-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Hello SAP további alkalmazáskiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="ce898-1014"><a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a> Install hello SAP Additional Application Server</span></span>

<span data-ttu-id="ce898-1015">Egy SAP további Application Server (AAS) telepítse az összes hello virtuális gépekre, hogy már kijelölve toohost SAP Application Server-példány.</span><span class="sxs-lookup"><span data-stu-id="ce898-1015">Install an SAP Additional Application Server (AAS) on all hello virtual machines that you've designated toohost an SAP Application Server instance.</span></span> <span data-ttu-id="ce898-1016">Például a <*SID*> - di - 1 túl <*SID*> - di -&lt;n&gt;.</span><span class="sxs-lookup"><span data-stu-id="ce898-1016">For example, on <*SID*>-di-1 too<*SID*>-di-&lt;n&gt;.</span></span>

> [!NOTE]
> <span data-ttu-id="ce898-1017">Ez befejezi a magas rendelkezésre állású SAP NetWeaver rendszer hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="ce898-1017">This finishes hello installation of a high-availability SAP NetWeaver system.</span></span> <span data-ttu-id="ce898-1018">A következő folytatásához feladatátvételi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ce898-1018">Next, proceed with failover testing.</span></span>
>


## <span data-ttu-id="ce898-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Hello SAP ASC/SCS példány feladatátvétel és SIOS replikációs tesztelése</span><span class="sxs-lookup"><span data-stu-id="ce898-1019"><a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a> Test hello SAP ASCS/SCS instance failover and SIOS replication</span></span>
<span data-ttu-id="ce898-1020">Könnyen tootest, és figyelje az SAP ASC/SCS-példány feladatátvevő és SIOS lemez replikációs Feladatátvevőfürt-kezelő és hello SIOS DataKeeper felügyelete és konfigurálása eszköz.</span><span class="sxs-lookup"><span data-stu-id="ce898-1020">It's easy tootest and monitor an SAP ASCS/SCS instance failover and SIOS disk replication by using Failover Cluster Manager and hello SIOS DataKeeper Management and Configuration tool.</span></span>

### <span data-ttu-id="ce898-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>A fürtcsomóponton SAP ASC/SCS-példány fut.</span><span class="sxs-lookup"><span data-stu-id="ce898-1021"><a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a> SAP ASCS/SCS instance is running on cluster node A</span></span>

<span data-ttu-id="ce898-1022">Hello **SAP PR1** fürtcsoport fürtcsomóponton A. fut. Például a **pr1-ASC-0**.</span><span class="sxs-lookup"><span data-stu-id="ce898-1022">hello **SAP PR1** cluster group is running on cluster node A. For example, on **pr1-ascs-0**.</span></span> <span data-ttu-id="ce898-1023">Rendelje hozzá a megosztott hello meghajtó S, amely része hello **SAP PR1** fürterőforrás-csoport, és melyik hello ASC/SCS példányt használ, toocluster csomópont azonosítójához.</span><span class="sxs-lookup"><span data-stu-id="ce898-1023">Assign hello shared disk drive S, which is part of hello **SAP PR1** cluster group, and which hello ASCS/SCS instance uses, toocluster node A.</span></span>

![61. ábra: Feladatátvevőfürt-kezelő: hello SAP < SID > fürtcsoport A csomóponton fut.][sap-ha-guide-figure-5000]

<span data-ttu-id="ce898-1025">_**61. ábra:** Feladatátvevőfürt-kezelő: hello SAP <*SID*> fürtcsoport A csomóponton fut._</span><span class="sxs-lookup"><span data-stu-id="ce898-1025">_**Figure 61:** Failover Cluster Manager: hello SAP <*SID*> cluster group is running on cluster node A_</span></span>

<span data-ttu-id="ce898-1026">A hello SIOS DataKeeper felügyeleti és a konfigurációs eszközt hogy adatokat a rendszer szinkron módon replikálja a hello forrás kötet meghajtó fürtcsomóponton egy toohello kötet célmeghajtó S fürtcsomóponton b hello megosztott lemezt Például, hogy a rendszer replikálja a **pr1-ASC-0 [10.0.0.40]** túl**pr1-ASC-1 [10.0.0.41]**.</span><span class="sxs-lookup"><span data-stu-id="ce898-1026">In hello SIOS DataKeeper Management and Configuration tool, you can see that hello shared disk data is synchronously replicated from hello source volume drive S on cluster node A toohello target volume drive S on cluster node B. For example, it's replicated from **pr1-ascs-0 [10.0.0.40]** too**pr1-ascs-1 [10.0.0.41]**.</span></span>

![62. ábra: SIOS DataKeeper replikálja hello helyi kötet fürtcsomópontból B toocluster csomópont][sap-ha-guide-figure-5001]

<span data-ttu-id="ce898-1028">_**62. ábra:** SIOS DataKeeper, a fürtcsomópont toocluster csomópont B hello helyi kötet replikálásához_</span><span class="sxs-lookup"><span data-stu-id="ce898-1028">_**Figure 62:** In SIOS DataKeeper, replicate hello local volume from cluster node A toocluster node B_</span></span>

### <span data-ttu-id="ce898-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Feladatátvételt az A csomópont toonode B</span><span class="sxs-lookup"><span data-stu-id="ce898-1029"><a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a> Failover from node A toonode B</span></span>

1.  <span data-ttu-id="ce898-1030">Válasszon egyet az alábbi beállítások tooinitiate feladatátvételét hello SAP, <*SID*> csomópont toocluster fürtcsomópontból b fürtcsoport</span><span class="sxs-lookup"><span data-stu-id="ce898-1030">Choose one of these options tooinitiate a failover of hello SAP <*SID*> cluster group from cluster node A toocluster node B:</span></span>
  - <span data-ttu-id="ce898-1031">Feladatátvevőfürt-kezelővel</span><span class="sxs-lookup"><span data-stu-id="ce898-1031">Use Failover Cluster Manager</span></span>  
  - <span data-ttu-id="ce898-1032">Feladatátvevő fürt PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="ce898-1032">Use Failover Cluster PowerShell</span></span>

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  <span data-ttu-id="ce898-1033">Indítsa újra a fürt csomópont A hello Windows vendég operációs rendszerben (hello SAP, az automatikus feladatátvételt kezdeményez <*SID*> csomópont toonode B fürt csoportot).</span><span class="sxs-lookup"><span data-stu-id="ce898-1033">Restart cluster node A within hello Windows guest operating system (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>  
3.  <span data-ttu-id="ce898-1034">Indítsa újra a fürt csomópont A hello Azure-portálon való (hello SAP, az automatikus feladatátvételt kezdeményez <*SID*> csomópont toonode B fürt csoportot).</span><span class="sxs-lookup"><span data-stu-id="ce898-1034">Restart cluster node A from hello Azure portal (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>  
4.  <span data-ttu-id="ce898-1035">Indítsa újra a fürt csomópont A Azure PowerShell használatával (ez elindít egy hello SAP automatikus átvétele <*SID*> csomópont toonode B fürt csoportot).</span><span class="sxs-lookup"><span data-stu-id="ce898-1035">Restart cluster node A by using Azure PowerShell (this initiates an automatic failover of hello SAP <*SID*> cluster group from node A toonode B).</span></span>

  <span data-ttu-id="ce898-1036">A feladatátvétel után hello SAP <*SID*> fürtcsoport fürtcsomóponton b fut. Futó például **pr1-ASC-1**.</span><span class="sxs-lookup"><span data-stu-id="ce898-1036">After failover, hello SAP <*SID*> cluster group is running on cluster node B. For example, it's running on **pr1-ascs-1**.</span></span>

  ![63. ábra: A Feladatátvevőfürt-kezelőben, hello SAP < SID > fürtcsoport van fürtcsomóponton futó B][sap-ha-guide-figure-5002]

  <span data-ttu-id="ce898-1038">_**Ábra 63**: A Feladatátvevőfürt-kezelőben, hello SAP <*SID*> fürtcsoport fürtcsomóponton B fut._</span><span class="sxs-lookup"><span data-stu-id="ce898-1038">_**Figure 63**: In Failover Cluster Manager, hello SAP <*SID*> cluster group is running on cluster node B_</span></span>

  <span data-ttu-id="ce898-1039">hello megosztott lemez már csatlakoztatva van a fürt csomópont b SIOS DataKeeper replikálódik adatok forrás kötet meghajtóról S fürt csomópont B tootarget kötet meghajtó fürtcsomóponton A. A replikáló például **pr1-ASC-1 [10.0.0.41]** túl**pr1-ASC-0 [10.0.0.40]**.</span><span class="sxs-lookup"><span data-stu-id="ce898-1039">hello shared disk is now mounted on cluster node B. SIOS DataKeeper is replicating data from source volume drive S on cluster node B tootarget volume drive S on cluster node A. For example, it's replicating from **pr1-ascs-1 [10.0.0.41]** too**pr1-ascs-0 [10.0.0.40]**.</span></span>

  ![Ábra 64: SIOS DataKeeper replikálja hello helyi kötet csomópont B toocluster fürtcsomópontról A][sap-ha-guide-figure-5003]

  <span data-ttu-id="ce898-1041">_**Ábra 64:** SIOS DataKeeper hello helyi kötet csomópont B toocluster fürtcsomópontról A replikálja._</span><span class="sxs-lookup"><span data-stu-id="ce898-1041">_**Figure 64:** SIOS DataKeeper replicates hello local volume from cluster node B toocluster node A_</span></span>
