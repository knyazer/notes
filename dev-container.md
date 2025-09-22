making dev containers is somewhat hard

easy option:
```
  distrobox create --nvidia --home /home/knyaz/ -i ubuntu -n dev_
```
this might work, and then you are lucky. If it does not work, you are depressed. You should not install drivers in the container, but check if nvidia-smi works. If it doesn't your fucked.

Then the hard option. Figure out the cuda version, after running nvidia-smi on the host.

Use the same major cuda, but latest minor to select an nvidia container version, e.g. for 12th cuda it is `12.9.1-cudnn-runtime-ubuntu24.04`


Now, make the container:
```
distrobox create --name nvidia --additional-flags "--gpus all" --image docker.io/nvidia/cuda:12.9.1-cudnn-runtime-ubuntu24.04
```

when trying to start it with `distrobox enter nvidia` it might fail due to cdi devices not being found or whatever.

then you should generate cdi definitions: `sudo nvidia-ctk cdi generate > /etc/cdi/nvidia.yaml` and check that they generated correctly by looking if `nvidia-ctk cdi list` returns any (yours) devices.

now, it still might not work because drivers or something are not loaded correctly.

Run in bash:
```bash
sudo modprobe nvidia
sudo modprobe nvidia-uvm
sudo modprobe nvidia-modeset

# ensure device nodes exist
if ! [ -e /dev/nvidia-uvm ]; then
  # this comes with the driver utilities package
  sudo nvidia-modprobe -u -c=0
fi

# verify
ls -l /dev/nvidia* | grep -E 'uvm|uvm-tools'
```

now all should work :)


