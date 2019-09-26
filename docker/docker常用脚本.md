# build脚本
#!/usr/bin/env bash

echo "==================== start build docker images =========================="
cd /www/docker/images/
old_IFS=$IFS
IFS=','
cmds=(
   'docker build -t xxx-base-env .',
   'docker build -t redis:3.2 -f redis.dockerfile .',
   'docker build -t mysql:5.6 -f mysql.dockerfile .',
   'docker build -t elasticsearch:5.6.5 -f es.dockerfile .',
   'docker build -t openresty -f openresty.dockerfile .'
)
for cmd in ${cmds[*]};do
     eval "$cmd"
done
IFS=$old_IFS
echo "==================== end build docker images    =========================="
# 