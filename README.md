# splunk-otel-for-centossix
 
- Download `curl -sSL https://raw.githubusercontent.com/jek-bao-choo/splunk-otel-for-centossix/main/splunk-otel-for-centossix > /tmp/splunk-otel-for-centossix`
- Move `sudo mv /tmp/splunk-otel-for-centossix /etc/rc.d/init.d/`
- Change mode `sudo chmod 755 /etc/rc.d/init.d/splunk-otel-for-centossix`
- Install service `sudo service splunk-otel-for-centossix install`
- Create a folder for collector if it doesn't exist `sudo mkdir -p /etc/otel/collector`
- `sudo touch /etc/otel/collector/splunk-otel-collector.conf`
- `sudo chmod 777 /etc/otel/collector/splunk-otel-collector.conf`
```bash
SPLUNK_CONFIG=/etc/otel/collector/agent_config.yaml
SPLUNK_ACCESS_TOKEN=$SPLUNK_ACCESS_TOKEN
SPLUNK_REALM=$SPLUNK_REALM
SPLUNK_API_URL=https://api.$SPLUNK_REALM.signalfx.com
SPLUNK_INGEST_URL=https://ingest.$SPLUNK_REALM.signalfx.com
SPLUNK_TRACE_URL=https://ingest.$SPLUNK_REALM.signalfx.com/v2/trace
SPLUNK_HEC_URL=$SPLUNK_HEC_URL
SPLUNK_HEC_TOKEN=$SPLUNK_HEC_TOKEN
SPLUNK_MEMORY_TOTAL_MIB=512
SPLUNK_BUNDLE_DIR=/usr/lib/splunk-otel-collector/agent-bundle
SPLUNK_COLLECTD_DIR=/usr/lib/splunk-otel-collector/agent-bundle/run/collectd
EOF
```
- Start `sudo service splunk-otel-for-centossix start`
- Download Java agent (if using Java) `sudo mkdir /opt/splunk && curl -L https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar -o /opt/splunk/splunk-otel-javaagent.jar`
- Set environment variables
- Add the following to java_opts if available `-javaagent:/opt/splunk/splunk-otel-javaagent.jar`