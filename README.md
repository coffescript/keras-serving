# keras-serving

keras->tensorflow+grpc+docker=>nodejs :whale::fire:

- example of bringing a keras model to production using tensorflow serving
- using custom XOR model with tensor.proto dimensions example
- building & training of the model works with python2.7 on the workstation
- exported model is served via grpc in a C++ server using a Docker-Container
- a nodejs server wraps the grpc api for a simple http POST endpoint

# Workflow (Unix)

## You will need

```
python
pip
docker (docker-compose)
```

## 1. Install Requirements

```
./prepare.sh
# install python dependencies via pip
# builds the docker image for tensorflow_serving (takes a while ~ 30 minutes)
# image size ~ 3.5 GB
```
checkout [build troubleshoot](build.md) if you are having trouble

## 2. Build, Train and Serialise Keras Model

```
python train.py
# results will be in (/result)
```

## 3. Load and Export Model as Tensorflow Graph

```
python export.py
# results will be in (/export)
```

## 4. Build & Run Containers via docker-compose

```
docker-compose up --build
# docker-compose up -d
# docker-compose stop
# docker-compose rm
```

## 5. Test API via curl

```
curl -X POST \
  http://localhost:8080/predict \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{"inputs": [0,1]}'
```
