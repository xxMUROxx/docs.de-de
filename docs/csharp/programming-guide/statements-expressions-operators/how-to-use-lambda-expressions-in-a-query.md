---
title: "Vorgehensweise: Verwenden von Lambdaausdrücken in einer Abfrage (C#-Programmierhandbuch) | Microsoft-Dokumentation"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- lambda expressions [C#], in LINQ
ms.assetid: 3cac4d25-d11f-4abd-9e7c-0f02e97ae06d
caps.latest.revision: 16
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 7bfc46015b0d4603c4d63478e804f862c0c65b68
ms.lasthandoff: 03/13/2017

---
# <a name="how-to-use-lambda-expressions-in-a-query-c-programming-guide"></a>Gewusst wie: Verwenden von Lambdaausdrücken in einer Abfrage (C#-Programmierhandbuch)
Sie können Lambdaausdrücke nicht direkt in der Abfragesyntax verwenden, sondern nur in Methodenaufrufen. Abfrageausdrücke können Methodenaufrufe enthalten. Einige Abfragevorgänge können nur in Methodensyntax ausgedrückt werden. Weitere Informationen zu den Unterschieden zwischen Abfragesyntax und Methodensyntax finden Sie unter [Abfragesyntax und Methodensyntax in LINQ](../../../csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md).  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel veranschaulicht, wie Sie Lambdaausdrücke in methodenbasierten Abfragen mithilfe des Standardabfrageoperators <xref:System.Linq.Enumerable.Where%2A?displayProperty=fullName> verwenden können. Bitte beachten Sie, dass die Methode <xref:System.Linq.Enumerable.Where%2A> in diesem Beispiel einen Eingabeparameter des Delegattyps <xref:System.Func%601> aufweist, und dass dieser Delegat eine Ganzzahl als Eingabe akzeptiert und einen booleschen Wert zurückgibt. Der Lambdaausdruck kann in diesen Delegat konvertiert werden. Wenn dies eine [!INCLUDE[vbtecdlinq](../../../csharp/includes/vbtecdlinq_md.md)]-Abfrage wäre, die die Methode <xref:System.Linq.Queryable.Where%2A?displayProperty=fullName> verwendet, wäre der Parametertyp `Expression<Func\<int,bool>>`, aber der Lambdaausdruck wäre der gleiche. Weitere Informationen zu Ausdruckstypen finden Sie unter <xref:System.Linq.Expressions.Expression?displayProperty=fullName>.  
  
 [!code-cs[csProgGuideLINQ#1](../../../csharp/programming-guide/arrays/codesnippet/CSharp/how-to-use-lambda-expressions-in-a-query_1.cs)]  
  
## <a name="example"></a>Beispiel  
 Das folgende Beispiel veranschaulicht, wie Sie Lambdaausdrücke in einem Methodenaufruf eines Abfrageausdrucks verwenden können. Der Lambdaausdruck ist erforderlich, da der Standardabfrageoperator <xref:System.Linq.Enumerable.Sum%2A> nicht durch Abfragesyntax aufgerufen werden kann.  
  
 Die Abfrage gruppiert die Studenten zunächst anhand ihres Jahrs wie in der `GradeLevel`-Enumeration definiert. Dann fügt sie die Gesamtergebnisse jedes Studenten in jeder Gruppe hinzu. Dazu sind zwei `Sum`-Operationen erforderlich. Mit der inneren `Sum`-Operation wird das Gesamtergebnis für jeden Studenten berechnet, und mit der äußeren `Sum`-Operation wird eine kombinierte laufende Summe für alle Studenten in der Gruppe berechnet.  
  
 [!code-cs[csProgGuideLINQ#2](../../../csharp/programming-guide/arrays/codesnippet/CSharp/how-to-use-lambda-expressions-in-a-query_2.cs)]  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
 Um diesen Code auszuführen, kopieren Sie die Methode, und fügen Sie sie in die `StudentClass` ein, die in [Vorgehensweise: Abfragen einer Auflistung von Objekten](../../../csharp/programming-guide/linq-query-expressions/how-to-query-a-collection-of-objects.md) bereitgestellt wird. Rufen Sie diese Methode dann in der `Main`-Methode auf.  
  
## <a name="see-also"></a>Siehe auch  
 [Lambda-Ausdrücke](../../../csharp/programming-guide/statements-expressions-operators/lambda-expressions.md)   
 [Ausdrucksbaumstrukturen](http://msdn.microsoft.com/library/fb1d3ed8-d5b0-4211-a71f-dd271529294b)
