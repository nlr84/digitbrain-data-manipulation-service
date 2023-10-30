
---
DataManipulation Service API Documentation
---
## Table of Contents

1. [Introduction](#introduction)
2. [Initial Configuration](#initial-configuration)
3. [Endpoints](#endpoints)
   - 3.1 [Health Check](#health-check)
   - 3.2[Unit Conversion](#unit-conversion)
   - 3.3 [File Conversion](#file-conversion)
   - 3.4 [Image Manipulation](#image-manipulation)
   - 3.5 [Video Manipulation](#video-manipulation)
4. [Annotations and Considerations](#annotationsandConsiderations)

---

## 1 - Introduction

The DataManipulation service is an API designed to handle a variety of tasks related to data conversion and manipulation. This document describes how to use each of the available endpoints.

## 2 - Initial Configuration

**Base URL**: `http://<your-server>:5000/`

**Supported conversions and manipulation Types**: 

- Files in base64: JSON, CSV, XLSX (JSON supports raw JSON)
- Video and images: base64 for images and videos.

## 3 - Endpoints

### 3.1 - Health Check

- **Endpoint**: `/api/v2/healthcheck`
- **Method**: `GET`
- **Description**: Checks if the service is operational.
- **Parameters**: None
- **Successful Response**: `OK`
- **Success Code**: `200`

---

### 3.2 - Unit Conversion

- **Endpoint**: `/api/v2/unit_conversion`
- **Method**: `POST`
- **Description**: Converts a quantity between two units.

**Payload**: 

- **from**: (Required) The unit being converted from.
- **to**: (Required) The unit being converted to.
- **value**: (Required) The value to convert.

**Payload Example**:

```json
{
    "from": "meters",
    "to": "inches",
    "value": 1
}
```

**Successful Response**: JSON object with the converted amount.

```json
{
    "unit": "inches",
    "value": 39.3701
}
```

---

### 3.3 - File Conversion

- **Endpoint**: `/api/v2/file_conversion`
- **Method**: `POST`
- **Description**: Converts files between different formats.

**Payload**: 

- **conversion_type**: (Required) Type of conversion, can be `"json_to_csv"`, `"csv_to_json"`, `"csv_to_xlsx"`, `"xlsx_to_csv"`.
- **file**: (Required) File data in base64 or in raw JSON format.

**Payload Examples**:

For JSON to CSV:
```json
{
    "conversion_type": "json_to_csv",
    "file": {
        "name": "John",
        "age": 30
    }
}
```

For CSV to JSON:
```json
{
    "conversion_type": "csv_to_json",
    "file": "Base64-Encoded-CSV"
}
```

**Responses**: 

- `200 OK` with converted data in base64 or in JSON format.
- `400 Bad Request` if the conversion type is incorrect or data is missing.
- `500 Internal Server Error` for general errors.

---

### 3.4 - Image Manipulation

- **Endpoint**: `/api/v2/manipulate_image`
- **Method**: `POST`
- **Description**: Applies effects to an image.

**Payload**: 

- **image**: (Required) Base64 encoded image.
- **resize**: (Optional) Array `[width, height]` to resize the image.
- **color**: (Optional) Color transformations, `"grayscale"`, `"negative"`.
- **rotation**: (Optional) Angle for rotation.

**Payload Example**:

```json
{
    "image": "<Base64-Encoded-Image>",
    "resize": [800, 600],
    "color": "grayscale",
    "rotation": 90
}
```

---

### 3.5 - Video Manipulation

- **Endpoint**: `/api/v2/manipulate_video`
- **Method**: `POST`
- **Description**: Manipulates a video based on provided parameters.

**Payload**: 

- **video**: (Required) Base64 encoded video.
- **color**: (Optional) `"grayscale"`.
- **rotation**: (Optional) Rotation angle.
- **resize**: (Optional) Array `[width, height]` for resizing.
- **split**: (Optional) Boolean to split the video.
- **cut_scene**: (Optional) Object with `start` and `end` time in seconds.

**Payload Example**:

```json
{
    "video": "<Base64-Encoded-Video>",
    "color": "grayscale",
    "rotation": 90,
    "resize": [1280, 720],
    "split": true,
    "cut_scene": {
        "start": 2,
        "end": 5
    }
}
```

---

### Response

- `200 OK`: With manipulated video, if `split` is set to `false`.
- `200 OK`: With an array of split video segments, if `split` is set to `true`.
- `400` Bad Request: The server could not understand the request.
- `500` Internal Server Error: An error occurred on the server.

---

# 4 - Annotations and Considerations

## 4.1 - File Format Conversion

File conversions may lead to a loss of information if the structure is not consistent within the file itself. More complex conversions could result in losing columns.

## 4.2 - Video manipulation

Video manipulation can be a slow process and should be considered in critical systems or those requiring high responsiveness.

In general terms, a 40-megabyte video may take around 2 to 6 seconds on average, depending on the modifications requested. The most computationally intensive task occurs when generating frames using the 'split' function.

###  4.2.1 - Internal Processing Order

It's important to note that video manipulation has an order to optimize processing times and workloads. The internal treatment order is as follows:

1. Apply scene cutting
2. Resize
3. Rotate
4. Apply color effects
5. Split into frames.

## 4.3 - Unit Converter

You should consult the table of supported units and their syntax, as contained in the [Supported units](Supported_units.md)

---
