---
title: "aaaSecure egy Azure Service Fabric fürt Tanúsítványos használata Windows rendszeren |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toosecure kommunikációs belül hello önálló vagy titkos, valamint az ügyfelek és a hello fürt közötti fürt."
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
ms.openlocfilehash: f0d411963615349a84edfc8125dec4ee5908f146
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Önálló fürtben, X.509-tanúsítványokat használ a Windows biztonságos
Ez a cikk ismerteti, hogyan toosecure hello közötti kommunikáció hello a különálló Windows-fürt különböző csomópontok módjáról, valamint tooauthenticate csatlakozó-ügyfeleket toothis fürt X.509-tanúsítványokat használ. Ez biztosítja, amelyek csak a jogosult felhasználók férhetnek hello fürt hello központilag telepített alkalmazások, és felügyeleti feladatok elvégzésére.  Tanúsítvány biztonsági engedélyezni kell a fürt hello hello fürt létrehozásakor.  

A fürt biztonsági például-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Tanúsítványok kell?
toostart, [hello önálló fürt csomag](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) hello fürtben található csomópontok a tooone. Hello a letöltött csomag, megtalálja a **ClusterConfig.X509.MultiMachine.json** fájlt. Nyissa meg hello fájlt, és tekintse át az hello területén **biztonsági** alatt hello **tulajdonságok** szakasz:

```JSON
"security": {
    "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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

Ez a szakasz ismerteti, amelyekre szüksége van a különálló Windows-fürt védelmét biztosító hello tanúsítványokat. Megadása esetén a fürt tanúsítvány, állítsa be hello **ClusterCredentialType** too_**X509**_. Külső kapcsolatok kiszolgálói tanúsítványának megadása esetén, állítsa be a hello **ServerCredentialType** túl_**X509**_. Bár nem kötelező, az ajánlott toohave mindkét ezek a tanúsítványok megfelelően védett fürt. Ha túl állítsa be ezeket az értékeket*X509* , majd a megfelelő tanúsítványok hello, vagy a Service Fabric rendszer kivételt jelez is meg kell adnia. Bizonyos esetekben előfordulhat, hogy csak szeretné toospecify hello _ClientCertificateThumbprints_ vagy _ReverseProxyCertificate_. Ezek a forgatókönyvek kell nem állíthatja _ClusterCredentialType_ vagy _ServerCredentialType_ too_X509_.


> [!NOTE]
> A [ujjlenyomat](https://en.wikipedia.org/wiki/Public_key_fingerprint) hello elsődleges identitás-tanúsítvány. Olvasási [hogyan tanúsítvány ujjlenyomata tooretrieve](https://msdn.microsoft.com/library/ms734695.aspx) toofind ki az Ön által létrehozott hello tanúsítványok hello ujjlenyomata.
> 
> 

hello következő tábla hello-tanúsítványokat sorolja fel a fürt beállítása lesz szükség:

| **CertificateInformation beállítás** | **Leírás** |
| --- | --- |
| ClusterCertificate |Tesztelési környezetben ajánlott. Ez a tanúsítvány szükséges toosecure hello kommunikációs fürt hello csomópontok között. Frissítés két különböző tanúsítványok, egy elsődleges és másodlagos használható. Hello elsődleges tanúsítvány ujjlenyomata hello beállított hello **ujjlenyomat** szakasz és az hello a másodlagos hello **ThumbprintSecondary** változók. |
| ClusterCertificateCommonNames |Az éles környezetben ajánlott. Ez a tanúsítvány szükséges toosecure hello kommunikációs fürt hello csomópontok között. Egy vagy két fürt közös kiszolgálótanúsítvány-nevek is használhatja. |
| ServerCertificate |Tesztelési környezetben ajánlott. Ez a tanúsítvány toohello ügyfél áll rendelkezésre, amikor tooconnect toothis fürt. Kényelmi célokat szolgál, dönthet úgy, toouse ugyanaz a tanúsítvány hello *ClusterCertificate* és *ServerCertificate*. Frissítés két különböző kiszolgálói tanúsítványok, egy elsődleges és másodlagos használható. Hello elsődleges tanúsítvány ujjlenyomata hello beállított hello **ujjlenyomat** szakasz és az hello a másodlagos hello **ThumbprintSecondary** változók. |
| ServerCertificateCommonNames |Az éles környezetben ajánlott. Ez a tanúsítvány toohello ügyfél áll rendelkezésre, amikor tooconnect toothis fürt. Kényelmi célokat szolgál, dönthet úgy, toouse ugyanaz a tanúsítvány hello *ClusterCertificateCommonNames* és *ServerCertificateCommonNames*. Használhat egy vagy két kiszolgálótanúsítványok köznapi neve. |
| ClientCertificateThumbprints |Ez olyan tanúsítványok, amelyet az tooinstall hitelesített hello ügyfeleken. Számos különböző ügyfél-tanúsítványok telepítve, amelyet az tooallow hozzáférés toohello fürt hello gépen lehet. Minden tanúsítvány ujjlenyomata hello beállított hello **CertificateThumbprint** változó. Ha hello **IsAdmin** túl*igaz*, majd hello ügyfél telepítve van-e a tanúsítvánnyal is tegye a rendszergazda hello fürt felügyeleti tevékenységek. Ha hello **IsAdmin** van *hamis*, ezzel a tanúsítvánnyal hello ügyfél csak az engedélyezett felhasználói hozzáférési jogosultságokat, általában csak olvasható hello műveleteket tudják végrehajtani. További információk a szerepkörök [szerepköralapú hozzáférés-vezérlést (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac) |
| ClientCertificateCommonNames |Set hello köznapi neve hello első ügyféltanúsítvány hello **CertificateCommonName**. Hello **CertificateIssuerThumbprint** hello ujjlenyomata a tanúsítvány kiállítója hello van. Olvasási [tanúsítványok használata](https://msdn.microsoft.com/library/ms731899.aspx) tooknow további információk a gyakori nevei és hello kibocsátó. |
| ReverseProxyCertificate |Tesztelési környezetben ajánlott. Ez az egy nem kötelező tanúsítvány, amely meg, ha azt szeretné, toosecure a [fordított Proxy](service-fabric-reverseproxy.md). Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ. |
| ReverseProxyCertificateCommonNames |Az éles környezetben ajánlott. Ez az egy nem kötelező tanúsítvány, amely meg, ha azt szeretné, toosecure a [fordított Proxy](service-fabric-reverseproxy.md). Ellenőrizze, hogy reverseProxyEndpointPort a NodeType tulajdonságok értéke van beállítva, ha ezt a tanúsítványt használ. |

Itt található példa fürtkonfiguráció ahol hello fürt, a kiszolgáló és az ügyfél tanúsítványok vannak-e megadva. Ne feledje, hogy a fürt / server / reverseProxy tanúsítványok, ujjlenyomat és köznapi név nem engedélyezett toobe konfigurálva együtt hello azonos tanúsítványtípus.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace hello localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
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
Ha felett tanúsítványt összegző szerint kibocsátó váltása, ne hello régi kibocsátó cert hello tanúsítványtároló hello új kibocsátó tanúsítvány telepítése után legalább 2 órát.

## <a name="acquire-hello-x509-certificates"></a>Szerezzen be hello X.509-tanúsítványokat
hello fürtön belüli kommunikáció toosecure, akkor először tooobtain X.509-tanúsítványokat a fürtcsomópontok esetén. Emellett toolimit kapcsolat toothis fürt tooauthorized gépek/felhasználók esetén szeretne tooobtain kell majd hello ügyfélszámítógépeknél tanúsítványok telepítése.

Termelési számítási feladatokhoz futtató fürtök esetén használjon egy [tanúsítvány hitelesítésszolgáltatói (CA)](https://en.wikipedia.org/wiki/Certificate_authority) X.509 tanúsítvány toosecure hello fürt aláírva. Ezek a tanúsítványok beszerzéséről részletekért lépjen túl[hogyan: tanúsítvány beszerzése](http://msdn.microsoft.com/library/aa702761.aspx).

Tesztelési célokra használó fürtök esetén dönthet úgy toouse egy önaláírt tanúsítványt.

## <a name="optional-create-a-self-signed-certificate"></a>Választható lehetőség: Hozzon létre egy önaláírt tanúsítványt
Egyirányú toocreate egy önaláírt tanúsítványt megfelelően védett toouse hello *CertSetup.ps1* hello könyvtárban hello Service Fabric SDK mappában parancsfájl *C:\Program Files\Microsoft SDKs\Service Fabric\ ClusterSetup\Secure*. Szerkessze a fájlt toochange hello alapértelmezett hello tanúsítvány neve (hello értéket keresi *CN = ServiceFabricDevClusterCert*). Futtassa ezt a parancsfájlt, `.\CertSetup.ps1 -Install`.

Mostantól exportálhatja hello tooa PFX tanúsítványfájl jelszóval védett. Először kapnak hello hello tanúsítvány ujjlenyomata. A hello *Start* menü, futtassa a hello *számítógép-tanúsítványok kezelése*. Keresse meg a toohello **helyi számítógép személyes** létrehozott mappa és a keresés hello csak tanúsítvány meg. Kattintson duplán a hello tanúsítvány tooopen azt, jelölje be hello *részletek* lapot, és görgessen lefelé toohello *ujjlenyomat* mező. Hello ujjlenyomat értékét másolja az alábbi PowerShell-paranccsal hello hello szóközök eltávolítása után.  Változás hello `String` tooa megfelelő biztonságos jelszó tooprotect értékét, és futtassa a következő PowerShell hello:

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

hello telepített tanúsítványok toosee hello részleteit a gép akkor is futtatható a következő PowerShell-paranccsal hello:

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Azt is megteheti, ha Azure-előfizetéssel, hajtsa végre a hello szakasz [tanúsítványok tooKey tároló hozzáadása](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-hello-certificates"></a>Hello tanúsítványok telepítése
Miután (oka) t, telepítheti azokat hello fürtcsomópontokon. A csomópontok kell toohave legújabb Windows PowerShell hello 3.x rajtuk. Szüksége lesz toorepeat ezeket a lépéseket minden csomóponton, a fürt és a kiszolgáló és az összes másodlagos tanúsítványokat.

1. Másolás hello .pfx (oka) t toohello csomópont.
2. Nyissa meg rendszergazdaként a PowerShell ablakot, és írja be a következő parancsok hello. Cserélje le a hello *$pswd* hello jelszó, amellyel toocreate ezt a tanúsítványt. Cserélje le a hello *$PfxFilePath* hello .pfx másolt toothis csomópont hello a teljes elérési úttal.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Most beállítása a hello ezt a tanúsítványt, hogy hello Service Fabric folyamat, amely hello hálózati szolgáltatás fiók alatt fut, használhatja a következő parancsfájl hello futtatásával. Adja meg a hello tanúsítványt és a "Hálózati szolgáltatás" hello szolgáltatásfiók hello ujjlenyomatot. A hozzáférés-vezérlési listák hello hello tanúsítvány helyesek hello tanúsítványt megnyitásával ellenőrizheti *Start* > *számítógép-tanúsítványok kezelése* és megnézi *feladatok*  >  *Titkos kulcsok kezelése*.
   
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
   
    # Specify hello user, hello permissions and hello permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of hello machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get hello current acl of hello private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add hello new ace toohello acl of hello private key
    $acl.SetAccessRule($accessRule)
   
    # Write back hello new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe hello access rights currently assigned toothis certificate.
    get-acl $keyFullPath| fl
    ```
4. Ismételje meg a hello felett minden egyes kiszolgáló-tanúsítvány. Használhatja ezen lépések tooinstall hello ügyféltanúsítványok hello gépeken, amelyet az tooallow hozzáférés toohello fürt.

## <a name="create-hello-secure-cluster"></a>Hello biztonságos fürt létrehozása
Hello konfigurálása után **biztonsági** hello szakasza **ClusterConfig.X509.MultiMachine.json** fájl, a Folytatás túl[a fürt létrehozása](service-fabric-cluster-creation-for-windows-server.md#createcluster) szakasz tooconfigure hello csomópontot, majd hozzon létre hello önálló fürtöt. Ne feledje toouse hello **ClusterConfig.X509.MultiMachine.json** fájl hello fürt létrehozása során. A parancs például hello következő látható:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Ha biztonságos hello önálló Windows sikeresen fut a fürt és telepítő hitelesített ügyfelek tooconnect tooit hello hajtsa végre az hello szakasz [Connect tooa biztonságos fürt PowerShell-lel](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit. Példa:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Ezt követően futtathatja más PowerShell-parancsok toowork a fürthöz. Például [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow a biztonságos fürtön található csomópontok listáját.


tooremove hello fürt, csatlakozás hello fürt, amelybe letöltötte a Service Fabric-csomag hello toohello csomópont, nyisson meg egy parancssort, és keresse meg a toohello csomagmappáihoz. Most futtassa a következő parancs hello:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Tanúsítvány nem megfelelő konfigurációs megakadályozhatja, hogy a hello fürt üzembe helyezése során várható. tooself-biztonsági problémák elemzéséhez, tekintse meg a megjelenítő eseménycsoportban *alkalmazási és Szolgáltatásnaplójában* > *Microsoft-Service Fabric*.
> 
> 

