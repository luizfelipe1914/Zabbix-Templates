

##### [EN]

### Configuration of Apache Tomcat for monitoring by http/status-page (JSON):

1. First of all, one user with manager-status role is necessary. 
   
   1.1. In the tomcat-users.xml, add the following line:
   
   ```xml
    <user username="zabbix" password="tomcat" roles="manager-status"/>
   ```

        1.2. In the context.xml file (<tomcat_install_directory>/webapps/manager/META-INF)of the context manager, change the allowed IPs for the status-page that will be accessible from Zabbix Server:

 -    Before:

```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve"       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1"/>
```

-    After:

```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
 allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|192\.168\.1\.65"/>
```

**WARNING**: Some characters(. and : ) are preceded by \ in the string in XML tag. In this case I used the IP 192.168.1.65, change for IP of your Zabbix Server in your network.

2. After the settings have been applied, restart the Tomcat service.

3. Test the settings with the command:
   
   ```shell
   curl --location 'http://<IP>:<PORT>/manager/status/all?JSON=true' -u 'zabbix:tomcat' 
   ```

4. A  JSON like the following will be returned:
   
   ```json
   {"tomcat":{
     "jvm":{
       "memory":{"free":"46570632","total":"134217728","max":"268435456"},
       "memorypool":[
         {"name":"G1 Eden Space","type":"Heap memory","usageInit":"7340032","usageCommitted":"75497472","usageMax":"-1","usageUsed":"58720256"},
         {"name":"G1 Old Gen","type":"Heap memory","usageInit":"126877696","usageCommitted":"54525952","usageMax":"268435456","usageUsed":"24471424"},
         {"name":"G1 Survivor Space","type":"Heap memory","usageInit":"0","usageCommitted":"4194304","usageMax":"-1","usageUsed":"4077904"},
         {"name":"CodeHeap 'non-nmethods'","type":"Non-heap memory","usageInit":"2555904","usageCommitted":"2555904","usageMax":"5767168","usageUsed":"1914752"},
         {"name":"CodeHeap 'non-profiled nmethods'","type":"Non-heap memory","usageInit":"2555904","usageCommitted":"3014656","usageMax":"123011072","usageUsed":"2846208"},
         {"name":"CodeHeap 'profiled nmethods'","type":"Non-heap memory","usageInit":"2555904","usageCommitted":"10878976","usageMax":"122880000","usageUsed":"10099200"},
         {"name":"Compressed Class Space","type":"Non-heap memory","usageInit":"0","usageCommitted":"4653056","usageMax":"1073741824","usageUsed":"4334256"},
         {"name":"Metaspace","type":"Non-heap memory","usageInit":"0","usageCommitted":"38862848","usageMax":"-1","usageUsed":"38175456"}
       ]
     },
     "connector":[
       {
         "name":"http-nio-8080",
         "threadInfo":{"maxThreads":"200","currentThreadCount":"10","currentThreadsBusy":"1"},
         "requestInfo":{"maxTime":"0","processingTime":"0","requestCount":"0","errorCount":"0","bytesReceived":"0","bytesSent":"0"}
       }],
     "context":[
       {
         "name":"localhost/docs","startTime":"Wed Oct 30 11:54:41 PDT 2024","startupTime":"0","tldScanTime":"0",
         "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
         "jsp":{"jspCount":"0","jspReloadCount":"0"},
         "wrapper":[
           {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
         ]
       },
       {
         "name":"localhost/","startTime":"Wed Oct 30 11:54:43 PDT 2024","startupTime":"0","tldScanTime":"0",
         "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
         "jsp":{"jspCount":"0","jspReloadCount":"0"},
         "wrapper":[
           {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
         ]
       },
       {
         "name":"localhost/examples","startTime":"Wed Oct 30 11:54:42 PDT 2024","startupTime":"0","tldScanTime":"0",
         "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
         "jsp":{"jspCount":"0","jspReloadCount":"0"},
         "wrapper":[
           {"servletName":"SessionExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"simpleimagepush","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"RequestHeaderExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"CompressionFilterTestServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"bytecounter","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"numberwriter","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"RequestParamExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"async1","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"async0","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"async3","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"CookieExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"stock","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"async2","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"HelloWorldExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"1","classLoadTime":"0"},
           {"servletName":"RequestInfoExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"responsetrailer","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"ServletToJsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
         ]
       },
       {
         "name":"localhost/ScadaBR","startTime":"Wed Oct 30 11:54:41 PDT 2024","startupTime":"110","tldScanTime":"0",
         "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
         "jsp":{"jspCount":"0","jspReloadCount":"0"},
         "wrapper":[
           {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"8","classLoadTime":"0"},
           {"servletName":"imageChart","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"RoseServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"reportEventExport","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"reportUserCommentExport","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"AdminServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"495","classLoadTime":"30"},
           {"servletName":"springDispatcher","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"467","classLoadTime":"4"},
           {"servletName":"asyncImageChart","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"httpDataSource","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"3","classLoadTime":"3"},
           {"servletName":"imageValue","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"AxisServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"reportExport","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"dwr-invoker","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"219","classLoadTime":"0"},
           {"servletName":"reportChart","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
         ]
       },
       {
         "name":"localhost/manager","startTime":"Wed Oct 30 11:54:43 PDT 2024","startupTime":"0","tldScanTime":"0",
         "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
         "jsp":{"jspCount":"0","jspReloadCount":"0"},
         "wrapper":[
           {"servletName":"Manager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"JMXProxy","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"3","classLoadTime":"0"},
           {"servletName":"HTMLManager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"Status","processingTime":"0","maxTime":"0","requestCount":"1","errorCount":"0","loadTime":"16","classLoadTime":"6"}
         ]
       },
       {
         "name":"localhost/host-manager","startTime":"Wed Oct 30 11:54:42 PDT 2024","startupTime":"0","tldScanTime":"0",
         "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
         "jsp":{"jspCount":"0","jspReloadCount":"0"},
         "wrapper":[
           {"servletName":"HTMLHostManager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"2","classLoadTime":"0"},
           {"servletName":"HostManager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
           {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
         ]
       }
     ]
   }}  
   ```
   
   5. In the /usr/lib/tomcat/externalscripts directory , run the following commands to make download of the discovery script:
      
      ```shell
      cd /usr/lib/tomcat/externalscripts
      wget https://raw.githubusercontent.com/luizfelipe1914/Zabbix-Templates/refs/heads/main/tomcat/tomcat_discovery
      chmod +x tomcat_discovery
      ```

        6.  Finally, import the template on Zabbix Frontend .

###### [PT-BR]

### Configuração do Apache Tomcat para monitoração via http/status-page (JSON):

1. Criar um usuário para autenticação:
   
   1.1. No arquivo tomcat-users.xml adicionar a seguinte linha:
   
   ```xml
    <user username="zabbix" password="tomcat" roles="manager-status"/>
   ```
   
   1.2. No arquivo context.xml do context manager (<tomcat_install_direcotory>/webapps/manager/META-INF), configurá-lo para que a status page esteja disponível para o Zabbix Server:
   
   - Original:  
   
   ```xml
   <Valve className="org.apache.catalina.valves.RemoteAddrValve"       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1"/>
   ```
   
   - Alterar para:
   
   ```xml
   <Valve className="org.apache.catalina.valves.RemoteAddrValve"
    allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|192\.168\.1\.65"/>
   ```

        **OBS**.: Alguns carecteres(. e :) devem ser escapados com \\ , utilizei o IP 192.168.1.65 apenas como exemplo. Utilize o IP do seu Zabbix Server.        

2 . Após proceder as modificações nos arquivos, reiniciar o serviço do Tomcat.

 3. Para validar  o acesso:

```shell
curl --location 'http://<IP>:<PORT>/manager/status/all?JSON=true' -u 'zabbix:tomcat' 
```

4. Devera ser retornado um JSON semelhante a esse:

```json
{"tomcat":{
  "jvm":{
    "memory":{"free":"46570632","total":"134217728","max":"268435456"},
    "memorypool":[
      {"name":"G1 Eden Space","type":"Heap memory","usageInit":"7340032","usageCommitted":"75497472","usageMax":"-1","usageUsed":"58720256"},
      {"name":"G1 Old Gen","type":"Heap memory","usageInit":"126877696","usageCommitted":"54525952","usageMax":"268435456","usageUsed":"24471424"},
      {"name":"G1 Survivor Space","type":"Heap memory","usageInit":"0","usageCommitted":"4194304","usageMax":"-1","usageUsed":"4077904"},
      {"name":"CodeHeap 'non-nmethods'","type":"Non-heap memory","usageInit":"2555904","usageCommitted":"2555904","usageMax":"5767168","usageUsed":"1914752"},
      {"name":"CodeHeap 'non-profiled nmethods'","type":"Non-heap memory","usageInit":"2555904","usageCommitted":"3014656","usageMax":"123011072","usageUsed":"2846208"},
      {"name":"CodeHeap 'profiled nmethods'","type":"Non-heap memory","usageInit":"2555904","usageCommitted":"10878976","usageMax":"122880000","usageUsed":"10099200"},
      {"name":"Compressed Class Space","type":"Non-heap memory","usageInit":"0","usageCommitted":"4653056","usageMax":"1073741824","usageUsed":"4334256"},
      {"name":"Metaspace","type":"Non-heap memory","usageInit":"0","usageCommitted":"38862848","usageMax":"-1","usageUsed":"38175456"}
    ]
  },
  "connector":[
    {
      "name":"http-nio-8080",
      "threadInfo":{"maxThreads":"200","currentThreadCount":"10","currentThreadsBusy":"1"},
      "requestInfo":{"maxTime":"0","processingTime":"0","requestCount":"0","errorCount":"0","bytesReceived":"0","bytesSent":"0"}
    }],
  "context":[
    {
      "name":"localhost/docs","startTime":"Wed Oct 30 11:54:41 PDT 2024","startupTime":"0","tldScanTime":"0",
      "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
      "jsp":{"jspCount":"0","jspReloadCount":"0"},
      "wrapper":[
        {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
      ]
    },
    {
      "name":"localhost/","startTime":"Wed Oct 30 11:54:43 PDT 2024","startupTime":"0","tldScanTime":"0",
      "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
      "jsp":{"jspCount":"0","jspReloadCount":"0"},
      "wrapper":[
        {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
      ]
    },
    {
      "name":"localhost/examples","startTime":"Wed Oct 30 11:54:42 PDT 2024","startupTime":"0","tldScanTime":"0",
      "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
      "jsp":{"jspCount":"0","jspReloadCount":"0"},
      "wrapper":[
        {"servletName":"SessionExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"simpleimagepush","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"RequestHeaderExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"CompressionFilterTestServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"bytecounter","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"numberwriter","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"RequestParamExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"async1","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"async0","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"async3","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"CookieExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"stock","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"async2","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"HelloWorldExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"1","classLoadTime":"0"},
        {"servletName":"RequestInfoExample","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"responsetrailer","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"ServletToJsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
      ]
    },
    {
      "name":"localhost/ScadaBR","startTime":"Wed Oct 30 11:54:41 PDT 2024","startupTime":"110","tldScanTime":"0",
      "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
      "jsp":{"jspCount":"0","jspReloadCount":"0"},
      "wrapper":[
        {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"8","classLoadTime":"0"},
        {"servletName":"imageChart","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"RoseServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"reportEventExport","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"reportUserCommentExport","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"AdminServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"495","classLoadTime":"30"},
        {"servletName":"springDispatcher","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"467","classLoadTime":"4"},
        {"servletName":"asyncImageChart","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"httpDataSource","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"3","classLoadTime":"3"},
        {"servletName":"imageValue","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"AxisServlet","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"reportExport","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"dwr-invoker","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"219","classLoadTime":"0"},
        {"servletName":"reportChart","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
      ]
    },
    {
      "name":"localhost/manager","startTime":"Wed Oct 30 11:54:43 PDT 2024","startupTime":"0","tldScanTime":"0",
      "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
      "jsp":{"jspCount":"0","jspReloadCount":"0"},
      "wrapper":[
        {"servletName":"Manager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"JMXProxy","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"3","classLoadTime":"0"},
        {"servletName":"HTMLManager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"Status","processingTime":"0","maxTime":"0","requestCount":"1","errorCount":"0","loadTime":"16","classLoadTime":"6"}
      ]
    },
    {
      "name":"localhost/host-manager","startTime":"Wed Oct 30 11:54:42 PDT 2024","startupTime":"0","tldScanTime":"0",
      "manager":{"activeSessions":"0","sessionCounter":"0","maxActive":"0","rejectedSessions":"0","expiredSessions":"0","sessionMaxAliveTime":"0","sessionAverageAliveTime":"0","processingTime":"0"},
      "jsp":{"jspCount":"0","jspReloadCount":"0"},
      "wrapper":[
        {"servletName":"HTMLHostManager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"jsp","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"2","classLoadTime":"0"},
        {"servletName":"HostManager","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"},
        {"servletName":"default","processingTime":"0","maxTime":"0","requestCount":"0","errorCount":"0","loadTime":"0","classLoadTime":"0"}
      ]
    }
  ]
}}  
```

5. No diretório /usr/lib/tomcat/externalscripts directory , execute os seguintes comandos para baixar o script de discovery:
   
   ```shell
   cd /usr/lib/tomcat/externalscripts
   wget https://raw.githubusercontent.com/luizfelipe1914/Zabbix-Templates/refs/heads/main/tomcat/tomcat_discovery
   chmod +x tomcat_discovery
   ```

   6. Por fim, importe o template no Zabbix Frontend .
