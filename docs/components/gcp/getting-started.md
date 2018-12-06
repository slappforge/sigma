# Getting Started with Sigma IDE on Google Cloud Platform


## Prerequisites

* a [**Google Cloud Platform subscription**](https://cloud.google.com/getting-started/);
if you already have a Google account, you can directly [sign up](https://console.cloud.google.com/freetrial).
* a [**GitHub account**](https://github.com/); free to sign up if you don't have one already
* a fairly new web browser; Sigma has been tested on **Google Chrome 59+**, **Mozilla Firefox 58+**,
**Microsoft Edge 41+** and **Apple Safari 10.1.2+**


## Create a service account key.

**NOTE:** This step can be omitted if you already have a saved
[service account key file](https://developers.google.com/identity/protocols/OAuth2ServiceAccount#creatinganaccount)
for your GCP project.

1. Open the [Cloud Platform dashboard](https://console.cloud.google.com/home/dashboard).

2. If you do not have any GCP projects, create a new project. You can simply follow
[Google's official guide](https://cloud.google.com/resource-manager/docs/creating-managing-projects#creating_a_project).

3. Select your GCP project from the header drop-down.

   ![Cloud Platform dashboard header with project selection drop-down](images/gcp-auth/01-gcp-dashboard-header-with-project-selection-dropdown.png)

4. Open the [GCP Service Accounts page](https://console.cloud.google.com/iam-admin/serviceaccounts).
You can use the **IAM & admin -> Service accounts** menu item from within any of the dashboard pages.

   ![Service Accounts: menu navigation](images/gcp-auth/02-service-accounts-menu-item.png)

   Here you will see all service accounts of your project.
   Even if you have not created any service accounts manually, you will probably have the Google-provisioned
   [default service accounts](https://cloud.google.com/compute/docs/access/service-accounts#user-managed_service_accounts)
   for _App Engine_ or _Compute Engine_. These accounts are generally sufficient for Sigma's authorizations.

   Alternatively, you can choose or create a different service account that you would like to use to authorize Sigma.
   The account should possess permissions to:

   * Create and write to a new [Cloud Storage bucket](https://cloud.google.com/storage/docs/key-terms#buckets)
   * Run builds on [CloudBuild (Container Builder) service](https://cloud.google.com/cloud-build/)
   * Manage deployments on the [Deployment Manager service](https://cloud.google.com/deployment-manager/)
   * Access other GCP services that you intend to use in your Sigma project

   **NOTE:** If you are using GCP as a secondary platform (e.g. integrating GCP into an AWS-based project)
   you would only need the last permission.

   Refer the [Cloud Platform documentation](https://developers.google.com/identity/protocols/OAuth2ServiceAccount#creatinganaccount)
   if you want to create a new service account for Sigma.

5. Click the drop-down at the end of the service account entry, and choose **Create key**.

   ![Service Account actions drop-down](images/gcp-auth/03-create-new-key-menu-option.png)

6. Ensure that **JSON** is selected as the **Key type**, and click **Create**.

   ![Create Private Key dialog](images/gcp-auth/04-create-new-key-popup-dialog.png)

7. The private key file will be downloaded into your browser; keep it private and safe.

   ![Private Key Saved confirmation dialog](images/gcp-auth/05-new-key-saved.png)


## Enable the required Google Cloud APIs for your GCP project.

If you hope to *deploy* your project on GCP (as opposed to *accessing* GCP APIs while running on a different platform like AWS),
you would need to enable the following APIs on your GCP project (which are not enabled by default):

* Cloud Functions API
* Cloud Build API
* Google Cloud Deployment Manager V2 API

1. Navigate to the [API Library page](https://console.cloud.google.com/apis/library).

   ![API Library: navigation menu item](images/gcp-enable-apis/01-api-library-menu-item.png)

2. Click **Enable APIs and Services** on the page header.

   ![API Library: **Enable APIs and Services** button](images/gcp-enable-apis/02-enable-apis-button.png)

3. Type the name of each API in the search bar.

   ![API Library: Searching for CloudBuild API](images/gcp-enable-apis/03-cloudbuild-api-search.png)

4. Click on the search result API tile, and click **Enable** on the API description page.

   ![API Library: CloudBuild API page with Enable button](images/gcp-enable-apis/04-cloudbuild-api-page-with-enable-button.png)


**NOTE:**

* It may take 2-3 minutes for an API to get enabled.
* If you wish to use any other APIs that are not listed in the
[default enabled APIs list](),
you will need to explicitly enable them as well - *regardless of the deployment platform*.


# Sign up for Sigma.

Create new Sigma account (if you haven't already):

1. Go to the [Sigma sign-in page](https://sigma.slappforge.com/#/signin).

   ![Sigma: sign-in page](../../images/signup/01-sigma-sign-in-page.png)

2. Click the **Create an account** link against the **New to Sigma?** message.

   ![Sigma: **Create an account** link](../../images/signup/02-create-an-account-link.png)

3. Fill in your first and last names, email, and phone number.

   ![Sigma: Sign up form](../../images/signup/03-sigma-sign-up-form.png)

4. Take note of your *username*: Sigma will automatically generate one, using your first and last names.
You are free to change this to your own preference, as long as someone else has not used it already.

   ![Sigma sign up: auto-generated username](../../images/signup/04-sigma-sign-up-auto-generated-username.png)

5. Choose a password. Passwords in Sigma need to be at least 8 letters long, and have at least one from each of:
uppercase, lowercase, numeric and symbolic (@, $, # etc.) character categories.
(For example, `Password#123` minimal (but **very** insecure) example.)
You can press and hold the **show password** button (eye icon) to confirm what you have typed.

   ![Sigma sign up: preview password](../../images/signup/05-sigma-sign-up-preview-password.png)

6. When all looks good, click **Sign Up**.
If Sigma succeeds in creating your account, you will be taken to a confirmation page (**Insert Confirmation Code**).
An email will be sent to the address you indicated (from `noreply@slappforge.com`), with a verification code.

   ![Sigma sign up: confirmation code pane](../../images/signup/06-sigma-sign-up-confirmation-code-pane.png)

7. Check your email for the verification email (including promotions, spam, trash etc. in case it does not show up in the inbox).
Once you find it, copy the 6-digit confirmation code in the email, paste it into the confirmation page, and click **Confirm**.

8. If all goes well, you will see the sign-in form with a **Confirmation successful!** message.
Now you can log in using your username (that you took note previously, during sign-up) and password.

   ![Sigma sign up: confirmation successful](../../images/signup/07-sigma-sign-up-confirmation-successful.png)


# Grant GCP access to Sigma.

1. After sign-in, you'll be in the **Cloud Credentials** page of Sigma.

   ![Sigma: Cloud Credentials page](../../images/cloud-credentials/01-cloud-credentials-page.png)

2. Switch to **GCP** tab, and click the **Authorize** button.

   ![Sigma: GCP Credentials tab](../../images/cloud-credentials/02-gcp-credentials-tab-not-authorized.png)

3. Copy the full content of the service account key file that you downloaded earlier,
and paste it into the text field in the **GCP Service Account Key** pop-up dialog.

   ![GCP Service Account Key dialog with a pasted key](../../images/cloud-credentials/03-service-account-key-dialog.png)

4. Click **Save**. The tab view will change with the name of the GCP project of the service account,
confirming that your service account key has been successfully interpreted.

   ![GCP Credentials tab updated with the project name](../../images/cloud-credentials/04-gcp-credentails-tab-authorized.png)

   If Sigma fails to validate your key, or is unable to access your GCP project using the key,
   an appropriate error will be displayed; if this happens, please revise the pasted key and its permissions, and try again.

   ![GCP Service Account Key: validation failure message](../../images/cloud-credentials/05-gcp-service-account-validation-failed.png)

5. Optionally, switch to the AWS tab and provide an [AWS key pair](../../getting_started.md#aws-credentials)
if you wish to also deploy on, or develop against, AWS.

   ![AWS Credentials tab with a key-pair configured](../../images/cloud-credentials/06-aws-credentials-tab-with-key-pair.png)

6. Sigma can save your GCP (and AWS) credentials in its [secure AWS Cognito data store](https://aws.amazon.com/cognito/faqs/#signup).
This is solely for your convenience; the content will be encrypted with your own password - also managed by Cognito -
or a PIN of your choice, in case you used a social media log-in.

   The IDE will automatically load and decrypt the credentials during later log-ins using the password or PIN that you enter,
   so that you would not need to provide them all over again.

   If you prefer not to save the credentials in this manner, turn off the **Save to Secure Storage** toggle.

   ![Credentials: **Save to Secure Storage** option](../../images/cloud-credentials/07-save-credentials-to-secure-storage-option.png)

7. Click **Load Editor**.