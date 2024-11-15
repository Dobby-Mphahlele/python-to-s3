# python-to-s3
writing to a file in s3 using python 

```python
import boto3
import pandas as pd
import io

# AWS credentials
AWS_ACCESS_KEY = ''
AWS_SECRET_KEY = ''
S3_BUCKET = ''
S3_REGION = ''

# Create an S3 client
s3 = boto3.client(
    's3',
    aws_access_key_id=AWS_ACCESS_KEY,
    aws_secret_access_key=AWS_SECRET_KEY,
    region_name=S3_REGION
)

# Sample Data
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Dobby', 'Sam', 'Machael'],
    'Age': [25, 30, 35, 40, 22, 45],
    'City': ['New York', 'Los Angeles', 'Chicago', 'Johannesburg', 'Pretoria', 'London'],
    'Title': ['Dr', 'Mr', 'Mr','Mr', 'Mr', 'Mr']
}

# Convert data to a DataFrame
df = pd.DataFrame(data)

# Save the DataFrame to a CSV in memory
csv_buffer = io.StringIO()
df.to_csv(csv_buffer, index=False)

# S3 object key (file name in S3)
s3_key = 'sample_data.csv'

# Upload the file to S3
try:
    s3.put_object(
        Bucket=S3_BUCKET,
        Key=s3_key,
        Body=csv_buffer.getvalue(),
        ContentType='text/csv'
    )
    print(f"File uploaded successfully to s3://{S3_BUCKET}/{s3_key}")
except Exception as e:
    print(f"Error uploading file: {e}")

```
