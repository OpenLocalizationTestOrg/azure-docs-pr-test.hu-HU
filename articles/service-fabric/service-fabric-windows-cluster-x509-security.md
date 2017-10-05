---
title: "Az Azure Service Fabric-fürt a tanúsítványok használata a Windows biztonságos |} Microsoft Docs"
description: "Ez a cikk ismerteti az önálló vagy titkos, valamint az ügyfelek előbb és a fürtön belüli kommunikáció biztonságossá tételére."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dekapur
ms.openlocfilehash: 71ece1e43cc3c4ac3350cd59633065de06672420
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos
A cikk ismerteti a különálló Windows fürt, a különböző csomópontok közötti kommunikáció védelméhez, valamint hogy használatával hitelesíti az ügyfeleket ehhez a fürthöz csatlakozik használ X.509 tanúsítvány. Ez biztosítja, hogy az csak a hitelesített felhasználóknak a fürthöz, a központilag telepített alkalmazások és felügyeleti feladatok elvégzésére.  Tanúsítvány biztonsági engedélyezni kell a fürt a fürt létrehozásakor.  

A fürt biztonsági például-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Tanúsítványok kell?
Kezdő-és [a különálló fürt csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) a fürtben található csomópontok egyikére. A letöltött csomagban található egy **ClusterConfig.X509.MultiMachine.json** fájlt. Nyissa meg a fájlt, és tekintse át a szakasz **biztonsági** alatt a **tulajdonságok** szakasz:

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

Ez a szakasz ismerteti a tanúsítványokat, amelyekre szüksége van a különálló Windows-fürt biztonságossá tételéhez. Megadása esetén a fürt tanúsítvány, állítsa be a **ClusterCredentialType** való  _**X509**_. Külső kapcsolatok kiszolgálói tanúsítványának megadása esetén, állítsa be a **ServerCredentialType** való  _**X509**_. Bár nem kötelező, ajánlott mindkét ezek a tanúsítványok megfelelően védett fürt rendelkezik. Ha ezeket az értékeket állít *X509* akkor is meg kell adnia a megfelelő tanúsítványok és a Service Fabric kivételt jelez. Bizonyos esetekben előfordulhat, hogy csak szeretne megadni a _ClientCertificateThumbprints_ vagy _ReverseProxyCertificate_. Ezek a forgatókönyvek kell nem állíthatja _ClusterCredentialType_ vagy _ServerCredentialType_ való _X509_.


> [!NOTE]
> A [ujjlenyomat](https://en.wikipedia.org/wiki/Public_key_fingerprint) tanúsítvány elsődleges azonosítója. Olvasási [hogyan lehet lekérni a tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695.aspx) tudja meg a létrehozott tanúsítványok ujjlenyomatát.
> 
> 

Az alábbi táblázat a tanúsítványok, a fürt beállítása lesz szükség:

| **CertificateInformation beállítás** | **Leírás** |
| --- | --- |
| ClusterCertificate |Tesztelési környezetben ajánlott. Ez a tanúsítvány szükséges a fürt a csomópontok közötti kommunikáció biztonságossá tételére. Frissítés két különböző tanúsítványok, egy elsődleges és másodlagos használható. Állítsa be az elsődleges tanúsítvány ujjlenyomatát a **ujjlenyomat** szakaszt, és hogy a másodlagos a **ThumbprintSecondary** változók. |
| ClusterCertificateCommonNames |Az éles környezetben ajánlott. Ez a tanúsítvány szükséges a fürt a csomópontok közötti kommunikáció biztonságossá tételére. Egy vagy két fürt közös kiszolgálótanúsítvány-nevek is használhatja. |
| ServerCertificate |Tesztelési környezetben ajánlott. Ezt a tanúsítványt az ügyfél áll rendelkezésre, ha csatlakozik a fürthöz. Kényelmi célokat szolgál, ha szeretné, használja ugyanazt a tanúsítványt a *ClusterCertificate* és *ServerCertificate*. Frissítés két különböző kiszolgálói tanúsítványok, egy elsődleges és másodlagos használható. Állítsa be az elsődleges tanúsítvány ujjlenyomatát a **ujjlenyomat** szakaszt, és hogy a másodlagos a **ThumbprintSecondary** változók. |
| ServerCertificateCommonNames |Az éles környezetben ajánlott. Ezt a tanúsítványt az ügyfél áll rendelkezésre, ha csatlakozik a fürthöz. Kényelmi célokat szolgál, ha szeretné, használja ugyanazt a tanúsítványt a *ClusterCertificateCommonNames* és *ServerCertificateCommonNames*. Használhat egy vagy két kiszolgálótanúsítványok köznapi neve. |
| ClientCertificateThumbprints |Ez olyan tanúsítványokat, amelyek a hitelesített ügyfelek telepíteni szeretné. Akkor segítségével számos különböző ügyfél-tanúsítványok engedélyezi a hozzáférést a fürthöz használni kívánt számítógépeken telepítve van. Állítsa be a minden tanúsítvány ujjlenyomatát a **CertificateThumbprint** változó. Ha a **IsAdmin** való *igaz*, majd az ügyfél és a tanúsítvány telepítve van-e is tegye a rendszergazda a fürt felügyeleti tevékenységek. Ha a **IsAdmin** van *hamis*, az ügyfél és a tanúsítvány csak a felhasználói hozzáférési jogosultságokat, általában csak olvasható engedélyezett műveleteket hajthatja végre. További információk a szerepkörök [szerepköralapú hozzáférés-vezérlést (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac) |
| ClientCertificateCommonNames |Az első ügyféltanúsítványt köznapi nevének beállítása a **CertificateCommonName**. A **CertificateIssuerThumbprint** az ujjlenyomat van, ezt a tanúsítványt kibocsátó. Olvasási [tanúsítványok használata](https://msdn.microsoft.com/library/ms731899.aspx) további információkat a gyakori nevei és a kibocsátó. |
| ReverseProxyCertificate |Tesztelési környezetben ajánlott. Ez az egy nem kötelező tanúsítvány, amely meg, hogy szeretné-e biztonságos a [fordított Proxy](service-fabric-reverseproxy.md). Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ. |
| ReverseProxyCertificateCommonNames |Az éles környezetben ajánlott. Ez az egy nem kötelező tanúsítvány, amely meg, hogy szeretné-e biztonságos a [fordított Proxy](service-fabric-reverseproxy.md). Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ. |

Íme példa fürtkonfiguráció, ahol a fürt, a kiszolgáló és az ügyfél tanúsítványok vannak-e megadva. Ne feledje, hogy a fürt / server / reverseProxy tanúsítványok, ujjlenyomat és köznapi név nem engedélyezett cert ugyanolyan együtt konfigurálni.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-roll-over"></a>Tanúsítvány váltása
Tanúsítvány egyszerű neve helyett ujjlenyomat használatakor felett tanúsítványt összegző konfigurációs Fürtfrissítés nem igényel.
Ha felett tanúsítványt összegző szerint kibocsátó váltása, tartsa a régi kibocsátó cert a CERT tárolja az új kibocsátó tanúsítvány telepítése után legalább 2 órát.

## <a name="acquire-the-x509-certificates"></a>Szerezzen be X.509-tanúsítványokat
A fürtön belüli kommunikáció védelméhez, először a fürtcsomópontok esetén, X.509-tanúsítványokat szerezzenek be. Továbbá a jogosult felhasználók vagy gépek fürthöz kapcsolat korlátozására, szüksége lesz beszerzése és az ügyfél gépek tanúsítványok telepítése.

Termelési számítási feladatokhoz futtató fürtök esetén használjon egy [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority) védelméhez a fürt X.509 tanúsítvány aláírására használatos. Ezek a tanúsítványok beszerzéséről további információkért látogasson el [hogyan: tanúsítvány beszerzése](http://msdn.microsoft.com/library/aa702761.aspx).

Tesztelési célokra használó fürtök esetén dönthet úgy, önaláírt tanúsítvány használatára.

## <a name="optional-create-a-self-signed-certificate"></a>Választható lehetőség: Hozzon létre egy önaláírt tanúsítványt
Egy módja megfelelően védett önaláírt tanúsítványt létrehozni a *CertSetup.ps1* parancsfájl a könyvtárban, a Service Fabric SDK mappában *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\ Biztonságos*. Szerkessze a fájlt a tanúsítványhoz tartozó alapértelmezett nevének módosításához (kívánt érték *CN = ServiceFabricDevClusterCert*). Futtassa ezt a parancsfájlt, `.\CertSetup.ps1 -Install`.

Exportálja a tanúsítványt egy PFX-fájl jelszóval védett. Először kapnak a tanúsítvány ujjlenyomatát. Az a *Start* menü, futtassa a *számítógép-tanúsítványok kezelése*. Keresse meg a **helyi számítógép személyes** mappa és a Keresés az imént tanúsítvány létrehozása. Kattintson duplán a tanúsítványra megnyitásához, jelölje be a *részletek* lapra, és görgessen le a *ujjlenyomat* mező. A szóközök eltávolítása után a PowerShell-parancsot, másolja az ujjlenyomat értékét.  Módosítsa a `String` a védelmét, és futtassa a következő PowerShell megfelelő biztonságos jelszó értéket:

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

A Részletek területen találja a számítógépen telepített tanúsítvány futtathatja a következő PowerShell-parancsot:

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Azt is megteheti, ha Azure-előfizetéssel rendelkezik, kövesse a szakasz [tanúsítványok hozzáadása a Key Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>A tanúsítványok telepítése
Miután (oka) t, telepítheti azokat a fürtcsomópontokon. A csomópontok kell rendelkeznie a legújabb Windows PowerShell 3.x rajtuk. Ismételje meg ezeket a lépéseket minden csomóponton, a fürt és a kiszolgáló és az összes másodlagos tanúsítványokat kell.

1. A .pfx fájl átmásolása a csomópont.
2. Nyissa meg rendszergazdaként a PowerShell ablakot, és írja be a következő parancsokat. Cserélje le a *$pswd* a, melyek a tanúsítvány létrehozásához használt jelszó. Cserélje le a *$PfxFilePath* a teljes elérési útját a .pfx másolja ezt a csomópontot.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Most beállítása az ezt a tanúsítványt, hogy a Service Fabric folyamat, amely a hálózati szolgáltatás fiók alatt fut, használhatja a következő parancsfájl futtatásával. Adja meg az ujjlenyomatot, a tanúsítványt és a "Hálózati szolgáltatás" a szolgáltatás fiók. Ellenőrizheti, hogy a tanúsítvány az ACL-ek nyissa meg a tanúsítvány helyesen *Start* > *számítógép-tanúsítványok kezelése* és megnézi *feladataival*  >  *Titkos kulcsok kezelése*.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current acl of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ace to the acl of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate.
    get-acl $keyFullPath| fl
    ```
4. Ismételje meg a fenti lépéseket minden kiszolgálói tanúsítvány. A lépések segítségével telepítse az ügyféltanúsítványokat az engedélyezi a hozzáférést a fürthöz használni kívánt gépekre.

## <a name="create-the-secure-cluster"></a>A biztonságos fürt létrehozása
Beállítása után a **biztonsági** szakasza a **ClusterConfig.X509.MultiMachine.json** fájl, folytathatja a [a fürt létrehozása](service-fabric-cluster-creation-for-windows-server.md#createcluster) szakasz a csomópontok és a különálló fürt létrehozásához. Fontos, hogy a **ClusterConfig.X509.MultiMachine.json** fájl a fürt létrehozása során. Például a parancs nézhet ki például a következőket:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Miután a fürt sikeresen fut, és a hitelesített ügyfelek csatlakoznak, hogy a telepítő a biztonságos önálló Windows, kövesse a szakasz [csatlakozás PowerShell használatával biztonságos fürthöz](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) a csatlakozáshoz. Példa:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Ezt követően futtathatja más PowerShell-parancsok használata ehhez a fürthöz. Például [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) való megjelenítéséhez a biztonságos fürt a csomópontok listáját.


Távolítsa el a fürtöt, a csomópont a fürt, amelybe letöltötte a Service Fabric-csomag csatlakozzon, nyisson meg egy parancssort, és keresse meg a csomag mappát. Most futtassa a következő parancsot:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Tanúsítvány nem megfelelő konfigurációs megakadályozhatja, hogy a fürt üzembe helyezése során várható. Önálló biztonsági problémák elemzéséhez, tekintse meg a megjelenítő eseménycsoportban *alkalmazási és Szolgáltatásnaplójában* > *Microsoft-Service Fabric*.
> 
> 

