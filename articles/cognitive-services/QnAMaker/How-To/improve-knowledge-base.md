---
title: 改进知识库 - QnA Maker
titleSuffix: Azure Cognitive Services
description: 通过主动学习提高知识库的质量。 查看、接受或拒绝，添加时不删除或更改现有问题。
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 02/27/2020
ms.author: diberry
ms.openlocfilehash: dea2bf3b34ca336f3932dd85bf587184ab6881db
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "77914987"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>使用主动学习改进知识库

利用[活动学习](../Concepts/active-learning-suggestions.md)，你可以通过基于用户提交的问题和答案对来建议替代问题，从而提高知识库的质量。 查看这些建议后，可以将其添加到现有问题，也可以拒绝它们。

知识库不会自动更改。 要使更改生效，必须接受建议。 这些建议会添加问题，但不会更改或删除现有问题。


## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>升级运行时版本以使用活动学习

运行时版本 4.4.0 及更高版本支持主动学习。 如果你的知识库是在早期版本上创建的，请[升级运行时](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)以使用此功能。

## <a name="turn-on-active-learning-to-see-suggestions"></a>开启活动学习以查看建议

默认情况下，主动学习处于关闭状态。 启用它可以查看建议的问题。 开启活动学习后，需要将客户端应用程序中的信息发送到 QnA Maker。 有关详细信息，请参阅[使用 GenerateAnswer 的体系结构流和从机器人训练 api](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot)。

1. 选择 "**发布**" 以发布知识库。 仅从 GenerateAnswer API 预测终结点收集活动学习查询。 QnA Maker 门户中的 "测试" 窗格查询不会影响主动学习。

1. 若要在 QnA Maker 门户中打开活动学习，请转到右上角，选择你的**名称**，转到 "[**服务设置**](https://www.qnamaker.ai/UserSettings)"。

    ![从 "服务设置" 页中打开活动学习的建议问题替代方法。 在右上方菜单中选择您的用户名，然后选择 "服务设置"。](../media/improve-knowledge-base/Endpoint-Keys.png)


1. 找到 QnA Maker 服务，然后切换到“主动学习”。

    > [!div class="mx-imgBorder"]
    > [![在 "服务设置" 页上，切换活动学习功能。如果无法切换该功能，则可能需要升级您的服务。](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > 上图中的确切版本仅显示为示例。 你的版本可能不同。

    启用**活动学习**后，该知识库将基于用户提交的问题，定期提出新问题。 可以通过再次切换设置来禁用“主动学习”。

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>接受知识库中的活动学习建议

活动学习在批准建议后将更改知识库或搜索服务，并保存和训练。 如果你批准此建议，则会将其添加为替代问题。

1. 若要查看建议的问题，请在 "**编辑**知识库" 页上选择 "**查看选项**"，然后选择 "**显示活动的学习建议**"。

    [![在门户的 "编辑" 部分中，选择 "显示建议"，以查看活动学习的新问题备选方案。](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. 通过选择 "**按建议筛选**" 来使用问题和答案对筛选知识库，以便仅显示建议。

    [![使用 "按建议筛选" 开关来仅查看活动学习的建议问题备选方案。](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. 每个 QnA 对建议新的问题替代选项，其中包含一个复选标记 `✔`，接受此问题或 `x` 拒绝建议。 选中复选标记可以添加问题。

    [通过选择绿色复选标记或红色删除标记，![选择或拒绝活动学习的建议问题替代项。](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    您可以通过在上下文工具栏中选择 "**全部添加**" 或 "**全部拒绝**" 来添加或删除_所有建议_。

1. 选择“保存并训练”，将所做的更改保存到知识库。

1. 选择 "**发布**" 以允许在[GenerateAnswer API](metadata-generateanswer-usage.md#generateanswer-request-configuration)中提供更改。

    如果有5个或更多个类似查询被聚集，每30分钟 QnA Maker 建议要接受或拒绝的替代问题。


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>使用 GenerateAnswer 并通过 bot 训练 Api 的体系结构流

机器人或其他客户端应用程序应使用以下体系结构流来使用活动学习：

* 机器人使用 GenerateAnswer API[从知识库获取答案](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)，并使用 `top` 属性获取多个答案。
* 机器人确定显式反馈：
    * 使用您自己的[自定义业务逻辑](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user)，筛选出低分数。
    * 在机器人或客户端应用程序中，显示用户的可能答案列表，并获得用户选定的答案。
* 机器人会[将选择的应答发送回 QnA Maker](#bot-framework-sample-code)的[训练 API](#train-api)。


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>使用 GenerateAnswer 请求中的 top 属性获取几个匹配的答案

提交问题以 QnA Maker 获取答案时，JSON 正文的 `top` 属性将设置要返回的应答的数目。

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>使用评分属性以及业务逻辑获取显示用户的答案列表

当客户端应用程序（如聊天机器人）收到响应时，将返回前3个问题。 使用 `score` 属性分析分数之间的邻近性。 此邻近范围取决于你自己的业务逻辑。

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>在问题具有相似的分数时的客户应用程序跟进

客户端应用程序显示问题，其中包含一个选项，让用户选择最能表示其意图的_单个问题_。

一旦用户选择了一个现有问题，客户端应用程序就会使用 QnA Maker 的训练 API 作为反馈发送用户的选择。 此反馈将完成活动的学习反馈循环。

## <a name="train-api"></a>训练 API

向训练 API POST 请求发送活动学习反馈 QnA Maker。 API 签名如下：

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|HTTP 请求属性|名称|类型|目的|
|--|--|--|--|
|URL 路由参数|知识库 ID|字符串|知识库的 GUID。|
|自定义子域|QnAMaker 资源名称|字符串|资源名称用作 QnA Maker 的自定义子域。 发布知识库后，可以在 "设置" 页上找到此功能。 它作为 `host`列出。|
|标头|Content-Type|字符串|发送到 API 的正文的媒体类型。 默认值为： `application/json`|
|标头|授权|字符串|终结点密钥 (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)。|
|文章正文|JSON 对象|JSON|培训反馈|

JSON 正文具有几个设置：

|JSON 正文属性|类型|目的|
|--|--|--|--|
|`feedbackRecords`|array|反馈列表。|
|`userId`|字符串|接受建议问题的人员的用户 ID。 用户 ID 格式由您来了解。 例如，电子邮件地址可以是体系结构中的有效用户 ID。 可选。|
|`userQuestion`|字符串|用户查询的确切文本。 必需。|
|`qnaID`|数字|[GenerateAnswer 响应](metadata-generateanswer-usage.md#generateanswer-response-properties)中找到的问题 ID。 |

示例 JSON 正文如下所示：

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

成功的响应将返回状态204，并且不会返回 JSON 响应正文。

### <a name="batch-many-feedback-records-into-a-single-call"></a>成批发送多条反馈记录到一个调用中

在客户端应用程序（如机器人）中，可以存储数据，然后在 `feedbackRecords` 数组的单个 JSON 正文中发送多条记录。

示例 JSON 正文如下所示：

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>机器人框架示例代码

如果用户的查询应该用于活动学习，则机器人框架代码需要调用训练 API。 需要编写两个代码片段：

* 确定是否应将查询用于主动学习
* 将查询发送回用于活动学习 QnA Maker 的训练 API

在[Azure 机器人示例](https://aka.ms/activelearningsamplebot)中，这两个活动都已进行了编程。

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>用C# Bot Framework 4.X 训练 API 的示例代码

下面的代码演示如何将信息与训练 API 一起发送回 QnA Maker。 GitHub 上提供了[完整的代码示例](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore)。

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="endpoint">Endpoint URI of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string endpoint, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = endpoint + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>用于为 Bot Framework 4.x 定型 API 的示例 Node.js 代码

下面的代码演示如何将信息与训练 API 一起发送回 QnA Maker。 GitHub 上提供了[完整的代码示例](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs)。

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);

            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>活动学习保存在导出的知识库中

如果你的应用程序启用了活动学习，并且你导出了该应用，则 tsv 文件中的 `SuggestedQuestions` 列将保留活动的学习数据。

`SuggestedQuestions` 列是隐式、`autosuggested`和显式 `usersuggested` 反馈信息的 JSON 对象。 此 JSON 对象的一个示例是，`help` 的单个用户提交的问题是：

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

你还可以使用 "下载变更 API" 来查看这些改动，使用 REST 或任何基于语言的 Sdk：
* [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.alterationsextensions.getasync?view=azure-dotnet)


当您重新导入此应用程序时，主动学习将继续收集相关信息并为您的知识库提出建议。



## <a name="best-practices"></a>最佳做法

有关使用主动学习的最佳做法，请参阅[最佳做法](../Concepts/best-practices.md#active-learning)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 GenerateAnswer API 的元数据](metadata-generateanswer-usage.md)
