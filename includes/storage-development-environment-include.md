## <a name="set-up-your-development-environment"></a>A fejlesztési környezet beállítása
Így most készen áll a tootry hello kódpéldák a jelen útmutató következő lépésként állítsa be a fejlesztési környezetet a Visual Studio.

### <a name="create-a-windows-console-application-project"></a>Windows-konzolalkalmazás projekt létrehozása
Hozzon létre egy új Windows-konzolalkalmazást a Visual Studióban. hello lépések bemutatják, hogyan toocreate egy konzolalkalmazást a Visual Studio 2017, azonban hello lépések hasonlóak a Visual Studio más verzióiban.

1. Válassza a **File** (Fájl) > **New** (Új) > **Project** (Projekt) lehetőséget.
2. Válassza az **Installed** (Telepítve) > **Templates** (Sablonok) > **Visual C#** > **Windows Classic Desktop** (Windows klasszikus asztal) lehetőséget
3. Válassza a **Console App (.NET Framework)** (Konzolalkalmazás (.NET keretrendszer) lehetőséget
4. Adjon meg egy nevet az alkalmazásnak hello **Name:** mező
5. Kattintson az **OK** gombra.

![A Visual Studio projektlétrehozási párbeszédpanelje](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Minden ebben az oktatóanyagban szereplő példák hozzáadhatók toohello `Main()` a Konzolalkalmazás metódusában `Program.cs` fájlt.

Hello Azure Storage ügyféloldali kódtárat bármilyen típusú .NET-alkalmazás, beleértve az Azure felhőalapú szolgáltatás, vagy a webes alkalmazás, és az asztali és mobil alkalmazások használható. Ebben az útmutatóban az egyszerűség kedvéért egy konzolalkalmazást használunk.

### <a name="use-nuget-tooinstall-hello-required-packages"></a>NuGet tooinstall szükséges hello csomagok használata
Nincsenek a projekt toocomplete ebben az oktatóanyagban szükséges tooreference két csomagok:

* [A Microsoft Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Ez a csomag programozott hozzáférést biztosít a tárfiókban lévő toodata erőforrásokat.
* [A Microsoft Azure Configuration Manager könyvtár a .NET-hez](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): Ez a csomag egy osztályt biztosít a konfigurációs fájlban található kapcsolati karakterlánc elemzéséhez, függetlenül attól, hogy az alkalmazás hol fut.

Használhat NuGet tooobtain mindkét csomagot. Kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal a projektjére a **Megoldáskezelőben**, és válassza a **Manage NuGet Packages** (NuGet-csomagok kezelése) lehetőséget.
2. Keresse rá az interneten a "windowsazure.Storage"kifejezésre, és kattintson a **telepítése** tooinstall hello a Storage ügyféloldali kódtára és annak függőségeit.
3. Keresse rá az interneten a "WindowsAzure.ConfigurationManager", és kattintson a **telepítése** tooinstall hello Azure Configuration Manager.

> [!NOTE]
> hello Storage ügyféloldali kódtár csomagja is megtalálhatók hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/). Azonban javasoljuk, hogy is, és telepítse a Storage ügyféloldali kódtár hello NuGet tooensure, hogy mindig legyen hello hello ügyféloldali kódtár legújabb verzióját.
> 
> hello ODataLib függőségek a Storage ügyféloldali kódtára a .NET hello érhető el a NuGet, nem a WCF Data Services hello ODataLib-csomagok által megoldott. hello ODataLib-kódtárak letölthetők közvetlenül, vagy a Nugeten keresztül kód projekt által hivatkozott. hello hello Storage ügyféloldali kódtár által használt konkrét ODataLib csomagok [OData](http://nuget.org/packages/Microsoft.Data.OData/), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/), és [Spatial](http://nuget.org/packages/System.Spatial/). Ezek a kódtárak hello Azure Table storage osztályai használják, amíg azok szükséges függőségek a Storage ügyféloldali kódtára hello való programozáshoz.
> 
> 

### <a name="determine-your-target-environment"></a>A célkörnyezet meghatározása
Ez az útmutató hello példák futó két környezet lehetőség közül választhat:

* A kód hello felhőben Azure Storage-fiók is futtathatók. 
* A kód hello Azure storage emulatorban is futtathatja. hello storage emulator emulálja hello felhőben Azure Storage-fiók egy helyi környezet. hello emulátor ingyenes lehetőséget biztosít a teszteléshez és a hibakereséshez a kódot, amíg az alkalmazása fejlesztés alatt áll. hello emulátor egy jól ismert fiókot és kulcsot használ. További információkért lásd: [használata hello Azure Storage Emulator fejlesztés és tesztelés](../articles/storage/common/storage-use-emulator.md)

Hello felhőben egy tárfiókot céloz meg, ha a tárfiók elsődleges elérési kulcsát hello másolása hello Azure-portálon. További információért lásd: [View and copy storage access keys](../articles/storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys) (A tárelérési kulcsok megtekintése és másolása).

> [!NOTE]
> Azure Storage társított költségekre nélül hello storage emulator tooavoid célba. Azonban ha tootarget Azure-tárfiók hello felhőben, az oktatóanyag végrehajtásával költségek elhanyagolhatóak.
> 
> 

### <a name="configure-your-storage-connection-string"></a>A tárolási kapcsolati karakterlánc konfigurálása
hello Azure Storage ügyféloldali kódtára a .NET használatával egy tárolási kapcsolati karakterlánc tooconfigure végpontok támogatja, és tárolási szolgáltatások eléréséhez használt hitelesítő adatok. hello legjobb módja toomaintain a tárolási kapcsolati karakterlánc egy konfigurációs fájlban van. 

Kapcsolati karakterláncokkel kapcsolatos további információkért lásd: [konfigurálása egy tárolási kapcsolati karakterlánc tooAzure](../articles/storage/common/storage-configure-connection-string.md).

> [!NOTE]
> A tárfiók kulcsára hasonló toohello gyökér szintű jelszavát a tárfiók. Legyen óvatos tooprotect a tárfiók kulcsára. Ne adja ki fixen, vagy hogy elmenti azt egy egyszerű szöveges fájl elérhető tooothers tooother felhasználók. Hello Azure-portál használatával, ha úgy véli, előfordulhat, hogy sérültek a kulcs újragenerálása.
> 
> 

tooconfigure a kapcsolati karakterláncot, nyissa meg hello `app.config` fájlt a Visual Studio megoldáskezelőjében. Adja hozzá a hello hello tartalmát `<appSettings>` elem alább látható. Cserélje le `account-name` hello nevet a tárfiók, és `account-key` rendelkező a hívóbetűre:

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

A konfiguráció beállítása például hasonló lesz a következőhöz:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==" />
```

tootarget hello storage emulatort, egy hivatkozást, amely leképezhető toohello jól ismert fióknevet és kulcsot is használhatja. Ebben az esetben a kapcsolati sztring beállítása a következő:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

