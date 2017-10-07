---
title: "Azure Active Directory-fürtöt aaaHPC Pack |} Microsoft Docs"
description: "Ismerje meg, hogyan toointegrate egy HPC Pack 2016 fürtön, az Azure Active Directoryval az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: 0806e544a468e27ca0567e18c55554811584fbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a>Az Azure-ban az Azure Active Directory egy HPC Pack fürt kezelése
[A Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) való integráció támogatja [Azure Active Directory](../../active-directory/index.md) (az Azure AD) a rendszergazdák, akik HPC Pack-fürt üzembe helyezése az Azure-ban.



Hajtsa végre a következő magas szintű feladatok hello a jelen cikk hello lépéseket: 
* A HPC Pack fürt manuálisan integrálása az Azure AD-bérlő
* Kezelése és az Azure-ban a HPC Pack fürt feladatok ütemezése 

Standard lépéseket toointegrate egy HPC Pack fürtmegoldás integrálása az Azure AD követi, más alkalmazásokhoz és szolgáltatásokhoz. Ez a cikk feltételezi, hogy jártas alapszintű felhasználói kezelése az Azure ad-ben. További információk és háttér: hello [Azure Active Directory dokumentációjának](../../active-directory/index.md) és hello a következő szakaszban.

## <a name="benefits-of-integration"></a>Integráció előnyei


Azure Active Directory (Azure AD) egy olyan több-bérlős felhőalapú címtár- és identitáskezelési felügyeleti szolgáltatás, amely az egyszeri bejelentkezés (SSO) hozzáférést biztosít a toocloud megoldásokat.

Integráció az Azure ad-val HPC Pack fürt segítségére lehetnek hello a következő célok érdekében:

* Hello hagyományos Active Directory-tartományvezérlőhöz eltávolítása hello HPC Pack fürtről. Ez csökkentheti a hello fürt karbantartásának, ha ez nem szükséges a vállalatba, illetve számlázhasson hello telepítési folyamatának hello költségeket.
* Kihasználhatja a következő előnyöket a Azure ad kerülnek hello:
    *   Egyszeri bejelentkezés 
    *   A helyi AD-azonosítójának használatával hello HPC Pack fürthöz az Azure-ban 

    ![Az Azure Active Directory-környezet](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a>Előfeltételek
* **Az Azure virtuális gépekre telepített HPC Pack 2016 fürt** - lépéseiért lásd: [HPC Pack 2016-fürt üzembe helyezése az Azure-ban](hpcpack-2016-cluster.md). Hello átjárócsomópont hello DNS-nevét és hello hitelesítő adatait. Ez a cikk hello végrehajtásához a fürt rendszergazdája van szüksége.

  > [!NOTE]
  > Az Azure Active Directory-integráció HPC Pack HPC Pack 2016 előtti verzióiban nem támogatott.



* **Ügyfélszámítógép** -olyan Windows vagy Windows Server ügyfél túl futtatása HPC Pack ügyfél segédprogramok van szüksége. Ha csak szeretné toouse hello HPC Pack webes portál vagy a REST API toosubmit feladatokat, az Ön által választott bármely ügyfélszámítógép is használhatja.

* **HPC Pack ügyfél segédprogramok** -hello hello szabad telepítési csomag elérhető a Microsoft Download Center hello használó ügyfélszámítógép hello HPC Pack ügyfél segédprogramokat telepíthet.


## <a name="step-1-register-hello-hpc-cluster-server-with-your-azure-ad-tenant"></a>1. lépés: Regisztrálja hello HPC-fürt kiszolgálót az Azure AD-bérlő
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Kattintson a **Active Directory** a bal oldali menü hello, és kattintson a kívánt címtár hello az előfizetésben. Rendelkeznie kell engedéllyel tooaccess erőforrások hello könyvtárban.
3. Kattintson a **felhasználók**, és győződjön meg arról, hogy a felhasználói fiókok már létrehozott vagy konfigurálva van.
4. Kattintson a **alkalmazások** > **Hozzáadás**, és kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**. Adja meg a következő információ hello varázslóban hello:
    * **Név** -HPCPackClusterServer
    * **Típus** – Itt adhatja meg **webes alkalmazáshoz és/vagy webes API-t**
    * **Bejelentkezés URL-cím**– hello hello mintát, amely alapértelmezés szerint az alap URL`https://hpcserver`
    * **App ID URI** - `https://<Directory_name>/<application_name>`. Cserélje le `<Directory_name`> az Azure AD-bérlőn, például a teljes név hello `hpclocal.onmicrosoft.com`, és cserélje le `<application_name>` hello névvel, akkor a korábban kiválasztott.

5. Hello alkalmazás hozzáadása után kattintson **konfigurálása**. A következő tulajdonságok hello konfigurálása:
    * Válassza ki **Igen** a **alkalmazás több-bérlős**
    * Válassza ki **Igen** a **felhasználói hozzárendelés szükséges tooaccess app**.

6. Kattintson a **Save** (Mentés) gombra. Mentés befejeztével kattintson **kezelése Manifest**. Ez a művelet az alkalmazás JavaScript object notation (JSON) jegyzékfájl tölti le. Szerkesztés letöltött hello jegyzékfájl hello megkeresésével `appRoles` beállítása és alkalmazás-szerepkör a következő hello felvétele:
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. Hello fájl mentéséhez. Hello portálon, kattintson a **kezelése Manifest** > **feltöltése Manifest**. Majd feltöltheti hello szerkesztett jegyzékben.
8. Kattintson a **felhasználók**, válasszon ki egy felhasználót, és kattintson a **hozzárendelése**. Hello elérhető szerepkörök (HpcUsers vagy HpcAdminMirror) toohello felhasználó hozzárendelését. Ismételje meg ezt a lépést hello directory további felhasználókat. Háttér-információkat fürt felhasználók, lásd: [fürt felhasználók kezelése](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).

   > [!NOTE] 
   > toomanage felhasználók, azt javasoljuk, hello Azure Active Directory előnézeti panelen a hello [Azure-portálon](https://portal.azure.com).
   >


## <a name="step-2-register-hello-hpc-cluster-client-with-your-azure-ad-tenant"></a>2. lépés: Az Azure AD-bérlő hello HPC-fürt ügyfél regisztrálása

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Kattintson a **Active Directory** a bal oldali menü hello, és kattintson a kívánt címtár hello az előfizetésben. Rendelkeznie kell engedéllyel tooaccess erőforrások hello könyvtárban.
3. Kattintson a **alkalmazások** > **Hozzáadás**, és kattintson a **a szerveztem által fejlesztett alkalmazás hozzáadása**. Adja meg a következő információ hello varázslóban hello:

    * **Név** -HPCPackClusterClient
    * **Típus** – Itt adhatja meg **natív ügyfélalkalmazás**
    * **Átirányítási URI** - `http://hpcclient`

4. Hello alkalmazás hozzáadása után kattintson **konfigurálása**. Másolás hello **ügyfél-azonosító** értékét, és mentse azt. Később szüksége az alkalmazás konfigurálásakor.

5. A **engedélyek tooother alkalmazások**, kattintson a **alkalmazás hozzáadása**. Keressen, és adja hozzá a hello HpcPackClusterServer alkalmazás (1. lépésben létrehozott).

6. A hello **delegált engedélyek** legördülő menüből válassza **hozzáférés HpcClusterServer**. Ezután kattintson a **Save** (Mentés) gombra.


## <a name="step-3-configure-hello-hpc-cluster"></a>3. lépés: Hello HPC-fürt konfigurálása

1. Csatlakoztassa az átjárócsomópont toohello HPC Pack 2016 az Azure-ban.

2. Indítsa el a HPC PowerShell.

3. Futtassa a következő parancs hello:

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    Ha

    * `AADTenant`Adja meg például a hello Azure AD bérlő neve`hpclocal.onmicrosoft.com`
    * `AADClientAppId`Adja meg a 2. lépésben létrehozott hello alkalmazás hello ügyfél-azonosító.

4. Indítsa újra a hello HpcSchedulerStateful szolgáltatást.

    Több központi csomóponttal rendelkező fürt esetén a következő PowerShell-parancsok hello átjárócsomópont tooswitch hello elsődleges replikán hello HpcSchedulerStateful szolgáltatás hello futtathatja:

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-hello-client"></a>4. lépés: Kezeléséhez, és küldje el a feladatok hello ügyfélről

tooinstall hello HPC Pack ügyfél segédprogramok a számítógépre letölthető Microsoft Download Center hello a HPC Pack 2016-telepítőfájlok (teljes telepítés). Ha hello a telepítés megkezdéséhez válassza hello telepítési beállítással a hello **HPC Pack ügyfél segédprogramok**.

tooprepare hello ügyfélszámítógép telepítése során használt hello tanúsítvány [HPC-fürttelepítés](hpcpack-2016-cluster.md) hello ügyfélszámítógépen. A szabványos Windows tanúsítvány felügyeleti eljárások tooinstall hello nyilvános tanúsítvány toohello **tanúsítványok – aktuális felhasználó** > **megbízható legfelső szintű hitelesítésszolgáltatók** tárolja. 

Mostantól hello HPC Pack parancsok futtatásához vagy hello HPC Pack feladat grafikus felhasználói felület toosubmit használja és fürt-feladatok kezelése hello Azure AD-fiókkal. További feladatok küldésének beállításai: [nyújt HPC feladatok tooan HPC Pack fürtön, az Azure-ban](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).

> [!NOTE]
> Tooconnect toohello HPC Pack fürtöt az Azure-ban a hello első alkalommal meg, megjelenik egy felugró ablakokat. Adja meg az Azure AD hitelesítő adatait toolog a. hello token gyorsítótárába. Az Azure használatát hello későbbi kapcsolatok toohello fürt token gyorsítótárazva, kivéve, ha a hitelesítés módosításai vagy gyorsítótárazott hello nincs bejelölve.
>
  
Például hello előző lépések végrehajtását követően alapján is kereshet feladatokat a helyi ügyfélről az alábbiak szerint:

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a>Az Azure AD-integrációs feladat elküldése hasznos parancsmagjai 

### <a name="manage-hello-local-token-cache"></a>Hello helyi jogkivonat gyorsítótára kezelése

HPC Pack 2016 toomanage hello helyi jogkivonat gyorsítótára két új HPC PowerShell-parancsmagokat kínál. Ezek a parancsmagok hasznosak feladatok nem interaktív elküldése. Tekintse meg a következő példa hello:

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-hello-credentials-for-submitting-jobs-using-hello-azure-ad-account"></a>Hello Azure AD-fiókot használó feladatok elküldésekor hello hitelesítő adatok beállítására 

Egyes esetekben érdemes lehet toorun hello feladat hello HPC-fürt felhasználói (a tartományhoz csatlakoztatott HPC-fürtre, egy tartományi felhasználói futtató; a nem tartományhoz csatlakoztatott HPC-fürtre, futtassa egy helyi felhasználói hello átjárócsomópont).

1. A következő parancsok tooset hello hitelesítő adatok hello használata:

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. Majd elküldeni az alábbiak szerint hello feladatot. hello/feladat fut a $localUser hello számítási csomóponton.

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   Ha `–Credential` nincs megadva `Submit-HpcJob`, hello feladat vagy tevékenység fut egy helyi csatlakoztatott felhasználói módon hello Azure AD-fiókot. (hello HPC-fürtben hoz létre egy helyi felhasználói azonos nevet, amint az Azure AD fiók toorun hello feladat hello hello.)
    
3. Határozza meg az Azure AD-fiókot hello kiterjesztett dátumát. Ez akkor hasznos, ha egy MPI feladat futó Linux csomópontok hello Azure AD-fiókkal.

   * Hello Azure AD-fiókot, maga a kibővített adatok beállítása

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * Állítsa be a kibővített adatok és a HPC-fürt felhasználóként
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

