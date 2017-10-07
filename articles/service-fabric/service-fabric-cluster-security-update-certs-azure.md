---
title: "az Azure Service Fabric-fürt aaaManage tanúsítványok |} Microsoft Docs"
description: "Ismerteti, hogyan tooadd új tanúsítványokat, a helyettesítő tanúsítványt és a remove tanúsítvány tooor a Service Fabric-fürt."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: 8e57bd95dbb800ecc04cf6988047e3abdc2fe56a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Hozzáadhat és eltávolíthat tanúsítványokat a Service Fabric-fürtök az Azure-ban
Javasoljuk, hogy megismerje a módját a Service Fabric X.509-tanúsítványokat használ, és hello ismernie kell a [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md). Ismernie kell fürt tanúsítvány és felhasználási területének, mielőtt végrehajtásának folytatásához.

Megadja, hogy két fürt tanúsítványok, egy elsődleges és másodlagos, amikor konfigurálja a szolgáltatás háló lehetővé biztonsági tanúsítvány tooclient tanúsítványok hozzáadása a fürt létrehozása során. Tekintse meg a túl[létrehozása egy azure-portálon fürttel](service-fabric-cluster-creation-via-portal.md) vagy [létrehozása az azure-fürttel Azure Resource Manageren keresztül](service-fabric-cluster-creation-via-arm.md) azok beállításával kapcsolatos részleteket létre idő. Ha csak egy fürt tanúsítványt ad meg, létrehozás ideje, majd használt hello elsődleges tanúsítványként. Fürt létrehozása után hozzáadhat egy új tanúsítványt, a egy másodlagos.

> [!NOTE]
> Biztonságos fürt esetén mindig szüksége lesz legalább egy érvényes (a nem visszavont és a nem lejárt) fürtre (elsődleges vagy másodlagos) telepített tanúsítvány (Ha nem, hello fürt leáll a működése). 90 napig minden érvényes tanúsítvány lejárati, elérni hello rendszer állít elő, egy figyelmeztetés nyomkövetést, és egy figyelmeztetés állapotesemény hello csomóponton. Jelenleg nincs e-mail vagy más, ebben a témakörben a küld a service fabric értesítést. 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-hello-portal"></a>Hello portálon fürt másodlagos tanúsítvány hozzáadásához

Másodlagos fürt tanúsítvány nem adható hozzá hello Azure-portálon keresztül. Toouse Azure rendelkezik, amely a powershell. a dokumentum későbbi szakaszában meghatározott hello folyamat.

## <a name="swap-hello-cluster-certificates-using-hello-portal"></a>A felcserélendő hello fürt tanúsítványok hello portál használatával

Miután sikeresen telepítette a fürt másodlagos tanúsítvány, ha azt szeretné, tooswap hello elsődleges és másodlagos, majd keresse meg a toohello biztonsági panel, és hello "Elsődleges felcserélés" beállítást választva hello helyi menü tooswap hello másodlagos cert a hello elsődleges cert.

![Lapozófájl-kapacitás tanúsítvány][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-hello-portal"></a>Távolítsa el a fürt tanúsítványt hello portál használatával

Biztonságos fürt esetén mindig szüksége lesz legalább egy érvényes (a nem visszavont és a nem lejárt) (elsődleges vagy másodlagos) telepített tanúsítvány Ha nem, hello fürt megszűnik működni.

a fürt biztonsági, Navigálás toohello biztonsági panel és select hello "Delete" kapcsolót hello helyi menüből hello másodlagos tanúsítvány használatát másodlagos tanúsítvány tooremove.

Ha a szándéka az elsődleges, van megjelölve, akkor kell tooswap tooremove hello tanúsítványt a másodlagos hello először, és törölje hello másodlagos, hello frissítés befejeződése után.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Erőforrás-kezelő Powershell-lel másodlagos tanúsítvány hozzáadásához

Ezek a lépések feltételezik, hogy ismeri a Resource Manager működésével és legalább egy Service Fabric-fürt Resource Manager-sablon használatával telepített, és be lesz szüksége hello fürtöt tooset használt hello sablont. Az Ön számára elfogadható JSON használatával is feltételezzük.

> [!NOTE]
> Ha a keresett mintasablon és, hogy toofollow mentén vagy kiindulási pontként használható paramétereket, majd töltse le azt a ez [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>A Resource Manager sablon szerkesztése

A következő mentén megkönnyítése érdekében minta 5-VM-1-NodeType tulajdonságok értéke-Secure_Step2.JSON frissítjük összes hello módosításokat tartalmazza. hello minta érhető el: [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Győződjön meg arról, hogy toofollow hello lépéseket**

**1. lépés:** hello Resource Manager sablon, amelyet fürtbe toodeploy nyissa meg. (Ha hello minta letöltötte a fenti tárház hello, majd használja az 5-VM-1-NodeType tulajdonságok értéke-Secure_Step1.JSON toodeploy biztonságos fürt, és nyisson meg, hogy a sablon mentése).

**2. lépés:** Hozzáadás **két új paraméterrel** "secCertificateThumbprint" és "secCertificateUrlValue" típusú "string" toohello paraméter szakasza a sablont. A következő kódrészletet hello lemásolhatja és toohello sablon hozzáadása. Attól függően, hogy a sablon hello forrását már előfordulhat, hogy ezeknek a definiált, ha így folytassa toohello következő lépésre. 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

**3. lépés:** módosítása toohello **Microsoft.ServiceFabric/clusters** erőforrás - keresse meg a sablon hello "Microsoft.ServiceFabric/clusters" erőforrás-definícióban. Az adott definíció tulajdonságai "Tanúsítvány" JSON található címke, amely a következő JSON részlet hello hasonlóan kell kinéznie:

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Új címke "thumbprintSecondary" hozzáadása, és adjon neki egy "[parameters('secCertificateThumbprint')]" értéket.  

Igen, most hello erőforrás-definíció hello hasonlóan kell kinéznie (attól függően, hogy a forrás hello sablon, nem lehet pontosan például az alábbi hello részlet). 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

Ha azt szeretné, túl**helyettesítő hello cert**, majd adja meg az új tanúsítvány hello az elsődleges és a mozgási hello aktuális elsődleges, a másodlagos. Ez az aktuális elsődleges tanúsítvány toohello új tanúsítvány egy központi telepítési lépésben hello helyettesítő eredményez.

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


**4. lépés:** módosítása túl**összes** hello **Microsoft.Compute/virtualMachineScaleSets** erőforrás-definíciókban - hello Microsoft.Compute/virtualMachineScaleSets erőforrás található. definíciója. Görgessen toohello "publisher": "Microsoft.Azure.ServiceFabric", "virtualMachineProfile" alatt.

A hello szolgáltatás háló publisher beállításaiban megtekintheti az alábbihoz hasonló.

![Json_Pub_Setting1][Json_Pub_Setting1]

Új tanúsítvány bejegyzések tooit hello hozzáadása

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

hello tulajdonságok most példához hasonló

![Json_Pub_Setting2][Json_Pub_Setting2]

Ha azt szeretné, túl**helyettesítő hello cert**, majd adja meg az új tanúsítvány hello az elsődleges és a mozgási hello aktuális elsődleges, a másodlagos. Ez az aktuális tanúsítvány toohello új tanúsítványt egy központi telepítési lépésben hello kapcsolódó kulcsváltást eredményez. 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
hello tulajdonságok most példához hasonló

![Json_Pub_Setting3][Json_Pub_Setting3]


**5. lépés:** módosításokat túl**összes** hello **Microsoft.Compute/virtualMachineScaleSets** erőforrás-definíciókban - hello Microsoft.Compute/virtualMachineScaleSets erőforrás található. definíciója. Görgessen toohello "vaultCertificates":, a "OSProfile". akkor kell kinéznie.


![Json_Pub_Setting4][Json_Pub_Setting4]

Adja hozzá a hello secCertificateUrlValue tooit. a következő kódrészletet hello használata:

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
Most Json eredő hello kell kinéznie.
![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Győződjön meg arról, hogy Ön a következő ismétlődő 4. és 5 Nodetypes/Microsoft.Compute/virtualMachineScaleSets erőforrás-definíciók minden hello a sablonban. Ha kihagyná valamelyik őket, hello tanúsítvány nem az adott VMSS beolvasása telepítve, és hogy előre az eredmény a fürtben, beleértve a hello fürt is (Ha nem érvényes tanúsítványokkal hello fürthöz használható biztonsági végén. Ezért kérjük, ellenőrizze, a folytatás előtt.
> 
> 


### <a name="edit-your-template-file-tooreflect-hello-new-parameters-you-added-above"></a>A fájl tooreflect hello új Sablonparaméterek korábban ismertetett módon hozzáadott szerkesztése
Ha hello hello mintát használ [git-tárház](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow jelszavat, megkezdheti hello minta 5-VM-1-NodeType tulajdonságok értéke-Secure.paramters_Step2.JSON toomake módosítások 

A Resource Manager-sablon paraméter fájl szerkesztése, hello két új secCertificateThumbprint és a paraméterek secCertificateUrlValue hozzáadása. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-hello-template-tooazure"></a>Hello sablon tooAzure telepítése

- Ön most már készen áll a toodeploy a sablon tooAzure vannak. Nyisson meg egy Azure PS 1 vagy újabb parancssort.
- Jelentkezzen be Azure-fiók tooyour, majd válassza a hello adott azure-előfizetés. Ez egy fontos eleme a hozzáférés toomore, mint egy azure-előfizetéssel rendelkező segítsen.

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

Hello sablon előzetes toodeploying tesztelje azt. Használjon hello ugyanahhoz az erőforráscsoporthoz tartozik, amely a fürt jelenleg a rendszer.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Hello sablon tooyour erőforrás-csoport központi telepítését. Használjon hello ugyanahhoz az erőforráscsoporthoz tartozik, amely a fürt jelenleg a rendszer. Hello New-AzureRmResourceGroupDeployment parancsot. Mivel hello alapértelmezett értéke, nem kell toospecify hello mód, **növekményes**.

> [!NOTE]
> Ha mód tooComplete, erőforrásokat, amelyek nem szerepelnek a sablon akaratlanul is törölheti. Ebben a forgatókönyvben azt nem használja.
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Ez egy kitöltött kimenő hello példa azonos powershell.

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

Hello központi telepítés befejezése után tooyour fürt használatával kapcsolódó levelezőprogramokkal hello új tanúsítványt, és hajtson végre néhány lekérdezést. Ha tudja toodo. Ezt követően törölheti a régi tanúsítvány hello. 

Ha önaláírt tanúsítványt használ, ne feledje tooimport a megbízható személyek nevű helyi tanúsítványtároló őket.

```powershell
######## Set up hello certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Gyors referenciaként Íme hello parancs tooconnect tooa biztonságos fürt 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Gyors referenciaként Íme hello parancs tooget fürt állapotát

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-toohello-cluster"></a>Alkalmazás tanúsítványok toohello fürt telepítése.

Hello azonos lépések ajánlásait az 5. lépéseket a keyvault toohello csomópontok alapján telepített toohave hello tanúsítványokat is használhat. ugyanúgy kell definiálására és használjon más paramétereket.


## <a name="adding-or-removing-client-certificates"></a>Hozzáadása vagy eltávolítása az ügyfél-tanúsítványok

Toohello fürt tanúsítványok hozzáadását adhat hozzá a service fabric-fürt ügyfél tanúsítványok tooperform felügyeleti műveleteihez.

Kétféle ügyféltanúsítványok - rendszergazda adhat hozzá vagy csak olvasható. Ezek ezután lehetnek használt toocontrol hozzáférés toohello felügyeleti műveletek és a lekérdezési műveletek hello fürtön. Alapértelmezés szerint hello fürt hozzáadja a tanúsítványokat toohello engedélyezett felügyeleti tanúsítványok listáját.

megadhatja, hogy minden ügyfél-tanúsítványok száma. Minden egyes és törléséről a konfigurációs frissítés toohello service fabric-fürt eredményez.


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>Ügyféltanúsítványok - rendszergazda felvétele vagy csak olvasható-portálon

1. Navigáljon a toohello biztonsági panelen, majd válassza ki a hello "+ hitelesítési" gomb felett hello biztonsági panel.
2. A hello hitelesítés hozzáadása panelen válassza a "Hitelesítés típusa" - "Csak olvasható" vagy "Admin ügyfélszámítógépekhez" hello
3. Most adja hello hitelesítési módját. Ez a tooService háló jelzi, hogy keresi ezt a tanúsítványt hello tulajdonosnévvel vagy ujjlenyomattal hello segítségével. Ez általában nem jó biztonsági gyakorlat toouse hello engedélyezési módszer tulajdonos neve. 

![Ügyfél-tanúsítvány hozzáadása][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-hello-portal"></a>Az ügyféltanúsítványok - rendszergazdai vagy csak olvasható használatával törlését hello portál

a fürt biztonsági, Navigálás toohello biztonsági panel és select hello "Delete" kapcsolót hello helyi menüből hello adott tanúsítvány használatát másodlagos tanúsítvány tooremove.



## <a name="next-steps"></a>Következő lépések
Ezek a cikkek további információt a kiszolgálófürt-felügyelet olvasható:

* [Service Fabric-fürt verziófrissítés folyamatáról és az Ön elvárásainak](service-fabric-cluster-upgrade.md)
* [Az ügyfelek a szerepköralapú hozzáférés-telepítő](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


