# pcollabora

Original from https://github.com/CollaboraOnline/Docker-CODE
Made some changes to be able to us it in with PLESK


# Installation

1. install docker image bluedart/collabora
2. set DOMAIN to your nexcloud domain. Collabora is ownly allowed to be used by this domain.
3. create a subdomain like "collabora.yourdomain.com"
4. Install letscrypt for the domain
5. Install Collabora app in Nextcloud which is under Productivity
6. Set your Collabora in Nextcloud via Admin -> Additional Settings -> Collabora Host = https://collabora.yourdomain.com
7. Add rewrite and Proxy rules for "collabora.yourdomain.com" like shown below. just copy and paste ;)
8. If you want to use the admin interface you have to set the variable PASSWORD and USERNAME for the docker image
9. Have fun ;)



Additional settings for rerouting http to https:
```
<IfModule mod_rewrite.c>
	RewriteEngine On
	RewriteCond %{HTTPS} off
	RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
</IfModule>
```


Additional settings for apache2 https :
```
# Encoded slashes need to be allowed
AllowEncodedSlashes NoDecode

# Container uses a unique non-signed certificate
SSLProxyEngine On
SSLProxyVerify None
SSLProxyCheckPeerCN Off
SSLProxyCheckPeerName Off

# keep the host
ProxyPreserveHost On

# static html, js, images, etc. served from loolwsd
# loleaflet is the client part of LibreOffice Online
ProxyPass           /loleaflet https://127.0.0.1:9980/loleaflet retry=0
ProxyPassReverse    /loleaflet https://127.0.0.1:9980/loleaflet

# WOPI discovery URL
ProxyPass           /hosting/discovery https://127.0.0.1:9980/hosting/discovery retry=0
ProxyPassReverse    /hosting/discovery https://127.0.0.1:9980/hosting/discovery

# Main websocket
ProxyPassMatch "/lool/(.*)/ws$" wss://127.0.0.1:9980/lool/$1/ws nocanon

# Admin Console websocket
ProxyPass   /lool/adminws wss://127.0.0.1:9980/lool/adminws

# Download as, Fullscreen presentation and Image upload operations
ProxyPass           /lool https://127.0.0.1:9980/lool
ProxyPassReverse    /lool https://127.0.0.1:9980/lool
```
