# Azure Oauth
This playbook deploys the [Oauth_Proxy](https://github.com/oauth2-proxy/oauth2-proxy) software specifically to function with Azure Active Directory

# Usage
An application must be created in Azure as in this [guide](https://oauth2-proxy.github.io/oauth2-proxy/auth-configuration#azure-auth-provider)

The role then takes three variables

```
azure_tenantid:  xxx-xxx-xxx-xxx | default is common same as Oauth proxy
webauth_client_id: xxxx-xxxx-xxxx # Found from the creation of the application
webauth_client_secret: xxxxxxxxxx # Generated inside of the application
```

This starts and enables the service. In order to consume it you will need to include the oauth.conf nginx snippit in your desired website as below this role is dependant on [mod-nginx](https://github.com/Darkbat91/mod-nginx) 


/etc/nginx/conf.d/myhost.conf
```
server {
    listen 443 ssl;
    root         /usr/share/nginx/html;
    server_name example.com;
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; 

    # Include the authorization backend
    include /etc/nginx/oauth.conf;
    
    location / {
            # send all requests to the `/auth` endpoint for authorization
            auth_request /auth/auth;
            error_page 401 = /auth/sign_in;
    }


}
```

This will define example.com with authentication to the defined azure application specifically within the tenant.

# Advanced

There are numerous other options within the defaults/main.yml that can change other parts of the behavior of the system