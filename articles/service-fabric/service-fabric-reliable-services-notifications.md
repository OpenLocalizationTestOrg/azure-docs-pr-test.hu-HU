---
title: "aaaReliable szolgáltatások értesítések |} Microsoft Docs"
description: "Service Fabric Reliable Services értesítések fogalmi dokumentációja"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: 8c43190d31dbe82d1dc7fa1c228128bdcc3684f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-notifications"></a><span data-ttu-id="3d320-103">Megbízható szolgáltatások értesítések</span><span class="sxs-lookup"><span data-stu-id="3d320-103">Reliable Services notifications</span></span>
<span data-ttu-id="3d320-104">Értesítések engedélyezése az ügyfelek tootrack hello kíváncsiak vagyunk tooan objektum alatt végzett módosításokat.</span><span class="sxs-lookup"><span data-stu-id="3d320-104">Notifications allow clients tootrack hello changes that are being made tooan object that they're interested in.</span></span> <span data-ttu-id="3d320-105">Kétféle típusú objektumok támogatja értesítéseket: *megbízható állapotkezelője* és *megbízható szótár*.</span><span class="sxs-lookup"><span data-stu-id="3d320-105">Two types of objects support notifications: *Reliable State Manager* and *Reliable Dictionary*.</span></span>

<span data-ttu-id="3d320-106">Értesítések használata gyakori okai a következők:</span><span class="sxs-lookup"><span data-stu-id="3d320-106">Common reasons for using notifications are:</span></span>

* <span data-ttu-id="3d320-107">Épület materializált nézetek, például a másodlagos indexek vagy szűrt nézeteinek hello replika állapotát összesíti.</span><span class="sxs-lookup"><span data-stu-id="3d320-107">Building materialized views, such as secondary indexes or aggregated filtered views of hello replica's state.</span></span> <span data-ttu-id="3d320-108">Példa: egy rendezett index összes kulcsok megbízható szótárban.</span><span class="sxs-lookup"><span data-stu-id="3d320-108">An example is a sorted index of all keys in Reliable Dictionary.</span></span>
* <span data-ttu-id="3d320-109">Küldő figyelési adatok, például az elmúlt egy órában hello hozzáadott felhasználók hello száma.</span><span class="sxs-lookup"><span data-stu-id="3d320-109">Sending monitoring data, such as hello number of users added in hello last hour.</span></span>

<span data-ttu-id="3d320-110">Értesítések elindulása részeként műveleteket alkalmazása esetén.</span><span class="sxs-lookup"><span data-stu-id="3d320-110">Notifications are fired as part of applying operations.</span></span> <span data-ttu-id="3d320-111">Miatt, amely értesítések gyors lehetséges és szinkron események nem tartalmazhatja a költséges műveleteket kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="3d320-111">Because of that, notifications should be handled as fast as possible, and synchronous events shouldn't include any expensive operations.</span></span>

## <a name="reliable-state-manager-notifications"></a><span data-ttu-id="3d320-112">Megbízható állapot Manager értesítéseire</span><span class="sxs-lookup"><span data-stu-id="3d320-112">Reliable State Manager notifications</span></span>
<span data-ttu-id="3d320-113">Megbízható állapotkezelője események hello értesítéseket biztosít:</span><span class="sxs-lookup"><span data-stu-id="3d320-113">Reliable State Manager provides notifications for hello following events:</span></span>

* <span data-ttu-id="3d320-114">Tranzakció</span><span class="sxs-lookup"><span data-stu-id="3d320-114">Transaction</span></span>
  * <span data-ttu-id="3d320-115">Véglegesítés</span><span class="sxs-lookup"><span data-stu-id="3d320-115">Commit</span></span>
* <span data-ttu-id="3d320-116">Állapot manager</span><span class="sxs-lookup"><span data-stu-id="3d320-116">State manager</span></span>
  * <span data-ttu-id="3d320-117">Építse újra</span><span class="sxs-lookup"><span data-stu-id="3d320-117">Rebuild</span></span>
  * <span data-ttu-id="3d320-118">A megbízható állapot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d320-118">Addition of a reliable state</span></span>
  * <span data-ttu-id="3d320-119">A megbízható állapot törlése</span><span class="sxs-lookup"><span data-stu-id="3d320-119">Removal of a reliable state</span></span>

<span data-ttu-id="3d320-120">Megbízható állapotkezelője hello aktuális aktív tranzakciók követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="3d320-120">Reliable State Manager tracks hello current inflight transactions.</span></span> <span data-ttu-id="3d320-121">hello mindössze annyi a változás egy értesítési toobe indította okozó tranzakció állapotban egy tranzakció véglegesítés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="3d320-121">hello only change in transaction state that causes a notification toobe fired is a transaction being committed.</span></span>

<span data-ttu-id="3d320-122">Megbízható állapotkezelője tart fenn például megbízható szótár és megbízható várólista megbízható állapotok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="3d320-122">Reliable State Manager maintains a collection of reliable states like Reliable Dictionary and Reliable Queue.</span></span> <span data-ttu-id="3d320-123">Megbízható állapotkezelője értesítések következik be, amikor megváltozik a gyűjtemény: olyan megbízható állapotban van hozzáadásakor vagy eltávolításakor, vagy újraépítésekor hello teljes gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="3d320-123">Reliable State Manager fires notifications when this collection changes: a reliable state is added or removed, or hello entire collection is rebuilt.</span></span>
<span data-ttu-id="3d320-124">hello megbízható állapotkezelője gyűjtemény újraépítésekor három esetben:</span><span class="sxs-lookup"><span data-stu-id="3d320-124">hello Reliable State Manager collection is rebuilt in three cases:</span></span>

* <span data-ttu-id="3d320-125">Helyreállítás: Replika indításakor azt anélkül állíthatóak vissza korábbi állapotba hello lemezről.</span><span class="sxs-lookup"><span data-stu-id="3d320-125">Recovery: When a replica starts, it recovers its previous state from hello disk.</span></span> <span data-ttu-id="3d320-126">Helyreállítási hello végén, használja **NotifyStateManagerChangedEventArgs** toofire helyreállított megbízható állapotok hello készletét tartalmazó esemény.</span><span class="sxs-lookup"><span data-stu-id="3d320-126">At hello end of recovery, it uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of recovered reliable states.</span></span>
* <span data-ttu-id="3d320-127">Másolás teljes: előtt a replika hello konfigurációkészlet csatlakozhat, a beépített toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3d320-127">Full copy: Before a replica can join hello configuration set, it has toobe built.</span></span> <span data-ttu-id="3d320-128">Egyes esetekben ehhez hello elsődleges replika toobe alkalmazott toohello tétlen másodlagos replika megbízható állapotkezelője állapot teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="3d320-128">Sometimes, this requires a full copy of Reliable State Manager's state from hello primary replica toobe applied toohello idle secondary replica.</span></span> <span data-ttu-id="3d320-129">Hello másodlagos replika használja a megbízható állapotkezelője **NotifyStateManagerChangedEventArgs** toofire azért szerzett hello elsődleges replikából megbízható állapotok hello készletét tartalmazó esemény.</span><span class="sxs-lookup"><span data-stu-id="3d320-129">Reliable State Manager on hello secondary replica uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of reliable states that it acquired from hello primary replica.</span></span>
* <span data-ttu-id="3d320-130">Visszaállítás: A vész-helyreállítási helyzetekben hello replika visszaállítható egy biztonsági másolatból keresztül **RestoreAsync**.</span><span class="sxs-lookup"><span data-stu-id="3d320-130">Restore: In disaster recovery scenarios, hello replica's state can be restored from a backup via **RestoreAsync**.</span></span> <span data-ttu-id="3d320-131">Ilyen esetekben megbízható állapotkezelője hello elsődleges replikán használ **NotifyStateManagerChangedEventArgs** toofire egy eseményt, amely megbízható állapotok azt hello biztonsági másolatból visszaállított hello készletét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3d320-131">In such cases, Reliable State Manager on hello primary replica uses **NotifyStateManagerChangedEventArgs** toofire an event that contains hello set of reliable states that it restored from hello backup.</span></span>

<span data-ttu-id="3d320-132">tranzakció értesítések és/vagy állapot manager értesítéseinek tooregister, van szüksége a hello tooregister **TransactionChanged** vagy **StateManagerChanged** megbízható állapotkezelője eseményeit.</span><span class="sxs-lookup"><span data-stu-id="3d320-132">tooregister for transaction notifications and/or state manager notifications, you need tooregister with hello **TransactionChanged** or **StateManagerChanged** events on Reliable State Manager.</span></span> <span data-ttu-id="3d320-133">Gyakori hely a ezek eseménykezelők tooregister az állapotalapú szolgáltatás hello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="3d320-133">A common place tooregister with these event handlers is hello constructor of your stateful service.</span></span> <span data-ttu-id="3d320-134">Amikor regisztrál a hello konstruktor, teljesíti-e nem hello élettartama során változás által okozott értesítésekhez **IReliableStateManager**.</span><span class="sxs-lookup"><span data-stu-id="3d320-134">When you register on hello constructor, you won't miss any notification that's caused by a change during hello lifetime of **IReliableStateManager**.</span></span>

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

<span data-ttu-id="3d320-135">Hello **TransactionChanged** eseménykezelő használ **NotifyTransactionChangedEventArgs** hello esemény tooprovide adatait.</span><span class="sxs-lookup"><span data-stu-id="3d320-135">hello **TransactionChanged** event handler uses **NotifyTransactionChangedEventArgs** tooprovide details about hello event.</span></span> <span data-ttu-id="3d320-136">Hello művelet tulajdonságot tartalmaz (például **NotifyTransactionChangedAction.Commit**), amely megadja, hogy a módosítás hello típusát.</span><span class="sxs-lookup"><span data-stu-id="3d320-136">It contains hello action property (for example, **NotifyTransactionChangedAction.Commit**) that specifies hello type of change.</span></span> <span data-ttu-id="3d320-137">Hello transaction tulajdonság, amely biztosítja, hogy módosultak a hivatkozás toohello tranzakció is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3d320-137">It also contains hello transaction property that provides a reference toohello transaction that changed.</span></span>

> [!NOTE]
> <span data-ttu-id="3d320-138">Napjainkban **TransactionChanged** események aktiválódnak csak akkor, ha hello tranzakció.</span><span class="sxs-lookup"><span data-stu-id="3d320-138">Today, **TransactionChanged** events are raised only if hello transaction is committed.</span></span> <span data-ttu-id="3d320-139">hello művelet megegyezik túl**NotifyTransactionChangedAction.Commit**.</span><span class="sxs-lookup"><span data-stu-id="3d320-139">hello action is then equal too**NotifyTransactionChangedAction.Commit**.</span></span> <span data-ttu-id="3d320-140">De a jövőbeli hello események előfordulhat, hogy kiváltott tranzakció állapotváltozások más típusú.</span><span class="sxs-lookup"><span data-stu-id="3d320-140">But in hello future, events might be raised for other types of transaction state changes.</span></span> <span data-ttu-id="3d320-141">Javasoljuk, hogy hello művelet ellenőrzése és a hello esemény feldolgozása csak akkor, ha egy, a várt.</span><span class="sxs-lookup"><span data-stu-id="3d320-141">We recommend checking hello action and processing hello event only if it's one that you expect.</span></span>
> 
> 

<span data-ttu-id="3d320-142">Példa **TransactionChanged** eseménykezelő.</span><span class="sxs-lookup"><span data-stu-id="3d320-142">Following is an example **TransactionChanged** event handler.</span></span>

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

<span data-ttu-id="3d320-143">Hello **StateManagerChanged** eseménykezelő használ **NotifyStateManagerChangedEventArgs** hello esemény tooprovide adatait.</span><span class="sxs-lookup"><span data-stu-id="3d320-143">hello **StateManagerChanged** event handler uses **NotifyStateManagerChangedEventArgs** tooprovide details about hello event.</span></span>
<span data-ttu-id="3d320-144">**NotifyStateManagerChangedEventArgs** két alosztályok rendelkezik: **NotifyStateManagerRebuildEventArgs** és **NotifyStateManagerSingleEntityChangedEventArgs**.</span><span class="sxs-lookup"><span data-stu-id="3d320-144">**NotifyStateManagerChangedEventArgs** has two subclasses: **NotifyStateManagerRebuildEventArgs** and **NotifyStateManagerSingleEntityChangedEventArgs**.</span></span>
<span data-ttu-id="3d320-145">Hello művelet tulajdonságával a **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello helytelen alosztályának:</span><span class="sxs-lookup"><span data-stu-id="3d320-145">You use hello action property in **NotifyStateManagerChangedEventArgs** toocast **NotifyStateManagerChangedEventArgs** toohello correct subclass:</span></span>

* <span data-ttu-id="3d320-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span></span>
* <span data-ttu-id="3d320-147">**NotifyStateManagerChangedAction.Add** és **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-147">**NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span></span>

<span data-ttu-id="3d320-148">Példa **StateManagerChanged** értesítéskezelő.</span><span class="sxs-lookup"><span data-stu-id="3d320-148">Following is an example **StateManagerChanged** notification handler.</span></span>

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a><span data-ttu-id="3d320-149">Megbízható szótár értesítések</span><span class="sxs-lookup"><span data-stu-id="3d320-149">Reliable Dictionary notifications</span></span>
<span data-ttu-id="3d320-150">Megbízható szótár események hello értesítéseket biztosít:</span><span class="sxs-lookup"><span data-stu-id="3d320-150">Reliable Dictionary provides notifications for hello following events:</span></span>

* <span data-ttu-id="3d320-151">Újraépítés: Meghívva **ReliableDictionary** helyreállt állapotában a helyreállított vagy másolt helyi állapot vagy a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="3d320-151">Rebuild: Called when **ReliableDictionary** has recovered its state from a recovered or copied local state or backup.</span></span>
* <span data-ttu-id="3d320-152">Törlés: Meghívva állapotának hello **ReliableDictionary** keresztül hello törölve lett **ClearAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="3d320-152">Clear: Called when hello state of **ReliableDictionary** has been cleared through hello **ClearAsync** method.</span></span>
* <span data-ttu-id="3d320-153">Adja hozzá: Hívható meg, ha egy elem túl hozzá lett adva**ReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="3d320-153">Add: Called when an item has been added too**ReliableDictionary**.</span></span>
* <span data-ttu-id="3d320-154">Frissítés: Hívható meg, ha az elem **IReliableDictionary** frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="3d320-154">Update: Called when an item in **IReliableDictionary** has been updated.</span></span>
* <span data-ttu-id="3d320-155">Eltávolítás: Hívható meg, ha az elem **IReliableDictionary** törölve lett.</span><span class="sxs-lookup"><span data-stu-id="3d320-155">Remove: Called when an item in **IReliableDictionary** has been deleted.</span></span>

<span data-ttu-id="3d320-156">tooget megbízható szótár értesítések, van szüksége a hello tooregister **DictionaryChanged** eseménykezelő a **IReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="3d320-156">tooget Reliable Dictionary notifications, you need tooregister with hello **DictionaryChanged** event handler on **IReliableDictionary**.</span></span> <span data-ttu-id="3d320-157">A közös helyen hello van az e eseménykezelők tooregister **ReliableStateManager.StateManagerChanged** értesítési hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3d320-157">A common place tooregister with these event handlers is in hello **ReliableStateManager.StateManagerChanged** add notification.</span></span>
<span data-ttu-id="3d320-158">Ha regisztrálja **IReliableDictionary** túl szerepel-e**IReliableStateManager** biztosítja, hogy nem hagyott ki belőle értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="3d320-158">Registering when **IReliableDictionary** is added too**IReliableStateManager** ensures that you won't miss any notifications.</span></span>

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="3d320-159">**ProcessStateManagerSingleEntityNotification** hello minta módszer van a fenti hello **OnStateManagerChangedHandler** példa hívások.</span><span class="sxs-lookup"><span data-stu-id="3d320-159">**ProcessStateManagerSingleEntityNotification** is hello sample method that hello preceding **OnStateManagerChangedHandler** example calls.</span></span>
> 
> 

<span data-ttu-id="3d320-160">hello előző kód beállítja hello **IReliableNotificationAsyncCallback** kommunikáljanak, jelszavat **DictionaryChanged**.</span><span class="sxs-lookup"><span data-stu-id="3d320-160">hello preceding code sets hello **IReliableNotificationAsyncCallback** interface, along with **DictionaryChanged**.</span></span> <span data-ttu-id="3d320-161">Mivel **NotifyDictionaryRebuildEventArgs** tartalmaz egy **IAsyncEnumerable** illesztő – amely toobe aszinkron módon--számba kell belüli értesítések elindulása esetén keresztül  **RebuildNotificationAsyncCallback** helyett **OnDictionaryChangedHandler**.</span><span class="sxs-lookup"><span data-stu-id="3d320-161">Because **NotifyDictionaryRebuildEventArgs** contains an **IAsyncEnumerable** interface--which needs toobe enumerated asynchronously--rebuild notifications are fired through **RebuildNotificationAsyncCallback** instead of **OnDictionaryChangedHandler**.</span></span>

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> <span data-ttu-id="3d320-162">A kód, megelőző feldolgozási hello rebuild értesítési részeként hello első hello megmarad az összesített állapota nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="3d320-162">In hello preceding code, as part of processing hello rebuild notification, first hello maintained aggregated state is cleared.</span></span> <span data-ttu-id="3d320-163">Hello megbízható gyűjtemény újraépíti az új állapot, mert az összes korábbi értesítés irrelevánsak.</span><span class="sxs-lookup"><span data-stu-id="3d320-163">Because hello reliable collection is being rebuilt with a new state, all previous notifications are irrelevant.</span></span>
> 
> 

<span data-ttu-id="3d320-164">Hello **DictionaryChanged** eseménykezelő használ **NotifyDictionaryChangedEventArgs** hello esemény tooprovide adatait.</span><span class="sxs-lookup"><span data-stu-id="3d320-164">hello **DictionaryChanged** event handler uses **NotifyDictionaryChangedEventArgs** tooprovide details about hello event.</span></span>
<span data-ttu-id="3d320-165">**NotifyDictionaryChangedEventArgs** öt alosztályok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3d320-165">**NotifyDictionaryChangedEventArgs** has five subclasses.</span></span> <span data-ttu-id="3d320-166">A hello művelet tulajdonsággal **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello helytelen alosztályának:</span><span class="sxs-lookup"><span data-stu-id="3d320-166">Use hello action property in **NotifyDictionaryChangedEventArgs** toocast **NotifyDictionaryChangedEventArgs** toohello correct subclass:</span></span>

* <span data-ttu-id="3d320-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span></span>
* <span data-ttu-id="3d320-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span></span>
* <span data-ttu-id="3d320-169">**NotifyDictionaryChangedAction.Add** és **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-169">**NotifyDictionaryChangedAction.Add** and **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span></span>
* <span data-ttu-id="3d320-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span></span>
* <span data-ttu-id="3d320-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="3d320-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span></span>

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a><span data-ttu-id="3d320-172">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="3d320-172">Recommendations</span></span>
* <span data-ttu-id="3d320-173">*Tegye* értesítési események lehető legnagyobb befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="3d320-173">*Do* complete notification events as fast as possible.</span></span>
* <span data-ttu-id="3d320-174">*Ne* hajtható végre a költséges műveleteket (például i/o-műveletek) szinkron események részeként.</span><span class="sxs-lookup"><span data-stu-id="3d320-174">*Do not* execute any expensive operations (for example, I/O operations) as part of synchronous events.</span></span>
* <span data-ttu-id="3d320-175">*Tegye* hello művelet típusának ellenőrzése előtt hello esemény feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="3d320-175">*Do* check hello action type before you process hello event.</span></span> <span data-ttu-id="3d320-176">Új művelettípusok hello jövőbeli lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="3d320-176">New action types might be added in hello future.</span></span>

<span data-ttu-id="3d320-177">Íme néhány dolgot tookeep figyelembe vételével:</span><span class="sxs-lookup"><span data-stu-id="3d320-177">Here are some things tookeep in mind:</span></span>

* <span data-ttu-id="3d320-178">Értesítések elindulása esetén hello művelet végrehajtásának részeként.</span><span class="sxs-lookup"><span data-stu-id="3d320-178">Notifications are fired as part of hello execution of an operation.</span></span> <span data-ttu-id="3d320-179">Például egy visszaállítási értesítést a visszaállítási művelet hello utolsó lépésként lép működésbe.</span><span class="sxs-lookup"><span data-stu-id="3d320-179">For example, a restore notification is fired as hello last step of a restore operation.</span></span> <span data-ttu-id="3d320-180">A visszaállítás nem fejeződik be, amíg hello értesítési eseményt dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="3d320-180">A restore will not finish until hello notification event is processed.</span></span>
* <span data-ttu-id="3d320-181">Értesítések elindulása műveletek alkalmazása hello részeként esetén, mert az ügyfelek csak a helyileg véglegesített műveletek értesítések:.</span><span class="sxs-lookup"><span data-stu-id="3d320-181">Because notifications are fired as part of hello applying operations, clients see only notifications for locally committed operations.</span></span> <span data-ttu-id="3d320-182">És mivel a műveletek csak a helyileg véglegesített toobe garantáltan (más szóval naplózása), előfordulhat, hogy, vagy előfordulhat, hogy nem vonható vissza a jövőbeli hello.</span><span class="sxs-lookup"><span data-stu-id="3d320-182">And because operations are guaranteed only toobe locally committed (in other words, logged), they might or might not be undone in hello future.</span></span>
* <span data-ttu-id="3d320-183">Hello visszaállítási elérési úton egy értesítés minden alkalmazott művelethez lép működésbe.</span><span class="sxs-lookup"><span data-stu-id="3d320-183">On hello redo path, a single notification is fired for each applied operation.</span></span> <span data-ttu-id="3d320-184">Ez azt jelenti, hogy ha tranzakció T1 Create(X) Delete(X) és Create(X) tartalmaz, kaphat egy értesítési X hello létrehozását, egy hello törlésre és hello létrehozásához ebben az esetben egy abban a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="3d320-184">This means that if transaction T1 includes Create(X), Delete(X), and Create(X), you'll get one notification for hello creation of X, one for hello deletion, and one for hello creation again, in that order.</span></span>
* <span data-ttu-id="3d320-185">Az egyes tranzakciókra vonatkozóan, több műveletet tartalmazó műveleteket a rendszer, amelyben beérkezett hello felhasználótól hello elsődleges replikán hello sorrendben alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="3d320-185">For transactions that contain multiple operations, operations are applied in hello order in which they were received on hello primary replica from hello user.</span></span>
* <span data-ttu-id="3d320-186">Feldolgozási hamis folyamat részeként néhány művelet lehet, hogy vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="3d320-186">As part of processing false progress, some operations might be undone.</span></span> <span data-ttu-id="3d320-187">Értesítések a visszavonási műveletek, működés közbeni hello replika hátsó tooa stabil pontjának állapota hello aktiválódnak.</span><span class="sxs-lookup"><span data-stu-id="3d320-187">Notifications are raised for such undo operations, rolling hello state of hello replica back tooa stable point.</span></span> <span data-ttu-id="3d320-188">Egy fontos visszavonási értesítések különbség, hogy eseményeket, amelyek ismétlődő kulcsok összesítése.</span><span class="sxs-lookup"><span data-stu-id="3d320-188">One important difference of undo notifications is that events that have duplicate keys are aggregated.</span></span> <span data-ttu-id="3d320-189">Például ha tranzakció T1 visszavonja, egy egyetlen értesítési tooDelete(X) láthatja.</span><span class="sxs-lookup"><span data-stu-id="3d320-189">For example, if transaction T1 is being undone, you'll see a single notification tooDelete(X).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d320-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d320-190">Next steps</span></span>
* [<span data-ttu-id="3d320-191">Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="3d320-191">Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="3d320-192">Megbízható szolgáltatások – első lépések</span><span class="sxs-lookup"><span data-stu-id="3d320-192">Reliable Services quick start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="3d320-193">Megbízható szolgáltatások biztonsági mentése és visszaállítása (katasztrófa utáni helyreállítás)</span><span class="sxs-lookup"><span data-stu-id="3d320-193">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="3d320-194">Fejlesztői leírás megbízható gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="3d320-194">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

