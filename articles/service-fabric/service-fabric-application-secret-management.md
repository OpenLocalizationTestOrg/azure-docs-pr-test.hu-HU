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
# <a name="managing-secrets-in-service-fabric-applications"></a><span data-ttu-id="6b926-103">A Service Fabric-alkalmazások titkos kulcsok kezelése</span><span class="sxs-lookup"><span data-stu-id="6b926-103">Managing secrets in Service Fabric applications</span></span>
<span data-ttu-id="6b926-104">Ez az útmutató végigvezeti hello felügyelete titkos kulcsokat a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6b926-104">This guide walks you through hello steps of managing secrets in a Service Fabric application.</span></span> <span data-ttu-id="6b926-105">Titkos kulcsok lehet bármely bizalmas adatokat, például a tárolási kapcsolati karakterláncok, jelszavak és egyéb értékek, amelyek nem egyszerű szöveges kezelje.</span><span class="sxs-lookup"><span data-stu-id="6b926-105">Secrets can be any sensitive information, such as storage connection strings, passwords, or other values that should not be handled in plain text.</span></span>

<span data-ttu-id="6b926-106">Ez az útmutató az Azure Key Vault toomanage kulcsok és titkos kulcsokat használ.</span><span class="sxs-lookup"><span data-stu-id="6b926-106">This guide uses Azure Key Vault toomanage keys and secrets.</span></span> <span data-ttu-id="6b926-107">Azonban *használatával* egy alkalmazás titkos kulcsainak felhő platform-független tooallow alkalmazások toobe telepített tooa fürt üzemeltetett tetszőleges helyre.</span><span class="sxs-lookup"><span data-stu-id="6b926-107">However, *using* secrets in an application is cloud platform-agnostic tooallow applications toobe deployed tooa cluster hosted anywhere.</span></span> 

## <a name="overview"></a><span data-ttu-id="6b926-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6b926-108">Overview</span></span>
<span data-ttu-id="6b926-109">hello ajánlott toomanage szolgáltatás konfigurációs beállításait van keresztül [szolgáltatás konfigurációs csomagok][config-package].</span><span class="sxs-lookup"><span data-stu-id="6b926-109">hello recommended way toomanage service configuration settings is through [service configuration packages][config-package].</span></span> <span data-ttu-id="6b926-110">Konfigurációs csomagok a következők: rendszerverzióval ellátott és a rendszerállapot-érvényesítési és automatikus visszaállítási felügyelt közbeni keresztül frissíthető.</span><span class="sxs-lookup"><span data-stu-id="6b926-110">Configuration packages are versioned and updatable through managed rolling upgrades with health-validation and auto rollback.</span></span> <span data-ttu-id="6b926-111">Ez az előnyben részesített tooglobal konfigurációs, mivel csökkenti a globális szolgáltatáskimaradás hello esélyét.</span><span class="sxs-lookup"><span data-stu-id="6b926-111">This is preferred tooglobal configuration as it reduces hello chances of a global service outage.</span></span> <span data-ttu-id="6b926-112">Titkosított titkokat sem kivétel.</span><span class="sxs-lookup"><span data-stu-id="6b926-112">Encrypted secrets are no exception.</span></span> <span data-ttu-id="6b926-113">A Service Fabric titkosítására és visszafejtésére értékek a tanúsítvány titkosítással konfigurációs csomag Settings.xml fájlban beépített lehetőséggel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6b926-113">Service Fabric has built-in features for encrypting and decrypting values in a configuration package Settings.xml file using certificate encryption.</span></span>

<span data-ttu-id="6b926-114">hello alábbi ábrán látható, a Service Fabric-alkalmazás titkos felügyeleti hello általános folyamata:</span><span class="sxs-lookup"><span data-stu-id="6b926-114">hello following diagram illustrates hello basic flow for secret management in a Service Fabric application:</span></span>

![titkos – áttekintés][overview]

<span data-ttu-id="6b926-116">Ez a folyamat négy fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="6b926-116">There are four main steps in this flow:</span></span>

1. <span data-ttu-id="6b926-117">Adatok rejtjelezése tanúsítványának beszerzése.</span><span class="sxs-lookup"><span data-stu-id="6b926-117">Obtain a data encipherment certificate.</span></span>
2. <span data-ttu-id="6b926-118">Hello tanúsítvány telepítése a fürtön.</span><span class="sxs-lookup"><span data-stu-id="6b926-118">Install hello certificate in your cluster.</span></span>
3. <span data-ttu-id="6b926-119">Titkosítani a titkos értékek hello tanúsítvánnyal alkalmazások telepítésekor, és azokat behelyezése egy szolgáltatás Settings.xml konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="6b926-119">Encrypt secret values when deploying an application with hello certificate and inject them into a service's Settings.xml configuration file.</span></span>
4. <span data-ttu-id="6b926-120">Olvassa el a titkosított értékeket abból Settings.xml visszafejti a hello rejtjelezése ugyanazt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="6b926-120">Read encrypted values out of Settings.xml by decrypting with hello same encipherment certificate.</span></span> 

<span data-ttu-id="6b926-121">[Az Azure Key Vault] [ key-vault-get-started] itt történik, a tanúsítványok biztonságos tárolási helyként, valamint egy módon tooget, az Azure Service Fabric-fürtök telepített tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="6b926-121">[Azure Key Vault][key-vault-get-started] is used here as a safe storage location for certificates and as a way tooget certificates installed on Service Fabric clusters in Azure.</span></span> <span data-ttu-id="6b926-122">Ha nem telepít tooAzure, nem kell a Service Fabric-alkalmazások toouse Key Vault toomanage titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="6b926-122">If you are not deploying tooAzure, you do not need toouse Key Vault toomanage secrets in Service Fabric applications.</span></span>

## <a name="data-encipherment-certificate"></a><span data-ttu-id="6b926-123">Adatok rejtjelezése tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="6b926-123">Data encipherment certificate</span></span>
<span data-ttu-id="6b926-124">Adatok rejtjelezése tanúsítvány szigorúan titkosítást használja és konfigurációs visszafejtése egy szolgáltatás Settings.xml értékei, és nem a hitelesítéshez használt vagy aláírási titkosított szöveg.</span><span class="sxs-lookup"><span data-stu-id="6b926-124">A data encipherment certificate is used strictly for encryption and decryption of configuration values in a service's Settings.xml and is not used for authentication or signing of cipher text.</span></span> <span data-ttu-id="6b926-125">hello tanúsítvány hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="6b926-125">hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="6b926-126">hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="6b926-126">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="6b926-127">a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6b926-127">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="6b926-128">hello tanúsítvány kulcshasználat adattitkosítás (10) kell tartalmaznia, és nem tartalmazhat kiszolgálóhitelesítés vagy ügyfél-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="6b926-128">hello certificate key usage must include Data Encipherment (10), and should not include Server Authentication or Client Authentication.</span></span> 
  
  <span data-ttu-id="6b926-129">Például, amikor a PowerShell használatával önaláírt tanúsítvány létrehozása, hello `KeyUsage` jelző túl meg kell`DataEncipherment`:</span><span class="sxs-lookup"><span data-stu-id="6b926-129">For example, when creating a self-signed certificate using PowerShell, hello `KeyUsage` flag must be set too`DataEncipherment`:</span></span>
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-hello-certificate-in-your-cluster"></a><span data-ttu-id="6b926-130">A fürt hello tanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="6b926-130">Install hello certificate in your cluster</span></span>
<span data-ttu-id="6b926-131">Ez a tanúsítvány hello fürt minden csomópontján telepítenie kell.</span><span class="sxs-lookup"><span data-stu-id="6b926-131">This certificate must be installed on each node in hello cluster.</span></span> <span data-ttu-id="6b926-132">Egy szolgáltatás Settings.xml tárolt futásidejű toodecrypt értéken lesz.</span><span class="sxs-lookup"><span data-stu-id="6b926-132">It will be used at runtime toodecrypt values stored in a service's Settings.xml.</span></span> <span data-ttu-id="6b926-133">Lásd: [hogyan toocreate Azure Resource Manager használatával fürt] [ service-fabric-cluster-creation-via-arm] a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6b926-133">See [how toocreate a cluster using Azure Resource Manager][service-fabric-cluster-creation-via-arm] for setup instructions.</span></span> 

## <a name="encrypt-application-secrets"></a><span data-ttu-id="6b926-134">Alkalmazás titkos kulcsok titkosítása</span><span class="sxs-lookup"><span data-stu-id="6b926-134">Encrypt application secrets</span></span>
<span data-ttu-id="6b926-135">Service Fabric SDK hello beépített titkos titkosítási és visszafejtési funkciókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6b926-135">hello Service Fabric SDK has built-in secret encryption and decryption functions.</span></span> <span data-ttu-id="6b926-136">Titkos értékek is kell beépített időpontban titkosított visszafejti és programozott módon olvassa el a szolgáltatáskód hibáit.</span><span class="sxs-lookup"><span data-stu-id="6b926-136">Secret values can be encrypted at built-time and then decrypted and read programmatically in service code.</span></span> 

<span data-ttu-id="6b926-137">a következő PowerShell-paranccsal hello használt tooencrypt titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="6b926-137">hello following PowerShell command is used tooencrypt a secret.</span></span> <span data-ttu-id="6b926-138">Ez a parancs csak titkosítja hello érték; létezik **nem** hello titkosított szöveg aláírásához.</span><span class="sxs-lookup"><span data-stu-id="6b926-138">This command only encrypts hello value; it does **not** sign hello cipher text.</span></span> <span data-ttu-id="6b926-139">Hello segítségével kell ugyanazt a rejtjelezése tanúsítványt, amely telepítve van-e a fürt tooproduce titkosított szöveg titkos értékeket:</span><span class="sxs-lookup"><span data-stu-id="6b926-139">You must use hello same encipherment certificate that is installed in your cluster tooproduce ciphertext for secret values:</span></span>

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="6b926-140">hello eredményül kapott base-64 kódolású karakterlánc tartalmazza a hello titkos titkosított szöveg, valamint hello tanúsítvány, de a használt tooencrypt adatait azt.</span><span class="sxs-lookup"><span data-stu-id="6b926-140">hello resulting base-64 string contains both hello secret ciphertext as well as information about hello certificate that was used tooencrypt it.</span></span>  <span data-ttu-id="6b926-141">hello base-64 kódolású karakterlánc szúrhatók be, a szolgáltatás Settings.xml konfigurációs fájl hello egyik paraméterének `IsEncrypted` attribútum értéke túl`true`:</span><span class="sxs-lookup"><span data-stu-id="6b926-141">hello base-64 encoded string can be inserted into a parameter in your service's Settings.xml configuration file with hello `IsEncrypted` attribute set too`true`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a><span data-ttu-id="6b926-142">Az alkalmazáspéldányok alkalmazás titkos kulcsok behelyezése</span><span class="sxs-lookup"><span data-stu-id="6b926-142">Inject application secrets into application instances</span></span>
<span data-ttu-id="6b926-143">Ideális esetben kell lennie az automatizált lehető telepítési toodifferent környezetekben.</span><span class="sxs-lookup"><span data-stu-id="6b926-143">Ideally, deployment toodifferent environments should be as automated as possible.</span></span> <span data-ttu-id="6b926-144">A titkos végrehajtása build környezetben, és hello titkosított titkos kulcsok megadásával paraméterként, alkalmazáspéldányok létrehozásakor lehet elvégezni.</span><span class="sxs-lookup"><span data-stu-id="6b926-144">This can be accomplished by performing secret encryption in a build environment and providing hello encrypted secrets as parameters when creating application instances.</span></span>

#### <a name="use-overridable-parameters-in-settingsxml"></a><span data-ttu-id="6b926-145">Felülbírálható paraméterekkel Settings.xml használata</span><span class="sxs-lookup"><span data-stu-id="6b926-145">Use overridable parameters in Settings.xml</span></span>
<span data-ttu-id="6b926-146">hello Settings.xml konfigurációs fájl lehetővé teszi, hogy az alkalmazás létrehozáskor megadható felülbírálható paramétereket.</span><span class="sxs-lookup"><span data-stu-id="6b926-146">hello Settings.xml configuration file allows overridable parameters that can be provided at application creation time.</span></span> <span data-ttu-id="6b926-147">Használjon hello `MustOverride` attribútum helyett egy érték megadása az egyik paraméter:</span><span class="sxs-lookup"><span data-stu-id="6b926-147">Use hello `MustOverride` attribute instead of providing a value for a parameter:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

<span data-ttu-id="6b926-148">toooverride értékek Settings.xml, deklarál egy felülbíráló paraméter ApplicationManifest.xml hello szolgáltatása esetében:</span><span class="sxs-lookup"><span data-stu-id="6b926-148">toooverride values in Settings.xml, declare an override parameter for hello service in ApplicationManifest.xml:</span></span>

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

<span data-ttu-id="6b926-149">Most hello értéket adhat meg egy *alkalmazás paraméter* hello alkalmazás példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6b926-149">Now hello value can be specified as an *application parameter* when creating an instance of hello application.</span></span> <span data-ttu-id="6b926-150">Az alkalmazáspéldány létrehozása parancsfájlalapú lehet powershellel, vagy az egyszerű integráció az összeállítási folyamat a C#, verziójához.</span><span class="sxs-lookup"><span data-stu-id="6b926-150">Creating an application instance can be scripted using PowerShell, or written in C#, for easy integration in a build process.</span></span>

<span data-ttu-id="6b926-151">PowerShell használatával, a hello paraméter megadott toohello `New-ServiceFabricApplication` parancs, mint egy [kivonattábla](https://technet.microsoft.com/library/ee692803.aspx):</span><span class="sxs-lookup"><span data-stu-id="6b926-151">Using PowerShell, hello parameter is supplied toohello `New-ServiceFabricApplication` command as a [hash table](https://technet.microsoft.com/library/ee692803.aspx):</span></span>

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

<span data-ttu-id="6b926-152">C# használatával alkalmazás paraméter meg van adva a egy `ApplicationDescription` , egy `NameValueCollection`:</span><span class="sxs-lookup"><span data-stu-id="6b926-152">Using C#, application parameters are specified in an `ApplicationDescription` as a `NameValueCollection`:</span></span>

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

## <a name="decrypt-secrets-from-service-code"></a><span data-ttu-id="6b926-153">Szolgáltatás kódból titkos kulcsok visszafejtéséhez</span><span class="sxs-lookup"><span data-stu-id="6b926-153">Decrypt secrets from service code</span></span>
<span data-ttu-id="6b926-154">Szolgáltatások a Service Fabric futtathatók hálózati szolgáltatás alapértelmezés szerint a Windows, és nem rendelkezik hozzáférési toocertificates csomóponton telepített hello néhány külön beállítások elvégzése nélkül.</span><span class="sxs-lookup"><span data-stu-id="6b926-154">Services in Service Fabric run under NETWORK SERVICE by default on Windows and don't have access toocertificates installed on hello node without some extra setup.</span></span>

<span data-ttu-id="6b926-155">Adatok rejtjelezése tanúsítványt használ, meg kell toomake meg arról, hogy a hálózati szolgáltatás vagy bármely felhasználói fiók hello szolgáltatás hozzáférés toohello tanúsítvány titkos kulccsal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6b926-155">When using a data encipherment certificate, you need toomake sure NETWORK SERVICE or whatever user account hello service is running under has access toohello certificate's private key.</span></span> <span data-ttu-id="6b926-156">A Service Fabric hozzáférést a szolgáltatás automatikusan Ha konfigurálja toodo úgy fogja kezelni.</span><span class="sxs-lookup"><span data-stu-id="6b926-156">Service Fabric will handle granting access for your service automatically if you configure it toodo so.</span></span> <span data-ttu-id="6b926-157">Ez a konfiguráció ApplicationManifest.xml felhasználók és biztonsági házirendeket a tanúsítványok megadásával teheti.</span><span class="sxs-lookup"><span data-stu-id="6b926-157">This configuration can be done in ApplicationManifest.xml by defining users and security policies for certificates.</span></span> <span data-ttu-id="6b926-158">A következő példa hello a hálózati szolgáltatás fiók hello adott van olvasási hozzáférési tooa tanúsítvány határozzák meg az ujjlenyomatot:</span><span class="sxs-lookup"><span data-stu-id="6b926-158">In hello following example, hello NETWORK SERVICE account is given read access tooa certificate defined by its thumbprint:</span></span>

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
> <span data-ttu-id="6b926-159">A tanúsítvány-ujjlenyomat másolásakor hello tanúsítványból tárolására beépülő modul a Windows, egy láthatatlan karakter el van helyezve a hello ujjlenyomat karakterlánc hello elején.</span><span class="sxs-lookup"><span data-stu-id="6b926-159">When copying a certificate thumbprint from hello certificate store snap-in on Windows, an invisible character is placed at hello beginning of hello thumbprint string.</span></span> <span data-ttu-id="6b926-160">Ez láthatatlan karakter okozhat hiba toolocate közben a tanúsítvány ujjlenyomatát, ezért lehet, hogy toodelete ezt a felesleges karaktert.</span><span class="sxs-lookup"><span data-stu-id="6b926-160">This invisible character can cause an error when trying toolocate a certificate by thumbprint, so be sure toodelete this extra character.</span></span>
> 
> 

### <a name="use-application-secrets-in-service-code"></a><span data-ttu-id="6b926-161">Alkalmazás titkos kulcsok használata szolgáltatáskódot</span><span class="sxs-lookup"><span data-stu-id="6b926-161">Use application secrets in service code</span></span>
<span data-ttu-id="6b926-162">hello API Settings.xml konfigurációs értékeket eléréséhez a konfigurációs csomag lehetővé teszi, hogy könnyen visszafejtése értékek, amelyek hello `IsEncrypted` attribútum értéke túl`true`.</span><span class="sxs-lookup"><span data-stu-id="6b926-162">hello API for accessing configuration values from Settings.xml in a configuration package allows for easy decrypting of values that have hello `IsEncrypted` attribute set too`true`.</span></span> <span data-ttu-id="6b926-163">Hello titkosított szöveg tartalmazza-e a titkosításhoz használt hello tanúsítvány adatait, mert nem kell toomanually keresés hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="6b926-163">Since hello encrypted text contains information about hello certificate used for encryption, you do not need toomanually find hello certificate.</span></span> <span data-ttu-id="6b926-164">hello szolgáltatást futtató hello csomóponton telepített toobe ugyanúgy kell hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="6b926-164">hello certificate just needs toobe installed on hello node that hello service is running on.</span></span> <span data-ttu-id="6b926-165">Egyszerűen hívás hello `DecryptValue()` metódus tooretrieve hello eredeti titkos érték:</span><span class="sxs-lookup"><span data-stu-id="6b926-165">Simply call hello `DecryptValue()` method tooretrieve hello original secret value:</span></span>

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a><span data-ttu-id="6b926-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b926-166">Next Steps</span></span>
<span data-ttu-id="6b926-167">További információ [futó alkalmazások különböző biztonsági engedélyekkel](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="6b926-167">Learn more about [running applications with different security permissions](service-fabric-application-runas-security.md)</span></span>

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
