---
title: "az Azure-ban fürt aaaSubmit feladatok tooan HPC Pack |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be egy helyi számítógép toosubmit feladatok tooan HPC Pack fürt az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 2918cf633917d8730487152e6a5ddb863eb8bb5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-tooan-hpc-pack-cluster-deployed-in-azure"></a>A helyi számítógép tooan HPC Pack fürtök Azure szolgáltatásba telepített HPC feladatok elküldéséhez
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Konfigurálja a helyszíni ügyfél számítógép toosubmit feladatok tooa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) fürt az Azure-ban. Ez a cikk bemutatja, hogyan tooset ügyféllel a helyi számítógép eszközök keresztül HTTPS típusú fürt toohello az Azure-ban toosubmit feladat. Ezzel a módszerrel több fürt felhasználók elküldheti a feladatok tooa felhőalapú HPC Pack fürt, de Csatlakozás közvetlenül toohello átjárócsomópont VM vagy Azure-előfizetéssel való hozzáférés nélkül.

![Küldje el a feladat tooa fürt az Azure-ban][jobsubmit]

## <a name="prerequisites"></a>Előfeltételek
* **Egy Azure virtuális Gépen telepített HPC Pack átjárócsomópont** -azt javasoljuk, hogy az automatikus eszközeit használja, mint egy [Azure gyors üzembe helyezés sablon](https://azure.microsoft.com/documentation/templates/) vagy egy [Azure PowerShell-parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toodeploy hello átjárócsomópont és fürt. Hello átjárócsomópont hello DNS-nevét és hello hitelesítő adatait. Ez a cikk hello végrehajtásához a fürt rendszergazdája van szüksége.
* **Ügyfélszámítógép** -HPC Pack ügyfél segédprogramok futtatható Windows vagy Windows Server ügyfél számítógépre van szüksége (lásd: [rendszerkövetelmények](https://technet.microsoft.com/library/dn535781.aspx)). Ha csak szeretné toouse hello HPC Pack webes portál vagy a REST API toosubmit feladatokat, az Ön által választott bármely ügyfélszámítógép is használhatja.
* **HPC Pack telepítési adathordozó** -tooinstall hello HPC Pack ügyfél segédprogramok hello szabad telepítési csomag, a HPC Pack (HPC Pack 2012 R2) legújabb verziója érhető el a [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Győződjön meg arról, hogy töltse le a hello hello átjárócsomópont VM telepített HPC Pack ugyanazt a verzióját.

## <a name="step-1-install-and-configure-hello-web-components-on-hello-head-node"></a>1. lépés: A telepítésével és beállításával hello webösszetevők hello átjárócsomópont
tooenable REST felület toosubmit feladatok toohello fürt a HTTPS PROTOKOLLOKON keresztül győződjön meg arról, hogy hello HPC Pack webösszetevők hello HPC Pack átjárócsomópont van konfigurálva. Ha még nincsenek telepítve, először telepítenie hello webösszetevők hello HpcWebComponents.msi telepítési fájl futtatásával. Ezt követően konfigurálja a hello összetevők hello HPC PowerShell parancsfájl futtatásával **Set-HPCWebComponents.ps1**.

Szükséges részletes eljárásokért lásd: [hello Microsoft HPC Pack webes összetevők telepítése](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> HPC Pack bizonyos Azure gyors üzembe helyezési sablonokat telepítse, és hello webösszetevők automatikusan konfigurálja. Ha hello [HPC Pack IaaS telepítési parancsfájl](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toocreate hello fürt, lehetősége van telepítése és konfigurálása hello webösszetevők hello központi telepítésének részeként.
> 
> 

**tooinstall hello webösszetevők**

1. Csatlakozás toohello átjárócsomópont VM hello hitelesítő adatok a fürt rendszergazdai fiók használatával.
2. Hello HPC Pack telepítési mappából futtassa a HpcWebComponents.msi hello központi csomóponton.
3. Hello kövesse hello varázsló tooinstall hello webösszetevők

**tooconfigure hello webösszetevők**

1. Hello központi csomóponton indítsa el a HPC PowerShell rendszergazdaként.
2. toochange directory toohello helye hello konfigurációs parancsfájl típus hello a következő parancsot:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. tooconfigure hello REST-felületen, és indítsa el a hello HPC webes szolgáltatás, a következő parancs típusa hello:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. Amikor felszólító tooselect tanúsítvány, válasszon hello tanúsítványt, amely megfelel a hello átjárócsomópont toohello nyilvános DNS-nevét. Például, ha Ön átjárócsomópont hello hello klasszikus üzembe helyezési modellel, tanúsítvány neve hello használó virtuális gépek néz CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Hello Resource Manager üzembe helyezési modellben használatakor hello tanúsítvány neve néz CN =&lt;*HeadNodeDnsName*&gt;.&lt; *régió*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > Újabb beállítást választja ezt a tanúsítványt a helyi számítógépről feladatok toohello átjárócsomópont elküldésekor. Ne válasszon, vagy adja meg egy tanúsítványt, amely megfelel a toohello számítógépneve hello átjárócsomópont hello Active Directory-tartományban (például: CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
5. tooconfigure hello webes portál a feladat elküldése, típus hello a következő parancsot:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Hello parancsfájl befejezése után állítsa le és indítsa újra a HPC Job Feladatütemező szolgáltatás hello hello a következő parancsok beírásával:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-hello-hpc-pack-client-utilities-on-an-on-premises-computer"></a>2. lépés: Hello HPC Pack ügyfél segédeszközök telepítése a helyi számítógépen
Ha azt szeretné, tooinstall hello HPC Pack ügyfél segédprogramok a számítógépen, le a HPC Pack telepítési fájlok (teljes telepítés) hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Ha hello a telepítés megkezdéséhez válassza hello telepítési beállítással a hello **HPC Pack ügyfél segédprogramok**.

toouse hello HPC Pack ügyféleszközök toosubmit feladatok toohello. átjárócsomópontjához VM, akkor is tooexport hello átjárócsomópont tanúsítványt kell, és telepítse azt hello ügyfélszámítógép. hello tanúsítványt kell lennie. CER formátumú.

**hello átjárócsomópont tooexport hello tanúsítványt**

1. Hello átjárócsomópont vegye fel hello tanúsítványok beépülő modul tooa a Microsoft Management Console hello helyi számítógépfiók számára. Tooadd hello beépülő modul lépéseiért lásd: [adja hozzá a tanúsítványok beépülő modul tooan hello MMC](https://technet.microsoft.com/library/cc754431.aspx).
2. A hello a konzolfán bontsa ki a **tanúsítványok – helyi számítógép** > **személyes**, és kattintson a **tanúsítványok**.
3. Keresse meg a hello tanúsítvány hello HPC Pack webösszetevők a beállított [1. lépés: Telepítse és a webes összetevők hello konfigurálása a hello átjárócsomópont](#step-1:-install-and-configure-the-web-components-on-the-head-node) (például CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
4. Kattintson a jobb gombbal a hello tanúsítványt, és kattintson a **feladataival** > **exportálása**.
5. Hello Tanúsítványexportáló varázslóban, kattintson **következő**, és győződjön meg arról, hogy **nem, nem exportálja a titkos kulcs hello** van kiválasztva.
6. Hajtsa végre hello hátralévő lépéseket hello varázsló tooexport hello tanúsítvány DER kódolású bináris X.509 (. CER) formátumban.

**tooimport hello tanúsítvány hello ügyfélszámítógépen**

1. Másolja a hello mappából hello átjárócsomópont tooa hello ügyfélszámítógépen exportált tanúsítványt.
2. Hello ügyfélszámítógépen certmgr.msc futtatásához.
3. A tanúsítvány-kezelőben bontsa ki a **tanúsítványok – aktuális felhasználó** > **megbízható legfelső szintű hitelesítésszolgáltatók**, kattintson a jobb gombbal **tanúsítványok**, és kattintson a **feladataival** > **importálási**.
4. Hello Tanúsítványimportáló varázslóban, kattintson **következő** és kövesse hello lépéseket tooimport hello exportált tanúsítványt a megbízható legfelső szintű hitelesítésszolgáltatók tárolására hello átjárócsomópont toohello.

> [!TIP]
> Láthatja a biztonsági figyelmeztetést, mert hello hitelesítésszolgáltatójától hello átjárócsomópont nem ismerik fel a hello ügyfélszámítógép. Tesztelési célokra, figyelmen kívül hagyhatja a figyelmeztetési és a teljes hello tanúsítvány importálása.
> 
> 

## <a name="step-3-run-test-jobs-on-hello-cluster"></a>3. lépés: Futtatás Tesztfeladatok hello fürtön
az Azure-ban a hello a fürt a konfigurációt, próbálkozzon egy futó feladat hello tooverify a helyi számítógépen. Használhatja például a HPC Pack grafikus eszközöket vagy a parancssori parancsokat toosubmit feladatok toohello fürt. Egy webes portál toosubmit feladatok is használható.

**toorun feladat elküldése parancsok hello ügyfélszámítógépen**

1. Egy ügyfélszámítógépen, amelyen telepítve vannak-e az hello HPC Pack ügyfél segédprogramok egy parancssor elindításához.
2. Írja be egy minta parancsot. Toolist hello fürtön futó összes feladatot írja be például a parancs a következő, attól függően, hogy a teljes DNS-nevét hello hello átjárócsomópont hello hasonló tooone:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    vagy
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Teljes DNS-nevét hello hello átjárócsomópont, ne hello IP-címet, hello Feladatütemező URL-címet használja. Ha hello IP-címet ad meg, egy hibaüzenet jelenik meg, hasonló túl "hello tanúsítvány szükséges a tooeither egy érvényes lánc megbízhatósági vagy hello megbízható főtanúsítványainak tárolójába helyezi toobe rendelkezik."
   > 
   > 
3. Amikor a rendszer kéri, írja be a hello felhasználónevet (hello formában &lt;tartománynév&gt;\\&lt;felhasználónév&gt;) és a jelszó hello HPC-fürt rendszergazdája vagy egy másik fürthöz felhasználó konfigurált. Választhat toostore hello hitelesítő adatok helyben több feladat műveletet.
   
    Feladatok listája jelenik meg.

**a HPC Job Manager toouse hello ügyfélszámítógépen**

1. Ha korábban egy fürt felhasználó tartományi hitelesítő adatok nem tárolja, amikor elküld egy feladatot, a hitelesítőadat-kezelőben hello hitelesítő adatokat adhat hozzá.
   
    a. A Vezérlőpult hello ügyfélszámítógépen indítsa el a hitelesítőadat-kezelő.
   
    b. Kattintson a **Windows hitelesítő adatok** > **egy általános hitelesítő adatok hozzáadása**.
   
    c. Adja meg a hello internetcím (például https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler vagy a https://&lt;HeadNodeDnsName&gt;.&lt; régió&gt;.cloudapp.azure.com/HpcScheduler), és hello felhasználónevét (&lt;tartománynév&gt;\\&lt;felhasználónév&gt;) és a jelszó hello fürt rendszergazdája vagy egy másik fürt felhasználói konfigurált.
2. Hello ügyfélszámítógépen indítsa el a HPC Job Manager használatát.
3. A hello **Head csomópont kiválasztása** párbeszédpanelen típus hello URL-cím toohello átjárócsomópont az Azure-ban (például https://&lt;HeadNodeDnsName&gt;. cloudapp.net vagy a https://&lt;HeadNodeDnsName&gt;. &lt;régió&gt;. cloudapp.azure.com).
   
    A HPC Job Manager megnyílik, és hello központi csomóponton lévő feladatok listáját jeleníti meg.

**hello központi csomóponton futó toouse hello webportál**

1. Indítsa el a webböngésző hello ügyfélszámítógépen, és adja meg a következő címeket, attól függően, hogy a teljes DNS-nevét hello hello átjárócsomópont hello egyikét:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    vagy
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Hello biztonsági megjelenő párbeszédpanelen írja be a HPC-fürt rendszergazdája hello hello tartományi hitelesítő adatait. (Is hozzáadhat más fürt felhasználók különböző szerepkörök. Lásd: [fürt felhasználók kezelése](https://technet.microsoft.com/library/ff919335.aspx).)
   
    hello webportál toohello feladat lista nézetének megnyitása.
3. egy minta feladatot, amely karakterláncot ad vissza, hello "Hello World" hello fürtből toosubmit kattintson **új feladat** hello bal oldali navigációs sáv.
4. A hello **új feladat** lap **küldésének lapjáról**, kattintson a **HelloWorld**. hello feladat elküldése lap jelenik meg.
5. Kattintson a **nyújt**. Ha a rendszer kéri, adja meg a HPC-fürt rendszergazdája hello hello tartományi hitelesítő adatok. hello feladatot küld, és hello Feladatazonosító jelenik meg hello **saját feladatok** lap.
6. tooview hello eredmények hello feladat küldő, kattintson hello Feladatazonosító, majd **nézet feladatai** tooview hello parancs kimenetében (alatt **kimeneti**).

## <a name="next-steps"></a>Következő lépések
* Is elküldheti a feladatok toohello hello Azure fürt [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Ha azt szeretné, hogy a Linux-ügyfél toosubmit fürt feladatait, tekintse meg a hello Python hello a minta [HPC Pack 2012 R2 SDK és mintakód](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
