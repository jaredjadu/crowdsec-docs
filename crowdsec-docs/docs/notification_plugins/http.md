---
id: http
title: HTTP Plugin
---

The HTTP plugin is by default shipped with your CrowdSec installation. The following guide shows how to configure, test and enable it.

Every alert which would pass the profile's filter would be dispatched to `http_default` plugin.

## Configuring the plugin

By default the configuration for HTTP plugin is located at these default location per OS:

- **Linux** `/etc/crowdsec/notifications/http.yaml`
- **FreeBSD** `/usr/local/etc/crowdsec/notifications/http.yaml`
- **Windows** `C:\ProgramData\CrowdSec\config\notifications\http.yaml`

Configure how to make web requests by providing the `url`, `method`, `headers` etc. 

### Adding the plugin configuration 

Configure how to make web requests by providing the `url`, `method`, `headers` etc. 

Example config which posts the alerts serialized into json to localhost server.

```yaml
# Don't change this
type: http

name: http_default # this must match with the registered plugin in the profile
log_level: info # Options include: trace, debug, info, warn, error, off

format: |  # This template receives list of models.Alert objects. The request body would contain this. 
  {{.|toJson}}

url: http://localhost # plugin will make requests to this url. Eg value https://www.example.com/
# unix_socket: /var/run/example.sock # plugin will send the `url` across the unix socket instead of opening a remote connection

method: POST # eg either of "POST", "GET", "PUT" and other http verbs is valid value. 

# headers:
#   Authorization: token 0x64312313

# skip_tls_verification:  # either true or false. Default is false

# group_wait: # duration to wait collecting alerts before sending to this plugin, eg "30s"

# group_threshold: # if alerts exceed this, then the plugin will be sent the message. eg "10"

# max_retry: # number of tries to attempt to send message to plugins in case of error.

# timeout: # duration to wait for response from plugin before considering this attempt a failure. eg "10s"

```

:::info
`format` is a [go template](https://pkg.go.dev/text/template), which is fed a list of [Alert](https://pkg.go.dev/github.com/crowdsecurity/crowdsec@master/pkg/models#Alert) objects.
:::

## Testing the plugin

Before enabling the plugin it is best to test the configuration so the configuration is validated and you can see the output of the plugin. 

```bash
cscli notifications test http_default
```

:::note
If you have changed the `name` property in the configuration file, you should replace `http_default` with the new name.
:::

## Enabling the plugin

In your profiles you will need to uncomment the `notifications` key and the `http_default` plugin list item.

```
#notifications:
# - http_default 
```

:::note
If you have changed the `name` property in the configuration file, you should replace `http_default` with the new name.
:::

:::warning
Ensure your YAML is properly formatted the `notifications` key should be at the top level of the profile.
:::

<details>

<summary>Example profile with http plugin enabled</summary>

```yaml
name: default_ip_remediation
#debug: true
filters:
 - Alert.Remediation == true && Alert.GetScope() == "Ip"
decisions:
 - type: ban
   duration: 4h
#duration_expr: Sprintf('%dh', (GetDecisionsCount(Alert.GetValue()) + 1) * 4)
#highlight-next-line
notifications:
#highlight-next-line
  - http_default
on_success: break
```

</details>

## Final Steps:

Let's restart crowdsec

```bash
sudo systemctl restart crowdsec
```
