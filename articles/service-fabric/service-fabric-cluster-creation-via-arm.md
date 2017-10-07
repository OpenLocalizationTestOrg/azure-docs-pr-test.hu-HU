---
title: "egy Azure Service Fabric aaaCreate fürt olyan sablon alapján |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset másolatot egy biztonságos Service Fabric fürt az Azure-ban Azure Resource Manager, az Azure Key Vault és az Azure Active Directory (Azure AD) segítségével az ügyfél-hitelesítéshez."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: a4563c58a68127720a8290c3be0df9d026833eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>A Service Fabric-fürt létrehozása az Azure Resource Manager használatával
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Ez az útmutató lépésről lépésre bemutatja, hogyan állítja be a biztonságos Azure Service Fabric-fürt az Azure-ban Azure Resource Manager használatával. A Microsoft tudomásul hello cikkben hosszú. Mindazonáltal kivéve, ha már alaposan tisztában hello tartalommal, kell meg arról, hogy toofollow minden lépés gondosan.

hello útmutató hello az alábbi eljárásokat tartalmazza:

* Az az Azure key vault tooupload tanúsítványok fürt- és a biztonság beállítása
* Védett fürt létrehozása az Azure-ban Azure Resource Manager használatával
* Felhasználók hitelesítése az Azure Active Directory (Azure AD) segítségével a kiszolgálófürt-felügyelet

A biztonságos fürt olyan fürt, amely megakadályozza a jogosulatlan hozzáférés toomanagement műveletek. Ez magában foglalja az üzembe helyezés, a frissítés és törlése alkalmazások, szolgáltatások és hello adatokat tartalmaznak. Egy nem biztonságos fürt olyan fürt, hogy bárki tooat bármikor csatlakozhat, és felügyeleti műveletek. Bár lehetséges toocreate egy nem biztonságos fürtre, határozottan ajánlott, hogy hozzon létre egy biztonságos fürt hello kezdettől. Egy nem biztonságos fürt később nem védhető, mert létre kell hozni egy új fürtöt.

hello fogalma létrehozása biztonságos fürtök van hello azonos, Linux vagy a Windows-fürtök legyenek. További információk és segítő parancsfájlok biztonságos Linux-fürtök létrehozásához, lásd: [biztonságos fürtök Linux rendszeren létrehozására](#secure-linux-clusters).

## <a name="sign-in-tooyour-azure-account"></a>Jelentkezzen be tooyour Azure-fiók
Ez az útmutató használ [Azure PowerShell][azure-powershell]. Amikor egy új PowerShell-munkamenetet indít el, jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését, Azure parancsok végrehajtása előtt.

Bejelentkezés tooyour Azure-fiók:

```powershell
Login-AzureRmAccount
```

Jelölje ki az előfizetését:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a>Kulcstároló beállítása
Ez a szakasz ismerteti, és a Service Fabric-alkalmazások számára az Azure Service Fabric-fürt kulcstároló létrehozása. A teljes körű útmutatót tooAzure Key Vault meg toohello [Key Vault – első lépések útmutató][key-vault-get-started].

A Service Fabric X.509 tanúsítvány toosecure egy fürt használja, és adja meg az alkalmazás biztonsági funkciók. A Service Fabric-fürtök az Azure Key Vault toomanage tanúsítványokat használ. A fürt telepítésekor az Azure-ban, hello Azure erőforrás-szolgáltató, amely felelős az Service Fabric-fürtök létrehozására kéri le a tanúsítványokat a Key Vault, és telepíti őket hello fürt virtuális gépeket.

hello alábbi ábrán látható, az Azure Key Vault, a Service Fabric-fürt és a hello Azure erőforrás-szolgáltató, amikor létrehozza a fürtöt a key vaultban tárolt tanúsítványokat használó hello kapcsolatát:

![Tanúsítvány telepítése ábrája][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
hello első lépés egy erőforráscsoportot kimondottan a key vaultban toocreate. Azt javasoljuk, hogy hello kulcstároló kerüljenek-e a saját erőforráscsoport. Ez a művelet lehetővé teszi hello számítási és tárolási erőforrás csoportokhoz, beleértve a Service Fabric-fürt, a kulcsok és titkos kulcsok elvesztése tartalmazó erőforráscsoport hello eltávolítását. a kulcstartót tartalmazó erőforráscsoportot hello _hello kell ugyanabban a régióban_ által használt hello fürtként.

Ha több régióba toodeploy fürtök, javasoljuk, hogy hello erőforráscsoport és oly módon, amely azt jelzi, mely régióhoz tartozik, hello kulcstároló neve.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
hello kimeneti kell kinéznie:

```powershell

    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-hello-new-resource-group"></a>Hozzon létre egy kulcstartót hello új erőforráscsoport
hello kulcstároló _engedélyezni kell a központi telepítési_ tooallow hello számítási erőforrás szolgáltató tooget tanúsítványokat, és telepíti azt a virtuálisgép-példányok:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

hello kimeneti kell kinéznie:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
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
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Használjon egy meglévő kulcstároló

egy meglévő kulcstároló toouse meg _engedélyezni kell a központi telepítési_ tooallow hello számítási erőforrás szolgáltató tooget tanúsítványokat, és telepítse a fürtcsomópontokon:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-tooyour-key-vault"></a>Tanúsítványok tooyour kulcstároló hozzáadása

A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz. További információ a tanúsítványok használatának módját a Service Fabric: [Service Fabric-fürt biztonsági forgatókönyvek][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Fürt és a kiszolgálói tanúsítványt (kötelező)
Ez a tanúsítvány szükséges toosecure fürt, és megakadályozza a jogosulatlan hozzáférés tooit. Fürt biztonsági két módon tartalmazza:

* Fürt hitelesítési: hitelesíti a fürt összevonási csomópontok kommunikációt. Csak olyan csomópontot, amely bizonyítja, ezzel a tanúsítvánnyal az identitásukat hello fürt csatlakozhat.
* Kiszolgálóhitelesítés: hitelesíti a hello fürt felügyeleti végpontok tooa felügyeleti ügyfél, így hello felügyeleti ügyfél tudja azt toohello valós fürt van van szó. Ez a tanúsítvány is biztosít az SSL hello HTTPS felügyeleti API és a Service Fabric Explorer HTTPS-KAPCSOLATON keresztül.

tooserve ezek céljából, hello tanúsítvány hello követelményeknek kell megfelelniük:

* hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.
* a kulcscseréhez használt, amely exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.
* hello tanúsítvány tulajdonosának nevét meg kell egyeznie a hello tartomány tooaccess hello Service Fabric-fürt használata. A megfelelő szükség tooprovide hello fürt HTTPS felügyeleti végpontok az SSL és a Service Fabric Explorerben talál. Hello egy hitelesítésszolgáltatótól (CA) származó nem kaphat az SSL-tanúsítvány. cloudapp.azure.com tartomány. Be kell szereznie egy egyéni tartománynevet a fürt számára. A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.

### <a name="application-certificates-optional"></a>Alkalmazás-tanúsítványok (nem kötelező)
Tetszőleges számú további tanúsítványokat is telepíthető egy fürt biztonsági okokból. Az fürt létrehozása előtt vegye figyelembe a hello alkalmazás biztonsági igénylő forgatókönyvek egy hello csomópont, például a telepített tanúsítvány toobe:

* Titkosítás és visszafejtés az alkalmazáskonfigurációs értékeket.
* Replikáció során az adatok csomópontok közötti titkosítása.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Az Azure erőforrás-szolgáltató használt tanúsítványok formázás
Adja hozzá, és a titkos kulcs fájlok használata (.pfx) közvetlenül a kulcstartót keresztül. Azonban hello Azure számítási erőforrás-szolgáltató szükséges kulcsokat toobe különleges JavaScript Object Notation (JSON) formátumban tárolja. hello formátum base 64 kódolású karakterlánc és titkos kulcs jelszava hello hello .pfx fájl tartalmazza. tooaccommodate ezeknek a követelményeknek, hello kulcsok kell helyezni egy JSON-karakterláncban és majd tárolt "titkos kulcsainak" hello kulcsot tároló.

a könnyebb, feldolgozni toomake egy [PowerShell modul az elérhető a Githubon][service-fabric-rp-helpers]. toouse hello modul, a következő hello:

1. Töltse le a hello hello tárház teljes tartalmát egy helyi könyvtárba.
2. Nyissa meg toohello helyi könyvtárba.
2. A PowerShell-ablakban hello ServiceFabricRPHelpers modul importálása:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

Hello `Invoke-AddCertToKeyVault` parancsot a PowerShell modul automatikusan a tanúsítvány titkos kulcsa alakítja JSON karakterláncnak és feltölti azt toohello kulcstároló. Hello parancs tooadd hello fürt tanúsítványt, és minden további alkalmazás tanúsítványok toohello kulcstároló használnia. Ismételje meg ezt a lépést a fürt kívánt tooinstall külön tanúsítványokra.

#### <a name="uploading-an-existing-certificate"></a>Létező tanúsítvány feltöltése

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Ha a hiba, például az egyik látható itt, hello Ez általában azt jelenti, hogy rendelkezik-e URL-cím erőforrás-ütközés. tooresolve hello ütközik, a módosítás hello kulcstároló neve.

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Miután hello ütközés feloldja, hello kimeneti kell néznek ki:

```

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Hello három előző karakterláncok, CertificateThumbprint SourceVault és CertificateURL, a biztonságos Service Fabric-fürt és az tooobtain tooset szükség, amely akkor használhatja az alkalmazás biztonsági alkalmazás tanúsítványokra. Hello karakterláncok nem menti, ha azok hello kulcs lekérdezésével tároló később nehéz tooretrieve lehet.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-toohello-key-vault"></a>Önaláírt tanúsítvány létrehozása, majd ismét feltölteni a toohello kulcstároló

Ha már feltöltött tanúsítványok toohello key vaultban, kihagyhatja ezt a lépést. Ebben a lépésben egy új önaláírt tanúsítvány létrehozása, majd ismét feltölteni a tooyour kulcstároló szolgál. Módosítsa a következő parancsfájl hello hello paramétereit, és futtassa, miután a rendszer kéri a tanúsítvány jelszót.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want hello .PFX toobe stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Ha a hiba, például az egyik látható itt, hello Ez általában azt jelenti, hogy rendelkezik-e URL-cím erőforrás-ütközés. tooresolve hello ütközést, hello kulcstároló nevének módosítása, RG nevét, és így tovább.

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Miután hello ütközés feloldja, hello kimeneti kell néznek ki:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context tooSubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: hello output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret toochackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Hello három előző karakterláncok, CertificateThumbprint SourceVault és CertificateURL, a biztonságos Service Fabric-fürt és az tooobtain tooset szükség, amely akkor használhatja az alkalmazás biztonsági alkalmazás tanúsítványokra. Hello karakterláncok nem menti, ha azok hello kulcs lekérdezésével tároló később nehéz tooretrieve lehet.

 Ezen a ponton rendelkeznie kell a következő helyen elemek hello:

* hello kulcstároló erőforráscsoportot.
* hello kulcstartó és az URL-CÍMÉT (PowerShell kimeneti megelőző hello SourceVault néven).
* hello fürt kiszolgálói hitelesítési tanúsítványt és annak hello kulcstároló URL-címet.
* hello alkalmazás tanúsítványokat és azok hello kulcstároló URL-címeit.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Az ügyfél-hitelesítéshez az Azure Active Directory beállítása

Az Azure AD lehetővé teszi, hogy a szervezetek (úgynevezett bérlők) toomanage felhasználói hozzáférés tooapplications. Alkalmazások oszthatók rendelkező webalapú bejelentkezési felhasználói felület és az egy natív ügyfél révén azokat. Ebben a cikkben azt feltételezzük, hogy már létrehozta a bérlő. Ha nem, olvassa el [hogyan tooget egy Azure Active Directory-bérlőazonosítóra][active-directory-howto-tenant].

A Service Fabric-fürt biztosít több belépési pontok tooits felügyeleti funkciót, beleértve a webalapú hello [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] és [Visual Studio] [service-fabric-manage-application-in-visual-studio]. Ennek eredményeképpen hozzon létre két az Azure AD alkalmazások toocontrol hozzáférés toohello fürt, egy webes alkalmazás és egy natív alkalmazás.

egyes hello lépések részt az Azure AD konfigurálása a Service Fabric-fürt toosimplify, a Windows PowerShell-parancsfájlokkal létrehoztunk.

> [!NOTE]
> Hello hello fürt létrehozása előtt az alábbi lépések elvégzése után. Hello parancsfájlok a fürt nevét és a végpontok várható, mivel a hello értékeket kell megtervezni, és nem az, hogy már létrehozta értékeket.

1. [Töltse le a hello parancsfájlok] [ sf-aad-ps-script-download] tooyour számítógép.
2. Kattintson a jobb gombbal hello zip-fájl, jelölje be **tulajdonságok**, jelölje be hello **Unblock** jelölőnégyzetet, majd kattintson a **alkalmaz**.
3. Hello zip-fájl kicsomagolásához.
4. Futtatás `SetupApplications.ps1`, és adja meg a hello TenantId, fürtnév, és WebApplicationReplyUrl paraméterekként. Példa:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    A TenantId találhatja meg hello PowerShell-parancs végrehajtása `Get-AzureSubscription`. Ez a parancs végrehajtása hello TenantId minden előfizetés jeleníti meg.

    ClusterName hello parancsfájl által létrehozott használt tooprefix hello Azure AD-alkalmazásokat. Nem kell toomatch hello tényleges fürtnév pontosan. Tervezett csak toomake az egyszerűbb toomap az Azure AD összetevők toohello Service Fabric-fürt, amely akkor van használatban.

    WebApplicationReplyUrl hello alapértelmezett végpont az Azure AD tooyour felhasználók ad vissza, miután a bejelentkezés befejezéséhez. Ez a végpont hello Service Fabric Explorer végpont a fürt, amely alapértelmezés szerint állítja be:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    A hello Azure AD bérlői rendszergazdai jogosultságokkal rendelkező fiók tooan rákérdezéses toosign áll. Miután bejelentkezik, hello parancsfájlt hoz létre hello web- és natív alkalmazások toorepresent a Service Fabric-fürt. Ha megnézi a hello hello bérlői alkalmazások [a klasszikus Azure portálon][azure-classic-portal], két új bejegyzést kell megjelennie:

   * *ClusterName*\_fürt
   * *ClusterName*\_ügyfél

   hello parancsfájl kinyomtatja hello JSON hello Azure Resource Manager sablonhoz szükséges hello fürt létrehozásakor hello a következő szakaszban, hogy a jó ötlet tookeep hello PowerShell ablak megnyitásához.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Service Fabric-fürt Resource Manager sablon létrehozása
Az előző hello kimenete hello ebben a szakaszban a Service Fabric fürt Resource Manager-sablon PowerShell-parancsok használhatók.

A minta Resource Manager-sablonok találhatók hello [Azure gyors üzembe helyezési sablon gyűjteménye a Githubon][azure-quickstart-templates]. Ezek a sablonok kiindulási pontként használható a fürt sablon.

### <a name="create-hello-resource-manager-template"></a>Hello Resource Manager-sablon létrehozása
Ez az útmutató használ hello [biztonságos 5-csomópontot tartalmazó fürtben] [ service-fabric-secure-cluster-5-node-1-nodetype] példa sablon és a sablon paramétereit. Töltse le `azuredeploy.json` és `azuredeploy.parameters.json` tooyour számítógépet, majd nyissa meg a fájlt a kedvenc szövegszerkesztőjével.

### <a name="add-certificates"></a>Tanúsítványok hozzáadása
Tanúsítványok tooa fürt Resource Manager-sablon hivatkozik, amely tartalmazza a hello tanúsítvány kulcsait kulcstároló hello hozzáadhat. Azt javasoljuk, hogy hello kulcstároló értékek helyez egy Resource Manager sablon paraméterfájl. Így tartja a Resource Manager hello sablonfájl újrafelhasználható és szabad értékek adott tooa központi telepítés.

#### <a name="add-all-certificates-toohello-virtual-machine-scale-set-osprofile"></a>Minden tanúsítványok toohello virtuálisgép-méretezési csoport osProfile hozzáadása
Minden tanúsítvány hello fürtben telepített hello osProfile szakaszában hello méretezési készlet erőforrás (Microsoft.Compute/virtualMachineScaleSets) kell konfigurálni. Ez a művelet arra utasítja a hello erőforrás szolgáltató tooinstall hello tanúsítvány hello virtuális gépeket. A telepítés hello fürt tanúsítvány és a bármely alkalmazás biztonsági tanúsítványok toouse tervezi meg az alkalmazások is tartalmazza:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-hello-service-fabric-cluster-certificate"></a>Hello Service Fabric-fürt tanúsítvány konfigurálása
hello fürt hitelesítési tanúsítványt kell konfigurálni mindkét hello Service Fabric-fürt erőforrás (Microsoft.ServiceFabric/clusters) pedig a Service Fabric-bővítményt a virtuálisgép-méretezési hello állítja be a hello virtuálisgép-méretezési készlet erőforrás. Ezzel az elrendezéssel hello Service Fabric erőforrás-szolgáltató tooconfigure lehetővé teszi, hogy a fürt hitelesítési és felügyeleti végpontok kiszolgálóhitelesítéshez.

##### <a name="virtual-machine-scale-set-resource"></a>Virtuálisgép-méretezési erőforrás:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>A Service Fabric-erőforrás:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Helyezze be az Azure AD konfigurálása
korábban létrehozott hello Azure AD-konfigurációs illeszthető közvetlenül a Resource Manager-sablon. Azonban azt javasoljuk, hogy először hello értékek azokat a paramétereket fájl tookeep hello Resource Manager sablon egyszer használatos kiolvasni szabad értékek adott tooa központi telepítés.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < a "konfigurálása – arm" ></a>Sablonparaméterek erőforrás-kezelő konfigurálása
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since hello link seems not toobe redirecting correctly --->
Végezetül hello kimeneti értékeinek hello kulcstartó és az Azure AD PowerShell parancsok toopopulate hello paraméterfájl használata:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Ezen a ponton rendelkeznie kell a következő helyen elemek hello:

* Kulcstároló erőforráscsoport
  * Key Vault
  * Fürt kiszolgálói hitelesítési tanúsítvány
  * Adatok rejtjelezése tanúsítvány
* Az Azure Active Directory-bérlő
  * Az Azure AD-alkalmazást a web-alapú felügyeleti és a Service Fabric Explorerrel
  * Natív ügyfél kezelése az Azure AD-alkalmazás
  * Felhasználók és a hozzárendelt szerepkör
* A Service Fabric-fürt Resource Manager-sablon
  * Kulcstároló keresztül konfigurált tanúsítványok
  * Az Azure Active Directory konfigurálva

hello a következő ábra azt mutatja be, ahol a kulcstartót és az Azure Active Directory beállítása felelnek meg a Resource Manager-sablon.

![Erőforrás-kezelő kapcsolatfüggőségi térképének megjelenítéséhez][cluster-security-arm-dependency-map]

## <a name="create-hello-cluster"></a>Hello fürt létrehozása
Most már áll készen toocreate hello fürt használatával [Azure-erőforrás sablon-üzembehelyezés][resource-group-template-deploy].

#### <a name="test-it"></a>Tesztelheti
A Resource Manager-sablon az alábbi PowerShell-parancs tootest hello használata a paraméterfájl:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Telepítés
Ha hello Resource Manager sablon teszt sikeres, használja a következő PowerShell-parancs toodeploy hello a Resource Manager-sablon paraméterfájl:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-tooroles"></a>Felhasználók tooroles hozzárendelése
A létrehozást követően hello alkalmazások toorepresent a fürthöz, a felhasználók toohello szerepkörök hozzárendelése a Service Fabric által támogatott: olvasási és a rendszergazda segítségét. Hello szerepköröket rendelhet hello segítségével [a klasszikus Azure portálon][azure-classic-portal].

1. A hello Azure-portálon, válassza a tooyour bérlői, majd válassza ki **alkalmazások**.
2. Válassza ki a hello webalkalmazás, melynek neve például `myTestCluster_Cluster`.
3. Kattintson a hello **felhasználók** fülre.
4. Válassza ki a felhasználói tooassign, és kattintson a hello **hozzárendelése** üdvözlő képernyőt hello alján gombra.

    ![Rendelje hozzá a felhasználók tooroles gomb][assign-users-to-roles-button]
5. Válassza ki a hello szerepkör tooassign toohello felhasználót.

    !["A felhasználók hozzárendelése" párbeszédpanel][assign-users-to-roles-dialog]

> [!NOTE]
> További információ a szerepkörök a Service Fabric: [szerepköralapú hozzáférés-vezérlés a Service Fabric ügyfelek](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused hello wrong redirection of hello link --->

## <a name="create-secure-clusters-on-linux"></a>Hozzon létre a biztonságos fürtök Linux rendszeren
toomake hello folyamat könnyebb adtunk egy [segítő parancsfájl](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). A segítő parancsfájl használata előtt győződjön meg arról, hogy már van telepítve az Azure parancssori felület (CLI), és az elérési úthoz. Győződjön meg arról, hogy hello parancsfájl engedélyek tooexecute futtatásával `chmod +x cert_helper.py` azt letöltése után. hello első lépése az Azure-fiók tooyour toosign hello parancssori felület használatával `azure login` parancsot. Tooyour Azure-fiókkal történő bejelentkezés után hello segítő parancsfájl használata a CA tanúsítvány aláírására használatos, mint hello parancs azt mutatja be a következő:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

hello - ifile paramétere lehet egy .pfx fájlba vagy a .pem fájl bemenetként hello tanúsítvány típus (pfx vagy pem, vagy ha az önaláírt tanúsítvány ss).
hello súgószöveg hello paraméter -h megjeleníti.


Ez a parancs visszaadja a következő három karakterláncok hello kimenetként hello:

* SourceVaultID, amely hello hello azonosítója hozott létre az Ön új KeyVault ResourceGroup
* CertificateUrl hello tanúsítvány eléréséhez
* CertificateThumbprint, amelyek használják a hitelesítéshez

hello a következő példa bemutatja, hogyan toouse hello parancsot:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Az alábbiak szerint hello három karakterláncok parancs által biztosított megelőző hello végrehajtása:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

hello tanúsítvány tulajdonosának nevét meg kell egyeznie a hello tartomány tooaccess hello Service Fabric-fürt használata. A megfelelés szükség tooprovide hello fürt HTTPS felügyeleti végpontok az SSL és a Service Fabric Explorerben talál. Nem szerezze be egy SSL-tanúsítványt egy hitelesítésszolgáltatóból hello `.cloudapp.azure.com` tartomány. Be kell szereznie egy egyéni tartománynevet a fürt számára. A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.

A tulajdonos neve hello bejegyzések toocreate biztonságos Service Fabric-fürt (nélkül az Azure AD), részben ismertetett módon [erőforrás-kezelő konfigurálása Sablonparaméterek](#configure-arm). Hello utasításokat követve csatlakozhat toohello biztonságos fürt [ügyfél-hozzáférési tooa fürt hitelesítéséhez](service-fabric-connect-to-secure-cluster.md). Linux preview fürtök nem támogatják az Azure AD-alapú hitelesítés. Felügyelet és az ügyfél szerepköröket rendelhet, lásd: hello [szerepkörök toousers hozzárendelése](#assign-roles) szakasz. Felügyelet és az ügyfél Linux preview fürt szerepkörök megadásakor tooprovide tanúsítvány-ujjlenyomatok hitelesítési rendelkezik. (Nincs megadva hello tulajdonos neve, mert a visszavont tanúsítványok, sem a tanúsítványlánc ellenőrzése zajlik, ebben az előzetes kiadásban.)

Ha önaláírt tanúsítványt toouse tesztelési, használhatja ugyanazon parancsfájl toogenerate egy hello. Majd feltöltheti hello tanúsítvány tooyour kulcstároló hello jelző megadásával `ss` hello tanúsítvány elérési útja és a tanúsítvány nevének megadása helyett. Például tekintse meg a következő parancs létrehozása és feltöltése egy önaláírt tanúsítványt hello:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Ez a parancs beolvasása hello azonos három karakterláncok: SourceVault CertificateUrl és CertificateThumbprint. Biztonságos Linux-fürt és a hely hol helyezkedik el, hello önaláírt tanúsítványt, hello karakterláncok toocreate használhatja. Hello önaláírt tanúsítvány tooconnect toohello fürt van szüksége. Hello utasításokat követve csatlakozhat toohello biztonságos fürt [ügyfél-hozzáférési tooa fürt hitelesítéséhez](service-fabric-connect-to-secure-cluster.md).

hello tanúsítvány tulajdonosának nevét meg kell egyeznie a hello tartomány tooaccess hello Service Fabric-fürt használata. A megfelelés szükség tooprovide hello fürt HTTPS felügyeleti végpontok az SSL és a Service Fabric Explorerben talál. Nem szerezze be egy SSL-tanúsítványt egy hitelesítésszolgáltatóból hello `.cloudapp.azure.com` tartomány. Be kell szereznie egy egyéni tartománynevet a fürt számára. A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.

Hello paraméterek az Azure-portálon hello hello segítő parancsfájlból hello leírtak szerint töltheti [fürt létrehozása az Azure-portálon hello](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) szakasz.

## <a name="next-steps"></a>Következő lépések
Ezen a ponton rendelkezik olyan Azure Active Directory-felügyeleti hitelesítés biztonságos fürttel. Ezt követően [csatlakoztassa tooyour fürtöt](service-fabric-connect-to-secure-cluster.md) és megtudhatja, hogyan túl[alkalmazás titkos kulcsok kezelése](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Az ügyfél-hitelesítéshez Azure Active Directory beállításának hibaelhárítása
Futtatása a problémát az ügyfél-hitelesítéshez az Azure AD beállítása közben, tekintse át a lehetséges megoldások hello ebben a szakaszban.

### <a name="service-fabric-explorer-prompts-you-tooselect-a-certificate"></a>Service Fabric Explorer kér tanúsítvány tooselect
#### <a name="problem"></a>Probléma
Sikeresen tooAzure AD a Service Fabric Explorerben a bejelentkezést, hello böngésző toohello kezdőlap adja vissza, de az üzenet tooselect tanúsítványt kér.

![SFX válassza a tanúsítványt párbeszédpanel][sfx-select-certificate-dialog]

#### <a name="reason"></a>Ok
hello felhasználói szerepet hello Azure AD-fürt alkalmazást, nincs hozzárendelve. Így az Azure AD-alapú hitelesítés nem sikerül, a Service Fabric-fürt. Service Fabric Explorer visszavált toocertificate hitelesítés.

#### <a name="solution"></a>Megoldás
Hello utasítások szerint állíthatja be az Azure AD és a felhasználói szerepkörök hozzárendelésére. Emellett javasoljuk, hogy kapcsolja be a "Felhasználói hozzárendelés szükséges tooaccess alkalmazás," `SetupApplications.ps1` does.

### <a name="connection-with-powershell-fails-with-an-error-hello-specified-credentials-are-invalid"></a>PowerShell-kapcsolatot egy hibaüzenettel meghiúsul: "hello megadva hitelesítő adatok érvénytelenek."
#### <a name="problem"></a>Probléma
Sikertelen, ha PowerShell tooconnect toohello fürt "AzureActiveDirectory" biztonsági módban, a sikeresen tooAzure AD a bejelentkezést, hello kapcsolódási hiba miatt: "hello megadva hitelesítő adatok érvénytelenek."

#### <a name="solution"></a>Megoldás
Ez a megoldás azonos hello megelőző egy hello.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer hibát ad vissza, amikor bejelentkezik: "AADSTS50011"
#### <a name="problem"></a>Probléma
TooAzure AD a Service Fabric Explorerben a toosign használatakor hello lap adja vissza a hiba: "AADSTS50011: hello válaszcímet &lt;URL-cím&gt; nem egyezik meg a hello válasz címek hello alkalmazáshoz konfigurált: &lt;guid&gt;."

![A válaszcím SFX nem egyezik.][sfx-reply-address-not-match]

#### <a name="reason"></a>Ok
hello (webalkalmazás) fürt Service Fabric Explorer jelölő kísérletet tesz az Azure AD elleni tooauthenticate, és hello kérelem részeként is biztosít hello átirányítási visszatérési URL-cím. De hello URL-címe nem szerepel a hello Azure AD alkalmazás **válasz URL-CÍMEN** listája.

#### <a name="solution"></a>Megoldás
A hello **konfigurálása** hello lapján cluster (webalkalmazás), adja hozzá az URL-címet a Service Fabric Explorer toohello hello **válasz URL-CÍMEN** listában, és cserélje le a hello elemek hello listában. Ha végzett, mentse a módosítást.

![Webes alkalmazás válasz URL-címe][web-application-reply-url]

### <a name="connect-hello-cluster-by-using-azure-ad-authentication-via-powershell"></a>Hello fürt csatlakoztatása az Azure AD hitelesítési PowerShell használatával
tooconnect hello Service Fabric-fürt, a következő PowerShell-parancs például hello használata:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

toolearn hello Connect-ServiceFabricCluster parancsmaggal kapcsolatban lásd: [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-hello-same-azure-ad-tenant-in-multiple-clusters"></a>Felhasználhatja a több fürt hello ugyanazt az Azure AD-bérlő?
Igen. Ne felejtse tooadd hello URL-címet a Service Fabric Explorer tooyour fürt (webalkalmazás) alkalmazást. Service Fabric Explorer nem működik.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Miért továbbra is szükséges egy kiszolgálói tanúsítványt az Azure AD be van kapcsolva?
FabricClient és FabricGateway a kölcsönös hitelesítést végezni. Az Azure AD-hitelesítés során az Azure AD-integrációs ügyféloldali toohello identitáskiszolgálók kínál, és hello kiszolgálótanúsítvány használt tooverify hello kiszolgáló identitását. A Service Fabric-tanúsítványokkal kapcsolatos további információkért lásd: [X.509-tanúsítványokat és a Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

