PAAS-Controller
=======

## What is it?

The PAAS Controller is a simplistic controller for managing a microservice cluster, it provides a web frontend and API for managing the applications and a monitoring agent to make sure the state of the cluster is maintained

#Requirements
- Python 2.7
- Redis for persistence store (if you don't want to install redis you can just run it in docker ```docker run -p 6379:6379 --name my-redis -d redis```)

#Environment variables

- REDIS_HOST (default: 127.0.0.1)

#Running it

The best way to run this is under a virtualenv

    virtualenv controller
    source controller/bin/activate
    pip install -r requirements.txt
    python server.py
    
Access the app at http://localhost:8000/web

## The web interface

The web interface is available on /web/

## The API

### Global endpoint

/api/global

Global variables:
```
curl /api/global/environment
```

### Application endpoint

/api/app/
/api/app/<app-name>

#Tests

To install a venv and run tests easily:

```
$ ./jenkins.sh
```

#API Documentation

#### Create an application
```

curl -X POST -H 'Content-type: application/json' servername/app/ -d '{ "name": "my-application-name" }'
```

This would create a new application and return the current app details:

```
{  
   "name": "my-application-name", 
   "state": "virgin", 
   "command": "", 
   "memory_in_mb": 512, 
   "ssl_certificate_name": "", 
   "ssl": "false", 
   "port": "55850", 
   "created_at": "2014-11-26T10:26:08.140731", 
   "environment": {}, 
   "docker_image": "", 
   "global_environment": { 
            "LOGGING_HOST": "a-global-host"
            }
   }
```

####Update an application
```
curl -X PATCH -H 'Content-type: application/json' servername/app/<app_name> -d '{ "environment": { "SLUG_URL": "http://sluglocaltion.tgz" }, "memory_in_mb": 128 }'
```

This would return an up to date json document of the application
```
{  
   "name": "my-application-name", 
   "state": "virgin", 
   "command": "", 
   "memory_in_mb": 128, 
   "ssl_certificate_name": "", 
   "ssl": "false", 
   "port": "55850", 
   "created_at": "2014-11-26T10:26:08.140731", 
   "environment": {}, 
   "docker_image": "", 
   "global_environment": { 
            "LOGGING_HOST": "a-global-host",
	    "SLUG_URL": "http://sluglocation.tgz"
            }
   }
```

####Delete an application

```
curl -X DELETE servername/app/my-application-name
```

This will remove an application and return:
```
{
    "app_port": "55850", 
    "message": "Application deleted"
}
```
