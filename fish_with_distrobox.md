When using fish with distrobox, if you create the container with

```
 distrobox create --nvidia --home /home/knyaz/ -i ubuntu -n dev
```

the fish might still be faulty; you should NOT edit a file in the container `/etc/fish/conf.d/distrobox_config.fish` which overrides the default tide shell :) (the distrobox will regenerate this file for some reason, maybe)

Instead, you SHOULD create an empty file `.config/fish/conf.d/distrobox_config.fish`, which will override the above one.
