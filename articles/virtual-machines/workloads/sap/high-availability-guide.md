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
# <a name="high-availability-for-sap-netweaver-on-azure-vms"></a>Magas rendelkezésre állás a SAP NetWeaver Azure virtuális gépeken

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


Az Azure virtuális gépek egy hello megoldás olyan szervezeteknek, amelyek szükség van a számítási, tárolási és hálózati erőforrásokat, minimális időben, valamint hosszú beszerzési ciklusok nélkül. Azure virtuális gépek toodeploy klasszikus alkalmazások, például SAP NetWeaver alapú ABAP, Java és egy ABAP + Java verem használható. Megbízhatóság és rendelkezésre állás további helyszíni erőforrások nélkül kiterjesztése. Az Azure virtuális gépek közötti kapcsolatot nyújthassanak, támogatja, így a Azure Virtual Machines integrálása a szervezete helyszíni tartományok, magánfelhőket és SAP rendszer fekvő.

Ez a cikk azt lépésének hello, hogy elvégezhető toodeploy magas rendelkezésre állású SAP rendszerek az Azure-ban hello Azure Resource Manager telepítési modell használatával. A Microsoft végigvezetik Önt a nagyobb feladatok:

* Megkeresi a hello hello felsorolt megfelelő SAP megjegyzések és a telepítési útmutatók [erőforrások] [ sap-ha-guide-2] szakasz. Ez a cikk kiegészíti az SAP-dokumentáció és az SAP megjegyzések, amelyek hello elsődleges erőforrásokat, amelyek segítségével telepítse, és az adott platformon SAP szoftver központi telepítése.
* Ismerje meg, hogy hello különbségei hello Azure Resource Manager üzembe helyezési modellben és hello Azure klasszikus üzembe helyezési modellben.
* További információk a Windows Server feladatátvételi fürtszolgáltatási kvórummódok, kiválaszthatja az Azure-telepítés számára megfelelő hello modellt.
* További információk a Windows Server feladatátvételi fürtszolgáltatási megosztott tár az Azure-szolgáltatásokhoz.
* Ismerje meg, hogyan toohelp megvédeni egyetlen ponton-az-hibát jelentő összetevők például a fejlett üzleti alkalmazások fejlesztői (ABAP) SAP központi szolgáltatások (ASC) / SAP központi szolgáltatások (SCS) és az adatbázis-kezelő rendszerek (DBMS) és a redundáns összetevők, például SAP Alkalmazáskiszolgáló, az Azure-ban.
* Kövesse a részletes példa egy telepítési és a konfiguráció egy magas rendelkezésre állású SAP rendszer Windows Server feladatátvételi fürtszolgáltatási fürtben az Azure-ban Azure Resource Manager használatával.
* További tudnivalók a toouse szükséges további lépéseket a Windows Server feladatátvételi fürtszolgáltatási az Azure-ban, de amelyek esetén nem szükséges egy helyszíni környezetben.

toosimplify üzembe helyezését és konfigurálását, ebben a cikkben használjuk hello SAP háromrétegű magas rendelkezésre állású Resource Manager-sablonok. hello sablonok automatizálni hello teljes infrastruktúra, amelyekre szüksége van a magas rendelkezésre állású SAP-rendszer telepítését. hello infrastruktúra SAP alkalmazás teljesítmény szabványos (SAP) méretezése a SAP rendszert is támogatja.

## <a name="217c5479-5595-4cd8-870d-15ab00d4f84c"></a>Előfeltételek
Mielőtt elkezdené, győződjön meg arról, hogy teljesülnek-e a következő részekben hello ismertetett hello előfeltételek. Is a hello felsorolt összes erőforrást meg arról, hogy toocheck [erőforrások] [ sap-ha-guide-2] szakasz.

Ebben a cikkben az Azure Resource Manager sablonok használjuk [háromrétegű SAP NetWeaver](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image/). Sablonok hasznos áttekintéséért lásd: [SAP Azure Resource Manager-sablonok](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/).

## <a name="42b8f600-7ba3-4606-b8a5-53c4f026da08"></a>Erőforrások
Ezek a cikkek SAP üzemelő példányok Azure terjed ki:

* [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]
* [SAP NetWeaver Azure virtuális gépek központi telepítését][deployment-guide]
* [Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése][dbms-guide]
* [Az Azure virtuális gépek magas rendelkezésre állás a SAP NetWeaver (Ez az útmutató)][sap-ha-guide]

> [!NOTE]
> Amikor csak lehetséges, amelyben tudatjuk a felhasználókkal, egy hivatkozás toohello utaló SAP telepítési útmutató (lásd: hello [SAP telepítési útmutatók][sap-installation-guides]). Az Előfeltételek és hello telepítésének folyamatát, az a jó ötlet tooread hello SAP NetWeaver telepítési útmutató gondosan kapcsolatos információkat. Ez a cikk ismerteti a SAP NetWeaver-alapú rendszereken használható az Azure virtuális gépek csak meghatározott feladatok.
>
>

Ezek SAP azonban kapcsolódó toohello a témakör az SAP, az Azure-ban:

| Megjegyzés száma | Cím |
| --- | --- |
| [1928533] |Az Azure-on SAP-alkalmazásokból: támogatott termékek és méretezése |
| [2015553] |A Microsoft Azure SAP: Előfeltételek támogatja |
| [1999351] |SAP Azure figyelésének továbbfejlesztett |
| [2178632] |Kulcs figyelési metrikákat az SAP, a Microsoft Azure |
| [1999351] |A Windows virtualizálási: fokozott figyelése |
| [2243692] |A prémium szintű Azure SSD-tárolón SAP DBMS példány használata |

További tudnivalók hello [korlátozások az Azure-előfizetések][azure-subscription-service-limits-subscription], többek között az általános alapértelmezett korlátozások és a maximális korlátozások.

## <a name="42156640c6-01cf-45a9-b225-4baa678b24f1"></a>Magas rendelkezésre állású SAP az Azure Resource Manager és a hello Azure klasszikus telepítési modell
eltérő módon a következő területeken hello jelennek meg hello Azure Resource Manager és az Azure klasszikus üzembe helyezési modellek:

- Erőforráscsoportok
- Azure belső terheléselosztó függőség hello Azure erőforráscsoport betöltése
- SAP multi-SID forgatókönyvek támogatása

### <a name="f76af273-1993-4d83-b12d-65deeae23686"></a>Erőforráscsoportok
Az Azure Resource Manager segítségével is erőforrás csoportok toomanage hello összes alkalmazás-erőforrásokat az Azure-előfizetéshez. Integrált megközelítést, egy erőforráscsoportban található összes erőforrást rendelkezik hello azonos életciklusát. Például minden erőforrások jönnek létre: hello azonos idő és azok törlődnek hello ugyanannyi időt vesz igénybe. További információk az [erőforráscsoportokról](../../../azure-resource-manager/resource-group-overview.md#resource-groups).

### <a name="3e85fbe0-84b1-4892-87af-d9b65ff91860"></a>Azure belső terheléselosztó függőség hello Azure erőforráscsoport betöltése

Hello Azure klasszikus üzembe helyezési modellel hello Azure belső terheléselosztó (Azure terheléselosztó szolgáltatás) és hello felhőalapú szolgáltatás csoportja közötti függőség van. Minden belső terheléselosztónak egy felhőalapú szolgáltatás csoportot kell.

Az Azure erőforrás-kezelőben, nem kell egy Azure-erőforrás csoport toouse Azure Load Balancer. egyszerűbb és rugalmasabb hello környezete.

### <a name="support-for-sap-multi-sid-scenarios"></a>SAP multi-SID forgatókönyvek támogatása

Az Azure erőforrás-kezelőben, akkor több példányt is telepíthet SAP rendszer azonosítója (SID) ASC/SCS egy fürt. Minden Azure belső terheléselosztóhoz több IP-cím támogatásának köszönhetően multi-SID-példányok is előfordulhatnak.

toouse hello Azure klasszikus üzembe helyezési modellel, kövesse az ismertetett eljárások hello [SAP NetWeaver az Azure-ban: az Azure-ban SIOS DataKeeper segítségével a Windows Server feladatátvételi fürtszolgáltatási SAP ASC/SCS fürtszolgáltatási példányok](http://go.microsoft.com/fwlink/?LinkId=613056).

> [!IMPORTANT]
> Javasoljuk, hogy a SAP telepítések használjon hello Azure Resource Manager üzembe helyezési modellben. Nem érhetők el hello klasszikus üzembe helyezési modellel számos előnyt kínál. További tudnivalók az Azure [üzembe helyezési modellel][virtual-machines-azure-resource-manager-architecture-benefits-arm].   
>
>

## <a name="8ecf3ba0-67c0-4495-9c14-feec1a2255b7"></a>Windows Server feladatátvételi fürtszolgáltatás
Az egy magas rendelkezésre állású SAP ASC/SCS telepítése és az adatbázis-kezelő, a Windows hello foundation Windows Server feladatátvételi fürtszolgáltatás.

A feladatátvevő fürt egy 1 + n különálló kiszolgálók csoportja (csomópontok), amelyek együttműködése az alkalmazások és szolgáltatások tooincrease hello rendelkezésre állási. Csomópont hiba lép fel, ha a Windows Server feladatátvételi fürtszolgáltatási kiszámítja-e hibák merülnek fel a megfelelő megőrzésével hello számát a fürt tooprovide alkalmazásokhoz és szolgáltatásokhoz. Választhat másik kvórum módok tooachieve Feladatátvételi fürtszolgáltatással.

### <a name="1a3c5408-b168-46d6-99f5-4219ad1b1ff2"></a>Kvórummódok
Windows Server feladatátvételi fürtszolgáltatási használatakor négy kvórummódok közül választhat:

* **Csomóponttöbbség**. Hello fürt mindegyik csomópontján szavazati is. hello Ez azt jelenti, hogy a fürt működését csak a szavazatot, többsége a több mint fele hello szavazatot. Azt javasoljuk, hogy a csomópontok páratlan számú fürtöknél ezt a beállítást. Például egy hét csomópontos fürtben három csomópont sikertelen lehet, és hello fürt stills többsége éri el, és továbbra is toorun.  
* **Csomópont- és lemeztöbbség**. Minden csomópont- és a kijelölt (a tanúsító lemez) hello fürttároló is szavazásra, mikor érhetők el, és a kommunikáció. hello fürt működését csak hello többsége a szavazatot, ez azt jelenti, hogy a több mint fele hello szavazatot. Ez a mód környezetben fürt páros számú csomópont teljesen logikus. Ha a csomópontok fele hello és hello lemez online állapotban, hello fürt kifogástalan állapotban marad.
* **Csomópont- és fájlmegosztás többsége**. Minden csomópont plusz a kijelölt fájlmegosztást (a tanúsító fájlmegosztás), amelyet a rendszergazda hello hoz létre szavazásra, függetlenül hello csomópontok és a fájlmegosztás elérhetők-e, és a kommunikáció. hello fürt működését csak hello többsége a szavazatot, ez azt jelenti, hogy a több mint fele hello szavazatot. Ez a mód környezetben fürt páros számú csomópont teljesen logikus. Hasonló toohello csomópont- és lemeztöbbség módban, de a tanúsító lemez helyett egy tanúsító fájlmegosztást használja. Ebben a módban egyszerűen tooimplement, de ha hello fájlmegosztás maga nem magas rendelkezésre állású, azt válnak a hibaérzékeny pontok kialakulását.
* **Nincs többség: Csak lemez**. hello fürt rendelkezik egy kvórum, ha egy csomópont elérhető, és hello fürttároló egy meghatározott lemezével kommunikál. Csak hello csomópontok is, hogy a lemez kommunikál hello fürt csatlakozhat. Azt javasoljuk, hogy nem használja ezt a módot.
 

## <a name="fdfee875-6e66-483a-a343-14bbaee33275"></a>Windows Server feladatátvételi fürtszolgáltatás a helyszíni
1. ábra mutatja a két csomópontból álló fürtben. Ha hello hello csomópontok közötti hálózati kapcsolat hibája esetén, és mindkét csomópont mentése marad, és fut, a kvórum lemez vagy fájlmegosztás meghatározza, hogy melyik csomópont továbbra is tooprovide hello fürt alkalmazásokhoz és szolgáltatásokhoz. hello csomópont, amelynek hozzáférési toohello kvórumlemez vagy fájlmegosztási hello csomópont, amely biztosítja, hogy a szolgáltatások továbbra is.

Ez a példa egy két csomópontot tartalmazó fürtben, mert hello csomópont- és fájlmegosztástöbbség kvórummód használjuk. hello csomópont- és lemeztöbbség is beállítás érvényes. Éles környezetben azt javasoljuk, hogy egy kvórumlemez használjon. Hálózati és tárolási rendszer technológia toomake használhatja azt magas rendelkezésre állású.

![1. ábra: Példa egy Windows Server feladatátvételi fürtszolgáltatás konfigurációs az SAP ASC/SCS az Azure-ban][sap-ha-guide-figure-1000]

_**1. ábra:** egy Windows Server feladatátvételi fürtszolgáltatás konfigurációs az SAP ASC/SCS az Azure-ban – példa_

### <a name="be21cf3e-fb01-402b-9955-54fbecf66592"></a>Megosztott tároló
1. ábra is mutatja egy két csomópontos megosztott tárolófürt. Egy helyi megosztott tárolási fürt hello fürt összes csomópontjának észleli a megosztott tároló. Zárolási mechanizmus hello adatok sérülés elleni védelmét. Minden csomópont észleli, ha egy másik csomópont meghibásodik. Ha egy csomópont meghibásodik, hello megmaradó csomópontra megkapja a tulajdonjogot hello tárolási erőforrások és szolgáltatások hello rendelkezésre állását biztosítja.

> [!NOTE]
> Például a magas rendelkezésre állás az egyes adatbázis-kezelő alkalmazások, egy megosztott lemez nem kell az SQL Server. SQL Server Always On fájlokat replikálja a DBMS adatainak és naplókönyvtárainak hello helyi lemez egy fürt csomópont toohello helyi lemez egy másik csomópont. Hello Windows fürtkonfiguráció ebben az esetben egy megosztott lemez nem szükséges.
>
>

### <a name="ff7a9a06-2bc5-4b20-860a-46cdb44669cd"></a>Hálózati és a névfeloldás
Ügyfélszámítógépek elérni hello fürt egy virtuális IP-címet és egy virtuális nevet adott hello DNS-kiszolgáló biztosítja. hello helyszíni csomópontok és hello DNS-kiszolgáló kezelni tud a több IP-címet.

A hagyományos telepítés, a két vagy több hálózati kapcsolatot használja:

* Egy dedikált kapcsolat toohello tároló
* Fürt belső hálózati kapcsolatot a hello szívverés
* Egy nyilvános hálózat, hogy az ügyfelek tooconnect toohello fürt használatára

## <a name="2ddba413-a7f5-4e4e-9a51-87908879c10a"></a>Windows Server feladatátvételi fürtszolgáltatás az Azure-ban
Képest toobare nélküli vagy saját felhőben történő alkalmazáshoz, Azure virtuális gépek Windows Server feladatátvételi fürtszolgáltatás további lépéseket tooconfigure igényel. Egy megosztott fürtlemez összeállításakor szüksége több IP-címek és virtuális állomás neve tooset hello SAP ASC/SCS példány.

Ebben a cikkben azt ismertetik az alapfogalmakat és hello további lépéseket szükséges toobuild egy SAP központi szolgáltatások magas rendelkezésre állású fürthöz az Azure-ban. Megmutatjuk, hogyan tooset hello külső eszköz SIOS DataKeeper fel, és hogyan tooconfigure hello Azure belső terheléselosztó. Ezen eszközök toocreate Windows feladatátvevő fürt egy tanúsító fájlmegosztást az Azure-ban használható.

![2. ábra: A Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban egy megosztott lemez nélkül][sap-ha-guide-figure-1001]

_**2. ábra:** Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban egy megosztott lemez nélkül_

### <a name="1a464091-922b-48d7-9d08-7cecf757f341"></a>SIOS DataKeeper megosztott lemezt az Azure-ban
A magas rendelkezésre állású SAP ASC/SCS példányához megosztott tárolót kell fürtbe. 2016 szeptemberétől Azure nem munkaajánlat megosztott tároló, amely szerint a megosztott tárolófürt toocreate is használhatja. Harmadik féltől származó szoftverek SIOS DataKeeper Cluster Edition toocreate tükrözött tárolási funkciókat biztosító, amely a fürt megosztott tárolási szimulálja is használhatja. hello SIOS megoldást biztosít a valós idejű szinkron replikálása. Ez az, hogy hogyan hozhat létre a fürt egy megosztott lemez erőforrás:

1. Rendeljen egy további Azure virtuális merevlemez (VHD) tooeach hello virtuális gépek (VM) a Windows fürtkonfiguráció.
2. Futtassa a SIOS DataKeeper Cluster Edition mindkét virtuális gép csomóponton.
3. Konfigurálása SIOS DataKeeper Cluster Edition, így azt tükrözi hello tartalom hello további csatolt virtuális merevlemez kötetének kötetről hello forrás virtuális gép toohello további csatolt virtuális merevlemez hello cél virtuális gép. SIOS DataKeeper kivonatolja hello forrás- és helyi köteteken, és adja őket az tooWindows Server feladatátvételi fürtszolgáltatási egy megosztott lemezként jeleníti.

További információk [SIOS DataKeeper](http://us.sios.com/products/datakeeper-cluster/).

![3. ábra: A Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban SIOS DataKeeper][sap-ha-guide-figure-1002]

_**3. ábra:** Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban SIOS DataKeeper_

> [!NOTE]
> Nincs szükség a megosztott lemez néhány DBMS termékekkel, például az SQL Server magas rendelkezésre állásra. SQL Server Always On fájlokat replikálja a DBMS adatainak és naplókönyvtárainak hello helyi lemez egy fürt csomópont toohello helyi lemez egy másik csomópont. Hello Windows fürtkonfiguráció ebben az esetben egy megosztott lemez nem szükséges.
>
>

### <a name="44641e18-a94e-431f-95ff-303ab65e0bcb"></a>A névfeloldás az Azure-ban
hello Azure cloud platform hello beállítás tooconfigure virtuális IP-címek, például csak fix IP-címek nem kínál. Az alternatív megoldások tooset be egy virtuális IP-cím tooreach hello fürt erőforrás hello felhőben van szüksége.
Azure hello Azure terheléselosztó szolgáltatás egy belső terheléselosztóhoz tartozik. Hello belső terheléselosztóhoz, az ügyfelek elérni hello fürt keresztül hello virtuális IP-címet.
Toodeploy hello belső terheléselosztó, amely tartalmazza a hello fürtcsomópontok hello erőforráscsoportban van szüksége. Ezt követően konfigurálja a szabályok továbbítási hello mintavételi hello belső terheléselosztó portok az összes szükséges portot.
hello az ügyfélszámítógépek csatlakozhatnak keresztül hello virtuális állomás neve. hello DNS-kiszolgáló oldja fel a hello IP-címet, és hello belső terheléselosztó leírók port továbbítási toohello hello fürtjének aktív csomópontja.

## <a name="2e3fec50-241e-441b-8708-0b1864f66dfa"></a>SAP NetWeaver magas rendelkezésre állás érdekében az Azure infrastruktúra,--szolgáltatás (IaaS)
tooachieve alkalmazás magas rendelkezésre állású, SAP, például SAP szoftver összetevők, a következő összetevők tooprotect hello kell:

* SAP Application Server-példány
* SAP ASC/SCS példány
* Adatbázis-kezelő kiszolgáló

Tudnivalók az SAP-összetevők a magas rendelkezésre állású forgatókönyvek védelméről további információkért lásd: [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver](planning-guide.md).

### <a name="93faa747-907e-440a-b00a-1ae0a89b1c0e"></a>Magas rendelkezésre állású SAP-alkalmazáskiszolgáló
Általában nem szükséges egy adott magas rendelkezésre állású megoldás hello SAP-kiszolgáló és a párbeszédpanel-példányok. Magas rendelkezésre állású redundancia által érhetők el, és másik példányát az Azure virtuális gépek több párbeszédpanel példánya konfigurálhatja. Két példányban Azure virtuális gépek telepítése legalább két SAP alkalmazáspéldányok kell rendelkeznie.

![4. ábra: Magas rendelkezésre állású SAP-alkalmazáskiszolgáló][sap-ha-guide-figure-2000]

_**4. ábra:** magas rendelkezésre állású SAP-alkalmazáskiszolgáló_

El kell helyezni, hogy a gazdagép-SAP Application Server példánya hello azonos Azure rendelkezésre állási csoport összes virtuális gépet. Az Azure rendelkezésre állási csoportok biztosítja, hogy:

* Az összes virtuális gép hello részét képező azonos frissítési tartományban. Frissítési tartományokhoz, például gondoskodik arról, hogy hello virtuális gépek nem frissített hello karbantartási tervezett leállások során ugyanannyi időt vesz igénybe.
* Az összes virtuális gép hello részét képező azonos tartalék tartományban. A tartalék tartomány például gondoskodik arról, hogy telepít virtuális gépeket, hogy nincs hibaérzékeny pontok kialakulását hatással van az összes virtuális gép hello rendelkezésre állását.

További tudnivalók túl[hello virtuális gépek rendelkezésre állásának kezelése][virtual-machines-manage-availability].

Mivel hello Azure storage-fiók egy potenciális hibaérzékeny pontok kialakulását, fontos toohave is legalább két Azure storage-fiókok, legalább két virtuális gép szétosztva. Az ideális beállítás az egyes virtuális gépek egy SAP párbeszédpanel példányát futtató hello lemezek során eltérő tárfiók volna telepíthetők.

### <a name="f559c285-ee68-4eec-add1-f60fe7b978db"></a>Magas rendelkezésre állású SAP ASC/SCS példány
5. ábra egy példát a magas rendelkezésre állású SAP ASC/SCS példánya.

![5. ábra: Magas rendelkezésre állású SAP ASC/SCS példány][sap-ha-guide-figure-2001]

_**5. ábra:** magas rendelkezésre állású SAP ASC/SCS példány_

#### <a name="b5b1fd0b-1db4-4d49-9162-de07a0132a51"></a>SAP ASC/SCS példány magas rendelkezésre állását a Windows Server feladatátvételi fürtszolgáltatási az Azure-ban
Képest toobare nélküli vagy saját felhőben történő alkalmazáshoz, Azure virtuális gépek Windows Server feladatátvételi fürtszolgáltatás további lépéseket tooconfigure igényel. toobuild Windows feladatátvevő fürt, szüksége megosztott fürtlemezre mutat, több IP-címek, több virtuális állomásnevek és az Azure belső terheléselosztót a fürtszolgáltatás egy SAP ASC/SCS példányát. A Microsoft hello cikk későbbi részében részletesebben ismertetik el.

![6. ábra: A Windows Server feladatátvételi fürtszolgáltatási SIOS DataKeeper használatával az Azure-ban SAP ASC/SCS konfigurációhoz][sap-ha-guide-figure-1002]

_**6. ábra:** Windows Server feladatátvételi fürtszolgáltatási az Azure-ban SIOS DataKeeper SAP ASC/SCS konfigurációhoz_

### <a name="ddd878a0-9c2f-4b8e-8968-26ce60be1027"></a>Magas rendelkezésre állású DBMS példány
hello adatbázis-kezelő a rendszer egyetlen pont, forduljon egy SAP rendszerben. Tooprotect kell azt a magas rendelkezésre állású megoldás segítségével. 7. ábra mutatja egy SQL Server Always On magas rendelkezésre állású megoldás az Azure-ban, a Windows Server feladatátvételi fürtszolgáltatási és hello Azure belső terheléselosztó. SQL Server Always On DBMS adatainak és naplókönyvtárainak fájlokat replikálja a saját adatbázis-kezelő replikáció használatával. Ebben az esetben akkor nem kell a fürt megosztott lemezeket, egyszerűbbé teszi a teljes hello beállítása.

![7. ábra: Példa egy magas rendelkezésre állású SAP DBMS, az SQL Server Always On][sap-ha-guide-figure-2003]

_**7. ábra:** egy magas rendelkezésre állású SAP DBMS, az SQL Server Always On – példa_

Hello Azure Resource Manager telepítési modell segítségével az Azure SQL Server fürtszolgáltatással kapcsolatos további információkért lásd: ezek a cikkek:

* [Always On rendelkezésre állási csoport konfigurálásához Azure virtuális gépek manuálisan erőforrás-kezelő használatával][virtual-machines-windows-portal-sql-alwayson-availability-groups-manual]
* [Egy Azure belső terheléselosztót egy Always On rendelkezésre állási csoport konfigurálása az Azure-ban][virtual-machines-windows-portal-sql-alwayson-int-listener]

## <a name="045252ed-0277-4fc8-8f46-c5a29694a816"></a>Végpontok közötti magas rendelkezésre állású környezetekben

### <a name="deployment-scenario-using-architectural-template-1"></a>Üzembe helyezési forgatókönyv segítségével 1. sablon architekturális.

8. ábrán egy SAP NetWeaver magas rendelkezésre állású architektúra példáját mutatja be az Azure-ban **egy** SAP rendszer. Ebben a forgatókönyvben be van állítva az alábbiak szerint:

- Egy kijelölt fürt hello SAP ASC/SCS példányát használja.
- Egy kijelölt fürt hello DBMS példányát használja.
- SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépek vannak telepítve.

![8. ábra: SAP magas rendelkezésre állású architekturális sablon 1, a kijelölt fürt ASC/SCS és adatbázis-kezelő][sap-ha-guide-figure-2004]

_**8. ábra:** SAP architekturális sablon 1 magas rendelkezésre állású, dedikált fürtök ASC/SCS és adatbázis-kezelő_

### <a name="deployment-scenario-using-architectural-template-2"></a>Üzembe helyezési forgatókönyv architekturális sablon 2 használatával

9. ábra egy SAP NetWeaver magas rendelkezésre állású architektúra példáját mutatja be az Azure-ban **egy** SAP rendszer. Ebben a forgatókönyvben be van állítva az alábbiak szerint:

- Egy kijelölt fürt használt **mindkét** hello SAP ASC/SCS példányt, és az adatbázis-kezelő hello.
- SAP alkalmazáskiszolgáló-példányok saját dedikált virtuális gépek vannak telepítve.

![9. ábra: SAP magas rendelkezésre állású architekturális sablon 2, a dedikált fürtökben ASC/SCS és egy dedikált adatbázis-kezelő fürt][sap-ha-guide-figure-2005]

_**9. ábra:** SAP magas rendelkezésre állású architekturális sablon 2, a dedikált fürtökben ASC/SCS és egy dedikált adatbázis-kezelő fürt_

### <a name="deployment-scenario-using-architectural-template-3"></a>Üzembe helyezési forgatókönyv a 3. sablon architekturális.

10. ábrán látható egy példa egy SAP NetWeaver magas rendelkezésre állású architektúra az Azure-ban **két** rendszerek, a SAP &lt;SID1&gt; és &lt;SID2&gt;. Ebben a forgatókönyvben be van állítva az alábbiak szerint:

- Egy kijelölt fürt használt **mindkét** hello SAP ASC/SCS SID1 példány *és* hello SAP ASC/SCS SID2 példány (egy fürt).
- Egy kijelölt fürt DBMS SID1 szolgál, és egy másik kijelölt fürt használt adatbázis-kezelő SID2 (fürtök).
- SAP alkalmazáskiszolgáló példányainak hello SAP rendszer SID1 rendelkezik saját dedikált virtuális gépek.
- SAP alkalmazáskiszolgáló példányainak hello SAP rendszer SID2 rendelkezik saját dedikált virtuális gépek.

![10. ábra: SAP magas rendelkezésre állású architekturális sablon 3, egy dedikált fürttel különböző ASC/SCS-példányok][sap-ha-guide-figure-6003]

_**10. ábra:** SAP magas rendelkezésre állású architekturális sablon 3, egy dedikált fürttel különböző ASC/SCS-példányok_

## <a name="78092dbe-165b-454c-92f5-4972bdbef9bf"></a>Hello infrastruktúra előkészítése

### <a name="prepare-hello-infrastructure-for-architectural-template-1"></a>1. sablon architekturális. hello infrastruktúra előkészítése
Az SAP Azure Resource Manager-sablonok segítségével egyszerűsítheti a szükséges erőforrások.

hello háromrétegű sablonok az Azure Resource Manager is támogatja a magas rendelkezésre állású forgatókönyvek, például architekturális sablon az 1., amely két fürttel rendelkezik. Minden fürthöz egy SAP hibaérzékeny pontot SAP ASC/SCS és adatbázis-kezelő.

Itt található, ahol kaphat Azure Resource Manager-sablonok azt írják le, a cikkben hello. példa:

* [Kép: Azure piactér](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image)  
* [Kép: egyéni](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image)

hello infrastruktúra tooprepare architekturális sablon 1:

- Az Azure portál, a hello hello **paraméterek** paneljén, hello **SYSTEMAVAILABILITY** mezőben válassza **magas rendelkezésre ÁLLÁSÚ**.

  ![11. ábra: Beállítása SAP magas rendelkezésre állású Azure Resource Manager-paraméterek][sap-ha-guide-figure-3000]

_**11. ábra:** beállítása SAP magas rendelkezésre állású Azure Resource Manager-paraméterek_


  hello sablonok létrehozása:

  * **Virtuális gépek**:
    * SAP alkalmazáskiszolgáló virtuális gépek: <*SAPSystemSID*> - di - <*száma*>
    * A fürt virtuális gépek ASC/SCS: <*SAPSystemSID*> - ASC - <*száma*>
    * Adatbázis-kezelő fürt: <*SAPSystemSID*> - db - <*száma*>

  * **Hálózati kártya tartozó összes virtuális géphez társított IP-címekkel rendelkező**:
    * <*SAPSystemSID*> - nic - di - <*száma*>
    * <*SAPSystemSID*> - nic - ASC - <*száma*>
    * <*SAPSystemSID*> - nic - db - <*száma*>

  * **Az Azure storage-fiókok**

  * **Rendelkezésre állási csoportok** esetében:
    * SAP alkalmazáskiszolgáló virtuális gépek: <*SAPSystemSID*> - avset - di
    * SAP ASC/SCS cluster virtuális gépek: <*SAPSystemSID*> - avset - ASC
    * Adatbázis-kezelő fürt virtuális gépek: <*SAPSystemSID*> - avset - adatbázis

  * **Az Azure belső terheléselosztó**:
    * Az összes port hello ASC/SCS példányhoz, és az IP-cím <*SAPSystemSID*> - lb - ASC
    * Az összes port hello SQL Server adatbázis-kezelő és az IP-cím <*SAPSystemSID*> - lb - adatbázis

  * **Hálózati biztonsági csoport**: <*SAPSystemSID*> - nsg - ASC-0  
    * Egy nyitott külső Remote Desktop Protocol (RDP) port toohello a <*SAPSystemSID*> virtuálisgép - ASC - 0

> [!NOTE]
> Az összes IP-címet oszt hello hálózati kártyák és az Azure belső terheléselosztók **dinamikus** alapértelmezés szerint. Túl módosításukra**statikus** IP-címeket. Azt ismerteti, hogy hogyan toodo ez hello cikk későbbi részében.
>
>

### <a name="c87a8d3f-b1dc-4d2f-b23c-da4b72977489"></a>Virtuális gépek telepítése a vállalati hálózati kapcsolat (létesítmények közötti) toouse éles környezetben
Éles rendszerek esetén SAP, az Azure virtuális gépek telepítése a [vállalati hálózati kapcsolat (létesítmények közötti)] [ planning-guide-2.2] Azure hely-hely típusú VPN vagy Azure ExpressRoute segítségével.

> [!NOTE]
> Az Azure Virtual Network-példányt is használhat. hello virtuális hálózat és alhálózat már létrehozott és előkészítése.
>
>

1.  Az Azure portál, a hello hello **paraméterek** paneljén, hello **NEWOREXISTINGSUBNET** mezőben válassza **meglévő**.
2.  A hello **SUBNETID** mezőben adja meg az elkészített Azure hálózati tervezett toodeploy az Azure virtuális gépek SubnetID hello teljes karakterlánc.
3.  az összes Azure-hálózat alhálózatok listájának tooget futtassa a PowerShell-parancsot:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets
  ```

  Hello **azonosító** mezőben látható hello **SUBNETID**.
4. az összes tooget **SUBNETID** értékek, futtassa a PowerShell-parancsot:

  ```PowerShell
  (Get-AzureRmVirtualNetwork -Name <azureVnetName>  -ResourceGroupName <ResourceGroupOfVNET>).Subnets.Id
  ```

  Hello **SUBNETID** dolgozunk:

  ```
  /subscriptions/<SubscriptionId>/resourceGroups/<VPNName>/providers/Microsoft.Network/virtualNetworks/azureVnet/subnets/<SubnetName>
  ```

### <a name="7fe9af0e-3cce-495b-a5ec-dcb4d8e0a310"></a>A tesztelési és bemutató csak felhőalapú SAP-példányok telepítése
Egy kizárólag felhőalapú üzembe helyezési modellben a magas rendelkezésre állású SAP rendszereket telepítheti központilag. Ez a központi telepítési típus elsősorban akkor hasznos, bemutató és tesztelési használatából adódó. Nem alkalmas a termelési használati eseteket.

- Az Azure portál, a hello hello **paraméterek** paneljén, hello **NEWOREXISTINGSUBNET** mezőben válassza **új**. Hagyja hello **SUBNETID** mező üres.

  hello SAP Azure Resource Manager sablon automatikusan létrehozza a hello Azure virtuális hálózat és alhálózat.

> [!NOTE]
> Toodeploy is szükség van legalább egy virtuális gép dedikált az Active Directory és DNS-ét hello ugyanazon Azure-beli virtuális hálózathoz. hello sablon nem hoz létre a virtuális gépek.
>
>


### <a name="prepare-hello-infrastructure-for-architectural-template-2"></a>Hello infrastruktúra előkészítése a 2. sablon architekturális.

Az Azure Resource Manager sablon használhatja az SAP toohelp egyszerűsítése SAP architekturális sablon 2 álló szükséges infrastruktúra központi telepítését.

Itt található, ahol kaphat Azure Resource Manager-sablonok a központi telepítési forgatókönyv:

* [Kép: Azure piactér](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-marketplace-image-converged)  
* [Kép: egyéni](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-3-tier-user-image-converged)


### <a name="prepare-hello-infrastructure-for-architectural-template-3"></a>Hello infrastruktúra előkészítése a 3. sablon architekturális.

Hello infrastruktúra előkészítése, és konfigurálja az SAP **multi-SID**. Például hozzáadhat egy SAP ASC/SCS másodpéldányt be egy *meglévő* fürtkonfiguráció. További információkért lásd: [konfigurálása egy SAP ASC/SCS másodpéldányt be egy meglévő fürt konfigurációs toocreate egy SAP multi-SID-konfigurációját az Azure Resource Manager][sap-ha-multi-sid-guide].

Ha azt szeretné, hogy egy új multi-SID-fürt toocreate, használhatja a hello multi-SID [gyorsindítási sablonok a Githubon](https://github.com/Azure/azure-quickstart-templates).
toocreate multi-SID új fürt, a következő három sablonok toodeploy hello kell:

* [Asc/SCS sablon](#ASCS-SCS-template)
* [Adatbázis-sablon](#database-template)
* [Alkalmazássablon-kiszolgálók](#application-servers-template)

hello alábbiakban rendelkezik hello sablonok és hello paraméterek hello sablonok tooprovide van szüksége további információt.

#### <a name="ASCS-SCS-template"></a>Asc/SCS sablon

hello ASC/SCS sablon használható a Windows Server feladatátvevő fürt, amelyen több ASC/SCS példány toocreate két virtuális gép telepítését.

hello ASC/SCS multi-SID-sablon hello mentése tooset [ASC/SCS multi-SID sablon][sap-templates-3-tier-multisid-xscs-marketplace-image], adja meg a következő paraméterek hello:

  - **Erőforrás-előtag**.  Állítsa be a hello erőforrás előtag használt tooprefix hello központi telepítése során létrehozott összes erőforrást. Hello erőforrások nem tartoznak tooonly egy SAP rendszer, mert hello előtag hello erőforrás nincs hello egy SAP rendszer biztonsági azonosítója.  hello előtagnak közé kell esnie **3-6 karakter**.
  - **A verem típus**. Válassza ki a hello SAP rendszer hello verem típusát. Hello verem típusától függően a Azure Load Balancer egy (ABAP vagy csak a Java), vagy két (ABAP + Java) magánhálózati IP-címek SAP rendszerenként rendelkezik.
  -  **Operációs rendszer típusa**. Válassza ki a hello virtuális gép operációs rendszerének hello.
  -  **SAP rendszer száma**. Válassza ki a hello szám SAP rendszerek kívánt tooinstall ennek a fürtnek.
  -  **Rendelkezésre állására**. Válassza ki **magas rendelkezésre ÁLLÁSÚ**.
  -  **Rendszergazda felhasználónevét és a rendszergazdai jelszó**. Hozzon létre egy új felhasználót, amely használt toosign toohello gépen.
  -  **Új vagy meglévő alhálózati**. Állítsa be, hogy egy új virtuális hálózat és alhálózat kell létrehozni, vagy használjon egy létező alhálózatot. Ha már van egy virtuális hálózatot, amely csatlakoztatott tooyour a helyszíni hálózat, válassza ki a **meglévő**.
  -  **Alhálózati azonosító**. Hello azonosítója hello alhálózati toowhich hello virtuális gépek kell csatlakoztatni. Válassza ki a virtuális magánhálózati (VPN) vagy ExpressRoute virtuális hálózat tooconnect hello virtuális gép tooyour a helyszíni hálózati hello alhálózati. hello azonosítója általában néz ki:

   /Subscriptions/ <*előfizetés-azonosító*> /resourceGroups/ <*erőforráscsoport-név*> /providers/Microsoft.Network/virtualNetworks/ <*virtuálishálózat-név*> /subnets/ <*alhálózat neve*>

hello sablon Azure Load Balancer egy példány, amely támogatja a több SAP rendszer telepíti.

- hello ASC példányok példányszámának 00, 10, 20 beállítást...
- hello SCS példányok példányszámának 01, 11, 21 beállítást...
- hello ASC sorba helyezni replikációs Server (SSZON) (csak Linux) példányok példányszámának 02, 12, 22 beállítást...
- hello SCS SSZON (csak Linux) példányok példányszámának 03., 13, 23 beállítást...

hello terheléselosztó tartalmazza (Linux 2) 1 VIP(s), a ASC/SCS 1 x-VIP és a SSZON (csak Linux esetén) 1 x-VIP.

hello alábbi lista tartalmazza-e minden terheléselosztási szabályok (ahol x egy SAP rendszer, például 1, 2, 3... hello hello száma):
- Minden SAP rendszerhez a Windows-specifikus portokat: 445-ös, 5985
- Asc portok (x0 száma): 32 x 0, 36 x 0, 39 x 0, 81-es x 0, 5 x 013, 5 x 014, 5 x 016
- SCS portok (x1 száma): 32 x 1, 33 x 1, 39 x 1, 81-es x 1, 5 x 113, 5 x 114, 5 x 116
- Linux (példányszámának x2) portok ASC SSZON: 33 x 2, 5 x 213, 5 x 214, 5 x 216
- Linux (példányszámának x3) portok SCS SSZON: 33 x 3, 5 x 313, 5 x 314, 5 x 316

hello terheléselosztó konfigurált toouse hello mintavételi portot (ahol x egy SAP rendszer, például 1, 2, 3... hello hello száma) a következő:
- Asc/SCS belső terheléselosztó mintavételi portot betölteni: 620 x 0
- SSZON belső terheléselosztó mintavételi portot (csak Linux) betöltése: 621 x 2

#### <a name="database-template"></a>Adatbázis-sablon

hello adatbázis sablon telepít egy vagy két virtuális gép használható tooinstall hello egy SAP rendszer relációs adatbázis-kezelő rendszerének (RDBMS). Például öt SAP rendszerekhez ASC/SCS sablonná telepíti, ha szüksége toodeploy Ez a sablon ötször.

hello adatbázis multi-SID-sablon, hello mentése tooset [adatbázis multi-SID sablon][sap-templates-3-tier-multisid-db-marketplace-image], adja meg a következő paraméterek hello:

  -  **SAP rendszerazonosító**. Adja meg azt szeretné, hogy tooinstall SAP rendszer hello hello SAP rendszer Azonosítóját. hello azonosító használandó előtagjaként hello telepített erőforrások esetén.
  -  **Operációs rendszer típusa**. Válassza ki a hello virtuális gép operációs rendszerének hello.
  -  **DbType**. Hello típusának kiválasztása hello adatbázis kívánt tooinstall hello fürtön. Válassza ki **SQL** Ha azt szeretné, hogy a Microsoft SQL Server tooinstall. Válassza ki **HANA** tudás az SAP HANA tooinstall hello virtuális gépeken. Győződjön meg arról, hogy tooselect hello megfelelő operációs rendszer típusa: válassza ki **Windows** SQL, és válasszon egy Linux terjesztési Hana. hello Azure Load Balancer, amely virtuális gépek lesznek csatlakoztatott toohello beállított toosupport kijelölt hello adatbázis típus:
    * **SQL**. hello terheléselosztó fog terheléselosztásához 1433-as port. Győződjön meg arról, hogy toouse ezt a portot, az SQL Server Always On beállítás.
    * **HANA**. hello terheléselosztó fog terheléselosztásához 35015 és 35017 portot. Győződjön meg arról, hogy tooinstall SAP HANA rendelkező példányszámának **50**.
    hello terheléselosztó 62550 mintavételi portot fogja használni.
  -  **SAP mérete**. SAP hello új rendszer hello számának beállítása ad meg. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.
  -  **Rendelkezésre állására**. Válassza ki **magas rendelkezésre ÁLLÁSÚ**.
  -  **Rendszergazda felhasználónevét és a rendszergazdai jelszó**. Hozzon létre egy új felhasználót, amely használt toosign toohello gépen.
  -  **Alhálózati azonosító**. Adja meg a hello hello alhálózat hello hello ASC/SCS sablon telepítése során használt, vagy hello azonosítója hello alhálózat létrehozott hello ASC/SCS sablon telepítésének részeként.

#### <a name="application-servers-template"></a>Alkalmazássablon-kiszolgálók

hello kiszolgálók alkalmazássablon SAP alkalmazáskiszolgáló-példányok egy SAP-rendszerhez használható két vagy több virtuális gépek telepíti. Például öt SAP rendszerekhez ASC/SCS sablonná telepíti, ha szüksége toodeploy Ez a sablon ötször.

hello alkalmazás kiszolgálók multi-SID-sablon, hello mentése tooset [kiszolgálók multi-SID alkalmazássablon][sap-templates-3-tier-multisid-apps-marketplace-image], adja meg a következő paraméterek hello:

  -  **SAP rendszerazonosító**. Adja meg azt szeretné, hogy tooinstall SAP rendszer hello hello SAP rendszer Azonosítóját. hello azonosító használandó előtagjaként hello telepített erőforrások esetén.
  -  **Operációs rendszer típusa**. Válassza ki a hello virtuális gép operációs rendszerének hello.
  -  **SAP mérete**. SAP hello új rendszer hello száma ad meg. Ha nem biztos abban, hogy hány SAP hello rendszer igényel, kérje meg a SAP technológia Partner vagy a rendszer integráló.
  -  **Rendelkezésre állására**. Válassza ki **magas rendelkezésre ÁLLÁSÚ**.
  -  **Rendszergazda felhasználónevét és a rendszergazdai jelszó**. Hozzon létre egy új felhasználót, amely használt toosign toohello gépen.
  -  **Alhálózati azonosító**. Adja meg a hello hello alhálózat hello hello ASC/SCS sablon telepítése során használt, vagy hello azonosítója hello alhálózat létrehozott hello ASC/SCS sablon telepítésének részeként.


### <a name="47d5300a-a830-41d4-83dd-1a0d1ffdbe6a"></a>Azure-beli virtuális hálózat
A jelen példában hello hello Azure-beli virtuális hálózat címtartománya 10.0.0.0/16. Egy alhálózat neve van **alhálózati**, a 10.0.0.0/24 címtartományt. A virtuális hálózaton található virtuális gépek és a belső terheléselosztók vannak telepítve.

> [!IMPORTANT]
> Nem módosításokat belül hello vendég operációs rendszer toohello hálózati beállításokat. Ez magában foglalja az IP-címek, a DNS-kiszolgálók és az alhálózatot. A hálózati beállítások konfigurálása az Azure-ban. hello Dynamic Host Configuration Protocol (DHCP) szolgáltatás tölti ki a beállításokat.
>
>

### <a name="b22d7b3b-4343-40ff-a319-097e13f62f9e"></a>DNS-IP-címek

tooset hello szükséges DNS-IP-címek, hello a következő lépéseket.

1.  Az Azure portál, a hello hello **DNS-kiszolgálók** panelen ellenőrizze, hogy a virtuális hálózat **DNS-kiszolgálók** beállítás értéke túl**egyéni DNS**.
2.  Válassza ki a hello alapján rendelkezik hálózati beállításait. További információkért tekintse meg a következő erőforrások hello:
    * [Vállalati hálózati kapcsolat (létesítmények közötti)][planning-guide-2.2]: hello IP-címek hello a helyi DNS-kiszolgálók hozzáadása.  
    Kibővítheti a helyszíni DNS kiszolgálók toohello virtuális gépek Azure-ban futó. A forgatókönyv, hozzáadhat hello IP-címei hello Azure hello DNS szolgáltatást futtató virtuális gépek.
    * [Csak felhőalapú telepítési][planning-guide-2.1]: hello tartozó további virtuális gép telepítése virtuális hálózati példányt, amely egy DNS-kiszolgálóként szolgál. Adja hozzá az IP-címei hello hello Azure virtuális gépekhez, amelyek toorun DNS-szolgáltatás beállítása.

    ![12. ábra: DNS-kiszolgálók konfigurálása az Azure-beli virtuális hálózathoz][sap-ha-guide-figure-3001]

    _**12. ábra:** DNS konfigurálása az Azure Virtual Network kiszolgálók_

  > [!NOTE]
  > Hello IP-címek hello DNS-kiszolgálók módosítja, ha szüksége van-e toorestart hello Azure virtuális gépek tooapply hello módosítása és terjesztése hello új DNS-kiszolgálók.
  >
  >

A fenti példában hello DNS-szolgáltatás telepítve és konfigurálva van a Windows virtuális gépek:

| Virtuálisgép-szerepkör | Virtuális gép állomásneve | Hálózati kártya neve | Statikus IP-cím |
| --- | --- | --- | --- |
| Első DNS-kiszolgáló |domcontr-0 |PR1-nic-domcontr-0 |10.0.0.10 |
| Második DNS-kiszolgáló |domcontr-1 |PR1-nic-domcontr-1 |10.0.0.11 |

### <a name="9fbd43c0-5850-4965-9726-2a921d85d73f"></a>Állomásnév és a statikus IP-címek hello SAP ASC/SCS fürtözött példány és az adatbázis-kezelő fürtözött példány

A helyszíni telepítéshez szüksége ezek fenntartott állomásneve és IP-címek:

| Virtuális állomás neve szerepkör | Virtuális állomás neve | Virtuális statikus IP-cím |
| --- | --- | --- |
| SAP ASC/SCS első fürt virtuális host name (kezelő) |PR1-ASC-vir |10.0.0.42 |
| SAP ASC/SCS példány virtuális állomás neve |PR1-ASC-sap |10.0.0.43 |
| SAP DBMS második fürt virtuális állomásnevet (kezelő) |PR1-dbms-vir |10.0.0.32 |

Hello fürt létrehozásakor létrehozása hello virtuális állomásnevek **pr1-ASC-vir** és **pr1-dbms-vir** és hello társított IP-címek, maga hello-fürt kezeléséhez. További információ toodo a, lásd: [fürtcsomópontok fürtkonfiguráció gyűjtése][sap-ha-guide-8.12.1].

Manuálisan is létrehozhat hello más két virtuális állomásnevek **pr1-ASC-sap** és **pr1-adatbázis-kezelő – sap**, és hello társított IP-címmel, hello DNS-kiszolgálón. hello fürtözött SAP ASC/SCS-példány és hello fürtözött adatbázis-kezelő példány használja ezeket az erőforrásokat. További információ toodo a, lásd: [hozzon létre egy virtuális nevet egy fürtözött SAP ASC/SCS példány][sap-ha-guide-9.1.1].

### <a name="84c019fe-8c58-4dac-9e54-173efd4b2c30"></a>Állítsa be a statikus IP-címek hello SAP virtuális gépekhez
Után hello virtuális gépek toouse a fürt központi telepítése, az összes virtuális gép szükséges tooset statikus IP-címeket. Ehhez hello Azure virtuális hálózat konfigurálása, és nem hello vendég operációs rendszer.

1.  Hello Azure-portálon, válassza ki **erőforráscsoport** > **hálózati kártya** > **beállítások** > **IP-cím** .
2.  A hello **IP-címek** panel alatt **hozzárendelés**, jelölje be **statikus**. A hello **IP-cím** mezőben adja meg, hogy szeretné-e toouse hello IP-címet.

  > [!NOTE]
  > Hello IP-cím hello hálózati kártya módosítja, ha szüksége van-e toorestart hello Azure virtuális gépek tooapply hello módosítása.  
  >
  >

  ![13. ábra: Állítsa be a statikus IP-címeket az egyes virtuális gépek hello hálózati kártya][sap-ha-guide-figure-3002]

  _**13. ábra:** hello hálózati kártya minden virtuális gép statikus IP-címek beállítása_

  Ismételje meg ezt a lépést minden hálózati interfészen esetén ez azt jelenti, hogy az összes virtuális gép, beleértve a virtuális gépek toouse szeretné az Active Directory és a DNS szolgáltatás.

A jelen példában vezetünk be a virtuális gépek és a statikus IP-címek:

| Virtuálisgép-szerepkör | Virtuális gép állomásneve | Hálózati kártya neve | Statikus IP-cím |
| --- | --- | --- | --- |
| Első SAP Application Server-példány |PR1-di-0 |PR1-nic-di-0 |10.0.0.50 |
| Második SAP Application Server-példány |PR1-di-1 |PR1-nic-di-1 |10.0.0.51 |
| ... |... |... |... |
| Utolsó SAP Application Server-példány |PR1-di-5 |PR1-nic-di-5 |10.0.0.55 |
| Első fürtcsomópontra ASC/SCS-példány |PR1-ASC-0 |PR1-nic-ASC-0 |10.0.0.40 |
| Második fürtcsomópont ASC/SCS-példány |PR1-ASC-1 |PR1-nic-ASC-1 |10.0.0.41 |
| Adatbázis-kezelő példány első fürtcsomópontra |PR1-db-0 |PR1-nic-db-0 |10.0.0.30 |
| Adatbázis-kezelő példány második fürtcsomópont |PR1-db-1 |PR1-nic-db-1 |10.0.0.31 |

### <a name="7a8f3e9b-0624-4051-9e41-b73fff816a9e"></a>Egy statikus IP-cím beállítása hello Azure belső terheléselosztóhoz

hello SAP Azure Resource Manager sablonnal hoz létre az Azure belső terheléselosztó hello SAP ASC/SCS példány és hello DBMS fürtön használt.

> [!IMPORTANT]
> hello hello virtuális állomásnevét hello SAP ASC/SCS az IP-címe hello azonos hello IP-címet az SAP ASC/SCS belső terheléselosztó hello: **pr1-lb-ASC**.
> hello hello virtuális nevét az adatbázis-kezelő rendszer hello IP-címe hello ugyanazt az adatbázis-kezelő belső terheléselosztó hello hello IP-címként: **pr1-lb-dbms**.
>
>

tooset egy statikus IP-címet hello Azure belső terheléselosztó:

1.  hello kezdeti telepítési beállítja hello belső terheléselosztó IP-cím túl**dinamikus**. Az Azure portál, a hello hello **IP-címek** panel alatt **hozzárendelés**, jelölje be **statikus**.
2.  Belső terheléselosztó hello hello IP-cím beállítása **pr1-lb-ASC** hello virtuális állomásnevet hello SAP ASC/SCS példány toohello IP-címét.
3.  Belső terheléselosztó hello hello IP-cím beállítása **pr1-lb-dbms** hello virtuális állomásnevet hello DBMS példány toohello IP-címét.

  ![14. ábra: Hello belső terheléselosztóhoz hello SAP ASC/SCS-példány beállítása a statikus IP-címek][sap-ha-guide-figure-3003]

  _**14. ábra:** statikus IP-címek hello belső terheléselosztóhoz hello SAP ASC/SCS-példány beállítása_

Ebben a példában két Azure belső terheléselosztók a statikus IP-címmel rendelkező vezetünk be:

| Az Azure belső terheléselosztási szerepköréhez | Az Azure belső terheléselosztó neve | Statikus IP-cím |
| --- | --- | --- |
| SAP ASC/SCS példány belső terheléselosztót |PR1-lb-ASC |10.0.0.43 |
| SAP DBMS belső terheléselosztó |PR1-lb-adatbázis-kezelő |10.0.0.33 |


### <a name="f19bd997-154d-4583-a46e-7f5a69d0153c"></a>Alapértelmezett ASC/SCS terheléselosztási szabályok hello Azure belső terheléselosztóhoz

hello SAP Azure Resource Manager-sablon létrehozza a hello portok:
* ABAP ASC példány, hello alapértelmezett példányszámának **00**
* A Java SCS példány, hello alapértelmezett példányszámának **01**

A SAP ASC/SCS példányát telepítésekor használnia kell a hello alapértelmezett példányszámának **00** a ABAP ASC példány és hello alapértelmezett példány számára vonatkozó **01** a Java SCS-példányhoz.

Ezután hozzon létre a szükséges belső terheléselosztási végpontok hello SAP NetWeaver portok.

toocreate szükséges belső terheléselosztási végpontok, először hozzon létre a terheléselosztási végpontok hello SAP NetWeaver ABAP ASC portok:

| Szolgáltatás/terheléselosztási szabály neve | Alapértelmezett portszámok | Konkrét portok (példányszámának 00 példány ASC) (SSZON 10) |
| --- | --- | --- |
| Sorba helyezni Server / *lbrule3200* |32 <*InstanceNumber*> |3200 |
| ABAP üzenet Server / *lbrule3600* |36 <*InstanceNumber*> |3600 |
| Belső ABAP üzenet / *lbrule3900* |39 <*InstanceNumber*> |3900 |
| A kiszolgáló HTTP-üzenet / *Lbrule8100* |81-es <*InstanceNumber*> |8100 |
| SAP Start ASC a HTTP / *Lbrule50013* |5 <*InstanceNumber*> 13 |50013 |
| SAP Start szolgáltatás ASC HTTPS / *Lbrule50014* |5 <*InstanceNumber*> 14 |50014 |
| Sorba helyezni replikációs / *Lbrule50016* |5 <*InstanceNumber*> 16 |50016 |
| SAP Start SSZON HTTP-kérelmekre *Lbrule51013* |5 <*InstanceNumber*> 13 |51013 |
| SAP Start SSZON HTTP-kérelmekre *Lbrule51014* |5 <*InstanceNumber*> 14 |51014 |
| Erőforrás-kezelő Win *Lbrule5985* | |5985 |
| Fájlmegosztás *Lbrule445* | |445 |

_**1. táblázat:** portszámokat hello SAP NetWeaver ABAP ASC példányok_

Ezután hozzon létre a terheléselosztási végpontok hello SAP NetWeaver Java SCS portok:

| Szolgáltatás/terheléselosztási szabály neve | Alapértelmezett portszámok | Konkrét portok (SCS példány példányszámának 01) (SSZON 11) |
| --- | --- | --- |
| Sorba helyezni Server / *lbrule3201* |32 <*InstanceNumber*> |3201 |
| Átjárókiszolgáló / *lbrule3301* |33 <*InstanceNumber*> |3301 |
| Java-üzenet Server / *lbrule3900* |39 <*InstanceNumber*> |3901 |
| A kiszolgáló HTTP-üzenet / *Lbrule8101* |81-es <*InstanceNumber*> |8101 |
| SAP Start SCS a HTTP / *Lbrule50113* |5 <*InstanceNumber*> 13 |50113 |
| SAP Start szolgáltatás SCS HTTPS / *Lbrule50114* |5 <*InstanceNumber*> 14 |50114 |
| Sorba helyezni replikációs / *Lbrule50116* |5 <*InstanceNumber*> 16 |50116 |
| SAP Start SSZON HTTP-kérelmekre *Lbrule51113* |5 <*InstanceNumber*> 13 |51113 |
| SAP Start SSZON HTTP-kérelmekre *Lbrule51114* |5 <*InstanceNumber*> 14 |51114 |
| Erőforrás-kezelő Win *Lbrule5985* | |5985 |
| Fájlmegosztás *Lbrule445* | |445 |

_**2. táblázat:** portszámokat hello SAP NetWeaver Java SCS-példányok_

![15. ábra: Alapértelmezett ASC/SCS terheléselosztási szabályok hello Azure belső terheléselosztóhoz][sap-ha-guide-figure-3004]

_**15. ábra:** alapértelmezett ASC/SCS terheléselosztási szabályok hello Azure belső terheléselosztóhoz_

Hello terheléselosztó hello IP-cím beállítása **pr1-lb-dbms** hello virtuális állomásnevet hello DBMS példány toohello IP-címét.

### <a name="fe0bd8b5-2b43-45e3-8295-80bee5415716"></a>Hello ASC/SCS alapértelmezett terheléselosztási hello Azure belső terheléselosztóhoz tartozó szabályok módosítása

Ha toouse eltérő számú SAP-ASC hello vagy SCS példányok, meg kell változtatnia hello neveit és értékeit a portok alapértelmezett értékekhez.

1.  Hello Azure-portálon, válassza ki  **<* SID*> - lb - ASC betöltése terheléselosztó ** > **terheléselosztási szabályok betöltése**.
2.  Minden terheléselosztási szabályok, amelyek toohello SAP ASC vagy SCS példány tartozik ezek az értékek módosítása:

  * Név
  * Port
  * Háttér-port

  Például ha azt szeretné, hogy toochange hello alapértelmezett ASC példány szám 00 too31, kell toomake hello módosításokat minden porthoz az 1.

  Példa port frissítési *lbrule3200*.

  ![16. ábra: Hello ASC/SCS alapértelmezett terheléselosztási hello Azure belső terheléselosztóhoz tartozó szabályok módosítása][sap-ha-guide-figure-3005]

  _**16. ábra:** módosítás hello ASC/SCS alapértelmezett terheléselosztási szabályok hello Azure belső terheléselosztóhoz_

### <a name="e69e9a34-4601-47a3-a41c-d2e11c626c0c"></a>Windows virtuális gépek toohello tartomány hozzáadása

Miután hozzárendelt egy statikus IP cím toohello virtuális gépek, hello virtuális gépek toohello tartomány hozzáadása.

![17. ábra: A virtuális gép tooa tartomány hozzáadása][sap-ha-guide-figure-3006]

_**17. ábra:** tartomány hozzáadása a virtuális gép tooa_

### <a name="661035b2-4d0f-4d31-86f8-dc0a50d78158"></a>Adja hozzá a beállításjegyzék-bejegyzések hello SAP ASC/SCS példány mindkét fürtcsomóponton

Az Azure terheléselosztó, hogy a kapcsolatok bezárása, amikor hello kapcsolat üresjáratban a megadott ideig (üresjárati időkorlátot) idő belső terheléselosztót tartalmaz. SAP munkafolyamatok párbeszédpanel példányok nyitott kapcsolatok toohello SAP sorba helyezni a, amint hello első sorba helyezni/created igények toobe küldött kérelmek feldolgozásához. Ezek a kapcsolatok általában marad a meghatározott hello munkahelyi folyamat vagy hello sorba helyezni folyamat újraindítja. Ha a beállított időn hello kapcsolat üresjáratban, hello Azure belső terheléselosztási terheléselosztó bezárása hello kapcsolatok. Ez nem hiba, mert hello SAP munkahelyi folyamat helyreállítja a csatolást hello kapcsolódási toohello sorba helyezni folyamatot, ha már nem létezik. Ezek a tevékenységek hello fejlesztői nyomkövetések SAP folyamatok vannak dokumentálva, de ezeket a nyomkövetéseket a felesleges tartalmat nagy mennyiségű hoznak létre. Egy jó ötlet toochange hello TCP/IP `KeepAliveTime` és `KeepAliveInterval` mindkét fürtcsomóponton. A módosításokat a hello TCP/IP-paraméterek SAP profil paraméterekkel, hello cikk későbbi részében leírt össze.

beállításjegyzék-bejegyzések tooadd mindkét fürtcsomóponton hello SAP ASC/SCS példány, először adja hozzá a Windows beállításjegyzék-bejegyzések mindkét Windows fürtcsomópontokon az SAP ASC/SCS:

| Elérési út | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Változó neve |`KeepAliveTime` |
| Változó típusa |REG_DWORD (decimális) |
| Érték |120000 |
| Hivatkozás toodocumentation |[https://technet.microsoft.com/en-us/library/cc957549.aspx](https://technet.microsoft.com/en-us/library/cc957549.aspx) |

_**3. táblázat:** módosítás hello első TCP/IP-paraméter_

Ezt követően adja hozzá a Windows beállításjegyzék-bejegyzések mindkét Windows fürtcsomópontokon az SAP ASC/SCS:

| Elérési út | HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters |
| --- | --- |
| Változó neve |`KeepAliveInterval` |
| Változó típusa |REG_DWORD (decimális) |
| Érték |120000 |
| Hivatkozás toodocumentation |[https://technet.microsoft.com/en-us/library/cc957548.aspx](https://technet.microsoft.com/en-us/library/cc957548.aspx) |

_**4. táblázat:** módosítás hello második TCP/IP-paraméter_

**tooapply hello módosításokat, indítsa újra mindkét fürtcsomópontot**.

### <a name="0d67f090-7928-43e0-8772-5ccbf8f59aab"></a>Egy SAP ASC/SCS példánya számára a Windows Server feladatátvételi fürtszolgáltatási fürt beállítása

A Windows Server feladatátvételi fürtszolgáltatási fürt egy SAP ASC/SCS-példány beállítása magában foglalja a ezeket a feladatokat:

- Fürtkonfiguráció hello fürtcsomópontok gyűjtése
- A fürt tanúsító fájlmegosztás beállítása

#### <a name="5eecb071-c703-4ccc-ba6d-fe9c6ded9d79"></a>Fürtkonfiguráció hello fürtcsomópontok gyűjtése

1.  Hello szerepkör hozzáadása és szolgáltatások varázsló vegye fel a Feladatátvételi fürtszolgáltatás tooboth fürtcsomóponton.
2.  Hello feladatátvevő fürt beállítása a Feladatátvevőfürt-kezelő használatával. A Feladatátvevőfürt-kezelőben válasszon **fürt létrehozása**, majd adja hozzá a hello első fürt, csomópont A. csak hello neve Nem hello második csomópont hozzáadása még; hello második csomópont egy későbbi lépésben fogja hozzáadni.

  ![18. ábra: Hello kiszolgáló vagy a virtuális gép neve hello első fürtcsomópont hozzáadása][sap-ha-guide-figure-3007]

  _**18. ábra:** Hozzáadás hello kiszolgáló vagy a virtuális gép hello első fürtcsomópont nevét_

3.  Adja meg a hello hello fürt hálózati neve (virtuális állomás neve).

  ![19. ábra: Hello fürt nevét adja meg.][sap-ha-guide-figure-3008]

  _**19. ábra:** hello fürt nevének megadása_

4.  Hello fürt létrehozása után futtassa a fürtellenőrzési tesztet.

  ![20. ábra: Hello fürt érvényesítési-ellenőrzés futtatása][sap-ha-guide-figure-3009]

  _**20. ábra:** hello fürt eredetiség ellenőrzésének futtatása_

  Lemezek ezen a ponton hello folyamat kapcsolatos figyelmeztetéseket figyelmen kívül hagyhatja. A tanúsító fájlmegosztás és hello SIOS megosztott lemezek később fogja hozzáadni. Ebben a szakaszban egy kvórum kapcsolatos tooworry nem szükséges.

  ![21. ábra: Kvórumlemez nem található][sap-ha-guide-figure-3010]

  _**21. ábra:** kvórumlemez nem található_

  ![22. ábra: Core fürterőforrás kell egy új IP-cím][sap-ha-guide-figure-3011]

  _**22. ábra:** Core fürterőforrás kell egy új IP-cím_

5.  Hello core fürtszolgáltatás hello IP-címének módosítása. hello fürt nem indítható el, amíg meg nem változtatja hello IP-cím hello core fürtszolgáltatás, mert hello kiszolgáló IP-címének hello tooone hello virtuális gép csomópontok. Ehhez a hello **tulajdonságok** hello core fürt szolgáltatás IP-erőforrás oldalán.

  Például tooassign IP-címet kell (a példánkban **10.0.0.42**) hello fürt virtuális állomás neve **pr1-ASC-vir**.

  ![23. ábra: A hello tulajdonságai párbeszédpanelen hello IP-címének módosítása][sap-ha-guide-figure-3012]

  _**23. ábra:** a hello **tulajdonságok** párbeszédpanelen, a módosítás hello IP-cím_

  ![24. ábra: Hello fürt fenntartott hello IP-cím hozzárendelése][sap-ha-guide-figure-3013]

  _**24. ábra:** hello fürt fenntartott hello IP-cím hozzárendelése_

6.  Hello fürt virtuális állomás neve online állapotba.

  ![25. ábra: Core a fürtszolgáltatás működik-e és fut, valamint hello javítsa ki az IP-cím][sap-ha-guide-figure-3014]

  _**25. ábra:** core a fürtszolgáltatás működik-e és fut, és a hello javítsa ki az IP-cím_

7.  Adja hozzá az hello második fürtcsomópontokat.

  Most, hogy hello core fürtszolgáltatás működik és elérhető, a második fürtcsomópont hello is hozzáadhat.

  ![26. ábra: Hello második fürt csomópont hozzáadása][sap-ha-guide-figure-3015]

  _**26. ábra:** Hozzáadás hello második fürtcsomópont_

8.  Adjon meg egy nevet hello második csomópont gazda.

  ![27. ábra: Adja meg a hello második fürtcsomópont gazdagép neve][sap-ha-guide-figure-3016]

  _**27. ábra:** hello második fürt csomópont gazdagép nevének megadása_

  > [!IMPORTANT]
  > Győződjön meg arról, hogy hello **adja hozzá az összes megfelelő tárolót toohello fürtöt** jelölőnégyzet **nem** kijelölt.  
  >
  >

  ![28. ábra: Jelölje be az hello jelölőnégyzetet][sap-ha-guide-figure-3017]

  _**28. ábra:** tegye **nem** válasszon hello jelölőnégyzetet_

  Kvórum és lemezek kapcsolatos figyelmeztetések figyelmen kívül hagyhatja. Hello kvórum és megosztási hello lemez később lesznek állítva, a [telepítése SIOS DataKeeper Cluster Edition SAP ASC/SCS megosztás olyan fürtlemez esetében][sap-ha-guide-8.12.3].

  ![29. ábra: Hello lemez kvórumával kapcsolatos figyelmeztetések mellőzése][sap-ha-guide-figure-3018]

  _**29. ábra:** hello lemez kvórumával kapcsolatos figyelmeztetések mellőzése_


#### <a name="e49a4529-50c9-4dcf-bde7-15a0c21d21ca"></a>A fürt tanúsító fájlmegosztás beállítása

Ezeket a feladatokat a fürt tanúsító fájlmegosztás beállítása foglal magában:

- Fájlmegosztás létrehozása
- A beállítás hello fájl megosztási tanúsító kvórum a Feladatátvevőfürt-kezelőben

##### <a name="06260b30-d697-4c4d-b1c9-d22c0bd64855"></a>Fájlmegosztás létrehozása

1.  Válassza ki a tanúsító fájlmegosztást egy kvórumlemez helyett. SIOS DataKeeper támogatja ezt a lehetőséget.

  Az ebben a cikkben szereplő példák hello hello tanúsító fájlmegosztás van az Azure-ban futó hello Active Directory és a DNS-kiszolgálón. hello tanúsító fájlmegosztás neve **domcontr-0**. Volna már konfigurált egy VPN-kapcsolat tooAzure (keresztül telephelyek közötti VPN vagy Azure expressroute-ot), mert az Active Directory és a DNS szolgáltatás a helyi, és nem megfelelő toorun fájl megosztása tanúsító.

  > [!NOTE]
  > Ha az Active Directory és a DNS szolgáltatás csak a helyszíni fut, ne állítson be a tanúsító fájlmegosztás hello Active Directory és a DNS a Windows operációs rendszeren, amely a helyszíni fut-e. Előfordulhat, hogy fut az Azure Active Directory és a DNS a helyszíni és a fürtcsomópontok közötti hálózati késés túl nagy, és a csatlakozási problémák miatt. Lehet, hogy tooconfigure hello tanúsító fájlmegosztás Bezárás toohello fürtcsomóponton futó Azure virtuális géphez.  
  >
  >

  hello kvórum meghajtó legalább 1024 MB szabad területre van szüksége. Azt javasoljuk, hogy a 2048 MB szabad lemezterületet a hello kvórum meghajtóra.

2.  Adja hozzá a hello fürtnévobjektum.

  ![30. ábra: Hello fürtnévobjektum hello megosztáson hello engedélyek hozzárendelése][sap-ha-guide-figure-3019]

  _**30. ábra:** hello fürtnévobjektum hello megosztáson hello engedélyek hozzárendelése_

  Ne feledje hello engedélyek hello fürtnévobjektum hello megosztást hello hatóság toochange adatok közé tartozik (a példánkban **pr1-ASC-vir$**).

3.  tooadd hello fürt neve objektum toohello listáról válassza ki **Hozzáadás**. Módosítsa a hello szűrő toocheck számítógép-objektumok hozzáadása toothose 31. ábrán látható.

  ![31. ábra: Változás hello objektumtípusok tooinclude számítógépek][sap-ha-guide-figure-3020]

  _**31. ábra:** hello objektumtípusok tooinclude számítógépek módosítása_

  ![32. ábra: Jelölje be hello számítógépek][sap-ha-guide-figure-3021]

  _**32. ábra:** válassza hello **számítógépek** jelölőnégyzetet_

4.  Adja meg a hello fürtnévobjektum 31. ábrán látható módon. Hello rekord már létrejött, mert hello engedélyek, módosíthatja, ahogy az ábra 30.

5.  Jelölje be hello **biztonsági** hello megosztást, és majd lapján részletesebb hello fürtnévobjektum engedélyeit.

  ![33. ábra: Hello biztonsági attribútumainak hello fürtnévobjektum a hello fájl megosztási Kvórum beállítása.][sap-ha-guide-figure-3022]

  _**33. ábra:** hello biztonsági attribútumainak hello fürtnévobjektum a hello fájl megosztási Kvórum beállítása_

##### <a name="4c08c387-78a0-46b1-9d27-b497b08cac3d"></a>Hello fájl megosztási tanúsító Kvórum beállítása a Feladatátvevőfürt-kezelőben

1.  Nyissa meg a hello konfigurálása fürtkvórum beállítása varázslót.

  ![34. ábra: Hello konfigurálása fürtkvórum beállítása varázsló indítása][sap-ha-guide-figure-3023]

  _**34. ábra:** Start hello konfigurálása fürtkvórum beállítása varázsló_

2.  A hello **kvórumkonfiguráció kiválasztása** lapon, hogy melyik **hello kvórum tanúsítójának kijelölése**.

  ![35. ábra: Választhat kvórumkonfigurációinak][sap-ha-guide-figure-3024]

  _**35. ábra:** választhat kvórumkonfigurációinak_

3.  A hello **kvórum Tanúsítójának kijelölése** lapon, hogy melyik **konfigurálása egy tanúsító fájlmegosztást**.

  ![36. ábra: Select hello tanúsító fájlmegosztás][sap-ha-guide-figure-3025]

  _**36. ábra:** hello tanúsító fájlmegosztás kiválasztása_

4.  Adja meg a hello UNC elérési út toohello fájlmegosztást (a példánkban \\domcontr-0\FSW). hello módosításokat végezhet, válassza ki a listáját toosee **következő**.

  ![37. ábra: Hello fájlmegosztási helyet hello tanúsító fájlmegosztás meghatározása][sap-ha-guide-figure-3026]

  _**37. ábra:** hello fájlmegosztási helyet hello tanúsító fájlmegosztás meghatározása_

5.  Válassza ki a hello módosításokat, és válassza **következő**. Toosuccessfully kell konfigurálnia a hello fürtkonfiguráció ahogy az ábra 38.  

  ![38. ábra: Megerősítése, hogy Ön már újrakonfigurálása hello fürt][sap-ha-guide-figure-3027]

  _**38. ábra:** , hogy Ön már újrakonfigurálása hello fürt megerősítése_

Windows feladatátvevő fürt hello sikeres telepítését követően a módosításokat kell toobe toosome küszöbértékek tooadapt feladatátvételi észlelési tooconditions tett az Azure-ban. hello megváltozott paraméterek toobe szerepelnek a blog: https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/. Feltételezve, hogy a két virtuális gépekre, amelyek build hello fürtkonfiguráció Windows ASC/SCS a szerepelnek hello ugyanazon az alhálózaton, hello következő paramétereket kell módosítani toobe toothese értékeket:
- SameSubNetDelay = 2
- SameSubNetThreshold = 15

Ezeket a beállításokat felhasználók tesztelése és egy jó biztonsági sérülés toobe elég rugalmas megadott hello egy oldalán. A hello ugyanakkor ezek a beállítások volt biztosítása gyors elég feladatátvételi valós hibaüzenet feltételekben az SAP-szoftver vagy a csomópont vagy Virtuálisgép-hiba. 

### <a name="5c8e5482-841e-45e1-a89d-a05c0907c868"></a>Hello SAP ASC/SCS fürtlemez megosztás SIOS DataKeeper Cluster Edition telepítése

Most már rendelkezik egy működő Windows Server feladatátvételi fürtszolgáltatás konfigurációs az Azure-ban. De tooinstall SAP ASC/SCS példánya egy megosztott lemez erőforrás van szüksége. Nem hozható létre megosztott hello lemezerőforrásokat van szüksége az Azure-ban. SIOS DataKeeper Cluster Edition egy olyan külső megoldás, megosztott toocreate lemezerőforrásokat is használhatja.

Az SAP ASC/SCS hello SIOS DataKeeper Cluster Edition telepítését a megosztás fürtlemez ezeket a feladatokat foglalja magában:

- .NET-keretrendszer 3.5 hello hozzáadása
- SIOS DataKeeper telepítése
- SIOS DataKeeper beállítása

#### <a name="1c2788c3-3648-4e82-9e0d-e058e475e2a3"></a>.NET-keretrendszer 3.5 hello hozzáadása
hello Microsoft .NET-keretrendszer 3.5-ös verzióját nem automatikusan aktiválva, vagy Windows Server 2012 R2 rendszeren telepítve. SIOS DataKeeper használatához hello .NET-keretrendszer toobe telepíthető DataKeeper minden csomóponton, telepítenie kell hello .NET-keretrendszer 3.5 hello vendég operációs rendszeren az hello fürt összes virtuális gépet.

Két módon tooadd hello .NET-keretrendszer 3.5 van:

- Hello-szerepkörök hozzáadása és szolgáltatások varázsló használata a Windows ahogy az ábra 39.

  ![39. ábra: Hello .NET-keretrendszer 3.5 telepítése hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével][sap-ha-guide-figure-3028]

  _**39. ábra:** telepítés hello .NET-keretrendszer 3.5 hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével_

  ![40. ábra: Telepítés folyamatjelző hello .NET-keretrendszer 3.5 telepítése hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével][sap-ha-guide-figure-3029]

  _**40. ábra:** sáv hello hozzáadása szerepkörök és szolgáltatások varázsló segítségével hello .NET-keretrendszer 3.5 telepítésekor a telepítési folyamat_

- Hello parancssori eszköz a dism.exe használata szükséges. Az ilyen típusú telepítés tooaccess hello SxS könyvtár a telepítési adathordozó Windows hello kell. Egy rendszergazda jogú parancssort írja be:

  ```
  Dism /online /enable-feature /featurename:NetFx3 /All /Source:installation_media_drive:\sources\sxs /LimitAccess
  ```

#### <a name="dd41d5a2-8083-415b-9878-839652812102"></a>SIOS DataKeeper telepítése

Telepítse a SIOS DataKeeper Cluster Edition hello fürt mindegyik csomópontján. SIOS DataKeeper, a megosztott tároló virtuális toocreate hozzon létre egy szinkronizált tükrözött, és majd szimulálása a fürt megosztott tároló.

Hello SIOS szoftver telepítése előtt hozzon létre hello tartományi felhasználó **DataKeeperSvc**.

> [!NOTE]
> Adja hozzá a hello **DataKeeperSvc** felhasználói toohello **helyi rendszergazdai** csoport mindkét fürtcsomóponton.
>
>

tooinstall SIOS DataKeeper:

1.  Szoftvertelepítés hello SIOS mindkét fürtcsomóponton.

  ![SIOS telepítő][sap-ha-guide-figure-3030]

  ![. Ábra 41: Hello SIOS DataKeeper telepítési első oldalán][sap-ha-guide-figure-3031]

  _**41. ábra:** hello SIOS DataKeeper telepítési első oldalára_

2.  A megjelenő ábra 42 hello párbeszédpanelen válassza ki a **Igen**.

  ![42. ábra: DataKeeper arról tájékoztatja, hogy a szolgáltatás le lesz tiltva][sap-ha-guide-figure-3032]

  _**42. ábra:** DataKeeper arról tájékoztatja, hogy a szolgáltatás le lesz tiltva_

3.  Hello párbeszédpanelen megjelenő ábra 43, azt javasoljuk, hogy kiválassza **tartomány vagy a kiszolgáló fiók**.

  ![43. ábra: A SIOS DataKeeper felhasználó kiválasztása][sap-ha-guide-figure-3033]

  _**43. ábra:** SIOS DataKeeper a felhasználó kiválasztása_

4.  Adja meg a hello tartományi fiók felhasználói nevét és SIOS DataKeeper létrehozott jelszavakat.

  ![44. ábra: Hello tartományi felhasználónév és jelszó megadása hello SIOS DataKeeper telepítése][sap-ha-guide-figure-3034]

  _**44. ábra:** hello SIOS DataKeeper telepítési hello tartományi felhasználónevet és jelszót adjon meg_

5.  Telepítse a SIOS DataKeeper példány, ahogy az ábra 45 hello Licenckulcs.

  ![45. ábra: Adja meg a SIOS DataKeeper licenckulcs][sap-ha-guide-figure-3035]

  _**45. ábra:** adja meg a SIOS DataKeeper licenckulcs_

6.  Amikor a rendszer kéri, indítsa újra a hello virtuális gépet.

#### <a name="d9c1fc8e-8710-4dff-bec2-1f535db7b006"></a>SIOS DataKeeper beállítása

Telepítése után SIOS DataKeeper mindkét csomóponton, toostart hello konfigurációs szüksége. hello hello konfigurációs célja toohave szinkron replikálása hello további csatlakoztatott virtuális merevlemezek tooeach hello virtuális gépek között.

1.  Indítsa el a hello DataKeeper felügyeleti és a konfigurációs eszközt, és válassza ki **Connect kiszolgáló**. (Az ábrán 46, ez a beállítás van körben piros.)

  ![46. ábra: SIOS DataKeeper kezelési és konfigurációs eszköz][sap-ha-guide-figure-3036]

  _**46. ábra:** SIOS DataKeeper felügyelete és konfigurálása eszköz_

2.  Adja meg hello nevét, vagy a TCP/IP-cím hello első csomópont hello felügyelete és konfigurálása eszköz kell csatlakoztatja, és, a második lépésben hello második csomópontra.

  ![47. ábra: Hello neve vagy a TCP/IP-cím hello első csomópont hello felügyelete és konfigurálása eszköz kell csatlakoztatja, és egy második lépésben hello második csomópont][sap-ha-guide-figure-3037]

  _**47. ábra:** hello neve vagy a TCP/IP-cím hello első csomópont hello felügyelete és konfigurálása eszköz kell csatlakoztatja, és egy második lépésben hello második csomópont_

3.  Hozzon létre hello fájlreplikálási feladat hello két csomópont között.

  ![48. ábra: A replikáció feladat létrehozása][sap-ha-guide-figure-3038]

  _**48. ábra:** replikációs feladat létrehozása_

  A varázsló végigvezeti egy fájlreplikálási feladat létrehozásának folyamatán hello.
4.  Adja meg a hello nevét, az TCP/IP-cím és a hello forráscsomópont lemezköteten.

  ![49. ábra: Hello replikációs feladat hello nevének meghatározása][sap-ha-guide-figure-3039]

  _**49. ábra:** Define hello hello replikációs feladat neve_

  ![50. ábra: Hello alap adatok hello csomópont, hogy hello aktuális forráscsomópont alkalmazandó meghatározása][sap-ha-guide-figure-3040]

  _**50. ábra:** hello csomópont, hogy hello aktuális forráscsomópont alkalmazandó hello adatainak megadása_

5.  Hello nevét, a TCP/IP-cím és a hello célcsomóponttal lemezkötetének megadása.

  ![51. ábra: Hello alap adatok hello csomópont, hogy hello aktuális célcsomóponttal alkalmazandó meghatározása][sap-ha-guide-figure-3041]

  _**51. ábra:** hello csomópont, hogy hello aktuális célcsomóponttal alkalmazandó hello adatainak megadása_

6.  Hello tömörítési algoritmust határozza meg. Ebben a példában azt javasoljuk, hogy a tömörítés hello replikálási adatfolyamból. Különösen abban az esetben az újraszinkronizálás hello replikálási adatfolyamból hello tömörítésének jelentősen csökkenti az újraszinkronizálás idő. Vegye figyelembe, hogy a tömörítési hello Processzor és memória szempontjából erőforrásokat egy virtuális gép használ. A hello tömörítési arány növekedése, hello használt CPU-erőforrások mennyiségét. Is módosíthatja ezt a beállítást később.

7.  Egy másik beállítás kell toocheck e hello replikáció aszinkron vagy szinkron módon történik-e. *Ha a SAP ASC/SCS konfigurációk, használnia kell a szinkron replikáció*.  

  ![52. ábra: A replikáció adatainak megadása][sap-ha-guide-figure-3042]

  _**52. ábra:** replikációs adatainak megadása_

8.  Határozza meg, hogy legyen-e az hello kötet replikált hello replikációs feladat által képviselt tooa Windows Server feladatátvételi fürtszolgáltatási fürtkonfiguráció egy megosztott lemezt. Hello SAP ASC/SCS konfigurációs, válassza a **Igen** , hogy a fürt látja hello Windows hello replikált a kötet olyan megosztott lemezzel, amelyet a fürt kötetként használhat.

  ![53. ábra: Kattintson az Igen tooset hello replikált kötet egy fürtkötetként][sap-ha-guide-figure-3043]

  _**53. ábra:** válasszon **Igen** tooset hello egy fürt kötet replikálása_

  Hello kötet létrehozása után hello DataKeeper felügyelete és konfigurálása eszköz látható, hogy hello fájlreplikálási feladat aktív.

  ![Ábra 54: DataKeeper szinkron tükrözés hello SAP ASC/SCS megosztás lemez a jelenleg aktív][sap-ha-guide-figure-3044]

  _**Ábra 54:** DataKeeper szinkron tükrözés hello SAP ASC/SCS osztozhat lemezen a jelenleg aktív_

  Ahogy az ábra 55 a Feladatátvevőfürt-kezelő most hello lemez DataKeeper lemezként jeleníti meg.

  ![55. ábra: A Feladatátvevőfürt-kezelő hello lemez DataKeeper replikált jeleníti meg][sap-ha-guide-figure-3045]

  _**Ábra 55:** Feladatátvevőfürt-kezelő látható hello lemez adott replikált DataKeeper_

## <a name="a06f0b49-8a7a-42bf-8b0d-c12026c5746b"></a>Hello SAP NetWeaver rendszer telepítése

Mert beállítások változhatnak attól függően hello használt adatbázis-kezelő rendszer azt nem leírják hello DBMS beállítása. Azonban feltételezzük, hogy magas rendelkezésre állású kérdéseket hello DBMS hello különböző DBMS szállítóktól támogatja az Azure-hello funkciókkal rendelkező tárgyalja. Például mindig bekapcsolva vagy adatbázis-tükrözést az SQL Server és Oracle Data Guard az Oracle-adatbázisok. Ebben a cikkben használjuk hello esetben azt további védelmi toohello adatbázis-kezelő nem vett fel.

Nincsenek semmilyen külön odafigyelést különböző adatbázis-kezelő szolgáltatásokat fürtözött SAP ASC/SCS konfigurálása az Azure-ban az ilyen kommunikál.

> [!NOTE]
> hello telepítési eljárásokat az SAP NetWeaver ABAP rendszerek, Java, és a ABAP + Java rendszerek csaknem azonosak. hello legfontosabb különbség, hogy rendelkezik-e az SAP ABAP rendszer ASC egy példánya. SAP Java rendszer hello egy SCS példány van. hello SAP ABAP + Java rendszer egy ASC példányával rendelkezik, és fut egy SCS példány hello ugyanazt a Microsoft feladatátvevő fürt csoport. Összes telepítési különbséget minden SAP NetWeaver telepítési verem explicit módon szerepelnek. Feltételezzük, hogy minden egyéb részeinek vannak hello azonos.  
>
>

### <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Telepítse a SAP, ha a magas rendelkezésre állású ASC/SCS példánya

> [!IMPORTANT]
> Győződjön meg a lap DataKeeper fájlja nem a tooplace tükrözött kötetek. DataKeeper nem támogatja a tükrözött kötetek. A lapozófájl hello ideiglenes D meghajtó egy Azure virtuális gép hello alapértelmezett hagyhatja. Ha még nem szerepel ott, helyezze át a hello Windows lap fájl toodrive D az Azure virtuális gép.
>
>

Ezeket a feladatokat, ha a magas rendelkezésre állású ASC/SCS példánya SAP telepítése foglal magában:

- Virtuális állomásnevet fürtözött hello SAP ASC/SCS példány létrehozása
- Hello SAP első fürtcsomópontra telepítése
- Hello SAP profil hello ASC/SCS példány módosítása
- A mintavétel a port hozzáadása
- Hello Windows tűzfal mintavételi port megnyitása

#### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Hozzon létre egy virtuális nevet fürtözött hello SAP ASC/SCS példány

1.  Hello Windows DNS-kezelőben hozzon létre egy DNS-bejegyzést hello ASC/SCS példányának hello virtuális állomás neve.

  > [!IMPORTANT]
  > hello toohello virtuális állomásnevét ASC/SCS-példány neve lehet hello hozzárendelt IP-cím hello azonos tooAzure terheléselosztóhoz rendelt hello IP-címként (**<*SID*> - lb - ASC **).  
  >
  >

  IP-cím hello virtuális SAP ASC/SCS állomásnév hello (**pr1-ASC-sap**) van hello azonos Azure terheléselosztó hello IP-címként (**pr1-lb-ASC**).

  ![Ábra 56: Hello DNS-bejegyzés hello SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím megadása][sap-ha-guide-figure-3046]

  _**Ábra 56:** hello DNS-bejegyzés hello SAP ASC/SCS fürt virtuális nevét és a TCP/IP-cím megadása_

2.  toodefine hello IP-hozzárendelt toohello virtuális állomás nevét, jelölje be **DNS-kezelő** > **tartomány**.

  ![57. ábra: Új virtuális nevét és TCP/IP-cím SAP ASC/SCS fürtnek megfelelő konfiguráció][sap-ha-guide-figure-3047]

  _**57. ábra:** új virtuális nevét és a TCP/IP-címtartományok SAP ASC/SCS fürtnek megfelelő konfiguráció_

#### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Hello SAP első fürtcsomópontra telepítése

1.  Hello első fürt csomópont lehetőség fürtcsomóponton A. végrehajtása Például a hello **pr1-ASC-0** állomás.
2.  tookeep hello alapértelmezett portok hello Azure belső terheléselosztót, válasszon:

  * **ABAP rendszer**: **ASC** szám példány **00**
  * **Java-rendszer**: **SCS** szám példány **01**
  * **ABAP + Java rendszer**: **ASC** szám példány **00** és **SCS** szám példány **01**

  toouse példány számok eltérő 00 hello ABAP ASC a példány és hello Java SCS példány 01, először szüksége toochange hello Azure belső alapértelmezett terheléselosztási szabályok, leírt [módosítás hello ASC/SCS alapértelmezett betöltése terheléselosztási szabályok hello Azure belső terheléselosztóhoz][sap-ha-guide-8.9].

hello tovább néhány feladatot nem hello szabványos SAP telepítési dokumentációjában leírt.

> [!NOTE]
> hello SAP dokumentáció ismerteti, hogyan tooinstall hello ASC/SCS fürt első csomópontjára.
>
>

#### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Hello SAP profil hello ASC/SCS példány módosítása

Egy új profil paraméter tooadd van szüksége. hello-profil paraméter megakadályozza, hogy a SAP-munkafolyamatok és hello sorba helyezni kiszolgáló közötti kapcsolat bezárása, ha túl sokáig üresjáratban. A Microsoft hello probléma esetén említett [adja hozzá a beállításjegyzék-bejegyzések mindkét fürtcsomóponton hello SAP ASC/SCS példány][sap-ha-guide-8.11]. A szakaszt azt is bevezette a két módosítások toosome Alapszintű TCP/IP kapcsolat paramétereit. A második lépésben tooset hello sorba helyezni server toosend szükség van egy `keep_alive` jelezze, hogy hello kapcsolatok nem találati hello Azure belső elosztott terhelésű üresjárati küszöbértéket.

toomodify hello SAP profil hello ASC/SCS példány:

1.  A profil paraméter toohello SAP ASC/SCS példány profil hozzáadása:

  ```
  enque/encni/set_so_keepalive = true
  ```
  A jelen példában hello elérési út:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Például SAP SCS toohello példány profil és a megfelelő elérési út:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  tooapply hello módosításokat, indítsa újra a hello SAP ASC /SCS példányt.

#### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Adjon hozzá egy mintavételi portot

Toomake hello fürtözési konfigurációs működnek hello belső elosztott terhelésű mintavételi használata Azure terheléselosztó. hello Azure belső terheléselosztó általában hello bejövő munkaterhelés részt vevő virtuális gépek között egyenlően osztja el. Azonban ez nem fog működni az egyes fürtkonfigurációk mert csak egy példány aktív. hello más példány passzív, hello munkaterhelés nem tudja elfogadni. A mintavétel funkció segítségével hello Azure belső load balancer rendel csak tooan aktív példány végzett munka során. Hello mintavételi funkciójú hello belső terheléselosztó előfordulások aktív, és majd cél hello munkaterhelés csak hello példány képes észlelni.

a mintavételi portot tooadd:

1.  Ellenőrizze a hello aktuális **ProbePort** hello a következő PowerShell-parancs futtatásával beállítása. Végrehajtja a hello virtuális gépek egyik hello fürt konfigurációban.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

2.  A mintavételi portot határozza meg. hello alapértelmezett mintavételi portszám **0**. A jelen példában használjuk a mintavételi portot **62000**.

  ![58. ábra: hello fürt konfigurációs mintavételi portot pedig 0 alapértelmezés szerint][sap-ha-guide-figure-3048]

  _**58. ábra:** hello alapértelmezett fürt konfigurációs mintavételi portot: 0_

  hello portszámot az SAP Azure Resource Manager-sablonok van meghatározva. PowerShell hello portszámát rendelhet hozzá.

  hello új ProbePort értéket tooset  **SAP <*SID*> IP ** fürterőforrás, futtassa a következő PowerShell-parancsfájl hello. Hello PowerShell változók a környezet frissítése. Hello parancsfájl futtatása után lesz felszólító toorestart hello SAP fürt csoport tooactivate hello változásait.

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

  Miután hello kapcsolása  **SAP <*SID*> ** fürtön a szerepkör hálózatra, ellenőrizze, hogy **ProbePort** toohello új érték van beállítva.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```

  ![59. ábra: Mintavételi hello fürt port hello új érték beállítása után][sap-ha-guide-figure-3049]

  _**59. ábra:** hello új érték beállítása után, mintavételi modulja hello fürt port_

#### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Nyissa meg a hello Windows tűzfal mintavételi portot

A Windows tűzfal mintavételi portot mindkét fürtcsomóponton tooopen van szüksége. A következő parancsfájl tooopen a Windows tűzfal mintavételi portot hello használata. Hello PowerShell változók a környezet frissítése.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of hello Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

Hello **ProbePort** értéke túl**62000**. Most már hozzáférhet a fájlmegosztáshoz hello  **\\\ascsha-clsap\sapmnt** a többi, például mert a **ascsha-dbas**.

### <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Hello adatbázis-példány telepítése

tooinstall hello adatbázis-példány, hajtsa végre az SAP-dokumentáció hello ismertetett hello folyamatot.

### <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Hello második fürtcsomópont telepítése

tooinstall hello második fürt, hello SAP telepítési útmutató hello lépéseit kövesse.

### <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Hello indítási típust hello SAP SSZON Windows szolgáltatáspéldány módosítása

Hello SAP SSZON Windows-szolgáltatás indítási típusát hello túl módosítása**automatikus (Késleltetett indítás)** mindkét fürtcsomóponton.

![60. ábra: Módosítani hello SAP SSZON példány toodelayed automatikus hello szolgáltatás típusát][sap-ha-guide-figure-3050]

_**60. ábra:** módosítani hello SAP SSZON példány toodelayed automatikus hello szolgáltatás típusát_

### <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Hello SAP elsődleges kiszolgáló telepítése

Hello elsődleges Application Server (szolgáltatói)-példány telepítése <*SID*> - di - 0 hello virtuális gépen, amely már a kijelölt toohost hello szolgáltatói CÍMEI. Nincsenek függőségek a Azure vagy DataKeeper-specifikus beállításokat.

### <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Hello SAP további alkalmazáskiszolgáló telepítése

Egy SAP további Application Server (AAS) telepítse az összes hello virtuális gépekre, hogy már kijelölve toohost SAP Application Server-példány. Például a <*SID*> - di - 1 túl <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> Ez befejezi a magas rendelkezésre állású SAP NetWeaver rendszer hello telepítését. A következő folytatásához feladatátvételi tesztelése.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Hello SAP ASC/SCS példány feladatátvétel és SIOS replikációs tesztelése
Könnyen tootest, és figyelje az SAP ASC/SCS-példány feladatátvevő és SIOS lemez replikációs Feladatátvevőfürt-kezelő és hello SIOS DataKeeper felügyelete és konfigurálása eszköz.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>A fürtcsomóponton SAP ASC/SCS-példány fut.

Hello **SAP PR1** fürtcsoport fürtcsomóponton A. fut. Például a **pr1-ASC-0**. Rendelje hozzá a megosztott hello meghajtó S, amely része hello **SAP PR1** fürterőforrás-csoport, és melyik hello ASC/SCS példányt használ, toocluster csomópont azonosítójához.

![61. ábra: Feladatátvevőfürt-kezelő: hello SAP < SID > fürtcsoport A csomóponton fut.][sap-ha-guide-figure-5000]

_**61. ábra:** Feladatátvevőfürt-kezelő: hello SAP <*SID*> fürtcsoport A csomóponton fut._

A hello SIOS DataKeeper felügyeleti és a konfigurációs eszközt hogy adatokat a rendszer szinkron módon replikálja a hello forrás kötet meghajtó fürtcsomóponton egy toohello kötet célmeghajtó S fürtcsomóponton b hello megosztott lemezt Például, hogy a rendszer replikálja a **pr1-ASC-0 [10.0.0.40]** túl**pr1-ASC-1 [10.0.0.41]**.

![62. ábra: SIOS DataKeeper replikálja hello helyi kötet fürtcsomópontból B toocluster csomópont][sap-ha-guide-figure-5001]

_**62. ábra:** SIOS DataKeeper, a fürtcsomópont toocluster csomópont B hello helyi kötet replikálásához_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Feladatátvételt az A csomópont toonode B

1.  Válasszon egyet az alábbi beállítások tooinitiate feladatátvételét hello SAP, <*SID*> csomópont toocluster fürtcsomópontból b fürtcsoport
  - Feladatátvevőfürt-kezelővel  
  - Feladatátvevő fürt PowerShell használata

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Indítsa újra a fürt csomópont A hello Windows vendég operációs rendszerben (hello SAP, az automatikus feladatátvételt kezdeményez <*SID*> csomópont toonode B fürt csoportot).  
3.  Indítsa újra a fürt csomópont A hello Azure-portálon való (hello SAP, az automatikus feladatátvételt kezdeményez <*SID*> csomópont toonode B fürt csoportot).  
4.  Indítsa újra a fürt csomópont A Azure PowerShell használatával (ez elindít egy hello SAP automatikus átvétele <*SID*> csomópont toonode B fürt csoportot).

  A feladatátvétel után hello SAP <*SID*> fürtcsoport fürtcsomóponton b fut. Futó például **pr1-ASC-1**.

  ![63. ábra: A Feladatátvevőfürt-kezelőben, hello SAP < SID > fürtcsoport van fürtcsomóponton futó B][sap-ha-guide-figure-5002]

  _**Ábra 63**: A Feladatátvevőfürt-kezelőben, hello SAP <*SID*> fürtcsoport fürtcsomóponton B fut._

  hello megosztott lemez már csatlakoztatva van a fürt csomópont b SIOS DataKeeper replikálódik adatok forrás kötet meghajtóról S fürt csomópont B tootarget kötet meghajtó fürtcsomóponton A. A replikáló például **pr1-ASC-1 [10.0.0.41]** túl**pr1-ASC-0 [10.0.0.40]**.

  ![Ábra 64: SIOS DataKeeper replikálja hello helyi kötet csomópont B toocluster fürtcsomópontról A][sap-ha-guide-figure-5003]

  _**Ábra 64:** SIOS DataKeeper hello helyi kötet csomópont B toocluster fürtcsomópontról A replikálja._
