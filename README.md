# domain_exporter

Exports the expiration time of your domains as prometheus metrics.

#### Environment variables

- `DOMAIN_EXPORTER_URL_PREFIX` — use when HTTP endpoint served with a prefix,
  e.g.: For this endpoint `http://example.org/exporters/domains` set to
  `/exporters/domains`. Not really required since useful only to prevent
  breaking human-oriented links. Defaults to empty string.

## Configuration

On the Prometheus settings, add the `domain_exporter` probe:

```yaml
- job_name: domain
  metrics_path: /probe
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - target_label: __address__
      replacement: localhost:9222 # domain_exporter address
  static_configs:
    - targets:
      - carlosbecker.com
      - carinebecker.com
      - watchub.pw
```

It works more or less like Prometheus's
[blackbox_exporter](https://github.com/prometheus/blackbox_exporter).

Alerting rules examples can be found on the
[_examples](https://github.com/xMlex/domain_exporter/tree/main/_examples)
folder.

You can configure `domain_exporter` to always export metrics for specific
domains. Create configuration file (`host` field is optional):

```yaml
domains:
- google.com
- name: reddit.com        
  host: whois.godaddy.com # <-- custom whois server for reddit.com
```

And pass file path as argument to `domain_exporter`:

```bash
domain_exporter --config=domains.yaml
```

Notice that if you do that, results are cached, and you should change your job 
`metrics_path` to `/metrics` instead.


**deb/rpm/apk**:

Download the `.apk`, `.deb` or `.rpm` from the [releases page][releases] and
install with the appropriate commands.

**manually**:

Download the pre-compiled binaries from the [releases page][releases] or clone
the repository build from source.

[releases]: https://github.com/xMlex/domain_exporter/releases
