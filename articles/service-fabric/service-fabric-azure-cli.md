---
title: "Ismerkedés az Azure Service Fabric XPlat parancssori felület"
description: "Ismerkedés az Azure Service Fabric XPlat parancssori felület"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: c3ec8ff3-3b78-420c-a7ea-0c5e443fb10e
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: ddf881f6c202a82a3f64773639aa29b660057f8d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-xplat-cli-to-interact-with-a-service-fabric-cluster"></a>A XPlat parancssori felület használatával kommunikál a Service Fabric-fürt

Service Fabric-fürt Linux gépekről Linux rendszeren a XPlat parancssori felület használatával kommunikálhat.

Az első lépés get a git-rep a CLI legújabb verzióját, és állítsa be az elérési úthoz a következő parancsokkal:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Az egyes parancsok az támogatja-e, adhatja meg az adott parancshoz tartozó súgó a parancs nevét.
Automatikus-kiegészítéshez a parancsok használata támogatott. Például a következő parancs lehetővé teszi az súgó összes alkalmazás parancs. 

```sh
 azure servicefabric application 
```

További szűrheti a Súgó gombra egy adott parancs az alábbi példában látható módon:

```sh
 azure servicefabric application  create
```

Ahhoz, hogy az automatikus-kiegészítés a CLI-t, a következő parancsokat:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

A következő parancsok csatlakozzon a fürthöz, és jelenjen meg a fürt a csomópontok:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Az elnevezett paramétert használja, és keresse a definíció, beírhatja--súgójának parancs után. Példa:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

A gépről egy többgépes fürtben való csatlakozáskor **, amely nem része a fürtnek**, használja a következő parancsot:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Cserélje le a PublicIPorFQDN címke a valódi IP vagy FQDN szükség szerint. A gépről egy többgépes fürtben való csatlakozáskor **részét képező a fürt**, használja a következő parancsot:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

A PowerShell segítségével, vagy a parancssori felület, amellyel kommunikálni tud a Linux Service Fabric-fürt létrehozása az Azure portálon keresztül.

> [!WARNING]
> Ezeken a fürtökön nem biztonságos, így, akkor előfordulhat, hogy lehet megnyitása az egy beépített adja hozzá a nyilvános IP-cím a fürtjegyzékben.

## <a name="using-the-xplat-cli-to-connect-to-a-service-fabric-cluster"></a>A XPlat parancssori felület használatával kapcsolódni a Service Fabric-fürt

A következő Azure parancssori felület parancsait azt ismertetik, hogyan kapcsolódik egy biztonságos fürthöz. A tanúsítvány részleteinél meg kell egyeznie a fürtcsomópontokon egy tanúsítványt.

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

Ha a tanúsítvány rendelkezik tanúsítványszolgáltatók (CA), adja hozzá a--ca-tanúsítvány-path paramétert, az alábbi példához hasonló szeretné: 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

Ha több hitelesítésszolgáltató, használják az elválasztó vessző.

Ha a tanúsítvány a köznapi név nem egyezik meg a csatlakozási végpont, használhatja a paraméter `--strict-ssl-false` az ellenőrzés elkerülésére, ahogy az az alábbi parancsot:

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

Ha szeretné a CA-ellenőrzés kihagyása, hozzáadhatja a--utasítsa el a nem engedélyezett hamis paraméter, ahogy az az alábbi parancsot: 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

A kapcsolódás után kell tudni futtatni, amellyel kommunikálni tud a fürt más parancssori felület parancsait.

## <a name="deploying-your-service-fabric-application"></a>A Service Fabric-alkalmazás telepítése

A következő parancsok futtatásával másolja, regisztrálja, és indítsa el a service fabric-alkalmazás hajtható végre:

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a>Az alkalmazás frissítése

A folyamat hasonlít a [folyamatok Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Build, másolása, regisztrálja és az alkalmazás létrehozása a projekt gyökérkönyvtárában. Ha az alkalmazás-példány neve `fabric:/MySFApp`, és a típus MySFApp, a parancs a következőképpen nézne ki:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Végezze el a módosítást az alkalmazáson, és építse újra a módosított szolgáltatást.  A módosított service manifest-fájl (ServiceManifest.xml) frissítése a frissített verziókat a szolgáltatás (és kód vagy Config vagy adatokat szükség szerint). Az alkalmazás és a módosított szolgáltatást a frissített verziószámú is módosíthatja a az alkalmazás jegyzékében (ApplicationManifest.xml).  Most másolja, és regisztrálja a frissített alkalmazás a következő parancsokkal:

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Az alkalmazásfrissítés most már elindíthatja a következő paranccsal:

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

Az alkalmazásfrissítés SFX használatával figyelheti. Az alkalmazás néhány percen belül frissült volna. Hiba történt a frissített alkalmazást is, és ellenőrizze az automatikus visszagörgetésre, ha a service fabric.

## <a name="converting-from-pfx-to-pem-and-vice-versa"></a>PEM, és ez fordítva is igaz PFX-ból

Szükség lehet a tanúsítvány telepítése a helyi számítógépen (a Windows vagy Linux), amely a másik lehetséges, hogy nem biztonságos fürtök eléréséhez. Például egy Windows-gépen, és ez fordítva is igaz biztonságos Linux fürt használata közben: szükség lehet a tanúsítvány próbaverzióról PFX PEM, és ez fordítva is igaz. 

A PEM-fájl át az PFX-fájlba, a következő paranccsal:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

PFX-fájl a következő paranccsal konvertálható PEM fájllá:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Részletes információkat az [OpenSSL-dokumentációban](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) talál.

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a>Hibaelhárítás


### <a name="copying-of-the-application-package-does-not-succeed"></a>Az alkalmazás csomag másolása sikertelen

Ellenőrizze, hogy `openssh` telepítve van. Alapértelmezés szerint Ubuntu asztali nincs telepítve. Telepítse a következő parancsot:

```sh
sudo apt-get install openssh-server openssh-client**
```

Ha a probléma továbbra is fennáll, próbálja letiltása a PAM ssh módosításával a `sshd_config` fájlt a következő parancsokkal:

```sh
sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
sudo service sshd reload
```

Ha a probléma továbbra is fennáll, próbálja növelni a száma ssh munkamenetek a következő parancsok végrehajtásával:

```sh
sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

Kulcsok használata az ssh hitelesítés (jelszó) szemben még nem támogatott (mivel a platform használatával ssh csomagok másolása), ezért használja inkább a jelszó-hitelesítést.

## <a name="next-steps"></a>Következő lépések

[Állítsa be a fejlesztési környezetet, és a Service Fabric-alkalmazás egy Linux-fürt telepítése.](service-fabric-get-started-linux.md)

## <a name="related-articles"></a>Kapcsolódó cikkek

* [A Service Fabric első lépései az Azure CLI 2.0 használatával](service-fabric-azure-cli-2-0.md)
