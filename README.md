# Olist-Business-Analysis-ELK
This is a project to analyse on the customers, products, sallers, reviews and orders of the Brazilian E-commerce giant Olist with **Elasticsearch** **Kibana** **Logstash**.

## Downloading and configuring ELK:

### Elasticsearch :
1.  Here's the link to the downloads page : [https://www.elastic.co/downloads/kibana](https://www.elastic.co/downloads/elasticsearch)
2.  Extract and launch a command prompt under the *bin* directory.
3.  Install Elasticsearch as a service with this command :
```
elasticsearch-service.bat install
```
4.  Launch Elasticsearch as a service :
 
```
elasticsearch-service.bat start
```


5.   change the password of the Elasticsearch user named 'elastic' :
   ```
   elasticsearch-reset-password.bat -i -u elastic
   ```

6.   Stop and restart the Elasticsearch service to apply changes :
   ```
elasticsearch-service.bat stop
```
   
```
elasticsearch-service.bat start
```

7.   Launch Elasticsearch using :
   ```
elasticsearch.bat
```

8.   Access to Elasticsearch via HTTPS at https://localhost:9200/ and authenticate with the password you've chosen for the *elastic* user :

   ![image](https://github.com/Eya-Jemal/Olist-Business-Analysis/assets/62604009/bf66a33a-f332-47ab-b43e-d92e65f609ee)
 
   
### Kibana :
1.  Here's the link to the downloads page : https://www.elastic.co/downloads/kibana
2.  
