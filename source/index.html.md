---
title: API Reference

language_tabs:
  - python

toc_footers:
  - 

includes:
  - 

search: true

---

# Introduction

Welcome to the Enigma API! 

You can use this API to setup enigma nodes, securely store data, and perform operations on the encrypted data.

Enigma is currently available in Python. 

You will find code examples in the dark area to the right.


# Deployment

## Install Seed Node

> To install enigma on the seed node, use this code:

```python
???
```

The Linux server will be used as seed node (i.e. first node in the cloud). 

Installation prerequisites:
1. Server running Linux (there no specific hardware requirements)
2. Have Docker installed, see <a href='https://docs.docker.com/engine/installation/linux/'>instructions here</a>

To deply enigma on the server copy the engima docker image to a folder of your choice. In the terminal window go to that folder and run the docker image:

`docker run enigma`

## Add Node 

> To add a new node use this code:

```python
node_added = enigma.node.add(10.255.16.8)
```

Adds a new node to the enigma cloud. 

`enigma.node.add(ip_address)`

**Returns:** `true` if node added succesfully, `false` otherwise

Parameter | Type | Description
--------- | ------- | -----------
ip_address | ip address | ip address of the seed node to connect to

<aside class="notice">
Requires having first deployed enigma on a seed node. See instructions above on how to do this.  
</aside>

# Access Control

## Grant Access

> To grant access to data use this code:

```python
addresses = ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD', '1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2', '3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy']
transaction_id = enigma.pair(addresses) 
```

Grant access to data associated with a set of specified addresses. Addresss are compatible with bitcoin addresses.

`enigma.pair(public_addresses)`

**Returns:** The `id` in the blockchain that has acceess control information, `false` if pairng is unsucessful

### Pairing parameters

Parameter | Type | Description
--------- | ------- | -----------
public_addresses | list of strings | addresses referencing data to grant access to 


## Revoke Access

> To revoke access to data use this code:

```python
addresses = ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD', '1BvBMSEYstWetqTFn5Au4m4GFg7xJaNVN2', '3J98t1WpEZ73CNmQviecrnyiWrnqRhWNLy']
revoke_successful = enigma.revoke(addresses) 
```

Revokes data access associated to a set of specified addresses. 

`enigma.revoke(public_addresses)`

**Returns:** `true` if access successfully revoked, `false` otherwise

### Revoke parameters

Parameter | Type | Description
--------- | ------- | -----------
public_addresses | list of strings | addresses to cut acccess to


# Data Management 

## Store Data

> To store data in the enigma cloud, use this code:

```python
data = [24.7, 115.19, 668.1, 12.3, 9.89, 245.4, 110.2, 3.58, 6.71, 2.35, 34.5, 330]
key = enigma.store(data) 
```

Securely stores data in the enigma cloud. 

`enigma.store(data)`

**Returns:** `key` referencing the data, `error` if undable to store the data

### Store parameters

Parameter | Type | Description
--------- | ------- | -----------
data | list or array | data to securely store in enigma, data can be in any format 

## Retreive Data

> To retreive data from the enigma cloud, use this code:

```python
my_key = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
data = enigma.load(my_key)
```

Extracts the data from the enigma cloud and reconstructs it in clear.

`enigma.load(key)`

**Returns:** list of `data` if access is permitted, `error` otherwise

### Load parameters

Parameter | Type | Description
--------- | ----------- | -----------
key | string | key used to access the data 

## Delete Data

> To delete data from the enigma cloud, use this code:

```python
my_key = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
data_deleted = enigma.delete(my_key)
```

Deletes data from the engima cloud.

`enigma.delete(key)`

**Returns:** `true` if data sucessfully deleted, `false` otherwise

### Delete parameters

Parameter | Type | Description
--------- | ----------- | -----------
key | string | key used to access the data 


# Data Processing 

> To run operations on enigma, use this code:

```python
key = "3048024100C918FACF8DEB2DEFD5FD3789B9E069EA97FC205E35F577EE31C4FBC6E448117D86BC8FBAFA362F922BF01B2F400001"
result = enigma.compute(mean, key)
```

Runs the operation defined by function for the list of specified keys.

`enigma.compute(function_name, keys)`

Supported functions: mean, standard_deviation, linear_regression

**Returns:** `result` if computation successful, `error` otherwise

<aside class="notice">
Will only run if keys provided have authorized access (also assumes addresses associated to data still have access?)
</aside>

### Compute parameters

Parameter | Type | Description
--------- | ----------- | -----------
function_name | enigma function | operation to run on the data, see list of possible functions above 
keys | list of strings | keys that have access to the data 

