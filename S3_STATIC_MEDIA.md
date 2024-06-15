## Media and Static files upload using django

### Step 1: Create an S3 Bucket

### Step 2: Configure Bucket Permissions(Policy)
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::nre-eccom/*"
        }
    ]
}
```
### Configur Cross-origin resource sharing (CORS)
```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```

### Install packages
```
pip install boto3 django-storages
```
### Make a file custom_storage.py
```
# custom_storages.py
from storages.backends.s3boto3 import S3Boto3Storage
class StaticStorage(S3Boto3Storage):
    location = 'static'
    # default_acl = 'public-read'

class MediaStorage(S3Boto3Storage):
    location = 'media'
    # default_acl = 'public-read'
```


### Setup settings.py
```
STATICFILES_DIRS = (os.path.join(BASE_DIR, "static"),)

# AWS S3 settings
AWS_ACCESS_KEY_ID = env('AWS_ACCESS_KEY_ID')
AWS_SECRET_ACCESS_KEY = env('AWS_SECRET_ACCESS_KEY')
AWS_STORAGE_BUCKET_NAME = env('AWS_STORAGE_BUCKET_NAME')
AWS_S3_CUSTOM_DOMAIN = f'{AWS_STORAGE_BUCKET_NAME}.s3.amazonaws.com'
AWS_S3_FILE_OVERWRITE = False

# Static files (CSS, JavaScript, Images)
STATICFILES_STORAGE = 'core.custom_storages.StaticStorage'
STATIC_URL = f'https://{AWS_S3_CUSTOM_DOMAIN}/static/'

# Media files (uploads)
DEFAULT_FILE_STORAGE = 'core.custom_storages.MediaStorage'
MEDIA_URL = f'https://{AWS_S3_CUSTOM_DOMAIN}/media/'

# Optional settings for cache control
AWS_S3_OBJECT_PARAMETERS = {
    'CacheControl': 'max-age=86400',
}
```

### Make custom file 
```
python manage.py collectstatic
```
