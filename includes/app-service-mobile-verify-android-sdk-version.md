Folyamatos fejlesztését miatt hello Android SDK-verzió az Android Studio telepítve nem egyeznek meg a hello kód hello verzióját. hello ebben az oktatóanyagban hivatkozott Android SDK legújabb hello írásának időpontjában 23, hello verziója. hello verziószám hello SDK jelennek meg új kiadásaiban, előfordulhat, hogy növelje, és elérhető legújabb verzióra hello használatát javasoljuk.

Verzióütközés két tünetei a következők:

- Build, illetve hello projekt újraépítéséhez kaphat például a gradle-lel hibaüzenetek "**toofind cél Google Inc.:Google APIs:n sikertelen**".
- Oldja fel az kódban szabványos Android objektumok alapján `import` előfordulhat, hogy utasítások generálása hibaüzenetek.

Mindkét jelenik meg, ha hello hello Android SDK az Android Studio telepített verziója nem egyeznek meg a letöltött hello projekt hello SDK célját. tooverify hello verzió, ellenőrizze a következő módosításokat hello:

1. Az Android Studióban kattintson **eszközök** > **Android** > **SDK Manager**. Ha nem telepítette a hello hello SDK Platform legújabb verzióját, majd kattintson a tooinstall azt. Jegyezze fel a hello verziószáma.
2. A hello **Project Explorer** lap **Gradle parancsfájlok**, nyissa meg hello fájl **build.gradle (modeule: alkalmazás)**. Győződjön meg arról, hogy hello **compileSdkVersion** és **buildToolsVersion** toohello legújabb SDK-verzió telepítve van beállítva. hello címkék nézhet ki:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. Hello Android Studio Project Explorer, kattintson a jobb gombbal a projektcsomópontra hello, válassza a **tulajdonságok**, és válassza a bal oldali oszlopban hello **Android**. Győződjön meg arról, hogy hello **Project Build Target** toohello értéke azonos SDK verzióra hello **targetSdkVersion**.

Az Android Studióban hello jegyzékfájl már nem használt toospecify hello cél SDK és a minimális SDK-verzió, eltérően hello esetet, amelyben eclipse-ben.
