docker build . -t snijderman.experiment.functions.dotnet5

docker run -it snijderman.experiment.functions.dotnet5 /bin/bash 

docker run --rm -d --name functionsUsingNet5 -p 1512:80 snijderman.experiment.functions.dotnet5