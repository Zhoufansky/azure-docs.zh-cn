---
title: Lucene 查询语法
titleSuffix: Azure Cognitive Search
description: 完整的 Lucene 查询语法参考，在 Azure 认知搜索中用于通配符、模糊搜索、正则表达式和其他高级查询构造。
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/10/2020
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: d35c96657f48905f37c9ebe246d81ebb9545cf27
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2020
ms.locfileid: "77149875"
---
# <a name="lucene-query-syntax-in-azure-cognitive-search"></a>Lucene Azure 认知搜索中的查询语法

你可以基于用于专用查询窗体的丰富[Lucene 查询分析器](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)语法针对 Azure 认知搜索编写查询：通配符、模糊搜索、邻近搜索、正则表达式是几个示例。 很多 Lucene 查询分析器语法[在 azure 认知搜索中](search-lucene-query-architecture.md)都是完整的，但*范围搜索*例外，在 azure 认知搜索通过 `$filter` 表达式构造。 

> [!NOTE]
> 完整的 Lucene 语法用于在[搜索文档](https://docs.microsoft.com/rest/api/searchservice/search-documents)API 的**搜索**参数中传递的查询表达式，而不是与用于该 api 的[$filter](search-filters.md)参数的[OData 语法](query-odata-filter-orderby-syntax.md)混淆。 这些不同的语法具有各自的规则用于构造查询、转义字符串等。

## <a name="how-to-invoke-full-parsing"></a>如何调用完整分析

设置 `queryType` 搜索参数来指定要使用的分析。 有效值包括 `simple|full`，其中默认值为 `simple`，`full` 则用于 Lucene。 

<a name="bkmk_example"></a> 

### <a name="example-showing-full-syntax"></a>显示完整语法的示例

下面的示例使用 Lucene 查询语法在索引中查找文档，其在 `queryType=full` 参数中清晰易见。 此查询返回酒店，其中类别字段包含字词“budget”和所有包含短语“recently renovated”的可搜索字段。 作为字词提升值 (3)，包含短语“最近更新”的文档排名会更高。  

`searchMode=all` 参数是在此示例中是相关的。 无论运算符何时出现在查询上，通常都应该设置 `searchMode=all` 以确保匹配所有条件。

```
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2019-05-06&querytype=full
```

 或者使用 POST：  

```
POST /indexes/hotels/docs/search?api-version=2019-05-06
{
  "search": "category:budget AND \"recently renovated\"^3",
  "queryType": "full",
  "searchMode": "all"
}
```

有关其他示例，请参阅[在 Azure 认知搜索中生成查询的 Lucene 查询语法示例](search-query-lucene-examples.md)。 有关指定查询参数的全部临时的详细信息，请参阅[搜索&#40;文档 Azure 认知搜索&#41;REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)。

> [!NOTE]  
>  Azure 认知搜索还支持[简单的查询语法](query-simple-syntax.md)，这是一种简单且可靠的查询语言，可用于简单的关键字搜索。  

##  <a name="bkmk_syntax"></a> 语法基础  
 下面的语法基础适用于所有使用 Lucene 语法的查询。  

### <a name="operator-evaluation-in-context"></a>上下文中的运算符评估

位置决定符号解释为运算符或者解释为字符串中的另一个字符。

例如，Lucene 完整语法中，波浪线 (~) 用于模糊搜索和邻近搜索。 如果放在引用短语之后，则 ~ 调用邻近搜索。 如果放在术语末尾，则 ~ 调用模糊搜索。

该术语中，例如“business~analyst”，字符不评估为运算符。 在此情况下，假设查询是术语或短语查询，则使用[词法分析](search-lucene-query-architecture.md#stage-2-lexical-analysis)的[全文搜索](search-lucene-query-architecture.md)会删除 ~ 并将术语“business~analyst”分为两部分：business 或 analyst。

上面的示例是波形符 (~)，不过相同原则也适用于每个运算符。

### <a name="escaping-special-characters"></a>转义特殊字符

 特殊字符必须进行转义才能用作搜索文本的一部分。 可以使用反斜杠 (\\) 为其添加前缀来进行转义。 需要转义的特殊字符包括：  
`+ - && || ! ( ) { } [ ] ^ " ~ * ? : \ /`  

 例如，若要对通配符进行转义，请使用 \\\*。

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>对 URL 中的不安全及保留字符进行编码

 请确保对 URL 中的所有不安全和保留字符进行编码。 例如，“#”是不安全字符，因为它是 URL 中的片段/定位标识符。 如果用于 URL，则该字符必须编码为 `%23`。 "&" 和 "=" 是保留字符的示例，因为它们分隔参数并在 Azure 认知搜索中指定值。 请参阅 [RFC1738：统一资源定位器 (URL)](https://www.ietf.org/rfc/rfc1738.txt) 获取更多详细信息。

 不安全字符为 ``" ` < > # % { } | \ ^ ~ [ ]``。 保留字符为 `; / ? : @ = + &`。

### <a name="precedence-operators-grouping-and-field-grouping"></a>优先运算符：分组和字段分组  
 可以使用圆括号创建子查询，其包括附加说明语句中的运算符。 例如，`motel+(wifi||luxury)` 将搜索包含“motel”术语以及“wifi”或“luxury”（或两者）的文档。

字段分组与之类似，但将分组范围限定为单个字段。 例如，`hotelAmenities:(gym+(wifi||pool))` 在“hotelAmenities”字段中搜索“gym”和“wifi”，或者“gym”和“pool”。  

### <a name="searchmode-parameter-considerations"></a>SearchMode 参数注意事项  
 `searchMode` 在[Azure 认知搜索中的简单查询语法](query-simple-syntax.md)中所述的查询的影响同样适用于 Lucene 查询语法。 也就是说，如果不清楚设置参数的方法的含义，那么 `searchMode` 与 NOT 运算符结合使用可能会导致查询结果异常。 如果保留默认值 `searchMode=any`，并使用 NOT 运算符，则该操作会作为 OR 操作进行计算，这样“New York”NOT“Seattle”会返回所有不是西雅图的城市。  

##  <a name="bkmk_boolean"></a>布尔运算符（AND、OR、NOT） 
 始终全部以大写字母指定文本布尔运算符 (AND、OR、NOT)。  

### <a name="or-operator-or-or-"></a>OR 运算符 `OR` 或 `||`

OR 运算符是一个竖条或管状字符。 例如：`wifi || luxury` 将搜索包含"wifi"或"luxury"（或两者）的文档。 由于 OR 是默认连接运算符，因此也可以省略，这样 `wifi luxury` 等同于 `wifi || luxuery`。

### <a name="and-operator-and--or-"></a>AND 运算符 `AND`、`&&` 或 `+`

AND 运算符为 & 号或加号。 例如：`wifi && luxury` 将搜索包含“wifi”和“luxury”的文档。 加号字符 (+) 用于所需术语。 例如，`+wifi +luxury` 规定两个术语必须出现在单个文档的某个字段中。


### <a name="not-operator-not--or--"></a>NOT 运算符 `NOT`、`!` 或 `-`

NOT 运算符为感叹号或减号。 例如：`wifi !luxury` 将搜索包含“wifi”和/或不包含“luxury”的文档。 `searchMode` 选项控制在没有 + 或 || 运算符的情况下，具有 NOT 运算符的术语与查询中的其他术语进行 ANDed 运算还是 ORed 运算。 请记住，`searchMode` 可设置为 `any`（默认）或 `all`。

使用 `searchMode=any` 可以以包含更多结果的方式提高查询的查全率，且默认情况下将解释为“OR NOT”。 例如，`wifi -luxury` 将匹配包含术语“wifi”或不包含术语“luxury”的文档。

使用 `searchMode=all` 可以以包含更少结果的方式提高查询的精确度，且默认情况下将解释为“AND NOT”。 例如，`wifi -luxury` 将匹配包含术语 `wifi` 或不包含术语 `luxury` 的文档。 这对于 - 运算符来说可能是更直观的行为。 因此，如果希望优化搜索的精确度（而非查全率），且用户在搜索中频繁使用 `-` 运算符，那么应考虑选择 `searchMode=all` 而不是 `searchMode=any`。

##  <a name="bkmk_querysizelimits"></a> 查询大小限制  
 可以向 Azure 认知搜索发送的查询大小有限制。 具体而言，最多可以有 1024 条子句（以 AND、OR 等分隔的表达式）。 此外，查询中任何单个术语的大小限制为大约 32 KB。 如果应用程序以编程方式生成搜索查询，则建议将其设计为不会生成无限大小的查询。  

##  <a name="bkmk_searchscoreforwildcardandregexqueries"></a> 对通配符和正则表达式查询评分
 Azure 认知搜索使用基于频率的评分（[TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)）进行文本查询。 但是，对于术语范围可能很广的通配符和正则表达式查询，则忽略频率因子，以防止排名偏向于比较少见的术语匹配。 通配符和正则表达式搜索对所有匹配项和正则表达式搜索进行相同处理。

##  <a name="bkmk_fields"></a>现场搜索  
您可以使用 `fieldName:searchExpression` 语法定义现场搜索操作，其中，搜索表达式可以是单个词或短语，也可以是括号中更复杂的表达式（可以选择使用布尔运算符）。 下面是部分示例：  

- 流派：爵士乐无历史记录  

- 艺术家：（“Miles Davis”、“John Coltrane”）

如果想要两个字符串评估为单个实体，请务必将多个字符串放置在引号内，正如这个在 `artists` 字段中搜索两个不同艺术家的情况一样。  

`fieldName:searchExpression` 中指定的字段必须是 `searchable` 字段。  有关如何在字段定义中使用索引属性的详细信息，请参阅[创建索引](https://docs.microsoft.com/rest/api/searchservice/create-index)。  

> [!NOTE]
> 使用现场搜索表达式时，无需使用 `searchFields` 参数，因为每个现场搜索表达式都显式指定了字段名称。 但是，如果要运行的查询中某些部分的作用域限定为特定字段，则仍可使用 `searchFields` 参数，其余部分则可应用于多个字段。 例如，查询 `search=genre:jazz NOT history&searchFields=description` 仅将 `jazz` 与 `genre` 字段匹配，而它会将 `NOT history` 与 `description` 字段匹配。 `fieldName:searchExpression` 中提供的字段名称始终优先于 `searchFields` 参数，这就是在此示例中，我们不需要在 `searchFields` 参数中包含 `genre`。

##  <a name="bkmk_fuzzy"></a> 模糊搜索  
 模糊搜索在构造相似的术语中查找匹配项。 对于 [Lucene 文档](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)，模糊搜索基于 [Damerau-Levenshtein 距离](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)。 模糊搜索可以将满足距离条件的项扩展到最多 50 个字词。 

 若要进行模糊搜索，请在单个词末尾使用“~”波形符，另附带指定编辑距离的可选参数（0 到 2 [默认] 之间的值）。 例如“blue~”或“blue~1”会返回“blue”、“blues”和“glue”。

 模糊搜索只能应用于术语，而不能应用于短语，但你可以在多部分名称或短语中单独追加每个单词的颚化符。 例如，"Unviersty ~ of ~" Wshington ~ "将匹配" 华盛顿大学 "。
 

##  <a name="bkmk_proximity"></a> 邻近搜索  
 邻近搜索用于搜索文档中彼此邻近的术语。 在短语末尾插入波形符“~”，后跟创建邻近边界的词数。 例如 `"hotel airport"~5` 将查找文档中彼此相距 5 个字以内的术语“酒店”和“机场”。  


##  <a name="bkmk_termboost"></a> 术语提升  
 术语提升是指相对于不包含术语的文档，提高包含提升术语的文档排名。 这不同于计分配置文件，因为计分配置文件提升某些字段，而非特定术语。  

以下示例有助于解释这些差异。 假设某个字段中存在提升匹配度的计分概要文件，例如 [musicstoreindex 示例](index-add-scoring-profiles.md#bkmk_ex)中的“流派”。 术语提升可用于进一步提升高于其他术语的某些搜索词。 例如 `rock^2 electronic` 将提升“流派”字段（高于搜索中其他搜索字段）中包含搜索词的文档。 另外，由于术语提升值 (2)，包含搜索词“rock”的文档的排名要比包含搜索词“electronic”的要高。  

 若要提升术语，请使用插入符号“^”，并且所搜索术语末尾还要附加提升系数（数字）。 还可以提升短语。 提升系数越高，术语相对于其他搜索词的相关性也越大。 默认情况下，提升系数是 1。 虽然提升系数必须是正数，但可以小于 1（例如 0.20）。  

##  <a name="bkmk_regex"></a> 正则表达式搜索  
 正则表达式搜索基于正斜杠“/”之间的内容查找匹配项，如在 [RegExp 类](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)中所记录的那样。  

 例如，若要查找包含“汽车旅馆”或“酒店”的文档，请指定 `/[mh]otel/`。  正则表达式搜索与单个词匹配。   

##  <a name="bkmk_wildcard"></a> 通配符搜索  
 可将通常可识别的语法用于多个 (*) 或单个 (?) 字符通配符搜索。 请注意，Lucene 查询分析器支持将这些符号与单个术语一起使用，但不能与短语一起使用。  

 例如，若要查找前缀为“note”的词（如“notebook”或“notepad”）的文档，请指定“note*”。  

> [!NOTE]  
>  不得将 * 或 ? 符号用作搜索的第一个字符。  
>  不对通配符搜索查询执行文本分析。 查询时，通配符查询术语与搜索索引中所分析的字词进行比较并展开。

## <a name="see-also"></a>请参阅  

+ [搜索文档](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [用于筛选器和排序的 OData 表达式语法](query-odata-filter-orderby-syntax.md)   
+ [Azure 认知搜索中的简单查询语法](query-simple-syntax.md)   
