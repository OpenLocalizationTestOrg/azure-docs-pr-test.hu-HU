---
title: "Gyűjtemény objektum szerializálása az Azure Service Fabric aaaReliable |} Microsoft Docs"
description: "Az Azure Service Fabric megbízható gyűjtemények objektum szerializálása"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 9d35374c-2d75-4856-b776-e59284641956
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/8/2017
ms.author: mcoskun
ms.openlocfilehash: 248defbe0ae6f65b4ac5dc7c74e80d8f1152ce94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Az Azure Service Fabric megbízható gyűjtemény objektum szerializálása
Megbízható gyűjtemények replikálja, és a gép hibák és áramkimaradások tartós szempontjából elemek toomake megmaradnak.
Tooreplicate és toopersist elemeket is, a megbízható gyűjteményeket kell tooserialize őket.

Megbízható gyűjtemények egy adott típus megfelelő szerializáló hello beszerzése megbízható állapotkezelője.
Megbízható állapotkezelője beépített objektumszerializáló tartalmazza, és lehetővé teszi, hogy adott naplófájltípusból regisztrált egyéni objektumszerializáló toobe.

## <a name="built-in-serializers"></a>Beépített objektumszerializáló

Megbízható állapotkezelője beépített szerializálót néhány általános típust tartalmazza, úgy, hogy azokat hatékonyabban akkor szerializálható alapértelmezés szerint. A más típusú megbízható állapotkezelője visszavált toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).
Beépített objektumszerializáló még hatékonyabbak, mivel azok típusát nem módosítható, és nincs szükségük a típus neve például hello típus tooinclude adatainak ismerik.

Megbízható állapotkezelője rendelkezik beépített szerializáló következő esetében: 
- GUID
- logikai érték
- Bájt
- sbyte
- Byte]
- Karakter
- Karakterlánc
- Decimális
- Dupla
- Lebegőpontos
- int
- uint
- hosszú
- ulong
- rövid
- ushort

## <a name="custom-serialization"></a>Egyedi szerializálás

Egyéni objektumszerializáló a gyakran használt tooincrease teljesítmény- vagy tooencrypt hello adatok hello hálózaton keresztül, és a lemezen. Egyéb okok mellett egyéni objektumszerializáló általában hatékonyabb, mint az általános szerializáló óta hello típus tooserialize adatainak, nem kell. 

[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) használt tooregister van az adott típusú T. hello egyéni szerializáló Ez a regisztráció megtörténik az hello StatefulServiceBase tooensure hello kialakításában, helyreállítás megkezdése előtt az összes megbízható gyűjtemények rendelkezik hozzáférési toohello megfelelő szerializáló tooread a megőrzött adatokat.

```C#
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed tooset OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> Egyéni objektumszerializáló beépített objektumszerializáló keresztül kap elsőbbséget. Például amikor egyéni szerializáló a(z) int regisztrálva van, nem használt tooserialize egész számok helyett hello beépített szerializálót egész szám.

### <a name="how-tooimplement-a-custom-serializer"></a>Hogyan tooimplement egyéni szerializáló

Egyéni szerializáló kell tooimplement hello [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) felületet.

> [!NOTE]
> IStateSerializer<T> egy túlterhelési írási és olvasási, amely fogad egy további T alapértéke nevű tartalmazza. Ez az API a különbözeti szerializálás van. Jelenleg különbözeti szerializálási funkció nem lesz közzétéve. Ezért ezek két túlterhelések nem hívják meg csak különbözeti szerializálási téve, és engedélyezve.

Az alábbiakban látható egy példa egyéni típus neve, amelyben négy tulajdonságok OrderKey

```C#
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

Az alábbiakban látható egy példa végrehajtásának IStateSerializer<OrderKey>.
Vegye figyelembe, hogy olvasási és írási addsortproperty() baseValue, a továbbítást kompatibilitás a megfelelő túlterhelést hívni.

```C#
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a>Bővíthetőség
Az egy [működés közbeni frissítés alkalmazás](service-fabric-application-upgrade.md), hello frissítés alkalmazott tooa részhalmazát csomópontok, egyszerre több frissítési tartományt. A folyamat során lesz az egyes frissítési tartományok hello az alkalmazás újabb verziója, és néhány frissítési tartományok hello az alkalmazás régebbi verzióján kell. Hello bevezetés alatt hello új az alkalmazás verziójúnak kell lennie képes tooread hello régi adatait, és hello régi az alkalmazás verziójúnak kell lennie képes tooread hello új adatait. Ha hello adatok formátuma nem kompatibilis, hello frissítés előre és hátra sikertelen vagy rosszabb, adatok előfordulhat, hogy elvész vagy sérült.

Ha beépített szerializáló használ, nincs tooworry kompatibilitással kapcsolatos.
Ha egyéni szerializáló vagy hello DataContractSerializer használ, hello adatok toobe végtelenül visszamenőleges rendelkezik, és továbbítja a kompatibilis.
Más szóval szerializáló verziói toobe képes tooserialize kell, és deszerializálni bármely verziójának hello típusú.

Adatok szerződés kell eljárni hello jól meghatározott versioning szabályainak hozzáadása, eltávolítása és mezők módosítása. Adategyezmény ismeretlen mezők foglalkoznak, hello szerializálása és deszerializálása folyamatba csatlakoztatás és osztályöröklődést foglalkozó is rendelkezik. További információkért lásd: [adategyezmény használatával](https://msdn.microsoft.com/library/ms733127.aspx).

Egyéni szerializáló felhasználók kell igazodnia használják, hogy visszamenőleges, és továbbítja a kompatibilis toomake hello szerializáló toohello útmutatást.
Gyakori módja az összes verzió támogatására információi hozzáadásával hello elején, és csak a választható tulajdonságok hozzáadását.
Így minden verzió annyi képes és irányítják a hello adatfolyam fennmaradó részét hello olvashatja.

## <a name="next-steps"></a>Következő lépések
  * [Szerializálási és frissítése](service-fabric-application-upgrade-data-serialization.md)
  * [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.
  * [Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.
  * Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).
  * Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).
  * Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).
