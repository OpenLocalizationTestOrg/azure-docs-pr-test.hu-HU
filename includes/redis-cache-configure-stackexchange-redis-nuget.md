.NET-alkalmazásokban használható hello **StackExchange.Redis** gyorsítótárügyfél, a Visual Studio NuGet-csomagot, amely leegyszerűsíti a hello konfigurációs gyorsítótár ügyfélalkalmazások használatával állítható be. 

> [!NOTE]
> További információkért lásd: hello [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) github lap és hello [StackExchange.Redis gyorsítótár ügyfél dokumentáció](http://github.com/StackExchange/StackExchange.Redis#documentation).
> 
> 

tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello StackExchange.Redis NuGet-csomagot, kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** válassza **NuGet-csomagok kezelése**. 

![NuGet-csomagok kezelése](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Típus **StackExchange.Redis** vagy **StackExchange.Redis.StrongName** szöveg hello keresőmezőbe, válassza ki a kívánt verziójával hello hello eredmények közül, majd kattintson **telepítése**.

> [!NOTE]
> Ha inkább toouse hello erős névvel ellátott verziója **StackExchange.Redis** ügyféloldali kódtár válassza **StackExchange.Redis.StrongName**; ellenkező esetben válassza **StackExchange.Redis**.
> 
> 

![StackExchange.Redis NuGet-csomag](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

hello NuGet csomag tölti le, és hozzáadja a hello szükséges összeállítási referenciát az ügyfél alkalmazás tooaccess Azure Redis Cache hello StackExchange.Redis gyorsítótár-ügyféllel.

> [!NOTE]
> Ha korábban már konfigurálta a projekt toouse StackExchange.Redis, ellenőrizze, hogy frissítéseket toohello csomagot hello **NuGet Package Manager**. a toocheck és telepítse a frissített verziói hello StackExchange.Redis NuGet-csomagot, kattintson **frissítések** a hello hello **NuGet-Csomagkezelő** ablak. Ha egy frissítés toohello StackExchange.Redis NuGet-csomag nem érhető el, frissítheti a projekt toouse hello frissített verziója.
> 
> 

Hello StackExchange.Redis NuGet-csomag kattintva is telepíthet **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **eszközök** menüt, és futó hello parancs követően – hello **Csomagkezelő konzol** ablak.
    
```
Install-Package StackExchange.Redis
```
