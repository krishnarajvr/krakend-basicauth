# krakend-basicauth
HTTP Basic authentication middleware for the [KrakenD-CE](https://github.com/devopsfaith/krakend-ce)

##### This is not intended to be used in production! It is just a initial version of a possible KrakenD component

## Install and test
```bash
git clone https://github.com/devopsfaith/krakend-ce.git
cd krakend-ce

#Modify handler_factory.go
#Add to imports: basicauth "github.com/krishnarajvr/krakend-basicauth/gin"
#Add to NewHandlerFactory (before "return handlerFactory"): handlerFactory = basicauth.New(handlerFactory, logger)

got get github.com/krishnarajvr/krakend-basicauth/gin

make build

./krakend run -c ./krakend.json -d

curl --user test:123456 http://localhost:8080/supu
```

## Example krakend.json
```json
{
   "version":2,
   "name":"My lovely gateway",
   "port":8080,
   "cache_ttl":"3600s",
   "timeout":"3s",
   "extra_config":{
      "github_com/devopsfaith/krakend-gologging":{
         "level":"DEBUG",
         "prefix":"[KRAKEND]",
         "syslog":false,
         "stdout":true
      }
   },
   "endpoints":[
      {
         "endpoint":"/supu",
         "method":"GET",
         "headers_to_pass":[
            "Authorization",
            "Content-Type"
         ],
         "extra_config":{
            "github_com/krishnarajvr/krakend-basicauth":{
               "username":"test",
               "password":"123456"
            }
         },
         "backend":[
            {
               "host":[
                  "http://127.0.0.1:8000"
               ],
               "url_pattern":"/__debug/supu",
            }
         ]
      }
   ]
}
```