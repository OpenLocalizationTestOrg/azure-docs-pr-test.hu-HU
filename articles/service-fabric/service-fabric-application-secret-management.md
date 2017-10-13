---
title: "A Service Fabric-alkalmazások titkos kulcsok kezelése |} Microsoft Docs"
description: "A cikkből megtudhatja, mennyire biztonságos a Service Fabric-alkalmazás titkos értékeit."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: d71924cda8bb3bffbe221946d80dba150359e38e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a>A Service Fabric-alkalmazások titkos kulcsok kezelése
Ez az útmutató végigvezeti a Service Fabric-alkalmazás a titkos kulcsok kezelése. Titkos kulcsok lehet bármely bizalmas adatokat, például a tárolási kapcsolati karakterláncok, jelszavak és egyéb értékek, amelyek nem egyszerű szöveges kezelje.

Ez az útmutató az Azure Key Vault használatával kulcsokat és titkos kódokat. Azonban *használatával* egy alkalmazás titkos kulcsainak van cloud platform-független bárhol üzemeltetett fürt központilag telepített alkalmazások. 

## <a name="overview"></a>Áttekintés
Az ajánlott módszer a szolgáltatás konfigurációs beállítások kezeléséhez van keresztül [szolgáltatás konfigurációs csomagok][config-package]. Konfigurációs csomagok a következők: rendszerverzióval ellátott és a rendszerállapot-érvényesítési és automatikus visszaállítási felügyelt közbeni keresztül frissíthető. Ez az előnyben részesített a globális konfigurációs, mivel csökkenti a veszélyét annak, hogy a globális szolgáltatáskimaradás. Titkosított titkokat sem kivétel. A Service Fabric titkosítására és visszafejtésére értékek a tanúsítvány titkosítással konfigurációs csomag Settings.xml fájlban beépített lehetőséggel rendelkezik.

A következő ábra szemlélteti a titkos kezelése a Service Fabric-alkalmazás általános folyamata:

![titkos – áttekintés][overview]

Ez a folyamat négy fő lépésből áll:

1. Adatok rejtjelezése tanúsítványának beszerzése.
2. Telepítse a tanúsítványt a fürtön.
3. Titkosítani a titkos értékek, a tanúsítvány az alkalmazás telepítésekor, és azokat behelyezése egy szolgáltatás Settings.xml konfigurációs fájlt.
4. Olvassa el a titkosított értékeket abból Settings.xml visszafejti rejtjelezése ugyanazzal a tanúsítvánnyal. 

[Az Azure Key Vault] [ key-vault-get-started] itt történik, a tanúsítványok biztonságos tárolási helyeként, valamint az Azure Service Fabric-fürtök telepített tanúsítványok lekérése is. Ha nem telepíti az Azure-ba, nem kell Key Vault segítségével kezelheti a Service Fabric-alkalmazások a titkos kulcsok.

## <a name="data-encipherment-certificate"></a>Adatok rejtjelezése tanúsítvány
Adatok rejtjelezése tanúsítvány szigorúan titkosítást használja és konfigurációs visszafejtése egy szolgáltatás Settings.xml értékei, és nem a hitelesítéshez használt vagy aláírási titkosított szöveg. A tanúsítvány a következő követelményeknek kell megfelelniük:

* A tanúsítványnak tartalmaznia kell egy titkos kulccsal.
* A kulcscseréhez használt, a személyes információcsere (.pfx) fájl exportálható léteznie kell a tanúsítványt.
* A tanúsítvány kulcshasználati adattitkosítás (10) kell tartalmaznia, és nem tartalmazhat kiszolgálóhitelesítés vagy ügyfél-hitelesítéshez. 
  
  Például, amikor a powershellel, önaláírt tanúsítvány létrehozása a `KeyUsage` jelzőt kell beállítani `DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>A tanúsítvány telepítése a fürt
Ez a tanúsítvány a fürt minden csomópontján telepítenie kell. Ez használható futásidőben visszafejteni a szolgáltatás Settings.xml tárolt értékek. Lásd: [hogyan hozhat létre egy fürtöt, Azure Resource Manager használatával] [ service-fabric-cluster-creation-via-arm] a telepítési utasításokat. 

## <a name="encrypt-application-secrets"></a>Alkalmazás titkos kulcsok titkosítása
A Service Fabric SDK beépített titkos titkosítási és visszafejtési funkciókkal rendelkezik. Titkos értékek is kell beépített időpontban titkosított visszafejti és programozott módon olvassa el a szolgáltatáskód hibáit. 

A következő PowerShell-parancsot a titkos kulcs titkosítására szolgál. Ez a parancs csak titkosítja a érték; létezik **nem** a titkosított szöveg aláírásához. Titkosított szöveg titkos értékek létrehozásához a fürtben telepített azonos rejtjelezése tanúsítványt kell használnia:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Az eredményül kapott base-64 karakterláncot tartalmaz, mind a titkos titkosított szöveg, valamint a titkosításhoz használt tanúsítvány adatait.  A base-64 kódolású karakterlánc szúrhatók be, a szolgáltatás Settings.xml tartalmazó konfigurációs fájl egyik paraméterének a `IsEncrypted` attribútum értékének beállítása `true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Az alkalmazáspéldányok alkalmazás titkos kulcsok behelyezése
Ideális esetben kell lennie az automatizált lehető különböző környezetekben történő telepítéséhez. A titkos végrehajtása build környezetben, és a titkosított titkos kulcsok megadásával paraméterként, alkalmazáspéldányok létrehozásakor kell elvégezni.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Felülbírálható paraméterekkel Settings.xml használata
A Settings.xml konfigurációs fájl lehetővé teszi, hogy az alkalmazás létrehozáskor megadható felülbírálható paramétereket. Használja a `MustOverride` attribútum helyett egy érték megadása az egyik paraméter:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

A Settings.xml érték felülírására deklarál egy felülbíráló paraméter ApplicationManifest.xml szolgáltatása esetében:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Most a értéket adhat meg egy *alkalmazás paraméter* az alkalmazás példányának létrehozásakor. Az alkalmazáspéldány létrehozása parancsfájlalapú lehet powershellel, vagy az egyszerű integráció az összeállítási folyamat a C#, verziójához.

PowerShell használatával, a paraméter van megadva a a `New-ServiceFabricApplication` parancs, mint egy [kivonattábla](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

C# használatával alkalmazás paraméter meg van adva a egy `ApplicationDescription` , egy `NameValueCollection`:

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a>Szolgáltatás kódból titkos kulcsok visszafejtéséhez
A Service Fabric szolgáltatások alapértelmezés szerint a Windows hálózati szolgáltatás alatt futnak, és nem férnek hozzá a csomópont egy külön beállítások elvégzése nélkül telepített tanúsítványok.

Ha adatok rejtjelezése tanúsítványt használ, meg kell győződnie, hogy a hálózati szolgáltatás, vagy tetszőleges felhasználói fiókot a szolgáltatás fut. a tanúsítvány titkos kulcsához hozzáfér. A Service Fabric fogja kezelni, a szolgáltatás-hozzáférés automatikusan engedélyezése, ha úgy állítja be ehhez. Ez a konfiguráció ApplicationManifest.xml felhasználók és biztonsági házirendeket a tanúsítványok megadásával teheti. A következő példában a hálózati szolgáltatás fiók olvasás való hozzáférése annak ujjlenyomata által definiált tanúsítványt:

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> Amikor másol egy tanúsítvány-ujjlenyomata a tanúsítvány tároló beépülő modul a Windows, egy láthatatlan karakter el van helyezve az ujjlenyomat karakterlánc elején. Ez láthatatlan karakter hibát okozhat, keresse meg a tanúsítvány ujjlenyomata által, ezért ügyeljen arra, hogy törli ezt a felesleges karaktert közben.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Alkalmazás titkos kulcsok használata szolgáltatáskódot
A konfigurációs értékek Settings.xml eléréséhez a konfigurációs csomag API lehetővé teszi, hogy az értékek, amelyek könnyen visszafejtése a `IsEncrypted` attribútum értékének beállítása `true`. Mivel a titkosított szöveg tartalmazza-e a titkosításhoz használt tanúsítvány adatait, nem kell manuálisan található a tanúsítvány. A tanúsítvány csak a szolgáltatás fut a csomóponton telepítenie kell. Egyszerűen hívja a `DecryptValue()` metódusának segítéségével lekérheti az eredeti titkos érték:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Következő lépések
További információ [futó alkalmazások különböző biztonsági engedélyekkel](service-fabric-application-runas-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
