# Using Firebase Admin SDK in Google Cloud Run

## Problem Overview

Deploying applications with integrated Firebase services to Google Cloud Run often presents challenges. While everything may function correctly in a local development environment, issues can arise when migrating to the cloud.

Typical scenarios include:

1. Developers download a JSON file containing Firebase service account credentials for use in their local environment, which works well locally.
2. However, complications arise when deploying to Google Cloud Run.

Many developers consider storing the JSON file containing Firebase service account credentials in GitHub Actions secrets. While this approach seems straightforward, it can lead to issues due to newline characters (\n) and other special characters in the JSON, causing environment variable setting failures. This article presents a more reliable and secure solution.

## Solution

Secret Manager

### Step 1: Implement GCP Secret Manager

1. Access the GCP Console and navigate to Secret Manager.
2. Create a new secret with a descriptive name, such as `firebase-service-account-key`.
3. Upload the contents of the `serviceaccountkey.json` file as the value for this secret.

### Step 2: Update Deployment Configuration

Next, update the deployment configuration. Modify the `gcloud run deploy` command in your `deploy.yml` (or equivalent configuration file) to reference the newly created secret.

```bash
    - name: Deploy to Cloud Run
      run: |-
        gcloud run deploy ${{ env.SERVICE_NAME }} \
        ...
          --set-secrets="FIREBASE_SERVICE_ACCOUNT_KEY=projects/<project-id>/secrets/firebase-service-account-key:latest" \
        ...
```

### Step 3: Access the Secret in Your Application

In your application code, you can now access the secret as follows:

```python
def get_secret():
    client = secretmanager.SecretManagerServiceClient()
    name = f"projects/{os.environ['PROJECT_ID']}/secrets/firebase-service-account-key/versions/latest"
    response = client.access_secret_version(request={"name": name})
    return json.loads(response.payload.data.decode("UTF-8"))

def init_firebase():
    if not firebase_admin._apps:
        try:
            service_account_info = get_secret()
            cred = credentials.Certificate(service_account_info)
            firebase_admin.initialize_app(cred, {
                'storageBucket': f"{service_account_info.get('project_id')}.appspot.com"
            })
        except Exception as e:
            print(f"Error initializing Firebase: {e}")
            raise
```

This approach avoids issues with special characters in environment variables by directly accessing the secret from Google Cloud Secret Manager.

## Conclusion

By leveraging GCP Secret Manager, we can securely store and access Firebase service account credentials in Google Cloud Run, avoiding potential issues with environment variables and providing a more robust solution for production deployments.