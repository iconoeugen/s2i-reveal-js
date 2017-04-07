# Reveal.js Presentation Openshift Source-to-image

## Build Docker image

``` bash
docker build -t iconoeugen/s2i-reveal-js .
```

## Want to try it right now?

To make the presenation available under a different context path, you can change the the following line accordingly:

``` bash
export CONTEXT_PATH=/
```

Download the latest Openshift S2I tool and run the build:

``` bash
s2i build https://github.com/iconoeugen/s2i-reveal-js.git --context-dir=test/test-app -e CONTEXT_PATH=${CONTEXT_PATH} iconoeugen/s2i-reveal-js reveal-js-sample-app
```

Test the generated documentation:

```
docker run -p 8080:8080 reveal-js-sample-app
```

**Accessing the application:**

``` bash
curl http://127.0.0.1:8080/${CONTEXT_PATH}
```

### Debug the scripts

To start each script manually to build the presentation and to run the HTTP server, you can start a docker instance:

``` bash
docker run -it --rm -p 8080:8080 -u $UID -v $PWD/test/test-app:/tmp/src iconoeugen/s2i-reveal-js bash
```

To build the presentation inside the running container:

``` bash
/usr/libexec/s2i/assemble
```

To run the HTTP server inside the running container:

``` bash
/usr/libexec/s2i/run
```

## Environment variables

To set these environment variables, you can place them as a key value pair into a .s2i/environment file inside your source code repository.

* **CONTEXT_PATH**

    The prefix of a URL path where the presentation will be made available. (Default: `/`)

## Initialize documentation in source code repository

``` bash
docker run -it --rm -u $UID -v docs:/opt/app-root/src s2i-reveal-js bash
```

Populate the presentation with a new Sphix documentation

```

```

## Openshift deploy template

### Create image stream

You must create an image stream for the S2I image builder before creating an application:

``` bash
oc create -f openshift/image-stream.json -n openshift
```

### Upload application template

To upload the template to your current project’s template library, pass the JSON file with the following command:

``` bash
oc create -f openshift/template.json
```
