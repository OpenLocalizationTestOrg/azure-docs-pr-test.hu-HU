---
title: "az Azure Service Fabric XPlat parancssori felületen lépései aaaGetting"
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
ms.openlocfilehash: e4baa30536b4d8668d8efad301ed8210eb9c0335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-xplat-cli-toointeract-with-a-service-fabric-cluster"></a>A Service Fabric-fürt hello toointeract XPlat parancssori felület használatával

Service Fabric-fürt Linux gépekről Linux hello XPlat parancssori felület használatával kommunikálhat.

hello első lépése hello hello CLI legújabb verziója van beszerezni hello git rep és a készlet a következő parancsok hello azt fel az elérési út használatával:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Az egyes parancsok az támogatja-e, adhatja meg, hogy a parancs hello hello parancs tooobtain hello súgó nevét.
Automatikus-végrehajtási hello parancsok esetén támogatott. Például hello a következő parancs által biztosított összes hello alkalmazás parancs segítséget. 

```sh
 azure servicefabric application 
```

További szűrheti hello súgó tooa adott parancs, mint hello a következő példa azt mutatja meg:

```sh
 azure servicefabric application  create
```

tooenable automatikus-kiegészítéshez a hello CLI, futtassa a következő parancsok hello:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

a következő parancsok hello csatlakozás toohello fürt és az Ön hello hello fürt csomópontja:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

toouse elnevezett paramétereket, és keresse a definíció, beírhatja – Súgó parancs után. Példa:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

A gép tooa többgépes fürtben kapcsolódáskor **nem részét képező hello fürt**, a következő parancs hello használata:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Cserélje le hello PublicIPorFQDN címke hello valós IP-cím vagy FQDN szükség szerint. A gép tooa többgépes fürtben kapcsolódáskor **részét képező hello fürt**, a következő parancs hello használata:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

A PowerShell segítségével, vagy a Linux Service Fabric-fürt a CLI toointeract létrehozott hello Azure-portálon keresztül.

> [!WARNING]
> Ezeken a fürtökön nem biztonságos, így, akkor előfordulhat, hogy kell megnyitása az egy beépített hello fürtjegyzékben hello nyilvános IP-cím hozzáadásával.

## <a name="using-hello-xplat-cli-tooconnect-tooa-service-fabric-cluster"></a>Hello tooconnect tooa Service Fabric-fürt XPlat parancssori felület használatával

a következő Azure parancssori felület parancsait hello ismertetik, hogyan tooconnect tooa biztonságos fürt. hello Tanúsítványadatok hello fürtcsomópontokon tanúsítványt meg kell egyeznie.

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

Ha a tanúsítvány tanúsítványszolgáltatók (CA), tooadd hello – a ca-tanúsítvány-elérési út paraméter a következő példa hello hasonlóan kell: 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

Ha több ügyfélcím, használják hello elválasztó vessző.

A hello tanúsítvány köznapi neve nem egyezik meg a hello csatlakozási végpont, használhatja hello paraméter `--strict-ssl-false` toobypass hello ellenőrzés, ahogy az a következő parancs hello:

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

Ha szeretné tooskip hello hitelesítésszolgáltató ellenőrzési, hozzáadhat hello – ahogy az a következő parancs hello utasítsa el a nem engedélyezett hamis paraméter: 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

A kapcsolódás után meg kell tudni toorun más CLI parancsok toointeract a hello fürt.

## <a name="deploying-your-service-fabric-application"></a>A Service Fabric-alkalmazás telepítése

A következő parancsok toocopy, regisztrálja, és indítsa el a service fabric-alkalmazás hello hello hajtható végre:

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a>Az alkalmazás frissítése

hello folyamatban a hasonló toohello [folyamatok Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Build, másolása, regisztrálja és az alkalmazás létrehozása a projekt gyökérkönyvtárában. Ha az alkalmazás-példány neve `fabric:/MySFApp`, és hello típus MySFApp, hello parancsok a következőképpen nézne ki:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Ellenőrizze a hello tooyour alkalmazás módosítsa, majd újraépítése a módosított hello szolgáltatást.  Frissítés hello módosított szolgáltatás jegyzékfájljában (ServiceManifest.xml) frissítése hello verzióival hello szolgáltatás (és kód vagy Config vagy adatokat szükség szerint). Is frissíteni hello verziószámú hello alkalmazás hello alkalmazás jegyzékfájlja (ApplicationManifest.xml) módosítása és hello módosított szolgáltatás.  Most másolja, és regisztrálja az frissített alkalmazás hello a következő parancsok használatával:

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Most hello az alkalmazásfrissítés indítható hello a következő parancsot:

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

Az alkalmazásfrissítés hello SFX használatával figyelheti. Néhány perc múlva hello alkalmazás lesz frissítve lett. Is hibával frissített alkalmazást, és ellenőrizze a hello automatikus visszaállítási funkciót az a service fabric.

## <a name="converting-from-pfx-toopem-and-vice-versa"></a>PFX-tooPEM, és ez fordítva is igaz alakítása

A helyi számítógépen (a Windows vagy Linux) tooaccess biztonságos fürtök különböző környezetben esetlegesen szükség lehet tooinstall egy tanúsítványt. Például a Windows-gépről, és ez fordítva is igaz biztonságos Linux fürt használata közben esetleg tooconvert a tanúsítvány PFX tooPEM, és ez fordítva is igaz. 

PEM fájl tooa PFX-fájlból, a következő parancs használata hello tooconvert:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

PFX-fájlból egy fájl tooa PEM, a következő parancs használata hello tooconvert:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Tekintse meg a túl[OpenSSL-dokumentáció](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) részleteiről.

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a>Hibaelhárítás


### <a name="copying-of-hello-application-package-does-not-succeed"></a>Hello alkalmazás csomag másolása sikertelen

Ellenőrizze, hogy `openssh` telepítve van. Alapértelmezés szerint Ubuntu asztali nincs telepítve. Telepítse a következő parancs hello:

```sh
sudo apt-get install openssh-server openssh-client**
```

Ha hello a probléma továbbra is fennáll, próbálja letiltása a PAM ssh hello módosításával `sshd_config` fájl a következő parancsok hello használata:

```sh
sudo vi /etc/ssh/sshd_config
#Change hello line with UsePAM toohello following: UsePAM no
sudo service sshd reload
```

Ha hello a probléma továbbra is fennáll, próbálja meg egyre hello több ssh munkamenetek hello a következő parancsok végrehajtásával:

```sh
sudo vi /etc/ssh/sshd\_config
# Add hello following toolines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

Billentyűparancsok használata ssh hitelesítés (a megakadályozását toopasswords) még nem támogatott (mivel hello platform használ ssh toocopy csomagok), ezért használja inkább a jelszó-hitelesítés.

## <a name="next-steps"></a>Következő lépések

[Hello fejlesztési környezet beállítását, és a Service Fabric-alkalmazás tooa Linux-fürt üzembe helyezése](service-fabric-get-started-linux.md)

## <a name="related-articles"></a>Kapcsolódó cikkek

* [A Service Fabric első lépései az Azure CLI 2.0 használatával](service-fabric-azure-cli-2-0.md)
