# RDATA
RDATA is a Data Layer that aim to be lightweight, fast, easy distribution and scalable. 

The main goal of this project is work with large amount of data inside on Reactioon services/tools. But on reactioon ecossystem the users can create their own tools, and on RDATA, can create anything.

# Install

The install process of RDATA is simple, don't require any aditional software and is available to just run.

## Process

1. The installr will create an directory on location that you choose
2. After create the folder, the install will copy binary files to run client,server and api.
3. After copy files the installer will create an rdata.yaml file with the install settings.
4. After copy settings file, an folder called `data` will be created to store your data.
5. After create the folder to store data, if you choose it, the service file will be created.
6. After create the service file, the installer will complete.

## Settings

The settings of your instance will be saved in an file called `rdata.yaml`, this file contains the whole instructions to run an instance. If you use the default location, the file will be located inside of `/reactioon/rdata/rdata.yaml`.

## Data store
All data will be saved in a folder called `data` inside of the instance.

## Instances
Each machine can run multiple instances of RDATA at the same time, use it to increase the performance of your solution.

## Library
The library to comunicate with server is available with Go, in the future we will port to other languages.  If you want create one, do it.

## Considerations
There are nothing, feel free to use in what you want. And if you want make contributions, just create an issue or pull request to it.

That's all! Thanks!  
José Wilker