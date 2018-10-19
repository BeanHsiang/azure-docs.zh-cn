---
title: 识别印刷文本和手写文本 - 计算机视觉
titleSuffix: Azure Cognitive Services
description: 与使用计算机视觉 API 识别图像中的印刷文本和手写文本相关的概念。
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 49cba0e9b6958beb07b6f074e6dc748679514525
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985305"
---
# <a name="recognizing-printed-and-handwritten-text"></a>识别打印文本和手写文本

计算机视觉可以在图像中检测并提取印刷文本或手写文本，这些图像包含各种具有不同表面和背景的对象（例如收据、海报、名片、信函、白板）。

文本识别省时省力。 拍摄文本图像比转录文本效率高。 文本识别可以将笔记数字化。 通过数字化可以快速方便地进行搜索。 它还减少了纸张乱堆的情况。

## <a name="text-recognition-requirements"></a>文本识别要求

计算机视觉可以识别符合以下要求的图像中的印刷和手写文本：

- 图像必须以 JPEG、PNG 或 BMP 格式显示
- 图像的文件大小必须不到 4 兆字节 (MB)
- 图像的尺寸必须介于 40 x 40 和 3200 x 3200 像素之间

> [!NOTE]
> 此技术当前处于预览状态，且仅适用于英语文本。

## <a name="next-steps"></a>后续步骤

了解关于[使用 OCR 提取文本](concept-extracting-text-ocr.md)的概念。