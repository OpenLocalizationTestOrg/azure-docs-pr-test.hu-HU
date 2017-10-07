---
title: "SQL-összekötő lépés által aaaGeneric. lépés |} Microsoft Docs"
description: "Ez a cikk van érdekében hello általános SQL-összekötő részletes használatával egy egyszerű HR rendszeren keresztül."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b1b5f89ab588de6f92f173a7bc00f97180067669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Általános SQL-összekötő – részletes útmutató
Ez a témakör részletesen ismerteti az. Egy egyszerű példa HR adatbázist hoz létre, és használja azt az egyes felhasználók és a csoporttagságuk importálásához.

## <a name="prepare-hello-sample-database"></a>Hello mintaadatbázis előkészítése
SQL Server rendszert futtató kiszolgálón, futtassa a hello SQL-parancsfájlt található [függelék](#appendix-a). Ezt a parancsfájlt a mintaadatbázis hello nevű GSQLDEMO hoz létre. hello hálózatiobjektum-modellje hello létrehozott adatbázis tűnik, hogy a kép:  
![Hálózatiobjektum-modellje](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

A felhasználó azt szeretné, hogy toouse tooconnect toohello adatbázis is létrehozhat. Ebben a bemutatóban hello felhasználói FABRIKAM\SQLUser nevezik, és hello tartományban található.

## <a name="create-hello-odbc-connection-file"></a>Hello ODBC kapcsolati fájl létrehozása
Általános SQL-összekötő hello ODBC tooconnect toohello távoli kiszolgálót használ. Először kell toocreate hello ODBC kapcsolati információkat tartalmazó fájl.

1. Indítsa el a kiszolgálón a hello ODBC segédprogramja:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Jelölje be hello lapon **fájl DSN**. Kattintson a **hozzáadása...** .  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. hello out-of-box illesztőprogram works azokat, amelyek válassza ki azt, és kattintson **következő >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Nevezze el hello fájl, például a **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. Kattintson a **Befejezés** gombra.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Idő tooconfigure hello kapcsolat. Hello adatforrás adjon meg helyes leírást, és adja meg az SQL Server rendszert futtató hello server hello nevét.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. Adja meg, hogyan tooauthenticate SQL. Ebben az esetben azt a Windows-hitelesítés használatára.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Adjon meg hello hello mintaadatbázis, **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Minden alapértelmezett ezen a képernyőn megtartása. Kattintson a **Befejezés** gombra.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. minden a várt módon működik, tooverify kattintson **tesztelése adatforrás**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Ellenőrizze, hogy hello teszt sikeres.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. hello ODBC konfigurációs fájl most kell fájl DSN látható.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

Most már tudunk hello fájl igazolnia kell, és hello összekötő létrehozásához.

## <a name="create-hello-generic-sql-connector"></a>Hello általános SQL-összekötő létrehozása
1. Hello Synchronization Service Manager felhasználói felületén, válassza ki **összekötők** és **létrehozása**. Válassza ki **általános SQL (Microsoft)** , és adjon neki könnyen megjegyezhető nevet.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Hello előző szakaszban létrehozott hello DSN fájl található, és töltse fel az toohello kiszolgáló. Adja meg a hello hitelesítő adatok tooconnect toohello adatbázis.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. Ebben a forgatókönyvben azt van így könnyen nekünk, és tegyük fel például, hogy nincsenek-e két objektumtípusok **felhasználói** és **csoport**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. toofind hello attribútumok, azt szeretnénk, ha azok összekötő toodetect hello hello tábla részei megtekintésével. Mivel a **felhasználók** egy fenntartott szó az SQL-ben, a szögletes szögletes zárójelbe [] tooprovide kell.  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Idő toodefine hello horgonyattribútum és hello megkülönböztető név attribútum. A **felhasználók**, két hello attribútumok felhasználónév és a EmployeeID hello kombinációja használjuk. A **csoport**, GroupName használjuk (nem fog reális valósághű, de ez a forgatókönyv működik).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Nem minden attribútumtípust észlelhető az SQL-adatbázisban. különösen nem hello hivatkozási attribútum típusa. Hello csoport objektumtípus el kell toochange hello OwnerID és MemberID tooreference.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. hello attribútumok azt összes hivatkozás attribútumok hello előző lépésben ezen értékek a következők hivatkoznak hello objektumtípus beállítást választani. Ebben az esetben hello felhasználói objektum típusa.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Hello globális paraméterek lapon jelölje be **vízjel** hello különbözeti stratégia szerint. Hello dátum és idő formátumban is beírhat **éééé-hh-nn óó: pp:**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. A hello **konfigurálása partíciók és hierarchiák** lapon, válassza ki mindkét típusú objektumokat.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. A hello **típusú objektumokat válasszon** és **attribútumok kiválasztása**, jelölje be a típusú objektumokat és az összes attribútum. A hello **horgonyok konfigurálása** kattintson **Befejezés**.

## <a name="create-run-profiles"></a>Futtatási profilok létrehozása
1. Hello Synchronization Service Manager felhasználói felületén, válassza ki **összekötők**, és **Configure Run Profiles**. Kattintson a **új profil**. Először **teljes importálás**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Hello típusának kiválasztása **teljes importálás (csak szakaszban)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Válassza ki a hello partíció **objektum = felhasználó**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Válassza ki **tábla** és típus **[felhasználók]**. Görgessen lefelé toohello többértékű objektum típushoz című rész, és adja meg a kép a következő hello hasonlóan hello adatok. Válassza ki **Befejezés** toosave hello lépés.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Válassza ki **új lépés**. Most, jelölje be **objektum = csoport**. Hello utolsó lapon mint a következő képen hello hello konfigurációt használja. Kattintson a **Befejezés** gombra.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. Nem kötelező: Ha kívánja, konfigurálhatja a további futtatási profilokat. Ez a forgatókönyv csak a teljes importálás hello használjuk.
7. Kattintson a **OK** módosítása toofinish futtatási profil szerepel.

## <a name="add-some-test-data-and-test-hello-import"></a>Adja hozzá az egyes tesztelési adatokat, valamint hello importálása
Töltse ki a mintaadatbázis néhány tesztadatot. Amikor elkészült, válassza ki a **futtatása** és **teljes importálás**.

Ez a felhasználó két telefonszámot és egy csoport néhány tagjához.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>A függelék
**SQL parancsfájl toocreate hello mintaadatbázis**

```SQL
---Creating hello Database---------
Create Database GSQLDEMO
Go
-------Using hello Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
