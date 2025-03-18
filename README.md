# Using Heroku Postgres in Private Space from Data Cloud
When using Heroku Postgres in a Private Space data in Data Cloud you need to use mTLS to connect, open up for external connects and whitelist the IP's connecting to Heroku Postgres. Steps are as follows:
1. In Data Cloud Setup find the CDP instance where your Data Cloud instance is hosted. Refer to the [documentation](https://help.salesforce.com/s/articleView?id=data.c360_a_data_cloud_ip_address_allowlist.htm&type=5) for the IP addresses to whitelist in CIDR format.
2. Refer to [Connecting to a Private or Shield Heroku Postgres Database from an External Resource](https://devcenter.heroku.com/articles/heroku-postgres-via-mtls) to configure external access and downloading the mTLS keys and certificates.
3. Update the whitelist of the IP addresses that connect e.g. using the Heroku CLI using the CIDR ranges found above.
```bash
heroku data:mtls:ip-rules:create postgresql-deep-41052 --app young-reef-43874 --description "Data Cloud, EU-Central, CDP2-1" --cidr "54.204.177.212/32"
heroku data:mtls:ip-rules:create postgresql-deep-41052 --app young-reef-43874 --description "Data Cloud, EU-Central, CDP2-2" --cidr "35.153.189.123/32"
...
heroku data:mtls:ip-rules:create postgresql-deep-41052 --app young-reef-43874 --description "Data Cloud, EU-Central, CDP2-6" --cidr "3.223.146.214/32"
```

When you connect to the database you need the credentials and add them into the Data Cloud connector.
