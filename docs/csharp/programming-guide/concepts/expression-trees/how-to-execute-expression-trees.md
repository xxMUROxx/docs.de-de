---
title: "Vorgehensweise: Ausführen von Ausdrucksbaumstrukturen (C#) | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: get-started-article
dev_langs:
- CSharp
ms.assetid: b8c40db5-2464-4bb9-9001-8c2bc7f006c5
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
ms.openlocfilehash: 0e7d061644aa6c0f36b7a193ded5485000ad0309
ms.lasthandoff: 03/13/2017

---
# <a name="how-to-execute-expression-trees-c"></a>Vorgehensweise: Ausführen von Ausdrucksbaumstrukturen (C#)
In diesem Thema erfahren Sie, wie eine Ausdrucksbaumstruktur ausgeführt wird. Die Ausführung einer Ausdrucksbaumstruktur gibt möglicherweise einen Wert zurück. Es kann jedoch auch nur eine Aktion ausgeführt werden, z.B. das Aufrufen einer Methode.  
  
 Nur Ausdrucksbaumstrukturen, die Lambdaausdrücke darstellen, können ausgeführt werden. Ausdrucksbaumstrukturen, die Lambdaausdrücke darstellen, sind vom Typ <xref:System.Linq.Expressions.LambdaExpression> oder <xref:System.Linq.Expressions.Expression%601>. Um diese Ausdrucksbaumstruktur auszuführen, rufen Sie die <xref:System.Linq.Expressions.LambdaExpression.Compile%2A>-Methode auf, um einen ausführbaren Delegaten zu erstellen und diesen anschließend aufzurufen.  
  
> [!NOTE]
>  Wenn der Typ des Delegaten unbekannt ist, d.h., wenn der Lambdaausdruck des Typs <xref:System.Linq.Expressions.LambdaExpression> und nicht <xref:System.Linq.Expressions.Expression%601> entspricht, müssen Sie die <xref:System.Delegate.DynamicInvoke%2A>-Methode auf dem Delegaten aufrufen, anstatt ihn direkt aufzurufen.  
  
 Wenn eine Ausdrucksbaumstruktur keinen Lambdaausdruck darstellt, können Sie einen neuen Lambdaausdruck erstellen, der die ursprüngliche Ausdrucksbaumstruktur als Textkörper besitzt, indem Sie die <xref:System.Linq.Expressions.Expression.Lambda%60%601%28System.Linq.Expressions.Expression%2CSystem.Collections.Generic.IEnumerable%7BSystem.Linq.Expressions.ParameterExpression%7D%29>-Methode aufrufen. Anschließend können Sie den Lambdaausdruck ausführen, wie weiter oben in diesem Abschnitt beschrieben.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Codebeispiel wird veranschaulicht, wie eine Ausdrucksbaumstruktur ausgeführt wird, die das Potenzieren darstellt, indem ein Lambdaausdruck erstellt und ausgeführt wird. Das Ergebnis, das die potenzierte Zahl darstellt, wird angezeigt.  
  
```csharp  
// The expression tree to execute.  
BinaryExpression be = Expression.Power(Expression.Constant(2D), Expression.Constant(3D));  
  
// Create a lambda expression.  
Expression<Func<double>> le = Expression.Lambda<Func<double>>(be);  
  
// Compile the lambda expression.  
Func<double> compiledExpression = le.Compile();  
  
// Execute the lambda expression.  
double result = compiledExpression();  
  
// Display the result.  
Console.WriteLine(result);  
  
// This code produces the following output:  
// 8  
```  
  
## <a name="compiling-the-code"></a>Kompilieren des Codes  
  
-   Fügen Sie einen Projektverweis auf „System.Core.dll“ hinzu, wenn nicht bereits darauf verwiesen wird.  
  
-   Binden Sie den System.Linq.Expressions-Namespace ein.  
  
## <a name="see-also"></a>Siehe auch  
 [Ausdrucksbaumstrukturen (C#)](../../../../csharp/programming-guide/concepts/expression-trees/index.md)   
 [How to: Modify Expression Trees (C#) (Vorgehensweise: Bearbeiten von Ausdrucksbaumstrukturen (C#))](../../../../csharp/programming-guide/concepts/expression-trees/how-to-modify-expression-trees.md)