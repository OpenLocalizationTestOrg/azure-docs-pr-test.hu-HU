---
title: "aaaAzure virtulálisgép-skálázási készletekben – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toofrequently kérdések a virtuálisgép-méretezési csoportok beolvasása."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: 0deb9e2bb79f87f17bbf748397b94dc53070cfbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Az Azure virtuálisgép-skálázási készletekben – gyakori kérdések

A válaszok kérdések a virtuálisgép-méretezési toofrequently beállítja az Azure-ban.

## <a name="autoscale"></a>Automatikus méretezés

### <a name="what-are-best-practices-for-azure-autoscale"></a>Mik az Azure automatikus skálázás ajánlott eljárásai?

Ajánlott eljárások az automatikus skálázás, lásd: [ajánlott eljárások a virtuális gépek automatikus skálázás](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Hol található a következő automatikus skálázás állomásalapú metrikák használó metrika neve?

Automatikus skálázás állomásalapú metrikák használó mérték nevét, lásd: [támogatott Azure-figyelő metrikák](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Vannak-e az Azure Service Bus témakör és a várólista hossza alapján automatikus skálázás bármely példái?

Igen. Az Azure Service Bus témakör és a várólista hossza alapján automatikus skálázás példákért lásd [Azure figyelő automatikus skálázás közös metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

A Service Bus-üzenetsorba használja a következő JSON hello:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Tároló várólista a következő JSON hello használata:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Cserélje a erőforrás egységes erőforrás-azonosítók (URI).


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Kell I Automatikus méretezéssel állomásalapú metrikák vagy a diagnosztika bővítmény használatával?

Az automatikus skálázási beállítás létrehozhat egy virtuális gép toouse gazdaszintű metrikák vagy Vendég operációsrendszer-alapú metrikákat.

A támogatott mérőszámok listájáért lásd: [Azure figyelő automatikus skálázás közös metrikák](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics). 

A teljes minta a virtuálisgép-méretezési csoportok, lásd: [speciális automatikus skálázás konfigurációs Resource Manager-sablonok használatával virtuálisgép-méretezési csoportok](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets). 

hello minta hello gazdaszintű CPU metrika és üzenet mérőszámot használja.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Hogyan állíthatom riasztási szabályok a virtuálisgép-méretezési csoportot?

A PowerShell vagy az Azure parancssori felület használatával virtuálisgép-méretezési csoportok metrikáját riasztásokat hozhat létre. További információkért lásd: [Azure figyelő PowerShell quick start minták](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) és [figyelő Azure platformfüggetlen parancssori felület gyors start minták](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

hello hello virtuálisgép-méretezési csoport targetresourceid azonosítója így néz ki: 

/Subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/Providers/Microsoft.COMPUTE/virtualMachineScaleSets/yourvmssname

Dönthet úgy, minden virtuális gép teljesítményszámláló hello metrika tooset, egy riasztás. További információkért lásd: [Resource Manager-alapú Windows virtuális gépek vendég operációs rendszer metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) és [Linux virtuális gépek vendég operációs rendszer metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) a hello [Azure figyelő automatikus skálázás közös metrikák](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)cikk.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>Hogyan állíthatom be automatikus skálázás a PowerShell segítségével állítsa be a virtuálisgép-méretezési?

automatikus skálázás a PowerShell segítségével állítsa be a virtuálisgép-méretezési mentése tooset lásd hello blogbejegyzésben [tooadd automatikus skálázás tooan Azure virtuálisgép-méretezési beállításának módját](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).




## <a name="certificates"></a>Tanúsítványok

### <a name="how-do-i-securely-ship-a-certificate-toohello-vm-how-do-i-provision-a-virtual-machine-scale-set-toorun-a-website-where-hello-ssl-for-hello-website-is-shipped-securely-from-a-certificate-configuration-hello-common-certificate-rotation-operation-would-be-almost-hello-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-toodo-this"></a>Hogyan tegye szeretnék biztonságosan küldje el a tanúsítvány toohello VM? Hogyan kiépíteni a virtuális gép méretezési készlet toorun egy webhelyet, ahol hello hello webhelyének SSL nyújtják biztonságosan tanúsítvány konfigurációról? (hello közös tanúsítvány forgatási művelet kellene lennie szinte hello ugyanaz, mint a konfigurációs-frissítési művelet.) Rendelkezik egy példa bemutatja, hogyan toodo Ez? 

toosecurely küldje el a tanúsítvány toohello VM, közvetlenül tanúsítványtárolóba a Windows hello ügyfél kulcstároló a is telepíthet a felhasználói tanúsítványt.

A következő JSON hello használata:

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

hello kód támogatja a Windows és Linux.

További információkért lásd: [létrehozás vagy frissítés egy virtuálisgép-méretezési készlet](https://msdn.microsoft.com/library/mt589035.aspx).


### <a name="example-of-self-signed-certificate"></a>Önaláírt tanúsítvány – példa

1.  Hozzon létre egy önaláírt tanúsítványt kulcstároló.

    A következő PowerShell-parancsok hello használata:

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    A parancs által biztosított, hogy hello hello Azure Resource Manager-sablon a megadott.

    Hogyan toocreate a kulcstároló, önaláírt tanúsítvány: példa [Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

2.  Hello Resource Manager-sablon módosítása

    Ez a tulajdonság túl hozzáadása**virtualMachineProfile**, hello részeként virtuálisgép-méretezési készlet erőforrás:

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-toouse-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Állítható be egy SSH-kulcspár toouse SSH hitelesítés állítson be úgy a Resource Manager-sablon a Linux virtuális gép skálával?  

Igen. a REST API hello **osProfile** hasonló toohello szabványos VM REST API. 

Tartalmaznak **osProfile** a sablonban:

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
Ez a JSON-tömb használatos [hello 101-vm-ssh-kulcsfájl GitHub gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).
 
hello operációsrendszer-profil is van használatban [hello grelayhost.json GitHub quick start sablon](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).

További információkért lásd: [létrehozás vagy frissítés egy virtuálisgép-méretezési készlet](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).
  

### <a name="how-do-i-remove-deprecated-certificates"></a>Hogyan távolíthatom elavult tanúsítványok? 

tooremove tanúsítványok eltávolítása hello régi tanúsítványt hello tároló tanúsítványok listája elavult. Hagyja meg minden hello tanúsítványt, amelyet az tooremain hello listában a számítógépen. Ez a virtuális gépek nem távolítja el hello tanúsítványt. Azt is nem bővíti ki hello tanúsítvány toonew hello virtuálisgép-méretezési csoportban lévő létrehozott virtuális gépeket. 

tooremove hello tanúsítványt a meglévő virtuális gépeken, parancsfájlokat egyéni kiterjesztés toomanually hello tanúsítványok eltávolítása a tanúsítványt a tanúsítványtárolóból.
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-hello-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-toostore-hello-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a>Hogyan do I behelyezése egy meglévő nyilvános SSH-kulcs hello virtuális gép méretezési készlet SSH réteg kiépítése során? I toostore hello SSH nyilvános kulcs értékeit az Azure Key Vault szeretne, és használja a Resource Manager sablon.

Ha meg van adva hello virtuális gépek csak a nyilvános SSH-kulcsot, nem kell tooput hello nyilvános kulcsokat a Key Vault. Nyilvános kulcsai nem titkos.
 
Megadhat egyszerű szöveges nyilvános SSH-kulcsok Linux virtuális gép létrehozásakor:

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
linuxConfiguration elem neve | Szükséges | Típus | Leírás
--- | --- | --- | --- |  ---
ssh | Nem | Gyűjtemény | Hello SSH kulcs konfigurációját a Linux operációs rendszer
Elérési út | Igen | Karakterlánc | Hello Linux elérési útjának megadása ahol hello SSH-kulcsok vagy tanúsítványt kell lennie
keyData | Igen | Karakterlánc | Megadja a base64-kódolású nyilvános SSH-kulcs

Egy vonatkozó példáért lásd: [hello 101-vm-ssh-kulcsfájl GitHub gyors üzembe helyezési sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-hello-same-key-vault-i-see-hello-following-message"></a>Futtatásakor `Update-AzureRmVmss` egynél több tanúsítvány hozzáadása a hello után ugyanaz a kulcs tárolóban, látom hello a következő üzenetet:
 
>Frissítés-AzureRmVmss: Lista titkos kulcsot tartalmaz többszöri /subscriptions/ < saját előfizetés-azonosító > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, ami nem engedélyezett.
 
Ez akkor fordulhat elő, ha toore-hozzáadása hello azonos tároló hello meglévő forrás tároló egy új tároló tanúsítvány használata helyett. Hello `Add-AzureRmVmssSecret` parancs nem működik megfelelően, ha további titkok hozzáadni.
 
tooadd hello a további titkok azonos kulcstároló, a frissítés hello $vmss.properties.osProfile.secrets[0].vaultCertificates lista.
 
Hello várt bemeneti szerkezetből, lásd: [létrehozás vagy frissítés egy virtuálisgép-csoport](https://msdn.microsoft.com/library/azure/mt589035.aspx).
 
Hello virtuális gép méretezési készlet objektum, amely a key vaultban hello hello titkos kulcs található. Adja hozzá a tanúsítvány (hello URL-cím és hello titkos Tárolónév) toohello hivatkozáslista társított hello tárolóban.

> [!NOTE] 
> Jelenleg nem távolítható el tanúsítványok virtuális gépek hello virtuális gép méretezési készlet API használatával.
>

Új virtuális gépek nem fognak rendelkezni hello régi tanúsítvány. Hello tanúsítvány van, és a már telepített virtuális gépek azonban hello régi tanúsítvánnyal rendelkezik.
 
### <a name="can-i-push-certificates-toohello-virtual-machine-scale-set-without-providing-hello-password-when-hello-certificate-is-in-hello-secret-store"></a>Leküldéses tanúsítványok virtuálisgép-méretezési toohello nélkül hello jelszót, ha hello tanúsítvány titkos tárolójában hello beállítása is?

Nem kell toohard-kód jelszavak parancsfájlokban. Jelszavak dinamikusan lekérdezésére hello engedélyekkel toorun hello telepítési parancsfájl használatára. Ha egy parancsfájlt, mely egy tanúsítványt az hello titkos tároló kulcstároló, hello titkos tároló `get certificate` parancs is kiírja hello jelszó hello .pfx fájl.
 
### <a name="how-does-hello-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-hello-sourcevault-value-when-i-have-toospecify-hello-absolute-uri-for-a-certificate-by-using-hello-certificateurl-property"></a>Hogyan hello titkok tulajdonsága a virtuálisgép-méretezési virtualMachineProfile.osProfile munkahelyi be? Miért kell meg hello sourceVault értéket, ha abszolút URI-azonosító tanúsítványt hello hello certificateUrl tulajdonság használatával toospecify? 

A Rendszerfelügyeleti (webszolgáltatások WinRM) tanúsítványhivatkozást hello hello operációsrendszer-profilt a titkos kulcsok tulajdonság jelen kell lennie. 

hello hello forrás tároló jelző célja tooenforce hozzáférés-vezérlési lista (ACL) házirendeket, amelyek a felhasználó Azure Cloud Service modell szerepel. Ha hello forrás tárolóban nincs megadva, a felhasználók, akik nem rendelkeznek engedéllyel toodeploy vagy hozzáférési kulcsait tooa kulcstároló lenne képes toothrough számítási erőforrás szolgáltató (CRP). Hozzáférés-vezérlési listák létezik még a erőforrásokat, amelyek nem léteznek.

Egy nem megfelelő tároló Forrásazonosítóval azonban egy érvényes kulcstároló URL-címet ad meg, ha rendszer hibát jelez, akkor kérdezze le a hello művelet során.
 
### <a name="if-i-add-secrets-tooan-existing-virtual-machine-scale-set-are-hello-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Ha a titkos kulcsok tooan meglévő virtuálisgép-méretezési csoport hozzáadásához vannak hello titkok beszúrta meglévő virtuális gépekhez, vagy csak újakat? 

Tanúsítványok tooall ad hozzá a virtuális gépeken, akár már létező azokat. Ha a virtuálisgép-méretezési készlet upgradePolicy tulajdonság értéke túl**manuális**, hello tanúsítvány kerül toohello VM hello VM kézi frissítés végrehajtásakor.
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a>Ha put tanúsítványok Linux virtuális gépekhez?

Hogyan toodeploy tanúsítványok Linux virtuális gép esetében: toolearn [telepíteni az ügyfél által felügyelt kulcstároló tanúsítványok tooVMs](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).
  
### <a name="how-do-i-add-a-new-vault-certificate-tooa-new-certificate-object"></a>Hogyan hozzá egy új tároló tooa új tanúsítvány tanúsítványobjektum?

tooadd tároló tanúsítvány tooan meglévő titkos kulcs, tekintse meg a következő PowerShell-példa hello. Csak egy titkos objektum használja.
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-toocertificates-if-you-reimage-a-vm"></a>Toocertificates mi történik, ha a virtuális gépek újból lemezképet létrehozni?

Ha Ön újból lemezképet létrehozni egy virtuális Gépet, a tanúsítványok törlődnek. Különösen törlések hello teljes operációsrendszer-lemezt. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-hello-key-vault"></a>Mi történik, ha a tanúsítvány törlése hello kulcstároló?

Ha a hello titkos hello kulcstároló törlődik, és ezt követően futtatni `stop deallocate` a virtuális géphez, majd indítsa el újra azokat hiba fog történni. hello hiba fordul elő, mert hello KSZT kell tooretrieve hello titkokat a hello kulcstároló, de ez nem lehetséges. Ebben a forgatókönyvben a virtuális gép méretezési modellel hello hello tanúsítványok törölheti. 

hello KSZT összetevő nem maradnak ügyfél titkos kulcsok. Ha `stop deallocate` hello virtuálisgép-méretezési csoportban lévő összes virtuális gép esetében hello gyorsítótár törlődik. Ebben a forgatókönyvben a titkos kulcsok lekért hello kulcstároló.

Ez a probléma nem tapasztal, amikor kiterjesztése, mert nincs a gyorsítótárazott másolatának hello titkos kulcsot az Azure Service Fabric (hello bérlő egyetlen-háló modell).
 
### <a name="why-do-i-have-toospecify-hello-exact-location-for-hello-certificate-url-httpsname-of-hello-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Miért kell toospecify hello pontos helyét hello tanúsítvány URL-címe (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), a [Service Fabric-fürt biztonsági forgatókönyvek](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?
 
hello Azure Key Vault dokumentáció szerint az adott hello beolvasni a titkos kulcs REST API hello hello titkos legújabb verzióját kell visszaadnia, ha nincs megadva hello verziója.
 
Módszer | URL-CÍME
--- | ---
GET | https://mykeyvault.vault.Azure.NET/secrets/ {titkos kulcs neve} / {titkos-version}? api-version = {api-version}

Cserélje le {*titkos-név*} hello névvel, és cserélje le {*titkos verziójú*} hello titkos hello verziójával tooretrieve szeretné. Előfordulhat, hogy zárva hello titkos kulcs verzióját. Ebben az esetben beolvasott hello jelenlegi verziójával.
  
### <a name="why-do-i-have-toospecify-hello-certificate-version-when-i-use-key-vault"></a>Miért van meg toospecify hello tanúsítvány verziója, Key Vault használatakor?

hello hello Key Vault követelmény toospecify hello tanúsítvány verziója célja toomake, akkor törölje a jelet toohello felhasználó milyen tanúsítvány van telepítve. a virtuális gépeken.

Ha egy virtuális gép létrehozása, és ezután frissítse a titkos kód hello a key vaultban, hello új tanúsítvány nincs letöltött tooyour virtuális gépek. De a virtuális gépek és az új virtuális gépek hello új titkos kulcs lekérése tooreference jelennek meg. tooavoid, a titkos kulcs verzióját szükséges tooreference áll.

### <a name="my-team-works-with-several-certificates-that-are-distributed-toous-as-cer-public-keys-what-is-hello-recommended-approach-for-deploying-these-certificates-tooa-virtual-machine-scale-set"></a>A csoport több olyan tanúsítvány, mint .cer nyilvános kulcsok toous elosztott működik. Mi az ajánlott módszer a tanúsítványok tooa virtuálisgép-méretezési csoport telepítéséhez hello?

toodeploy .cer nyilvános kulcsok tooa virtuális gép méretezési, létrehozhat egy .pfx-fájlt, amely csak a .cer fájlt tartalmaz. toodo ehhez, `X509ContentType = Pfx`. Például C# vagy PowerShell x509Certificate2 objektumként hello .cer fájl betöltése, és ezután hívja meg hello metódust. 

További információkért lásd: [X509Certificate.Export metódus (X509ContentType, karakterlánc)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="i-do-not-see-an-option-for-users-toopass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a>A beállítás a tanúsítványok felhasználók toopass base64 karakterláncként nem látható. A legtöbb más erőforrás-szolgáltatók kell ezt a beállítást.

egy tanúsítványt, mint a Base64 kódolású karakterlánc benyújtása tooemulate kibonthatja hello legújabb verzióval ellátott URL-címet a Resource Manager-sablon. A következő JSON-tulajdonság az erőforrás-kezelő sablonban hello a következők:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-toowrap-certificates-in-json-objects-in-key-vaults"></a>Van toowrap tanúsítványok a JSON-objektumok a kulcstárolók?

A virtuálisgép-méretezési csoportok és a virtuális gépek tanúsítványokat kell csomagolni, a JSON-objektumok. 

Alkalmazás/x-pkcs12 hello tartalomtípus is támogatja. Alkalmazás/x-pkcs12 használatával, lásd: [PFX-tanúsítványokat az Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).
 
Jelenleg nem támogatjuk .cer kiterjesztésű fájlokat. toouse .cer fájlt, a .pfx-tárolók exportálhatja őket.



## <a name="compliance"></a>Megfelelőség

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Azok a virtuálisgép-skálázási készletekben PCI-kompatibilis?

Virtuálisgép-méretezési csoportok vékony API felső szintű rétege hello KSZT. Összetevőket egyaránt hello számítógépes platform hello Azure szolgáltatás fájában a részét képezik.

A megfelelőség szempontjából a virtuálisgép-méretezési csoportok hello Azure számítási platform alapvető részét képezik. Egy csoport, eszközök, folyamatok, telepítési módszer, biztonsági vezérlők, just-in-time (JIT) fordítási, figyelés, riasztás, és így tovább, azok megosztása hello KSZT magát. Virtuálisgép-méretezési csoportok a Payment Card Industry (PCI)-kompatibilis, mert a KSZT hello hello aktuális PCI adatok biztonsági szabvány (DSS) igazolási része.

További információkért lásd: [Microsoft Trust Center hello](https://www.microsoft.com/TrustCenter/Compliance/PCI).






## <a name="extensions"></a>Bővítmények

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>A virtuálisgép-méretezési készlet bővítmény törlése

egy virtuálisgép-méretezési toodelete virtuáliskapcsoló-kiterjesztés, a következő PowerShell-példa használata hello beállítása:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
Hello bővítménynév érték található `$vmss`.
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a>Egy virtuális gép méretezési sablon példa, amely az Operations Management Suite?

A virtuális gép méretezési sablon példa, amely az Operations Management Suite, lásd: hello második példáját [Azure Service Fabric-fürt üzembe helyezése, és engedélyezze a megfigyelést Naplóelemzési használatával](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).
   
### <a name="extensions-seem-toorun-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-toofail-what-can-i-do-toofix-this"></a>Bővítmények toorun párhuzamos virtuálisgép-méretezési csoportok tűnnek. Ennek hatására a saját egyéni parancsfájl-kiterjesztés toofail. Mi a teendő toofix Ez?

Virtuálisgép-méretezési csoportok, a bővítmény alkalmazás-előkészítés kapcsolatos toolearn lásd [bővítmény alkalmazás-előkészítés az Azure virtuálisgép-méretezési csoportok](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).
 
 
### <a name="how-do-i-reset-hello-password-for-vms-in-my-virtual-machine-scale-set"></a>Hogyan tegye alaphelyzetbe állít egy hello jelszó virtuális gépek a virtuálisgép-méretezési csoportban lévő?

a virtuálisgép-méretezési a virtuális gépek tooreset hello jelszó megadásához használja a virtuális gép eléréséhez kapcsolódó kiegészítő mezőket. 

A következő PowerShell-példa hello használata:

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-tooall-vms-in-my-virtual-machine-scale-set"></a>Hogyan hozzá a virtuálisgép-méretezési csoportban lévő egy bővítmény tooall virtuális gépek?

Ha a házirend értéke túl**automatikus**, hello sablon ismételt üzembe helyezéssel hello új bővítmény tulajdonságokkal rendelkező összes virtuális gépet frissíti.

Ha a házirend értéke túl**manuális**, előbb módosítsa a hello kiterjesztését, és manuálisan frissítse a virtuális gépek összes példányán.

  
### <a name="if-hello-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-hello-vms-not-match-hello-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-hello-scripts-that-are-currently-configured-on-hello-virtual-machine-scale-set-executed-or-are-hello-scripts-that-were-configured-when-hello-vm-was-first-created-used"></a>Ha egy meglévő virtuálisgép-méretezési csoport társított hello bővítmények frissítése, a meglévő virtuális gépek hatással? (Ez azt jelenti, hogy a rendszer hello virtuális gépek *nem* egyezés hello virtuális gép méretezési modellel?) Vagy azok figyelmen kívül hagy? Ha egy meglévő számítógép szolgáltatás beforrott vagy lemezképet, hello olyan parancsfájlok, amelyek jelenleg be van állítva a hello virtuálisgép-méretezési csoport hajtotta végre, vagy hello parancsfájlok konfigurált virtuális gép létrehozásának hello használata esetén?

Ha a virtuálisgép-méretezési hello hello-bővítmény definíciójának modell frissül, és hello upgradePolicy tulajdonsága túl**automatikus**, hello virtuális gépek frissíti. Ha hello upgradePolicy tulajdonsága túl**manuális**, bővítmények megjelölt hello modell nem megfelelő. 

Ha egy meglévő virtuális gép szolgáltatás beforrott, újraindítás jelenik meg, és vannak hello bővítmények nem futtatják újra. Ha rendszerképének, például az operációs rendszer hello meghajtó lecserélését hello forráslemezkép. Bármely futtató és specializációt hello legújabb modellből, például a bővítmények, futtathatók.
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-tooan-azure-ad-domain"></a>Hogyan virtuális gép méretezési készlet tooan az Azure AD tartományhoz csatlakozni?

toojoin egy virtuális gép méretezési készlet tooan Azure Active Directory (Azure AD) tartományhoz, definiálhat egy bővítmény. 

egy bővítmény toodefine hello JsonADDomainExtension tulajdonság használja:

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-tooinstall-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>A virtuálisgép-méretezési készlet bővítmény próbál tooinstall, amelyet újra kell indítani. Például: "commandToExecute": "powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature-Name FS-Resource-Manager – IncludeManagementTools"

Ha a virtuálisgép-méretezési készlet bővítmény próbál tooinstall, amelyet újra kell indítani, használhatja a hello Azure Automation célállapot-konfiguráció (Automation DSC) bővítmény. Windows Server 2012 R2 hello operációs rendszer esetén a Azure lekéri a hello a Windows Management Framework (WMF) 5.0 telepítés újraindítások idejére, és a Folytatás hello konfigurációjával kapcsolatban. 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Hogyan bekapcsolni kártevőirtó a virtuálisgép-méretezési csoportban lévő?

a antimalware-t a virtuálisgép-méretezési tooturn megadásához használja a következő PowerShell-példa hello:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve hello most recent version number of hello extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-tooexecute-a-custom-script-thats-hosted-in-a-private-storage-account-hello-script-runs-successfully-when-hello-storage-is-public-but-when-i-try-toouse-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a>Egyéni parancsfájl, amely egy privát tárfiókban lévő tooexecute kell. hello parancsprogram sikeresen lefut, nyilvános hello tárolás esetén, de közben toouse egy közös hozzáférésű Jogosultságkód (SAS), az nem sikerül. Ez az üzenet jelenik meg: "Kötelező paraméter hiányzik az érvényes a közös hozzáférésű Jogosultságkód". Hivatkozás + SAS jól működik a helyi böngészőből.

személyes tárfiókokban, üzemeltetett egyéni parancsfájl tooexecute hello tárfiók hívóbetűjét, a név védett beállítások megadása. További információkért lásd: [egyéni parancsfájl kiterjesztése a Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).







## <a name="networking"></a>Hálózat
 
### <a name="is-it-possible-tooassign-a-network-security-group-nsg-tooa-scale-set-so-that-it-will-apply-tooall-hello-vm-nics-in-hello-set"></a>Azt a hálózati biztonsági csoport (NSG) tooa méretezési, lehetséges tooassign azért, hogy tooall hello virtuális gép hálózati adapterek hello készlet fog alkalmazni?

Igen. Hálózati biztonsági csoport közvetlen tooa méretezési hello networkinterfaceconfigurations tulajdonságok szakaszának hello hálózati profil Vezérlőpultjának lehet alkalmazni. Példa:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-hello-same-subscription-and-same-region"></a>Hogyan egy virtuális IP-címcsere a virtuálisgép-skálázási készletekben hello azonos előfizetésben és ugyanabban a régióban?

Ha két virtuális virtulálisgép-skálázási készletekben az Azure Load Balancer első-végpontok, és vannak hello azonos előfizetésével és régiójával, képes mindegyiknél hello nyilvános IP-címek felszabadítása, és rendelje hozzá a többi toohello. Lásd: [virtuális IP-Címcsere: kék-zöld telepítése Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) például. Ez utalnak késleltetés, bár, hello erőforrások felszabadítása/lefoglalt hello szintjén. A gyorsabb elem toouse Azure Application Gateway két háttérkészletek menüpontot, és útválasztási szabályainak. Alternatív megoldásként az alkalmazásba sikerült működteti [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) amelyek támogatást nyújt az gyors váltás átmeneti és üzemi tárhelyek között.
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-toouse-for-static-private-ip-address-allocation"></a>Hogyan adja meg a privát IP-címek toouse statikus magánhálózati IP-címek hozzárendelését a számos?

Megadott alhálózatból származó IP-címek van kiválasztva. 

hello elosztási módszert a virtuális gép méretezési készlet IP-címek mindig "dinamikus" azonban, hogy nem jelenti azt, hogy az IP-címeket módosíthatja. Ebben az esetben "dinamikus" csak azt jelenti, hogy nincs megadva hello IP-címet a PUT kérelmek. Adjon meg statikus hello hello alhálózat-beállításokat. 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-tooan-existing-azure-virtual-network"></a>Hogyan telepíthető egy virtuális gép méretezési készlet tooan meglévő Azure virtuális hálózat? 

a virtuálisgép-méretezési toodeploy tooan meglévő Azure virtuális hálózat, olvassa el [központi telepítése egy virtuális gép méretezési készlet tooan meglévő virtuális hálózat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet). 

### <a name="how-do-i-add-hello-ip-address-of-hello-first-vm-in-a-virtual-machine-scale-set-toohello-output-of-a-template"></a>Hogyan hello IP-cím hozzáadása a hello a virtuálisgép-méretezési első Virtuálisgép-sablon toohello kimeneti csoportban?

tooadd hello IP-címe hello első virtuális gép egy virtuális gép méretezési készlet toohello kimenet egy sablon, lásd: [ARM: első VMSS privát IP-címek](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Méretezési készlet használható a hálózat elérését gyorsítja fel?

Igen. hálózat, az elérését gyorsítja fel toouse beállítása enableAcceleratedNetworking tootrue a méretezési készlet által networkinterfaceconfigurations tulajdonsággal. Például
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-hello-dns-servers-used-by-a-scale-set"></a>Hogyan konfigurálható a méretezési készlet használt hello DNS-kiszolgálók?

toocreate egy Virtuálisgép-méretezési készlet egy egyéni DNS-konfigurációval, vegye fel a dnsSettings JSON csomag toohello méretezési networkinterfaceconfigurations tulajdonságok szakasz. Példa:
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-tooassign-a-public-ip-address-tooeach-vm"></a>Hogyan konfigurálható a méretezési készlet tooassign egy nyilvános IP-cím tooeach VM?

a Virtuálisgép-méretezési készlet, amely toocreate hozzárendel egy nyilvános IP-cím tooeach VM, legyen hello Microsoft.Compute/virtualMAchineScaleSets erőforrás hello API verziója 2017-03-30, és adja hozzá a _publicipaddressconfiguration_ JSON-csomag a méretezési toohello IP-konfigurációk szakasz. Példa:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-toowork-with-multiple-application-gateways"></a>Beállítható a méretezési készlet toowork több Alkalmazásátjárót?

Igen. Hozzáadhat hello erőforrás azonosítók több Application Gateway háttér cím készletek toohello _applicationGatewayBackendAddressPools_ hello listájában _IP-konfigurációk_ a méretezési szakasz hálózati profilt.

## <a name="scale"></a>Méretezés

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>Milyen esetben volna létrehozni egy virtuálisgép-méretezési beállítása kevesebb mint két virtuális gépek?

Egyik oka toocreate egy virtuális gép méretezési csoportban kevesebb mint két virtuális toouse hello rugalmas tulajdonságait egy virtuálisgép-méretezési van beállítva. Például telepíthet egy virtuálisgép-méretezési csoport a nulla virtuális gépek toodefine az infrastruktúra nélkül költségek futtató virtuális fizet. Ezután amikor készen áll a toodeploy virtuális gépeket, megnöveli hello "" hello virtuális gép méretezési készlet toohello éles példányok száma.

Előfordulhat, hogy hozzon létre egy virtuálisgép-méretezési kevesebb mint két virtuális gépek állítsa be a másik OK, ha aggódik, kevesebb, mint a rendelkezésre állási készlet különálló virtuális gépek használatával rendelkezésre állással. Virtuálisgép-méretezési csoportok olyan módon toowork magánháztartás számítási egységek használatával, amelyek helyettesíthető biztosítják. Ez egységes a legfontosabb különbséget a virtuálisgép-méretezési csoportok és a rendelkezésre állási készletek. Sok állapot nélküli munkaterheléseket nem követik nyomon egyedi. Hello terhelés esik, csökkentheti a tooone számítási egység, és majd vertikális felskálázás toomany, amikor hello munkaterhelés növeli.

### <a name="how-do-i-change-hello-number-of-vms-in-a-virtual-machine-scale-set"></a>Hogyan változtathatom hello virtuálisgép-méretezési csoportban lévő virtuális gépek száma?

Tekintse meg a toochange hello virtuális gépek száma a virtuálisgép-méretezési csoportban lévő [hello példányok száma a virtuálisgép-méretezési csoport módosítása](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Hogyan határozza meg az egyéni riasztások az egyes küszöbértékek elérésekor?

Hogyan kezeli a megadott küszöbértékek riasztások néhány beleszólása van. Például a testreszabott webhookok adhat meg. a következő webhook példa hello Resource Manager sablon alapján van:

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

Ebben a példában egy riasztás tooPagerduty.com kerül, a küszöbérték elérésekor.



## <a name="patching-and-operations"></a>Javítás és műveletek

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a>Hogyan hozható létre egy meglévő erőforráscsoportot beállított terjedő skálán?

Méretezési csoportok létrehozása a meglévő erőforrás csoport még nem lehetséges a hello Azure-portálon, de megadhat egy létező erőforráscsoportot, amikor egy méretezési telepítése Azure Resource Manager-sablonok állítható be. A méretezési készletben, Azure PowerShell vagy a parancssori felület használatával létrehozásakor egy meglévő erőforráscsoportot is megadható.

### <a name="can-we-move-a-scale-set-tooanother-resource-group"></a>A Microsoft azt áthelyezheti egy méretezési tooanother erőforráscsoport?

Igen, áthelyezheti méretezési készlet erőforrások tooa új előfizetéshez vagy erőforráscsoporthoz.

### <a name="how-tooi-update-my-virtual-machine-scale-set-tooa-new-image-how-do-i-manage-patching"></a>Hogyan tooI frissíteni a virtuális gép méretezési tooa új lemezkép? Hogyan kezelhetők a javítás?

a virtuális gép méretezési tooa új lemezképet, és a javítás toomanage, tooupdate lásd [frissítéséhez egy virtuálisgép-méretezési csoport](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-hello-reimage-operation-tooreset-a-vm-without-changing-hello-image-that-is-i-want-reset-a-vm-toofactory-settings-rather-than-tooa-new-image"></a>Hello kép megváltoztatása nélkül is használható hello lemezkép-visszaállítási művelet tooreset egy virtuális Gépet? (Ez azt jelenti, hogy kívánt alaphelyzetbe állítása a virtuális gép toofactory beállításait ahelyett, hogy új lemezképet tooa.)

Igen, hello kép megváltoztatása nélkül hello lemezkép-visszaállítási művelet tooreset egy virtuális Gépet is használhatja. Azonban, ha a virtuálisgép-méretezési csoport platform lemezképre hivatkozik a `version = latest`, a virtuális gép tooa frissítheti az operációs rendszer újabb hívásakor kép `reimage`.

További információkért lásd: [virtuálisgép-méretezési csoportban lévő összes virtuális gépek kezeléséhez](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).



## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Hogyan rendszerindítási diagnosztika bekapcsolásához?

a rendszerindítási diagnosztika tooturn először hozzon létre egy tárfiókot. Ez a JSON-tömb, majd be a virtuálisgép-méretezési csoport **virtualMachineProfile**, és hello virtuálisgép-méretezési csoport frissítése:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

Egy új virtuális gép létrehozásakor hello hello VM InstanceView tulajdonsága hello képernyőképe, és így tovább hello részleteit jeleníti meg. Íme egy példa:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a>Virtuális gép tulajdonságai

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-hello-fault-domain-for-each-of-hello-100-vms-in-my-virtual-machine-scale-set"></a>Hogyan szerezhetek tulajdonság adatait az egyes virtuális gépek több hívás nélkül? Például hogyan kellene elérni hello tartalék tartomány minden hello 100 virtuális gépek a virtuálisgép-méretezési csoportban lévő?

tooget tulajdonság információ az egyes virtuális gépek anélkül, hogy több hívások hívása `ListVMInstanceViews` egy REST API végrehajtásával `GET` az erőforrás URI azonosítója a következő hello:

/Subscriptions/ < ELŐFIZETÉS_AZONOSÍTÓJA > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtuális gépek vannak? $expand argumentum = instanceView & $select = instanceView

### <a name="can-i-pass-different-extension-arguments-toodifferent-vms-in-a-virtual-machine-scale-set"></a>I teljen különböző argumentumok toodifferent virtuális gépek egy virtuálisgép-méretezési csoportban lévő?

Nem, nem adható át másik argumentumok toodifferent virtuális gépek egy virtuálisgép-méretezési csoportban lévő. Bővítmények azonban egyedi tulajdonságainak hello hello futtatnak hello számítógépnév meg például a virtuális gép alapján működhet. Bővítmények is lekérdezheti a példány metaadatainak az http://169.254.169.254 tooget hello VM további információt.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Miért van a virtuális gép méretezési VM számítógép nevének és a virtuális gép azonosítók között lévő hézagokat? Például: 0, 1, 3...

Nincsenek a virtuális gép méretezési VM számítógép nevének és a virtuális gép azonosítók között lévő hézagokat, mert a virtuálisgép-méretezési csoport **szükségesnél több erőforrás** tulajdonsága toohello alapértelmezett értékének **igaz**. Ha túl elhelyezésétől értéke**igaz**, mint a kért jönnek létre további virtuális gépek. Extra virtuális Gépekért törlődnek. Ebben az esetben azt szerezhet telepítés megbízhatóságát, azonban összefüggő elnevezési és összefüggő hálózati címfordítás (NAT) hello költségén szabályok. 

Ez a tulajdonság túl állíthatja be**hamis**. Kis virtuálisgép-méretezési csoportok, a központi telepítés megbízhatóság jelentősen ez nincs hatással.

### <a name="what-is-hello-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-hello-vm-when-should-i-choose-one-over-hello-other"></a>Mi az, hogy a virtuálisgép-méretezési csoportban lévő virtuális gép törlése és hello virtuális gép felszabadítása hello különbségének? Amikor kell választani egy más hello?

hello virtuálisgép-méretezési csoportban lévő virtuális gép törlése és hello virtuális gép felszabadítása közötti fő különbség, hogy `deallocate` hello virtuális merevlemezeket (VHD) nem törölhető. Nincsenek futó társított tárolási költségek `stop deallocate`. Előfordulhat, hogy egy alkalmazni vagy hello más hello a következő okok valamelyike miatt:

- Azt szeretné, hogy a számítási költségek fizető toostop, de azt szeretné, hogy a virtuális gépek hello tookeep hello lemez állapotát.
- Virtuális gépek halmaza toostart gyorsabban sikerült horizontális egy virtuálisgép-méretezési csoport szeretné.
  - Kapcsolódó toothis esetben előfordulhat, hogy létrehozta a saját automatikus skálázás-motor és a kívánt gyorsabb végpontok közötti skálán.
- A virtuálisgép-méretezési csoport, amely nem egyenlően elosztva tartalék tartományok vagy a frissítési tartományok rendelkezik. Ez lehet, mert szelektív módon törli a virtuális gépek vagy virtuális gépek elhelyezésétől után törölték. Futó `stop deallocate` követ `start` hello a virtuális gép méretezési készletben egyenletes elosztása az hello virtuális gépek tartalék tartományok vagy a frissítési tartományok.

