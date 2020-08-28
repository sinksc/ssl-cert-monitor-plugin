# ssl-cert-monitor-plugin (Linux/Unix)
Lightweight Rundeck Job Step plugin to monitor HTTPS certificate expiration dates. No more calendar reminders to renew your SSL certs! If you are responsible for keeping a website's SSL certificate up to date, you can create a job using this plugin and schedule it to check your site on a recurring basis. For example, run weekly and notify you when your certificate is within 30 days of expiring.

## Usage

### ssl / cert / monitor

For a given **valid** HTTPS URL and a threshhold measured in days, if that URL's HTTPS certificate will expire in the specified number of days or less, it will trigger a failure. This in turn can trigger Notifications or more advanced error processing. 

Note that the URL provided must be reachable from the target node that the Rundeck job will execute on (or the Rundeck server itself if running on the local node). To test this, you can try running the following command directly on the target node:

```shell
curl --insecure --verbose --head https://example.com
```

This job step is lightweight in that it can check any certificate for any website that the target node has access to. It does not require interaction with the SSL keystore or the web server itself. It is the equivalent of loading the URL in your browser and examining the returned SSL certificate.

#### Failure Codes

This step returns the following exit codes:

| Exit Code |  Reason  |
|:----------:|:-------- |
|      0     | Success. Certificate is valid for the period of time specified. |
|      1     | Certificate expiration falls within notification threshhold. Time to renew. |
|      2     | Invalid arguments specified. |
|      3     | General curl command failure (output will display error details). |
|      4     | Site was contacted, but no SSL details found in the response. |

##### Log Filtering (Optional)

The exit code is also assigned to "RUNDECK:DATA:EXIT_CODE" for use in Key-Value log filtering, if desired. [See here for more information.](https://docs.rundeck.com/docs/manual/log-filters/key-value-data.html)

## Building

1. To build the plugin, simply zip it but exclude the .git files:

    ```shell
    zip -r ssl-cert-monitor-plugin.zip ssl-cert-monitor-plugin -x *.git*
    ```

2. Then copy the zip file to the plugins directory:

    ```shell
    cp ssl-cert-monitor-plugin.zip $RDECK_BASE/libext
    ```
