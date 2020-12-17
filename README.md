Description
This will help to monitor kubernetes with zabbix. This template is customized for Zabbix 4.0 version.

Installation
Install zabbix-agent 4.0 at kubernetes master node
Copy k8s-stats.py to /etc/zabbix/scripts/ and give execute permission to the file: chmod +x k8s-stats.py
Copy k8s.conf to /etc/zabbix/zabbix_agentd.d/.
Import Zabbix template (k8s_zabbix_templates_4.xml) to Zabbix server
Create zabbix user in Kubernetes using zabbix-user-example.yml
Set it's token and API server url in k8s-stats.py
Apply template to zabbix host
Create zabbix user in Kubernetes
$ kubectl apply -n kube-system -f zabbix-user-example.yml
Retrieve API SERVER
$ APISERVER=https://$(kubectl -n default get endpoints kubernetes --no-headers | awk '{ print $2 }')
$ echo $APISERVER
Retrieve TOKEN
$ TOKENNAME=$(kubectl get sa/zabbix-user -n kube-system -o jsonpath='{.secrets[0].name}')
$ TOKEN=$(kubectl -n kube-system get secret $TOKENNAME -o jsonpath='{.data.token}'| base64 --decode)
$ echo $TOKEN
