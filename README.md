# Manage Network Hierarchy in IBM Qradar

IBM Qradar has been the leading SIEM in the cybersecurity market.

In this repository you will find a script that will allow you to backup or update the network hierarchy in Qradar using 
the 
API. The current way to update the network hierarchy is through the GUI, but if you have a large number of networks 
this is a nightmare.

The format of the scripts is Jupyter Notebook and I will do my best to make the code as readable as possible. 
You can feel free to modify the code to your liking.

Everything shown in this repository is for educational purposes, if you want to use them in productive environments you must make sure to do the respective tests before affecting any system.

The official IBM documentation on the use of the API is at https://www.ibm.com/docs/en/qradar-common?topic=api-endpoint-documentation-supported-versions

Qradar is an IBM product and rights are owned by them.

Must check

- [x]  Have an authorized service token with admin privileges. Variable=SEC_TOKEN
- [x]  Network connectivity with the Qradar console through port 433. Variable=URL_base

## How it works

- Step 1: The script will get the current network hierarchy as backup. JSON format.
- Step 2: The script will get the domains because we need the domain_id to create the network hierarchy. 
- Step 3: You need to manually create a CSV file with the new networks in like this:
```csv
id,group,name,description,cidr,domain_id,country_code
1,Grupo-1,DMZ,New Network,1.1.1.0/24,1,CO
```
I recommend using Excel to create the CSV file and I will leave you a template in the repository.
(I did not include the location field because it is optional)

- Step 4: As the API is designed to **REPLACE** the current network hierarchy, we need to merge the CSV file with the 
current network hierarchy.
- Step 5: Send the new network hierarchy to Qradar.
- Step 6: Check the new network hierarchy in the Qradar console.
- Step 7: If everything is OK, you can send me a coffee ;)

## API Reference:

This are the API endpoints that are used in the script.

#### get_network_hierarchy: Retrieves the staged network hierarchy.

```https
   GET - /config/network_hierarchy/staged_networks
```
Response Description

Network Hierarchy - A JSON string that contains network_hierarchy objects.

#### PUT network_hierarchy/staged_networks: Replaces the current network hierarchy with the input that is provided.

```https
   PUT - /config/network_hierarchy/staged_networks
```

Network Hierarchy - A JSON string that contains network_hierarchy objects. this is an example of the JSON that is required to update the network hierarchy.

```json
[
{
"id": 4,
"group": "DMZ",
"name": "External",
"description": "network description",
"cidr": "0.0.0.1/32",
"domain_id": 0,
"location": {"type": "Point", "coordinates": [-75.69805556, 45.41111111]},
"country_code": "CA"
},
{
"id": 5,
"group": "DMZ",
"name": "External",
"description": "network description",
"cidr": "0.0.0.2/32",
"domain_id": 0,
"location": {"type": "Point", "coordinates": [-66.646332, 45.964993]},
"country_code": "CA"
}
]
```
GET domains: Retrieves the list of domains.

```https
   GET - /config/domain_management/domains
```



## Authors

- [@chmedinap](https://www.github.com/chmedinap)

