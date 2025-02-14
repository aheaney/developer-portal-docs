---
title: ml_gaze_recognition.h

---

# ml_gaze_recognition.h



## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md)** <br></br>Static information about the Gaze Recognition system. Populate with [MLGazeRecognitionGetStaticData()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitiongetstaticdata).  |
| struct | **[MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md)** <br></br>Information about the state of the Gaze Recognition system. This structure must be initialized by calling [MLGazeRecognitionStateInit()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#void-mlgazerecognitionstateinit) before use.  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef struct [MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) | **[MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#struct-mlgazerecognitionstaticdata)** <br></br>Static information about the Gaze Recognition system. Populate with [MLGazeRecognitionGetStaticData()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitiongetstaticdata).  |
| typedef struct [MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) | **[MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#struct-mlgazerecognitionstate)** <br></br>Information about the state of the Gaze Recognition system. This structure must be initialized by calling [MLGazeRecognitionStateInit()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#void-mlgazerecognitionstateinit) before use.  |

## Enums

|                | Name           |
| -------------- | -------------- |
| enum | **[MLGazeRecognitionError](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#enums-mlgazerecognitionerror)** <br></br> { <br></br>[MLGazeRecognitionError_None](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognitionerror-none),<br></br> [MLGazeRecognitionError_Generic](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognitionerror-generic),<br></br> [MLGazeRecognitionError_Ensure32Bits](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognitionerror-ensure32bits) = 0x7FFFFFFF<br></br>}<br></br>A set of possible error codes that the Gaze Recognition system can report.  |
| enum | **[MLGazeRecognitionBehavior](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#enums-mlgazerecognitionbehavior)** <br></br> { <br></br>[MLGazeRecognition_Unknown](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-unknown) = 0,<br></br> [MLGazeRecognition_EyesClosed](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-eyesclosed) = 1,<br></br> [MLGazeRecognition_Blink](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-blink) = 2,<br></br> [MLGazeRecognition_Fixation](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-fixation) = 3,<br></br> [MLGazeRecognition_Pursuit](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-pursuit) = 4,<br></br> [MLGazeRecognition_Saccade](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-saccade) = 5,<br></br> [MLGazeRecognition_BlinkLeft](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-blinkleft) = 6,<br></br> [MLGazeRecognition_BlinkRight](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-blinkright) = 7,<br></br> [MLGazeRecognition_Ensure32Bits](/api-ref/api/Files/ml__gaze__recognition_8h.md#enums-mlgazerecognition-ensure32bits) = 0x7FFFFFFF<br></br>}<br></br>A set of mutually-exclusive behaviors that the Gaze Recognition system can report.  |

## Functions

|                | Name           |
| -------------- | -------------- |
| void | **[MLGazeRecognitionStaticDataInit](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#void-mlgazerecognitionstaticdatainit)**([MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) * inout_state)<br></br>Initialize [MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) with version.  |
| void | **[MLGazeRecognitionStateInit](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#void-mlgazerecognitionstateinit)**([MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) * inout_state)<br></br>Initialize [MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) with version.  |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) | **[MLGazeRecognitionCreate](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitioncreate)**([MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) * out_handle)<br></br>Create Gaze Recognition.  |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) | **[MLGazeRecognitionDestroy](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitiondestroy)**([MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) handle)<br></br>Destroy Gaze Recognition.  |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) | **[MLGazeRecognitionGetStaticData](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitiongetstaticdata)**([MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) handle, [MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) * out_data)<br></br>Get static information about Gaze Recognition.  |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) | **[MLGazeRecognitionGetState](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitiongetstate)**([MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) handle, [MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) * out_state)<br></br>Get information about the user's gaze.  |

## Enums Documentation

### MLGazeRecognitionError {#enums-mlgazerecognitionerror}

| Enumerator | Value | Description |
| ---------- | ----- | ----------- |
| MLGazeRecognitionError_None | | No error, tracking is nominal. |
| MLGazeRecognitionError_Generic | | Gaze Recognition system failed. |
| MLGazeRecognitionError_Ensure32Bits |  0x7FFFFFFF| Ensure enum is represented as 32 bits. |



A set of possible error codes that the Gaze Recognition system can report. 




**API Level:**
  * 20




-----------

### MLGazeRecognitionBehavior {#enums-mlgazerecognitionbehavior}

| Enumerator | Value | Description |
| ---------- | ----- | ----------- |
| MLGazeRecognition_Unknown |  0| Unknown. |
| MLGazeRecognition_EyesClosed |  1| Both eyes closed. |
| MLGazeRecognition_Blink |  2| Blink detected. Both eyes open, close, and open. |
| MLGazeRecognition_Fixation |  3| User is fixating, eye position is stable. |
| MLGazeRecognition_Pursuit |  4| User is pursuing, eye velocity is low but nonzero. |
| MLGazeRecognition_Saccade |  5| User is making a saccade, eye velocity is high. |
| MLGazeRecognition_BlinkLeft |  6| Left eye blink, right eye open. |
| MLGazeRecognition_BlinkRight |  7| Right eye blink, left eye open. |
| MLGazeRecognition_Ensure32Bits |  0x7FFFFFFF| |



A set of mutually-exclusive behaviors that the Gaze Recognition system can report. 




**API Level:**
  * 24




-----------


## Types Documentation

### MLGazeRecognitionStaticData {#struct-mlgazerecognitionstaticdata}

```cpp
typedef struct MLGazeRecognitionStaticData MLGazeRecognitionStaticData;
```

Static information about the Gaze Recognition system. Populate with [MLGazeRecognitionGetStaticData()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitiongetstaticdata). 



[More Info](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md)


**API Level:**
  * 20




-----------

### MLGazeRecognitionState {#struct-mlgazerecognitionstate}

```cpp
typedef struct MLGazeRecognitionState MLGazeRecognitionState;
```

Information about the state of the Gaze Recognition system. This structure must be initialized by calling [MLGazeRecognitionStateInit()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#void-mlgazerecognitionstateinit) before use. 



[More Info](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md)


**API Level:**
  * 20




-----------


## Functions Documentation

### MLGazeRecognitionStaticDataInit {#void-mlgazerecognitionstaticdatainit}

```cpp
static inline void MLGazeRecognitionStaticDataInit(
    MLGazeRecognitionStaticData * inout_state
)
```

Initialize [MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) with version. 

**Parameters**

|  |   |   |
|--|--|--|
| [MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) * |inout_state|Sets up the version for inout_state and nulls pointer for the [MLCoordinateFrameUID](/api-ref/api/Modules/group___perception/struct_m_l_coordinate_frame_u_i_d.md).|
**Required Permissions**:

  * None 





**API Level:**
  * 20




-----------

### MLGazeRecognitionStateInit {#void-mlgazerecognitionstateinit}

```cpp
static inline void MLGazeRecognitionStateInit(
    MLGazeRecognitionState * inout_state
)
```

Initialize [MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) with version. 

**Parameters**

|  |   |   |
|--|--|--|
| [MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) * |inout_state|Sets up the version for inout_state and zeros all the fields.|
**Required Permissions**:

  * None 





**API Level:**
  * 20




-----------

### MLGazeRecognitionCreate {#mlresult-mlgazerecognitioncreate}

```cpp
MLResult MLGazeRecognitionCreate(
    MLHandle * out_handle
)
```

Create Gaze Recognition. 

**Parameters**

|  |   |   |
|--|--|--|
| [MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) * |out_handle|A pointer to an [MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) which will contain a handle to Gaze Recognition. If this operation fails, out_handle will be [ML_INVALID_HANDLE](/api-ref/api/Modules/group___platform/group___platform.md#enums-ml-invalid-handle).|

**Returns**

|  |   |   |
|--|--|--|
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_InvalidParam|The out_handle parameter was not valid (null). |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_Ok|Gaze Recognition was successfully created. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_PerceptionSystemNotStarted|Perception System has not been started. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_PermissionDenied|The application lacks permission. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_UnspecifiedFaiure|Gaze Recognition was not created successfully.|
**Required Permissions**:

  * com.magicleap.permission.EYE_TRACKING (protection level: dangerous) 





**API Level:**
  * 20




-----------

### MLGazeRecognitionDestroy {#mlresult-mlgazerecognitiondestroy}

```cpp
MLResult MLGazeRecognitionDestroy(
    MLHandle handle
)
```

Destroy Gaze Recognition. 

**Parameters**

|  |   |   |
|--|--|--|
| [MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) |handle|A handle to Gaze Recognition created by [MLGazeRecognitionCreate()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitioncreate).|

**Returns**

|  |   |   |
|--|--|--|
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_InvalidParam|The Gaze Recognition handle was not valid. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_Ok|The Gaze Recognition was successfully destroyed. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_PerceptionSystemNotStarted|Perception System has not been started. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_UnspecifiedFailure|The Gaze Recognition was not successfully destroyed.|
**Required Permissions**:

  * None 





**API Level:**
  * 20




-----------

### MLGazeRecognitionGetStaticData {#mlresult-mlgazerecognitiongetstaticdata}

```cpp
MLResult MLGazeRecognitionGetStaticData(
    MLHandle handle,
    MLGazeRecognitionStaticData * out_data
)
```

Get static information about Gaze Recognition. 

**Parameters**

|  |   |   |
|--|--|--|
| [MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) |handle|A handle to Gaze Recognition created by [MLGazeRecognitionCreate()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitioncreate). |
| [MLGazeRecognitionStaticData](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_static_data.md) * |out_data|Target to populate the data about Gaze Recognition.|

**Returns**

|  |   |   |
|--|--|--|
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_InvalidParam|The out_data parameter was not valid (null). |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_Ok|Gaze Recognition static data was successfully received. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_PerceptionSystemNotStarted|Perception System has not been started. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_UnspecifiedFailure|Failed to receive Gaze Recognition static data.|
**Required Permissions**:

  * None 





**API Level:**
  * 20




-----------

### MLGazeRecognitionGetState {#mlresult-mlgazerecognitiongetstate}

```cpp
MLResult MLGazeRecognitionGetState(
    MLHandle handle,
    MLGazeRecognitionState * out_state
)
```

Get information about the user's gaze. 

**Parameters**

|  |   |   |
|--|--|--|
| [MLHandle](/api-ref/api/Modules/group___platform/group___platform.md#uint64-t-mlhandle) |handle|A handle to Gaze Recognition created by [MLGazeRecognitionCreate()](/api-ref/api/Modules/group___gaze_recognition/group___gaze_recognition.md#mlresult-mlgazerecognitioncreate). |
| [MLGazeRecognitionState](/api-ref/api/Modules/group___gaze_recognition/struct_m_l_gaze_recognition_state.md) * |out_state|Information about the gaze.|

**Returns**

|  |   |   |
|--|--|--|
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_InvalidParam|The out_state parameter was not valid (null). |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_Ok|gaze Recognition state was successfully received. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_PerceptionSystemNotStarted|Perception System has not been started. |
| [MLResult](/api-ref/api/Modules/group___platform/group___platform.md#int32-t-mlresult) |MLResult_UnspecifiedFailure|Failed to receive gaze Recognition state data.|
**Required Permissions**:

  * None 





**API Level:**
  * 20




-----------



## Source code

```cpp
// %BANNER_BEGIN%
// ---------------------------------------------------------------------
// %COPYRIGHT_BEGIN%
// Copyright (c) 2021 Magic Leap, Inc. All Rights Reserved.
// Use of this file is governed by the Software License Agreement,
// located here: https://www.magicleap.com/software-license-agreement-ml2
// Terms and conditions applicable to third-party materials accompanying
// this distribution may also be found in the top-level NOTICE file
// appearing herein.
// %COPYRIGHT_END%
// ---------------------------------------------------------------------
// %BANNER_END%

#pragma once

#include "ml_api.h"
#include "ml_types.h"

ML_EXTERN_C_BEGIN

typedef enum MLGazeRecognitionError {
  MLGazeRecognitionError_None,
  MLGazeRecognitionError_Generic,
  MLGazeRecognitionError_Ensure32Bits = 0x7FFFFFFF
} MLGazeRecognitionError;

typedef enum MLGazeRecognitionBehavior {
  MLGazeRecognition_Unknown = 0,
  MLGazeRecognition_EyesClosed = 1,
  MLGazeRecognition_Blink = 2,
  MLGazeRecognition_Fixation = 3,
  MLGazeRecognition_Pursuit = 4,
  MLGazeRecognition_Saccade = 5,
  MLGazeRecognition_BlinkLeft = 6,
  MLGazeRecognition_BlinkRight = 7,
  MLGazeRecognition_Ensure32Bits = 0x7FFFFFFF
} MLGazeRecognitionBehavior;

typedef struct MLGazeRecognitionStaticData {
  uint32_t version;
  MLCoordinateFrameUID vergence;
} MLGazeRecognitionStaticData;

ML_STATIC_INLINE void MLGazeRecognitionStaticDataInit(MLGazeRecognitionStaticData *inout_state) {
  if (inout_state) {
    inout_state->version = 1u;
  }
}

typedef struct MLGazeRecognitionState {
  uint32_t version;
  MLTime timestamp;
  MLGazeRecognitionError error;
  MLGazeRecognitionBehavior behavior;
  MLVec2f eye_left;
  MLVec2f eye_right;
  float onset_s;
  float duration_s;
  float velocity_degps;
  float amplitude_deg;
  float direction_radial;
} MLGazeRecognitionState;

ML_STATIC_INLINE void MLGazeRecognitionStateInit(MLGazeRecognitionState *inout_state) {
  if (inout_state) {
    inout_state->version = 1u;
    inout_state->timestamp = 0;
    inout_state->error = MLGazeRecognitionError_None;
    inout_state->behavior = MLGazeRecognition_Unknown;
    inout_state->eye_left.x = 0.0f;
    inout_state->eye_left.y = 0.0f;
    inout_state->eye_right.x = 0.0f;
    inout_state->eye_right.y = 0.0f;
    inout_state->onset_s = 0.0f;
    inout_state->duration_s = 0.0f;
    inout_state->velocity_degps = 0.0f;
    inout_state->amplitude_deg = 0.0f;
    inout_state->direction_radial = 0.0f;
  }
}

ML_API MLResult ML_CALL MLGazeRecognitionCreate(MLHandle *out_handle);

ML_API MLResult ML_CALL MLGazeRecognitionDestroy(MLHandle handle);

ML_API MLResult ML_CALL MLGazeRecognitionGetStaticData(MLHandle handle,
                                                   MLGazeRecognitionStaticData *out_data);

ML_API MLResult ML_CALL MLGazeRecognitionGetState(MLHandle handle,
                                              MLGazeRecognitionState *out_state);

ML_EXTERN_C_END
```



