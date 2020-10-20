
```
docker run -p 9000:9000 -d --name minio1 \
-e "MINIO_ACCESS_KEY=0XSQ5GZDIOU207Q7N48F" \
-e "MINIO_SECRET_KEY=lDwbml1vKpdp9cD16A3f8bMLiXtJegnzINPa3zIw" \
-v /apprun/minio/data:/data \
-v /apprun/minio/config:/root/.minio \
minio/minio server /data
```
