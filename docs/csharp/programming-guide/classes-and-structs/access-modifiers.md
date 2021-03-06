---
title: Zugriffsmodifizierer (C#-Programmierhandbuch) | Microsoft-Dokumentation
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- C# Language, access modifiers
- access modifiers [C#], about
ms.assetid: 6e81ee82-224f-4a12-9baf-a0dca2656c5b
caps.latest.revision: 32
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
ms.openlocfilehash: 9940012829038f585ad78a10b70fe2941753e40e
ms.lasthandoff: 03/13/2017

---
# <a name="access-modifiers-c-programming-guide"></a>Zugriffsmodifizierer (C#-Programmierhandbuch)
Alle Typen und Typmember haben eine Zugriffsebene, die steuert, ob sie von anderem Code in Ihrer Assembly oder anderen Assemblys verwendet werden können. Sie können die folgenden Zugriffsmodifizierer verwenden, um den Zugriff auf einen Typ oder Member anzugeben, wenn Sie sie deklarieren:  
  
 [public](../../../csharp/language-reference/keywords/public.md)  
 Auf den Typ oder Member kann von jedem Code in der gleichen Assembly oder einer anderen Assembly, die darauf verweist, zugegriffen werden.  
  
 [private](../../../csharp/language-reference/keywords/private.md)  
 Auf den Typ oder Member kann nur von Code in der gleichen Klasse oder Struktur zugegriffen werden.  
  
 [protected](../../../csharp/language-reference/keywords/protected.md)  
 Auf den Typ oder Member kann nur von Code in der gleichen Klasse oder Struktur oder in einer Klasse, die von dieser Klasse abgeleitet ist, zugegriffen werden.  
  
 [internal](../../../csharp/language-reference/keywords/internal.md)  
 Auf den Typ oder Member kann von jedem Code in der gleichen Assembly zugegriffen werden, jedoch nicht von Code in einer anderen Assembly.  
  
 `protected internal`  
 Auf den Typ oder Member kann von jedem Code in der gleichen Assembly, in der er deklariert ist, oder von jeder abgeleiteten Klasse in einer anderen Assembly zugegriffen werden. Zugriff von einer anderen Assembly muss innerhalb einer Klassendeklaration erfolgen, die von der Klasse abgeleitet ist, in der das geschützte interne Element deklariert wird, und dies muss über eine Instanz des Typs der abgeleiteten Klasse erfolgen.  
  
 Die folgenden Beispiele veranschaulichen, wie Zugriffsmodifizierer für einen Typ und Member angegeben werden:  
  
 [!code-cs[csProgGuideObjects#72](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/access-modifiers_1.cs)]  
  
 Nicht alle Zugriffsmodifizierer können von allen Typen oder Membern in allen Kontexten verwendet werden, und in einigen Fällen wird der Zugriff auf einen Typmember vom Zugriff auf den enthaltenden Typ eingeschränkt. Die folgenden Abschnitte enthalten ausführlichere Informationen zu diesem Zugriff.  
  
## <a name="class-and-struct-accessibility"></a>Zugriff auf Klasse und Strukturen  
 Klassen und Strukturen, die innerhalb eines Namespace (mit anderen Worten, die nicht in anderen Klassen oder Strukturen geschachtelt sind) direkt deklariert werden, können entweder öffentlich oder intern sein. Wenn kein Modifizierer angegeben ist, wird standardmäßig „internal“ verwendet.  
  
 Strukturmember, einschließlich geschachtelter Klassen und Strukturen, können als öffentlich, intern oder privat deklariert werden. Klassenmember, einschließlich geschachtelter Klassen und Strukturen, können public, protected internal, protected, internal oder private sein. Die Zugriffsebene für Klassenmember und Strukturmember, einschließlich geschachtelter Klassen und Strukturen, ist standardmäßig privat. Auf private geschachtelte Typen kann nicht von außerhalb des enthaltenden Typs zugegriffen werden.  
  
 Abgeleitete Klassen können keine höhere Verfügbarkeit als ihre Basistypen aufweisen. Das heißt, dass Sie über keine öffentliche Klasse `B` verfügen können, die von einer internen Klasse `A` abgeleitet wird. Wenn dies zulässig wäre, müssten Sie `A` öffentlich machen, da auf alle geschützten oder internen Member von `A` aus der abgeleiteten Klasse zugegriffen werden kann.  
  
 Sie können mithilfe des InternalsVisibleTo-Attributs bestimmten anderen Assemblys den Zugriff auf die internen Typen ermöglichen. Weitere Informationen finden Sie unter [Friend-Assemblys](http://msdn.microsoft.com/library/df0c70ea-2c2a-4bdc-9526-df951ad2d055).  
  
## <a name="class-and-struct-member-accessibility"></a>Zugriff auf Klassen- und Strukturmember  
 Klassenmember (einschließlich geschachtelter Klassen und Strukturen) können mit einem der fünf Zugriffstypen deklariert werden. Strukturmember können nicht als geschützt deklariert werden, da Strukturen keine Vererbung unterstützen.  
  
 Normalerweise ist der Zugriff auf einen Member nicht höher als der Zugriff des Typs, der ihn enthält. Allerdings kann auf einen öffentlichen Member einer internen Klasse möglicherweise von außerhalb der Assembly zugegriffen werden, wenn der Member Schnittstellenmethoden implementiert oder virtuelle Methoden überschreibt, die in einer öffentlichen Basisklasse definiert sind.  
  
 Der Typ jedes Members, der ein Feld, eine Eigenschaft oder ein Ereignis ist, muss mindestens über dieselben Zugriffstypen verfügen wie der Member selbst. Ebenso müssen der Rückgabetyp und die Parametertypen eines Members, der eine Methode, Indexer oder Delegat ist, mindestens über dieselben Zugriffstypen verfügen, wie der Member selbst. Zum Beispiel können Sie nicht über eine öffentliche Methode `M` verfügen, die eine Klasse `C` zurückgibt, außer wenn `C` ebenfalls öffentlich ist. Ebenso können Sie nicht über eine geschützte Eigenschaft des Typs `A` verfügen, wenn `A` als privat deklariert wurde.  
  
 Benutzerdefinierte Operatoren müssen immer als öffentlich deklariert werden. Weitere Informationen finden Sie unter [Operator (C#-Referenz)](../../../csharp/language-reference/keywords/operator.md).  
  
 Destruktoren können nicht über Zugriffsmodifizierer verfügen.  
  
 Um die Zugriffsebene für eine Klasse oder Struktur festzulegen, fügen Sie das entsprechende Schlüsselwort zur Memberdeklaration hinzu, wie im folgenden Beispiel dargestellt.  
  
 [!code-cs[csProgGuideObjects#73](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/access-modifiers_2.cs)]  
  
> [!NOTE]
>  Die geschützte interne Zugriffsebene heißt geschützt ODER intern, nicht geschützt UND intern. Anders gesagt, kann auf einen geschützten internen Member aus einer Klasse in der gleichen Assembly, einschließlich der abgeleiteten Klassen, zugegriffen werden. Um Zugriff auf abgeleitete Klassen in derselben Assembly zu beschränken, deklarieren Sie die Klasse selbst als intern, und ihre Member als geschützt.  
  
## <a name="other-types"></a>Andere Typen  
 Schnittstellen, die direkt innerhalb eines Namespace deklariert werden, können als öffentlich oder intern deklariert werden, und haben wie Klassen und Strukturen standardmäßig internen Zugriff. Schnittstellenmember sind immer öffentlich, da es der Zweck einer Schnittstelle ist, anderen Typen den Zugriff auf eine Klasse oder Struktur zu ermöglichen. Auf Schnittstellenmember können keine Zugriffsmodifizierer angewendet werden.  
  
 Enumerationsmember sind immer öffentlich, und es können keine Zugriffsmodifizierer angewendet werden.  
  
 Delegaten verhalten sich wie Klassen und Strukturen. Standardmäßig haben sie internen Zugriff, wenn Sie direkt innerhalb eines Namespace deklariert werden, und privaten Zugriff bei Schachtelung.  
  
## <a name="c-language-specification"></a>C#-Programmiersprachenspezifikation  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## <a name="see-also"></a>Siehe auch  
 [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)   
 [Klassen und Strukturen](../../../csharp/programming-guide/classes-and-structs/index.md)   
 [Schnittstellen](../../../csharp/programming-guide/interfaces/index.md)   
 [private](../../../csharp/language-reference/keywords/private.md)   
 [public](../../../csharp/language-reference/keywords/public.md)   
 [internal](../../../csharp/language-reference/keywords/internal.md)   
 [protected](../../../csharp/language-reference/keywords/protected.md)   
 [class](../../../csharp/language-reference/keywords/class.md)   
 [struct](../../../csharp/language-reference/keywords/struct.md)   
 [interface](../../../csharp/language-reference/keywords/interface.md)
