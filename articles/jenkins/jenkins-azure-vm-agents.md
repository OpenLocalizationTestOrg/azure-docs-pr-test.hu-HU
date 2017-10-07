---
title: "Azure virtuális gép ügynökök aaaUse Jenkins folyamatos integrációját."
description: "Azure virtuálisgép-ügynökök használata Jenkins alárendelt csomópontokként."
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 2388e6919d0280372166fbd325d80dafb00d7550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Azure virtuálisgép-ügynökök használata a Jenkins szolgáltatással fenntartott folyamatos integrációhoz.

A gyors üzembe helyezés bemutatja, hogyan toouse hello Jenkins Azure VM ügynökök beépülő modul toocreate az igény szerinti Linux (Ubuntu) ügynök az Azure-ban.

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezés:

* Ha még nem rendelkezik egy Jenkins master, Kezdésként használhatja az hello [megoldás sablon](install-jenkins-solution-template.md) 
* Tekintse meg a túl[hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) Ha még nem rendelkezik egy egyszerű Azure szolgáltatást.

## <a name="install-azure-vm-agents-plugin"></a>Azure-beli virtuálisgép-ügynökhöz tartozó beépülő modul telepítése

Ha később elindítják a hello [Megoldássablonban](install-jenkins-solution-template.md), hello Azure Virtuálisgép-ügynök beépülő modul telepítve van-e hello Jenkins fő.

Ellenkező esetben a hello telepítése **Azure VM ügynökök** hello Jenkins irányítópult belül a beépülő modul.

## <a name="configure-hello-plugin"></a>Hello beépülő modul konfigurálása

* Belül hello Jenkins irányítópultján kattintson **kezelése Jenkins -> rendszer beállítása ->**. Toohello a hello lap alján görgessen és hello legördülő hello szakasz található **adja hozzá az új felhőalapú**. Hello menüben válassza ki a **Microsoft Azure virtuális gép ügynökök**
* Kiválaszthat egy meglévő fiókot hello Azure hitelesítő adatok legördülő menüből.  új tooadd **Microsoft Azure szolgáltatás egyszerű** adja meg a következő értékek hello: előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont.

![Azure-beli Hitelesítő adatok](./media/jenkins-azure-vm-agents/service-principal.png)

* Kattintson a **ellenőrizze konfigurációs** toomake meg arról, hogy hello-profil konfigurációjának helyességéről.
* Hello konfiguráció mentéséhez, és folytassa a következő lépés toohello.

## <a name="template-configuration"></a>Sablon konfigurálása

### <a name="general-configuration"></a>Általános konfiguráció
Ezután konfigurálja a sablont használja toodefine egy Azure Virtuálisgép-ügynök. 

* Kattintson a **Hozzáadás** tooadd egy sablont. 
* Adja meg az új sablon nevét. 
* Hello címkére írja be a "ubuntu." Ez a címke hello feladat konfigurálása során használatos.
* Válassza ki a kívánt régiót hello a hello kombinált lista.
* Jelölje be hello szükséges Virtuálisgép-méretet.
* Adja meg hello Azure Storage-fiók nevét, vagy hagyja üresen toouse hello alapértelmezett neve "jenkinsarmst."
* Adja meg a hello megőrzési időtartamot percben. Ez a beállítás határozza meg a hello hány perc Jenkins várja meg, amíg az üresjárati ügynök automatikusan törlése előtt. Adjon meg 0, ha nem szeretné, hogy üresjárati ügynökök toobe automatikusan törli.

![Általános konfiguráció](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Rendszerkép-konfiguráció

toocreate Linux (Ubuntu) ügynök, válassza ki **rendszerképet a referencia** és hello használja a következő konfigurációs példaként. Tekintse meg a túl[Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) hello legújabb Azure támogatott képek.

* Lemezkép kiadója: Canonical
* Lemezkép tartalma: UbuntuServer
* Lemezkép termékváltozat: 14.04.5-LTS
* Lemezkép verzió: legfrissebb
* Operációs rendszer típusa: Linux
* Indítási metódus: SSH
* Rendszergazdai hitelesítő adatok megadása
* A virtuálisgép-inicializáló parancsprogram indításához írja be:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Rendszerkép-konfiguráció](./media/jenkins-azure-vm-agents/image-config.png)

* Kattintson a **sablon ellenőrzése** tooverify hello konfigurációs.
* Kattintson a **Save** (Mentés) gombra.

## <a name="create-a-job-in-jenkins"></a>Feladat létrehozása a Jenkinsben

* Belül hello Jenkins irányítópultján kattintson **új elem**. 
* Adjon meg egy nevet, válassza ki a **Freestyle projekt** lehetőséget, majd kattintson az **OK** gombra.
* A hello **általános** lapon, jelölje be "Korlátozása, amelyben futtathatók a projekt" és "ubuntu" címke kifejezés típusa. Ekkor megjelenik a "ubuntu" hello legördülő.
* Kattintson a **Save** (Mentés) gombra.

![Feladat beállítása](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Új projekt kiépítése

* Lépjen vissza a toohello Jenkins irányítópult.
* Kattintson a jobb gombbal hello új feladatot létrehozni, majd kattintson a **Létrehozás most**. Megkezdődik a fordítási folyamat. 
* Hello build befejeződése után nyissa meg túl**a konzol kimeneti**. Láthatja, hogy hello build távolról lett végrehajtva Azure-on.

![Konzolkimenet](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Referencia

* Azure Friday videó: [Folyamatos integráció a Jenkins szolgáltatással Azure-beli virtuálisgép-ügynökök használatával](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Támogatási információ és konfigurációs lehetőségek: [Azure-beli virtuálisgép-ügynök Jenkins beépülő modul Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 

