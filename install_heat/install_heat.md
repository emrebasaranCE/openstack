## 1. Installing the Orchestration Service **Heat**

As we can see from the [documentation](https://docs.openstack.org/heat/latest/getting_started/on_devstack.html), Heat service is fully integrated into DevStack. In order to enable heat, we have to add this line into our local.conf file:
```bash
enable_service h-eng h-api h-api-cfn h-api-cw

enable_plugin heat https://opendev.org/openstack/heat

```

After saving this to our local.conf, we have to re-run our stack like this:
```bash
./unstack.sh
./stack.sh
```

After these stages, our system is ready to be used!
