---
title: "aaaRun Java-alkalmazáskiszolgáló a klasszikus Azure virtuális gép |} Microsoft Docs"
description: "Ebben az oktatóanyagban létre hello klasszikus üzembe helyezési modellel, és bemutatja hogyan erőforrást használ, egy Windows virtuális toocreate számítógéphez, és konfigurálja azt toorun Apache Tomcat alkalmazáskiszolgáló."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 2d9f586c9f628d3738522b320996b95b078d7454
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-java-application-server-on-a-virtual-machine-created-with-hello-classic-deployment-model"></a>Hogyan toorun egy Java-alkalmazáskiszolgáló virtuális gépen létre hello klasszikus telepítési modell
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. A Resource Manager sablon toodeploy Java 8 és Tomcat egy webalkalmazást, lásd: [Itt](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).

A virtuális gép tooprovide server funkcióinak használata az Azure-ban. Például egy Azure-on futó virtuális gép konfigurált toohost egy Java-alkalmazáskiszolgáló, például az Apache Tomcat is lehet.

Ez az útmutató befejezése után fog megértéséhez toocreate egy virtuális gépet az Azure-on fut, és konfigurálja a Java-alkalmazáskiszolgáló toorun. Ismerje meg lesz, és hajtsa végre a következő feladatok hello:

* Hogyan toocreate egy virtuális gépet, amely egy Java fejlesztői készlet (JDK) már telepítve van.
* Tooremotely hogyan tooyour virtuálisgép jelentkezni.
* Hogyan tooinstall egy Java-alkalmazáskiszolgáló--Apache Tomcat – a virtuális gépen.
* Hogyan toocreate egy végpontot a virtuális géphez.
* Hogyan tooopen hello egy portot az alkalmazás-kiszolgáló tűzfal.

hello befejeződött a virtuális gépen futó Tomcat telepítési eredményez.

![Apache Tomcat rendszerű virtuális gép][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a>a virtuális gépek toocreate
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).  
2. Kattintson a **új**, kattintson a **számítási**, majd kattintson a **láthatja az összes** hello a **a kiemelt alkalmazások**.
3. Kattintson a **JDK**, kattintson a **JDK 8** a hello **JDK** ablaktáblán.  
   Virtuálisgép-lemezképek támogató **JDK 6** és **JDK 7** érhető el, ha örökölt alkalmazásokat, amelyek nem kész toorun JDK 8-ban.
4. Hello JDK 8 ablaktáblában jelölje ki **klasszikus**, majd kattintson a **létrehozása**.
5. A hello **alapjai** panel:
   1. Adja meg a hello virtuális gép nevét.
   2. Adja meg egy nevet a hello rendszergazda hello **felhasználónév** mező. Ne feledje ezt a nevet, és ahhoz tartozó jelszót, a következő mező hello követő hello. Már szükség Amikor távolról jelentkeznek toohello virtuális gépen.
   3. Adjon meg egy jelszót hello **új jelszó** mezőben, majd adja meg újból hello **jelszó megerősítése** mező. Ezt a jelszót a rendszergazdai fiók hello szolgál.
   4. Jelölje be hello megfelelő **előfizetés**.
   5. A hello **erőforráscsoport**, kattintson a **hozzon létre új** és adja meg egy új erőforráscsoportot hello nevét. Vagy kattintson a **meglévő** , és válassza ki az elérhető erőforráscsoportok hello.
   6. Jelöljön ki egy helyet, ahol hello virtuális gép található, mint például **déli középső Régiójában**.
6. Kattintson a **Tovább** gombra.
7. A hello **virtuálisgép-lemezkép mérete** panelen válassza **A1 szabványos** vagy egy másik megfelelő lemezképet.
8. Kattintson a **Kiválasztás** gombra.

9. A hello **beállítások** panelen kattintson a **OK**. Azure által biztosított hello alapértelmezett értékeket is használhat.  
10. A hello **összegzés** panelen kattintson a **OK**.

## <a name="tooremotely-sign-in-tooyour-virtual-machine"></a>virtuális gép tooyour tooremotely bejelentkezés
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **virtuális gépek (klasszikus)**. Ha szükséges, kattintson a **további szolgáltatások** : hello bal alsó sarokban a hello szolgáltatás kategóriák. Hello **virtuális gépek (klasszikus)** bejegyzés szerepel hello **számítási** csoport.
3. Kattintson a kívánt toosign a hello virtuális gép hello nevére.
4. Miután elindult hello virtuális gép, hello hello ablaktábla tetején a menüben kapcsolatait fogadja.
5. Kattintson a **Connect** (Csatlakozás) gombra.
6. Válaszoljon toohello kérni fogja a szükséges tooconnect toohello virtuális gépként. Általában megnyitásakor vagy mentésekor hello .rdp fájlt, amely hello kapcsolódási adatait tartalmazza. Előfordulhat, hogy rendelkezik toocopy hello URL-cím: port hello hello .rdp fájl első sorának hello utolsó része, és illessze be a távoli bejelentkezési alkalmazás.

## <a name="tooinstall-a-java-application-server-on-your-virtual-machine"></a>a virtuális gép Java-alkalmazáskiszolgáló tooinstall
Másolhatja Java application server tooyour virtuális gépet, vagy a telepítő keresztül Java-alkalmazáskiszolgáló telepítése.

Ez az oktatóanyag hello Java application server tooinstall Tomcat használja.

1. Ha be van jelentkezve tooyour virtuális gép, nyissa meg a böngésző-munkamenet túl[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).
2. Kattintson duplán az hello hivatkozásának **32-bites vagy 64 bites Windows Service telepítőjét**. Ez a módszer használatával Tomcat telepíti egy Windows-szolgáltatás.
3. Amikor a rendszer kéri, válassza ki a toorun hello telepítő.
4. Hello belül **Apache Tomcat telepítő** varázslóban kövesse hello tooinstall Tomcat kéri. Ez az oktatóanyag hello céljából hello Alapértelmezések elfogadása rendben. Amikor eléri a hello **Apache Tomcat telepítővarázsló befejezése hello** párbeszédpanelen opcionálisan ellenőrizheti a **futtassa: Apache Tomcat** toohave Tomcat start most. Kattintson a **Befejezés** toocomplete hello Tomcat telepítési folyamat.

## <a name="toostart-tomcat"></a>toostart Tomcat

Nyissa meg a virtuális gép és a futó hello parancsot a parancssorba manuálisan is kezdeményezhető Tomcat **nettó&nbsp;start&nbsp;Tomcat8**.

Miután Tomcat fut, elérheti hello URL-cím megadásával Tomcat <8080> hello virtuális gép böngészőben.

toosee Tomcat külső gépeken futtatja, akkor toocreate egy olyan végpont szükséges, és nyissa meg egy portot.

## <a name="toocreate-an-endpoint-for-your-virtual-machine"></a>a virtuális gép végpont toocreate
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **virtuális gépek (klasszikus)**.
3. Kattintson a hello hello virtuális gép nevét, a Java-alkalmazáskiszolgáló futtató.
4. Kattintson a **Végpontok** elemre.
5. Kattintson az **Add** (Hozzáadás) parancsra.
6. A hello **végpont hozzáadása** párbeszédpanel:
   1. Adjon meg egy nevet a hello végpont; például **HttpIn**.
   2. Válassza ki **TCP** hello protokollhoz.
   3. Adja meg **80** hello nyilvános port.
   4. Adja meg **8080** hello magánhálózati port.
   5. Válassza ki **letiltott** hello fix IP-cím számára.
   6. Hello hozzáférés-vezérlési lista, hagyja üresen.
   7. Kattintson a hello **OK** tooclose hello párbeszédpanel gombra, és hello végpont létrehozásához.

## <a name="tooopen-a-port-in-hello-firewall-for-your-virtual-machine"></a>a virtuális gép hello tűzfal port tooopen
1. Tooyour virtuálisgép jelentkezni.
2. Kattintson a **Windows Start**.
3. Kattintson a **vezérlőpultot**.
4. Kattintson a **rendszer és biztonság**, kattintson a **Windows tűzfal**, és kattintson a **speciális beállítások**.
5. Kattintson a **bejövő szabályok**, és kattintson a **új szabály**.  
   ![Új bejövő szabály][NewIBRule]
6. A hello **szabálytípus**, jelölje be **Port**, és kattintson a **következő**.  
   ![Új bejövő szabály port][NewRulePort]
7. A hello **protokoll és portok** képernyőn válassza ki **TCP**, adja meg **8080-as** hello, **adott helyi port**, majd kattintson az **Következő**.  
  ![Új bejövő szabály][NewRuleProtocol]
8. A hello **művelet** képernyőn válassza ki **hello csatlakozás engedélyezése**, és kattintson a **következő**.
   ![Új bejövő szabály művelet][NewRuleAction]
9. A hello **profil** képernyőn **tartomány**, **titkos**, és **nyilvános** van kiválasztva, és kattintson **tovább** .
   ![Új bejövő szabály profil][NewRuleProfile]
10. A hello **neve** képernyőn, adjon meg egy nevet hello szabályhoz, például **HttpIn** (hello szabály neve nincs szükség toomatch hello végpont nevét, azonban), és kattintson a **Befejezés**.  
    ![Új bejövő szabály neve][NewRuleName]

Ezen a ponton a Tomcat webhely egy külső böngészőből megtekinthető legyen. A böngésző hello cím ablakban írja be a hello URL-cím  **http://*a\_DNS\_neve*. cloudapp.net**, ahol ***a\_DNS\_neve*** hello virtuális gép létrehozásakor megadott hello DNS-neve.

## <a name="application-lifecycle-considerations"></a>Alkalmazás életciklusa kapcsolatos szempontok
* Nem sikerült létrehozni a saját webalkalmazások archívumából (WAR) és toohello adja hozzá **webapps** mappa. Például egy alapszintű Java szolgáltatás lap (JSP) dinamikus webes projekt létrehozása és exportálni kell a WAR-fájlt. Ezután másolja hello WAR toohello Apache Tomcat **webapps** mappa hello virtuális gépen, majd futtassa a böngészőben.
* Alapértelmezés szerint hello Tomcat-szolgáltatás telepítve van, ha van állítva, akkor toostart manuálisan. Megváltoztathatja azt toostart automatikusan hello szolgáltatások beépülő modul használatával. Hello szolgáltatások beépülő modul indításához kattintson **Windows Start**, **felügyeleti eszközök**, majd **szolgáltatások**. Kattintson duplán a hello **Apache Tomcat** szolgáltatást, és állítsa **indítási típus** túl**automatikus**.

    ![A szolgáltatás toostart beállítása automatikusan][service_automatic_startup]

    Hello, hogy automatikusan elinduljon Tomcat előnye, hogy futásának indításakor hello virtuális gép újraindításakor, (például a számítógép újraindítása szükséges szoftverfrissítések telepítése után).

## <a name="next-steps"></a>Következő lépések
Érdemes lehet egyéb szolgáltatások (például az Azure Storage, service bus és SQL-adatbázis) megismerheti a Java-alkalmazások tooinclude. Rendelkezésre álló hello információk megtekintéséhez hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from hello "toocreate an ednpoint for your virtual mache" 3/17/2017,
     toouse hello new portal.
6. In hello **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In hello **New endpoint details** dialog box:
-->
