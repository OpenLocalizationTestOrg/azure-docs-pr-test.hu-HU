---
title: "Virtuális gépek DBMS telepítésének SAP NetWeaver aaaAzure |} Microsoft Docs"
description: "Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5654dac7-4204-4387-b312-3d8b2898eb3a
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2016
ms.author: sedusch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 501f6fbc2baa379b706e95d2bfba377ac129b382
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-dbms-deployment-for-sap-netweaver"></a>Az SAP NetWeaver Azure virtuális gépek adatbázis-kezelő telepítése
[767598 ]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
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
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
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
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

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
[virtual-machines-azurerm-versus-azuresm]:../../../resource-manager-deployment-model.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:../../virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:../../linux/cli-deploy-templates.md 
[virtual-machines-deploy-rmtemplates-powershell]:../../virtual-machines-windows-ps-manage.md 
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
[virtual-machines-manage-availability-linux]:../../linux/manage-availability.md
[virtual-machines-manage-availability-windows]:../../windows/manage-availability.md
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

Ez az útmutató a megvalósítás és a Microsoft Azure hello SAP szoftverek telepítését hello dokumentációja részét képezi. Ez az útmutató elolvasása, előtt olvassa el a hello [tervezési és megvalósítási útmutató][planning-guide]. Ez a dokumentum különböző relációs adatbázis Management Systems (RDBMS) és a Microsoft Azure virtuális gépek (VM) SAP együtt kapcsolódó termékek hello Azure infrastruktúra használja, mint a szolgáltatási (IaaS) képességeket hello telepítését ismerteti.

hello papír kiegészítés hello SAP dokumentáció és az SAP, képviselő hello elsődleges erőforrások telepítése és a SAP szoftver központi telepítését az adott platformok.

## <a name="general-considerations"></a>Általános megfontolások
Ez a fejezet az SAP-kapcsolódó adatbázis-kezelő rendszert futtató Azure virtuális gépeken szempontok be. Néhány hivatkozások találhatók toospecific adatbázis-kezelő rendszerek ebben a fejezetben. Ehelyett hello adott adatbázis-kezelő rendszerek kezelése a dokumentum után ez a fejezet.

### <a name="definitions-upfront"></a>Definíciók előzetes megfizetése esetén
Hello a dokumentum a következő feltételek hello használjuk:

* IaaS: Szolgáltatott infrastruktúra.
* A PaaS: Platform szolgáltatásként.
* SaaS: Szolgáltatott szoftver.
* SAP összetevő: egy egyedi SAP alkalmazás, például ECC, a Sávszélesség, a megoldás Manager vagy a EP  SAP-összetevők a hagyományos ABAP vagy Java technológiák vagy egy nem NetWeaver alapú alkalmazás, például üzleti objektumok is alapulhat.
* SAP-környezetben: egy vagy több SAP összetevők logikailag egy csoportba tooperform például fejlesztési, a QAS, a képzési, a vész-Helyreállítási vagy a éles üzleti függvényt.
* SAP fekvő: Utal toohello egész SAP eszközök az ügyfél informatikai fekvő. hello SAP fekvő minden üzemi és nem éles környezetben tartalmaz.
* SAP-verzió: hello DBMS réteg és kombinációja alkalmazás réteget, például egy SAP ERP fejlesztőrendszer, SAP BW gazdagépes tesztrendszer, SAP CRM éles rendszer stb. Azure-környezetekhez, hogy a rendszer nem támogatott toodivide egyes két réteg között a helyszíni és az Azure. Ez azt jelenti, hogy egy SAP rendszer vagy a helyszínen telepített vagy telepítve van az Azure-ban. Telepíthet azonban hello különböző egy SAP fekvő Azure vagy a helyszíni rendszeren. Például sikerült hello SAP CRM fejlesztési és tesztelési az Azure-ban rendszerek központi telepítéséhez, de SAP CRM éles rendszer helyszíni hello.
* Csak felhőalapú telepítési: egy központi telepítést, amelyen keresztül a pont-pont nincs csatlakoztatva a hello Azure-előfizetés, vagy ExpressRoute kapcsolat toohello helyszíni hálózati infrastruktúra. Közös Azure dokumentációja az ilyen típusú központi telepítések egyaránt "Csak felhőalapú" telepítések leírása. Ezzel a módszerrel telepített virtuális gépek hello interneten keresztül érhetők el, és a nyilvános Internet végpontok hozzárendelt toohello virtuális gépek Azure-ban. hello a helyszíni Active Directory (AD) és a DNS nincs kibővítve az ilyen típusú központi telepítések tooAzure. Ezért hello virtuális gépek részei hello a helyszíni Active Directoryban. Megjegyzés: Ebben a dokumentumban csak felhő alapú telepítések esetén is meg van adva teljes SAP tájak, amely a helyszíni kizárólag az Azure Active Directory vagy a névfeloldás kiterjesztés nélkül futtatja nyilvános felhőbe. Csak felhőalapú konfigurációk nem támogatottak a termelési SAP rendszerek vagy konfigurációk, ahol az SAP STM vagy más helyszíni erőforrásokat kell toobe használt Azure és a helyszínen található erőforrások üzemeltetett SAP-rendszerek között.
* Létesítmények közötti: Egy olyan forgatókönyvet, hol a virtuális gépek az Azure-előfizetéssel, amely rendelkezik pont-pont, többhelyes vagy hello helyszíni datacenter(s) és az Azure között ExpressRoute kapcsolat telepített tooan ismertet. Közös Azure dokumentációja, az ilyen típusú központi telepítések egyaránt létesítmények közötti forgatókönyv leírása. hello hello kapcsolat oka tooextend a helyi tartomány, a helyszíni Active Directory és a helyszíni DNS az Azure. hello helyszíni fekvő kiterjesztett toohello Azure eszközök hello előfizetés van. Ha ezt a bővítményt, hello virtuális gépek hello a helyi tartomány része lehet. Tartományi felhasználók hello helyszíni tartomány hello kiszolgálókat érhetnek el, és szolgáltatásokat futtathatja a virtuális gépek (például adatbázis-kezelő szolgáltatás). Virtuális gépek közötti kommunikációt és a névfeloldás telepítése a helyszíni és az Azure-ban telepített virtuális gépek lehetséges. Várhatóan toobe hello leggyakoribb példánkban Azure SAP eszközök telepítéséhez. További információkért lásd: [Ez a cikk] [ vpn-gateway-cross-premises-options] és [Ez a cikk][vpn-gateway-site-to-site-create].

> [!NOTE]
> Létesítmények közötti telepítések SAP rendszert futtató Azure virtuális gépek esetén a helyszíni-tartománybeli tagsággal SAP rendszerek éles SAP rendszerek támogatottak. Létesítmények közötti konfigurációk támogatottak a kijelzők telepítése, vagy végezze el az SAP tájak az Azure. Az Azure-ban futó hello teljes SAP fekvő szükség van a helyszíni tartományi és REKLÁMOK részévé virtuális gépek. Hello dokumentáció korábbi verzióiban megtartásról kapcsolatos hibrid-IT-forgatókönyvek, ahol hello kifejezés "Hibrid" hello valójában a létesítmények közötti hálózati kapcsolatot a helyszíni és az Azure közötti feltört eszköz. Ebben az esetben "Hibrid" is, hogy hello Azure virtuális gépek hello azt jelenti, hogy a helyszíni Active Directory.
> 
> 

Néhány Microsoft dokumentációjában létesítmények közötti forgatókönyv különösen az adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ konfiguráció egy kicsit másképp ismerteti. Az SAP hello dokumentumokat hello esetben hello létesítmények közötti forgatókönyv éppen forrni kezd toohaving le egy pont-pont vagy titkos (ExpressRoute) kapcsolatot, valamint toohello tény, hogy hello SAP fekvő elosztása a helyszíni és az Azure között.

### <a name="resources"></a>Erőforrások
hello következő útmutatók érhetők el a hello témakör Azure SAP telepítések:

* [Azure virtuális gépek tervezési és megvalósítási az SAP NetWeaver][planning-guide]
* [SAP NetWeaver Azure virtuális gépek központi telepítését][deployment-guide]
* [Az SAP NetWeaver (Ez a dokumentum) az Azure virtuális gépek DBMS-telepítés][dbms-guide]

a következő SAP megjegyzések hello kapcsolódó toohello a témakör az Azure SAP:

| Megjegyzés száma | Cím |
| --- | --- |
| [1928533] |Az Azure-on SAP-alkalmazásokból: támogatott termékek és az Azure virtuális Gépen |
| [2015553] |A Microsoft Azure SAP: Előfeltételek támogatja |
| [1999351] |Hibaelhárítási továbbfejlesztett Azure megfigyelése az SAP |
| [2178632] |Kulcs figyelési metrikákat az SAP, a Microsoft Azure |
| [1409604] |A Windows virtualizálási: fokozott figyelése |
| [2191498] |Az Azure Linux SAP: fokozott figyelése |
| [2039619] |SAP-alkalmazást a Microsoft Azure használatával hello Oracle-adatbázishoz: támogatott termékek és termékverziók |
| [2233094] |DB6: Azure-ban IBM DB2 Linux, UNIX és a Windows - további információ az SAP-alkalmazásokból |
| [2243692] |A Microsoft Azure (IaaS) virtuális gép Linux: SAP licenc problémák |
| [1984787] |SUSE LINUX Enterprise Server 12: Telepítési megjegyzések |
| [2002167] |Red Hat Enterprise Linux 7.x: telepítés és frissítés |
| [2069760] |Oracle Linux 7.x SAP telepítése és frissítése |
| [1597355] |A Linux rendszerhez használt lapozóterület javaslat |
| [2171857] |Oracle Database 12c - fájlrendszer Linux-támogatás |
| [1114181] |Oracle Database 11g - fájlrendszer Linux-támogatás |


Hello is olvasható [Állapotváltozás Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux tartalmazó összes SAP-jegyzetet.

Rendelkeznie kell Microsoft Azure architektúra hello és a Microsoft Azure virtuális gépeken telepített és üzemeltetett hogyan ismeretét. További információt a <https://azure.microsoft.com/documentation/>

> [!NOTE]
> Folyamatban van, **nem** megvitatása a Microsoft Azure Platform, egy hello Microsoft Azure Platform (PaaS) szolgáltatás által kínált. A dokumentum tárgya rendszert futtató egy adatbázis-kezelő rendszer (DBMS) a Microsoft Azure virtuális gépek (IaaS) ugyanúgy, mint a helyszíni környezetben hello DBMS kell futtatni. Adatbázis-képességeket és funkciók között ezek két ajánlatok nagyon eltérőek, és kell nem keverhetők egymás mellett. További információ: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Azt is megvitatása IaaS, mivel általában hello Windows, Linux és adatbázis-kezelő telepítése és konfigurálása van alapvetően hello ugyanaz, mint bármely virtuális gép vagy a helyszíni volna telepítése operációs rendszer nélküli számítógép. Van azonban néhány architektúra és a rendszer felügyeleti megvalósítási döntésekhez, különböző IaaS használata során. hello a jelen dokumentum célja tooexplain hello adott használt architekturális és a rendszer felügyeleti módszer közötti különbségeket, elő kell készíteni az infrastruktúra-szolgáltatási használatakor.

Általában hello különbséggel, hogy a dokumentum ismerteti, amelyek általános területei a következők:

* SAP rendszerek tooensure hello megfelelő virtuális gép/lemez elrendezését tervezési rendelkezik hello megfelelő adatok fájlelrendezés, és a munkaterhelés számára elegendő IOPS érhető el.
* Hálózat használatának szempontjai IaaS.
* Adott adatbázis szolgáltatások toouse rendelés toooptimize hello adatbázis elrendezésben.
* Infrastruktúra-szolgáltatási biztonsági mentési és visszaállítási szempontok.
* Különböző központi telepítési lemezképek használatával.
* Magas rendelkezésre állás érdekében Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>RDBMS üzembe helyezésének struktúra
A rendezés toofollow Ez a fejezet, a szükséges toounderstand Mi a bemutatott [ez] [ deployment-guide-3] hello fejezete [telepítési útmutató] [ deployment-guide]. Ismerete hello másik Virtuálisgép-sorozat és és az Azure Standard és prémium szintű Storage különbségek legyen megértettem, és ez a fejezet elolvasása előtt ismert.

2015. márciusi, amíg a lemezek, amely tartalmazza az operációs rendszer volt korlátozott too127 GB-nál. Ez a korlátozás kapott vissza a 2015. márciusi (További információ az ellenőrzéshez <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/>). Ott is rendelkezik hello tartalmazó hello operációs rendszer lemezeken ugyanaz, mint bármilyen más lemez méretét. Ettől függetlenül azt még inkább egy struktúra, a központi telepítés hello operációs rendszer, az adatbázis-kezelő és a végleges SAP bináris fájlok esetén külön hello adatbázisfájlok alapján. Ezért várhatóan SAP rendszereken futó Azure virtuális gépek hello alap virtuális gép (vagy lemez) hello operációs rendszer, az adatbázis felügyeleti rendszer futtatható fájljainak és az SAP végrehajtható fájlok telepítve rendelkezik. külön lemezeken tárolja az Azure Storage (Standard vagy prémium szintű Storage) és logikai lemezek toohello eredeti Azure operációsrendszer-lemezkép VM kapcsolt hello DBMS adatok és naplófájlok helyét. 

Függő Azure Standard vagy prémium szintű Storage (például használatával hello DS-méretek és GS sorozatnak virtuális gépeken) használni nincs más kvótákat, az Azure-ban ismertetett [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows]. A lemez elrendezése megtervezésekor a következő elemek hello toofind hello legjobb egyensúlyt hello kvóták szükséges:

* az adatfájlok számának hello.
* hello fájlokat tartalmazó lemezeket hello száma.
* hello IOPS kvóták önálló lemez.
* lemezenként hello adatátvitelt.
* egy Virtuálisgép-méretet lehetséges adatlemeznek hello száma.
* hello teljes tárterületek átviteli sebességének egy virtuális Gépet biztosít.

Azure érvénybe lépteti az IOPS kvóta adatok lemezenként. Ezek a kvóták eltérőek lemezeken Azure standard szintű tárolást és a prémium szintű Storage működtetnek. I/o-késések különböznek a is nagyon hello két típusú tárolókat prémium szintű Storage tényezők jobb i/o-késések kézbesítéséhez. Mindegyik hello eltérő típusú virtuális gép rendelkezik-e, hogy-e képes tooattach adatlemezek száma korlátozott. Egy másik korlátozás, hogy csak bizonyos Virtuálisgép-típusokon kihasználhatják a prémium szintű Azure Storage. Ez azt jelenti, hogy hello döntési bizonyos VM típusok esetében előfordulhat, hogy nem csak kell által vezérelt hello CPU és memória, is hello iops-érték, amelyet késleltetés és a lemez, amely általában a lemezek számát hello méretezése vagy prémium szintű Storage lemezek típusú hello átviteli követelmények. Különösen a prémium szintű Storage a lemez mérete hello is előfordulhat, hogy meghatározni iops-érték és az egyes lemezek megvalósítani toobe igénylő átviteli hello száma.

hello tényt, hogy általános IOPS arány, csatlakoztatva, lemezek hello száma hello és hello mérete hello VM összes kötődnek együtt, egy Azure konfigurálása egy SAP rendszer toobe eltér a helyszíni telepítéshez az okozhat. hello IOPS-korlátok vonatkoznak Logikaiegység-számonként esetében általában konfigurálhatók a helyszíni telepítésekkel. Mivel az Azure Storage ezen korlátok rögzített vagy hello lemez típusa Premium Storage függ. Ezért a helyszíni telepítésekkel rendelkező látható felhasználói konfigurációkat, amelyek különleges végrehajtható fájlok, például SAP sok más köteteket használ, és adatbázis-kezelő vagy az ideiglenes adatbázisok vagy tábla szóközöket speciális kötetet hello adatbázis-kiszolgálók. Ha ilyen egy a helyszíni rendszer áthelyezett tooAzure, azt tooa pazarlás lehetséges IOPS sávszélesség által végrehajtható fájlok, vagy ne hajtsa végre a bármelyikéhez vagy nem IOPS nagy adatbázisokhoz lemezét jelentős vezethet. Ezért az Azure virtuális gépeken azt javasoljuk, hogy hello adatbázis-kezelő és az SAP végrehajtható fájlok lehetőleg hello OS lemezre telepíteni.

hello hello adatbázisfájlokat és a naplófájlok és a használt, Azure Storage hello típusú elhelyezését definiálni kell IOPS, a késés és a átviteli követelmények. A sorrend toohave elég IOPS hello tranzakciónapló, valószínűleg kényszerített tooleverage hello tranzakciónapló több lemez fájlt, vagy használjon nagyobb prémium szintű Storage-lemezt. Ebben az esetben egy volna létre szoftveres RAID-(például Windows Storage készletet a Windows vagy MDADM és LVM (logikai kötetkezelő) Linux) hello lemezzel, hello tranzakciónapló tartalmazó.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Egy Azure virtuális gép D:\ meghajtón egy nem tárolt meghajtót, amely egyes helyi lemezek hello Azure számítási csomóponton alapját. Mivel nem tárolt, ez azt jelenti, hogy a D:\ meghajtóra hello végrehajtott módosítások toohello tartalom elvész, amikor hello virtuális gép újraindítása után. "Végrehajtott módosítások" azt jelenti, mentett fájlokat, a létrehozott címtárakat, a telepített alkalmazások, stb.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure virtuális gépek automatikusan, amely a helyi lemezek alapját hello Azure számítási csomópont nem tárolt meghajtó /mnt/resource meghajtót csatlakoztatni. Mivel nem tárolt, ez azt jelenti, hogy bármilyen változás toocontent /mnt/resource a elvesznek, ha hello virtuális gép újraindítása után. Végrehajtott módosítások azt jelenti,-fájlokat, a létrehozott címtárakat, a telepített alkalmazások, a stb.
> 
> 

- - -
Hello Azure Virtuálisgép-sorozat függ hello helyi lemezein hello számítási csomópont megjelenítése más-más teljesítménybeli, amely kategorizálhatók, például:

* A0-A7: Erősen korlátozott teljesítményét. Nem használható a semmit túl a windows lapozófájlja
* A8-A11: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség
* D sorozatú: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség
* DS-méretek: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség
* G-sorozat: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség
* GS sorozatnak: Nagyon jó teljesítményt nyújt az egyes tízezer IOPS és > 1GB/s átviteli sebesség

A fenti utasítások alkalmazzák a SAP hitelesített toohello Virtuálisgép-típusokon. használja ki az egyes adatbázis-kezelő funkciók – például a tempdb vagy ideiglenes táblán terület szerint jogosultak hello Virtuálisgép-sorozat kiváló IOPS és átviteli sebesség.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>A virtuális gépek és adatlemezek gyorsítótárazás
Adatlemezek hello portal, vagy ha azt csatlakoztatási feltöltött lemezek tooVMs létrehozásakor azt kiválaszthatja, hogy hello i/o-forgalom hello virtuális gép és az Azure storage található lemezek között gyorsítótárba kerüljenek-e. Azure Standard és prémium szintű Storage használata a két technológia az ilyen típusú gyorsítótár. Mindkét esetben hello gyorsítótár maga hello ideiglenes lemezt (a Windows D:\) vagy /mnt/resource Linux virtuális gép hello azonos meghajtók használt hello alapját lemez lenne.

Az Azure standard szintű tárolást hello lehetséges gyorsítótár típusok a következők:

* Gyorsítótárazása
* Olvasási gyorsítótárazás lemezre
* Olvasási és írási gyorsítótárazást

A sorrend tooget következetes és determinisztikus teljesítményét, célszerű hello gyorsítótárazást az Azure standard szintű tárolást tartalmazó lemezein **DBMS kapcsolatos adatokat fájlok, naplófájlok és tábla terület too'NONE "**. hello gyorsítótárazását hello VM hello alapértelmezett maradjanak.

Prémium szintű Azure Storage hello alábbi gyorsítótárazási beállítások vannak:

* Gyorsítótárazása
* Olvasási gyorsítótárazás lemezre

Prémium szintű Azure Storage ajánlott tooleverage **olvasási gyorsítótárazás lemezre az adatfájlok** hello SAP-adatbázist, és válassza a **hello lemezek, a naplófájl (oka) t gyorsítótárazása**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Szoftveres RAID
Már fentiek szerint kell toobalance hello száma IOPS szükséges hello adatbázis-fájlok között hello lemezeinek száma miatt konfigurálhatja, és az Azure virtuális gép / lemez vagy a prémium szintű Storage lemeztípus biztosít maximális IOPS hello. Az IOPS-lemezek terhelése hello legegyszerűbb módja toodeal toobuild szoftveres RAID különböző lemezek hello keresztül. Majd helyezze adatforrás adatfájljainak hello SAP adatbázis-kezelő számos hello kívül hello szoftveres RAID faragottnak logikai Egységeket. Függő hello követelményeiről érdemes tooconsider hello használata prémium szintű Storage, valamint két hello óta három különböző prémium szintű Storage lemezek segítségével magasabb IOPS kvóta, mint a szokásos tároló mérete alapján. Emellett hello jelentős jobban késleltetéséről a prémium szintű Azure Storage által biztosított. 

Ugyanez vonatkozik toohello tranzakciónapló hello különböző adatbázis-kezelő rendszerek. Ezek csak a további Tlog-fájlok hozzáadása a nem segít egyszerre csak egy hello fájlok írása hello adatbázis-kezelő rendszerek óta. Ha magasabb IOPS-díjszabás szükség van egy tárolási alapú egy szabványos lemez biztosíthat, akkor is paritásos több szabványos tárolólemezek keresztül, vagy használhatja a nagyobb prémium szintű Storage lemez típusa túl magasabb IOPS-díjszabás is megadja tényezők kisebb késést biztosít hello írásra I/o hello tranzakciós naplóba.

Az Azure-környezetekhez, amely volna alkalmazást egy szoftveres RAID használatával észlelt helyzetek a következők:

* Tranzakciónapló napló/mégis szükség magasabb iops-értéket, mint az Azure biztosít egyetlen lemezen. Fent említett Ez a logikai egység épület több lemezt a szoftveres RAID használatával keresztül megoldhatók.
* I/o munkaterhelés a egyenetlen eloszlását hello különböző adatok fájlokat hello SAP-adatbázist. Ilyen esetekben egy fedezheti hello kvóta gyakran ahelyett, hogy elérte-e egy adatfájlt. Mivel más adatfájlok még nem kapják meg egyetlen lemezt a Bezárás toohello IOPS beállított kvótát. Ilyen esetekben hello a legegyszerűbb megoldás az egyik toobuild logikai egység több lemezt a szoftveres RAID használatával keresztül. 
* Nem tudja, milyen hello pontos i/o munkaterhelési adatok fájlonként, és csak nagyjából tudja, mit hello a teljes IOPS munkaterhelés elleni hello DBMS van. Ennek legegyszerűbb toodo egy logikai Egységet a hello segítségével egy RAID szoftver toobuild. több lemez a logikai Egységnek mögött kvótáinak hello összege kell majd teljesítéséhez IOPS ismert hello sebessége.

- - -
> ![Windows][Logo_Windows] Windows
> 
> Azt javasoljuk, hogy Windows tárolóhelyek használatával, ha a Windows Server 2012 vagy újabb verzióját futtatja. Hatékonyabb, mint a korábbi Windows Windows csíkozást. Előfordulhat, hogy szükséges toocreate hello Windows Tárolókészletek és a tárolóhelyek PowerShell-parancsok használata Windows Server 2012 operációs rendszert. PowerShell-parancsok hello itt található <https://technet.microsoft.com/library/jj851254.aspx>
> 
> ![Linux][Logo_Linux] Linux
> 
> Csak a MDADM és a LVM (logikai kötetkezelő) nem támogatott toobuild egy szoftveres RAID Linux rendszeren. További információkért olvassa el a következő cikkek hello:
> 
> * [Konfigurálja a szoftveres RAID Linux] [ virtual-machines-linux-configure-raid] (a MDADM)
> * [A Linux virtuális gép az Azure-ban LVM konfigurálása][virtual-machines-linux-configure-lvm]
> 
> 

- - -
Prémium szintű Azure Storage képes toowork rendszerint Virtuálisgép-sorozat hasznosítására vonatkozó szempontok a következők:

* I/o-késéseket okoz, amelyek esetében toowhat SAN/NAS-eszközök kézbesítése bezárásához.
* Igény szerinti tényezők jobb késleltetéséről Azure standard szintű tárolást biztosíthat, mint a.
* Mint mi érhető el az egyes virtuális gép elleni több Standard tárolási lemezzel rendelkező virtuális gépenként magasabb IOPS írja be.

Hello óta az alapul szolgáló Azure Storage replikálja az egyes tooat legalább három lemezt tárolócsomópontok egyszerű RAID 0 csíkozást használható. Nincs szükség tooimplement RAID5 vagy RAID1 van.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>A Microsoft Azure Storage
A Microsoft Azure Storage tárolók hello alap virtuális gép (és az operációs rendszer) és a lemezek vagy a Blobok tooat legalább három különálló tárolócsomópontok. Egy tárfiókhoz vagy a felügyelt lemezes létrehozásakor választási lehetőség van a védelem itt látható módon:

![Georeplikáció engedélyezve az Azure Storage-fiókban][dbms-guide-figure-100]

Az Azure Storage helyi replikációs (helyileg redundáns) biztosít, hogy néhány ügyfél sikerült biztosít toodeploy tooinfrastructure hibája miatt adatvesztés elleni védelmi szintek. A fentiek szerint a ötödik alatt módosítás hello egyik első három négy különböző lehetőségek közül. Keresése szorosabb, azokat is különböztetünk meg:

* **Prémium szintű helyileg redundáns tárolás (LRS)**: prémium szintű Azure Storage szolgáltatás nyújt nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása. Nincsenek hello hello adatainak három replikák azonos Azure-adatközpontban egy Azure-régióhoz. hello példányokra van a különböző tartalék és frissítési tartományok (fogalmak című [ez] [ planning-guide-3.2] hello fejezetében [tervezési útmutatója][planning-guide]). Nem megfelelő tooa tárolási csomópont hibája vagy a lemezt érintő hibákkal működik fog hello adatok replikáját, esetén egy új replika automatikusan létrejön.
* **Helyileg redundáns tárolás (LRS)**: hello hello adatainak három replikák vannak ebben az esetben egy Azure-régióhoz azonos Azure-adatközpontban. hello példányokra van a különböző tartalék és frissítési tartományok (fogalmak című [ez] [ planning-guide-3.2] hello fejezetében [tervezési útmutatója][planning-guide]). Nem megfelelő tooa tárolási csomópont hibája vagy a lemezt érintő hibákkal működik fog hello adatok replikáját, esetén egy új replika automatikusan létrejön. 
* **Földrajzi redundancia tárolás (GRS)**: Ebben az esetben van egy aszinkron replikáció, amely hírcsatornák további három hello adatok egy másik Azure régióban, amely a legtöbb esetben hello replikáinak hello ugyanabban a földrajzi régióban (például Észak-Európában és nyugati régiója Európa). Az eredmény három további replikákat, így sum hat replikák szerepelnek. Ez a változata kiegészítése hello adatok hello földrajzi replikált Azure régióban helyének (írásvédett Georedundáns) olvasás céljából.
* **Redundáns tárolás (ZRS) zónát**: Ebben az esetben az adatok megmaradnak hello hello három replikáinak hello egy Azure-régióban. A [ez] [ planning-guide-3.1] hello fejezete [tervezési útmutatója] [ planning-guide] egy Azure-régió lehet adatközpontok közel. Az LRS hello esetben hello replikák volna kell elosztani hello különböző adatközpontokban, amelyek egy Azure-régiót.

További információ található [Itt][storage-redundancy].

> [!NOTE]
> Az adatbázis-kezelő történő telepítések esetén földrajzi redundáns tárolás hello használata nem ajánlott
> 
> Az Azure Storage Georeplikáció aszinkron. Egyes lemezek replikációját egyetlen virtuális gép nem szinkronizálja a rendszer a zárolás lépés tooa csatlakoztatva. Így nincs megfelelő tooreplicate DBMS különböző lemezek elosztott vagy fájlok szemben a szoftveres RAID több lemez alapján telepített. Adatbázis-kezelő szoftver szükséges, hogy pontosan hello állandó lemezegységet szinkronizálva más logikai egységek és a mögöttes lemezek/forgórészek között. Adatbázis-kezelő szoftver használ a különböző mechanizmusok toosequence I/O írási tevékenység, és egy adatbázis-kezelő jelenti, hogy hello replikáció által megcélzott hello lemezegységet sérült-e, ha ezek még néhány ezredmásodperc változhat. Ezért ha valóban a konfigurációját egy adatbázis több lemezre georeplikált felhőbe, ilyen replikációs kell toobe adatbázis eszközök és funkciók végrehajtását. Egy nem igazolható a Azure Storage-Georeplikáció tooperform a feladatot. 
> 
> hello probléma a legegyszerűbb tooexplain példa rendszerrel. Tételezzük fel, hogy egy SAP rendszer Azure, amelynek tartalmazó adatbázis-kezelő hello adatforrás adatfájljainak nyolc lemezek plusz egy lemezt tartalmazó hello tranzakciósnapló-fájljának töltve. A kilenc lemezekre írt toothem egy egységes módszer szerint toohello DBMS, hogy hello adatok írása toohello adatok vagy a tranzakciós naplófájlok adatokkal rendelkeznek.
> 
> Ahhoz, tooproperly földrajzi-replikálás hello adatokat, és egy egységes adatbázis képet, hello tartalom karbantartása összes kilenc lemezek volna rendelkezik toobe georeplikált hello pontos rendelés hello i/o-műveletek végrehajtódtak hello kilenc különböző lemezek ellen. Azure Storage georeplikáció azonban nem engedélyezi a lemezek közötti toodeclare függőségek. Ez azt jelenti, hogy a Microsoft Azure Storage a georeplikáció nem tud hello tényt, hogy a kilenc különböző lemezek hello tartalmának más kapcsolódó tooeach és hello adatváltozásokat konzisztens csak akkor, ha a hello rendelés hello i/o-műveletek replikál történt összes hello kilenc lemezek között.
> 
> Túl magas, hogy hello georeplikált képek hello forgatókönyvben nem ad meg egy kép konzisztens adatbázis, a rendszer teljesítményét, amely mutatja, földrajzi redundáns tárolás súlyosan is is van folyamatban esélyét hatással a teljesítményre. Összefoglalva nem az ilyen típusú adattároló redundanciája, amely az adatbázis-kezelő típus munkaterhelésekhez.
> 
> 

#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Virtuális merevlemezek a virtuális gép az Azure Storage szolgáltatásfiókok leképezése
Ez a fejezet csak érvényes tooAzure Storage-fiókok. Ha azt tervezi, toouse kezelt lemezek, ebben a fejezetben említett hello korlátozások nem alkalmazhatók. Felügyelt lemezek kapcsolatos további információkért olvassa el a fejezet [kezelt lemezek] [ dbms-guide-managed-disks] a jelen útmutató.

Egy Azure Storage-fiók nem csak olyan felügyeleti szerkezetet, de a tárgyat korlátozás is. Mivel a döntésről bővebben egy Azure standard szintű Storage-fiókot vagy egy Azure prémium szintű Storage-fiók e hello korlátozások eltérőek lehetnek. hello pontos lehetőségeit és korlátait felsorolt [Itt][storage-scalability-targets]

Ezért az Azure standard szintű tárolást fontos toonote korlátozva van a hello IOPS / tárfiók (sor: a "Teljes kérelmek gyakorisága" tartalmazó [hello cikk][storage-scalability-targets]). Emellett egy kezdeti korlátozás van (2015. július) az Azure-előfizetés / 100 tárfiókok. Ezért ajánlott toobalance IOPS virtuális gépek közötti több tárfiókot, ha Azure standard szintű tárolást használ. Mivel egy virtuális ideális használ egy tárfiókot, ha lehetséges. Ezért ha döntésről bővebben az adatbázis-kezelő központi telepítések, ahol minden Azure standard szintű tárolást a tárolt virtuális merevlemez elérhetik a kvótakorlát, csak telepítsen Azure standard szintű tárolást használó Azure Storage-fiók / 30-40 VHD-k. A hello ugyanakkor, ha kihasználja a prémium szintű Azure Storage és toostore nagy adatbázis kötetek, valószínűleg finom IOPS értékben. Egy Azure prémium szintű Storage-fiók azonban eltérő módon határértéknél erősebb korlátozást az adatmennyiség egy Azure standard szintű Tárfiók. Ennek eredményeképpen csak telepítése virtuális merevlemezek belül egy Azure prémium szintű Tárfiók csak korlátozott számú előtt szerezze meg a hello adatok kötet korlátot. Hello végén a "virtuális SAN" képességei iops-érték és/vagy kapacitása korlátozott, vegye egy Azure Storage-fiókot. Ennek eredményeképpen hello feladat marad, mint a helyszíni központi telepítések, toodefine hello elrendezést a VHD-k hello különböző SAP rendszerek hello különböző "képzeletbeli SAN eszközök teljesebb" vagy az Azure Storage-fiókok hello.

Az Azure standard szintű Storage, nem ajánlott eltérőek a tárolási fiók tooa toopresent tárhelyhez lehetőleg egyetlen virtuális gép.

Hello DS vagy a GS sorozatnak Azure virtuális gépek használata esetén is lehetséges toomount VHD-k szabványos Azure Storage-fiókok és a prémium szintű Storage-fiókok. Használati esetekben, például virtuális merevlemezek és adatbázis-kezelő adatok biztonsági mentés írása a szabványos tároló biztonsági és a prémium szintű Storage-naplófájlokat származnak toomind ahol ilyen heterogén tárolás sikerült javítható. 

Felhasználói telepítés és a vizsgálati körülbelül 30 too40 adatbázis az adatfájlok és a naplófájlokat tartalmazó VHD-k helyezhetők egy egyetlen Azure standard szintű tárfiók elfogadható teljesítménnyel alapján. A korábban említett hello korlátozása egy Azure prémium szintű Tárfiók, valószínűleg toobe hello adatok kapacitás tárolható, és nem IOPS.

A SAN eszközök helyszíni, megosztása megköveteli bizonyos figyelőket a rendelés tooeventually észleli a szűk keresztmetszetek egy Azure Storage-fiók. hello Azure Figyelőbővítmény az SAP és hello Azure-portálon is kijelölhető használt toodetect eszközök Azure Storage-fiókok, amelyek lehet, olyan optimálisnál IO teljesítmény foglalt.  Ha ez a helyzet észlel, ajánlott toomove foglalt virtuális gépek tooanother Azure Storage-fiókot. Tekintse meg a toohello [telepítési útmutató] [ deployment-guide] talál részletes információt hogyan tooactivate hello SAP gazdagép figyelési lehetőségek körét.

Ajánlott eljárások Azure standard szintű tárolást és a szabványos Azure Storage-fiókok összefoglalójához egy másik cikkben itt található <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>

#### <a name="f42c6cb5-d563-484d-9667-b07ae51bce29"></a>Felügyelt lemezek
Felügyelt lemezek olyan új erőforrást az Azure Resource Manager az Azure Storage-fiókok tárolt VHD-k helyett használható. Felügyelt lemezek automatikusan igazodni hello azok hello virtuális gép rendelkezésre állási csoport csatolt tooand ezért növekedése hello rendelkezésre állását a virtuális gép és a hello hello virtuális gépen futó szolgáltatásokat. toolearn több, olvassa el a hello [a cikk áttekintése](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview).

SAP jelenleg csak prémium felügyelt lemezeit támogatja. Olvasási SAP Megjegyzés [1928533] további részleteket.

#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-tooazure-premium-storage"></a>Prémium szintű Storage Azure standard szintű tárolást tooAzure adatbázis-kezelő virtuális gépek áthelyezése telepítve
Egy forgatókönyvek, ahol ügyfélként a kívánt toomove Azure standard szintű Storage-ból telepített virtuális gép prémium szintű Azure Storage előforduló azt. Ha a lemezek Azure Storage-fiókok vannak tárolva, ez azonban nem lehet fizikai áthelyezésével hello adatok nélkül. Számos módon tooachieve hello célja van:

* Egy új Azure prémium szintű Tárfiók egyszerűen minden virtuális merevlemezek, virtuális Alaplemez, valamint adatokat VHD-k sikerült másolása. Gyakran kiválasztott VHD-k száma hello Azure standard szintű tárolást hello tényt, hogy szükséges-e hello adatmennyiség miatt nem. Azonban ennyi virtuális merevlemezek hello IOPS miatt van szüksége. Most, hogy tooAzure helyezi át a prémium szintű Storage módszert sikerült módba kevesebb virtuális merevlemezek tooachieve hello azonos IOPS teljesítményt. Mivel hello Azure standard szintű tárolást hello kell fizetnie a használt adatokat, és nem hello névleges lemezméretet, hello VHD-k száma volt valóban számít költségei tekintetében. Azonban a prémium szintű Azure Storage volna kell fizetnie hello névleges lemez méretét. Ezért hello ügyfelek többsége próbálkozzon tookeep hello Azure VHD-k a prémium szintű Storage: hello szám szükséges tooachieve hello IOPS átviteli szükséges. Igen a legtöbb ügyfél döntse el, elleni hello módja egy egyszerű 1:1 másolat.
* Ha még nincs csatlakoztatva, az SAP-adatbázis egy adatbázis biztonsági mentése is tartalmazó egyetlen virtuális merevlemez csatlakoztatása. Hello biztonsági mentés, után összes virtuális merevlemez hello VHD-t tartalmazó hello biztonsági mentés beleértve leválasztotta és másolási hello kiinduló virtuális merevlemez, és VHD hello hello biztonsági mentéssel be egy prémium szintű Azure Storage-fiókba. Majd központilag hello VM hello alap virtuális merevlemez és a csatlakoztatási hello VHD hello biztonsági másolat alapján. Most hozzon létre további üres prémium tárolólemezek hello virtuális gép számára, amelyek használt toorestore hello-adatbázisból. A parancs feltételezi, hogy hello adatbázis-kezelő lehetővé teszi toochange elérési utak toohello adatainak és naplókönyvtárainak fájlok hello visszaállítási folyamat részeként.
* Egy másik lehetőség, egy változata hello korábbi folyamat, ahol csak prémium szintű Azure Storage hello biztonsági másolat virtuális merevlemez másolása és csatolja a újonnan telepített és telepített virtuális gépek ellen.
* hello negyedik lehetőséget válassza amikor rászoruló toochange hello számos az adatbázis fájlt. Ebben az esetben egy SAP homogén rendszer másolása exportálási/importálási használatával hajtaná végre. Helyezze a virtuális Merevlemezt, amely egy Azure prémium szintű Storage-fiók történő másolása fájlok exportálása és tooa toorun hello importálási folyamatok használt virtuális gép csatlakoztatása. Az ügyfelek ezt a lehetőséget használja, főleg, ha azok szeretné, hogy toodecrease hello számos fájlt.

Felügyelt lemezek használatakor tooPremium tárolási áttelepítheti szerint:

1. Hello virtuális gép felszabadítása
2. Ha szükséges, méretezze át a hello virtuális gép tooa méretét, amely támogatja a prémium szintű Storage (például a DS vagy a GS)
3. Hello lemez felügyelt fiók típusa tooPremium (SSD) módosítása
4. A virtuális gép elindítása

### <a name="deployment-of-vms-for-sap-in-azure"></a>Virtuális gépek telepítése az SAP, az Azure-ban
Microsoft Azure toodeploy virtuális gépek több lehetőséget kínál, és a kapcsolódó lemezek. Ezáltal ez a beállítás fontos toounderstand hello különbségek mivel hello virtuális gépek előkészített központi telepítési módszer hello függ eltérőek lehetnek. Általában azt megismerhetők az alábbi fejezetek hello leírt hello forgatókönyvek.

#### <a name="deploying-a-vm-from-hello-azure-marketplace"></a>Hello Azure Piactérről származó virtuális gép telepítése
Ön például a Microsoft vagy harmadik féltől származó tootake megadott hello Azure piactér toodeploy lemezképet a virtuális Gépet. Miután telepítette a virtuális Gépet az Azure-ban, akkor hajtsa végre a hello azonos irányelvek és eszközök tooinstall hello SAP szoftver a virtuális Gépen belül, mint a helyszíni környezetben. Szoftvertelepítés hello SAP hello Azure virtuális Gépen belül, a SAP és a Microsoft ajánlott feltöltése és tárolja hello SAP telepítési adathordozóján lemezek vagy toocreate egy Azure virtuális gép működik-e fájlkiszolgálóként"", amely tartalmazza az összes hello szükséges SAP telepítési adathordozóján.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Virtuális gép és ügyfél-specifikus általános lemezkép központi telepítése
Miatt toospecific javítás követelményeit az operációs rendszer vagy az adatbázis-kezelő verziója hello Azure piactér megadott hello képek előfordulhat, hogy nem felelnek meg az igényeinek. Ezért szükség lehet a saját "private" az operációs rendszer vagy adatbázis-kezelő Virtuálisgép-lemezkép, amely ezután többször is telepíthető virtuális gépek toocreate. tooprepare duplikálva lettek-e ilyen egy "private" lemezképet, az operációs rendszer kell általánosítva a hello hello a helyszíni virtuális gép. Tekintse meg a toohello [telepítési útmutató] [ deployment-guide] kapcsolatos részletes tudnivalókért toogeneralize egy virtuális Gépet.

Ha már telepítette a helyszíni virtuális gépre (különösen a 2-réteg rendszerek) SAP tartalom, módosíthatja hello SAP rendszerbeállítások hello telepítése Azure virtuális gép hello hello példány keresztül SAP Szoftverellátáshoz hello által támogatott eljárás átnevezése után Manager (SAP Megjegyzés [1619720]). Ellenkező esetben hello hello Azure VM telepítése után később hello SAP szoftvert is telepíthet.

Től hello a hello SAP alkalmazás által használt adatbázis tartalmát vagy létrehozhat hello tartalom frissen egy SAP telepítése, vagy importálhatja a tartalom Azure hello DBMS toodirectly képességei kihasználva vagy egy adatbázis-kezelő adatbázis biztonsági mentése a virtuális merevlemez használatával biztonsági mentést a Microsoft Azure-tárolóba. Ebben az esetben, sikerült is készítsen elő VHD-k hello DBMS adatok és jelentkezzen helyi fájlok és majd importálja azokat lemezként az Azure. De hello adatátvitelt adatbázis-kezelő, amely a helyszíni tooAzure első betöltődnek helyszíni előkészített toobe VHD lemezek keresztül is működhetnek.

#### <a name="moving-a-vm-from-on-premises-tooazure-with-a-non-generalized-disk"></a>A virtuális gép áthelyezése a helyszíni tooAzure nem általánosított lemez
Helyszíni tooAzure (a növekedési és a shift) az adott SAP rendszerrel toomove tervezi. Hello lemez, hello az operációs rendszer, a hello SAP bináris fájljai, és a végleges DBMS bináris fájljait tartalmazó plus hello lemezek hello adatokkal feltölteni hajtható végre, és a naplófájlok hello DBMS tooAzure. A fenti #2 tooscenario ellentétes venni hello állomásnév, SAP SID és SAP felhasználói fiókok hello Azure virtuális Gépen, azok hello a helyszíni környezetben volt konfigurálva. Ezért normalizálása üdvözlő kép, nem szükséges. Ebben az esetben az létesítmények közötti forgatókönyv, ahol hello SAP fekvő egy részét futtatja a helyszíni és a részeket az Azure többnyire vonatkozik.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Magas rendelkezésre állás és vészhelyreállítás Azure virtuális gépek
Az Azure kínál a következő magas rendelkezésre állású (HA) és a vészhelyreállítás (DR) helyreállítási funkciók, akkor használjuk SAP, az adatbázis-kezelő rendszerekhez toodifferent összetevők alkalmazandó hello

### <a name="vms-deployed-on-azure-nodes"></a>Az Azure-csomópontokra telepített virtuális gépek
hello Azure Platform nem biztosít például az élő áttelepítési funkciók telepített virtuális gépekhez. Ez azt jelenti, ha nincs karbantartási szükséges a kiszolgálófürt, amelyre telepítve van egy virtuális Gépet, hello VM kell tooget leáll és újraindul. Az Azure-ban karbantartási használatával történik így nevű kiszolgálók fürtök frissítési tartományok. Egyszerre csak egy frissítési tartomány áll rendelkezésre. Egy újraindítás közben közben a virtuális gép le van állítva hello szolgáltatás megszakítását, karbantartást végeznek és a virtuális gép újraindul. A legtöbb adatbázis-kezelő beszállító azonban adja meg a magas rendelkezésre állású és vész-helyreállítási funkció, amely gyorsan újraindítja hello DBMS szolgáltatások egy másik csomópontjára, ha hello elsődleges csomópont nem érhető el. hello Azure Platform kínál a virtuális gépek funkcióit toodistribute, tárolási és más Azure-szolgáltatások között, amely a tervezett karbantartások vagy infrastruktúra hibák frissítési tartományok tooensure csak befolyásolná a virtuális gépek vagy szolgáltatások egy szűk részhalmaza.  Gondos tervezéssel lehetséges tooachieve rendelkezésre állási szintet összehasonlítható tooon helyszíni infrastruktúra.

A Microsoft Azure rendelkezésre állási készletek virtuális gépek logikai csoportosítása vagy, amely biztosítja a virtuális gépek és egyéb szolgáltatásokat elosztott toodifferent hiba és a tartományok frissítése a fürtön belüli úgy, hogy nem csak lenne egy csomópont meghibásodásakor egy bármikor ( olvasásiidő[ez (Linux)] [ virtual-machines-manage-availability-linux] vagy [a (Windows)] [ virtual-machines-manage-availability-windows] cikkben olvashat).

A célra konfigurált virtuális gépek, ahogy az képen látható bevezetésekor toobe kell:

![Az adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ konfiguráció rendelkezésre állási csoport meghatározását][dbms-guide-figure-200]

Ha azt szeretné, hogy a magas rendelkezésre állású konfiguráció toocreate DBMS telepítések (amely független a hello egyéni adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ szolgáltatását használja), hello adatbázis-kezelő virtuális gépek kell:

* Adja hozzá a virtuális gépek toohello hello azonos Azure Virtual Network (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* virtuális gépek magas rendelkezésre ÁLLÁSÚ konfiguráció hello hello is kell hello ugyanazon az alhálózaton. Névfeloldás hello különböző alhálózatok között nincs lehetőség az csak felhőalapú környezetekben csak IP-feloldási működik. Pont-pont vagy ExpressRoute-kapcsolat használatával létesítmények közötti telepítésekhez, egy legalább egy alhálózattal rendelkező hálózatot már létrejött. Névfeloldás történik toohello alapján történik a helyszíni AD-házirendek és a hálózati infrastruktúra. 

[comment]: <> (Ha továbbra is igaz az ARM ESZKÖZBEN MSSedusch TODO tesztelése)

#### <a name="ip-addresses"></a>IP-címek
Lehetőleg toosetup hello virtuális gépek magas rendelkezésre ÁLLÁSÚ konfigurációk rugalmas módon. Hagyatkoznia IP címek tooaddress hello magas rendelkezésre ÁLLÁSÚ közös belül hello magas rendelkezésre ÁLLÁSÚ konfiguráció esetén nem megbízható, az Azure-ban statikus IP-címek használhatók. Az Azure-ban van a két "Leállítás" fogalmak:

* Azure-portálon vagy az Azure PowerShell-parancsmag Stop-AzureRmVM leállítást: Ebben az esetben hello virtuális gép leállítási lekérdezi és deszerializálni lefoglalt. Az Azure-fiókjával már nem fizetnie kell a virtuális Gépet úgy, hogy hello csak költségek, amelyiknél hello felhasznált tárterület. Azonban ha hello magánhálózati IP-cím hello hálózati adapter nem statikus, hello IP-cím van történik, és nem garantált, hogy hello hálózati illesztő hello régi IP-cím hello virtuális gép újraindítása után ismét lekérdezi. Hello végrehajtása Azure-portálon keresztül hello leállt, vagy Stop-AzureRmVM automatikusan meghívásával deaktiválás foglalási okoz. Ha nem szeretné, hogy toodeallocate hello számítógép használja-e a Stop-AzureRmVM - StayProvisioned 
* Hello az operációs rendszer szint virtuális gép leállítása, ha a hello VM lekérdezi állítsa le és ne vonja lefoglalva. Azonban ebben az esetben az Azure-fiókjával továbbra is fizetnie kell a virtuális gép, hello hello ellenére, hogy a rendszer leállítása. Ebben az esetben hello IP-cím tooa hello hozzárendelése VM fennmarad leállt. A Leállítás fázisában hello belül a virtuális gép automatikusan kényszeríti deaktiválás foglalási.

Még a létesítmények közötti forgatókönyv alapértelmezés szerint egy leállítási és deaktiválás foglalási azt jelenti, hogy hello IP-címek a virtuális gép, hello deaktiválás hozzárendelés akkor is, ha a helyi házirendek segítségével a DHCP-beállítások különböző. 

* hello kivétel valamelyik rendeli hozzá a statikus IP-cím tooa hálózati kapcsolat, ha leírt [Itt][virtual-networks-reserved-private-ip].
* Ebben az esetben hello IP-cím rögzített marad mindaddig, amíg hello hálózati illesztő nem törlődik.

> [!IMPORTANT]
> Rendelés tookeep hello teljes központi telepítésben lévő egyszerű és kezelhető is legyen egyértelmű ajánlás toosetup hello hello oly módon, hogy van működő névfeloldás között különböző virtuális gépek érintett hello Azure-ban az adatbázis-kezelő magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási konfiguráció együttműködve virtuális gépeket.
> 
> 

## <a name="deployment-of-host-monitoring"></a>Host Monitoring központi telepítése
Az SAP-alkalmazásokból Azure virtuális gépek hatékony használatát SAP szükséges hello képességét tooget állomás monitoring hello hello Azure virtuális gépeket futtató fizikai állomások adatait. Egy adott SAP a gazdagép ügynöke javítási szintje szükség, amely lehetővé teszi, hogy ezt a lehetőséget az SAPOSCOL és SAP a gazdagép ügynöke. hello pontos javítási szintje SAP Megjegyzés ismertetett [1409604].

Kapcsolatban, hogy a gazdagép adatok tooSAPOSCOL és SAP Állomásügynök összetevők telepítési és hello életciklusának kezelését ezek az összetevők hello részletekért tekintse meg a toohello [telepítési útmutatója][deployment-guide]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>SQL Server rögzítésen tooMicrosoft
### <a name="sql-server-iaas"></a>SQL Server IaaS
A Microsoft Azure-től kezdődően egyszerűen áttelepítheti a meglévő SQL Server-alkalmazások a Windows Server platform tooAzure virtuális gépek épül. SQL Server virtuális gépen lehetővé teszi, hogy Ön tooreduce hello teljes tulajdonosi költség, a központi telepítés, a felügyeleti és a vállalati körének alkalmazások karbantartása ezen alkalmazások tooMicrosoft Azure könnyen áttelepítésével. Az SQL Server egy Azure virtuális gépen, rendszergazdák és fejlesztők számára továbbra is használhatja hello helyszíni fejlesztési és a felügyeleti eszközök érhetők el. 

> [!IMPORTANT]
> Azt nem hanem Microsoft Azure SQL Database esetén ez a Platform, egy szolgáltatás-ajánlat a Microsoft Azure Platform hello. hello ismertető dokumentum tárgya hello SQL Server terméket futtató, a helyszíni központi telepítések elvégzéséhez az Azure virtuális gépek, emelés hello Azure szolgáltatás képesek infrastruktúra ismert. Adatbázis-képességeket és funkciók között ezekről az ajánlatokról két különböző, és kell nem keverhetők egymás mellett. További információ: <https://azure.microsoft.com/services/sql-database/>
> 
> 

Erősen ajánlott tooreview [ez] [ virtual-machines-sql-server-infrastructure-services] a folytatás előtt.

Hello a következő szakaszok darabok hello dokumentáció alapján hello hivatkozásra a fenti részeinek összesített értéket, és említett. SAP körül rögzítésen is szerepelnek, és néhány olyan fogalmat, részletesebben ismertetjük. Azonban ajánlott toowork keresztül hello dokumentáció első fent hello SQL Server adott dokumentációjában elolvasása előtt.

Van néhány SQL Server IaaS konkrét információk tudnia kell, mielőtt továbblépne:

* **Virtuális gép SLA**: van szolgáltatásiszint-szerződésben garantált virtuális gépeken futó Azure, amely itt található: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL-verzió támogatja**: számára, SAP, támogatott SQL Server 2008 R2 és újabb rendszer a Microsoft Azure virtuális gépen. Korábbi verziók nem támogatottak. Tekintse át az általános [támogatási nyilatkozattal](https://support.microsoft.com/kb/956893) további részleteket. Vegye figyelembe, hogy általában SQL Server 2008 Microsoft által támogatott is. Azonban miatt toosignificant funkciót az SAP, megjelent az SQL Server 2008 R2, SQL Server 2008 R2 a SAP hello minimális verziójának. Ne feledje, hogy az SQL Server 2012 és a 2014 lett bővítve mélyebb integrálása hello IaaS-forgatókönyveknél (például biztonsági mentése közvetlenül az Azure Storage szemben). Ezért azt korlátozza a papír tooSQL Server 2012 és a legújabb javítási szintje a 2014 az Azure-bA.
* **Az SQL által nyújtott szolgáltatások**: legtöbb SQL Server szolgáltatást támogat a Microsoft Azure virtuális gépeken néhány kivétellel. **SQL Server feladatátvételi fürtszolgáltatási megosztott lemezek használata nem támogatott**.  Elosztott technológiák, például adatbázis-tükrözés, AlwaysOn rendelkezésre állási csoportok, replikáció, a Naplóküldés és a Service Broker támogatottak egyetlen Azure-régión belül. Az SQL Server AlwaysOn is támogatott különböző Azure-régiók közötti itt leírtak szerint: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Felülvizsgálati hello [támogatási nyilatkozattal](https://support.microsoft.com/kb/956893) további részleteket. A hogyan toodeploy AlwaysOn-konfigurációjának látható példa [ez] [ virtual-machines-workload-template-sql-alwayson] cikk. Emellett, olvassa el a dokumentált gyakorlati tanácsok hello [Itt][virtual-machines-sql-server-infrastructure-services] 
* **SQL-teljesítmény**: vagyunk abban, hogy a Microsoft Azure futtatott virtuális gépek jól hajtsa végre az összehasonlítási tooother nyilvános felhő virtualizálási ajánlatokat, de az egyes eredmények változhat. Tekintse meg [ez] [ virtual-machines-sql-server-performance-best-practices] cikk.
* **Az Azure piactérről lemezképekkel**: hello leggyorsabb módon toodeploy egy új Microsoft Azure virtuális gép toouse hello Azure Piactérről származó lemezkép. Nincsenek hello Azure piactérről, amelyekben megtalálható az SQL Server lemezképeket. hello képek, ahol az SQL Server már telepítve van az SAP NetWeaver alkalmazások azonnal nem használható. hello oka hello alapértelmezett SQL Server-rendezés telepítve van ezen lemezképek és nem szükséges a SAP NetWeaver rendszerek hello rendezés belül. A rendezés toouse kép, ellenőrizze a hello lépéseit fejezetben [kívül a Microsoft Azure piactérről hello SQL kiszolgálói lemezkép használatával][dbms-guide-5.6]. 
* Tekintse meg [díjszabás](https://azure.microsoft.com/pricing/) további információt. Hello [SQL Server 2012 licencelése útmutató](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) és [SQL Server 2014 licencelési útmutató](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) egy fontos erőforrás is vannak.

### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>SQL Server-beállítási útmutatója SAP-kapcsolódó SQL Server-telepítések az Azure virtuális gépeken
#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>VM-/ VHD-szerkezet SAP-kapcsolódó SQL Server-telepítések kapcsolatos javaslatok
Megfelelően hello általános leírása, SQL Server végrehajtható fájlokat kell helyét, vagy telepíteni hello rendszermeghajtóján hello virtuális gép operációsrendszer-lemez (C: meghajtó\).  Általában a legtöbb hello SQL Server rendszer-adatbázisokat nem használhatók magas szinten SAP NetWeaver munkaterhelés szerint. Ezért hello rendszeradatbázisokban (master, msdb, és a modell) SQL-kiszolgáló továbbra is hello C:\ meghajtó is. Kivétel tempdb, amely néhány SAP ERP-szolgáltatásokat és minden BW munkaterhelések hello esetben előfordulhat, hogy nagyobb adatmennyiség vagy hello nem sorolhatók I/O műveletek kötet lehet eredeti virtuális gép. Ilyen rendszerek esetében hello a következő lépéseket kell végrehajtani:

* Helyezze át a hello elsődleges tempdb adatok (oka) t toohello hello elsődleges adatfájllal hello SAP adatbázis azonos logikai meghajtón.
* Adja hozzá a semmilyen további tempdb adatok fájlok tooeach hello az más logikai meghajtókat tartalmazó hello SAP felhasználói adatbázis adatfájlt.
* Adjon hozzá hello tempdb logfile toohello logikai meghajtót, hello felhasználói adatbázis naplófájlját tartalmazó.
* **Kizárólag a helyi SSD meghajtókat használó Virtuálisgép-típusokon** hello számítási csomópont tempdb adatainak és naplókönyvtárainak a fájlok előfordulhat, hogy helyezhető el a D:\ meghajtóra hello. Azonban előfordulhat, hogy ajánlott toouse több tempdb adatfájlokat. Ne feledje eltérőek a D:\ meghajtóra kötetek hello Virtuálisgép-típussá alapján.

Ezek a beállítások engedélyezése a tempdb tooconsume képes tooprovide hello rendszermeghajtó állónál nagyobb területet. A sorrend toodetermine hello megfelelő tempdb mérete egy hello tempdb méretek meglévő operációs rendszert, a helyi ellenőrizheti. Ilyen konfiguráció emellett lehetővé tenné IOPS számok tempdb, amely nem adható meg a rendszermeghajtó hello ellen. Újra a helyi rendszert futtató rendszerek lehet használt toomonitor i/o-munkaterhelés tempdb ellen, hogy Ön származtathatók-hello IOPS számok toosee várja meg a tempdb.

Virtuálisgép-konfiguráció, egy SAP-adatbázist az SQL Server fut, amely, és ahol kerülnek, hello D:\ meghajtóra tempdb adatok és a tempdb logfile alábbihoz hasonlóan fog kinézni:

![Az Azure infrastruktúra-szolgáltatási virtuális gép SAP referencia-konfiguráció][dbms-guide-figure-300]

Vegye figyelembe, hogy hello D:\ meghajtóra van különböző méretű hello Virtuálisgép-típussá függ. Az tempdb hello méretkövetelményt függő valószínűleg kényszerített toopair tempdb adatok hello a naplófájlok adatbázis adatok SAP naplófájlokat és azokban az esetekben, ahol D:\ meghajtóra túl kicsi.

#### <a name="formatting-hello-disks"></a>Hello lemezek formázása
Az SQL Server hello NTFS blokkméret tartalmazó SQL Server adatainak és naplókönyvtárainak lemezek fájlok 64K kell lennie. Nincs szükség tooformat hello D:\ meghajtón van. A meghajtó rendelkezik előre formázott.

A sorrend toomake arról, hogy hello visszaállítási vagy -adatbázisok létrehozását nem inicializálja hello adatfájlok által hello fájlok hello tartalmának nullázást, egy győződjön meg arról, hogy hello felhasználói környezet hello SQL Server-szolgáltatás fut-e a van beállítva egyes. Általában hello Windows felügyeleti csoportba tartozó felhasználók rendelkezik ezekkel a jogosultságokkal. Ha hello SQL Server szolgáltatás hello felhasználó nem Windows rendszergazda felhasználó környezetében fut, akkor tooassign adott felhasználó hello felhasználó jobbra "Feladatok".  A Microsoft Tudásbázis hello részletek: <https://support.microsoft.com/kb/2574695>

#### <a name="impact-of-database-compression"></a>Adatbázis-tömörítés hatása
A konfigurációk, ahol a i/o-sávszélesség korlátozó tényezővé válhat minden mértékcsoport, ami csökkenti az IOPS toostretch hello munkaterhelés egy futtathatók az IaaS-forgatókönyveknél, például Azure segítséget. Ezért ha még nem végzett, SQL Server PAGE-tömörítés alkalmazása erősen ajánlott SAP és a Microsoft egy meglévő SAP adatbázis tooAzure feltöltés előtt.

hello ajánlás tooperform adatbázis tömörítési tooAzure feltöltés előtt adja ki két oka:

* feltöltött adatok toobe hello mérete kisebb.
* hello tömörítési végrehajtási hello időtartama rövidebb, feltéve, hogy az egyik használható erősebb hardver, további processzorok vagy magasabb i/o-sávszélesség legfeljebb I/O várakozási ideje a helyszíni.
* Adatbázis kisebb méretű vezethet a lemezfoglaláshoz tooless költségek

Adatbázis típusú tömörítést, valamint egy Azure virtuális gépek azonban nem helyi. További részletes információt a toocompress létező SAP SQL Server-adatbázis, ellenőrizze, itt: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>

### <a name="sql-server-2014--storing-database-files-directly-on-azure-blob-storage"></a>SQL Server 2014 – közvetlenül az Azure Blob Storage tárolóban tárolni adatbázisfájlok
Az SQL Server 2014 megnyitja hello lehetőségét toostore adatbázisfájlok közvetlenül az Azure Blob-tároló hello "burkolót" felhasználókat ezekbe a csoportokba a virtuális merevlemez nélkül. Különösen a szabványos Azure Storage vagy kisebb Virtuálisgép-típusokon lehetővé teszi adott iops-érték, amely akkor kikényszeríti a lemezek, amelyek csatlakoztatott toosome kisebb Virtuálisgép-típusokon lehetnek korlátozott számú hello korlátai által megszabott de kapcsolatos forgatókönyvek. Ez a módszer a felhasználói adatbázisokat azonban nem az SQL Server rendszer-adatbázisokat. Azt is működik, az SQL Server adatainak és naplókönyvtárainak fájlokat. Ha azt szeretné, hogy toodeploy egy SAP SQL Server-adatbázis használati ideje ahelyett, hogy így "alkalmazásburkoló" azt a virtuális merevlemezeket, vegye figyelembe a következőket hello:

* hello használt Tárfiók igények toobe a hello egy Azure-régióban, a másik pedig a virtuális gép az SQL Server fut használt toodeploy hello hello.
* Különböző Azure Storage-fiókok keresztül korábbi VHD-k hello terjesztési felsorolt feltételek vonatkoznak, a központi telepítések, valamint a metódus. Azt jelenti, hogy hello hello korlátozások hello Azure Storage-fiók i/o-műveletek száma.

[comment]: <> (MSSedusch TODO, de ez a beállítás használja hálózati és tárolási sávszélesség-nem, akkor nem?)

A központi telepítési típus adatait az itt felsorolt: <https://docs.microsoft.com/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure>

Rendelés toostore SQL Server adatfájlokhoz közvetlenül a prémium szintű Azure Storage kell itt dokumentált toohave a minimális SQL Server 2014 rendszerhez – javítás kiadásban: <https://support.microsoft.com/kb/3063054>. Az SQL Server 2014 hello kiadott verziójával működik a Azure standard szintű tárolást SQL Server-adatfájlok tárolásához. Azonban a hello nagyon azonos javítások másik adatsorozatot tartalmaz, amelyek hello közvetlen használata az Azure Blob Storage SQL Server-adatfájlok és a biztonsági mentések megbízhatóbb tartalmaznak. Ezért azt javasoljuk, ezek a javítások általában.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 pufferkészlet-kiterjesztés
Az SQL Server 2014 bevezetett új szolgáltatása, a pufferkészlet-kiterjesztés nevezzük. Ez a funkció az SQL Server, amelyet a helyi SSD-k egy kiszolgáló vagy a virtuális gép által támogatott második szintű gyorsítótárával memória hello pufferkészlet terjeszti ki. Ez lehetővé teszi egy nagyobb adatok "a memóriában" munkakészletének tookeep. Összehasonlított tooaccessing Azure standard szintű tárolást hello hozzáférés hello puffer készletbe, amely egy Azure virtuális gép helyi SSD meghajtókon tárolt hello kiterjesztését be a gyorsabb számos tényező közé tartoznak.  Ezért hello helyi D:\ meghajtóra, amelyek kiváló IOPS és átviteli hello VM típusú, ami lehet egy nagyon ésszerű módszer tooreduce hello IOPS betöltése Azure Storage ellen, és jelentősen javíthatja a lekérdezések válaszidejét. Ez vonatkozik, különösen akkor, ha nem a prémium szintű tároló. Prémium szintű Storage és a prémium szintű Azure olvasási gyorsítótár hello hello számítási csomóponton hello használata esetén ajánlott az adatfájlok, nincs lényeges különbség várható. Oka az, hogy mindkét gyorsítótárak (SQL Server a pufferkészlet-kiterjesztés és prémium szintű Storage olvasási gyorsítótár) hello helyi lemezek hello számítási csomópontok használ.
Ez a funkció kapcsolatos további tudnivalókért tekintse meg a jelen dokumentáció: <https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension> 

### <a name="backuprecovery-considerations-for-sql-server"></a>SQL Server biztonsági mentés/helyreállítás szempontjai
Az Azure SQL-kiszolgáló telepítésekor meg kell vizsgálni a biztonsági mentési módszer. Akkor is, ha a rendszer hello rendszer nem hatékony, hello SAP adatbázis SQL-kiszolgáló által üzemeltetett másolatot kell rendszeres időközönként. Azure Storage három képek tartja, mert a biztonsági mentés már kevésbé fontosak a tekintetben toocompensating tárolási crash. hello prioritású virtuális gép a megfelelő biztonsági mentési és helyreállítási tervek karbantartása oka, hogy több is kompenzálják a logikai kézi/hibákat idő helyreállítási funkciók pont megadásával. Így hello célja tooeither biztonsági mentések toorestore hello adatbázis használata vissza bizonyos tooa pont idő vagy toouse hello a biztonsági másolatok Azure tooseed egy másik rendszer hello meglévő adatbázis másolása. Például, hogy nem átvinni egy 2 szintű SAP konfigurációs tooa 3 szintű rendszer beállítása hello azonos rendszer biztonsági mentésének visszaállításával.

Három különböző módon toobackup SQL Server-tooAzure tárolási van:

1. SQL Server 2012 CU4 és magasabb can natív biztonsági mentési adatbázisok tooa URL-címet. Ez részleteit a hello blog [új funkció az SQL Server 2014-rész 5 – biztonsági mentés/visszaállítás fejlesztések](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Című [SQL Server 2012 SP1 CU4 vagy újabb][dbms-guide-5.5.1].
2. SQL Server-verziókban előzetes tooSQL 2012 CU4 használhat egy átirányítási funkció toobackup tooa VHD és alapvetően áthelyezése hello írási adatfolyam felé az Azure tárolási helye, amely konfigurálva van. Című [SQL Server 2012 SP1 CU3 és a korábbi kiadásokban][dbms-guide-5.5.2].
3. végső hello metódus-tooperform egy hagyományos SQL Server biztonsági másolat toodisk parancs, a lemez eszköz. Ez azonos toohello helyszíni telepítési minta és részletei a jelen dokumentum nem tárgyalja.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 és újabb verziók
Ez a funkció lehetővé teszi toodirectly biztonsági mentési tooAzure BLOB Storage tárolóban. Ezzel a módszerrel nélkül biztonsági másolatot tooother lemezek, amelyek lemez és IOPS kapacitás volna felhasználását. hello ötlet alapjában ezt:

 ![Azure Storage-BLOBBÓL SQL Server 2012 biztonsági másolat tooMicrosoft használatával][dbms-guide-figure-400]

hello ebben az esetben előnye, hogy egy nem kell toospend lemezek toostore SQL Server biztonsági mentése. Így kevesebb lemezt lefoglalt rendelkezik, és hello IOPS használhat adatok és a naplófájlok a lemez teljes sávszélesség. Vegye figyelembe, hogy hello maximális méretét, a biztonsági mentés korlátozott tooa legfeljebb 1 TB-os "Korlátozások" hello című részben ismertetett módon: <https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url#limitations>. Ha hello biztonsági másolatának mérete, annak ellenére, hogy az SQL Server biztonsági másolat tömörítésével lehetett meghaladná a 1 TB-nál, hello fejezetben ismertetett funkció [SQL Server 2012 SP1 CU3 és a korábbi kiadásokban] [ dbms-guide-5.5.2] ebben a dokumentumban kell a toobe használt.

[Kapcsolódó dokumentáció](https://docs.microsoft.com/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure) nem közvetlenül az Azure BLOB-tároló toorestore hello visszaállítása az Azure Blob-tárolóban biztonsági másolatból adatbázisok leíró javasoljuk, hogy hello biztonsági mentés > 25 GB. Ebben a cikkben hello javaslat teljesítménnyel kapcsolatos szempontok és toofunctional korlátozások miatt nem egyszerűen alapul. Ezért különböző feltételeket alkalmazhatnak eseti alapon.

Ilyen típusú biztonsági mentést beállítása és kihasználhatók dokumentációjában található [ez](https://docs.microsoft.com/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) oktatóanyag

Példa hello feladatütemezési lépés olvasható [Itt](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Biztonsági mentések automatizálása, van a legmagasabb fontosság toomake meg arról, hogy minden egyes biztonsági másolathoz hello Blobok vannak más a neve. Máskülönben felül, és hello restore-lánc megszakad.

Sorrendben nem toomix be többek között hello három különböző típusú biztonsági mentéseket ajánlott toocreate különböző tárolók a biztonsági mentésekhez használt tárfiók hello alatt. hello tárolók lehet virtuális gép csak vagy virtuális gép és a biztonsági mentés típusa. hello séma nézhet:

 ![Azure Storage-BLOBBA – SQL Server 2012 biztonsági másolat tooMicrosoft külön Tárfiókot a különböző tárolók használata][dbms-guide-figure-500]

Hello a fenti példában hello biztonsági mentések volna nem futtatható, irányítson át ugyanazt a tárhelyet fiók, ahol hello hello a virtuális gépek vannak telepítve. Egy új tárfiókot kifejezetten a hello biztonsági mentések lenne. Belül hello storage-fiókok nem lenne hello típusú biztonsági mentési és hello nevet a virtuális gép mátrixa létre különböző tárolók. Ilyen Szegmentálás teszi, hogy könnyebben tooadministrate hello biztonsági másolatainak hello különböző virtuális gépek.

hello Blobok egy közvetlenül írja hello biztonsági mentések, ne adja hozzá a virtuális gépek hello adatlemezek toohello száma. Ezért hello egyes VM SKU hello adatok csatlakoztatott adatlemezek hello legfeljebb egy sikerült maximalizálása és tranzakciós naplófájlt, és továbbra is végre hajtani egy biztonsági mentési tárolót ellen. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 és rendszerek régebbi kiadásaiban
hello el kell végeznie a rendelés tooachieve a biztonsági mentés közvetlenül szemben Azure Storage első lépéseként lenne túl kapcsolódó toodownload hello msi[ez](https://www.microsoft.com/download/details.aspx?id=40740) KBA cikk.

Töltse le a hello x64 telepítési fájl- és hello dokumentációját. hello fájl nevű programot telepíti: "Microsoft SQL Server biztonsági másolat tooMicrosoft Azure eszköz". Alaposan olvassa el a hello dokumentációt hello termék.  hello eszköz alapvetően hello a következő módon működik:

* Az SQL Server ügyféloldali hello, egy hello SQL Server biztonsági másolat helye van megadva (ne használjon hello D:\ meghajtóra ez).
* hello az eszköz lehetővé teszi toodefine szabályok, amelyek a biztonsági mentések toodifferent Azure Storage tárolók használt toodirect különböző típusú lehet.
* Miután hello szabályok helyen, a hello eszköz hello írási adatfolyama hello biztonsági mentési tooone a hello VHD-k/lemezek toohello Azure tárolási helye, amely korábban meghatározott irányítja át.
* hello eszköz hagy egy kis helyettes néhány KB méretű VHD/lemez, amely meg van adva az SQL Server hello hello a biztonsági mentés. **Mivel ez szükséges toorestore újra az Azure Storage hello tárolóhelyen kell hagyni ezt a fájlt.**
  * Ha elvesztette hello helyettes fájl (például a veszteség hello adathordozón lévő hello helyettes fájl), és biztonsági mentése a Microsoft Azure Storage-fiók tooa hello lehetőséget választotta, előfordulhat, hogy helyreállítása hello helyettes fájl által a Microsoft Azure Storage keresztül letölti az hello tároló el helyezni. Meg kell majd helyezzen hello helyettes fájl egy mappát a helyi számítógépen hello ahol hello eszköz konfigurált toodetect és feltöltése toohello ugyanabban a tárolóban hello ugyanazt a titkosítási jelszót Ha titkosítás hello eredeti szabály volt használva. 

Ez azt jelenti, hogy hello séma SQL Server újabb kiadásaiban a fent leírt módon elhelyezheti helyen, valamint az SQL Server kiadásai, amely nem engedélyezi a közvetlen cím az Azure tárolási helyét.

Ez a metódus nem használható az SQL Server újabb kiadásokban, amely támogatja a biztonsági mentését natív Azure Storage ellen. Kivételek, ahol az Azure hello natív biztonsági mentéssel korlátozásai blokkolják a natív biztonsági mentés végrehajtása az Azure.

#### <a name="other-possibilities-toobackup-sql-server-databases"></a>Egyéb lehetőségek toobackup SQL Server-adatbázisok
Egyéb lehetőségek toobackup adatbázisok tooattach további adatok lemezek tooa, amelyekkel toostore biztonsági mentéseket a virtuális gép. Ebben az esetben meg arról, hogy nem futnak teljes hello lemezek toomake kellene. Ha hello esetben toounmount hello lemez lenne szükséges, és így toospeak "archiválhatja", és cserélje le egy új üres lemez. Elérési út leáll, ha azt szeretné, tookeep ezeket külön Azure Storage-fiókok a VHD-k hello hello adatbázis fájlokkal gazdarendszerhez hello a virtuális merevlemezeket.

A második lehetőség, toouse a nagy virtuális gépek, amelyeken sok lemezek csatlakoztatva, például egy 32VHDs rendelkező D14. A rugalmas környezetekben, ahol sikerült készít megosztások, amely szerint a rendszer használja majd biztonsági mentésének célhelyét hello különböző adatbázis-kezelő kiszolgálók tárolóhelyek toobuild használja.

Néhány ajánlott eljárás a kapott a következő dokumentált [Itt](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) is. 

#### <a name="performance-considerations-for-backupsrestores"></a>A biztonsági mentés/visszaállítás teljesítménnyel kapcsolatos szempontok
Operációs rendszer nélküli telepítések, mint a biztonsági mentés/visszaállítás teljesítménye minden függ hány kötetek párhuzamosan olvasható, és előfordulhat, hogy milyen átviteli sebességgel hello a köteteket. Ezenkívül hello a biztonsági másolatok tömörítését által használt CPU-felhasználás előfordulhat, hogy fontos szerepet játszik a virtuális gépek csak másolatot tooeight CPU szálak. Ezért egy veheti fel:

* hello lemezek száma kevesebb hello használt toostore hello adatfájlok hello kisebb hello a teljes teljesítményt való olvasáskor.
* hello VM CPU szálainak száma kisebb hello hello, hello hello súlyosabb hatását a biztonsági másolatok tömörítését.
* kevesebb célok (BLOB, virtuális merevlemezek vagy lemezek) hello toowrite hello történő biztonsági mentés, kisebb hello átviteli hello.
* hello kisebb hello Virtuálisgép-méretet, hello kisebb hello tárolási átviteli kvótát írása és olvasása Azure Storage-ból. E hello közvetlenül másolatoknak a Azure Blob, vagy hogy ezeket tárolja az Azure BLOB újra tárolt VHD-k függetlenül.

A Microsoft Azure Storage-BLOBBA hello biztonsági mentési célként a korábbi kiadásokban használatakor minden egyes biztonsági másolat korlátozott toodesignating csak egy URL-cím célját áll.

De a régebbi kiadásokban hello "Microsoft SQL Server biztonsági másolat tooMicrosoft Azure eszköz" használva határozhat meg egynél több cél. Az egynél több cél hello biztonsági mentés méretezhető és hello átviteli hello biztonsági másolatának nagyobb. Ez azt eredményezi majd, valamint az Azure Storage-fiók hello több fájlt. A tesztelés során, több fájl célhelyre mindenképpen érhet el egy hello átviteli, melyik el lehet érni, a biztonsági mentési bővítményekkel hello segítségével végrehajtani az SQL Server 2012 SP1 CU4 meg. Akkor is nem blokkolja a hello 1 TB-os korlát hello natív biztonsági mentéssel hasonlóan az Azure.

Ne feledje azonban, hello átviteli is hello Azure Storage-fiókot használ hello biztonsági mentés helye hello függ. Előfordulhat, hogy képet toolocate hello tárfiók más régióban, mint hello virtuális gépek futnak. Például meg szeretné futtatni hello Virtuálisgép-konfiguráció Nyugat-Európa, de put hello Storage-fiók mentése elleni tooback használhatja a Észak-Európa. Amely alapértékekkel hello biztonsági mentési átviteli hatással van, és, a 150MB/s átviteli valószínűleg nem toogenerate vagy toobe lehetséges azokban az esetekben, ahol hello céloldali tárfiók és hello virtuális gépek futnak a hello tűnik regionális ugyanabban az adatközpontban.

#### <a name="managing-backup-blobs"></a>Biztonsági mentési bináris objektumok kezelése
Nincs olyan követelmény toomanage hello biztonsági mentések saját. Hello elvárás, hogy hány blobok gyakori tranzakciónapló biztonsági mentései végrehajtásával jöttek létre, mert ezek blobok felügyeleti könnyen is működési hello Azure-portálon. Ezért fontos recommendable tooleverage egy Azure Tártallózó. Van több jó néhányat a meglévők közül érhető el, amelyek segíthetnek az Azure-tárfiók toomanage

* Microsoft Visual Studio telepített Azure SDK-val (<https://azure.microsoft.com/downloads/>)
* A Microsoft Azure Tártallózó (<https://azure.microsoft.com/downloads/>)
* Harmadik féltől származó eszközök

Bővebb információt a biztonsági másolat és az Azure-on SAP, tekintse meg túl[SAP biztonsági útmutató hello](sap-hana-backup-guide.md) további információt.

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Microsoft Azure piactérről hello kívül SQL kiszolgálói lemezkép használatával
A Microsoft hello Azure piactérről, amely már tartalmazza az SQL Server verzióinak a virtuális gépek kínál. Az SAP-ügyfelek, akik licencek szükségesek az SQL Server és a Windows a problémát valószínűleg egy lehetőség toobasically borítóján hello szükség által telepített SQL Server virtuális gépeinek forgó licenceket. Rendelés toouse az SAP, kép a következő szempontok hello kell végrehajtott toobe:

* hello SQL Server nem-a próbaverziók csak egy "Csak Windows" virtuális Gépet az Azure piactérről telepített-nál magasabb költségek szerezni. Tekintse meg az alábbi cikkek toocompare árak: <https://azure.microsoft.com/pricing/details/virtual-machines/windows/> és <https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/>. 
* Csak használhatja az SQL Server-verziókban, SAP, például az SQL Server 2012 által támogatott.
* hello rendezés hello SQL Server-példány, érhető el hello Azure piactér hello virtuális gépeken telepített nincs hello rendezés SAP NetWeaver hello SQL Server-példány toorun igényel. Hello rendezés módosíthatja, ha a szakasz a következő hello hello utasításait.

#### <a name="changing-hello-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Hello Microsoft Windows vagy SQL Server virtuális gép az SQL Server-rendezés módosítása
Miután hello SQL Server-rendszerképeit hello Azure piactér nem állíthatók be toouse hello rendezést, amely azonban szükséges a SAP NetWeaver alkalmazások, toobe hello telepítés után azonnal módosítani kell. Az SQL Server 2012-ben ezt megteheti a hello lépéseket követve, amint hello virtuális gép van telepítve, és a rendszergazda a hello toolog telepített virtuális gép:

* Nyisson meg egy Windows parancssori ablakban "rendszergazdaként".
* Hello directory tooC:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012 módosítása.
* Hello parancs hajtható végre: Setup.exe/test/művelet REBUILDDATABASE InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS = =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
  * `<local_admin_account_name`> hello-fiók van definiálva hello rendszergazdai fiók keresztül hello gyűjtemény első alkalommal hello a virtuális gép hello telepítésekor.

hello folyamat csak kell eltarthat néhány percig. Rendelés toomake meg arról, hogy a hello lépés befejeződött hello megfelelő eredménnyel, hogy hajtsa végre a lépéseket követve hello:

* Nyissa meg az SQL Server Management Studiót.
* Nyissa meg a lekérdezési ablakban.
* Hello parancs sp_helpsort hello SQL Server főadatbázis hajtható végre.

szükségeskonfiguráció-hello eredmény hasonlóan kell kinéznie:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Ha ez nem hello eredményt, állítsa le a SAP telepítése, és vizsgálja meg, miért hello setup parancs volt a várt módon működik. Az SQL Server-példány az SQL Server különböző kódlapokat alakzatot SAP NetWeaver alkalmazások telepítését, mint egy fent említett hello van **nem** támogatott.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server magas rendelkezésre állású az SAP, az Azure-ban
Ahogy azt korábban említettük, a dokumentum korábbi, nincs nincs lehetősége toocreate megosztott tárolóról, ami hello legrégebbi SQL Server magas rendelkezésre állású funkcióit hello használata szükséges. Ez a funkció a a Windows Server feladatátvételi fürt (WSFC) egy megosztott lemez hello felhasználói adatbázisok (és végül a tempdb) segítségével telepíti két vagy több SQL Server-példányokat. Ez a hello hosszú ideig szabványos magas rendelkezésre állású mód, amelyet SAP is támogat. Azure nem támogatja a megosztott tárolót, mert az SQL Server magas rendelkezésre állású konfiguráció a megosztott lemez fürtözött konfigurációban nem kell megvalósítani. Számos más magas rendelkezésre állású módszer azonban továbbra is lehetségesek, és ismerteti a következő részekben hello.

#### <a name="sql-server-log-shipping"></a>SQL Server-Naplóküldés
SQL Server-Naplóküldés hello módszerek egyikét a magas rendelkezésre ÁLLÁS. Ha hello virtuális gépek hello magas rendelkezésre ÁLLÁSÚ konfigurációban részt vevő névfeloldás működik, nincs probléma, és hello beállítása az Azure-ban nem különböznek a telepítés, amely a helyszínen történik. A csak az IP-megoldáshoz toorely nem ajánlott. A Naplóküldés és hello alapelvei a Naplóküldés körül toosetting tanúsítványinformációit ellenőrizze a dokumentáció:

<https://docs.microsoft.com/SQL/Database-Engine/log-Shipping/About-log-Shipping-SQL-Server>

Az order tooreally érhető el a magas rendelkezésre állású, egyet kell toodeploy hello vannak ilyen a Naplóküldés konfigurációs toobe belül belül hello azonos Azure rendelkezésre állási csoport virtuális gépekhez.

#### <a name="database-mirroring"></a>Az adatbázis-tükrözés
SAP által támogatott adatbázis-tükrözés (lásd az SAP megjegyzést [965908]) hello SAP kapcsolati karakterláncot a feladatátvételi partner meghatározása támaszkodik. A létesítmények közötti hello esetben azt feltételezzük, hogy adott hello két virtuális gépek vannak a hello ugyanabban a tartományban és hello felhasználói környezetben hello runbookpéldányok futása, valamint egy tartományi felhasználói két SQL Server és a jogosultsága az érintett hello két SQL Server-példányokat. Ezért hello beállítása az Azure-adatbázis-tükrözés nem eltérő egy tipikus helyszíni telepítés vagy a konfigurációs.

Csak felhőalapú telepítések hello legegyszerűbb módszer toohave egy másik tartomány beállítása az Azure toohave ezen az adatbázis-kezelő virtuális gépek (és ideális esetben dedikált SAP virtuális gépek) egy tartományon belül.

Ha egy tartomány nem lehetséges, egyik is használható tanúsítványok hello adatbázis-tükrözési végpontot, itt: <https://docs.microsoft.com/sql/database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql>

Egy adatbázis-tükrözés az Azure-ban mentése oktatóanyag tooset itt található: <https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server> 

#### <a name="sql-server-always-on"></a>SQL Server Always On
Always On támogatja a helyszíni SAP (lásd az SAP megjegyzést [1772688]), támogatott toobe együtt használja, a SAP, az Azure-ban. hello tényt, hogy nem áll az Azure-ban képes toocreate megosztott lemezek nem jelenti azt, hogy egy nem hozható létre különböző virtuális gépek közötti mindig a Windows Server feladatátvételi fürt (WSFC) konfigurálása során. Ez csak azt jelenti, hogy nem hello lehetőségét toouse egy megosztott lemez, a kvórum hello fürtkonfiguráció. Ezért egy WSFC mindig a konfiguráció az Azure-ban, és nem egyszerűen válassza hello kvórum típus, amelyik a megosztott lemez. hello környezetet Azure virtuális gépek vannak telepítve, a virtuális gépek hello neve és hello virtuális gépeken belül hello oldja fel ugyanabban a tartományban. Ez igaz csak Azure-vagy létesítmények közötti telepítések esetén. Néhány szempontot körül hello SQL Server rendelkezésre állási csoport figyelőjét (nem toobe hello Azure rendelkezésre állási csoport összetéveszthető) telepítése óta Azure ezen a ponton a időben nem engedélyezi a toosimply AD/DNS-objektum létrehozása, mivel előfordulhat a helyszíni. Ezért néhány különböző telepítési lépésekre szükséges tooovercome hello adott viselkedését Azure.

Azt is számításba kell egy rendelkezésre állási csoport figyelőjének segítségével a következők:

* Egy rendelkezésre állási csoport figyelőjének használata csak akkor lehetséges a Windows Server 2012 vagy újabb vendég operációs rendszer a virtuális gép hello. Windows Server 2012 van szüksége arról, hogy ez a javítókészlet alkalmaz az toomake: <https://support.microsoft.com/kb/2854082> 
* A Windows Server 2008 R2 a javítás nem létezik, és a mindig bekapcsolva kell toobe használt hello azonos módon az adatbázis-tükrözés hello kapcsolati karakterlánc a feladatátvételi partner megadásával (a SAP default.pfl paraméter adatbázisok/mss/server hello – SAP Megjegyzés lásd: [965908]).
* Ha az egy rendelkezésre állási csoport figyelőjével, hello adatbázis virtuális gépek használatával csatlakoztatott toobe tooa dedikált terheléselosztóhoz. Nevek feloldása csak felhőalapú telepítések esetén, vagy igényel minden virtuális gép, egy SAP hello ugyanazt a virtuális hálózatot, vagy egy SAP alkalmazás réteg hello karbantartás hello etc\host fájl a igényelnének rendszer (alkalmazás-kiszolgálókat, adatbázis-kezelő kiszolgáló és (A) SCS kiszolgáló) rendelés tooget hello virtuális gépek nevénél hello SQL Server virtuális gépek feloldva. A sorrend tooavoid, hogy Azure van-e az esetekben, amikor mindkét VM mellékesen leállítási új IP-címek hozzárendelése egy rendeljen statikus IP-címek toohello hálózati illesztők hello mindig a konfigurációban (meghatározása a statikus IP-címnek a virtuális gépek [ez] [ virtual-networks-reserved-private-ip] cikk)

[comment]: <> (Régi blogok)
[comment]: <> (< https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx>, < https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Különleges lépésekre megadása kötelező, ha a hozzárendelt hello WSFC fürtkonfiguráció amikor hello fürt kell egy különös IP-cím létrehozása, mert az aktuális funkciójú Azure rendelne hello fürtnév hello hello csomópont hello fürtként IP-cím jön létre. Ez azt jelenti, hogy egy manuális lépés kell elvégezni tooassign egy másik IP-cím toohello fürthöz.
* rendelkezésre állási csoport figyelőjének hello TCP/IP-végpontok, hozzárendelt hello elsődleges és másodlagos replikák rendelkezésre állási csoport hello futó toohello virtuális gépeket az Azure-ban létrehozott toobe lesz.
* Előfordulhat, a szükséges toosecure ezeket a végpontokat a hozzáférés-vezérlési listák.

[comment]: <> (Teendők régi blog)
[comment]: <> (részletes utasítások hello és szükségleti az AlwaysOn-konfigurációjának telepítése Azure-cikkek legjobb észlelt, amikor útmutató alapján hello oktatóanyag elérhető [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[comment]: <> (Előre konfigurált AlwaysOn beállításai hello Azure katalógusában keresztül < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[comment]: <> (Legjobb ismertetett [this][virtual-machines-windows-classic-ps-sql-int-listener] az oktatóanyagban egy rendelkezésre állási csoport figyelőjének létrehozása történik)
[comment]: <> (A hozzáférés-vezérlési listák biztonságossá tétele hálózati végpont magyarázatát legjobb itt:)
[comment]: <> (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[comment]: <> (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Lehetséges toodeploy egy SQL Server mindig a rendelkezésre állási csoport különböző Azure-régiókat, valamint felett. Ez a szolgáltatás kihasználja a hello Azure VNet – Vnet-kapcsolatot ([további részleteket][virtual-networks-configure-vnet-to-vnet-connection]).

[comment]: <> (Teendők régi blog)
[comment]: <> (Ilyen esetben az SQL Server AlwaysOn rendelkezésre állási csoportok hello beállítása az alábbiakban ismertetjük: < https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Az SQL Server magas rendelkezésre állás az Azure-ban összegzése
A készenléti lemezkép megadott hello tényt, hogy az Azure Storage védi az hello tartalmat, nincs egy kisebb OK tooinsist. Ez azt jelenti, hogy a magas rendelkezésre állású forgatókönyv kell tooonly elleni, a következő esetekben hello:

* Hello VM elérhetetlensége miatt toomaintenance hello kiszolgálófürtön az Azure-ban vagy más meggondolásból egész
* SQL Server-példányban hello szoftverekkel kapcsolatos problémák
* Ha adatok törlése és időpontban helyreállítási szükséges manuális hiba védelme

Megtekint egy is is, hogy hello első két esetben elegendő az adatbázis-tükrözés vagy mindig bekapcsolva, mivel hello harmadik esetben csak elegendő Naplóküldés egyező technológiák.

Toobalance van szüksége a mindig bekapcsolva, összehasonlított tooDatabase összetettebb telepítő hello hello előnyei mindig meg a tükrözést. Ezek előnyeit is listázva lehet, például:

* Olvasható másodlagos másodpéldányokra.
* Biztonsági másolatok a másodlagos replikákon.
* Jobb méretezhetőséget.
* Több mint egy másodlagos replikákon.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Az SQL Server általános az SAP, az Azure összegzése
Az útmutatóban számos javaslatok és ajánlott elolvasni, több alkalommal az Azure telepítésének tervezése előtt. Általában azonban lehet, hogy toofollow hello felső tíz általános adatbázis-kezelő Azure adott pontokon:

[comment]: <> (2.3 a nagyobb átviteli sebesség mint mi? Mint egy virtuális merevlemez?)
1. Hello legújabb DBMS kiadását, például az SQL Server 2014, az Azure-ban a legtöbb előnnyel rendelkezik a hello használja. Az SQL Server Ez az SQL Server 2012 SP1 CU4, többek között az Azure Storage találkoznak biztonsági hello szolgáltatást szeretné. Azonban SAP együtt azt javasoljuk, legalább SQL Server 2014 SP1 CU1 vagy SQL Server 2012 SP2 és hello legújabb CU.
2. Tervezze meg gondosan az SAP rendszer fekvő Azure toobalance hello adatok fájlelrendezés és Azure korlátozások:
   * Túl sok nem rendelkezik, de elég érhető el a szükséges IOPS tooensure rendelkezik.
   * Felügyelt lemezek használatakor nem, ne feledje, hogy IOPS is korlátozottak egy Azure Storage-fiókot és a Storage-fiókok belül minden Azure-előfizetésre korlátozódnak ([további részleteket][azure-subscription-service-limits]). 
   * Csak paritásos helyét a lemezeken, ha egy magasabb átviteli tooachieve van szüksége.
3. Ne telepítse a szoftvert, vagy a D:\ meghajtóra hello adatmegőrzési nem állandó, és semmit, a meghajtó kapcsolat megszakad, a Windows újraindítását igénylő fájlokra.
4. Ne használjon Azure standard szintű tárolást gyorsítótárazása lemezen.
5. Ne használjon georeplikált Storage-fiókok Azure.  Adatbázis-kezelő munkaterhelések esetén használja a helyileg redundáns.
6. A DBMS gyártója által biztosított elérhető HA/DR megoldás tooreplicate adatbázis adatok felhasználásával.
7. Mindig névfeloldás használata, akkor az nem IP-címeket.
8. Hello legmagasabb adatbázis tömörítés használata lehetséges. Az SQL Server Ez az oldal tömörítést.
9. Ügyeljen arra, hogy az SQL Server-lemezképek hello Azure Piactérről származó használatával. Ha egy SQL Server hello használatához módosítania kell a hello példány rendezési bármely SAP NetWeaver rendszer telepítését megelőzően.
10. Telepítése és konfigurálása az Azure-figyelési állomás SAP hello leírtak [telepítési útmutató][deployment-guide].

## <a name="specifics-toosap-ase-on-windows"></a>Rögzítésen tooSAP ASE Windows rendszeren
A Microsoft Azure-től kezdődően egyszerűen áttelepítheti a meglévő SAP ASE alkalmazások tooAzure virtuális gépek. SAP ASE virtuális gépen lehetővé teszi, hogy Ön tooreduce hello teljes tulajdonosi költség, a központi telepítés, a felügyeleti és a vállalati körének alkalmazások karbantartása ezen alkalmazások tooMicrosoft Azure könnyen áttelepítésével. SAP mértékéig egy Azure virtuális gépen, rendszergazdák és fejlesztők számára továbbra is használhatja hello helyszíni fejlesztési és a felügyeleti eszközök érhetők el.

Nincs a szolgáltatásiszint-szerződésben garantált a hello Azure virtuális gépek, amely itt található: <https://azure.microsoft.com/support/legal/sla/virtual-machines>

Folyamatban, abban, hogy a Microsoft Azure futtatott virtuális gépek jól hajtja végre az összehasonlítási tooother nyilvános felhő virtualizálási ajánlatokat, de az egyes eredmények változhat. Méretezéssel hello különböző SAP hitelesített VM termékváltozatok külön SAP Megjegyzés megadott számú SAP SAP [1928533].

Kimutatások és javaslatokat a legutóbb toohello használat Azure Storage, SAP virtuális gépek telepítési vagy SAP-figyelés alkalmazása toodeployments SAP hajlamosnak SAP-alkalmazásokból teljes hello megadott első négy fejezeteknek: a jelen dokumentum együtt.

### <a name="sap-ase-version-support"></a>SAP ASE verzióinak támogatása
SAP jelenleg támogatja SAP ASE verzió 16,0 SAP Business Suite való használatára. Minden frissítés, a SAP ASE kiszolgálóhoz vagy a használt termékek szolgáltatáson keresztül kizárólag SAP Business Suite JDBC és az ODBC-illesztőprogramok toobe SAP szolgáltatás piactér következő hello: <https://support.sap.com/swdc>.

Telepítések esetében a helyszíni, ne töltse le frissítéseket hello SAP ASE kiszolgálón, vagy hello JDBC és ODBC-illesztőprogramok közvetlenül a Sybase webhelyek. Javítások, amely SAP Business Suite termékek helyszíni használatát támogatja, és Azure virtuális gépek, tekintse meg a következő SAP megjegyzések hello részletes tájékoztatást:

* [1590719]
* [1973241]

Általános információk az SAP Business Suite futó SAP ASE hello található [Állapotváltozás](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP ASE beállítási útmutatója SAP kapcsolatos SAP ASE telepítések az Azure virtuális gépeken
#### <a name="structure-of-hello-sap-ase-deployment"></a>Hello SAP ASE telepítési szerkezete
Megfelelően hello általános leírása, SAP ASE végrehajtható fájlokat kell helyét, vagy telepíteni hello rendszermeghajtóján hello virtuális gép operációsrendszer-lemez (c: meghajtó\). Általában a legtöbb hello SAP ASE rendszer és az eszközök adatbázisok nem valóban használja merevlemez SAP NetWeaver munkaterhelés szerint. Ezért a rendszer hello, és eszközök adatbázisok (master, model, saptools, sybmgmtdb, sybsystemdb) hello C:\ meghajtón is maradhat. 

Kivétel lehet hello tartalmazó összes munkahelyi táblák és az ideiglenes táblák SAP ASE, amely néhány SAP ERP-szolgáltatásokat és minden BW munkaterhelések esetén lehet szükség, vagy magasabb adatok vagy i/o műveletek kötet, amely nem fér el az eredeti hello által létrehozott ideiglenes adatbázis A virtuális gép operációsrendszer-lemez (c: meghajtó\).

Attól függően, hogy hello tooinstall hello rendszer használt SAPInst/SWPM verziója, hello adatbázis tartalmazhat:

* Egy egyetlen SAP ASE tempdb, amely SAP ASE telepítésekor jön létre
* Egy SAP ASE tempdb hozta létre SAP ASE és egy SAP telepítési rutin hello által létrehozott további saptempdb telepítése
* Egy SAP ASE és egy további tempdb manuálisan lett létrehozva telepítése által létrehozott SAP ASE tempdb (például a következő SAP Megjegyzés [1752266]) toomeet ERP/BW adott tempdb követelmények

Adott ERP-szolgáltatásokat vagy az összes BW munkaterhelések esetén az így, figyelembe véve tooperformance tookeep hello tempdb eszközei hello továbbá létrehozott egy másik C:\ meghajtóra tempdb (SWPM, vagy manuálisan). Ha nincsenek további tempdb, ajánlott egy toocreate (SAP Megjegyzés [1752266]).

Az ilyen rendszerek hello az alábbi lépéseket a tempdb továbbá létrehozott hello kell végrehajtani:

* Hello első tempdb eszköz toohello első eszköz hello SAP-adatbázis áthelyezése
* Adja hozzá a tempdb eszközök tooeach hello egy eszközhöz hello SAP-adatbázist tartalmazó virtuális merevlemezeket

A konfiguráció lehetővé teszi, hogy a tempdb tooeither képes tooprovide hello rendszermeghajtó állónál nagyobb területet használ. Referenciaként egy hello tempdb eszköz méretek meglévő operációs rendszert, a helyi ellenőrizheti. Vagy ilyen konfiguráció lehetővé tenné IOPS számok tempdb, amely nem adható meg a rendszermeghajtó hello ellen. A helyi rendszert futtató rendszerek újra használt toomonitor i/o-munkaterhelés tempdb ellen is lehet.

Soha ne helyezze el azokat az eszközöket, SAP ASE alakzatot hello hello VM a D:\ meghajtóra. Ez a is vonatkozik toohello tempdb, akkor is, ha hello tempdb tartott hello objektumok csak átmeneti.

#### <a name="impact-of-database-compression"></a>Adatbázis-tömörítés hatása
A konfigurációk, ahol a i/o-sávszélesség korlátozó tényezővé válhat minden mértékcsoport, ami csökkenti az IOPS toostretch hello munkaterhelés egy futtathatók az IaaS-forgatókönyveknél, például Azure segítséget. Ezért erősen ajánlott toomake arra, hogy SAP ASE tömörítési használja-e egy meglévő SAP adatbázis tooAzure feltöltés előtt.

hello ajánlás tooperform tömörítési feltöltés tooAzure, ha nincs megvalósítva előtt megadott kívül számos oka van:

* hello adatok feltöltése toobe tooAzure mérete alacsonyabb
* hello tömörítési végrehajtási hello időtartama rövidebb, feltéve, hogy az egyik használható erősebb hardver, további processzorok vagy magasabb i/o-sávszélesség legfeljebb I/O várakozási ideje a helyszíni
* Adatbázis kisebb méretű vezethet a lemezfoglaláshoz tooless költségek

A LOB - és adattömörítés Azure virtuális gépeken üzemeltetett, mint a helyszíni virtuális gép működik. Hogyan toocheck tömörítési már használja egy meglévő SAP ASE adatbázis, ha ellenőrizni SAP Megjegyzés vonatkozó részletes információért [1750510].

#### <a name="using-dbacockpit-toomonitor-database-instances"></a>DBACockpit toomonitor adatbázis-példány használata
SAP rendszerek, amelyek használják az SAP ASE adatbázis platformként, hello DBACockpit érhető el a következő tranzakcióban DBACockpit beágyazott böngészőablakot vagy Webdynpro. Azonban a figyelés összes funkcióját hello és felügyelete hello adatbázis hello Webdynpro végrehajtásának hello DBACockpit érhető el.

Mint a helyszíni rendszerek számos lépést szükséges tooenable hello Webdynpro végrehajtásának hello DBACockpit által használt összes SAP NetWeaver funkciót. Hajtsa végre az SAP Megjegyzés [1245200] tooenable hello webdynpros használatát, és létre hello szükséges azokat. Ha újabb megjegyzések hello hello utasításait követve, konfigurálnia is hello internetes kommunikáció kezelése (icm) hello portok toobe http és https-kapcsolatokhoz használt együtt. hello alapértelmezés szerint http így néz ki:

> ICM/server_port_0 = PROT HTTP, PORT = 8000, PROCTIMEOUT = 600, IDŐTÚLLÉPÉS = = 600
> 
> ICM/server_port_1 = PROT HTTPS, PORT = 443$ $, PROCTIMEOUT = 600, IDŐTÚLLÉPÉS = = 600
> 
> 

és a tranzakció DBACockpit hozott létre hello hivatkozások hasonló toothis fog kinézni:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Attól függően, ha, és hogyan hello Azure virtuálisgép üzemeltetési hello SAP rendszer-webhelyek, keresztül csatlakozik többhelyes vagy expressroute-ot (a létesítmények közötti megvalósítási forma) szüksége toomake meg arról, hogy adott ICM használnak egy teljesen minősített állomásnév, amely feloldható a hello a gép tooopen megkísérlése DBACockpit a hello. Lásd az SAP megjegyzést [773830] hogyan határozza meg a ICM toounderstand hello teljesen minősített állomásnév attól függően, hogy a profil paraméterek és a set paraméter icm/host_name_full explicit módon ha szükséges.

Ha egy kizárólag felhőalapú forgatókönyv nélkül közötti kapcsolatot nyújthassanak a helyszíni és az Azure közötti VM hello telepítette, meg kell toodefine egy nyilvános IP-címet és egy domainlabel. nyilvános DNS-neve hello hello VM néz hello formátuma:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

További részletekért kapcsolódó toohello DNS-név található [Itt][virtual-machines-azurerm-versus-azuresm].

Beállítás hello SAP profil paraméter icm/host_name_full toohello DNS-név hello Azure virtuális gép hello hivatkozás hasonlóan néznének ki:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Ebben az esetben szüksége toomake arra, hogy:

* Bejövő szabályok toohello hálózati biztonsági csoport hozzáadása a hello hello TCP/IP-használt portok toocommunicate ICM az Azure-portálon
* Bejövő szabályok toohello Windows tűzfal-konfiguráció hello TCP/IP használt portok toocommunicate a hello ICM hozzáadása

A rendelkezésre álló összes javításokat egy automatizált importált, ajánlott tooperiodically alkalmazása hello javítási gyűjtemény SAP Megjegyzés alkalmazható tooyour SAP verzió:

* [1558958]
* [1619967]
* [1882376]

SAP ASE DBA Vezérlőpult vonatkozó további információ található a következő SAP megjegyzések hello:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP ASE biztonsági mentés/helyreállítás szempontjai
Az Azure SAP ASE telepítésekor meg kell vizsgálni a biztonsági mentési módszer. Akkor is, ha a rendszer hello rendszer nem hatékony, SAP ASE által üzemeltetett hello SAP-adatbázist kell készül biztonsági másolat rendszeres időközönként. Azure Storage három képek tartja, mert a biztonsági mentés már kevésbé fontosak a tekintetben toocompensating tárolási crash. hello elsődleges fenntartása a megfelelő biztonsági mentési és helyreállítási tervek oka, hogy több is kompenzálják a logikai kézi/hibákat idő helyreállítási funkciók pont megadásával. Így hello célja tooeither biztonsági mentések toorestore hello adatbázis használata vissza bizonyos tooa pont idő vagy toouse hello a biztonsági másolatok Azure tooseed egy másik rendszer hello meglévő adatbázis másolása. Például, hogy nem átvinni egy 2 szintű SAP konfigurációs tooa 3 szintű rendszer beállítása hello azonos rendszer biztonsági mentésének visszaállításával.

Biztonsági mentése és visszaállítása egy Azure works adatbázis hello azonos módon ugyanúgy, mint a helyszíni. Tekintse meg az SAP megjegyzéseket:

* [1588316]
* [1585981]

biztonsági másolat létrehozása konfigurációk és ütemezési biztonsági mentések leírását. Attól függően, hogy a stratégia és a vonatkozó igényeket is beállíthat adatbázishoz és naplófájlokhoz memóriaképek toodisk alakzatot hello lemezek közül, vagy vegyen fel egy további lemezt hello biztonsági mentés. adatvesztés esetén hiba tooreduce hello veszélye, akkor ajánlott toouse egy lemezt, ahol nincs adatbázis eszköz.

Adat - és LOB mellett tömörítési SAP ASE is biztosít, a biztonsági másolatok tömörítését. kevesebb lemezterület hello adatbázis és a napló toooccupy kiírja az ajánlott toouse a biztonsági másolatok tömörítését. További információkért lásd: a SAP Megjegyzés [1588316]. Adatok tömörítésével hello biztonsági mentés egyben kritikus fontosságú tooreduce hello adatok toobe továbbítható, ha azt tervezi, toodownload biztonsági mentések vagy Azure virtuális gép tooon helyszíni hello a biztonsági mentési memóriaképek tartalmazó virtuális merevlemezeket.

Ne használja a D:\ meghajtóra adatbázist vagy naplófájlokat memóriakép célként.

#### <a name="performance-considerations-for-backupsrestores"></a>A biztonsági mentés/visszaállítás teljesítménnyel kapcsolatos szempontok
Operációs rendszer nélküli telepítések, mint a biztonsági mentés/visszaállítás teljesítménye minden függ hány kötetek párhuzamosan olvasható, és előfordulhat, hogy milyen átviteli sebességgel hello a köteteket. Ezenkívül hello a biztonsági másolatok tömörítését által használt CPU-felhasználás előfordulhat, hogy fontos szerepet játszik a virtuális gépek csak másolatot tooeight CPU szálak. Ezért egy veheti fel:

* hello a használt lemezeket toostore száma kevesebb hello hello adatbázis eszközök, kisebb hello hello a teljes átviteli sebesség mértéke a olvasása
* hello VM CPU szálainak száma kisebb hello hello, hello súlyosabb hatását a biztonsági másolatok tömörítését hello
* hello kevesebb célok (paritásos könyvtárak, lemezek) toowrite hello történő biztonsági mentés, kisebb hello átviteli hello

célok toowrite toothere tooincrease hello száma két módon, amely lehet igényeitől függően használt/kombinált:

* Szétosztott biztonsági mentési célkötet hello keresztül rendelés tooimprove hello IOPS átviteli sebességének adott csíkozott köteten több csatlakoztatott lemezek
* A biztonsági másolat beállításainak létrehozása SAP ASE szinten, használó egynél több cél directory toowrite hello határozza meg

Egy köteten több csatlakoztatott lemezek keresztül szétosztott esik szó című szakaszában talál. Több könyvtárak használatával hello SAP ASE memóriakép konfigurációban további információkért tekintse meg a toohello dokumentációját a tárolt eljárás sp_config_dump, amely használt toocreate hello memóriakép konfigurációjának hello [Sybase az Adatközpont](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Vészhelyreállítás Azure virtuális gépek
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Adatreplikálás SAP Sybase replikációs kiszolgálóval
A hello SAP Sybase replikációs Server SRS-SAP ASE biztosít meleg készenléti megoldás tootransfer adatbázis tranzakciók tooa távoli helyre aszinkron módon történik. 

hello telepítési és SRS működik, valamint gyakorlatilag egy virtuális Gépet, mint a helyszíni Azure virtuálisgép-szolgáltatásokban üzemeltetett működését.

Az egy későbbi kiadásban tervezett ASE HADR SAP replikációs kiszolgálón keresztül. Akkor lesz tesztelése szabaddá tehető, és a Microsoft Azure platformon, amint érhető el.

## <a name="specifics-toosap-ase-on-linux"></a>Rögzítésen tooSAP ASE Linux rendszeren
A Microsoft Azure-től kezdődően egyszerűen áttelepítheti a meglévő SAP ASE alkalmazások tooAzure virtuális gépek. SAP ASE virtuális gépen lehetővé teszi, hogy Ön tooreduce hello teljes tulajdonosi költség, a központi telepítés, a felügyeleti és a vállalati körének alkalmazások karbantartása ezen alkalmazások tooMicrosoft Azure könnyen áttelepítésével. SAP mértékéig egy Azure virtuális gépen, rendszergazdák és fejlesztők számára továbbra is használhatja hello helyszíni fejlesztési és a felügyeleti eszközök érhetők el.

Üzembe helyezéséhez az Azure virtuális gépek fennállt fontos tooknow hello hivatalos SLA-k, amely itt található: <https://azure.microsoft.com/support/legal/sla>

SAP méretezési információkat és a virtuális gép termékváltozatok hitelesített SAP listája megtalálható SAP Megjegyzés [1928533]. Méretezéssel dokumentumokat az Azure virtuális gépek itt található további SAP <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> és itt <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Kimutatások és javaslatokat a legutóbb toohello használat Azure Storage, SAP virtuális gépek telepítési vagy SAP-figyelés alkalmazása toodeployments SAP hajlamosnak SAP-alkalmazásokból teljes hello megadott első négy fejezeteknek: a jelen dokumentum együtt.

hello alábbi két SAP megjegyzések általános információkat tartalmaznak ASE Linux-és ASE a felhő hello:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP ASE verzióinak támogatása
SAP jelenleg támogatja SAP ASE verzió 16,0 SAP Business Suite való használatára. Minden frissítés, a SAP ASE kiszolgálóhoz vagy a használt termékek szolgáltatáson keresztül kizárólag SAP Business Suite JDBC és az ODBC-illesztőprogramok toobe SAP szolgáltatás piactér következő hello: <https://support.sap.com/swdc>.

Telepítések esetében a helyszíni, ne töltse le frissítéseket hello SAP ASE kiszolgálón, vagy hello JDBC és ODBC-illesztőprogramok közvetlenül a Sybase webhelyek. Javítások, amely SAP Business Suite termékek helyszíni használatát támogatja, és Azure virtuális gépek, tekintse meg a következő SAP megjegyzések hello részletes tájékoztatást:

* [1590719]
* [1973241]

Általános információk az SAP Business Suite futó SAP ASE hello található [Állapotváltozás](https://www.sap.com/community/topic/ase.html)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP ASE beállítási útmutatója SAP kapcsolatos SAP ASE telepítések az Azure virtuális gépeken
#### <a name="structure-of-hello-sap-ase-deployment"></a>Hello SAP ASE telepítési szerkezete
Hello általános leírása, megfelelően SAP ASE végrehajtható legyen található vagy rendszerbe hello legfelső szintű fájl a virtuális gép (/sybase) hello telepítve. Általában a legtöbb hello SAP ASE rendszer és az eszközök adatbázisok nem valóban használja merevlemez SAP NetWeaver munkaterhelés szerint. Ezért a rendszer hello, és eszközök adatbázisok (master, model, saptools, sybmgmtdb, sybsystemdb) hello legfelső szintű fájlrendszerben is maradhat. 

Kivétel lehet hello tartalmazó összes munkahelyi táblák és az ideiglenes táblák SAP ASE, igénylő néhány SAP ERP-szolgáltatásokat és minden BW munkaterhelések esetén előfordulhat, hogy nagyobb adatmennyiség vagy az i/o-műveletek, kötet, amely nem fér el az eredeti hello által létrehozott ideiglenes adatbázis A virtuális gép operációsrendszer-lemezt.

Attól függően, hogy hello tooinstall hello rendszer használt SAPInst/SWPM verziója, hello adatbázis tartalmazhat:

* Egy egyetlen SAP ASE tempdb, amely SAP ASE telepítésekor jön létre
* Egy SAP ASE tempdb hozta létre SAP ASE és egy SAP telepítési rutin hello által létrehozott további saptempdb telepítése
* Egy SAP ASE és egy további tempdb manuálisan lett létrehozva telepítése által létrehozott SAP ASE tempdb (például a következő SAP Megjegyzés [1752266]) toomeet ERP/BW adott tempdb követelmények

Adott ERP-szolgáltatásokat vagy az összes BW munkaterhelések esetén az így, legutóbb tooperformance, továbbá létrehozott hello tempdb (SWPM, vagy manuálisan) külön fájlrendszeren, amely egyetlen Azure adatlemezt vagy Linux RAID jelölhető tookeep hello tempdb eszközei az Azure data több lemezre kiterjedő. Ha nincsenek további tempdb, ajánlott egy toocreate (SAP Megjegyzés [1752266]).

Az ilyen rendszerek hello az alábbi lépéseket a tempdb továbbá létrehozott hello kell végrehajtani:

* Hello első tempdb directory toohello első fájlrendszer hello SAP-adatbázis áthelyezése
* A tempdb könyvtárak tooeach hello SAP adatbázis fájlrendszer tartalmazó hello lemezek hozzáadása

A konfiguráció lehetővé teszi, hogy a tempdb tooeither képes tooprovide hello rendszermeghajtó állónál nagyobb területet használ. Referenciaként egy hello tempdb directory méretek meglévő operációs rendszert, a helyi ellenőrizheti. Vagy ilyen konfiguráció lehetővé tenné IOPS számok tempdb, amely nem adható meg a rendszermeghajtó hello ellen. A helyi rendszert futtató rendszerek újra használt toomonitor i/o-munkaterhelés tempdb ellen is lehet.

Soha ne helyezze el SAP ASE könyvtárak /mnt vagy a virtuális gép hello /mnt/resource. Ez a is vonatkozik toohello tempdb, akkor is, ha hello tempdb tartott hello objektumok csak ideiglenes /mnt vagy /mnt/resource nem az alapértelmezett Azure virtuális gép ideiglenes adhatja, amely nem állandó. Hello Azure virtuális gép ideiglenes terület kapcsolatos további részletek találhatók [Ez a cikk][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Adatbázis-tömörítés hatása
A konfigurációk, ahol a i/o-sávszélesség korlátozó tényezővé válhat minden mértékcsoport, ami csökkenti az IOPS toostretch hello munkaterhelés egy futtathatók az IaaS-forgatókönyveknél, például Azure segítséget. Ezért erősen ajánlott toomake arra, hogy SAP ASE tömörítési használja-e egy meglévő SAP adatbázis tooAzure feltöltés előtt.

hello ajánlás tooperform tömörítési feltöltés tooAzure, ha nincs megvalósítva előtt megadott kívül számos oka van:

* hello adatok feltöltése toobe tooAzure mérete alacsonyabb
* hello tömörítési végrehajtási hello időtartama rövidebb, feltéve, hogy az egyik használható erősebb hardver, további processzorok vagy magasabb i/o-sávszélesség legfeljebb I/O várakozási ideje a helyszíni
* Adatbázis kisebb méretű vezethet a lemezfoglaláshoz tooless költségek

A LOB - és adattömörítés Azure virtuális gépeken üzemeltetett, mint a helyszíni virtuális gép működik. Hogyan toocheck tömörítési már használja egy meglévő SAP ASE adatbázis, ha ellenőrizni SAP Megjegyzés vonatkozó részletes információért [1750510]. Adatbázis tömörítési kapcsolatos további információkért lásd: SAP Megjegyzés [2121797].

#### <a name="using-dbacockpit-toomonitor-database-instances"></a>DBACockpit toomonitor adatbázis-példány használata
SAP rendszerek, amelyek használják az SAP ASE adatbázis platformként, hello DBACockpit érhető el a következő tranzakcióban DBACockpit beágyazott böngészőablakot vagy Webdynpro. Azonban a figyelés összes funkcióját hello és felügyelete hello adatbázis hello Webdynpro végrehajtásának hello DBACockpit érhető el.

Mint a helyszíni rendszerek számos lépést szükséges tooenable hello Webdynpro végrehajtásának hello DBACockpit által használt összes SAP NetWeaver funkciót. Hajtsa végre az SAP Megjegyzés [1245200] tooenable hello webdynpros használatát, és létre hello szükséges azokat. Ha újabb megjegyzések hello hello utasításait követve, konfigurálnia is hello internetes kommunikáció kezelése (icm) hello portok toobe http és https-kapcsolatokhoz használt együtt. hello alapértelmezés szerint http így néz ki:

> ICM/server_port_0 = PROT HTTP, PORT = 8000, PROCTIMEOUT = 600, IDŐTÚLLÉPÉS = = 600
> 
> ICM/server_port_1 = PROT HTTPS, PORT = 443$ $, PROCTIMEOUT = 600, IDŐTÚLLÉPÉS = = 600
> 
> 

és a tranzakció DBACockpit hozott létre hello hivatkozások hasonló toothis fog kinézni:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Attól függően, ha, és hogyan hello Azure virtuálisgép üzemeltetési hello SAP rendszer-webhelyek, keresztül csatlakozik többhelyes vagy expressroute-ot (a létesítmények közötti megvalósítási forma) szüksége toomake meg arról, hogy adott ICM használnak egy teljesen minősített állomásnév, amely feloldható a hello a gép tooopen megkísérlése DBACockpit a hello. Lásd az SAP megjegyzést [773830] hogyan határozza meg a ICM toounderstand hello teljesen minősített állomásnév attól függően, hogy a profil paraméterek és a set paraméter icm/host_name_full explicit módon ha szükséges.

Ha egy kizárólag felhőalapú forgatókönyv nélkül közötti kapcsolatot nyújthassanak a helyszíni és az Azure közötti VM hello telepítette, meg kell toodefine egy nyilvános IP-címet és egy domainlabel. nyilvános DNS-neve hello hello VM néz hello formátuma:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com
> 
> 

További részletekért kapcsolódó toohello DNS-név található [Itt][virtual-machines-azurerm-versus-azuresm].

Beállítás hello SAP profil paraméter icm/host_name_full toohello DNS-név hello Azure virtuális gép hello hivatkozás hasonlóan néznének ki:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit
> 
> 

Ebben az esetben szüksége toomake arra, hogy:

* Bejövő szabályok toohello hálózati biztonsági csoport hozzáadása a hello hello TCP/IP-használt portok toocommunicate ICM az Azure-portálon
* Bejövő szabályok toohello Windows tűzfal-konfiguráció hello TCP/IP használt portok toocommunicate a hello ICM hozzáadása

A rendelkezésre álló összes javításokat egy automatizált importált, ajánlott tooperiodically alkalmazása hello javítási gyűjtemény SAP Megjegyzés alkalmazható tooyour SAP verzió:

* [1558958]
* [1619967]
* [1882376]

SAP ASE DBA Vezérlőpult vonatkozó további információ található a következő SAP megjegyzések hello:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496]    
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP ASE biztonsági mentés/helyreállítás szempontjai
Az Azure SAP ASE telepítésekor meg kell vizsgálni a biztonsági mentési módszer. Akkor is, ha a rendszer hello rendszer nem hatékony, SAP ASE által üzemeltetett hello SAP-adatbázist kell készül biztonsági másolat rendszeres időközönként. Azure Storage három képek tartja, mert a biztonsági mentés már kevésbé fontosak a tekintetben toocompensating tárolási crash. hello elsődleges fenntartása a megfelelő biztonsági mentési és helyreállítási tervek oka, hogy több is kompenzálják a logikai kézi/hibákat idő helyreállítási funkciók pont megadásával. Így hello célja tooeither biztonsági mentések toorestore hello adatbázis használata vissza bizonyos tooa pont idő vagy toouse hello a biztonsági másolatok Azure tooseed egy másik rendszer hello meglévő adatbázis másolása. Például, hogy nem átvinni egy 2 szintű SAP konfigurációs tooa 3 szintű rendszer beállítása hello azonos rendszer biztonsági mentésének visszaállításával.

Biztonsági mentése és visszaállítása egy Azure works adatbázis hello azonos módon ugyanúgy, mint a helyszíni. Tekintse meg az SAP megjegyzéseket:

* [1588316]
* [1585981]

biztonsági másolat létrehozása konfigurációk és ütemezési biztonsági mentések leírását. Attól függően, hogy a stratégia és a vonatkozó igényeket is beállíthat adatbázishoz és naplófájlokhoz memóriaképek toodisk alakzatot hello lemezek közül, vagy vegyen fel egy további lemezt hello biztonsági mentés. az adatvesztés esetén ez hiba tooreduce hello veszélye, ajánlott toouse egy lemezt, ahol nincs adatbázis könyvtár vagy fájl is található.

Adat - és LOB mellett tömörítési SAP ASE is biztosít, a biztonsági másolatok tömörítését. kevesebb lemezterület hello adatbázis és a napló toooccupy kiírja az ajánlott toouse a biztonsági másolatok tömörítését. További információkért lásd: a SAP Megjegyzés [1588316]. Adatok tömörítésével hello biztonsági mentés egyben kritikus fontosságú tooreduce hello adatok toobe továbbítható, ha azt tervezi, toodownload biztonsági mentések vagy Azure virtuális gép tooon helyszíni hello a biztonsági mentési memóriaképek tartalmazó virtuális merevlemezeket.

Ne használjon hello Azure virtuális gép ideiglenes terület /mnt vagy /mnt/resource adatbázist vagy naplófájlokat memóriakép célként.

#### <a name="performance-considerations-for-backupsrestores"></a>A biztonsági mentés/visszaállítás teljesítménnyel kapcsolatos szempontok
Operációs rendszer nélküli telepítések, mint a biztonsági mentés/visszaállítás teljesítménye minden függ hány kötetek párhuzamosan olvasható, és előfordulhat, hogy milyen átviteli sebességgel hello a köteteket. Ezenkívül hello a biztonsági másolatok tömörítését által használt CPU-felhasználás előfordulhat, hogy fontos szerepet játszik a virtuális gépek csak másolatot tooeight CPU szálak. Ezért egy veheti fel:

* hello a használt lemezeket toostore száma kevesebb hello hello adatbázis eszközök, kisebb hello hello a teljes átviteli sebesség mértéke a olvasása
* hello VM CPU szálainak száma kisebb hello hello, hello súlyosabb hatását a biztonsági másolatok tömörítését hello
* hello kevesebb célok (Linux szoftveres RAID lemezek) toowrite hello történő biztonsági mentés, kisebb hello átviteli hello

célok toowrite toothere tooincrease hello száma két módon, amely lehet igényeitől függően használt/kombinált:

* Szétosztott biztonsági mentési célkötet hello keresztül rendelés tooimprove hello IOPS átviteli sebességének adott csíkozott köteten több csatlakoztatott lemezek
* A biztonsági másolat beállításainak létrehozása SAP ASE szinten, használó egynél több cél directory toowrite hello határozza meg

Egy köteten több csatlakoztatott lemezek keresztül szétosztott esik szó című szakaszában talál. Több könyvtárak használatával hello SAP ASE memóriakép konfigurációban további információkért tekintse meg a toohello dokumentációját a tárolt eljárás sp_config_dump, amely használt toocreate hello memóriakép konfigurációjának hello [Sybase az Adatközpont](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Vészhelyreállítás Azure virtuális gépek
#### <a name="data-replication-with-sap-sybase-replication-server"></a>Adatreplikálás SAP Sybase replikációs kiszolgálóval
A hello SAP Sybase replikációs Server SRS-SAP ASE biztosít meleg készenléti megoldás tootransfer adatbázis tranzakciók tooa távoli helyre aszinkron módon történik. 

hello telepítési és SRS működik, valamint gyakorlatilag egy virtuális Gépet, mint a helyszíni Azure virtuálisgép-szolgáltatásokban üzemeltetett működését.

NEM támogatott ezen a ponton a időben történő ASE HADR SAP replikációs kiszolgálón keresztül. Előfordulhat, hogy ez tesztelése, és adott ki a Microsoft Azure-platformok jövőbeli hello.

## <a name="specifics-toooracle-database-on-windows"></a>Rögzítésen tooOracle Windows-adatbázisba
Oracle szoftver a Microsoft Windows Hyper-V és az Azure Oracle toorun támogatja. A részleteket a hello és általános támogatja a Windows Hyper-V és az Azure: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Következő hello általános támogatási hello adott forgatókönyv az Oracle-adatbázisok, ami az SAP-alkalmazásokból támogatott is. Részletek megnevezett hello dokumentum részében.

### <a name="oracle-version-support"></a>Oracle által támogatott verzió
Oracle-verziókról és a megfelelő operációsrendszer-verziók, a SAP futó Azure virtuális gépeken Oracle találhatók SAP Megjegyzés: a támogatott [2039619].

Általános információk az SAP Business Suite futó Oracle található 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle beállítási útmutatója SAP-telepítések az Azure virtuális gépeken
#### <a name="storage-configuration"></a>Tároló konfigurálása
Csak egyetlen példány Oracle NTFS formátumú lemezek használata támogatott. Az adatbázisfájlok hello VHD-k vagy kezelt lemezek alapján NTFS fájlrendszert kell tárolni. Ezek a lemezek csatlakoztatott toohello Azure virtuális Gépen, és Azure lap BLOB Storage alapuló (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) vagy felügyelt lemezeket (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Valamilyen hálózati meghajtók vagy távoli megosztások, például az Azure file szolgáltatás:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

vannak **nem** Oracle adatbázis-fájlok támogatott!

A Azure lap BLOB-tároló vagy kezelt lemezek mérete alapján használja, ez a dokumentum fejezetben szereplő hello [gyorsítótárazást a virtuális gépek és az adatlemezek] [ dbms-guide-2.1] és [Microsoft Azure Storage] [ dbms-guide-2.3] toodeployments az Oracle-adatbázishoz hello is vonatkoznak.

Ahogy korábban hello hello dokumentum általános része, az Azure-lemezeket a IOPS átviteli kvótái létezik. hello pontos kvóták hello VM típusától függően használhatók. A kvótával rendelkező VM Típuslista található [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows].

tooidentify hello támogatott Azure Virtuálisgép-típusokon, tekintse meg a tooSAP Megjegyzés [1928533].

Mindaddig, amíg hello aktuális IOPS kvóta lemezenként hello követelményének eleget tesz, akkor lehetséges toostore összes hello DB fájlok egy egyetlen csatlakoztatott lemezen. 

Ha magasabb iops-értéket szükség, erősen ajánlott toouse ablak Tárolókészletek (csak érhető el a Windows Server 2012 és újabb) vagy a Windows szétosztott a Windows 2008 R2 toocreate egy nagy méretű logikai eszköz több csatlakoztatott lemezek keresztül (lásd még: fejezet [Szoftver RAID] [ dbms-guide-2.2] a jelen dokumentum). Ezt a módszert hello felügyeleti általános toomanage hello lemezterület leegyszerűsíti, és ezzel elkerülheti a hello elérhető toomanually fájlok szét több csatlakoztatott lemez.

#### <a name="backup--restore"></a>Biztonsági mentés / helyreállítás
Biztonsági mentés / visszaállítás funkció, SAP BR hello * eszközök az Oracle által támogatott kell hello azonos módon történik a szabványos Windows Server operációs rendszerek és Hyper-V. Oracle-helyreállítás-kezelő (RMAN) biztonsági mentések toodisk és visszaállítási a lemezről is támogatott.

#### <a name="high-availability"></a>Magas rendelkezésre állás
Oracle Data Guard magas rendelkezésre állású és vész-helyreállítási célokra támogatott. A részletek megtalálhatók [ez] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentációját.

#### <a name="other"></a>Egyéb
Minden más általános például Azure rendelkezésre állási készletek vagy SAP figyelés témakörei első három fejezeteknek: a jelen dokumentum hello Oracle-adatbázishoz, valamint a virtuális gépek központi telepítéseknél hello leírtak szerint.

## <a name="specifics-toooracle-database-on-oracle-linux"></a>Rögzítésen tooOracle adatbázis Oracle Linux
Oracle szoftver a Microsoft Windows Hyper-V és az Azure Oracle toorun támogatja. A részleteket a hello és általános támogatja a Windows Hyper-V és az Azure: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Következő hello általános támogatási hello adott forgatókönyv az Oracle-adatbázisok, ami az SAP-alkalmazásokból támogatott is. Részletek megnevezett hello dokumentum részében.

### <a name="oracle-version-support"></a>Oracle által támogatott verzió
Oracle-verziókról és a megfelelő operációsrendszer-verziók, a SAP futó Azure virtuális gépeken Oracle találhatók SAP Megjegyzés: a támogatott [2039619].

Általános információk az SAP Business Suite futó Oracle található 1DX: <https://www.sap.com/community/topic/oracle.html>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle beállítási útmutatója SAP-telepítések az Azure virtuális gépeken
#### <a name="storage-configuration"></a>Tároló konfigurálása
Csak egyetlen példány használatával ext3 Oracle, ext4 és xfs formázott lemezek támogatott. Minden adatbázisfájlt a rendszerek óráit fájlt a VHD-k vagy kezelt lemezek alapján kell tárolni. Ezek a lemezek csatlakoztatott toohello Azure virtuális Gépen, és Azure lap BLOB Storage alapuló (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) vagy felügyelt lemezeket (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Valamilyen hálózati meghajtók vagy távoli megosztások, például az Azure file szolgáltatás:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

vannak **nem** Oracle adatbázis-fájlok támogatott!

A Azure lap BLOB-tároló vagy kezelt lemezek mérete alapján használja, ez a dokumentum fejezetben szereplő hello [gyorsítótárazást a virtuális gépek és az adatlemezek] [ dbms-guide-2.1] és [Microsoft Azure Storage] [ dbms-guide-2.3] toodeployments az Oracle-adatbázishoz hello is vonatkoznak.

Ahogy korábban hello hello dokumentum általános része, az Azure-lemezeket a IOPS átviteli kvótái létezik. hello pontos kvóták hello VM típusától függően használhatók. A kvótával rendelkező VM Típuslista található [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows].

tooidentify hello támogatott Azure Virtuálisgép-típusokon, tekintse meg a tooSAP Megjegyzés [1928533]

Mindaddig, amíg hello aktuális IOPS kvóta lemezenként hello követelményének eleget tesz, akkor lehetséges toostore összes hello DB fájlok egy egyetlen csatlakoztatott lemezen. 

Ha magasabb iops-értéket szükség, mindenképpen toouse LVM (logikai kötetkezelő) vagy MDADM toocreate egy logikai sok több csatlakoztatott lemezek keresztül. További információ a fejezet [szoftver RAID] [ dbms-guide-2.2] ebben a dokumentumban. Ezt a módszert hello felügyeleti általános toomanage hello lemezterület leegyszerűsíti, és ezzel elkerülheti a hello elérhető toomanually fájlok szét több csatlakoztatott lemez.

#### <a name="backup--restore"></a>Biztonsági mentés / helyreállítás
Biztonsági mentés / visszaállítás funkció, SAP BR hello * eszközök az Oracle által támogatott kell hello azonos módon, az operációs rendszer és a Hyper-V. Oracle-helyreállítás-kezelő (RMAN) biztonsági mentések toodisk és visszaállítási a lemezről is támogatott.

#### <a name="high-availability"></a>Magas rendelkezésre állás
Oracle Data Guard magas rendelkezésre állású és vész-helyreállítási célokra támogatott. A részletek megtalálhatók [ez] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentációját.

#### <a name="other"></a>Egyéb
Minden más általános például Azure rendelkezésre állási készletek vagy SAP figyelés témakörei első három fejezeteknek: a jelen dokumentum hello Oracle-adatbázishoz, valamint a virtuális gépek központi telepítéseknél hello leírtak szerint.

## <a name="specifics-for-hello-sap-maxdb-database-on-windows"></a>Az SAP MaxDB adatbázist a Windows hello rögzítésen
### <a name="sap-maxdb-version-support"></a>SAP MaxDB verzióinak támogatása
SAP jelenleg támogatja SAP MaxDB verzió 7.9 SAP NetWeaver szolgáltatáson alapuló termékek az Azure-ban való használatra. Minden frissítés, a SAP MaxDB kiszolgálóhoz vagy a használt SAP NetWeaver szolgáltatáson alapuló termékek JDBC és az ODBC-illesztőprogramok toobe találhatók kizárólag keresztül hello SAP szolgáltatás piactér következő <https://support.sap.com/swdc>.
SAP NetWeaver futó SAP MaxDB általános tájékoztatást található <https://www.sap.com/community/topic/maxdb.html>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Microsoft Windows-verziók és az Azure virtuális gép típusú támogatott SAP MaxDB adatbázis-kezelő
toofind hello támogatott Microsoft-Windows-verzió SAP MaxDB DBMS Azure, lásd:

* [SAP termék rendelkezésre állási mátrix (PAM)][sap-pam]
* SAP Megjegyzés [1928533]

Lehetőleg toouse hello legújabb verziója hello Microsoft Windows, amely Microsoft Windows 2012 R2 operációs rendszerének.

### <a name="available-sap-maxdb-documentation"></a>Rendelkezésre álló SAP MaxDB dokumentáció
Az SAP vegye figyelembe a következő hello található SAP MaxDB dokumentáció frissítése hello listája [767598 ]

### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP MaxDB beállítási útmutatója SAP-telepítések az Azure virtuális gépeken
#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Tárolás beállítása
Az Azure storage ajánlott eljárások az SAP MaxDB kövesse hello általános javaslatok fejezetben említett [RDBMS üzembe helyezésének struktúra][dbms-guide-2].

> [!IMPORTANT]
> Más adatbázisokhoz, például SAP MaxDB is rendelkeznek, adatokat és a naplófájlokat. Azonban az SAP MaxDB terminológia hello megfelelő kifejezés "volume" (nem "file"). Nincsenek például SAP MaxDB adatok és a naplózási kötetek. Ne tévessze össze ezeket az operációs rendszer lemezkötetek használja. 
> 
> 

Rövid kell:

* Azure Storage-fiókok használatához állítsa be a hello Azure storage-fiók, amely tárolja a hello SAP MaxDB adatainak és naplókönyvtárainak kötetek (azaz fájlok) túl**helyi redundáns tárolás (LRS)** fejezet meghatározott [Microsoft Azure Storage] [dbms-guide-2.3].
* Külön hello IO útvonal SAP MaxDB adatkötetnél (azaz fájlok) a hello IO útvonal naplózási kötetek (azaz fájlok). Ez azt jelenti, hogy SAP MaxDB adatköteteken (azaz fájlok) van telepítve egy logikai meghajtó toobe SAP MaxDB naplózási kötetek (azaz fájlok) van telepítve egy másik logikai meghajtó toobe.
* Hello megfelelő gyorsítótárazási típusú lemezek, attól függően, hogy használ-e az SAP MaxDB adat- vagy naplófájl kötetek (azaz fájlok), és hogy használ-e Azure Standard vagy prémium szintű Azure Storage fejezetben leírtak [gyorsítótárazást a virtuális gépek és adatlemezek][dbms-guide-2.1].
* Mindaddig, amíg hello aktuális IOPS kvóta lemezenként hello követelményének eleget tesz, akkor lehetséges toostore összes hello az adatkötetek egyetlen csatlakoztatott lemez, és is tárolhatja az összes adatbázis naplózási kötetek másik egyetlen csatlakoztatott lemezen.
* Ha további iops-érték és/vagy a hely szükség, mindenképpen toouse Microsoft ablak Tárolókészletek (csak érhető el a Microsoft Windows Server 2012 és újabb) vagy a Microsoft Windows a Microsoft Windows 2008 R2 toocreate szétosztott egy nagy méretű logikai eszköz keresztül több csatlakoztatott lemez. További információ a fejezet [szoftver RAID] [ dbms-guide-2.2] ebben a dokumentumban. Ezt a módszert hello felügyeleti általános toomanage hello lemezterület leegyszerűsíti, és ezzel elkerülheti a hello elérhető manuálisan elosztása több csatlakoztatott lemezre fájlok.
* Hello legmagasabb szintű IOPS követelményeknek is használhatja prémium szintű Azure Storage elérhető a DS-méretek és GS sorozatnak virtuális gépeket.

![Az Azure infrastruktúra-szolgáltatási virtuális gép SAP MaxDB DBMS referencia-konfiguráció][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Biztonsági mentés és helyreállítás
Az Azure SAP MaxDB telepítésekor kell áttekinteni a biztonsági mentési módszer. Akkor is, ha a rendszer hello rendszer nem hatékony, SAP MaxDB által üzemeltetett hello SAP-adatbázist kell készül biztonsági másolat rendszeres időközönként. Azure Storage három képek tartja, mert a biztonsági mentés már kevésbé fontos a rendszer tárolási hiba és a fontosabb felügyeleti vagy működési hibák elleni védelme. hello elsődleges karbantartása, a megfelelő biztonsági mentési és visszaállítási terv oka, hogy is kompenzálják a logikai vagy kézi-e hibákat időpontban helyreállítási képességek biztosításával. Hello célja így tooeither használja biztonsági mentések toorestore hello adatbázis tooa bizonyos pont idő vagy toouse hello a biztonsági másolatok Azure tooseed egy másik system hello meglévő adatbázis másolása. Például, hogy nem átvinni egy 2 szintű SAP konfigurációs tooa 3 szintű rendszer beállítása hello azonos rendszer biztonsági mentésének visszaállításával.

SAP Megjegyzés: a felsorolt biztonsági mentése és visszaállítása egy Azure works hello azonos módon az hajtja végre a helyszíni rendszerekhez, így használhatja a szabványos SAP MaxDB biztonsági mentés/visszaállítás eszközök ismertetett hello SAP MaxDB dokumentáció dokumentumokat adatbázis [767598 ]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>A biztonsági mentés és helyreállítás a teljesítménnyel kapcsolatos szempontok
Operációs rendszer nélküli telepítések, mint a biztonsági mentési és visszaállítási teljesítménye minden hány kötetek párhuzamos és hello sebességét, a köteteket, olvassa el a függő. Emellett a biztonsági másolatok tömörítését által használt CPU-felhasználás hello is fontos szerepet játszik rendelkező virtuális gépeken tooeight CPU szálak fel. Ezért egy veheti fel:

* a használt lemezeket toostore hello adatbázis eszközök száma kevesebb hello hello, hello alacsonyabb hello teljes átviteli sebesség olvasása
* hello VM CPU szálainak száma kisebb hello hello, hello súlyosabb hatását a biztonsági másolatok tömörítését hello
* hello kevesebb célok (paritásos könyvtárak, lemezek) toowrite hello történő biztonsági mentés, hello alacsonyabb hello átviteli sebesség

tooincrease hello száma a toowrite céloz, két lehetőség használható, valószínűleg a kombinálva, igényeitől függően:

* Hogy a biztonsági mentéshez különböző köteteken
* Szétosztott biztonsági mentési célkötet hello keresztül rendelés tooimprove hello IOPS átviteli sebességének adott csíkozott köteten több csatlakoztatott lemezek
* Dedikált logikai lemezen eszközök rendelkeznek:
  * SAP MaxDB biztonsági mentési kötetek (azaz fájlok)
  * SAP MaxDB adatköteteken (azaz fájlok)
  * SAP MaxDB naplózási kötetek (azaz fájlok)

Egy kötet szétosztott keresztül több csatlakoztatott lemez rendelkezik már tárgyalt, a fejezet [szoftver RAID] [ dbms-guide-2.2] ebben a dokumentumban. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Más
Más általános témaköröket, például az Azure rendelkezésre állási készletek vagy SAP figyelési is érvényesek a jelen dokumentum hello SAP MaxDB adatbázis virtuális gépek központi telepítéseknél első három fejezetek hello leírtak szerint.
Más SAP MaxDB-specifikus beállításokat átlátszó tooAzure virtuális gép, és ismerteti a különböző dokumentumok SAP megjegyzés szerepel [767598 ] és ezek a SAP megjegyzések:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Az SAP liveCache Windows rögzítésen
### <a name="sap-livecache-version-support"></a>SAP liveCache verzióinak támogatása
Azure virtuális gépek támogatott SAP liveCache minimális verziója **SAP LC/LCAPPS 10.0 SP 25** beleértve **liveCache 7.9.08.31** és **LCA-Build 25**, az engedélyezett **EhP 2 SAP SCM 7.0** és magasabb.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Microsoft Windows-verziók és az Azure virtuális gép típusú támogatott SAP liveCache adatbázis-kezelő
toofind hello támogatott Microsoft-Windows-verzió SAP liveCache Azure, lásd:

* [SAP termék rendelkezésre állási mátrix (PAM)][sap-pam]
* SAP Megjegyzés [1928533]

Lehetőleg toouse hello legújabb verziója hello Microsoft Windows Server operációs rendszerének. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache beállítási útmutatója SAP-telepítések az Azure virtuális gépeken
#### <a name="recommended-azure-vm-types"></a>Az Azure Virtuálisgép-típusokon ajánlott
SAP liveCache hatalmas számításokat alkalmazás, a memória és CPU hello mennyiségét és sebességét SAP liveCache teljesítményének jelentős hatással van. 

SAP által támogatott hello Azure virtuális gép esetében (SAP Megjegyzés [1928533]), minden virtuális Processzor-erőforrások lefoglalt dedikált fizikai Processzor-erőforrások hello hipervizor által támogatott virtuális gép toohello. Nincs elhelyezésétől (és a Processzor-erőforrások verseny ezért) történik.

Az összes Azure virtuális gép példány által támogatott eszköztípusok SAP, hasonlóan hello Virtuálisgép-memória a 100 %-os leképezve toohello fizikai memória – elhelyezésétől (túlzott foglaltsága), például nem használt.

Az ehhez a perspektívához lehetőleg toouse hello új D sorozatú vagy a DS sorozatnak (prémium szintű Azure Storage kombináltan) Azure VM-típus 60 %-kal gyorsabb processzorok, mint A-sorozatú hello rendelkeznek. Hello legnagyobb RAM Memóriát és CPU terhelést, használhatja a G-sorozat és GS sorozatnak (prémium szintű Azure Storage együtt) a virtuális gépek hello legújabb Intel® Xeon® processzorral rendelkezik kétszer hello memóriáját és négy alkalommal hello tartós állapotú meghajtót (SSD) hello D a E5 v3 termékcsalád / DS-méretek.

#### <a name="storage-configuration"></a>Tárolás beállítása
SAP liveCache SAP MaxDB technológia alapul, mint az összes hello az Azure storage kapcsolatos bevált gyakorlatokat fejezetben SAP MaxDB az említett [tárkonfiguráció] [ dbms-guide-8.4.1] SAP liveCache is érvényesek. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Dedikált Azure virtuális gép liveCache
SAP liveCache intenzíven használ számítási teljesítménnyel rendelkezik, hatékony használatának ajánlott toodeploy egy dedikált Azure virtuális gépen. 

![Azure virtuális gép számára a hatékony használati eset liveCache dedikált][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Biztonsági mentés és visszaállítás
Biztonsági mentés és visszaállítás, beleértve a teljesítménnyel kapcsolatos megfontolások már ismertetett hello vonatkozó SAP MaxDB fejezeteiben [biztonsági mentése és visszaállítása] [ dbms-guide-8.4.2] és [a biztonsági mentéshez a teljesítménnyel kapcsolatos szempontok Visszaállítás és][dbms-guide-8.4.3]. 

#### <a name="other"></a>Egyéb
Általános témaköröket már ismertetett hello vonatkozó SAP MaxDB [ez] [ dbms-guide-8.4.4] fejezet. 

## <a name="specifics-for-hello-sap-content-server-on-windows"></a>Az SAP-kiszolgálót a Windows hello rögzítésen
hello SAP tartalom kiszolgáló-e egy különálló, kiszolgálóalapú összetevő toostore például különböző formátumokban elektronikus dokumentumok. hello SAP tartalomkiszolgáló technológia fejlesztése által biztosított és toobe használt alkalmazások közötti az SAP-alkalmazásokból. Külön rendszeren települ. Tipikus tartalom van betanítása anyagot és Tudásbázis adatraktár vagy a technikai rajzok származó hello mySAP PLM dokumentumkezelő rendszer dokumentációjában talál. 

### <a name="sap-content-server-version-support"></a>SAP kiszolgáló által támogatott verzió
SAP jelenleg támogatja:

* **SAP tartalomkiszolgáló** verziójával **6.50 (vagy újabb)**
* **SAP MaxDB 7.9 verziója**
* **Microsoft IIS (Internet Information Server) 8.0-s (és újabb)**

Lehetőleg toouse hello legújabb verziója SAP tartalom kiszolgáló, amely a jelen dokumentum írása hello időpontjában **6.50-es verzió SP4**, és hello legújabb verziójának **Microsoft IIS 8.5**. 

Ellenőrizze a hello legújabb támogatott verziója a SAP-kiszolgáló és a Microsoft IIS hello [SAP termék rendelkezésre állási mátrix (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Támogatott Microsoft-Windows- és Azure virtuális gép SAP-kiszolgálóhoz
kimenő SAP-kiszolgálóhoz az Azure, a támogatott Windows-verzió toofind lásd:

* [SAP termék rendelkezésre állási mátrix (PAM)][sap-pam]
* SAP Megjegyzés [1928533]

Lehetőleg toouse hello legújabb verzióját a Microsoft Windows Server.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP tartalomkiszolgáló beállítási útmutatója SAP-telepítések az Azure virtuális gépeken
#### <a name="storage-configuration"></a>Tárolás beállítása
SAP tartalomkiszolgáló toostore fájlok hello SAP MaxDB adatbázis konfigurálja, ha minden az Azure storage ajánlott eljárásokat fejezetben SAP MaxDB az említett ajánlás [tárkonfiguráció] [ dbms-guide-8.4.1] is érvényes hello SAP tartalomkiszolgáló forgatókönyvhöz. 

Ha SAP tartalomkiszolgáló toostore fájlok hello fájlrendszer konfigurálja, a rendszer toouse dedikált logikai meghajtó ajánlott. Windows tárolóhelyek használatával lehetővé teszi tooalso növekedése logikai lemez méretét és IOPS átviteli fejezetben leírtak [szoftver RAID][dbms-guide-2.2]. 

#### <a name="sap-content-server-location"></a>SAP tartalomkiszolgáló helye
SAP kiszolgáló van üzembe helyezett hello toobe azonos Azure-régió, és Azure VNET hello SAP rendszer telepítési helyét. A kijelölt Azure virtuális gép vagy a toodeploy SAP tartalom kiszolgáló-összetevők hello hello SAP rendszert futtató ugyanabból virtuális kíván-e szabad toodecide áll. 

![Azure virtuális gép dedikált SAP-kiszolgálóhoz][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP gyorsítótár-kiszolgálón
hello SAP gyorsítótár-kiszolgálót egy további kiszolgálóalapú összetevő tooprovide too(cached) dokumentumokhoz helyileg. hello SAP gyorsítótár-kiszolgáló gyorsítótárazza a hello dokumentumok tartalom SAP-kiszolgálók. Ez az toooptimize hálózati forgalmat, ha a dokumentumokat csak egyszer más helyről beolvasott toobe rendelkezik. Általános szabály hello, hogy hello SAP gyorsítótár-kiszolgáló toobe fizikailag Bezárás toohello ügyfél, aki hozzáfér a hello SAP gyorsítótár-kiszolgálót. 

Itt két lehetőség közül választhat:

1. **Ügyfél egy olyan háttér SAP rendszer** SAP háttérrendszer konfigurált tooaccess SAP tartalomkiszolgáló, SAP rendszer akkor egy ügyfél. Mivel SAP rendszer és a SAP-kiszolgálót is telepített hello azonos Azure-régiót – hello azonos Azure-adatközpontban – azok fizikailag Bezárás tooeach más. Így nincs szükség toohave egy dedikált SAP-kiszolgáló nincs. SAP UI ügyfelek (SAP grafikus felhasználói felületének vagy a webes böngésző) hozzáférési hello SAP rendszer közvetlenül, és hello SAP rendszer beolvassa dokumentumok hello SAP-kiszolgálóhoz.
2. **Az ügyfél áll egy helyszíni webböngésző** hello SAP tartalomkiszolgáló hello webböngésző közvetlenül elérni beállított toobe lehet. Ebben az esetben a futtató helyszíni – webböngésző hello SAP-kiszolgálóhoz az ügyfél. A helyszíni adatközpont és az Azure-adatközpontban helyezi különböző fizikai helyen (ideális esetben Bezárás tooeach más). A helyszíni adatközpontját Azure telephelyek közötti VPN- vagy ExpressRoute keresztül csatlakoztatott tooAzure. Bár mindkét lehetőséget kínálnak biztonságos VPN-hálózati kapcsolat tooAzure, pont-pont hálózati kapcsolat nem biztosít a hálózati sávszélesség és a késleltetés SLA hello helyszíni adatközpontját és hello Azure datacenter között. toospeed hozzáférés toodocuments fel, tegye hello következők egyikét:
   1. Helyszíni SAP gyorsítótár-kiszolgálóra telepítse, zárja be a helyszíni webböngészőt toohello (beállítást [ez] [ dbms-guide-900-sap-cache-server-on-premises] . ábrát)
   2. Azure ExpressRoute, a nagy sebességű és kis késleltetésű dedikált hálózati kapcsolatot a helyszíni adatközpontban és Azure-adatközpontban kínál, amelyek konfigurálásához.

![A beállítás tooinstall SAP gyorsítótár-kiszolgáló helyi][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Biztonsági mentés / helyreállítás
Ha hello SAP tartalomkiszolgáló toostore fájlok hello SAP MaxDB adatbázis konfigurálja, hello biztonsági mentési/visszaállítási művelet és a teljesítménnyel kapcsolatos már ismertetett SAP MaxDB fejezet [biztonsági mentése és visszaállítása] [ dbms-guide-8.4.2] és fejezet [teljesítménnyel kapcsolatos szempontok a biztonsági mentési és visszaállítási][dbms-guide-8.4.3]. 

Ha hello SAP tartalomkiszolgáló toostore fájlok hello fájlrendszer konfigurálja, a egy beállítás nem tooexecute manuális biztonsági mentés/visszaállítás hello teljes fájlstruktúra, ahol hello dokumentumok találhatók. Hasonló tooSAP MaxDB biztonsági mentés/visszaállítás szolgáltatást, akkor egy dedikált lemezkötetet toohave ajánlott a biztonsági mentési cél. 

#### <a name="other"></a>Egyéb
Más SAP kiszolgáló speciális beállításokkal átlátszó tooAzure virtuális gép, és ismerteti a különböző dokumentumok és SAP megjegyzések:

* <https://Service.SAP.com/contentserver> 
* SAP Megjegyzés [1619726]  

## <a name="specifics-tooibm-db2-for-luw-on-windows"></a>A Windows LUW DB2 rögzítésen tooIBM
A Microsoft Azure-ban egyszerűen áttelepítheti a meglévő SAP alkalmazást IBM DB2 Linux, UNIX és a Windows (LUW) tooAzure virtuális gépekhez. Az IBM DB2 LUW az SAP, rendszergazdák és fejlesztők számára továbbra is használhatja hello azonos fejlesztői és felügyeleti eszközöket, amelyek elérhetők a helyszínen.
Általános információk az SAP Business Suite futó IBM DB2 LUW is található az SAP közösségi hálózati Állapotváltozás-: hello <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

További információért és frissítéseire vonatkozó DB2 Azure LUW az SAP, tekintse meg a SAP Megjegyzés [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX és a Windows által támogatott verzió
A Microsoft Azure virtuális gép Services LUW IBM DB2 SAP 10.5 DB2 verzió frissítésétől esetén támogatott.

Támogatott SAP-termékek és Azure Virtuálisgép-típusokon kapcsolatos információkért tekintse meg a Megjegyzés tooSAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX és a Windows konfigurációs irányelveket SAP-telepítések az Azure virtuális gépeken
#### <a name="storage-configuration"></a>Tárolás beállítása
Az adatbázisfájlok hello NTFS fájlrendszerben közvetlenül csatlakoztatott lemezek alapján kell tárolni. Ezek a lemezek csatlakoztatott toohello Azure virtuális Gépen, és az Azure lap BLOB Storage alapulnak (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) vagy felügyelt lemezeket (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Valamilyen hálózati meghajtók vagy távoli megosztások, például a következő Azure Fájlszolgáltatások hello vannak **nem** támogatott adatbázis-fájlok: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

A Azure lap BLOB-tároló vagy kezelt lemezek mérete alapján használja, ha ez a dokumentum fejezetben szereplő hello [RDBMS üzembe helyezésének struktúra] [ dbms-guide-2] is alkalmazhat az IBM DB2 hello toodeployments LUW adatbázis. 

Ahogy korábban hello hello dokumentum általános része, az lemezek IOPS átviteli kvótái létezik. hello pontos kvóták hello Virtuálisgép-típussá használt függ. A kvótával rendelkező VM Típuslista található [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows].

Mindaddig, amíg hello aktuális IOPS kvóta lemezenként lehetséges toostore elegendő az összes adatbázis-fájlokat egy egyetlen csatlakoztatott lemez hello. 

A teljesítmény szempontokat is tekintse meg a toochapter "biztonsági és teljesítmény megfontolások az adatbázis Adatkönyvtárak" a SAP telepítési útmutatóban.

Azt is megteheti, használhatja a Windows Tárolókészletek (csak érhető el a Windows Server 2012 és újabb), vagy a Windows 2008 R2, Windows csíkozást fejezetben ismertetett [szoftver RAID] [ dbms-guide-2.2] a jelen dokumentum toocreate egy nagy méretű logikai eszköz több lemez keresztül.
Hello DB2 tárolási útvonalat a sapdata és saptmp könyvtárak tartalmazó hello lemezek meg kell adnia egy fizikai lemez szektormérete 512 KB. Windows Tárolókészletek használata esetén létre kell hoznia hello manuálisan hello paraméter használatával parancssori felületen keresztül tárolókészletek "-LogicalSectorSizeDefault". További információkért lásd: <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.

#### <a name="backuprestore"></a>Biztonsági mentés / visszaállítás
IBM DB2 hello biztonsági mentés/visszaállítás funkcióját a LUW támogatott hello azonos módon történik a szabványos Windows Server operációs rendszerek és Hyper-V.

Meg kell győződnie arról, hogy rendelkezik-e egy érvényes adatbázis biztonsági mentési stratégia helyen. 

Operációs rendszer nélküli telepítések, mint biztonsági mentés/visszaállítás függ hány kötetek párhuzamosan olvasható, és előfordulhat, hogy milyen átviteli sebességgel hello a köteteket. Ezenkívül hello a biztonsági másolatok tömörítését által használt CPU-felhasználás előfordulhat, hogy fontos szerepet játszik a virtuális gépek csak másolatot tooeight CPU szálak. Ezért egy veheti fel:

* hello a használt lemezeket toostore száma kevesebb hello hello adatbázis eszközök, kisebb hello hello a teljes átviteli sebesség mértéke a olvasása
* hello VM CPU szálainak száma kisebb hello hello, hello súlyosabb hatását a biztonsági másolatok tömörítését hello
* hello kevesebb célok (paritásos könyvtárak, lemezek) toowrite hello történő biztonsági mentés, hello alacsonyabb hello átviteli sebesség

a célok toowrite tooincrease hello száma, a két lehetőség közül választhat lehet igényeitől függően használt/kombinált:

* Szétosztott biztonsági mentési célkötet hello rendelés tooimprove hello IOPS átviteli sebességének adott csíkozott köteten több lemez keresztül
* Egynél több cél directory toowrite hello történő biztonsági mentés használatával

#### <a name="high-availability-and-disaster-recovery"></a>Magas rendelkezésre állás és vészhelyreállítás
A Microsoft Fürtkiszolgáló (MSCS) nem támogatott.

DB2 magas rendelkezésre állású vész-helyreállítási (HADR) használata támogatott. Ha hello virtuális gépek magas rendelkezésre ÁLLÁSÚ konfiguráció hello névfeloldás működik, hello beállítása az Azure-ban nem Miben különböznek a telepítés, amely a helyszínen történik. A csak az IP-megoldáshoz toorely nem ajánlott.

Ne használjon Georeplikáció hello storage-fiókok hello adatbázis lemezek tárolására. További információkért tekintse meg a toochapter [Microsoft Azure Storage] [ dbms-guide-2.3] és fejezet [magas rendelkezésre állás és vészhelyreállítás Azure virtuális gépeken futó] [ dbms-guide-3].

#### <a name="other"></a>Egyéb
Minden más általános például Azure rendelkezésre állási készletek vagy SAP figyelés témakörei az IBM DB2 LUW a virtuális gépek telepítéséhez, valamint a dokumentum első három fejezetek hello leírtak szerint. 

Lásd még: toochapter [az SAP Azure összegzése az SQL Server általános][dbms-guide-5.8].

## <a name="specifics-tooibm-db2-for-luw-on-linux"></a>A Linux LUW DB2 rögzítésen tooIBM
A Microsoft Azure-ban egyszerűen áttelepítheti a meglévő SAP alkalmazást IBM DB2 Linux, UNIX és a Windows (LUW) tooAzure virtuális gépekhez. Az IBM DB2 LUW az SAP, rendszergazdák és fejlesztők számára továbbra is használhatja hello azonos fejlesztői és felügyeleti eszközöket, amelyek elérhetők a helyszínen. Általános információk az SAP Business Suite futó IBM DB2 LUW is található az SAP közösségi hálózati Állapotváltozás-: hello <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

További információért és frissítéseire vonatkozó DB2 Azure LUW az SAP, tekintse meg a SAP Megjegyzés [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX és a Windows által támogatott verzió
A Microsoft Azure virtuális gép Services LUW IBM DB2 SAP 10.5 DB2 verzió frissítésétől esetén támogatott.

Támogatott SAP-termékek és Azure Virtuálisgép-típusokon kapcsolatos információkért tekintse meg a Megjegyzés tooSAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX és a Windows konfigurációs irányelveket SAP-telepítések az Azure virtuális gépeken
#### <a name="storage-configuration"></a>Tárolás beállítása
Minden adatbázisfájlt a közvetlenül csatlakoztatott lemezek alapján fájlrendszeren kell tárolni. Ezek a lemezek csatlakoztatott toohello Azure virtuális Gépen, és az Azure lap BLOB Storage alapulnak (<https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs>) vagy felügyelt lemezeket (<https://docs.microsoft.com/azure/storage/storage-managed-disks-overview>). Valamilyen hálózati meghajtók vagy távoli megosztások, például a következő Azure Fájlszolgáltatások hello vannak **nem** támogatott adatbázis-fájlok:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

A Azure lap BLOB-tároló mérete alapján használja, ha ez a dokumentum fejezetben szereplő hello [RDBMS üzembe helyezésének struktúra] [ dbms-guide-2] is alkalmazhat az IBM DB2 hello toodeployments LUW adatbázis.

Ahogy korábban hello hello dokumentum általános része, az lemezek IOPS átviteli kvótái létezik. hello pontos kvóták hello Virtuálisgép-típussá használt függ. A kvótával rendelkező VM Típuslista található [itt (Linux)] [ virtual-machines-sizes-linux] és [itt (Windows)][virtual-machines-sizes-windows].

Mindaddig, amíg hello aktuális IOPS kvóta lemezenként lehetséges toostore elegendő az összes adatbázis fájlok egyetlen lemezen hello.

A teljesítmény szempontokat is tekintse meg a toochapter "biztonsági és teljesítmény megfontolások az adatbázis Adatkönyvtárak" a SAP telepítési útmutatóban.

Másik megoldásként használhatja LVM (logikai kötetkezelő) vagy MDADM fejezetben leírtak [szoftver RAID] [ dbms-guide-2.2] dokumentum toocreate egy nagy méretű logikai eszköz több lemez keresztül.
Hello DB2 tárolási útvonalat a sapdata és saptmp könyvtárak tartalmazó hello lemezek meg kell adnia egy fizikai lemez szektormérete 512 KB.

#### <a name="backuprestore"></a>Biztonsági mentés / visszaállítás
IBM DB2 hello biztonsági mentés/visszaállítás funkcióját az való LUW megadása támogatott. a hello azonos módon történik a szabványos Linux telepítését a helyszínen.

Meg kell győződnie arról, hogy rendelkezik-e egy érvényes adatbázis biztonsági mentési stratégia helyen.

Operációs rendszer nélküli telepítések, mint biztonsági mentés/visszaállítás függ hány kötetek párhuzamosan olvasható, és előfordulhat, hogy milyen átviteli sebességgel hello a köteteket. Ezenkívül hello a biztonsági másolatok tömörítését által használt CPU-felhasználás előfordulhat, hogy fontos szerepet játszik a virtuális gépek csak másolatot tooeight CPU szálak. Ezért egy veheti fel:

* hello a használt lemezeket toostore száma kevesebb hello hello adatbázis eszközök, kisebb hello hello a teljes átviteli sebesség mértéke a olvasása
* hello VM CPU szálainak száma kisebb hello hello, hello súlyosabb hatását a biztonsági másolatok tömörítését hello
* hello kevesebb célok (paritásos könyvtárak, lemezek) toowrite hello történő biztonsági mentés, hello alacsonyabb hello átviteli sebesség

a célok toowrite tooincrease hello száma, a két lehetőség közül választhat lehet igényeitől függően használt/kombinált:

* Szétosztott biztonsági mentési célkötet hello rendelés tooimprove hello IOPS átviteli sebességének adott csíkozott köteten több lemez keresztül
* Egynél több cél directory toowrite hello történő biztonsági mentés használatával

#### <a name="high-availability-and-disaster-recovery"></a>Magas rendelkezésre állás és vészhelyreállítás
DB2 magas rendelkezésre állású vész-helyreállítási (HADR) használata támogatott. Ha hello virtuális gépek magas rendelkezésre ÁLLÁSÚ konfiguráció hello névfeloldás működik, hello beállítása az Azure-ban nem Miben különböznek a telepítés, amely a helyszínen történik. A csak az IP-megoldáshoz toorely nem ajánlott.

Ne használjon Georeplikáció hello storage-fiókok hello adatbázis lemezek tárolására. További információkért tekintse meg a toochapter [Microsoft Azure Storage] [ dbms-guide-2.3] és fejezet [magas rendelkezésre állás és vészhelyreállítás Azure virtuális gépeken futó] [ dbms-guide-3].

#### <a name="other"></a>Egyéb
Minden más általános például Azure rendelkezésre állási készletek vagy SAP figyelés témakörei az IBM DB2 LUW a virtuális gépek telepítéséhez, valamint a dokumentum első három fejezetek hello leírtak szerint.

Lásd még: toochapter [az SAP Azure összegzése az SQL Server általános][dbms-guide-5.8].

