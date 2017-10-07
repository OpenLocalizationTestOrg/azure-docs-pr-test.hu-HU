---
title: "a Windows rendszerbiztonság használatával a Windows rendszerű fürtre aaaSecure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure csomópontok és ügyfél-csomópont biztonsági önálló fürtön, a Windows rendszeren futó Windows biztonsági használatával."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 44f3011eb630357f342052a48d6c852b17dccec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Biztonságos Windows önálló fürtben, a Windows biztonsági
tooprevent jogosulatlan hozzáférés tooa Service Fabric-fürt, biztosítania kell hello fürt. Biztonsági különösen fontos, termelési számítási feladatokhoz hello fürt futtatásakor. Ez a cikk ismerteti, hogyan tooconfigure csomópontok és ügyfél-csomópont biztonsági hello Windows biztonsági használatával *művelet* fájlt.  hello folyamat toohello felel meg a biztonsági lépés konfigurálható [létrehozása a Windows rendszert futtató önálló fürtön](service-fabric-cluster-creation-for-windows-server.md). Hogyan használja a Service Fabric a Windows biztonsági kapcsolatos további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).

> [!NOTE]
> Érdemes hello kiválasztását-csomópontok biztonsági gondosan, mivel nem egy biztonsági választott tooanother fürt frissítése. toochange hello biztonsági kiválasztása, hogy toorebuild hello teljes fürt.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Csoportosan felügyelt szolgáltatásfiókot használó Windows biztonságának konfigurálása  
hello minta *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurációs fájl hello együtt letöltött [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag tartalmaz egy sablon konfigurálásához a Windows biztonsági használatával [csoportosan felügyelt szolgáltatásfiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  
  
| **Konfigurációs beállítás** | **Leírás** |  
| --- | --- |  
| WindowsIdentities |Hello fürt és az ügyfél identitások tartalmazza. |  
| ClustergMSAIdentity |Csomópont-csomópont biztonsági konfigurálja. Egy csoport felügyelt szolgáltatásfiók. |  
| ClusterSPN |A csoportosan felügyelt szolgáltatásfiók SPN a teljesen minősített tartománynév|  
| ClientIdentities |Konfigurálja az ügyfél-csomópont biztonsági. Felhasználói fiókok tömb. |  
| Identitás |hello ügyfelek identitását, a tartományi felhasználók. |  
| IsAdmin |IGAZ, akkor hello tartományi felhasználónak rendszergazdai hozzáféréssel rendelkezik az ügyfél, a felhasználó ügyfél-hozzáférési hamis megadása |  
  
[Csomópont toonode biztonsági](service-fabric-cluster-security.md#node-to-node-security) úgy, hogy van-e konfigurálva **ClustergMSAIdentity** Ha szüksége van a service fabric toorun csoportosan felügyelt szolgáltatásfiók alatt. A sorrend toobuild megbízhatósági kapcsolatokat csomópontok között is el kell tisztában legyen egymással. A két különböző módon lehet elvégezni: Adja meg a csoportosan felügyelt szolgáltatásfiók, amely tartalmazza az összes csomópont hello fürt hello vagy hello tartományhoz gép csoport, amely tartalmazza az összes csomópont hello fürtben. Határozottan javasoljuk hello [csoportosan felügyelt szolgáltatásfiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) módszert használja, különösen a nagyobb fürtökkel (több mint 10 csomópontok) vagy fürtök valószínűleg toogrow vagy zsugorítani.  
Ez a módszer nem követeli meg egy tartományi csoport, amelynek a fürt rendszergazdák hozzáférési jogok tooadd rendelkezik, és tagok eltávolítása hello létrehozását. Ezek a fiókok automatikus jelszókezelés is hasznosak. További információkért lásd: [Ismerkedés a csoportosan felügyelt szolgáltatásfiókok](http://technet.microsoft.com/library/jj128431.aspx).  
 
[Ügyfél toonode biztonsági](service-fabric-cluster-security.md#client-to-node-security) segítségével konfigurálható: **ClientIdentities**. A sorrend tooestablish közötti megbízhatósági kapcsolat egy ügyfél és a hello fürt konfigurálnia kell hello fürt tooknow melyik ügyfél identitások megbízhatónak is. Ez kétféleképpen végezhető: Adja meg a hello tartományi csoport felhasználók, amelyek csatlakozni, vagy adjon meg hello csomópont Tartományfelhasználók csatlakoztatható. A Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt: rendszergazdai és felhasználói. Hozzáférés-vezérlés segítségével hello hello a fürt rendszergazdai toolimit hozzáférés toocertain típusú különböző csoportok számára, így biztonságosabb hello fürt a fürt működését.  A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások. A hozzáférés-vezérlést további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric-ügyfelek](service-fabric-cluster-security-roles.md).  
 
hello a következő példa **biztonsági** szakasz csoportosan felügyelt szolgáltatásfiókot használó Windows biztonsági konfigurálja, és megadja a gépek adott hello *ServiceFabric.clusterA.contoso.com* csoportosan felügyelt szolgáltatásfiók hello fürt, és hogy részét képezik. *CONTOSO\usera* felügyeleti ügyfél hozzáfér:  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a>A számítógép csoport használata Windows biztonságának konfigurálása  
hello minta *ClusterConfig.Windows.MultiMachine.JSON* konfigurációs fájl hello együtt letöltött [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag tartalmazza a sablon a Windows biztonsági beállításainak megadása.  Windows biztonsági beállítások konfigurálása a hello **tulajdonságok** szakasz: 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| **Konfigurációs beállítás** | **Leírás** |
| --- | --- |
| ClusterCredentialType |**ClusterCredentialType** értéke túl*Windows* Ha ClusterIdentity határoz meg egy Active Directory gép csoport nevét. |  
| ServerCredentialType |Állítsa be a túl*Windows* tooenable az ügyfelek Windows biztonsági.<br /><br />Ez azt jelzi, hogy hello ügyfelek hello és hello fürtön magát az Active Directory-tartományban futnak. |  
| WindowsIdentities |Hello fürt és az ügyfél identitások tartalmazza. |  
| ClusterIdentity |A számítógép csoport neve, domain\machinegroup, tooconfigure-csomópontok biztonsági használja. |  
| ClientIdentities |Konfigurálja az ügyfél-csomópont biztonsági. Felhasználói fiókok tömb. |  
| Identitás |Adja hozzá a hello tartományi felhasználót, tartomány\felhasználónév, az ügyfél-azonosító hello. |  
| IsAdmin |Set tootrue toospecify, hogy a tartományi felhasználó hello rendszergazda ügyfélelérési és hamis értéket, a felhasználó ügyfél-hozzáférési rendelkezik. |  

[Csomópont toonode biztonsági](service-fabric-cluster-security.md#node-to-node-security) konfigurálható beállítás **ClusterIdentity** Ha azt szeretné, hogy az Active Directory-tartományba tartozó számítógép csoport toouse. További információkért lásd: [létrehoz egy gép csoportot az Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[Ügyfél-csomópont biztonsági](service-fabric-cluster-security.md#client-to-node-security) konfigurálható **ClientIdentities**. egy ügyfél és a hello fürt közötti megbízhatóság tooestablish hello fürt tooknow hello ügyfél identitásokat tartalmaz, amelyek a fürt hello is megbízhat kell konfigurálnia. Megbízhatósági kapcsolat két különböző módon is létrehozása:

- Adja meg a hello tartományi csoport felhasználók csatlakozhatnak.
- Adja meg a hello csomópont Tartományfelhasználók csatlakoztatható.

A Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt: rendszergazdai és felhasználói. Hozzáférés-vezérlés lehetővé teszi, hogy hello fürt rendszergazdája toolimit hozzáférés toocertain típusú fürtműveletekben különböző csoportok számára, így hello fürt biztonságosabb.  A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek. Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.  

hello a következő példa **biztonsági** szakasz konfigurálja a Windows biztonsági, adja meg a gépek adott hello *ServiceFabric/clusterA.contoso.com* hello fürt részét képezik, és határozza meg, hogy  *CONTOSO\usera* felügyeleti ügyfél hozzáfér:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric egy olyan tartományvezérlőre nem telepíthető. Győződjön meg arról, hogy nincs-e hello IP-cím hello tartományvezérlő tartalmazhat, ha a gép csoportjával művelet, vagy csoportos felügyelt szolgáltatásfiók (gMSA).
>
>

## <a name="next-steps"></a>Következő lépések
Windows biztonsági hello beállítása után *művelet* hello fürtlétrehozás a memóriakihasználtság, a fájl [létrehozása a Windows rendszert futtató önálló fürtön](service-fabric-cluster-creation-for-windows-server.md).

További információ a hogyan-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés, lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).

Lásd: [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md) példák a PowerShell vagy a FabricClient keresztül kapcsolódik.
