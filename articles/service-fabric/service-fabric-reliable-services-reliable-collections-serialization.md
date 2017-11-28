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
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a><span data-ttu-id="a0b6a-103">Az Azure Service Fabric megbízható gyűjtemény objektum szerializálása</span><span class="sxs-lookup"><span data-stu-id="a0b6a-103">Reliable Collection object serialization in Azure Service Fabric</span></span>
<span data-ttu-id="a0b6a-104">Megbízható gyűjtemények replikálja, és a gép hibák és áramkimaradások tartós szempontjából elemek toomake megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-104">Reliable Collections' replicate and persist their items toomake sure they are durable across machine failures and power outages.</span></span>
<span data-ttu-id="a0b6a-105">Tooreplicate és toopersist elemeket is, a megbízható gyűjteményeket kell tooserialize őket.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-105">Both tooreplicate and toopersist items, Reliable Collections' need tooserialize them.</span></span>

<span data-ttu-id="a0b6a-106">Megbízható gyűjtemények egy adott típus megfelelő szerializáló hello beszerzése megbízható állapotkezelője.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-106">Reliable Collections' get hello appropriate serializer for a given type from Reliable State Manager.</span></span>
<span data-ttu-id="a0b6a-107">Megbízható állapotkezelője beépített objektumszerializáló tartalmazza, és lehetővé teszi, hogy adott naplófájltípusból regisztrált egyéni objektumszerializáló toobe.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-107">Reliable State Manager contains built-in serializers and allows custom serializers toobe registered for a given type.</span></span>

## <a name="built-in-serializers"></a><span data-ttu-id="a0b6a-108">Beépített objektumszerializáló</span><span class="sxs-lookup"><span data-stu-id="a0b6a-108">Built-in Serializers</span></span>

<span data-ttu-id="a0b6a-109">Megbízható állapotkezelője beépített szerializálót néhány általános típust tartalmazza, úgy, hogy azokat hatékonyabban akkor szerializálható alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-109">Reliable State Manager includes built-in serializer for some common types, so that they can be serialized efficiently by default.</span></span> <span data-ttu-id="a0b6a-110">A más típusú megbízható állapotkezelője visszavált toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="a0b6a-110">For other types, Reliable State Manager falls back toouse hello [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx).</span></span>
<span data-ttu-id="a0b6a-111">Beépített objektumszerializáló még hatékonyabbak, mivel azok típusát nem módosítható, és nincs szükségük a típus neve például hello típus tooinclude adatainak ismerik.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-111">Built-in serializers are more efficient since they know their types cannot change and they do not need tooinclude information about hello type like its type name.</span></span>

<span data-ttu-id="a0b6a-112">Megbízható állapotkezelője rendelkezik beépített szerializáló következő esetében:</span><span class="sxs-lookup"><span data-stu-id="a0b6a-112">Reliable State Manager has built-in serializer for following types:</span></span> 
- <span data-ttu-id="a0b6a-113">GUID</span><span class="sxs-lookup"><span data-stu-id="a0b6a-113">Guid</span></span>
- <span data-ttu-id="a0b6a-114">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0b6a-114">bool</span></span>
- <span data-ttu-id="a0b6a-115">Bájt</span><span class="sxs-lookup"><span data-stu-id="a0b6a-115">byte</span></span>
- <span data-ttu-id="a0b6a-116">sbyte</span><span class="sxs-lookup"><span data-stu-id="a0b6a-116">sbyte</span></span>
- <span data-ttu-id="a0b6a-117">Byte]</span><span class="sxs-lookup"><span data-stu-id="a0b6a-117">byte[]</span></span>
- <span data-ttu-id="a0b6a-118">Karakter</span><span class="sxs-lookup"><span data-stu-id="a0b6a-118">char</span></span>
- <span data-ttu-id="a0b6a-119">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0b6a-119">string</span></span>
- <span data-ttu-id="a0b6a-120">Decimális</span><span class="sxs-lookup"><span data-stu-id="a0b6a-120">decimal</span></span>
- <span data-ttu-id="a0b6a-121">Dupla</span><span class="sxs-lookup"><span data-stu-id="a0b6a-121">double</span></span>
- <span data-ttu-id="a0b6a-122">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="a0b6a-122">float</span></span>
- <span data-ttu-id="a0b6a-123">int</span><span class="sxs-lookup"><span data-stu-id="a0b6a-123">int</span></span>
- <span data-ttu-id="a0b6a-124">uint</span><span class="sxs-lookup"><span data-stu-id="a0b6a-124">uint</span></span>
- <span data-ttu-id="a0b6a-125">hosszú</span><span class="sxs-lookup"><span data-stu-id="a0b6a-125">long</span></span>
- <span data-ttu-id="a0b6a-126">ulong</span><span class="sxs-lookup"><span data-stu-id="a0b6a-126">ulong</span></span>
- <span data-ttu-id="a0b6a-127">rövid</span><span class="sxs-lookup"><span data-stu-id="a0b6a-127">short</span></span>
- <span data-ttu-id="a0b6a-128">ushort</span><span class="sxs-lookup"><span data-stu-id="a0b6a-128">ushort</span></span>

## <a name="custom-serialization"></a><span data-ttu-id="a0b6a-129">Egyedi szerializálás</span><span class="sxs-lookup"><span data-stu-id="a0b6a-129">Custom Serialization</span></span>

<span data-ttu-id="a0b6a-130">Egyéni objektumszerializáló a gyakran használt tooincrease teljesítmény- vagy tooencrypt hello adatok hello hálózaton keresztül, és a lemezen.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-130">Custom serializers are commonly used tooincrease performance or tooencrypt hello data over hello wire and on disk.</span></span> <span data-ttu-id="a0b6a-131">Egyéb okok mellett egyéni objektumszerializáló általában hatékonyabb, mint az általános szerializáló óta hello típus tooserialize adatainak, nem kell.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-131">Among other reasons, custom serializers are commonly more efficient than generic serializer since they don't need tooserialize information about hello type.</span></span> 

<span data-ttu-id="a0b6a-132">[IReliableStateManager.TryAddStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) használt tooregister van az adott típusú T. hello egyéni szerializáló Ez a regisztráció megtörténik az hello StatefulServiceBase tooensure hello kialakításában, helyreállítás megkezdése előtt az összes megbízható gyűjtemények rendelkezik hozzáférési toohello megfelelő szerializáló tooread a megőrzött adatokat.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-132">[IReliableStateManager.TryAddStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer--1?Microsoft_ServiceFabric_Data_IReliableStateManager_TryAddStateSerializer__1_Microsoft_ServiceFabric_Data_IStateSerializer___0__) is used tooregister a custom serializer for hello given type T. This registration should happen in hello construction of hello StatefulServiceBase tooensure that before recovery starts, all Reliable Collections have access toohello relevant serializer tooread their persisted data.</span></span>

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
> <span data-ttu-id="a0b6a-133">Egyéni objektumszerializáló beépített objektumszerializáló keresztül kap elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-133">Custom serializers are given precedence over built-in serializers.</span></span> <span data-ttu-id="a0b6a-134">Például amikor egyéni szerializáló a(z) int regisztrálva van, nem használt tooserialize egész számok helyett hello beépített szerializálót egész szám.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-134">For example, when a custom serializer for int is registered, it is used tooserialize integers instead of hello built-in serializer for int.</span></span>

### <a name="how-tooimplement-a-custom-serializer"></a><span data-ttu-id="a0b6a-135">Hogyan tooimplement egyéni szerializáló</span><span class="sxs-lookup"><span data-stu-id="a0b6a-135">How tooimplement a custom serializer</span></span>

<span data-ttu-id="a0b6a-136">Egyéni szerializáló kell tooimplement hello [IStateSerializer<T> ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) felületet.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-136">A custom serializer needs tooimplement hello [IStateSerializer<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) interface.</span></span>

> [!NOTE]
> <span data-ttu-id="a0b6a-137">IStateSerializer<T> egy túlterhelési írási és olvasási, amely fogad egy további T alapértéke nevű tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-137">IStateSerializer<T> includes an overload for Write and Read that takes in an additional T called base value.</span></span> <span data-ttu-id="a0b6a-138">Ez az API a különbözeti szerializálás van.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-138">This API is for differential serialization.</span></span> <span data-ttu-id="a0b6a-139">Jelenleg különbözeti szerializálási funkció nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-139">Currently differential serialization feature is not exposed.</span></span> <span data-ttu-id="a0b6a-140">Ezért ezek két túlterhelések nem hívják meg csak különbözeti szerializálási téve, és engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-140">Hence, these two overloads are not called until differential serialization is exposed and enabled.</span></span>

<span data-ttu-id="a0b6a-141">Az alábbiakban látható egy példa egyéni típus neve, amelyben négy tulajdonságok OrderKey</span><span class="sxs-lookup"><span data-stu-id="a0b6a-141">Following is an example custom type called OrderKey that contains four properties</span></span>

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

<span data-ttu-id="a0b6a-142">Az alábbiakban látható egy példa végrehajtásának IStateSerializer<OrderKey>.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-142">Following is an example implementation of IStateSerializer<OrderKey>.</span></span>
<span data-ttu-id="a0b6a-143">Vegye figyelembe, hogy olvasási és írási addsortproperty() baseValue, a továbbítást kompatibilitás a megfelelő túlterhelést hívni.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-143">Note that Read and Write overloads that take in baseValue, call their respective overload for forwards compatibility.</span></span>

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

## <a name="upgradability"></a><span data-ttu-id="a0b6a-144">Bővíthetőség</span><span class="sxs-lookup"><span data-stu-id="a0b6a-144">Upgradability</span></span>
<span data-ttu-id="a0b6a-145">Az egy [működés közbeni frissítés alkalmazás](service-fabric-application-upgrade.md), hello frissítés alkalmazott tooa részhalmazát csomópontok, egyszerre több frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-145">In a [rolling application upgrade](service-fabric-application-upgrade.md), hello upgrade is applied tooa subset of nodes, one upgrade domain at a time.</span></span> <span data-ttu-id="a0b6a-146">A folyamat során lesz az egyes frissítési tartományok hello az alkalmazás újabb verziója, és néhány frissítési tartományok hello az alkalmazás régebbi verzióján kell.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-146">During this process, some upgrade domains will be on hello newer version of your application, and some upgrade domains will be on hello older version of your application.</span></span> <span data-ttu-id="a0b6a-147">Hello bevezetés alatt hello új az alkalmazás verziójúnak kell lennie képes tooread hello régi adatait, és hello régi az alkalmazás verziójúnak kell lennie képes tooread hello új adatait.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-147">During hello rollout, hello new version of your application must be able tooread hello old version of your data, and hello old version of your application must be able tooread hello new version of your data.</span></span> <span data-ttu-id="a0b6a-148">Ha hello adatok formátuma nem kompatibilis, hello frissítés előre és hátra sikertelen vagy rosszabb, adatok előfordulhat, hogy elvész vagy sérült.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-148">If hello data format is not forward and backward compatible, hello upgrade may fail, or worse, data may be lost or corrupted.</span></span>

<span data-ttu-id="a0b6a-149">Ha beépített szerializáló használ, nincs tooworry kompatibilitással kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-149">If you are using  built-in serializer, you do not have tooworry about compatibility.</span></span>
<span data-ttu-id="a0b6a-150">Ha egyéni szerializáló vagy hello DataContractSerializer használ, hello adatok toobe végtelenül visszamenőleges rendelkezik, és továbbítja a kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-150">However, if you are using a custom serializer or hello DataContractSerializer, hello data have toobe infinitely backwards and forwards compatible.</span></span>
<span data-ttu-id="a0b6a-151">Más szóval szerializáló verziói toobe képes tooserialize kell, és deszerializálni bármely verziójának hello típusú.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-151">In other words, each version of serializer needs toobe able tooserialize and de-serialize any version of hello type.</span></span>

<span data-ttu-id="a0b6a-152">Adatok szerződés kell eljárni hello jól meghatározott versioning szabályainak hozzáadása, eltávolítása és mezők módosítása.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-152">Data Contract users should follow hello well-defined versioning rules for adding, removing, and changing fields.</span></span> <span data-ttu-id="a0b6a-153">Adategyezmény ismeretlen mezők foglalkoznak, hello szerializálása és deszerializálása folyamatba csatlakoztatás és osztályöröklődést foglalkozó is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-153">Data Contract also has support for dealing with unknown fields, hooking into hello serialization and deserialization process, and dealing with class inheritance.</span></span> <span data-ttu-id="a0b6a-154">További információkért lásd: [adategyezmény használatával](https://msdn.microsoft.com/library/ms733127.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0b6a-154">For more information, see [Using Data Contract](https://msdn.microsoft.com/library/ms733127.aspx).</span></span>

<span data-ttu-id="a0b6a-155">Egyéni szerializáló felhasználók kell igazodnia használják, hogy visszamenőleges, és továbbítja a kompatibilis toomake hello szerializáló toohello útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-155">Custom serializer users should adhere toohello guidelines of hello serializer they are using toomake sure it is backwards and forwards compatible.</span></span>
<span data-ttu-id="a0b6a-156">Gyakori módja az összes verzió támogatására információi hozzáadásával hello elején, és csak a választható tulajdonságok hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-156">Common way of supporting all versions is adding size information at hello beginning and only adding optional properties.</span></span>
<span data-ttu-id="a0b6a-157">Így minden verzió annyi képes és irányítják a hello adatfolyam fennmaradó részét hello olvashatja.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-157">This way each version can read as much it can and jump over hello remaining part of hello stream.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0b6a-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0b6a-158">Next steps</span></span>
  * [<span data-ttu-id="a0b6a-159">Szerializálási és frissítése</span><span class="sxs-lookup"><span data-stu-id="a0b6a-159">Serialization and upgrade</span></span>](service-fabric-application-upgrade-data-serialization.md)
  * [<span data-ttu-id="a0b6a-160">Fejlesztői leírás megbízható gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="a0b6a-160">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * <span data-ttu-id="a0b6a-161">[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-161">[Upgrading your Application Using Visual Studio](service-fabric-application-upgrade-tutorial.md) walks you through an application upgrade using Visual Studio.</span></span>
  * <span data-ttu-id="a0b6a-162">[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a0b6a-162">[Upgrading your Application Using Powershell](service-fabric-application-upgrade-tutorial-powershell.md) walks you through an application upgrade using PowerShell.</span></span>
  * <span data-ttu-id="a0b6a-163">Szabályozhatja, hogy az alkalmazás használatával frissíti [frissítése paraméterek](service-fabric-application-upgrade-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="a0b6a-163">Control how your application upgrades by using [Upgrade Parameters](service-fabric-application-upgrade-parameters.md).</span></span>
  * <span data-ttu-id="a0b6a-164">Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).</span><span class="sxs-lookup"><span data-stu-id="a0b6a-164">Learn how toouse advanced functionality while upgrading your application by referring too[Advanced Topics](service-fabric-application-upgrade-advanced.md).</span></span>
  * <span data-ttu-id="a0b6a-165">Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a0b6a-165">Fix common problems in application upgrades by referring toohello steps in [Troubleshooting Application Upgrades](service-fabric-application-upgrade-troubleshooting.md).</span></span>
