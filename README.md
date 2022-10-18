# splunk-otel-for-centossix
 
- Create a CentOS 6 instance
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
- Check stats `sudo service splunk-otel-for-centossix status`

- Optional: Download Java agent (if using Java) `sudo mkdir /opt/splunk && curl -L https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar -o /opt/splunk/splunk-otel-javaagent.jar`
- Optional: Set environment variables
- Optional: Add the following to java_opts if available `-javaagent:/opt/splunk/splunk-otel-javaagent.jar`

---

# This is useful for adding to A+ Instructions Template
We can read and decide if it is CentOS 6 or 7 then install splunk-otel-collector acorrdingly.

```bash
export OTEL_SERVICE_NAME=<name your service>
export OTEL_RESOURCE_ATTRIBUTES=deployment.environment=<indicate your env>
export SPLUNK_REALM=<realm redacted>
export SPLUNK_ACCESS_TOKEN=<token redacted>
export SPLUNK_HEC_URL=<url redacted>
export SPLUNK_HEC_TOKEN=<token redacted>
export SPLUNK_CONFIG=/etc/otel/collector/agent_config.yaml
echo "SPLUNK_REALM: $OTEL_SERVICE_NAME"
echo "SPLUNK_ACCESS_TOKEN: $OTEL_RESOURCE_ATTRIBUTES"
echo "SPLUNK_REALM: $SPLUNK_REALM"
echo "SPLUNK_ACCESS_TOKEN: $SPLUNK_ACCESS_TOKEN"
echo "SPLUNK_HEC_URL: $SPLUNK_HEC_URL"
echo "SPLUNK_HEC_TOKEN: $SPLUNK_HEC_TOKEN"
echo "SPLUNK_CONFIG: $SPLUNK_CONFIG"
# https://github.com/signalfx/splunk-otel-collector/blob/main/docs/getting-started/linux-manual.md#other
# We need to specific agent_config.yaml because by default it uses gateway_config.yaml
# Hence, the above environment variables are required

CENTOS_RELEASE=$(cat /etc/centos-release)
CENTOS_RHEL_VERSION=$(rpm -E %{rhel})
echo "CENTOS_RELEASE: $CENTOS_RELEASE"
echo "CENTOS_RHEL_VERSION: $CENTOS_RHEL_VERSION"

if [ -n "$SPLUNK_REALM" ] && [ -n "$SPLUNK_ACCESS_TOKEN" ] && [ -n "$SPLUNK_HEC_URL" ] && [ -n "$SPLUNK_HEC_TOKEN" ];  then
    if [ $CENTOS_RHEL_VERSION -eq 6 ]; then
      # Install splunk-otel-for-centossix
      echo "Processing request to install opentelemetry collector for CentOS 6"

      echo "Downloading splunk-otel-for-centossix command: sudo curl -sSL https://raw.githubusercontent.com/jek-bao-choo/splunk-otel-for-centossix/main/splunk-otel-for-centossix > /tmp/splunk-otel-for-centossix"
      sudo curl -sSL https://raw.githubusercontent.com/jek-bao-choo/splunk-otel-for-centossix/main/splunk-otel-for-centossix > /tmp/splunk-otel-for-centossix

      echo "Moving to init.d command: sudo mv /tmp/splunk-otel-for-centossix /etc/rc.d/init.d/"
      sudo mv /tmp/splunk-otel-for-centossix /etc/rc.d/init.d/

      echo "Changing mode command: sudo chmod 755 /etc/rc.d/init.d/splunk-otel-for-centossix"
      sudo chmod 755 /etc/rc.d/init.d/splunk-otel-for-centossix
      
      echo "Installing splunk-otel-for-centossix in init.d command: sudo service splunk-otel-for-centossix install"
      sudo service splunk-otel-for-centossix install

      sudo mkdir -p /etc/otel/collector
      sudo touch /etc/otel/collector/splunk-otel-collector.conf
      sudo chmod 777 /etc/otel/collector/splunk-otel-collector.conf
sudo cat <<EOF > /etc/otel/collector/splunk-otel-collector.conf
SPLUNK_CONFIG=$SPLUNK_CONFIG
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

      echo "Verifying /etc/otel/collector/splunk-otel-collector.conf content: $(cat /etc/otel/collector/splunk-otel-collector.conf)"

      echo "Starting opentelemetry collector for CentOS 6 command: sudo service splunk-otel-for-centossix start"
      sudo service splunk-otel-for-centossix start

      echo "Checking opentelemetry collector for CentOS 6 status command: sudo service splunk-otel-for-centossix status"
      sudo service splunk-otel-for-centossix status
      
    else
      # Install splunk-otel-collector for CentOS 7 and above

      echo "Downloading splunk-otel-collector for CentOS 7 and above command: sudo curl -sSL https://dl.signalfx.com/splunk-otel-collector.sh > /tmp/splunk-otel-collector.sh"
      sudo curl -sSL https://dl.signalfx.com/splunk-otel-collector.sh > /tmp/splunk-otel-collector.sh

      echo "Installing splunk-otel-collector for CentOS 7 and above command: sudo sh /tmp/splunk-otel-collector.sh --realm ${SPLUNK_REALM} -- ${SPLUNK_ACCESS_TOKEN} --mode agent --hec-url ${SPLUNK_HEC_URL} --hec-token ${SPLUNK_HEC_TOKEN}"
      sudo sh /tmp/splunk-otel-collector.sh --realm ${SPLUNK_REALM} -- ${SPLUNK_ACCESS_TOKEN} --mode agent --hec-url ${SPLUNK_HEC_URL} --hec-token ${SPLUNK_HEC_TOKEN}

      echo "Checking opentelemetry collector for CentOS 7 and above status command: sudo systemctl status splunk-otel-collector"
      sudo systemctl status splunk-otel-collector

    fi

    # Download Java agent (if using Java)
    echo "Creating directory for opentelemetry java agent command: sudo mkdir -p /opt/splunk"
    sudo mkdir -p /opt/splunk

    echo "Downloading opentelemetry java agent to /opt/splunk/ command: sudo curl -L https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar -o /opt/splunk/splunk-otel-javaagent.jar"
    sudo curl -L https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar -o /opt/splunk/splunk-otel-javaagent.jar
    
  fi
```
