---
title: "Debuggen von Ausdrucksbäumen in Visual Studio (C#) | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 1369fa25-0fbd-4b92-98d0-8df79c49c27a
caps.latest.revision: 4
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 14ab67e78a3b4c4819ddca36a406526e78f5485e
ms.lasthandoff: 03/13/2017

---
# <a name="debugging-expression-trees-in-visual-studio-c"></a>Debuggen von Ausdrucksbäumen in Visual Studio (C#)
Sie können die Struktur und den Inhalt von Ausdrucksbaumstrukturen beim Debuggen Ihrer Anwendung analysieren. Um einen schnellen Überblick über die Ausdrucksbaumstruktur zu erhalten, können Sie die `DebugView`-Eigenschaft verwenden, die nur im Debugmodus verfügbar ist. Weitere Informationen zum Debugging finden Sie unter [Debuggen in Visual Studio](https://docs.microsoft.com/visualstudio/debugger/debugging-in-visual-studio).  
  
 Die `DebugView`-Eigenschaft stellt den Inhalt von Ausdrucksbaumstrukturen mithilfe von Visual Studio-Schnellansichten übersichtlicher dar. Weitere Informationen finden Sie unter [Create Custom Visualizers (Erstellen benutzerdefinierter Schnellansichten)](https://docs.microsoft.com/visualstudio/debugger/create-custom-visualizers-of-data).  
  
### <a name="to-open-a-visualizer-for-an-expression-tree"></a>So öffnen Sie eine Schnellansicht für eine Ausdrucksbaumstruktur  
  
1.  Klicken Sie auf das Lupensymbol, das in **DataTips** neben der `DebugView`-Eigenschaft einer Ausdrucksbaumstruktur, neben einem **Überwachungsfenster**, einem **Auto**- oder neben einem **Lokalfenster** angezeigt wird.  
  
     Eine Liste von Schnellansichten wird angezeigt.  
  
2.  Klicken Sie auf die gewünschte Schnellansicht.  
  
 Jeder Ausdruckstyp wird in der Schnellansicht angezeigt, wie in den folgenden Abschnitten beschrieben.  
  
## <a name="parameterexpressions"></a>ParameterExpressions  
 <xref:System.Linq.Expressions.ParameterExpression>-Variablennamen werden mit dem „$“-Symbol zu Beginn dargestellt.  
  
 Wenn ein Parameter nicht über einen Namen verfügt, wird ihm ein automatisch generierter Name zugewiesen, z.B. `$var1` oder `$var2`.  
  
### <a name="examples"></a>Beispiele  
  
|Ausdruck|`DebugView`-Eigenschaft|  
|----------------|--------------------------|  
|`ParameterExpression numParam =  Expression.Parameter(typeof(int), "num");`|`$num`|  
|`ParameterExpression numParam =  Expression.Parameter(typeof(int));`|`$var1`|  
  
## <a name="constantexpressions"></a>ConstantExpressions  
 Für <xref:System.Linq.Expressions.ConstantExpression>-Objekte, die Integerwerte, Zeichenfolgen und `null` darstellen, wird der Wert der Konstante dargestellt.  
  
 Für numerische Typen, die über Standardsuffixe als C#-Literale verfügen, wird das Suffix zum Wert hinzugefügt. Die folgende Tabelle zeigt die verschiedenen numerischen Typen und ihre zugeordneten Suffixe.  
  
|Typ|Suffix|  
|----------|------------|  
|<xref:System.UInt32>|U|  
|<xref:System.Int64>|L|  
|<xref:System.UInt64>|UL|  
|<xref:System.Double>|D|  
|<xref:System.Single>|F|  
|<xref:System.Decimal>|M|  
  
### <a name="examples"></a>Beispiele  
  
|Ausdruck|`DebugView`-Eigenschaft|  
|----------------|--------------------------|  
|`int num = 10; ConstantExpression expr = Expression.Constant(num);`|10|  
|`double num = 10; ConstantExpression expr = Expression.Constant(num);`|10D|  
  
## <a name="blockexpression"></a>BlockExpression  
 Wenn sich der Typ eines <xref:System.Linq.Expressions.BlockExpression>-Objekts vom Typ des letzten Ausdrucks im Block unterscheidet, wird der Typ in der `DebugInfo`-Eigenschaft in spitzen Klammern (\< und >) dargestellt. Andernfalls wird der Typ des <xref:System.Linq.Expressions.BlockExpression>-Objekts nicht angezeigt.  
  
### <a name="examples"></a>Beispiele  
  
|Ausdruck|`DebugView`-Eigenschaft|  
|----------------|--------------------------|  
|`BlockExpression block = Expression.Block(Expression.Constant("test"));`|`.Block() {`<br /><br /> `"test"`<br /><br /> `}`|  
|`BlockExpression block =  Expression.Block(typeof(Object), Expression.Constant("test"));`|`.Block<System.Object>() {`<br /><br /> `"test"`<br /><br /> `}`|  
  
## <a name="lambdaexpression"></a>LambdaExpression.  
 <xref:System.Linq.Expressions.LambdaExpression>-Objekte werden zusammen mit ihren Delegattypen angezeigt.  
  
 Wenn ein Lambda-Ausdruck über keinen Namen verfügt, wird ihm ein automatisch generierter Name zugewiesen, z.B. `#Lambda1` oder `#Lambda2`.  
  
### <a name="examples"></a>Beispiele  
  
|Ausdruck|`DebugView`-Eigenschaft|  
|----------------|--------------------------|  
|`LambdaExpression lambda =  Expression.Lambda<Func<int>>(Expression.Constant(1));`|`.Lambda #Lambda1<System.Func'1[System.Int32]>() {`<br /><br /> `1`<br /><br /> `}`|  
|`LambdaExpression lambda =  Expression.Lambda<Func<int>>(Expression.Constant(1), "SampleLambda", null);`|`.Lambda SampleLambda<System.Func'1[System.Int32]>() {`<br /><br /> `1`<br /><br /> `}`|  
  
## <a name="labelexpression"></a>LabelExpression  
 Bei Angabe eines Standardwerts für das <xref:System.Linq.Expressions.LabelExpression>-Objekt wird dieser Wert vor dem <xref:System.Linq.Expressions.LabelTarget>-Objekt angezeigt.  
  
 Das `.Label`-Token gibt den Anfang der Bezeichnung an. Das `.LabelTarget`-Token gibt den Zielort an, zu dem gewechselt werden soll.  
  
 Wenn eine Bezeichnung nicht über einen Namen verfügt, wird ihr ein automatisch generierter Name zugewiesen (z.B. `#Label1` oder `#Label2`).  
  
### <a name="examples"></a>Beispiele  
  
|Ausdruck|`DebugView`-Eigenschaft|  
|----------------|--------------------------|  
|`LabelTarget target = Expression.Label(typeof(int), "SampleLabel"); BlockExpression block = Expression.Block( Expression.Goto(target, Expression.Constant(0)), Expression.Label(target, Expression.Constant(-1)));`|`.Block() {`<br /><br /> `.Goto SampleLabel { 0 };`<br /><br /> `.Label`<br /><br /> `-1`<br /><br /> `.LabelTarget SampleLabel:`<br /><br /> `}`|  
|`LabelTarget target = Expression.Label(); BlockExpression block = Expression.Block( Expression.Goto(target5), Expression.Label(target5));`|`.Block() {`<br /><br /> `.Goto #Label1 { };`<br /><br /> `.Label`<br /><br /> `.LabelTarget #Label1:`<br /><br /> `}`|  
  
## <a name="checked-operators"></a>Überprüfte Operatoren  
 Überprüften Operatoren wird in der Ansicht das Symbol "#" vorangestellt. Der überprüfte Additionsoperator wird z.B. als `#+` angezeigt.  
  
### <a name="examples"></a>Beispiele  
  
|Ausdruck|`DebugView`-Eigenschaft|  
|----------------|--------------------------|  
|`Expression expr = Expression.AddChecked( Expression.Constant(1), Expression.Constant(2));`|`1 #+ 2`|  
|`Expression expr = Expression.ConvertChecked( Expression.Constant(10.0), typeof(int));`|`#(System.Int32)10D`|  
  
## <a name="see-also"></a>Siehe auch  
 [Ausdrucksbaumstrukturen (C#)](../../../../csharp/programming-guide/concepts/expression-trees/index.md)   
 [Debuggen in Visual Studio](https://docs.microsoft.com/visualstudio/debugger/debugging-in-visual-studio)   
 [Erstellen benutzerdefinierter Schnellansichten](https://docs.microsoft.com/visualstudio/debugger/create-custom-visualizers-of-data)
