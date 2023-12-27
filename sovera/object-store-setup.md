# Object Store Setup



To configure the application to use object storage instead of local storage, you need to modify the environment variables in the `.env` file or the `docker-compose.yaml` file.

In the `.env` file, you need to set the `STORAGE_TYPE` to `s3` and provide the necessary S3 configuration details. Here is an example:

``` python
STORAGE_TYPE=s3
S3_ENDPOINT=https://your-bucket-name.storage.s3.clooudflare.com
S3_BUCKET_NAME=your-bucket-name
S3_ACCESS_KEY=your-access-key
S3_SECRET_KEY=your-secret-key
S3_REGION=your-region
```

In the `docker-compose.yaml` file, you need to set the `STORAGE_TYPE` to `s3` and provide the necessary S3 configuration details under the `environment` section. Here is an example:

``` yaml
STORAGE_TYPE: s3
S3_ENDPOINT: 'https://your-bucket-name.storage.s3.clooudflare.com'
S3_BUCKET_NAME: 'your-bucket-name'
S3_ACCESS_KEY: 'your-access-key'
S3_SECRET_KEY: 'your-secret-key'
S3_REGION: 'your-region'
```

After setting these environment variables, the application will use the specified S3 bucket for storage instead of local storage.

Here is the relevant code that initializes the storage based on these environment variables:

``` type:Quoted,lang:Python,path:api/config.py,lines:138-144
self.STORAGE_TYPE = get_env('STORAGE_TYPE')
self.STORAGE_LOCAL_PATH = get_env('STORAGE_LOCAL_PATH')
self.S3_ENDPOINT = get_env('S3_ENDPOINT')
self.S3_BUCKET_NAME = get_env('S3_BUCKET_NAME')
self.S3_ACCESS_KEY = get_env('S3_ACCESS_KEY')
self.S3_SECRET_KEY = get_env('S3_SECRET_KEY')
self.S3_REGION = get_env('S3_REGION')
```

To configure the application to use object storage instead of local storage, you need to modify the environment variables in the `.env` file or the `docker-compose.yaml` file. Set the `STORAGE_TYPE` to `s3` and provide the necessary S3 configuration details. After setting these environment variables, the application will use the specified S3 bucket for storage instead of local storage.
