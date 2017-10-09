---
title: "a Service Fabric aaaCreate fürthöz hello Azure-portálon |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure használatával biztonságos Service Fabric fürt tooset hello Azure-portál és az Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 045f71b491260e741ce7a54a75c440e1b33059a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-hello-azure-portal"></a>A Service Fabric-fürt létrehozása az Azure-ban hello Azure-portálon
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Ez a cikk részletesen ismerteti, amely végigvezeti Önt az Azure-ban az Azure-portálon hello biztonságos Service Fabric-fürt beállításának lépésein hello. Ez az útmutató bemutatja, hogyan hello a következő lépéseket:

* Key Vault toomanage kulcsok fürt a biztonság beállítása.
* Védett fürt létrehozása az Azure-ban hello Azure-portálon keresztül.
* Rendszergazdák tanúsítványok segítségével hitelesíti.

> [!NOTE]
> A speciális biztonsági beállítások, például a felhasználói hitelesítés az Azure Active Directory és az alkalmazás biztonsági tanúsítványok beállítását [Azure Resource Manager használatával a fürt létrehozása][create-cluster-arm].
> 
> 

A biztonságos fürtre a fürt, amely megakadályozza a jogosulatlan hozzáférés toomanagement műveletek, beleértve telepítése, frissítése és törlése, alkalmazások, szolgáltatások és bennük hello adatok. Egy nem biztonságos fürt olyan fürt, hogy bárki tooat bármikor csatlakozhat, és felügyeleti műveletek. Bár lehetséges toocreate egy nem biztonságos fürt, akkor **erősen ajánlott toocreate biztonságos fürt**. Egy nem biztonságos fürt **később nem védhető** -léteznie kell egy új fürtöt.

hello fogalmak vannak hello ugyanazt a biztonságos fürtök létrehozásához, hogy hello fürtök Linux-fürtök és a Windows-fürtök. További információk és segítő parancsfájlok biztonságos Linux-fürtök létrehozásához, ellenőrizze a [biztonságos fürtök Linux rendszeren létrehozására](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters). hello révén hello segítő parancsfájl a megadott paraméterek Igen közvetlenül be hello portálra hello szakaszban leírt módon [fürt létrehozása az Azure-portálon hello](#create-cluster-portal).

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure
Ez az útmutató használ [Azure PowerShell][azure-powershell]. Új PowerShell-munkamenet indításakor be tooyour Azure-fiókra, és jelölje ki az előfizetését az Azure parancsok végrehajtása előtt.

Jelentkezzen be tooyour azure fiók:

```powershell
Login-AzureRmAccount
```

Jelölje ki az előfizetését:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>A Key Vault beállítása
Ez a kijelző hello útmutató végigvezeti a kulcstároló, és a Service Fabric-alkalmazások számára az Azure Service Fabric-fürtök létrehozása. A Key Vault teljes körű, lásd: hello [Key Vault – első lépések útmutató][key-vault-get-started].

A Service Fabric X.509 tanúsítvány toosecure egy fürt használja. Az Azure Key Vault az Azure Service Fabric-fürtök használt toomanage tanúsítványait. A fürt telepítésekor az Azure-ban, a Service Fabric-fürtök létrehozásáért felelős hello Azure erőforrás-szolgáltató kéri le a tanúsítványokat a Key Vault, és telepíti azokat a virtuális gépek hello fürtön.

hello következő diagram azt ábrázolja, hello kapcsolat kulcstároló, a Service Fabric-fürt és a hello Azure erőforrás-szolgáltató által használt tanúsítványok Key Vault tárolódnak, amikor létrehozza a fürt között:

![Tanúsítvány telepítése][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása
hello első lépés egy új erőforráscsoportot kimondottan a Key Vault toocreate. Key Vault üzembe a saját erőforráscsoport ajánlott, hogy a számítási és tárolási erőforráscsoportok – például a Service Fabric-fürt rendelkező hello erőforráscsoport - eltávolíthatja a kulcsok és titkos elvesztése nélkül. a Key Vault rendelkező hello erőforráscsoport hello kell lennie és hello fürt által használt ugyanabban a régióban.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: hello output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Kulcstároló létrehozása
A kulcstároló hello új erőforráscsoport létrehozása. Key Vault hello **engedélyezni kell a központi telepítési** tooallow hello Service Fabric erőforrás szolgáltató tooget tanúsítványokat, és telepítse az fürtcsomóponton:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions tooKeys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions tooSecrets   :    all


    Tags                             :
```

Ha egy meglévő kulcstároló, a központi telepítés Azure parancssori felület használatával engedélyezheti azt:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-tookey-vault"></a>Tanúsítványok tooKey tároló hozzáadása
A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz. További információ a tanúsítványok használatának módját a Service Fabric: [Service Fabric-fürt biztonsági forgatókönyvek][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Fürt és a kiszolgálói tanúsítványt (kötelező)
Ez a tanúsítvány szükséges toosecure fürt, és megakadályozza a jogosulatlan hozzáférés tooit. Fürt biztonsági néhány módon tartalmazza:

* **Fürt hitelesítési:** hitelesíti a fürt összevonási csomópontok kommunikációt. Csak olyan csomópontot, amely bizonyítja, ezzel a tanúsítvánnyal az identitásukat hello fürt csatlakozhat.
* **Kiszolgálóhitelesítés:** hitelesíti hello fürt felügyeleti végpontok tooa felügyeleti ügyfél, így hello felügyeleti ügyfél tudja azt toohello valós fürt van van szó. Ez a tanúsítvány is biztosít SSL hello HTTPS felügyeleti API és a Service Fabric Explorer HTTPS-KAPCSOLATON keresztül.

tooserve ezek céljából, hello tanúsítvány hello követelményeknek kell megfelelniük:

* hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.
* a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.
* hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello használt tartomány tooaccess hello Service Fabric-fürt. Ez azért szükség tooprovide SSL hello fürt HTTPS felügyeleti végpontok és a Service Fabric Explorerben talál. Hello egy hitelesítésszolgáltatótól (CA) származó nem kaphat az SSL-tanúsítvány `.cloudapp.azure.com` tartomány. Szerezzen be egy egyéni tartománynevet a fürt számára. Amikor kér le tanúsítványt a CA hello tanúsítvány tulajdonosának neve meg kell egyeznie hello egyéni tartománynevet használja a fürthöz.

### <a name="client-authentication-certificates"></a>Ügyfél-hitelesítési tanúsítványok
További ügyfél-tanúsítványok fürt felügyeleti feladatok rendszergazdák hitelesítéséhez. A Service Fabric két olyan hozzáférési szintje van: **admin** és **írásvédett felhasználó**. Legalább a rendszergazdai hozzáférést a rendszer egy tanúsítványt kell használni. A további felhasználói szintű hozzáférés érdekében egy külön tanúsítványt kell megadni. A hozzáférés szerepkörök további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric ügyfelek][service-fabric-cluster-security-roles].

Nem kell a Service Fabric tooupload ügyfél hitelesítési tanúsítványok tooKey tároló toowork. Ezek a tanúsítványok csak a megadott toousers, akik jogosultak a kiszolgálófürt-felügyelet toobe van szükség. 

> [!NOTE]
> Az Azure Active Directory rendszer hello ajánlott módja tooauthenticate ügyfelek fürt felügyeleti műveleteket. Azure Active Directory toouse, kell [hozzon létre egy fürtöt, Azure Resource Manager használatával][create-cluster-arm].
> 
> 

### <a name="application-certificates-optional"></a>Alkalmazás-tanúsítványok (nem kötelező)
Tetszőleges számú további tanúsítványokat is telepíthető egy fürt biztonsági okokból. Az fürt létrehozása előtt vegye figyelembe a hello alkalmazás biztonsági igénylő forgatókönyvek egy hello csomópont, például a telepített tanúsítvány toobe:

* Az alkalmazás konfigurációs értékeit titkosítását és visszafejtését
* Replikáció során az adatok csomópontok közötti titkosítása 

Tanúsítványok nem állítható be, amikor hello Azure-portálon keresztül egy fürtöt hoz létre. fürt telepítéskor tooconfigure tanúsítványok, kell [hozzon létre egy fürtöt, Azure Resource Manager használatával][create-cluster-arm]. Alkalmazás tanúsítványok toohello fürt létrehozása után is hozzáadhat.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Az Azure erőforrás-szolgáltató használt tanúsítványok formázás
Titkos kulcs fájlok (.pfx) is felvehető és használható közvetlenül a Key Vault keresztül. Azonban hello Azure erőforrás-szolgáltató szükséges kulcsokat toobe tartalmaz, mint a base-64 kódolású karakterlánc és hello titkos kulcs jelszava hello .pfx különleges JSON formátumban tárolja. Ezek a követelmények, a kulcsok kell helyezni egy JSON-karakterláncban és majd tárolt tooaccommodate *titkok* a Key Vault.

Ez a könnyebb feldolgozni toomake, egy PowerShell-modul van [elérhető a Githubon][service-fabric-rp-helpers]. Kövesse az alábbi lépéseket toouse hello modul:

1. Töltse le a hello hello tárház teljes tartalmát egy helyi könyvtárba. 
2. A PowerShell-ablakban hello modul importálása:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

Hello `Invoke-AddCertToKeyVault` parancsot a PowerShell modul automatikusan a tanúsítvány titkos kulcsa alakítja JSON karakterláncnak és feltölti azt tooKey tárolóban. Ezzel tooadd hello fürt tanúsítvány, és minden további alkalmazás tanúsítványok tooKey tárolóban. Ismételje meg ezt a lépést a fürt kívánt tooinstall külön tanúsítványokra.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: hello output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomyvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Ezek a hello Key Vault szükséges előfeltételek maradéktalanul a Service Fabric fürt Resource Manager-sablon, amely telepíti a csomópont hitelesítési, felügyeleti végpont biztonsági és hitelesítési és minden további alkalmazásbiztonságot tanúsítványok konfigurálása X.509-tanúsítványokat használó szolgáltatások. Ezen a ponton hello telepítése az Azure-ban most már rendelkezik:

* Key Vault erőforráscsoport
  * Key Vault
    * Fürt kiszolgálói hitelesítési tanúsítvány

< /a "létrehozása, fürt, portal" ></a>

## <a name="create-cluster-in-hello-azure-portal"></a>Hello Azure-portálon a fürt létrehozása
### <a name="search-for-hello-service-fabric-cluster-resource"></a>Keresse meg a Service Fabric-fürt erőforrás hello
![Keresse meg a Service Fabric fürt sablonjának hello Azure-portálon.][SearchforServiceFabricClusterTemplate]

1. Jelentkezzen be toohello [Azure-portálon][azure-portal].
2. Kattintson a **új** tooadd egy új erőforrás-sablont. Keresse meg a Service Fabric-fürt sablon hello hello **piactér** alatt **mindent**.
3. Válassza ki **Service Fabric-fürt** hello listából.
4. Keresse meg a toohello **Service Fabric-fürt** panelen kattintson a **létrehozása**,
5. Hello **létrehozása a Service Fabric-fürt** panelen a következő négy lépést hello rendelkezik.

#### <a name="1-basics"></a>1. Alapvető beállítások
![Képernyőfelvétel az új erőforráscsoport létrehozásához.][CreateRG]

Hello alapvető beállítások panel kell tooprovide hello alapvető adatokat a fürt számára.

1. Adja meg a fürt hello neve.
2. Adjon meg egy **felhasználónév** és **jelszó** a távoli asztal hello virtuális gépek számára.
3. Győződjön meg arról, hogy tooselect hello **előfizetés** , amelyet a fürt toobe hatályba léptetni, különösen akkor, ha több előfizetéssel rendelkezik.
4. Hozzon létre egy **új erőforráscsoport**. Hello neve megegyezik a hello fürtön, mivel segíti a keresés, azokat később, különösen akkor, ha toomake módosítások tooyour telepítési próbált, vagy törölje a fürtöt, ajánlott toogive.
   
   > [!NOTE]
   > Bár eldöntheti, hogy egy meglévő erőforráscsoportot toouse, továbbra is egy célszerű toocreate egy új erőforráscsoportot. Így nem kell könnyen toodelete fürtöket.
   > 
   > 
5. Jelölje be hello **régió** kívánt toocreate hello fürt. Ugyanabban a régióban, amely a kulcsot tároló van hello kell használnia.

#### <a name="2-cluster-configuration"></a>2. Fürtkonfiguráció
![Csomópont-típus létrehozása][CreateNodeType]

Konfigurálja a fürtcsomópontokat. Csomóponttípusok hello Virtuálisgép-méretek, a virtuális gépek hello számát és azok tulajdonságait határozza meg. A fürt rendelkezhet egynél több csomóponttípus, de hello elsődleges csomóponttípushoz (hello elsőt hello Portal meghatározó) kell rendelkeznie legalább öt virtuális gépeket, ez ugyanis hello csomóponttípus ahol a Service Fabric rendszerszolgáltatások kerülnek. Ne konfiguráljon **elhelyezési tulajdonságok** , mert a rendszer automatikusan hozzáadja egy alapértelmezett elhelyezési "NodeTypeName" tulajdonságát.

> [!NOTE]
> Egy általános forgatókönyv több csomópont esetében az előtér-szolgáltatás és a háttér-szolgáltatás tartalmazó alkalmazást. Azt szeretné, hogy tooput hello előtér-szolgáltatást a kisebb rendelkező virtuális gépek (VM-méretek D2 beállításaihoz hasonlóan) portok nyitva toohello Internet, de arra tooput hello háttér-szolgáltatást a nagyobb virtuális gépek (VM-méretek például D4 D6, D15 és így tovább) internetre portot.
> 
> 

1. Adjon nevet a csomóponttípus (1 too12 karakter csak betűket és számokat tartalmazó).
2. minimális hello **mérete** hello elsődleges csomóponthoz tartozó virtuális gépek típus célja a hello **tartóssági** réteg hello fürt választja. hello hello tartóssági szint alapértelmezés szerint bronz. A durability további információkért lásd: [hogyan toochoose hello Service Fabric fürt megbízhatóság és a tartós][service-fabric-cluster-capacity].
3. Hello Virtuálisgép-méretet és tarifacsomagjának kiválasztása. D sorozatú virtuális gépek SSD meghajtók rendelkeznek, és erősen ajánlott állapotalapú alkalmazásokhoz. Nem használja a virtuális gép SKU részleges maggal rendelkező vagy a rendelkezésre álló szabad kapacitás 7 GB-nál kisebb. 
4. minimális hello **szám** hello elsődleges csomóponthoz tartozó virtuális gépek típus célja a hello **megbízhatóság** réteg választja. hello hello megbízhatósági szint alapértelmezés szerint ezüst. A megbízhatóság további információkért lásd: [hogyan toochoose hello Service Fabric fürt megbízhatóság és a tartós][service-fabric-cluster-capacity].
5. Válassza ki a hello hello csomóponttípus virtuális gépeinek számával. Méretezheti felfelé vagy lefelé hello virtuális gépek számát csomóponttípus meg, de hello elsődleges csomóponttípushoz, a minimális hello célja a választott hello megbízhatóság szintje. Egyéb legalább 1 virtuális gép is.
6. Egyéni végpontokat konfigurálhat. Ez a mező tooenter, amelyet az tooexpose keresztül hello Azure Load Balancer toohello portok vesszővel tagolt listája lehetővé teszi az alkalmazások a nyilvános internethez. Például, ha azt tervezi, hogy egy webes alkalmazás tooyour fürt toodeploy, adja meg az "80" Itt tooallow forgalom 80-as porton a fürtbe. A végpontok további információkért lásd: [alkalmazások folytatott kommunikáció][service-fabric-connect-and-communicate-with-services]
7. Fürt konfigurálása **diagnosztika**. Alapértelmezés szerint a diagnosztika engedélyezve vannak a a fürt tooassist a problémák elhárításához. Ha azt szeretné, hogy a toodisable diagnosztika módosítása hello **állapot** túl váltása**ki**. Diagnosztika kikapcsolása van **nem** ajánlott.
8. Válassza ki a háló frissítési mód beállítása a fürt kívánt hello. Válassza ki **automatikus**, ha szeretné, hogy hello rendszer tooautomatically felvételével kapcsolatban hello elérhető legújabb verzióra, majd próbálja tooupgrade a fürt tooit. Hello mód beállítása túl**manuális**, ha azt szeretné, hogy toochoose valamelyik támogatott verzióra.

> [!NOTE]
> Csak a service Fabric támogatott verzióit futtató fürtök támogatott. Hello kiválasztásával **manuális** mód, készítésének hello felelősségi tooupgrade meg a fürt tooa támogatott verziója. További információ a hello háló frissítési módot: hello [service fabric-fürt-frissítési dokumentum.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Biztonság
![Képernyőfelvétel az Azure-portál biztonsági beállításokkal.][SecurityConfigs]

utolsó lépésként hello tooprovide tanúsítvány információk toosecure hello fürt hello Key Vault és információk a korábban létrehozott tanúsítvány használatával.

* Hello elsődleges tanúsítvány mezők feltöltése hello töltsenek kapott hello kimenet **fürt tanúsítvány** tooKey tárolót használó hello `Invoke-AddCertToKeyVault` PowerShell-parancsot.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Ellenőrizze a hello **speciális beállítások konfigurálása** ügyféltanúsítványainak tooenter mezőben **rendszergazdai ügyfél** és **írásvédett ügyfél**. Adja meg ezeket a mezőket az admin ügyféltanúsítvány ujjlenyomata hello és az írásvédett felhasználó ügyféltanúsítvány ujjlenyomata hello egységeit. Amikor a rendszergazdák próbálnak tooconnect toohello fürt, kapnak, az itt megadott hozzáférés csak akkor, ha rendelkeznek a megfelelő hello ujjlenyomat értékek ujjlenyomattal rendelkező tanúsítványt.  

#### <a name="4-summary"></a>4. Összefoglalás
![Képernyőfelvétel a hello start Bizottsága megjelenítése "A Service Fabric-fürt." ][Notifications]

toocomplete hello fürt létrehozása, kattintson a **összegzés** toosee hello konfigurációkat megadott, vagy töltse le hello toodeploy a fürt, amely használt Azure Resource Manager sablon. Hello kötelező beállítások megadása után hello **OK** gomb akkor válik zöld és hello Fürtlétrehozási folyamat indításához kattintson az.

Hello létrehozásának folyamata hello értesítések tekintheti meg. (Kattintson a hello "harang" ikonra hello állapotsorban hello jobb felső sarkában a képernyőn közelében.) Kattintott **PIN-kód tooStartboard** hello fürt létrehozásakor látni fogja **Fabric-fürt üzembe helyezése** toohello rögzítve **Start** tábla.

### <a name="view-your-cluster-status"></a>A fürt állapotának megtekintése
![Képernyőfelvétel a hello irányítópulton fürt adatait.][ClusterDashboard]

A fürt létrehozása után vizsgálhatja meg a fürt hello portálon:

1. Nyissa meg túl**Tallózás** kattintson **Service Fabric-fürtök**.
2. Keresse meg a fürthöz, és kattintson rá.
3. Most már megtekintheti a fürt hello irányítópulton, beleértve a nyilvános végpontot hello fürt és a hivatkozás tooService Fabric Explorer hello részleteit.

Hello **csomópont Monitor** szakasz hello fürt irányítópult panelen, amelyek a megfelelő és nem kifogástalan állapotú virtuális gépek hello számát jelzi. Hello fürt állapotát, további információkat is talál [Service Fabric rendszerállapot-modell bemutatása][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric-fürtök bizonyos számú csomópontok toobe mindig toomaintain rendelkezésre állási be kell, és állapot - hivatkozott tooas "kvórum fenntartása" megőrzéséhez. Therfore, nincs általában le hello fürt összes gépeket biztonságos tooshut kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-tooa-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Távoli kapcsolódás tooa virtuálisgép-méretezési csoport példányt vagy egy fürt csomópontja
A NodeType tulajdonságok értéke hello mindegyikének ad meg a fürt eredmény egy virtuálisgép-méretezési csoportban lévő első beállításról. Lásd: [távoli csatlakozás tooa virtuálisgép-méretezési csoport példány] [ remote-connect-to-a-vm-scale-set] részleteiről.

## <a name="next-steps"></a>Következő lépések
Ezen a ponton hogy a biztonságos fürt felügyeleti hitelesítési tanúsítványokat használnak. Ezt követően [csatlakoztassa tooyour fürtöt](service-fabric-connect-to-secure-cluster.md) és megtudhatja, hogyan túl[alkalmazás titkos kulcsok kezelése](service-fabric-application-secret-management.md).  Emellett megismerése [Service Fabric támogatási lehetőségek](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
