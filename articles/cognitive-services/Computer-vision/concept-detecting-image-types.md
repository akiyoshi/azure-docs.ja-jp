---
title: 画像の種類の検出 - Computer Vision
titleSuffix: Azure Cognitive Services
description: Computer Vision API を使用した画像種類の検出に関連する概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: ecdbdd60af41ee14070f2f45dfe50875e38ac3b3
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342009"
---
# <a name="detecting-image-types"></a>イメージの種類の検出

Computer Vision は、画像のコンテンツの種類を分析して、画像がクリップ アートかどうか、可能性を評価したスケール、または線画かどうかを示すことができます。

## <a name="detecting-clip-art"></a>クリップ アートの検出

Computer Vision は、次の表に示すように、画像を分析して、画像がクリップ アートである可能性を 0 から 3 のスケールで評価します。

| 値 | 意味 |
|-------|---------|
| 0 | クリップ アートではない |
| 1 | あいまい |
| 2 | 通常のクリップ アート |
| 3 | 良好なクリップ アート |

### <a name="clip-art-detection-examples"></a>クリップ アート検出の例

次の JSON 応答では、サンプル画像がクリップ アートである可能性を評価するときに Computer Vision が返すものを示します。

![Vision Analyze チーズのクリップ アート](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Vision Analyze 家の庭](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>線画の検出

Computer Vision は画像を分析し、画像が線画かどうかを示すブール値を返します。

### <a name="line-drawing-detection-examples"></a>線画の検出例

次の JSON 応答では、サンプル画像が線画かどうかを示すときに Computer Vision が返すものを示します。

![Vision Analyze ライオンの線画](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Vision Analyze 花](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>次の手順

[イメージへのタグ付け](concept-tagging-images.md)および[イメージの分類](concept-categorizing-images.md)に関する概念を確認します。