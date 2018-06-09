---
Description: Load a texture from a texture.
ms.assetid: 126e71e1-a3b2-418b-be35-434a2e9472ca
title: D3DX10LoadTextureFromTexture function
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# D3DX10LoadTextureFromTexture function

Load a texture from a texture.

## Syntax


```C++
HRESULT D3DX10LoadTextureFromTexture(
   ID3D10Resource           *pSrcTexture,
   D3DX10_TEXTURE_LOAD_INFO *pLoadInfo,
   ID3D10Resource           *pDstTexture
);
```



## Parameters

<dl> <dt>

*pSrcTexture* 
</dt> <dd>

Type: **[**ID3D10Resource**](/windows/desktop/api/D3D10/nn-d3d10-id3d10resource)\***

Pointer to the source texture. See [**ID3D10Resource**](/windows/desktop/api/D3D10/nn-d3d10-id3d10resource).

</dd> <dt>

*pLoadInfo* 
</dt> <dd>

Type: **[**D3DX10\_TEXTURE\_LOAD\_INFO**](d3dx10-texture-load-info.md)\***

Pointer to texture loading parameters. See [**D3DX10\_TEXTURE\_LOAD\_INFO**](d3dx10-texture-load-info.md).

</dd> <dt>

*pDstTexture* 
</dt> <dd>

Type: **[**ID3D10Resource**](/windows/desktop/api/D3D10/nn-d3d10-id3d10resource)\***

Pointer to the destination texture. See [**ID3D10Resource Interface**](/windows/desktop/api/D3D10/nn-d3d10-id3d10resource).

</dd> </dl>

## Return value

Type: **[**HRESULT**](https://msdn.microsoft.com/windows/desktop/455d07e9-52c3-4efb-a9dc-2955cbfd38cc)**

The return value is one of the values listed in [Direct3D 10 Return Codes](d3d10-graphics-reference-returnvalues.md).

## Requirements



|                   |                                                                                        |
|-------------------|----------------------------------------------------------------------------------------|
| Header<br/> | <dl> <dt>D3DX10Tex.h</dt> </dl> |



## See also

<dl> <dt>

[Texture Functions in D3DX 10](d3d10-graphics-reference-d3dx10-functions-texturing.md)
</dt> </dl>

 

 



