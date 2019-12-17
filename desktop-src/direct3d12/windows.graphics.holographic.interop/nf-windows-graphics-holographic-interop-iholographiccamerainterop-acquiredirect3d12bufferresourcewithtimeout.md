---
title: IHolographicCameraInterop::AcquireDirect3D12BufferResourceWithTimeout
description: Acquires a Direct3D 12 buffer resource, with an optional timeout.
ms.localizationpriority: low
ms.topic: reference
ms.date: 12/13/2019
---

# IHolographicCameraInterop::AcquireDirect3D12BufferResourceWithTimeout method

> [!NOTE]
> **Some information relates to pre-released product, which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here.**

> [!IMPORTANT]
> The feature described in this topic is implemented in 
Windows 10, version 1903 (10.0; Build 18362), but the `Windows.Graphics.Holographic.Interop.h` header file is available only in the [Windows 10 SDK Insider Preview](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK).

Acquires a Direct3D 12 buffer resource, with an optional timeout.

After committing a resource to a [HolographicFrame](/uwp/api/windows.graphics.holographic.holographicframe) by calling [IHolographicQuadLayerUpdateParametersInterop::CommitDirect3D12Resource](/windows/win32/api/windows.graphics.holographic.interop/nf-windows-graphics-holographic-interop-iholographicquadlayerupdateparametersinterop-commitdirect3d12resource), your application should consider control of that resource to be held by the system. That control can last for a few frames while the frame that the buffer was committed to makes its way through the presentation queue. To determine when the system has relinquished control, call 
[AcquireDirect3D12TextureResource](/windows/win32/api/windows.graphics.holographic.interop/nf-windows-graphics-holographic-interop-iholographiccamerainterop-acquiredirect3d12bufferresource). If the buffer can't be acquired by the time your application is ready to start rendering a new [HolographicFrame](/uwp/api/windows.graphics.holographic.holographicframe), then you should create a new resource, and add it to the buffer queue.

This method accepts an optional timeout value. When this value is specified, the system will wait for that many milliseconds for the buffer to become available. The default behavior is to not wait. When no timeout value is specified, if this method is called and the buffer is not ready to be acquired, the method call will fail with the error code **E_NOTREADY**.

## Syntax

```cpp
HRESULT AcquireDirect3D12BufferResourceWithTimeout(
  ID3D12Resource *pResourceToAcquire,
  ID3D12CommandQueue *pCommandQueue,
  UINT64 duration
);
```

## Parameters

`pResourceToAcquire`
Type: **[ID3D12Resource](/windows/win32/api/d3d12/nn-d3d12-id3d12resource)\***

The Direct3D 12 resource to acquire.

`pCommandQueue`
Type: **[ID3D12CommandQueue](/windows/win32/api/d3d12/nn-d3d12-id3d12commandqueue)\***

The Direct3D 12 command queue to use for transitioning the state of this resource when acquiring it for your application. The resource will be in the **D3D12_RESOURCE_STATE_COMMON** state when it is acquired.

`duration`
Type: **[UINT64](/windows/win32/winprog/windows-data-types)**

If this parameter is set to a non-zero value, the call will wait for that amount of time for the buffer to be acquired. If the timeout period elapses before the buffer can be acquired, the method will fail with the error code **E_TIMEOUT**. This parameter is specified in 100-nanosecond units, similar to the [TimeSpan.Duration](/uwp/api/windows.foundation.timespan.duration) field.

## Returns
**S_OK** if successful, otherwise returns an [HRESULT](/windows/win32/com/structure-of-com-error-codes) error code indicating the reason for failure. Also see [COM Error Codes (UI, Audio, DirectX, Codec)](/windows/win32/com/com-error-codes-10).

## Requirements
| &nbsp; | &nbsp; |
| ---- |:---- |
| **Target Platform** | Windows |
| **Header** | windows.graphics.holographic.interop.h |