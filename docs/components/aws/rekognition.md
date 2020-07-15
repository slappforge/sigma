---
description: Analyse images using highly scalable, deep learning technology, with AWS Rekognition on Sigma IDE
---

# ![](images/rekognition/rekognition_icon.svg) AWS Rekognition

Amazon Rekognition makes it easy to add image and video analysis to applications with highly scalable, deep learning 
technology that requires no machine learning expertise to use. With Amazon Rekognition, you can identify objects, people, 
text, scenes, and activities in images and videos, as well as detect any inappropriate content. Amazon Rekognition also 
provides highly accurate facial analysis and facial search capabilities that you can use to detect, analyze, and compare 
faces for a wide variety of user verification, people counting, and public safety use cases.

## Rekognition resource

Sigma IDE currently supports the following image based analytic operations via AWS Rekognition resource component.
* Compare Faces
* Detect Faces
* Detect Labels
* Detect Text
* Recognize Celebrities

### Rekognition resource operations

To use Rekognition operations from a Lambda function, first drag a Rekognition resource from the resources panel and 
drop on the required line of the lambda code editor. Then on the configuration panel, select the desired **Operation**.

#### Compare Faces

**Compare Faces** operation can be used to compare a face in a **source input image** with each of the 100 largest 
faces detected in a **target input image**. Following are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
(_Source Image_) Image Source | :white_check_mark: | :x: | Whether the source image is provided as **a reference to an S3 object** or as **base64-encoded image bytes**
(_Source Image_) Bucket Name | :white_check_mark: | :white_check_mark: | Name of the bucket containing the source image<br/>_(Only applicable if the **Image Source** is **S3**)_
(_Source Image_) File Name | :white_check_mark: | :white_check_mark: | S3 object key of the source image<br/>_(Only applicable if the **Image Source** is **S3**)_
(_Source Image_) File Version | :x: | :white_check_mark: | If versioning is enabled, S3 object version of the source image<br/>_(Only applicable if the **Image Source** is **S3**)_
(_Source Image_) Content | :white_check_mark: | :white_check_mark: | Blob of source image bytes up to 5 MBs<br/>_(Only applicable if the **Image Source** is **Bytes**)_
(_Target Image_) Image Source | :white_check_mark: | :x: | Whether the target image is provided as **a reference to an S3 object** or as **base64-encoded image bytes**
(_Target Image_) Bucket Name | :white_check_mark: | :white_check_mark: | Name of the bucket containing the target image<br/>_(Only applicable if the **Image Source** is **S3**)_
(_Target Image_) File Name | :white_check_mark: | :white_check_mark: | S3 object key of the target image<br/>_(Only applicable if the **Image Source** is **S3**)_
(_Target Image_) File Version | :x: | :white_check_mark: | If versioning is enabled, S3 object version of the target image<br/>_(Only applicable if the **Image Source** is **S3**)_
(_Target Image_) Content | :white_check_mark: | :white_check_mark: | Blob of target image bytes up to 5 MBs<br/>_(Only applicable if the **Image Source** is **Bytes**)_
Similarity Threshold | :x: | :white_check_mark: | The minimum level of confidence in the face matches that a match must meet to be included in the result

#### Detect Faces

**Detect Faces** operation can be used to detect the 100 largest faces in a provided image. Following are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Image Source | :white_check_mark: | :x: | Whether the image is provided as **a reference to an S3 object** or as **base64-encoded image bytes**
Bucket Name | :white_check_mark: | :white_check_mark: | Name of the bucket containing the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Name | :white_check_mark: | :white_check_mark: | S3 object key of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Version | :x: | :white_check_mark: | If versioning is enabled, S3 object version of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
Content | :white_check_mark: | :white_check_mark: | Blob of image bytes up to 5 MBs<br/>_(Only applicable if the **Image Source** is **Bytes**)_
Facial Attributes | :x: | :x: | Which facial attributes to be returned in the result.<br/><br/>If a value is not selected or **DEFAULT** is selected, following subset of facial attributes will be returned.<br/>`BoundingBox`, `Confidence`, `Pose`, `Quality`, and `Landmarks`<br/><br/>If **ALL** is selected, all facial attributes will be returned.

#### Detect Labels

**Detect Labels** operation can be used to detect instances of real-world entities within an image (JPEG or PNG) provided 
as input. This includes objects like flower, tree, and table; events like wedding, graduation, and birthday party; and 
concepts like landscape, evening, and nature. Following are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Image Source | :white_check_mark: | :x: | Whether the image is provided as **a reference to an S3 object** or as **base64-encoded image bytes**
Bucket Name | :white_check_mark: | :white_check_mark: | Name of the bucket containing the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Name | :white_check_mark: | :white_check_mark: | S3 object key of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Version | :x: | :white_check_mark: | If versioning is enabled, S3 object version of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
Content | :white_check_mark: | :white_check_mark: | Blob of image bytes up to 5 MBs<br/>_(Only applicable if the **Image Source** is **Bytes**)_
Max Labels | :x: | :white_check_mark: | Maximum number of labels to be return in the result. The specified number of highest confidence labels will be returned.
Min Confidence | :x: | :white_check_mark: | The minimum level of confidence for the labels to be included in the result

#### Detect Text

**Detect Text** operation can be used to detect text in the input image and to convert it into machine-readable text. 
Following are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Image Source | :white_check_mark: | :x: | Whether the image is provided as **a reference to an S3 object** or as **base64-encoded image bytes**
Bucket Name | :white_check_mark: | :white_check_mark: | Name of the bucket containing the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Name | :white_check_mark: | :white_check_mark: | S3 object key of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Version | :x: | :white_check_mark: | If versioning is enabled, S3 object version of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
Content | :white_check_mark: | :white_check_mark: | Blob of image bytes up to 5 MBs<br/>_(Only applicable if the **Image Source** is **Bytes**)_

#### Recognize Celebrities

**Recognize Celebrities** operation can be used to detect and recognize celebrities in the input image. 
Following are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Image Source | :white_check_mark: | :x: | Whether the image is provided as **a reference to an S3 object** or as **base64-encoded image bytes**
Bucket Name | :white_check_mark: | :white_check_mark: | Name of the bucket containing the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Name | :white_check_mark: | :white_check_mark: | S3 object key of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
File Version | :x: | :white_check_mark: | If versioning is enabled, S3 object version of the image<br/>_(Only applicable if the **Image Source** is **S3**)_
Content | :white_check_mark: | :white_check_mark: | Blob of image bytes up to 5 MBs<br/>_(Only applicable if the **Image Source** is **Bytes**)_


