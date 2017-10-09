---
title: "aaaLinux és nyílt forráskódú számítási az Azure-on |} Microsoft Docs"
description: "Felsorolja a Linux és nyílt forráskódú számítási cikkek Azure Linux alapvető használati, beleértve néhány alapvető fogalmait fut, vagy az Azure és az egyéb olyan tartalmat, az egyes technológiák és optimalizálás Linux képek feltöltése."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: a7e608b5-26ea-41e0-b46b-1a483a257754
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/27/2016
ms.author: rasquill
ms.openlocfilehash: 3ce0dcc65f28d0dddb29f654409f6dae56213bc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="linux-and-open-source-computing-on-azure"></a>Nyílt forráskódú és Linux-megoldások az Azure-ban
Minden hello dokumentáció toocreate kell, és a Linux-alapú virtuális gépeket hello klasszikus üzembe helyezési modellben található.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

## <a name="get-started"></a>Első lépések
* [Bevezetés az Azure-on Linux](intro-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [A gyakran ismételt kérdések kapcsolatos Azure virtuális gépek hello klasszikus telepítési modellel létrehozott](classic/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A virtuális gépek képek kapcsolatos](../windows/classic/about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A saját Distro lemezkép feltöltése](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) (és a vonatkozó utasításokat is használ egy [Azure-Endorsed terjesztési](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json))
* [Jelentkezzen be tooa Linux virtuális gép használatával hello a klasszikus Azure portálon](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="set-up"></a>Beállítás
* [Telepítse az Azure parancssori felület (Azure CLI)](../../cli-install-nodejs.md)

## <a name="tutorials"></a>oktatóanyagokat
* [Hello LÁMPA verem telepíthető egy Linux virtuális gép az Azure-ban](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ruby sínek webalkalmazás egy Azure virtuális gépen](classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
* [Útmutató: telepítés Apache Qpid Proton-C AMQP és a Service Bus](../../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Adatbázisok
* [Az Azure-beli MySQL teljesítményének optimalizálása](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MySQL-fürtök](classic/mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Cassandra Linux Azure-on futó, és azt elérő Node.js-ből](classic/cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A több főkiszolgálós MariaDbs-fürt létrehozása](classic/mariadb-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="hpc"></a>HPC
* [Ismerkedés az Azure-ban HPC Pack-fürtben lévő Linux számítási csomópontok](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A Microsoft HPC Pack NAMD futó Linux számítási csomópontok az Azure-ban](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A Linux RDMA fürt toorun MPI alkalmazások beállítása](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="docker"></a>Docker
* [Hello Docker Virtuálisgép-bővítmény a hello Azure parancssori felület (CLI) használatával](classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hello Docker Virtuálisgép-bővítmény a hello Azure-portál használatával](classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hogyan toouse docker-gép az Azure-on](docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="ubuntu"></a>Ubuntu
* [Útmutató: a MySQL-fürtök](classic/mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hogyan: Node.js és Cassandra](classic/cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="opensuse"></a>OpenSUSE
* [Hogyan: telepítheti és futtathatja a MySQL](classic/mysql-on-opensuse.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="coreos"></a>CoreOS
* [Útmutató: Azure CoreOS használata](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="planning"></a>Tervezés
* [Az Azure infrastruktúra-szolgáltatások megvalósítási irányelvei](../windows/infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Linux-felhasználónevek kiválasztása](usernames.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hogyan tooconfigure rendelkezésre állási készlet hello klasszikus üzembe helyezési modellben lévő virtuális gépek](../windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hogyan tooSchedule Azure virtuális gépek tervezett karbantartása](classic/planned-maintenance-schedule.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hello virtuális gépek rendelkezésre állásának kezelése](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [A Linux virtuális gépek Azure-ban tervezett karbantartás](planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="deployment"></a>Környezet
* [Egyéni Linuxot futtató virtuális gép létrehozása](../windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [hello alapjai: a Linux virtuális gép tooMake sablon rögzítése](classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Nem támogatott Disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="management"></a>Kezelés
* [SSH](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hogyan tooReset jelszó vagy SSH tulajdonságok Linux](classic/reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Legfelső szintű használ](use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="azure-resources"></a>Azure-erőforrások
* [hello Azure Linux ügynök](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Az Azure Virtuálisgép-bővítmények és szolgáltatások](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Egyéni adatok hogy egy virtuális gép toouse a felhő inicializálás](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>Storage
* [Egy adatlemez tooa Linux virtuális gép csatlakoztatása](../windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [A Linux virtuális gép adatlemez leválasztása](classic/detach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Hálózat
* [Hogyan tooset be a klasszikus virtuális gépen az Azure-végpontok](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="troubleshooting"></a>Hibaelhárítás
* [Secure Shell (SSH) kapcsolatok tooa Linux-alapú Azure virtuális gép hibakeresése](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Egy új Linux virtuális gép létrehozása az Azure klasszikus üzembe helyezési problémáinak elhárítása](classic/troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)  
* [Klasszikus üzembe helyezési elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure-ban](../windows/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) 

## <a name="reference"></a>Referencia
* [Az Azure parancssori felület parancsait Azure szolgáltatásfelügyelet (asm) módban](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure szolgáltatásfelügyelet REST API-n](https://msdn.microsoft.com/library/azure/ee460799.aspx)

## <a name="general-links"></a>Általános hivatkozások
a következő hivatkozások hello Microsoft blogok, Technet lapokat, és külső webhelyek helyett az Azure.com webhelyre dokumentáció a fenti vannak. Az Azure és a hello nyílt forráskódú informatika világában, amelyek gyorsan változó célozza, már majdnem bizonyos, amely a következő hivatkozások hello elavultak, *annak ellenére, hogy* hello tényt, hogy mi a legjobb toocontinually kell újabb témakörök hozzáadása és eltávolítása elavult néhányat a meglévők közül. Ha azt korábban nem talált meg egy, adjon ossza meg velünk tudja hello megjegyzésekben, vagy küldje el a lekérési kérelem tooour [GitHub-tárház](https://github.com/Azure/azure-content/).

* [ASP.NET 5 futó Linux Docker-tárolók használata](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
* [Hogyan tooDeploy OpenLogic a CentOS Virtuálisgép-lemezkép](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
* [SUSE frissítési infrastruktúra](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
* [SUSE Linux Enterprise Server SAP felhő készülék könyvtára](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
* [Magas rendelkezésre állású Linux felépítése 12 lépéseket az Azure-on](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
* [Az Azure szolgáltatásban az Azure parancssori felület, node.js, jhawk kiépítése Linux automatizálásához](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
* [Az Azure-on GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
* [Azure-beli freebsd rendszerű](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
* [Freebsd rendszerű egyszerű telepítése](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
* [A testre szabott freebsd rendszerű lemezképek központi telepítése](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
* [Kaspersky AV Linux rendszerű fájlkiszolgáló](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL
* [Az Azure-8 nyílt forráskódú NoSql-adatbázisok](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
* [Slideshare (MSOpenTech): Az Azure-on CouchDb élmény](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
* [CouchDB,--szolgáltatás a node.js, a CORS és Grunt fut](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
* [A Windows hello Azure Redis Cache szolgáltatás a redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
* [ASP.NET-munkamenetállapot-szolgáltató az Redis előzetes bejelentése](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
* [Blog: RavenHQ most elérhető hello Azure piactéren](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big Data
* [Hadoop telepítése Azure Linux virtuális gépeken](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
* [Az Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relációs adatbázis
* [A Microsoft Azure-ban MySQL magas rendelkezésre állás architektúra](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
* [Corosync, ILB használatával pg_bouncer Postgres telepítése](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux nagy teljesítményű számítástechnikai (HPC)
* [Gyors üzembe helyezés sablon: SLURM fürt léptetési](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (és [blogbejegyzés](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
* [Gyors üzembe helyezés sablon: Linux számítási csomópontok HPC-fürt létrehozása](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, felügyeleti és optimalizálása
Hello world devops, felügyeleti és optimalizálási meglehetősen kiterjedtnek lesz, és nagyon gyorsan módosítása, vegye figyelembe a kiindulási pont az alábbi hello listából.

* [Videó: Az Azure virtuális gépek: használatával Chef, Puppet és a Linux virtuális gépek kezelése a Docker](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)
* [Az útmutató tooautomated Kubernetes fürttelepítés CoreOS és nagy befejezése](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
* [Kubernetes megjelenítő](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Az alárendelt Jenkins beépülő modul az Azure-bA](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
* [GitHub-tár: Jenkins tároló beépülő modul Azure-hoz](https://github.com/jenkinsci/windows-azure-storage-plugin)
* [Külső gyártó: Hudson alárendelt beépülő modul Azure-hoz](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
* [Külső gyártó: Hudson tároló beépülő modul Azure-hoz](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Videó: Mi az Chef, és hogyan működik?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)
* [Videó: Hogyan tooUse Azure automatizálása a Linux virtuális gépek](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* [Blog: Hogyan toodo Powershell DSC Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
* [GitHub: DSC Docker-ügyfelekhez](https://github.com/anweiss/DockerClientDSC)
* [Az Azure-bA a csomagoló beépülő modul](https://github.com/msopentech/packer-azure)

