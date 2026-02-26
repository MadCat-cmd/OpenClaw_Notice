
## How to setup the local lan dashboard

After the installation of Openclaw, the dasboard Web UI can only be accessed by local machine. That mean the Web UI 
gate is bind by `127.0.0.1`. If you want to access the Web UI dashboard from other deivces in same lan, some configuration 
should be changed. 

The configuration file is a json file, location is in following path:
```
/home/pi/.openclaw/openclaw.jason
```

The new openclaw require secure context to access the dashboard ui, that mean you the `https` singing is needed. If you
still want to use `http` access, that mean only use `<ip>:<port>` to access the dashboard in lan. the browser should runs in a 
a **non-secure** context and blocks WebCrypto. By default, OpenClaw **blocks** Control UI connections without device identity.

following configuration of `gateway` section should setup:

- set up the lan access, default value is `loopback`, which allow the dashboad can only be accessed by local machine
  ```
  openclaw config set gateway.bind lan
  ```

- tolggle the `Insecure-auth` behavior, set the `Insecure-auth` to true:
  ```
  openclaw config set gateway.controlUi.allowInsecureAuth true
  ```

- set `dangerouslyDisableDeviceAuth` to true,  disables Control UI device identity checks and is a severe security downgrade. Revert quickly after emergency use.

  ```
  openclaw config get gateway.controlUi.dangerouslyDisableDeviceAuth
  ```

-  enables Host-header origin fallback mode, but is a dangerous security downgrade
  ```
  openclaw config set gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback true
  ```



