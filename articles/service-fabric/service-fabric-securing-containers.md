---
title: "a Service Fabric tároló biztonsági aaaAzure |} Microsoft Docs"
description: "Ismerje meg, most toosecure tárolószolgáltatásainak."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 88faf4e8f949c2f7743756b6272ca672d9710630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-security"></a>Tároló biztonsági

A Service Fabric lehetővé teszi a szolgáltatások egy tároló tooaccess belül olyan tanúsítvány, amely a Windows vagy Linux rendszerű fürt (5.7-es vagy újabb verzió) hello csomópontjára telepítve van. Emellett a Service Fabric is támogatja csoportosan felügyelt szolgáltatásfiók (csoportosan felügyelt szolgáltatásfiókok) Windows tárolók. 

## <a name="certificate-management-for-containers"></a>Az tárolókat Tanúsítványkezelő

A tároló szolgáltatások tanúsítvány megadásával biztosíthatja. hello tanúsítvány hello fürt hello csomópontjára telepítenie kell. hello tanúsítvány információ alapján hello hello alkalmazásjegyzékben `ContainerHostPolicies` címke, a következő kódrészletet mutat be hello:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Hello alkalmazás indításakor hello futásidejű hello tanúsítványok olvas, és a PFX-fájlt, és minden tanúsítvány jelszava. A PFX-fájlt és a jelszó használata a következő környezeti változók hello hello tároló belül érhetők el: 

* **[CodePackageName] Certificate_ _ [CertName] _PFX**
* **[CodePackageName] Certificate_ _ [CertName] _Password**

hello tároló szolgáltatás vagy folyamat felelős a hello PFX-fájl importálása hello tárolóba. tooimport hello tanúsítvány, használhat `setupentrypoint.sh` a parancsfájlok vagy egyéni kód hello tároló folyamaton belül végrehajtani. A következő példakód C# hello PFX-fájl importálásához:

```c#
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
A PFX-tanúsítvány hitelesítése hello alkalmazás vagy szolgáltatás-vagy más szolgáltatásokkal biztonságos commmunication használható.


## <a name="set-up-gmsa-for-windows-containers"></a>Windows-tárolók csoportosan felügyelt szolgáltatásfiók beállítása

csoportosan felügyelt szolgáltatásfiók (felügyelt szolgáltatásfiókok. csoport), a hitelesítő adatok megadását fájl mentése tooset (`credspec`) hello fürt összes csomópontján el van helyezve. hello fájlt is másolhatja az összes olyan csomóponton, a Virtuálisgép-bővítmény használatával.  Hello `credspec` hello csoportosan felügyelt szolgáltatásfiók fiókadatok kell tartalmaznia. További információ a hello `credspec` fájlba, lásd: [szolgáltatásfiókok](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). hello hitelesítő adatok megadását és hello `Hostname` címke hello alkalmazásjegyzék vannak megadva. Hello `Hostname` címke hello csoportosan felügyelt fiók nevét, amely alatt fut, tároló hello egyeznie kell.  Hello `Hostname` címke lehetővé teszi, hogy maga hello tároló tooauthenticate tooother szolgáltatások hello tartomány Kerberos-hitelesítéssel.  Egy minta hello megadásának `Hostname` és hello `credspec` hello az alkalmazásjegyzék megjelenik-e a következő kódrészletet hello:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Következő lépések

* [Egy Windows-tároló tooService Windows Server 2016 háló telepítése](service-fabric-get-started-containers.md)
* [Egy Docker-tároló tooService Linux háló telepítése](service-fabric-get-started-containers-linux.md)
