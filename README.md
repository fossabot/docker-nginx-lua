[![License](http://img.shields.io/:license-mit-blue.svg?style=flat)](https://emanuelemazzotta.com/mit-license)
[![Docker Pulls](https://img.shields.io/docker/pulls/emazzotta/docker-nginx-lua.svg?style=flat)](https://hub.docker.com/r/emazzotta/docker-nginx-lua/)
[![Docker Layers](https://images.microbadger.com/badges/image/emazzotta/docker-nginx-lua.svg?style=flat)](https://microbadger.com/images/emazzotta/docker-nginx-lua "Microbadger Docker Layers")
[![Docker Version Tag](https://images.microbadger.com/badges/version/emazzotta/docker-nginx-lua.svg?style=flat)](https://microbadger.com/images/emazzotta/docker-nginx-lua "Microbadger Docker Info")
[![Docker Commit](https://images.microbadger.com/badges/commit/emazzotta/docker-nginx-lua.svg?style=flat)](https://microbadger.com/images/emazzotta/docker-nginx-lua "Microbadger Docker Commit")
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Femazzotta%2Fdocker-nginx-lua.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Femazzotta%2Fdocker-nginx-lua?ref=badge_shield)

# Docker Nginx Lua

A Docker project for a recent version of the Nginx webserver and the module `more_set_headers` to specify custom headers such as a server name like `1337-server` instead of `nginx` or `apache`.
This also contains LuaJIT so that lua can be used in nginx configurations.
Another module this nginx build contains is [Google's ngx_pagespeed module](https://github.com/pagespeed/ngx_pagespeed)

## Usage

```bash
docker run -v <my_conf_dir>:/etc/nginx/conf.d -v /var/ngx_pagespeed_cache -p 80:80 emazzotta/docker-nginx-lua
```

## Examples

### More Set Headers

```
http {
    ...
    more_set_headers 'Server: 1337-server';
    ...
}
```

### Lua

```
server {   
    ...
    location ~ / {
        rewrite_by_lua '
        for lang in (ngx.var.http_accept_language .. ","):gmatch("([^,]*),") do
            if string.sub(lang, 0, 2) == "en" then
                ngx.redirect("/en/")
            end
            if string.sub(lang, 0, 2) == "de" then
                ngx.redirect("/de/")
            end
        end
        ngx.redirect("/en/")';
    }
    ...
}
```

### Pagespeed

```
server {
    ...
    pagespeed on;
    pagespeed FileCachePath /var/cache/nginx;
    pagespeed XHeaderValue "Pagespeed";
    pagespeed RewriteLevel CoreFilters;
    ...
}
```

## Author

[Emanuele Mazzotta](mailto:hello@mazzotta.me)



## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Femazzotta%2Fdocker-nginx-lua.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Femazzotta%2Fdocker-nginx-lua?ref=badge_large)