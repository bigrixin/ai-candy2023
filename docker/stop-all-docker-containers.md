---
icon: xmark-to-slot
---

# Stop all docker containers

@echo off

echo ----------------------------------\
echo Stop all Docker containers\
echo ----------------------------------

for /f "tokens=\*" %%i in ('docker ps -q') do docker stop %%i

echo.\
echo ----------------------------------\
echo All Docker containers stopped\
echo ----------------------------------

docker ps
