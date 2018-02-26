# Frequently asked Questions

:question: What kind of browser should I have to get the optimum use of Sigma?

:bulb: Sigma is compatible with all modern popular browsers such as Google Chrome, Mozilla Firefox, Apple Safari and Microsoft Edge. If you face any difficulty in one of these or any other browser, please let us know.

:question: Sigma asks for an access key for my AWS account. What is the purpose of this key and which permissions does it require?

:bulb: Sigma uses this key to access your AWS account for a wide range of activities, such as listing your existing resources on the UI, building your project and also for deploying your project. In the current version of Sigma, we expect this key to have full AWS permissions, but in future versions we'll provide you a feature to fine tune them according to the scope of your project.

:question: Where does Sigma keep this AWS key and how secure is it?

:bulb: When you are providing the AWS key, you have 2 options. The first option is that you can choose **not to persist** the key. In that case, your key will only be stored in browser session storage and will be discarded if you logout of sigma or close the browser. The drawback of this option is you are required to provide the AWS key at each login.

:bulb: If you chose to persist the key with Sigma, it will be encrypted with your login password (or the encryption password in case you are using social login) and stored in AWS Cognito. So only you can retrieve and decrypt those values.
