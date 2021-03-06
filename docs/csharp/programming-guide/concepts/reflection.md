---
title: Reflektion (C#) | Microsoft-Dokumentation
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: f80a2362-953b-4e8e-9759-cd5f334190d4
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: ab40ab2258703670576084eccf7fd7e1b113d08d
ms.lasthandoff: 03/13/2017

---
# <a name="reflection-c"></a>Reflektion (C#)
Reflektion bietet Objekte (vom Typ <xref:System.Type>), die Assemblys, Module und Typen beschreiben. Mithilfe von Reflektion können Sie dynamisch eine Instanz eines Typs erzeugen, den Typ an ein vorhandenes Objekt binden und Typinformationen von vorhandenen Objekten abfragen. Ebenso wird erläutert wie die Methoden vorhandener Objekte aufgerufen und auf ihre Felder und Eigenschaften zugegriffen werden kann. Wenn Sie Attribute in Ihrem Code verwenden, können Sie mit Reflektion auf diese zugreifen. Weitere Informationen finden Sie unter [Attribute](https://msdn.microsoft.com/library/5x6cd29c).  
  
 Hier sehen Sie ein einfaches Beispiel für Reflektion mit der statischen Methode `GetType`, die von allen Typen der Basisklasse `Object` geerbt wird, zum Abrufen von Typinformationen einer Variable:  
  
```csharp  
// Using GetType to obtain type information:  
int i = 42;  
System.Type type = i.GetType();  
System.Console.WriteLine(type);  
```  
  
 Ausgabe:  
  
 `System.Int32`  
  
 Im folgenden Beispiel wird Reflektion verwendet, um den vollständigen Namen der geladenen Assembly abzurufen.  
  
```csharp  
// Using Reflection to get information from an Assembly:  
System.Reflection.Assembly info = typeof(System.Int32).Assembly;  
System.Console.WriteLine(info);  
```  
  
 Ausgabe:  
  
 `mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089`  
  
> [!NOTE]
>  Die C#-Schlüsselwörter `protected` und `internal` haben in IL keine Bedeutung und werden nicht in Reflektions-APIs verwendet. Die entsprechenden Begriffe in IL sind *Family* und *Assembly*. Verwenden Sie die Eigenschaft <xref:System.Reflection.MethodBase.IsAssembly%2A> zum Identifizieren einer `internal`-Methode. Verwenden Sie <xref:System.Reflection.MethodBase.IsFamilyOrAssembly%2A> zum Identifizieren einer `protected internal`-Methode.  
  
## <a name="reflection-overview"></a>Übersicht über Reflektion  
 Reflektion ist in folgenden Situationen nützlich:  
  
-   Wenn Sie in den Metadaten Ihres Programms auf Attribute zugreifen müssen Weitere Informationen finden Sie unter [Abrufen von Informationen aus Attributen](http://msdn.microsoft.com/library/37dfe4e3-7da0-48b6-a3d9-398981524e1c).  
  
-   Zum Untersuchen und Instanziieren von Typen in einer Assembly  
  
-   Zum Erstellen neuer Typen zur Laufzeit Verwenden Sie die Klassen in <xref:System.Reflection.Emit>.  
  
-   Zum Ausführen von spätem Binden mit Zugriff auf Methoden der zur Laufzeit erstellten Typen Weitere Informationen finden Sie im Thema [Dynamisches Laden und Verwenden von Typen](http://msdn.microsoft.com/library/db985bec-5942-40ec-b13a-771ae98623dc).  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
 Weitere Informationen finden Sie unter:   
  
-   [Reflektion](http://msdn.microsoft.com/library/d1a58e7f-fb39-4d50-bf84-e3b8f9bf9775)  
  
-   [Anzeigen von Typinformationen](http://msdn.microsoft.com/library/7e7303a9-4064-4738-b4e7-b75974ed70d2)  
  
-   [Reflektion und generische Typen](http://msdn.microsoft.com/library/f7180fc5-dd41-42d4-8a8e-1b34288e06de)  
  
-   <xref:System.Reflection.Emit>  
  
-   [Abrufen von Informationen aus Attributen](http://msdn.microsoft.com/library/37dfe4e3-7da0-48b6-a3d9-398981524e1c)  
  
## <a name="see-also"></a>Siehe auch  
 [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)   
 [Assemblys in der Common Language Runtime (CLR)](https://msdn.microsoft.com/library/k3677y81)
