---
title: "a Service Fabric-alkalmazások aaaManaging titkok |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toosecure titkos értékei a Service Fabric-alkalmazás."
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
ms.openlocfilehash: b8cafcb681d95aaa1b8e9a1afaac78ba5b7f58b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a>A Service Fabric-alkalmazások titkos kulcsok kezelése
Ez az útmutató végigvezeti hello felügyelete titkos kulcsokat a Service Fabric-alkalmazás. Titkos kulcsok lehet bármely bizalmas adatokat, például a tárolási kapcsolati karakterláncok, jelszavak és egyéb értékek, amelyek nem egyszerű szöveges kezelje.

Ez az útmutató az Azure Key Vault toomanage kulcsok és titkos kulcsokat használ. Azonban *használatával* egy alkalmazás titkos kulcsainak felhő platform-független tooallow alkalmazások toobe telepített tooa fürt üzemeltetett tetszőleges helyre. 

## <a name="overview"></a>Áttekintés
hello ajánlott toomanage szolgáltatás konfigurációs beállításait van keresztül [szolgáltatás konfigurációs csomagok][config-package]. Konfigurációs csomagok a következők: rendszerverzióval ellátott és a rendszerállapot-érvényesítési és automatikus visszaállítási felügyelt közbeni keresztül frissíthető. Ez az előnyben részesített tooglobal konfigurációs, mivel csökkenti a globális szolgáltatáskimaradás hello esélyét. Titkosított titkokat sem kivétel. A Service Fabric titkosítására és visszafejtésére értékek a tanúsítvány titkosítással konfigurációs csomag Settings.xml fájlban beépített lehetőséggel rendelkezik.

hello alábbi ábrán látható, a Service Fabric-alkalmazás titkos felügyeleti hello általános folyamata:

![titkos – áttekintés][overview]

Ez a folyamat négy fő lépésből áll:

1. Adatok rejtjelezése tanúsítványának beszerzése.
2. Hello tanúsítvány telepítése a fürtön.
3. Titkosítani a titkos értékek hello tanúsítvánnyal alkalmazások telepítésekor, és azokat behelyezése egy szolgáltatás Settings.xml konfigurációs fájlt.
4. Olvassa el a titkosított értékeket abból Settings.xml visszafejti a hello rejtjelezése ugyanazt a tanúsítványt. 

[Az Azure Key Vault] [ key-vault-get-started] itt történik, a tanúsítványok biztonságos tárolási helyként, valamint egy módon tooget, az Azure Service Fabric-fürtök telepített tanúsítványok. Ha nem telepít tooAzure, nem kell a Service Fabric-alkalmazások toouse Key Vault toomanage titkos kulcsok.

## <a name="data-encipherment-certificate"></a>Adatok rejtjelezése tanúsítvány
Adatok rejtjelezése tanúsítvány szigorúan titkosítást használja és konfigurációs visszafejtése egy szolgáltatás Settings.xml értékei, és nem a hitelesítéshez használt vagy aláírási titkosított szöveg. hello tanúsítvány hello követelményeknek kell megfelelniük:

* hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.
* a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.
* hello tanúsítvány kulcshasználat adattitkosítás (10) kell tartalmaznia, és nem tartalmazhat kiszolgálóhitelesítés vagy ügyfél-hitelesítéshez. 
  
  Például, amikor a PowerShell használatával önaláírt tanúsítvány létrehozása, hello `KeyUsage` jelző túl meg kell`DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-hello-certificate-in-your-cluster"></a>A fürt hello tanúsítvány telepítése
Ez a tanúsítvány hello fürt minden csomópontján telepítenie kell. Egy szolgáltatás Settings.xml tárolt futásidejű toodecrypt értéken lesz. Lásd: [hogyan toocreate Azure Resource Manager használatával fürt] [ service-fabric-cluster-creation-via-arm] a telepítési utasításokat. 

## <a name="encrypt-application-secrets"></a>Alkalmazás titkos kulcsok titkosítása
Service Fabric SDK hello beépített titkos titkosítási és visszafejtési funkciókkal rendelkezik. Titkos értékek is kell beépített időpontban titkosított visszafejti és programozott módon olvassa el a szolgáltatáskód hibáit. 

a következő PowerShell-paranccsal hello használt tooencrypt titkos kulcs. Ez a parancs csak titkosítja hello érték; létezik **nem** hello titkosított szöveg aláírásához. Hello segítségével kell ugyanazt a rejtjelezése tanúsítványt, amely telepítve van-e a fürt tooproduce titkosított szöveg titkos értékeket:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

hello eredményül kapott base-64 kódolású karakterlánc tartalmazza a hello titkos titkosított szöveg, valamint hello tanúsítvány, de a használt tooencrypt adatait azt.  hello base-64 kódolású karakterlánc szúrhatók be, a szolgáltatás Settings.xml konfigurációs fájl hello egyik paraméterének `IsEncrypted` attribútum értéke túl`true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Az alkalmazáspéldányok alkalmazás titkos kulcsok behelyezése
Ideális esetben kell lennie az automatizált lehető telepítési toodifferent környezetekben. A titkos végrehajtása build környezetben, és hello titkosított titkos kulcsok megadásával paraméterként, alkalmazáspéldányok létrehozásakor lehet elvégezni.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Felülbírálható paraméterekkel Settings.xml használata
hello Settings.xml konfigurációs fájl lehetővé teszi, hogy az alkalmazás létrehozáskor megadható felülbírálható paramétereket. Használjon hello `MustOverride` attribútum helyett egy érték megadása az egyik paraméter:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

toooverride értékek Settings.xml, deklarál egy felülbíráló paraméter ApplicationManifest.xml hello szolgáltatása esetében:

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

Most hello értéket adhat meg egy *alkalmazás paraméter* hello alkalmazás példányának létrehozásakor. Az alkalmazáspéldány létrehozása parancsfájlalapú lehet powershellel, vagy az egyszerű integráció az összeállítási folyamat a C#, verziójához.

PowerShell használatával, a hello paraméter megadott toohello `New-ServiceFabricApplication` parancs, mint egy [kivonattábla](https://technet.microsoft.com/library/ee692803.aspx):

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
Szolgáltatások a Service Fabric futtathatók hálózati szolgáltatás alapértelmezés szerint a Windows, és nem rendelkezik hozzáférési toocertificates csomóponton telepített hello néhány külön beállítások elvégzése nélkül.

Adatok rejtjelezése tanúsítványt használ, meg kell toomake meg arról, hogy a hálózati szolgáltatás vagy bármely felhasználói fiók hello szolgáltatás hozzáférés toohello tanúsítvány titkos kulccsal rendelkezik. A Service Fabric hozzáférést a szolgáltatás automatikusan Ha konfigurálja toodo úgy fogja kezelni. Ez a konfiguráció ApplicationManifest.xml felhasználók és biztonsági házirendeket a tanúsítványok megadásával teheti. A következő példa hello a hálózati szolgáltatás fiók hello adott van olvasási hozzáférési tooa tanúsítvány határozzák meg az ujjlenyomatot:

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
> A tanúsítvány-ujjlenyomat másolásakor hello tanúsítványból tárolására beépülő modul a Windows, egy láthatatlan karakter el van helyezve a hello ujjlenyomat karakterlánc hello elején. Ez láthatatlan karakter okozhat hiba toolocate közben a tanúsítvány ujjlenyomatát, ezért lehet, hogy toodelete ezt a felesleges karaktert.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Alkalmazás titkos kulcsok használata szolgáltatáskódot
hello API Settings.xml konfigurációs értékeket eléréséhez a konfigurációs csomag lehetővé teszi, hogy könnyen visszafejtése értékek, amelyek hello `IsEncrypted` attribútum értéke túl`true`. Hello titkosított szöveg tartalmazza-e a titkosításhoz használt hello tanúsítvány adatait, mert nem kell toomanually keresés hello tanúsítványt. hello szolgáltatást futtató hello csomóponton telepített toobe ugyanúgy kell hello tanúsítványt. Egyszerűen hívás hello `DecryptValue()` metódus tooretrieve hello eredeti titkos érték:

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
