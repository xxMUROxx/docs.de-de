---
title: "Vorgehensweise: Initialisieren eines Wörterbuchs mit einem Auflistungsinitialisierer (C#-Programmierhandbuch) | Microsoft-Dokumentation"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- collection initializers [C#], with Dictionary
ms.assetid: 25283922-f8ee-40dc-a639-fac30804ec71
caps.latest.revision: 10
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 60443456be4b4fbb13dad6cb9c372a1af332c2fd
ms.lasthandoff: 03/13/2017

---
# <a name="how-to-initialize-a-dictionary-with-a-collection-initializer-c-programming-guide"></a>Gewusst wie: Initialisieren eines Wörterbuchs mit einem Auflistungsinitialisierer (C#-Programmierhandbuch)
Eine <xref:System.Collections.Generic.Dictionary`2> contains a collection of key/value pairs. Its <xref:System.Collections.Generic.Dictionary`2.Add*> Methode akzeptiert zwei Parameter: einen für den Schlüssel und einen für den Wert. Beim Initialisieren akzeptiert eine <xref:System.Collections.Generic.Dictionary`2>, or any collection whose `Add`-Methode mehrere Parameter. Jeder Satz von Parametern muss wie im folgenden Beispiel in Klammern eingeschlossen sein.  
  
## <a name="example"></a>Beispiel  
 Das folgende Codebeispiel zeigt einen <xref:System.Collections.Generic.Dictionary`2> is initialized with instances of type `StudentName`.  
  
 [!code-cs[csProgGuideLINQ#34](../../../csharp/programming-guide/arrays/codesnippet/CSharp/how-to-initialize-a-dictionary-with-a-collection-initializer_1.cs)]  
  
 Beachten Sie die zwei Paare geschweifter Klammern in jedem Element der Auflistung. Die inneren Klammern umschließen den Objektinitialisierer für `StudentName`, und die äußeren Klammern umschließen den Initialisierer für das Schlüssel/Wertpaar, das `students`<xref:System.Collections.Generic.Dictionary`2> hinzugefügt wird. Schließlich wird der ganze Auflistungsinitialisierer für das Wörterbuch in geschweifte Klammern eingeschlossen.  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
 Um diesen Code auszuführen, kopieren Sie die Klasse, und fügen Sie sie in ein Visual C#-Konsolenanwendungsprojekt ein, das in [!INCLUDE[vs_current_short](../../../csharp/programming-guide/classes-and-structs/includes/vs_current_short_md.md)] erstellt wurde. Standardmäßig wird dieses Projekt mit Version 3.5 von [!INCLUDE[dnprdnshort](../../../csharp/getting-started/includes/dnprdnshort_md.md)] verwendet und verfügt über einen Verweis auf „System.Core.dll“ und eine using-Anweisung für „System.Linq“. Wenn eine oder mehrere dieser Anforderungen im Projekt nicht vorhanden sind, können Sie sie manuell hinzufügen.   
  
## <a name="see-also"></a>Siehe auch  
 [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)   
 [Objekt- und Auflistungsinitialisierer](../../../csharp/programming-guide/classes-and-structs/object-and-collection-initializers.md)
