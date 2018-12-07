# Google Cloud Platform Components

[Google Cloud Platform (GCP)](https://cloud.google.com/) is a fast-growing array of cloud services,
accompanied by identity and access management (IAM), quotas and pricing, and deployment and lifecycle management utilities.
[Google Cloud Functions](https://cloud.google.com/functions/) under
[GCP Compute](https://cloud.google.com/docs/overview/cloud-platform-services#computing-hosting) and
[Firebase Cloud Functions](https://firebase.google.com/docs/functions/) under
[Firebase](https://firebase.google.com/)
are GCP's main approaches to serverless computing.
However, GCP services are easily accessible on any computing platform, including serverless functions deployed on AWS et al.


## Authorization

GCP entities are grouped by
[Organizations and Projects](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy).
In order to access GCP entities within your non-GCP Sigma app, you need to provide
[service account credentials](https://cloud.google.com/iam/docs/understanding-service-accounts)
for the corresponding GCP project.
The credentials are typically downloaded in the form of a JSON file when you
[create a new service account for your project](https://developers.google.com/identity/protocols/OAuth2ServiceAccount#creatinganaccount).

The JSON file typically takes the format:

```
{
  "type": "service_account",
  "project_id": "<GCP project ID>",
  "private_key_id": "<alphanumeric ID of the private key>",
  "private_key": "-----BEGIN PRIVATE KEY-----\n<private key content>\n-----END PRIVATE KEY-----\n",
  "client_email": "<project ID>@<project ID>.iam.gserviceaccount.com",
  "client_id": "<numeric client ID>",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://accounts.google.com/o/oauth2/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/<encoded client_email>"
}
```

You can follow one of the approaches described under the [**Authorization** page](authorization.md)
in order to provide the service account credentials (key) to Sigma.

After authorization, you would be able to access the list of natively supported GCP services,
under the **GCP Resources** child tab of the **Resources** pane.


## Resource modes

### Triggers and Operations

If your project has GCP set as the **Base Platform** (i.e. the project is configured to be deployed on GCP),
you can use certain GCP services (*Cloud Storage*, *Cloud Pub/Sub* etc.) as *event sources*
to [trigger](../../concepts/triggers.md) your Cloud Functions.

Regardless of the platform, you can perform [operations](../../concepts/operations.md)
against GCP services and entities from within any project, even a non-GCP (e.g. AWS) one.

### New vs existing resources
 
In GCP-based projects, you can define [*new resources*](../../concepts/resources.md#new-resources-vs.-existing-resources)
(topics, buckets etc.) under supported GCP services.
These will be bound to your Sigma project, and managed (created, updated and deleted)
along with the project's overall deployment, inside the designated GCP project.

Alternatively, in all (GCP as well as non-GCP) projects, you can integrate with *already existing* GCP entities
that are already available in your GCP project.
These will not be added to the deployment configurations of your Sigma project, so would need to be managed externally.


## Where to go from here

* [Getting Started with Sigma IDE on Google Cloud Platform](./getting-started.md)