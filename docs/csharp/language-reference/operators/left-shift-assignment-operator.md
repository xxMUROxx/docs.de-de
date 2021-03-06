---
title: "Operator &lt;&lt;= (C#-Referenz) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-csharp"
ms.topic: "article"
f1_keywords: 
  - "<<=_CSharpKeyword"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "<<=-Operator (Linksschiebezuweisung) [C#]"
  - "Linksschiebezuweisungsoperator (<<=) [C#]"
ms.assetid: 3bc99c78-1edb-4827-86fc-bce6c3048871
caps.latest.revision: 16
author: "BillWagner"
ms.author: "wiwagn"
caps.handback.revision: 16
---
# Operator &lt;&lt;= (C#-Referenz)
Der Linksschiebezuweisungsoperator.  
  
## Hinweise  
 Ein Ausdruck der Form  
  
```  
x <<= y  
```  
  
 wird ausgewertet als  
  
```  
x = x << y  
```  
  
 mit der Ausnahme, dass `x` nur einmal ausgewertet wird.  Der [Operator \<\<](../../../csharp/language-reference/operators/left-shift-operator.md) verschiebt `x` um die durch `y` angegebenen Anzahl von Bits nach links.  
  
 Der Operator `<<=` kann nicht direkt überladen werden. Benutzerdefinierte Typen können jedoch den [Operator \<\<](../../../csharp/language-reference/operators/left-shift-operator.md) überladen \(siehe [operator](../../../csharp/language-reference/keywords/operator.md)\).  
  
## Beispiel  
 [!code-cs[csRefOperators#12](../../../csharp/language-reference/operators/codesnippet/CSharp/left-shift-assignment-operator_1.cs)]  
  
## Siehe auch  
 [C\#\-Referenz](../../../csharp/language-reference/index.md)   
 [C\#\-Programmierhandbuch](../../../csharp/programming-guide/index.md)   
 [C\#\-Operatoren](../../../csharp/language-reference/operators/index.md)