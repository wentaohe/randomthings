To set up authentication for `pandas_gbq` to query BigQuery, you typically have two common methods: 

1. **Using Application Default Credentials (ADC)**:
   Google Cloud recommends using Application Default Credentials (ADC) to authenticate your applications. ADC looks for service account keys in the following order:
   - The environment variable `GOOGLE_APPLICATION_CREDENTIALS`
   - The default service account that Compute Engine, Google Kubernetes Engine, App Engine, Cloud Functions, and Cloud Run provide for the instance, if available.
   
   Once ADC is set up:
   ```python
   import pandas_gbq
   
   sql = "SELECT * FROM `your_project_id.your_dataset.your_table`"
   df = pandas_gbq.read_gbq(sql, project_id='your_project_id')
   ```

   To set up ADC using a service account JSON key:
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-file.json"
   ```

2. **Using OAuth 2.0**:
   `pandas_gbq` can use user account credentials to authenticate. It will walk you through the OAuth 2.0 flow in the browser to obtain an access token.

   ```python
   import pandas_gbq
   
   sql = "SELECT * FROM `your_project_id.your_dataset.your_table`"
   df = pandas_gbq.read_gbq(sql, project_id='your_project_id', credentials=None, auth_local_webserver=True)
   ```

   The `auth_local_webserver=True` parameter will use a local webserver to obtain the OAuth 2.0 token. If set to `False`, it will provide a URL for you to visit and obtain the token manually.

For **BigQuery Multitenant**:

BigQuery Multitenant refers to the ability to have datasets from multiple Google Cloud projects in a single query. To query across multiple projects, you need to have appropriate permissions on all projects involved. The authentication methods remain the same, but you need to ensure that the credentials used have the necessary permissions on all projects being queried.

Make sure to:
- Have the right permissions set in BigQuery for the datasets/tables you want to query.
- Use fully qualified table and dataset names in the queries (e.g., `project_id.dataset_id.table_id`).

If you are using multiple credentials for different datasets, you might need to manage these credentials separately and perform separate queries for each dataset, then combine the results in Python.
