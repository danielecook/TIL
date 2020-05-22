# Docker alias for quickly running images

Stick this in your bash profile if you want to quickly startup bash with docker.

```bash
function dr {
  images=$(docker images --format "{{.Repository}}:{{.Tag}}" | grep "$1")
  if [[ ${images} = *$'\n'* ]]; then
    echo "${images}"
  else
    image=$(echo ${images} | awk '{ print $1 }')
    echo ${image}
    docker run -it ${image} /bin/bash
  fi;
}
```

Just type `dr image/name` and the alias will run the equivelent:

`docker run -it ${image} /bin/bash`