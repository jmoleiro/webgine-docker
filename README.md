# WebGine Docker Images

This repository will contain the Dockerfiles for the WebGine platform.

Currently we ony provide the images for the WebGine CI environment, but we plan to provide more images for hosting the WebGine platform.

## WebGine CI

Image to integrate WebGine on CI environments. Is based on the 
[Official PHP 8.1+Apache Docker image](https://hub.docker.com/layers/library/php/8.1-apache/images/sha256-1890a914d868d701e51b70f2a1b3b482162875d560b44290e2511f05f2cb2b13), with some parts from
[composer-and-node-ci image](https://github.com/carlos-algms/composer-and-node-ci) but using NVM for Node, among other changes, like [SQLSrv Drivers](https://github.com/microsoft/msphpsql).















