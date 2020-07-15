---
description: Easily translate text content between multiple languages, with AWS Translate on Sigma IDE
---

# ![](images/translate/translate_icon.svg) AWS Translate

Amazon Translate is a neural machine translation service that delivers fast, high-quality, and affordable language 
translation. Amazon Translate can be utilized to localize content - such as websites and applications - for international 
users, and to easily translate large volumes of text efficiently.

## Translate resource

Sigma IDE currently supports the following text translation related operations via AWS Translate resource component.
* Translate Text
* List Terminologies
* Get Terminology
* Delete Terminology
* Import Terminology

### Translate resource operations

To use Translate operations from a Lambda function, first drag a Translate resource from the resources panel and 
drop on the required line of the lambda code editor. Then on the configuration panel, select the desired **Operation**.

#### Translate Text

**Translate Text** operation can be used to translate a text from the source language to the target language. Following 
are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Text | :white_check_mark: | :white_check_mark: | The text to translate which can be a maximum of 5,000 bytes long
Source Language | :white_check_mark: | :white_check_mark: | The language of the input text, which can be either selected from a pre-defined set of languages, or a custom language code can be provided.<br/><br/> In case the language is selected as **Auto**, Translate will determine the source language using [Amazon Comprehend](https://docs.aws.amazon.com/comprehend/latest/dg/comprehend-general.html).
Target Language | :white_check_mark: | :white_check_mark: | The language of the translated text, which can be either selected from a pre-defined set of languages, or a custom language code can be provided.
Terminology Names | :x: | :x: | The list of the terminologies to be used for the translation

#### List Terminologies

**List Terminologies** operation can be used to list the custom terminologies associated with your AWS account. Following 
are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Max Results | :white_check_mark: | :white_check_mark: | The maximum number of custom terminologies to be returned
Next Token | :x: | :white_check_mark: | If the previous result of the request was truncated, the token to fetch the next group of custom terminologies

#### Get Terminology

**Get Terminology** operation can be used to retrieve a custom terminology associated with your AWS account. Following 
are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Terminology Name | :white_check_mark: | :white_check_mark: | The name of the custom terminology to be retrieved
Data Format | :white_check_mark: | :x: | The data format of the custom terminology being retrieved, either **CSV** or **TMX**

#### Delete Terminology

**Delete Terminology** operation can be used to remove a custom terminology from your AWS account. Following 
are the fields related to this operation.

Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Terminology Name | :white_check_mark: | :white_check_mark: | The name of the custom terminology to be removed

#### Import Terminology
**Import Terminology** operation can be used to create or update a custom terminology, depending on whether or not one 
already exists for the given terminology name. Importing a terminology with the same name as an existing one will merge 
the terminologies based on the chosen **Merge Strategy**. Following are the fields related to this operation.
                                                          
Field              | Required            | Supports Variables  | Description
---                | :---:               | :---:               | ---
Merge Strategy | :white_check_mark: | :x: | The merge strategy of the custom terminology being imported. Currently, only the **OVERWRITE** merge strategy is supported.
Terminology Name  | :white_check_mark: | :white_check_mark: | The name of the custom terminology being imported
Description  | :x: | :white_check_mark: | The description of the custom terminology being imported
Terminology Data | :white_check_mark: | :white_check_mark: | The content of the file containing the custom terminology data
Terminology Data Format | :white_check_mark: | :x: | The data format of the custom terminology. Either **CSV** or **TMX**
Encryption Key | :white_check_mark: | :x: | Whether to associate an encryption key with the custom terminology
Encryption Key Type | :white_check_mark: | :x: | The type of encryption key to encrypt the custom terminology<br/><br/>_(Only applicable if the **Encryption Key** is enabled)_
Encryption Key ID | :white_check_mark: | :white_check_mark: | The ARN of the encryption key to encrypt the custom terminology<br/><br/>_(Only applicable if the **Encryption Key** is enabled)_

