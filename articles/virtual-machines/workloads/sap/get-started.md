---
title: "az Azure virtuális gépeken SAP lépései aaaGetting |} Microsoft Docs"
description: "További tudnivalók az SAP megoldások futó virtuális gépek (VM) a Microsoft Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0ac390f8e1c802505b8f9304a12868364fa60f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-for-hosting-and-running-sap-workload-scenarios"></a><span data-ttu-id="5a983-103">Az Azure üzemeltetési és SAP munkaterhelés forgatókönyvek fut</span><span class="sxs-lookup"><span data-stu-id="5a983-103">Using Azure for hosting and running SAP workload scenarios</span></span>
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

[azure-cli]:../../../cli-install-nodejs.md
[azure-portal]:https://portal.azure.com
[azure-ps]:/powershell/azureps-cmdlets-docs
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md#subscription

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
[deploy-template-cli]:../../../resource-group-template-deploy.md
[deploy-template-portal]:../../../resource-group-template-deploy.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c


[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056
[ha-guide]:sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
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
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
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
[virtual-machines-linux-capture-image-resource-manager-capture]:../../linux/capture-image.md#capture-the-vm
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
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
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

<span data-ttu-id="5a983-104">Válassza ki a Microsoft Azure-bA az SAP készen áll a cloud partner, tudja futtatni a kritikus fontosságú SAP számítási feladatokhoz és a forgatókönyvek olyan méretezhető, kompatibilis és vállalati bizonyítása platformon tooreliably áll.</span><span class="sxs-lookup"><span data-stu-id="5a983-104">By choosing Microsoft Azure as your SAP ready cloud partner, you are able tooreliably run your mission critical SAP workloads and scenarios on a scalable, compliant, and enterprise-proven platform.</span></span>  <span data-ttu-id="5a983-105">Hello méretezhetőséget, rugalmas, és a költségeket, Azure-os megtakarítást.</span><span class="sxs-lookup"><span data-stu-id="5a983-105">Get hello scalability, flexibility, and cost savings of Azure.</span></span> <span data-ttu-id="5a983-106">Bővített hello partnerek között a Microsoft és az SAP SAP-alkalmazásokból futtathatja az Azure - fejlesztési és tesztelési célú és éles forgatókönyvek között, és teljes mértékben támogatja.</span><span class="sxs-lookup"><span data-stu-id="5a983-106">With hello expanded partnership between Microsoft and SAP, you can run SAP applications across dev/test and production scenarios in Azure - and be fully supported.</span></span> <span data-ttu-id="5a983-107">Az SAP NetWeaver tooSAP S4/HANA, SAP BI, Linux tooWindows, SAP HANA tooSQL tudunk akkor jelez.</span><span class="sxs-lookup"><span data-stu-id="5a983-107">From SAP NetWeaver tooSAP S4/HANA, SAP BI, Linux tooWindows, SAP HANA tooSQL, we have you covered.</span></span> 

<span data-ttu-id="5a983-108">Az SAP NetWeaver szolgáltatókörnyezetekben mellett hello Azure különböző DBMS, üzemeltethet különböző SAP munkaterhelés egyéb forgatókönyvek, például SAP-bA az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="5a983-108">Besides hosting SAP NetWeaver scenarios with hello different DBMS on Azure, you can host different other SAP workload scenarios, like SAP BI on Azure.</span></span> <span data-ttu-id="5a983-109">SAP NetWeaver telepítések Azure natív virtuális gépekre vonatkozó dokumentáció itt található hello szakaszban "SAP NetWeaver Azure virtuális gépeken futó."</span><span class="sxs-lookup"><span data-stu-id="5a983-109">Documentation regarding SAP NetWeaver deployments on Azure native Virtual Machines can be found in hello section "SAP NetWeaver on Azure Virtual Machines."</span></span> 

<span data-ttu-id="5a983-110">Azure rendelkezik natív Azure virtuális gép ajánlatokat, amelyek legalább egyszer nőnek CPU és memória erőforrások toocover SAP munkaterhelés, amely kihasználja a SAP HANA-méretet.</span><span class="sxs-lookup"><span data-stu-id="5a983-110">Azure has native Azure Virtual Machine offers that are ever growing in size of CPU and memory resources toocover SAP workload that leverages SAP HANA.</span></span> <span data-ttu-id="5a983-111">A témakörrel kapcsolatos további információkért kereshető hello dokumentumok hello szakaszban SAP HANA Azure virtuális gépeken."</span><span class="sxs-lookup"><span data-stu-id="5a983-111">For more information on this topic, look up hello documents under hello section SAP HANA on Azure Virtual Machines."</span></span>

<span data-ttu-id="5a983-112">SAP Hana Azure hello egyediségének egy olyan egyedi ajánlat, leszámítva verseny Azure megadó.</span><span class="sxs-lookup"><span data-stu-id="5a983-112">hello uniqueness of Azure for SAP HANA is a unique offer that sets Azure apart from competition.</span></span> <span data-ttu-id="5a983-113">A további memória és CPU-erőforrást üzemeltetési rendelés tooenable SAP HANA érintő forgatókönyvek, az Azure kínál hello használati ügyfél kibővített SAP dedikált hello célból be (60 TB kibővített) TB too20 igénylő SAP HANA-telepítések futó operációs rendszer nélküli hardver S/4HANA vagy más SAP HANA-munkaterhelés memóriát.</span><span class="sxs-lookup"><span data-stu-id="5a983-113">In order tooenable hosting more memory and CPU resource demanding SAP scenarios involving SAP HANA, Azure offers hello usage of customer dedicated bare-metal hardware for hello purpose of running SAP HANA deployments that require up too20 TB (60 TB scale-out) of memory for S/4HANA or other SAP HANA workload.</span></span> <span data-ttu-id="5a983-114">Az SAP HANA Azure (nagy példány) az egyedi Azure megoldás lehetővé teszi toorun SAP HANA hello SAP alkalmazásréteg vagy natív Azure virtuális gépeken futó munkaterhelések közel-vő réteg dedikált hello operációs rendszer nélküli hardveren.</span><span class="sxs-lookup"><span data-stu-id="5a983-114">This unique Azure solution of SAP HANA on Azure (Large Instances) allows you toorun SAP HANA on hello dedicated bare-metal hardware with hello SAP application layer or workload middle-ware layer hosted in native Azure Virtual Machines.</span></span> <span data-ttu-id="5a983-115">Ez a megoldás leírása több dokumentumok hello szakaszban "SAP HANA Azure (nagy példány)."</span><span class="sxs-lookup"><span data-stu-id="5a983-115">This solution is documented in several documents in hello section "SAP HANA on Azure (Large Instances)."</span></span>   

<span data-ttu-id="5a983-116">SAP munkaterhelés szolgáltatókörnyezetekben az Azure-ban is hozhat létre és Single-Sign-On Azure tevékenység Directory toodifferent SAP-összetevők és az SAP SaaS identitásintegráció követelményeinek, vagy PaaS kínál.</span><span class="sxs-lookup"><span data-stu-id="5a983-116">Hosting SAP workload scenarios in Azure also can create requirements of Identity integration and Single-Sign-On using Azure Activity Directory toodifferent SAP components and SAP SaaS or PaaS offers.</span></span> <span data-ttu-id="5a983-117">Ilyen integrációs és Single-Sign-On Azure Active Directory (AAD) és az SAP entitások forgatókönyvek listája leírt és hello része "AAD SAP Identitásintegráció és Single-Sign-On."</span><span class="sxs-lookup"><span data-stu-id="5a983-117">A list of such integration and Single-Sign-On scenarios with Azure Active Directory (AAD) and SAP entities is described and documented in hello section "AAD SAP Identity Integration and Single-Sign-On."</span></span>


## <a name="sap-hana-on-sap-hana-on-azure-large-instances"></a><span data-ttu-id="5a983-118">SAP HANA az SAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-118">SAP HANA on SAP HANA on Azure (Large Instances)</span></span>

### <a name="overview-and-architecture-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="5a983-119">Áttekintés és az SAP HANA Azure (nagy példányok) architektúrája</span><span class="sxs-lookup"><span data-stu-id="5a983-119">Overview and architecture of SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="5a983-120">cím: aaaOverview és architektúra az SAP HANA, az Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-120">title: aaaOverview and Architecture of SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="5a983-121">Összefoglalás: A architektúra és a műszaki üzembe helyezési útmutatójában biztosít információkat toohelp SAP központilag telepíti a hello új SAP HANA Azure (nagy példány) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5a983-121">Summary: This Architecture and Technical Deployment Guide provides information toohelp you deploy SAP on hello new SAP HANA on Azure (Large Instances) in Azure.</span></span> <span data-ttu-id="5a983-122">Már nem tervezett toobe egy átfogó képet fedi le beállításai között SAP-megoldások, de a kezdeti üzembe helyezése és folyamatban lévő műveletek ahelyett, hogy hasznos információkhoz.</span><span class="sxs-lookup"><span data-stu-id="5a983-122">It is not intended toobe a comprehensive guide covering specific setup of SAP solutions, but rather useful information in your initial deployment and ongoing operations.</span></span> <span data-ttu-id="5a983-123">Nem váltja fel az SAP-dokumentáció kapcsolódó SAP HANA toohello telepítése (vagy hello fedik le a hello témakör számos SAP támogatási megjegyzések).</span><span class="sxs-lookup"><span data-stu-id="5a983-123">It should not replace SAP documentation related toohello installation of SAP HANA (or hello many SAP Support Notes that cover hello topic).</span></span> <span data-ttu-id="5a983-124">Áttekintést nyújt, és részletesen ismerteti hello további a SAP HANA telepítése az Azure (nagy példány).</span><span class="sxs-lookup"><span data-stu-id="5a983-124">It gives you an overview and provides hello additional detail of installing SAP HANA on Azure (Large Instances).</span></span>

<span data-ttu-id="5a983-125">Frissített: 2017. július.</span><span class="sxs-lookup"><span data-stu-id="5a983-125">Updated: July 2017</span></span>

[<span data-ttu-id="5a983-126">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-126">This guide can be found here</span></span>](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="infrastructure-and-connectivity-toosap-hana-on-azure-large-instances"></a><span data-ttu-id="5a983-127">Infrastruktúra és a csatlakozási tooSAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-127">Infrastructure and connectivity tooSAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="5a983-128">cím: aaaInfrastructure és csatlakozási tooSAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-128">title: aaaInfrastructure and Connectivity tooSAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="5a983-129">Összefoglalás: Után hello beszerzési SAP HANA Azure (nagy példány) az Ön és a vállalati Microsoft-fiókok ügyfélszolgálatától hello közötti le van zárva, különböző hálózati konfigurációkban szükségesek a rendelés tooensure megfelelő kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5a983-129">Summary: After hello purchase of SAP HANA on Azure (Large Instances) is finalized between you and hello Microsoft enterprise account team, various network configurations are required in order tooensure proper connectivity.</span></span>  <span data-ttu-id="5a983-130">A dokumentum vázol fel hello, amely rendelkezik a következő információ hello megosztott toobe szükség.</span><span class="sxs-lookup"><span data-stu-id="5a983-130">This document outlines hello information that has toobe shared with hello following information is required.</span></span> <span data-ttu-id="5a983-131">Ez a dokumentum bemutatja, milyen információkat vannak összegyűjtött toobe és milyen konfigurációs parancsfájlokat kell toobe futtatni.</span><span class="sxs-lookup"><span data-stu-id="5a983-131">This document outlines what information has toobe collected and what configuration scripts have toobe run.</span></span> 

<span data-ttu-id="5a983-132">Frissített: 2017. július.</span><span class="sxs-lookup"><span data-stu-id="5a983-132">Updated: July 2017</span></span>

[<span data-ttu-id="5a983-133">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-133">This guide can be found here</span></span>](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="install-sap-hana-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="5a983-134">SAP HANA SAP HANA Azure (nagy példányok) telepítése</span><span class="sxs-lookup"><span data-stu-id="5a983-134">Install SAP HANA in SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="5a983-135">cím: aaaInstall SAP HANA az SAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-135">title: aaaInstall SAP HANA on SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="5a983-136">Összefoglalás: Ez a dokumentum hello beállítása eljárásai SAP HANA telepítése az Azure-nagy példányon ismertet.</span><span class="sxs-lookup"><span data-stu-id="5a983-136">Summary: This document outlines hello setup procedures for installing SAP HANA on your Azure Large Instance.</span></span> 

<span data-ttu-id="5a983-137">Frissített: 2017. július.</span><span class="sxs-lookup"><span data-stu-id="5a983-137">Updated: July 2017</span></span>

[<span data-ttu-id="5a983-138">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-138">This guide can be found here</span></span>](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-and-disaster-recovery-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="5a983-139">Magas rendelkezésre állási és vészhelyreállítási helyreállítási SAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-139">High availability and disaster recovery of SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="5a983-140">cím: aaaHigh rendelkezésre állás és a katasztrófa utáni helyreállítás az SAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-140">title: aaaHigh Availability and Disaster Recovery of SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="5a983-141">Összefoglalás: Magas rendelkezésre állású (HA) és a vész-helyreállítási (DR) szempontot nagyon fontos a kritikus fontosságú SAP HANA futó Azure (nagy példányok) kiszolgáló (k).</span><span class="sxs-lookup"><span data-stu-id="5a983-141">Summary: High Availability (HA) and Disaster Recovery (DR) are very important aspects of running your mission-critical SAP HANA on Azure (Large Instances) server(s).</span></span> <span data-ttu-id="5a983-142">Az importálás toowork az SAP, a rendszer integráló, és/vagy a Microsoft tooproperly tervezővel, és az Ön hello jobb elérhető HA/DR-stratégia megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="5a983-142">It's import toowork with SAP, your system integrator, and/or Microsoft tooproperly architect and implement hello right HA/DR strategy for you.</span></span> <span data-ttu-id="5a983-143">Fontos tudnivalók találhatók, például a helyreállítási-célkitűzés (RPO) és a helyreállítási idő célkitűzés (RTO), adott tooyour környezetben, figyelembe kell venni.</span><span class="sxs-lookup"><span data-stu-id="5a983-143">Important considerations like Recovery Point Objective (RPO) and Recovery Time Objective (RTO), specific tooyour environment, must be considered.</span></span>  <span data-ttu-id="5a983-144">Ez a dokumentum ismerteti a beállítások a magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási előnyben részesített szintjének engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a983-144">This document explains your options for enabling your preferred level of HA and DR.</span></span>

<span data-ttu-id="5a983-145">Frissített: 2016. December.</span><span class="sxs-lookup"><span data-stu-id="5a983-145">Updated: December 2016</span></span>

[<span data-ttu-id="5a983-146">Ez a dokumentum itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-146">This document can be found here</span></span>](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="troubleshooting-and-monitoring-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="5a983-147">Hibaelhárítás és SAP HANA Azure (nagy példányok) figyelése</span><span class="sxs-lookup"><span data-stu-id="5a983-147">Troubleshooting and monitoring of SAP HANA on Azure (Large Instances)</span></span>
<span data-ttu-id="5a983-148">cím: aaaTroubleshooting és a figyelés az SAP HANA Azure (nagy példány)</span><span class="sxs-lookup"><span data-stu-id="5a983-148">title: aaaTroubleshooting and Monitoring of SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="5a983-149">Összefoglalás: Ez az útmutató ismerteti, amely hasznos létrehozása az Azure-alapú környezetben az SAP HANA figyelését, valamint az olyan további hibaelhárítási információkat.</span><span class="sxs-lookup"><span data-stu-id="5a983-149">Summary: This guide covers information that is useful in establishing monitoring of your SAP HANA on Azure environment as well as additional troubleshooting information.</span></span> 

<span data-ttu-id="5a983-150">Frissített: 2016. December.</span><span class="sxs-lookup"><span data-stu-id="5a983-150">Updated: December 2016</span></span>

[<span data-ttu-id="5a983-151">Ez a dokumentum itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-151">This document can be found here</span></span>](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sap-hana-on-azure-virtual-machines"></a><span data-ttu-id="5a983-152">SAP HANA az Azure-beli virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-152">SAP HANA on Azure Virtual Machines</span></span>

### <a name="getting-started-with-sap-hana-on-azure"></a><span data-ttu-id="5a983-153">Ismerkedés az Azure SAP HANA</span><span class="sxs-lookup"><span data-stu-id="5a983-153">Getting started with SAP HANA on Azure</span></span>
<span data-ttu-id="5a983-154">cím: Azure virtuális gépeken a manuális telepítésére a SAP HANA aaaQuickstart útmutatója</span><span class="sxs-lookup"><span data-stu-id="5a983-154">title: aaaQuickstart guide for manual installation of SAP HANA on Azure VMs</span></span>

<span data-ttu-id="5a983-155">Összefoglalás: A gyors üzembe helyezési útmutató segítségével egypéldányos SAP HANA rendszerének Azure virtuális gépeken által manuális telepítés SAP NetWeaver 7.5 és SAP HANA SP12 tooset.</span><span class="sxs-lookup"><span data-stu-id="5a983-155">Summary: This quickstart guide helps tooset up a single-instance SAP HANA system on Azure VMs by a manual installation of SAP NetWeaver 7.5 and SAP HANA SP12.</span></span> <span data-ttu-id="5a983-156">hello az útmutató feltételezi, hogy ismeri a Azure IaaS alapok például hogyan toodeploy virtuális gépek vagy virtuális hálózatok keresztül hello Azure-portál vagy Powershell vagy parancssori felület beleértve hello beállítás toouse json sablonok-e hello olvasó.</span><span class="sxs-lookup"><span data-stu-id="5a983-156">hello guide presumes that hello reader is familiar with Azure IaaS basics like how toodeploy virtual machines or virtual networks either via hello Azure portal or Powershell/CLI including hello option toouse json templates.</span></span> <span data-ttu-id="5a983-157">Továbbá várható, hogy ismeri a SAP HANA, SAP NetWeaver és hogyan tooinstall azt a helyszíni-e hello olvasó.</span><span class="sxs-lookup"><span data-stu-id="5a983-157">Furthermore it's expected that hello reader is familiar with SAP HANA, SAP NetWeaver and how tooinstall it on-premises.</span></span>

<span data-ttu-id="5a983-158">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-158">Updated: June 2017</span></span>

[<span data-ttu-id="5a983-159">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-159">This guide can be found here</span></span>](hana-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="s4hana-sap-cal-deployment-on-azure"></a><span data-ttu-id="5a983-160">S/4HANA SAP CAL telepítése az Azure-on</span><span class="sxs-lookup"><span data-stu-id="5a983-160">S/4HANA SAP CAL deployment on Azure</span></span>
<span data-ttu-id="5a983-161">cím: aaaDeploy SAP S/4HANA vagy BW/4HANA az Azure-on</span><span class="sxs-lookup"><span data-stu-id="5a983-161">title: aaaDeploy SAP S/4HANA or BW/4HANA on Azure</span></span>

<span data-ttu-id="5a983-162">Összefoglalás: Az útmutató segítségével S SAP 4HANA Azure-ban felhő készülék teszteléshez toodemonstrate hello központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="5a983-162">Summary: This guide helps toodemonstrate hello deployment of SAP S/4HANA on Azure using SAP Cloud Appliance Library.</span></span> <span data-ttu-id="5a983-163">Felhő készülék teszteléshez egy olyan szolgáltatás, amely lehetővé teszi a toodeploy SAP alkalmazások az Azure által.</span><span class="sxs-lookup"><span data-stu-id="5a983-163">SAP Cloud Appliance Library is a service by SAP that allows toodeploy SAP applications on Azure.</span></span> <span data-ttu-id="5a983-164">hello az útmutató lépésről lépésre hello központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="5a983-164">hello guide describes step by step hello deployment.</span></span>

<span data-ttu-id="5a983-165">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-165">Updated: June 2017</span></span>

[<span data-ttu-id="5a983-166">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-166">This guide can be found here</span></span>](cal-s4h.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="high-availability-of-sap-hana-in-azure-virtual-machines"></a><span data-ttu-id="5a983-167">Magas rendelkezésre állású SAP HANA az Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-167">High Availability of SAP HANA in Azure Virtual Machines</span></span>
<span data-ttu-id="5a983-168">cím: aaaHigh rendelkezésre állási az SAP HANA Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-168">title: aaaHigh Availability of SAP HANA on Azure Virtual Machines</span></span>

<span data-ttu-id="5a983-169">Összefoglalás: Ez az útmutató részletes útmutatást a le a hello magas rendelkezésre állású konfiguráció hello SUSE 12 operációs rendszer, és az SAP HANA tooaccommodate HANA rendszer replikációs automatikus feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="5a983-169">Summary: This guide leads you through hello high availability configuration of hello SUSE 12 OS and SAP HANA tooaccommodate HANA System replication with automatic failover.</span></span> <span data-ttu-id="5a983-170">hello útmutató adott SUSE és az Azure virtuális gépek is.</span><span class="sxs-lookup"><span data-stu-id="5a983-170">hello guide is specific for SUSE and Azure Virtual Machines.</span></span> <span data-ttu-id="5a983-171">hello útmutató nem vonatkozik még Red Hat vagy az operációs rendszer nélküli vagy a saját felhő- vagy más-Azure nyilvános felhőben történő alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="5a983-171">hello guide does not apply yet for Red Hat or bare-metal or private cloud or other non-Azure public cloud deployments.</span></span>

<span data-ttu-id="5a983-172">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-172">Updated: June 2017</span></span>

[<span data-ttu-id="5a983-173">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-173">This guide can be found here</span></span>](sap-hana-high-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-backup-overview-on-azure-vms"></a><span data-ttu-id="5a983-174">A biztonsági mentés áttekintése SAP HANA Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-174">SAP HANA backup overview on Azure VMs</span></span>
<span data-ttu-id="5a983-175">cím: aaaBackup útmutató SAP Hana Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-175">title: aaaBackup guide for SAP HANA on Azure Virtual Machines</span></span>

<span data-ttu-id="5a983-176">Összefoglalás: Ez az útmutató SAP HANA-Azure virtuális gépeken futó biztonsági mentési lehetőségek alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="5a983-176">Summary: This guide provides basic information about backup possibilities running SAP HANA on Azure Virtual Machines.</span></span>

<span data-ttu-id="5a983-177">Frissített: 2017. március.</span><span class="sxs-lookup"><span data-stu-id="5a983-177">Updated: March 2017</span></span>

[<span data-ttu-id="5a983-178">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-178">This guide can be found here</span></span>](sap-hana-backup-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="sap-hana-file-level-backup-on-azure-vms"></a><span data-ttu-id="5a983-179">SAP HANA szintű biztonsági mentés Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-179">SAP HANA file level backup on Azure VMs</span></span>
<span data-ttu-id="5a983-180">cím: storage-pillanatfelvételekkel alapján aaaSAP HANA biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="5a983-180">title: aaaSAP HANA backup based on storage snapshots</span></span>

<span data-ttu-id="5a983-181">Összefoglalás: Ez az útmutató használatával kapcsolatban nyújt információkat pillanatkép-alapú biztonsági mentések az Azure virtuális gépeken futó Azure virtuális gépeken SAP HANA futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="5a983-181">Summary: This guide provides information about using snapshot-based backups on Azure VMs when running SAP HANA on Azure Virtual Machines.</span></span>

<span data-ttu-id="5a983-182">Frissített: 2017. március.</span><span class="sxs-lookup"><span data-stu-id="5a983-182">Updated: March 2017</span></span>

[<span data-ttu-id="5a983-183">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-183">This guide can be found here</span></span>](sap-hana-backup-file-level.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="sap-hana-snapshot-based-backups-on-azure-vms"></a><span data-ttu-id="5a983-184">SAP HANA-pillanatkép-alapú biztonsági mentések Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-184">SAP HANA snapshot based backups on Azure VMs</span></span>
<span data-ttu-id="5a983-185">cím: Azure biztonsági mentés HANA fájlszintű aaaSAP</span><span class="sxs-lookup"><span data-stu-id="5a983-185">title: aaaSAP HANA Azure Backup on file level</span></span>

<span data-ttu-id="5a983-186">Összefoglalás: Ez az útmutató használatával kapcsolatban nyújt információkat SAP HANA szintű biztonsági mentés SAP HANA futtató Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-186">Summary: This guide provides information about using SAP HANA file level backup running SAP HANA on Azure Virtual Machines</span></span>

<span data-ttu-id="5a983-187">Frissített: 2017. március.</span><span class="sxs-lookup"><span data-stu-id="5a983-187">Updated: March 2017</span></span>

[<span data-ttu-id="5a983-188">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-188">This guide can be found here</span></span>](sap-hana-backup-storage-snapshots.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a><span data-ttu-id="5a983-189">SAP NetWeaver telepítése Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="5a983-189">SAP NetWeaver deployed on Azure Virtual Machines</span></span>

### <a name="deploy-sap-ides-system-on-windows-and-sql-server-through-sap-cal-on-azure"></a><span data-ttu-id="5a983-190">A Windows és az SQL Server SAP CAL az Azure-on keresztül SAP IDES rendszer központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5a983-190">Deploy SAP IDES system on Windows and SQL Server through SAP CAL on Azure</span></span>
<span data-ttu-id="5a983-191">cím: Azure virtuális gépeken Microsoft SUSE Linux SAP NetWeaver aaaTesting</span><span class="sxs-lookup"><span data-stu-id="5a983-191">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span> 

<span data-ttu-id="5a983-192">Összefoglalás: Ez a dokumentum ismerteti a Windows és az SQL Server Azure-ban felhő készülék teszteléshez alapján SAP IDES rendszer hello telepítési.</span><span class="sxs-lookup"><span data-stu-id="5a983-192">Summary: This document describes hello deployment of an SAP IDES system based on Windows and SQL Server on Azure using SAP Cloud Appliance Library.</span></span> <span data-ttu-id="5a983-193">SAP felhő készülék könyvtárban egy SAP szolgáltatás, amely lehetővé teszi az SAP termékek hello telepítési Azure.</span><span class="sxs-lookup"><span data-stu-id="5a983-193">SAP Cloud appliance Library is an SAP service that allows hello deployment of SAP products on Azure.</span></span> <span data-ttu-id="5a983-194">Ez a dokumentum lépésről lépésre végighalad egy SAP IDES rendszer hello telepítése.</span><span class="sxs-lookup"><span data-stu-id="5a983-194">This document goes step by step through hello deployment of an SAP IDES system.</span></span> <span data-ttu-id="5a983-195">hello IDES rendszer csak egy példa több más dozen alkalmazások esetében is telepíthető a Microsoft Azure SAP felhő készülék keresztül.</span><span class="sxs-lookup"><span data-stu-id="5a983-195">hello IDES system is just an example for several other dozen applications that can be deployed through SAP Cloud appliance on Microsoft Azure.</span></span>

<span data-ttu-id="5a983-196">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-196">Updated: June 2017</span></span>

[<span data-ttu-id="5a983-197">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-197">This guide can be found here</span></span>](cal-ides-erp6-erp7-sp3-sql.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


### <a name="quickstart-guide-for-netweaver-on-suse-linux-on-azure"></a><span data-ttu-id="5a983-198">SUSE Linux Azure NetWeaver a gyors üzembe helyezési útmutatója</span><span class="sxs-lookup"><span data-stu-id="5a983-198">Quickstart guide for NetWeaver on SUSE Linux on Azure</span></span>
<span data-ttu-id="5a983-199">cím: Azure virtuális gépeken Microsoft SUSE Linux SAP NetWeaver aaaTesting</span><span class="sxs-lookup"><span data-stu-id="5a983-199">title: aaaTesting SAP NetWeaver on Microsoft Azure SUSE Linux VMs</span></span> 

<span data-ttu-id="5a983-200">Összefoglalás: Ez a cikk ismerteti különböző dolgok tooconsider SAP NetWeaver Microsoft Azure SUSE Linux virtuális gépek (VM) futtatja.</span><span class="sxs-lookup"><span data-stu-id="5a983-200">Summary: This article describes various things tooconsider when you're running SAP NetWeaver on Microsoft Azure SUSE Linux virtual machines (VMs).</span></span> <span data-ttu-id="5a983-201">SAP NetWeaver hivatalosan támogatott SUSE Linux virtuális gépek Azure-on.</span><span class="sxs-lookup"><span data-stu-id="5a983-201">SAP NetWeaver is officially supported on SUSE Linux VMs on Azure.</span></span> <span data-ttu-id="5a983-202">Linux-verziók, SAP kernel verzió és egyéb részletek kapcsolatos összes részleteket található SAP Megjegyzés 1928533 "SAP-alkalmazások az Azure-on: a támogatott és az Azure Virtuálisgép-típusokon".</span><span class="sxs-lookup"><span data-stu-id="5a983-202">All details regarding Linux versions, SAP kernel versions, and other details can be found in SAP Note 1928533 "SAP Applications on Azure: Supported Products and Azure VM types".</span></span>

<span data-ttu-id="5a983-203">Frissített: 2016 szeptemberétől</span><span class="sxs-lookup"><span data-stu-id="5a983-203">Updated: September 2016</span></span>

[<span data-ttu-id="5a983-204">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-204">This guide can be found here</span></span>](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <span data-ttu-id="5a983-205"><a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Tervezési és megvalósítási</span><span class="sxs-lookup"><span data-stu-id="5a983-205"><a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Planning and implementation</span></span>
<span data-ttu-id="5a983-206">cím: virtuális gépek aaaAzure tervezési és megvalósítási az SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="5a983-206">title: aaaAzure Virtual Machines planning and implementation for SAP NetWeaver</span></span>

<span data-ttu-id="5a983-207">Összefoglalás: Ez a dokumentum esetén hello útmutató toostart rendelkező, a SAP NetWeaver futtató Azure virtuális gépek gondolat.</span><span class="sxs-lookup"><span data-stu-id="5a983-207">Summary: This document is hello guide toostart with if you are thinking about running SAP NetWeaver in Azure Virtual Machines.</span></span> <span data-ttu-id="5a983-208">A tervezési és megvalósítási útmutató segítségével kiértékelheti, hogy egy meglévő vagy tervezett SAP NetWeaver-alapú rendszeren telepített tooan Azure virtuális gépek környezet lehet-e.</span><span class="sxs-lookup"><span data-stu-id="5a983-208">This planning and implementation guide helps you evaluate whether an existing or planned SAP NetWeaver-based system can be deployed tooan Azure Virtual Machines environment.</span></span> <span data-ttu-id="5a983-209">Több SAP NetWeaver telepítési forgatókönyvek foglalkozik, és tartalmazza, amelyek adott tooAzure SAP-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="5a983-209">It covers multiple SAP NetWeaver deployment scenarios, and includes SAP configurations that are specific tooAzure.</span></span> <span data-ttu-id="5a983-210">hello dokumentum, és minden hello szükséges konfigurációs adatokat kell hello SAP vagy az Azure ügyféloldali toorun SAP fekvő hibrid ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5a983-210">hello paper lists and describes all hello necessary configuration information you’ll need on hello SAP/Azure side toorun a hybrid SAP landscape.</span></span> <span data-ttu-id="5a983-211">Tooensure magas rendelkezésre állású rendszerek SAP NetWeaver-alapú infrastruktúra-szolgáltatási vehet igénybe intézkedések is tartoznak.</span><span class="sxs-lookup"><span data-stu-id="5a983-211">Measures you can take tooensure high availability of SAP NetWeaver-based systems on IaaS are also covered.</span></span>

<span data-ttu-id="5a983-212">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-212">Updated: June 2017</span></span>

<span data-ttu-id="5a983-213">[Ez az útmutató itt található][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="5a983-213">[This guide can be found here][planning-guide]</span></span>

### <a name="high-availability-configurations-of-sap-netweaver-in-azure-vms"></a><span data-ttu-id="5a983-214">SAP NetWeaver az Azure virtuális gépeken, magas rendelkezésre állási konfigurációban</span><span class="sxs-lookup"><span data-stu-id="5a983-214">High Availability configurations of SAP NetWeaver in Azure VMs</span></span>
<span data-ttu-id="5a983-215">cím: virtuális gépek magas rendelkezésre állás a SAP NetWeaver aaaAzure</span><span class="sxs-lookup"><span data-stu-id="5a983-215">title: aaaAzure Virtual Machines high availability for SAP NetWeaver</span></span>

<span data-ttu-id="5a983-216">Összefoglalás: Ebben a dokumentumban azt lépésének hello, hogy elvégezhető toodeploy magas rendelkezésre állású SAP rendszerek az Azure-ban hello Azure Resource Manager telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="5a983-216">Summary: In this document, we cover hello steps that you can take toodeploy high-availability SAP systems in Azure by using hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="5a983-217">A Microsoft végigvezetik Önt a nagyobb feladatok.</span><span class="sxs-lookup"><span data-stu-id="5a983-217">We walk you through these major tasks.</span></span> <span data-ttu-id="5a983-218">A hello dokumentum azt írja le hogyan egyetlen ponton-az-hibát jelentő összetevők például a fejlett üzleti alkalmazások fejlesztői (ABAP) SAP központi szolgáltatások (ASC) / SAP központi szolgáltatások (SCS) és az adatbázis-kezelő rendszerek (DBMS) és a redundáns összetevők, például SAP Alkalmazáskiszolgáló védelmét, ha az Azure virtuális gépeken futó toobe fog.</span><span class="sxs-lookup"><span data-stu-id="5a983-218">In hello document, we describe how single-point-of-failure components like Advanced Business Application Programming (ABAP) SAP Central Services (ASCS)/SAP Central Services (SCS) and database management systems (DBMS), and redundant components like SAP Application Server are going toobe protected when running in Azure VMs.</span></span> <span data-ttu-id="5a983-219">A részletes példa egy telepítési és a konfiguráció egy magas rendelkezésre állású SAP rendszer Windows Server feladatátvételi fürtszolgáltatási fürtben az Azure-ban bemutatott, és ebben a dokumentumban leírt.</span><span class="sxs-lookup"><span data-stu-id="5a983-219">A step-by-step example of an installation and configuration of a high-availability SAP system in a Windows Server Failover Clustering cluster in Azure is demonstrated and shown in this document.</span></span>

<span data-ttu-id="5a983-220">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-220">Updated: June 2017</span></span>

[<span data-ttu-id="5a983-221">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-221">This guide can be found here</span></span>](high-availability-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="realizing-multi-sid-deployments-of-sap-netweaver-in-azure-vms"></a><span data-ttu-id="5a983-222">SAP NetWeaver az Azure virtuális gépeken, bevezetett példányai Multi-SID megvalósítása</span><span class="sxs-lookup"><span data-stu-id="5a983-222">Realizing Multi-SID deployments of SAP NetWeaver in Azure VMs</span></span>
<span data-ttu-id="5a983-223">cím: aaaCreate egy SAP NetWeaver multi-SID-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="5a983-223">title: aaaCreate an SAP NetWeaver multi-SID configuration</span></span> 

<span data-ttu-id="5a983-224">Összefoglalás: Ez a dokumentum egy hozzáadása toohello dokumentum magas rendelkezésre állás a SAP NetWeaver Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="5a983-224">Summary: This document is an addition toohello document High availability for SAP NetWeaver on Azure VMs.</span></span> <span data-ttu-id="5a983-225">Miatt toonew funkcióit tartalmazza, a kapott a következő 2016 szeptemberétől Azure-ban akkor több SAP NetWeaver ASC/SCS példánya Azure virtuális gépek két lehetséges toodeploy.</span><span class="sxs-lookup"><span data-stu-id="5a983-225">Due toonew functionality in Azure that got introduced in September 2016, it is possible toodeploy multiple SAP NetWeaver ASCS/SCS instances in a pair of Azure VMs.</span></span> <span data-ttu-id="5a983-226">Ilyen konfiguráció csökkentheti a virtuális gépek szükséges toodeploy toorealize magas rendelkezésre állású SAP NetWeaver konfigurációk hello száma.</span><span class="sxs-lookup"><span data-stu-id="5a983-226">With such a configuration, you can reduce hello number of VMs necessary toodeploy toorealize highly available SAP NetWeaver configurations.</span></span> <span data-ttu-id="5a983-227">az útmutató hello hello ilyen multi-SID-konfigurációk telepítése.</span><span class="sxs-lookup"><span data-stu-id="5a983-227">hello guide describes hello setup of such multi-SID configurations.</span></span>

<span data-ttu-id="5a983-228">Frissített: 2016. December.</span><span class="sxs-lookup"><span data-stu-id="5a983-228">Updated: December 2016</span></span>

[<span data-ttu-id="5a983-229">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-229">This guide can be found here</span></span>](high-availability-multi-sid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <span data-ttu-id="5a983-230"><a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Az Azure virtuális gépeken SAP NetWeaver központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5a983-230"><a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Deployment of SAP NetWeaver in Azure VMs</span></span>
<span data-ttu-id="5a983-231">cím: aaaAzure SAP NetWeaver virtuális gépek központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5a983-231">title: aaaAzure Virtual Machines deployment for SAP NetWeaver</span></span>

<span data-ttu-id="5a983-232">Összefoglalás: A jelen dokumentum SAP NetWeaver szoftver toovirtual gépek az Azure-ban telepítése eljárási útmutatót biztosít.</span><span class="sxs-lookup"><span data-stu-id="5a983-232">Summary: This document provides procedural guidance for deploying SAP NetWeaver software toovirtual machines in Azure.</span></span> <span data-ttu-id="5a983-233">Három adott központi telepítési forgatókönyve esetén hangsúlyt hello Azure figyelési bővítmények az SAP, például hibaelhárítási hello Azure figyelési kiterjesztéseinek SAP javaslatok engedélyezése a dokumentum összpontosít.</span><span class="sxs-lookup"><span data-stu-id="5a983-233">This paper focuses on three specific deployment scenarios, with an emphasis on enabling hello Azure Monitoring Extensions for SAP, including troubleshooting recommendations for hello Azure Monitoring Extensions for SAP.</span></span> <span data-ttu-id="5a983-234">A dokumentum azt feltételezi, hogy elolvasta hello tervezési és megvalósítási útmutató.</span><span class="sxs-lookup"><span data-stu-id="5a983-234">This paper assumes that you’ve read hello planning and implementation guide.</span></span>

<span data-ttu-id="5a983-235">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-235">Updated: June 2017</span></span>

<span data-ttu-id="5a983-236">[Ez az útmutató itt található][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="5a983-236">[This guide can be found here][deployment-guide]</span></span>

### <span data-ttu-id="5a983-237"><a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>Adatbázis-kezelő telepítési útmutatója</span><span class="sxs-lookup"><span data-stu-id="5a983-237"><a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>DBMS deployment guide</span></span>
<span data-ttu-id="5a983-238">cím: aaaAzure SAP NetWeaver virtuális gépek DBMS központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5a983-238">title: aaaAzure Virtual Machines DBMS deployment for SAP NetWeaver</span></span>

<span data-ttu-id="5a983-239">Összefoglalás: A dokumentum tervezési és megvalósítási szempontjai hello SAP együtt kell futtatnia az adatbázis-kezelő rendszerek ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5a983-239">Summary: This paper covers planning and implementation considerations for hello DBMS systems that should run in conjunction with SAP.</span></span> <span data-ttu-id="5a983-240">Hello első részében általános szempontok felsorolása és jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5a983-240">In hello first part, general considerations are listed and presented.</span></span> <span data-ttu-id="5a983-241">hello hello papír következő részei vonatkoznak az SAP által támogatott Azure-ban különböző DBMS toodeployments.</span><span class="sxs-lookup"><span data-stu-id="5a983-241">hello following parts of hello paper relate toodeployments of different DBMS in Azure that are supported by SAP.</span></span> <span data-ttu-id="5a983-242">Különböző DBMS jelenik meg a következők: SQL Server, SAP ASE és Oracle.</span><span class="sxs-lookup"><span data-stu-id="5a983-242">Different DBMS presented are SQL Server, SAP ASE, and Oracle.</span></span> <span data-ttu-id="5a983-243">Az adott részeket, amikor SAP rendszert használ az Azure-on, az adott adatbázis-kezelő együtt a tooaccount rendelkezik szempontok ismertetése.</span><span class="sxs-lookup"><span data-stu-id="5a983-243">In those specific parts, considerations you have tooaccount for when you are running SAP systems on Azure in conjunction with those DBMS are discussed.</span></span> <span data-ttu-id="5a983-244">Témák, például az Azure különböző DBMS jelennek meg az SAP-alkalmazásokból hello használati hello által támogatott biztonsági mentési és magas rendelkezésre állású módszerek.</span><span class="sxs-lookup"><span data-stu-id="5a983-244">Subjects like backup and high availability methods that are supported by hello different DBMS on Azure are presented for hello usage with SAP applications.</span></span>

<span data-ttu-id="5a983-245">Frissített: 2017. június.</span><span class="sxs-lookup"><span data-stu-id="5a983-245">Updated: June 2017</span></span>

<span data-ttu-id="5a983-246">[Ez az útmutató itt található][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="5a983-246">[This guide can be found here][dbms-guide]</span></span>

### <a name="using-azure-site-recovery-for-sap-workload"></a><span data-ttu-id="5a983-247">Azure Site Recovery segítségével SAP-munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="5a983-247">Using Azure Site Recovery for SAP workload</span></span>
<span data-ttu-id="5a983-248">cím: aaaSAP NetWeaver: egy vész-helyreállítási megoldást az Azure Site Recovery szolgáltatással építése</span><span class="sxs-lookup"><span data-stu-id="5a983-248">title: aaaSAP NetWeaver: Building a Disaster Recovery Solution with Azure Site Recovery</span></span> 

<span data-ttu-id="5a983-249">Összefoglalás: Ez a dokumentum ismerteti a hello módja hogyan az Azure Site Recovery services hello célra vész-helyreállítási eljárással kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="5a983-249">Summary: This document describes hello way how Azure Site Recovery services can be used for hello purpose of handling disaster recovery scenarios.</span></span> <span data-ttu-id="5a983-250">Esetekben, amikor Azure használják vész-helyreállítási helyként egy helyszíni SAP fekvő Azure Site Recovery Services használatával.</span><span class="sxs-lookup"><span data-stu-id="5a983-250">Cases where Azure is used as disaster recovery location for an on-premise SAP landscape using Azure Site Recovery Services.</span></span> <span data-ttu-id="5a983-251">Hello dokumentumban ismertetett egy másik helyzet lehet, hello következőkre Azure (A2A) katasztrófa utáni helyreállítás eset, és hogyan kezeli az Azure Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="5a983-251">Another scenario described in hello document is hello Aure-to-Azure (A2A) disaster recovery case and how it is managed with Azure Site Recovery.</span></span>  

<span data-ttu-id="5a983-252">Frissített: 2017. augusztus.</span><span class="sxs-lookup"><span data-stu-id="5a983-252">Updated: August 2017</span></span>

[<span data-ttu-id="5a983-253">Ez az útmutató itt található</span><span class="sxs-lookup"><span data-stu-id="5a983-253">This guide can be found here</span></span>](http://aka.ms/asr-sap)

